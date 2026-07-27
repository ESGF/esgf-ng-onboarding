# Operator Runbook: Onboarding an Institution to the ESGF-NG West Catalog

**Audience:** ESGF2-US / Globus / Argonne operators
**Scope:** Authorizing a new institution (with a legitimate data node and project) to publish to the West STAC catalog via the STAC Transaction API.

## Background: how authorization works

The Transaction API (FastAPI) is the only Kafka producer. Publishers never touch Kafka resources directly. Instead, they authenticate to the API with a Globus token and are authorized entirely by **Globus group membership**.

On each request the authorizer (`globus_authorizer.py`):

1. Introspects the caller's Globus access token (validates `active`, audience, scope, issuer).
2. Calls the Globus Groups API for the caller's **active** memberships.
3. Loads the access control policy and keeps only entitlements whose trailing group UUID matches one of the caller's groups.
4. Allows the operation only if a surviving entitlement grants the requested **role** on **both** the item's project and its node.

The access control policy is a flat, line-oriented entitlement list of the form:

```txt
<resource>:<id>:role=<ROLE>:group:<GLOBUS_GROUP_UUID>
```

- `<resource>` is `project` or `node`
- `<id>` is a specific project/node id, or `*` for all
- `<ROLE>` is one of `CREATE`, `UPDATE`, `DELETE`, `REPLICATE`, `REVOKE`
- `<GLOBUS_GROUP_UUID>` is the institution's Globus group

Each environment (integration and production) has its own policy, loaded by the API from `TRANSACTION_CLIENT__GLOBUS_POLICY_PATH` (e.g. an S3 object). The policy is cached in-process (`policy_cache_ttl_seconds`) and per-token auth is cached up to 5 minutes (`authorizer_cache_ttl_seconds`), so policy and membership changes are not instant.

## Prerequisites to verify before onboarding

- **Form submission received** via the ESGF-NG Onboarding form, including: institution `institution_id` (CMIP), target project(s), data node address, and the Globus identities of the designated group managers.
- **Project collection exists.** Confirm the target project has a STAC collection (West: https://discovery.west.esgf.io). If not, the project's CV/schema must first be registered via `esgf-vocab`/`esgvoc` and coordinated with the WIP co-chairs (WCRP) or ESGF-XC. You cannot proceed until the collection and controlled vocabulary is present.
- **Node is legitimate.** Confirm the submitted node address is a real ESGF data node operated by the institution.
- **Manager identities resolve.** Confirm each designated manager's Globus identity (username/email) resolves to the correct person.

## Onboarding steps

### 1. Create the institution's Globus group

Create the group with the [Globus CLI](https://docs.globus.org/cli/), authenticated as the `groups-creator` service account. There is already a client identity created in the Globus developer console under the **ESGFNG** project. Do not create the group through the web app.

1. Retrieve the `groups-creator` credentials from AWS Secrets Manager and export them as the Globus CLI client-identity environment variables. With these set, the CLI acts as the service account for all subsequent commands:

```bash
CREDS=$(aws secretsmanager get-secret-value \
  --secret-id groups-creator --query SecretString --output text)
export GLOBUS_CLI_CLIENT_ID=$(echo "$CREDS" | jq -r .GLOBUS_CLIENT_ID)
export GLOBUS_CLI_CLIENT_SECRET=$(echo "$CREDS" | jq -r .GLOBUS_CLIENT_SECRET)
```

(Adjust the `.GLOBUS_CLIENT_ID` / `.GLOBUS_CLIENT_SECRET` JSON keys to match how the secret was stored.)

2. Create the group:

```bash
globus group create "ESGF Publishers - <data_node>"
```

The command returns the new group's UUID — record it for step 3. This is the single group used for all of that institution's nodes and projects.

**Resources:** [Globus CLI documentation](https://docs.globus.org/cli/) · [Client credentials with `GLOBUS_CLI_CLIENT_ID`](https://docs.globus.org/cli/environment_variables/#client_credentials_with_globus_cli_client_id)

### 2. Assign group managers

Grant the designated individuals the **Manager** (or **Administrator**) role in the group so they can add/remove their own members going forward. Add the initial members if requested, otherwise the managers will.

Using the same `groups-creator`-authenticated CLI session from step 1, invite the managers. `<GROUP_UUID>` is the value recorded in step 3; `<identity>` is a Globus username (e.g. `jdoe@uchicago.edu`) or identity UUID:

```bash
# Manager (can add/remove members):
globus group member invite <GROUP_UUID> <identity> --role manager

# Or full admin (can also change roles and edit the group):
globus group member invite <GROUP_UUID> <identity> --role admin
```

`invite` sends an invitation the person must accept before the role takes effect. Confirm status with `globus group member list <GROUP_UUID>`.

### 3. Record the group UUID

Copy the group's UUID (e.g. `e3329078-b8f6-11f0-9fdd-0e7d9e9fc9e3`). This UUID is what binds authorization in the policy.

### 4. Add entitlements to the access control policy

Append entries binding the institution's project(s) and node to the group UUID, one line per role. Typical publisher grant:

```txt
project:<PROJECT_ID>:role=CREATE:group:<GROUP_UUID>
project:<PROJECT_ID>:role=UPDATE:group:<GROUP_UUID>
node:<NODE_ID>:role=CREATE:group:<GROUP_UUID>
node:<NODE_ID>:role=UPDATE:group:<GROUP_UUID>
node:<NODE_ID>:role=REPLICATE:group:<GROUP_UUID>
```

Add `DELETE`/`REVOKE` on the node and/or project only if the institution needs to retract or revoke. Scope `<PROJECT_ID>`/`<NODE_ID>` to the specific project and node. Avoid `*` for production grants. Remember a request must match on **both** a project entitlement and a node entitlement for the same role.

### 5. Deploy the policy

Publish the updated policy to the location the target environment's API loads (`TRANSACTION_CLIENT__GLOBUS_POLICY_PATH`). Start with **integration**, then promote to **production**. Allow for the policy cache TTL before expecting the change to take effect.

### 6. Verify in integration

Have a group member obtain a token and publish a test item to the integration environment:

- Transaction API: `https://transaction-int.west.esgf.io`
- Discovery/STAC API: `https://discovery-int.west.esgf.io`

Confirm the API returns `202 Accepted` and the item appears in the integration STAC catalog. A `401` means token/membership/scope issues; a `403`/authorization failure means the entitlement isn't matching (check project **and** node ids, role, and group UUID).

### 7. Promote to production and notify

Once integration is verified, ensure the production policy contains the same entitlements:

- Transaction API: `https://transaction.west.esgf.io`
- Discovery/STAC API: `https://discovery.west.esgf.io`

Notify the institution that they are authorized, and point them at the publisher configuration (`esg.yaml`) and membership-management instructions in the onboarding repo.

## Post-onboarding notes

- **Membership is self-service.** After setup, group managers add/remove publishers themselves via Globus.
- **Propagation delay.** Newly added members may need a fresh login and up to ~5 minutes; removed members lose access after the auth cache expires.
- **Group UUID is load-bearing.** If a group is ever deleted and recreated, its UUID changes and every policy entry referencing it must be updated.
- **Adding projects/nodes later.** To authorize an existing institution for an additional project or node, just append entitlements for that project/node against the same group UUID.
