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

* Look for your project’s collection here: 
  * [West STAC API](https://discovery.west.esgf.io)
  * [Metagrid](https://metagrid.esgf.io)

***If the project your institution needs to contribute to already has a collection in the ESGF STAC catalog, skip to the next section.***

Before you can publish to a new ESGF project, you must first register the project’s schema with the esgf-vocab project. When esgf-vocab is populated with your project schema, the project’s collection should appear automatically in the ESGF STAC catalogs.

After you’ve worked with the esgf-vocab team to add your project’s schema to esgf-vocab, watch the collection endpoint (see links above) for your collection to appear. If it doesn’t appear automatically within two business days, open an issue in the [esgf-vocab repository](https://github.com/ESGF/esgf-vocab) for assistance.

## Register an Institution
***If your institution has published to ESGF-NG before (presumably with a different ESGF project), skip to the next section.***

Before individuals at your institution can publish to ESGF projects, you must first register your institution with ESGF-NG.

Use the following Google form to share the institution name (CMIP) institution_id, and the ID(s) of the individuals who will add/remove other members of your team.
For the West STAC catalog, please use the [ESGF-NG Onboarding form](https://docs.google.com/forms/d/e/1FAIpQLSfB8PriU--KLeiIlc1WvRaHNCXqrYwUIFh2SADMuWskcL-DFA/viewform?usp=header)

Wait for your institution’s group to be created. When your group is created…
1. Add members at your institution
2. Review membership regularly & remove members

## Publish to an ESGF Project
Use the form above to request publication permissions for users at your institution.

The form includes a question about publication/write access. Before datasets can be published to a project collection and indexed in ESGF, the associated CMIP institution_id must be granted permission to publish to that collection.

When your institution has been granted permission to publish to the project’s collection…
1. Configure publisher for the ESGF staging environment.
  * West Staging/testing: (STAC API URL, Metagrid URL)
2. Publish a test dataset and confirm that it appears properly in the staging environment. If not, consult ______ for assistance.
3. Configure publisher for the ESGF production environment.
  * West Production: (STAC API URL, Metagrid URL)
4. Now, you can run the ESGF Publisher application to publish datasets to the ESGF project.
