# Template for the OHDSI ETL Wrapper
An ETL template for future OHDSI projects.
Provides a standard repository structure that implements the functionality of the [delphyne](https://github.com/thehyve/delphyne) Python package.

NOTE: This template has only been tested with PostgreSQL.

## Getting Started
_TODO_

## Repository overview

### `/root`
- `main.py` triggers the ETL execution. Requires minimal customization of e.g. default connection arguments and pipeline version info.
- `README-sample.md` provides a basic README file for the project, including instructions on how to run the ETL.
  Use it to **replace this readme** after completing the initial ETL setup.
- `requirements.txt` should contain project-specific ETL dependencies. It is **highly recommended** to pin packages to a specific version.
  `delphyne` is a mandatory dependency and is always included by default.

### `/config`
ETL configuration folder, contains:
- `config-sample.yml`, the general pipeline configuration options. Copy and rename this file to create as many configurations as needed for different pipeline execution scenarios. 
- `source_config-sample.yml`, the configuration for source data files. Copy and rename this file to `source_config.yml` (if you have source data files) to specify the file properties.
- `logging-sample.yml`, the logging configuration. Copy and rename this file to `logging.yml` to customize the logging behavior (this is the only file name that will be recognized by the ETL).


All files in this folder except for the provided examples will be automatically ignored by git, so that any confidential configuration is not accidentally shared on Github. 

### `/docs`
Project documentation folder. It is recommended to place here the markdown version of the mapping specifications generated by e.g. Rabbit in a Hat. This can be used as the source for the repository's [Github pages site](https://help.github.com/en/github/working-with-github-pages/creating-a-github-pages-site).

### `/other`
Contains a collection of scripts that are meant to be **executed independently** from the ETL pipeline, often in the initial phases of the project. 
Examples of intended uses are data exploration and cleanup, the preparation of mapping tables, and the generation of reports.
In most cases, these are project specific files. Any generic scripts used at this stage, can be added here (e.g. the ontology mapping script provided currently).

### `/resources`
Meant to contain data used by the ETL, such as mapping tables and custom vocabularies.
Unit test data should not be placed in this folder, but under `src/test/`.

Folder structure conventions:
- `custom_vocabularies` should contain project-specific vocabularies, if any. Remove this folder if not used.
- `mapping_tables` collects the mappings of project-specific variables and values to standard OMOP concept_ids. 
The mapping tables should adopt an **Usagi-compatible format** whenever possible.
- `synthetic_data` should contain data for E2E tests, generated with the Rabbit in a Hat functionality.  

### `src/main/python`
Contains the main ETL code.
- `custom_tables` collects any custom table needed in the project that is not available from the selected `delphyne` CDM version.
These will be automatically handled by the ETL once bound to a `Base` object, also provided in the `delphyne` package.
The `TreatmentLine` table is provided as an example. Remove this folder if no custom table is needed.
- `transformation/` collects the project-specific transformation scripts for each source data table - target CDM table combination.
The scripts must follow the mapping specifications closely, and vice-versa the mapping specifications must reflect any implementation decision made in the scripts.
- `util/` should contain any functions or classes needed in the transformation scripts that is not already available in the repository dependencies. 
An example is the implementation of project-specific VariableConceptMapper subclasses. 
- `wrapper.py` specifies the order of operations executed during each ETL run. 
Add project-specific scripts from the `transformation/` and `sql/` folders to the `run()` method to have them executed by the pipeline.

### `/src/main/sql`
This contains (Postgre)SQL-based transformation scripts. The provided `sample_script.sql` contains the code to populate the 
`observation_period` table from existing CDM tables.

### `/src/test/R`
This folder contains the end-to-end tests based on the test framework that can be automatically generated by Rabbit in a Hat.
See `readme.md` in the folder for details on how to build and use the framework.

Other tests, e.g. Python unit tests, can also be placed in a new `/src/test/python` folder.

## Planned developments
 - A future template release will replace the R framework with Python-based unit tests for better integration with GitHub workflows.
 - ...

