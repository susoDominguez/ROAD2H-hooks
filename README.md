# ROAD2H-hooks
Documentation for hooks providing CDS Services for COPD reviewing and clinical recommendations suggestions as specified in ROAD2H project

````
http://www.road2h.org/

`````
## Hooks folder
Contains CDS hook definitions for providing clinical support when managing COPD patients. The hook labelled as 'copd-assess' provides an assessment of the severity of COPD on the current patient, given his/her COPD-related measurements. Additionally, it returns a list of COPD drug types in order of preference as suggested in GOLD 2017 COPD guideline. The second hook, 'copd-careplan-select', provides ontology-based support using the previous call response and other factors such as comorbidities, current vaccinations, etc. Both responses are standardised using CDS card format.

## Json_files
Contains CDS card responses to the examples displayed in the CDS hook definitions.

## ForecastEffect FHIR Resource
This folder contains the schema for the new FHIR resource which describes potential effects if a particular care action is applied, as described in the TMR model.

## Database forms
This folder holds the forms feds to the Data Management Interoperability Microservice in order to extract and manipulate data from a COPD patient to convert them into new data for finding current COPD severity and drugs priority, copd-assess hook, or for providing ontology-based decision support using the TMR web service, copd-careplan-select hook. 
