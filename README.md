# esgf-ng-onboarding

ESGF-NG is a new catalog for ESGF data, first used for the CMIP7 cycle. The data itself is available at ESGF data nodes distributed around the world. The catalog contains searchable metadata for data items with references to data locations. To make data stored at a data node discoverable by the research community, it is *published* in the ESGF-NG catalog.

This repository provided instructions and information about how to obtain permission to make changes to the ESGF-NG catalog. Changes include:
* Adding new items to the catalog (an action typically performed by a modeling center)
* Updating existing items in the catalog (e.g., retracting an item or adding a new location where the data is available)

## ESGF Project Confirmation
Before proceeding, confirm that there’s a collection in the ESGF STAC catalog for the project to which you’re contributing.

* Look for your project’s collection here:
  * [West STAC API](https://discovery.west.esgf.io)
  * [East STAC API](https://api.stac.esgf.ceda.ac.uk)
  * [Metagrid](https://metagrid.esgf-west.org)

***If the project your institution needs to contribute to already has a collection in the ESGF STAC catalog, skip to the next section.***

Before you can publish to a new ESGF project, you must first register the project’s schema. When esgf-vocab is populated with your project schema, the project’s collection should appear automatically in the ESGF STAC catalogs.

* If you are coordinating with the WCRP, please reach out to the WIP (infrastructure panel) co-chairs (link at IPO site).
* If this project is not affiliated with WCRP research, please reach out to the ESGF-XC.

After you’ve received confirmation from the appropriate group, you will develop a Controlled Vocabulary (CV) for your project that will be managed via [esgf-vocab](esgf.github.io/esgf-vocab) (the `esgvoc` package).   We recommend you verify the CVs using the software.  Once complete, the corresponding STAC catalog should populate in the West Discovery API.

## Choosing a Catalog
There are currently two ESGF-NG catalogs ("East" and "West"). The catalogs are synchronized, such that changes made in either catalog are reflected automatically in the other. Updates may be made in either catalog.

The choice of which catalog to use for making updates is principally a choice between the organizations who operate the catalogs and the authentication systems used by each catalog. EGI Check-in (used by the East catalog) and Globus (used by the West catalog). These authentication systems are managed by distinct organizations in distinct locations with distinct governance systems.
* The East catalog is operated by the Center for Environmental Data Analysis in the United Kingdom. The East catalog uses EGI Check-in for authentication. EGI Check-in is operated by the European Union's research community.
* The West catalog is operated by the U.S. Dept. of Energy's ESGF2-US project (Oak Ridge National Laboratory, Argonne National Laboratory, and Lawrence Livermore National Laboratory). The West catalog uses Globus for authentication. Globus is operated by the University of Chicago.

Onboarding to ESGF-NG happens at the institutional level. We onboard institutions, not individuals. Each ESGF-NG catalog has its own onboarding process. The processes are very similar, but each has distinct points of contact and specific instructions.

* If your institution prefers to use Globus for ESGF data publication, follow the [instructions for onboarding to the ESGF West catalog](west-catalog/README.md).
* If your institution prefers to use EGI Check-in for ESGF data publication, follow the [instructions for onboarding to the ESGF East catalog](east-catalog/README.md).
