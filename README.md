# ROAD2H-hooks
Documentation concerning the specification of a pair of CDS hooks that trigger notifications for remote clinical decision support (CDS) on the management of patients with Chronic Obstructive Pulmonary Disease (COPD) (and possibly with Cardiovascular Disease (CVD) and/or Chronic Kidney Disease (CKD) comorbidities). The provided CDS services are (a) the assessment of the COPD symptom severity in a COPD patient and, (b) a collection of one or more personalised COPD treatment planning proposals that take into account potential conflicts with CVD and CKD comorbidities.

The CDS hooks habe been developed as part of the ROAD2H project.

````
http://www.road2h.org/

`````
## Hooks folder
This folder contains the pair of hook specifications, with examples, to trigger the available CDS services for COPD management. The hook with Id 'copd-assess' provides patient information on previous and current COPD symptoms severity measurements and current COPD treatment; it is associated with the COPD severity assessment CDS service. The hook with Id 'copd-careplan-review' provides patient information on active COPD treatments, the current pulmonologist-verified COPD severity assessment as well as on the status of current vaccinations and active comorbidities. This hook is associated with the COPD treatment planning CDS service.

## Cards folder
Contains CDS card responses to the examples displayed as part of the CDS hook definitions in the hooks folder.

## ForecastEffect FHIR Resource
This folder contains the schema for the new FHIR type, ForecastEffect, that describes the expected effects of a care action on the overall health of a patient if a particular clinical treatment or service is administered. The FHIR forecastEffect permits mapping clinical suggestions in the form of Transition-based Medical Recommendations (TMRs) to FHIR types. FHIR does not include resources to describe potential outcomes.

## ROAD2H use cases
This folder contains the collection of twenty use cases used as part of the evaluation of the COPD CDS system. Additionally, it contains two sub-folders, one for each hook, where the use cases have been formalised to be applied as input to the COPD CDS system.
