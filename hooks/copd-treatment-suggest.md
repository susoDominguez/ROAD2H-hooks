# <mark>`copd-recommendations-suggest`</mark>

| Metadata | Value
| ---- | ----
| specificationVersion | 1.2
| hookVersion | 1.2
| hookMaturity | [0 - Draft](../../specification/1.0/#hook-maturity-model)

## Workflow

<mark>The `copd-recommendations-suggest` hook fires when a practitioner has reviewed the airflow limitation severity of a patient, by identifying one of the 
four COPD groups and selecting one or more drug types that are suitable for the COPD group and the patient at hand. The CDS Service then provides a collection
of personalised guidelines based on GOLD COPD 2017 where each of which contains non-conflictive clinical recommendations taking into account practitioner/patient choices -COPD group and medication-, immunization, lifestyle -smoker status- and comorbidities potentially affecting COPD treatment.</mark>

## Context

<mark></mark>

Field | Optionality | Prefetch Token | Type | Description
----- | -------- | ---- | ---- | ----
<mark>`patientID`</mark> | REQUIRED | Yes | *string* | <mark>identifier of current patient</mark>
<mark>`userId`</mark> | OPTIONAL | Yes | *string* | <mark>identifier of current practitioner</mark>
<mark>`encounterId`</mark> | OPTIONAL | Yes | *string* | <mark>identifier of current encounter</mark>
<mark>`birthDate`</mark> | REQUIRED | No | *string* | <mark>date of birth of patient, used to identify whether Pneumococcal vaccine should be administered on patients of sixty-five years of age or older</mark>
<mark>`currentSmoker`</mark> | REQUIRED | No | *boolean* | <mark>identifies current patient as a regular smoker or otherwise</mark>
<mark>`hasCardiovascularDisease`</mark> | REQUIRED | No | *boolean* | <mark>Is Cardiovascular disease present in record?</mark>
<mark>`hasRenalDisease`</mark> | REQUIRED | No | *boolean* | <mark>Is Renal disease present in record?</mark>
<mark>`hasAnnualInfluenzaVaccine`</mark> | REQUIRED | No | *boolean* | <mark>Has taken annual influenza vaccine?</mark>
<mark>`hasPneumococcalVaccine`</mark> | REQUIRED | No | *boolean* | <mark>Has taken Pneumococcal vaccine?</mark>
<mark>`copdGroup`</mark> | REQUIRED | No | *string* | <mark>string identifying COPD group as preliminary recorded at current COPD review.</mark>
<mark>`treatmentOptions`</mark> | OPTIONAL | No | *Medication* | <mark>Resource containing user-selected COPD treatments. For the optional case, the CDS Service would select ALL suggested treatments for the given COPD group as sspecified at GOLD 2017 COPD guideline</mark>

### Examples


```json
{
    "context" : { 
        "patientId" : "18", 
        "userId" : "Practitioner/123456", 
        "encounterId" : "987654",
        "birthDate" : "1954-01-01",
        "currentSmoker" : true,
        "hasCardiovascularDisease": true,
        "hasRenalDisease": false,
        "hasAnnualInfluenzaVaccine" : true,
        "hasPneumococcalVaccine" : false, 
        "copdGroup" : "A",
        "treatmentOptions" : {
                    "resourceType": "Medication",
                    "id": "suggested_treatments",
                    "code": {
                      "coding": [
                        {
                          "code": "Laba",
                          "display": "LABA"
                        },
                        {
                          "code": "Lama",
                          "display": "LAMA"
                        },
                        {
                          "code": "LabaLama",
                          "display": "LABA + LAMA"
                        }
                      ],
                      "text": "Suggested tretments by DSS"
                   }
         } 
    }
}
```

## Change Log

Version | Description
---- | ----
1.2 | tokenised approach
