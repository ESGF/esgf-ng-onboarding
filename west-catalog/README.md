# Onboarding to the ESGF-NG West Catalog

The ESGF-NG West STAC catalog is operated in the United States of America by the ESGF2-US project (Oak Ridge National Laboratory, Argonne National Laboratory, Lawrence Livermore National Laboratory). It is one of (currently) two ESGF STAC catalogs ("East" and "West"). All ESGF STAC catalogs are synchronized: changes made in one are automatically reflected in the other(s).

This document explains how to obtain permission to publish and update items using the ESGF-NG West STAC catalog.

## Overview

Catalog onboarding happens at the *institution* and *ESGF project* levels. We onboard institutions rather than individuals. Permissions to distinct ESGF projects are granted separately. These instructions will walk you through the process of onboarding your institution so individuals at your institution can be authorized to make changes to one or more ESGF projects in the STAC catalog.

The following requirements must be met in order for individuals at your institution to publish (or update) items in an ESGF project in the West ESGF-NG catalog. The remainder of this document explains how to satisfy these requirements and how to begin publishing once they’re satisfied.

1. The ESGF project must have a *collection* in the ESGF-NG catalog.
2. Your institution’s ESGF team must have an *ESGF Globus group*, and you must add authorized publishers to it.
3. Your **data node** must be registered and bound to your Globus group in the access control policy.
4. The ESGF West operations team must *authorize* your group to publish to the relevant project(s) and node.

## How authorization works

Publishing is authorized by **Globus group membership**. Operators add entries to the access control policy that bind your node and project to your group's UUID, one per role, e.g.:

```txt
node:<NODE_ID>:role=CREATE:group:<YOUR_GLOBUS_GROUP_UUID>
project:<PROJECT_ID>:role=CREATE:group:<YOUR_GLOBUS_GROUP_UUID>
```

Roles are `CREATE`, `UPDATE`, `DELETE`, `REPLICATE`, and `REVOKE`. A publish request is allowed only if a user is an **active member** of a group granted the needed role on **both** the item's project and node. So to publish, a user simply needs to be in your group. Removing them from the group revokes access.

## Register an Institution
***If your institution has published to ESGF-NG before (presumably with a different ESGF project), skip to the next section.***

Before individuals at your institution can publish to ESGF projects, you must first register your institution with ESGF-NG.

Use the following Google form to share the institution name (CMIP) institution_id, and the ID(s) of the individuals who will add/remove other members of your team.
For the West STAC catalog, please use the [ESGF-NG Onboarding form](https://docs.google.com/forms/d/e/1FAIpQLSfB8PriU--KLeiIlc1WvRaHNCXqrYwUIFh2SADMuWskcL-DFA/viewform?usp=header)

Wait for your institution’s group to be created. When your group is created…
1. [Add members at your institution](manage-group-membership.md)
2. Review membership regularly & remove members

## Register Your Data Node
***If your institution has already published from this node, skip to the next section.***

Your node must be bound to your Globus group before you can publish from it. Using the [ESGF-NG Onboarding form](https://docs.google.com/forms/d/e/1FAIpQLSfB8PriU--KLeiIlc1WvRaHNCXqrYwUIFh2SADMuWskcL-DFA/viewform?usp=header), provide your node address (e.g. `esgf-node.llnl.gov`). Operators create your institution's Globus group and add the node/project entries to the access control policy for you. You do not need to create the group.

## Publish to an ESGF Project
Use the form above to request publication permissions for users at your institution.

The form includes a question about publication/write access. Before datasets can be published to a project collection and indexed in ESGF, the associated CMIP institution_id must be granted permission to publish to that collection.

When your institution has been granted permission to publish to the project’s collection…
1. Configure publisher for the ESGF staging environment.
  * West Staging/testing: (STAC API URLs)
    - Add the following to your `esg.yaml` config file for test publishing:
```
stac_config:
  stac_client:
    client_id: a34e738b-efa7-49b0-bc11-c74e6c6ec011
    redirect_uri: https://auth.globus.org/v2/web/auth-code
  token_storage_file: ~/.esgf-publisher.json
  stac_transaction_api:
    client_id: 7467bc71-1417-43f0-a7a9-a26c45757c36
    access_control_policy: https://esgf2.s3.amazonaws.com/access_control_policy.json
    scope_string: https://auth.globus.org/scopes/7467bc71-1417-43f0-a7a9-a26c45757c36/transaction
    base_url: https://transaction-int.west.esgf.io
  stac_api: https://discovery-int.west.esgf.io
```
2. Publish a test dataset and confirm that it appears properly in the staging environment. If not, consult [esgf-user@llnl.gov](mailto:esgf-user@llnl.gov) for assistance.
3. Configure publisher for the ESGF production environment.
  * West Production: (STAC API URLs)
    - Add the following to your `esg.yaml` config file for production publishing:
```
stac_config:
  stac_client:
    client_id: 40ef1be1-5d35-4a69-a571-8ca8bec8f211
    redirect_uri: https://auth.globus.org/v2/web/auth-code
  token_storage_file: ~/.esgf-publisher.json
  stac_transaction_api:
    client_id: 66ae998e-9e67-4eea-bf9d-7d0e1eb0946f
    access_control_policy: https://esgf2.s3.amazonaws.com/access_control_policy.json
    scope_string: https://auth.globus.org/scopes/66ae998e-9e67-4eea-bf9d-7d0e1eb0946f/transaction
    base_url: https://transaction.west.esgf.io
  stac_api: https://discovery.west.esgf.io
```
4. Upgrade your publisher to the most recent version: `pip install --upgrade esgcet`
5. Now, you can run the ESGF Publisher application to publish datasets to the ESGF project.  For more information see the https://esg-publisher.readthedocs.org/ site.
