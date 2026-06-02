# Onboarding to the ESGF-NG East Catalog

**Note: Currently ESGF East's EGI Check-In auth only supports 8 hour tokens, for long running publication tasks please use the [ESGF-NG West Catalog](west-catalog/README.md)** 

The ESGF-NG East STAC catalog is operated in the United Kingdom by the CEDA (Center for Environmental Data Analysis). It is one of (currently) two ESGF STAC catalogs ("East" and "West"). All ESGF STAC catalogs are synchronized: changes made in one are automatically reflected in the other(s).

This document explains how to obtain permission to publish and update items using ESGF-NG East.

## Overview

To publish or update a dataset record user must have the relevant permission for the dataset's *ESGF project* and for the *node* where the data is stored. Both of which are granted separately. These instructions will walk you through the process of applying for *project* and *node* permissions.

The following requirements must be met in order to publish (or update) items in an ESGF project via ESGF-NG East. The remainder of this document explains how to satisfy these requirements and how to begin publishing once they’re satisfied.

1. The ESGF project must have a *collection* in the ESGF-NG catalog and be registered with ESGF-NG East.
2. The data node's address must be registered with ESGF-NG East.
3. **Optional:** your institution maybe registered with ESGF-NG East which will allow administrators to manage authorised individuals to your institution.
4. Either the ESGF East operations team or your institution administrators must *authorise* you to publish to the relevant project and node.

## Register an ESGF Project
Before proceeding, confirm that there’s a collection in the ESGF STAC catalog for the project to which you’re contributing.

* Look for your project’s collection here: [East STAC API URL; Metagrid URL]

Before you can publish to a new ESGF project, you must first register the project’s schema with the esgf-vocab project. When esgf-vocab is populated with your project schema, the project’s collection should appear automatically in the ESGF STAC catalogs.

After you’ve worked with the esgf-vocab team to add your project’s schema to esgf-vocab, watch the collection endpoint (see links above) for your collection to appear. If it doesn’t appear automatically within two business days, email _______ for assistance.

## Register a data node
***If the data node you are publishing on behalf of has had data published to ESGF-NG before, skip to the next section.***

Before data at a node can be published it must first be registered with ESGF-NG.

E-mail the appropriate address below and share the node's address (e.g. ceda.ac.uk).
For the East STAC catalog, email: <support@ceda.ac.uk>

Wait for your node's group to be created.

## **Optional:** Register an Institution
***If your institution wishes to manage their users' permissions for projects and/or nodes, they must first register their institution with ESGF-NG.***

E-mail the appropriate address below and share the institution name (CMIP institution_id), the projects and nodes they would like to manage, and the email(s) of the individuals who will add/remove other members of your team.
For the East STAC catalog, email:  <support@ceda.ac.uk>

Wait for your institution’s group to be created. When your group is created…
1. Add members at your institution
2. Review membership regularly & remove members

## Publish to an ESGF Project
To publish to ESGF-NG you must obtain permissions to publish to both the project and node.

If your institution has been registered for a project or node you can enroll using these links:
* https://aai.egi.eu/auth/realms/id/account/#/enroll?groupPath=/esgf.vo.egi.eu/project/{PROJECT_ID}/insitution/{INSTITUTION_ID}
* https://aai.egi.eu/auth/realms/id/account/#/enroll?groupPath=/esgf.vo.egi.eu/node/{NODE_ID}/insitution/{INSTITUTION_ID}
or contact your institution administrator.

If your institution is not registered, you can apply directly for the project or node permission with these links:
* https://aai.egi.eu/auth/realms/id/account/#/enroll?groupPath=/esgf.vo.egi.eu/project/{PROJECT_ID}
* https://aai.egi.eu/auth/realms/id/account/#/enroll?groupPath=/esgf.vo.egi.eu/node/{NODE_ID}

When you've has been granted permission to publish to the relevant projects and nodes…
1. Configure publisher for the ESGF staging environment.
  * East Staging/testing: (<https://transaction-int.east.esgf.io>, <https://esgf-ui.ceda.ac.uk>)
2. Publish a test dataset and confirm that it appears properly in the staging environment. If not, consult <support@ceda.ac.uk> for assistance.
3. Configure publisher for the ESGF production environment.
  * East Production: (<https://transaction.east.esgf.io>, <https://esgf-ui.ceda.ac.uk>)
4. Now, you can run the ESGF Publisher application to publish datasets to the ESGF project.
