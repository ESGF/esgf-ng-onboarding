# Onboarding to the ESGF-NG West Catalog

The ESGF-NG West STAC catalog is operated in the United States of America by the ESGF2-US project (Oak Ridge National Laboratory, Argonne National Laboratory, Lawrence Livermore National Laboratory). It is one of (currently) two ESGF STAC catalogs ("East" and "West"). All ESGF STAC catalogs are synchronized: changes made in one are automatically reflected in the other(s).

This document explains how to obtain permission to publish and update items using the ESGF-NG West STAC catalog.

## Overview

Catalog onboarding happens at the *institution* and *ESGF project* levels. We onboard institutions rather than individuals. Permissions to distinct ESGF projects are granted separately. These instructions will walk you through the process of onboarding your institution so individuals at your institution can be authorized to make changes to one or more ESGF projects in the STAC catalog.

The following requirements must be met in order for individuals at your institution to publish (or update) items in an ESGF project in the West ESGF-NG catalog. The remainder of this document explains how to satisfy these requirements and how to begin publishing once they’re satisfied.

1. The ESGF project must have a *collection* in the ESGF-NG catalog.
2. Your institution’s ESGF team must have an *ESGF Globus group* and you must add authorized individuals to your team’s group.
3. The ESGF West operations team must *authorize* your team’s group to publish to the ESGF project’s collection.

## Register an ESGF Project
Before proceeding, confirm that there’s a collection in the ESGF STAC catalog for the project to which you’re contributing.

* Look for your project’s collection here: [West STAC API URL; Metagrid URL]

***If the project your institution needs to contribute to already has a collection in the ESGF STAC catalog, skip to the next section.***

Before you can publish to a new ESGF project, you must first register the project’s schema with the esgf-vocab project. When esgf-vocab is populated with your project schema, the project’s collection should appear automatically in the ESGF STAC catalogs.

After you’ve worked with the esgf-vocab team to add your project’s schema to esgf-vocab, watch the collection endpoint (see links above) for your collection to appear. If it doesn’t appear automatically within two business days, email _______ for assistance.

## Register an Institution
***If your institution has published to ESGF-NG before (presumably with a different ESGF project), skip to the next section.***

Before individuals at your institution can publish to ESGF projects, you must first register your institution with ESGF-NG.

E-mail the appropriate address below and share the institution name (CMIP institution_id), and the ID(s) of the individuals who will add/remove other members of your team.
For the West STAC catalog, email:  _________

Wait for your institution’s group to be created. When your group is created…
1. Add members at your institution
2. Review membership regularly & remove members

## Publish to an ESGF Project
To obtain permission for individuals at your institution to publish to a project’s collection on one of ESGF’s two catalogs, email ______________ and ask for your CMIP institution_id to be permitted to publish to the project’s collection.

When your institution has been granted permission to publish to the project’s collection…
1. Configure publisher for the ESGF staging environment.
  * West Staging/testing: (STAC API URLs)
    - Add the following to your `esg.yaml` config file for test publishing:
```
stac_config:
  stac_client:
    client_id:  
    redirect_uri: 
  token_storage_file: ~/.esgf-publisher.json 
  stac_transaction_api:
    client_id: 
    access_control_policy: https://esgf2.s3.amazonaws.com/access_control_policy.json
    scope_string:  
    base_url: 
  stac_api: 
```
2. Publish a test dataset and confirm that it appears properly in the staging environment. If not, consult ______ for assistance.
3. Configure publisher for the ESGF production environment.
  * West Production: (STAC API URLs)
    - Add the following to your `esg.yaml` config file for production publishing:
```
stac_config:
  stac_client:
    client_id:  
    redirect_uri: 
  token_storage_file: ~/.esgf-publisher.json 
  stac_transaction_api:
    client_id: 
    access_control_policy: https://esgf2.s3.amazonaws.com/access_control_policy.json
    scope_string:  
    base_url: 
  stac_api: 
``` 
4. Now, you can run the ESGF Publisher application to publish datasets to the ESGF project.
