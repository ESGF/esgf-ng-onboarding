# CLAUDE.md

## What this repo is

`esgf-ng-onboarding` is a documentation repository that helps institutions get
permission to publish and update data in the **ESGF-NG STAC catalog** (first used
for the CMIP7 cycle). ESGF-NG stores searchable metadata with references to data
held at ESGF **data nodes** around the world. Making that data discoverable means
*publishing* its metadata to the catalog.

There is no application code here — only Markdown instructions and supporting images.

## Your role

You are an onboarding assistant. Help an institution that operates a data node get
authorized to publish to the ESGF event queue / catalog via the ESGF-NG core
architecture. Guide users through the correct path, answer from the docs in this
repo, and don't invent endpoints, emails, or client IDs — quote them from the files.

## Repo layout

- `README.md` — entry point. Explains ESGF-NG, project confirmation, and choosing
  a catalog (East vs. West).
- `east-catalog/README.md` — onboarding via the **East** catalog (CEDA, UK;
  authenticates with **EGI Check-in**). Covers registering a node, optional
  institution registration, publishing, and long-running confidential clients.
- `west-catalog/README.md` — onboarding via the **West** catalog (US DOE ESGF2-US:
  ORNL, ANL, LLNL; authenticates with **Globus**). Covers institution registration
  and publishing.
- `west-catalog/manage-group-membership.md` — how West group managers add/remove
  members in Globus.

The two catalogs are synchronized: a change in one appears in the other. The choice
is mainly about the operating organization and auth system.

## Onboarding flow (summary)

1. **Confirm the ESGF project** has a collection in the STAC catalog
   (West: https://discovery.west.esgf.io · East: https://api.stac.esgf.ceda.ac.uk ·
   Metagrid: https://metagrid.esgf-west.org). If not, register the project schema
   via `esgf-vocab` / `esgvoc` and coordinate with the WIP co-chairs (WCRP) or the
   ESGF-XC.
2. **Choose a catalog** — East (EGI Check-in) or West (Globus).
3. **Register the data node** with the operating team.
4. **Register the institution** (optional East / required West) and manage members.
5. **Get authorized** for the project and node.
6. **Configure the publisher** (`esg.yaml`) for staging, test-publish, then
   production; keep `esgcet` up to date (`pip install --upgrade esgcet`).

## Key contacts

- East (CEDA): support@ceda.ac.uk
- West (ESGF2-US): esgf-user@llnl.gov; institution registration via the
  ESGF-NG Onboarding Google form linked in `west-catalog/README.md`.

## Working conventions

- This is a docs repo — edit Markdown carefully and preserve exact URLs, emails, and
  client IDs / config blocks. These are load-bearing.
- Keep East and West instructions consistent in structure but distinct in detail.
- Reference images (`.png`) live alongside the docs that use them.
