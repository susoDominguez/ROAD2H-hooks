# <mark>`copd-assessment`</mark>

| Metadata | Value
| ---- | ----
| specificationVersion | 1.1
| hookVersion | 2,0
| hookMaturity | [0 - Draft](../../specification/1.0/#hook-maturity-model)

## Workflow

<mark>The `copd-assessment` hook is triggered when the practicioner is assessing the severity of the airflow limitation on the current patient to adjust medication, if needed. The context contains pairs of measurements of CAT score and mMRC dyspnoea scale, one taken on the current encounter and another as stored on the previous visit. Similarly for the number of exacerbations. Additionally, it contains information on whether asthma is present as well as on the previous diagnosis: the identified COPD group and active medication.</mark>

## Context

<mark></mark>

Field | Optionality | Prefetch Token | Type | Description
----- | -------- | ---- | ---- | ----
<mark>`encounterId`</mark> | OPTIONAL | No | *string* | <mark>identifier of current encounter</mark>
<mark>`medication`</mark> | OPTIONAL | No | *object* | <mark>COPD medication currently active. Omission of this resource suggests  patient has just been newly diagnosed with COPD at this encounter.</mark>
<mark>`previousAssessment`</mark> | OPTIONAL | No | *object* | <mark>FHIR Bundle of Observations in 'final' state representing  COPD group, CAT score, mMRC dyspnoea scale and number of exacerbations as recorded on the previous COPD-related encounter. Omission of the bundle resource entirely suggests  patient has just been newly diagnosed with COPD at this encounter.</mark>
<mark>`currentAssessment`</mark> | REQUIRED | No | *object* | <mark>FHIR Bundle of Observations in 'preliminary' state representing  CAT score, mMRC dyspnoea scale and number of exacerbations as measured at the current encounter.</mark>
<mark>`asthma`</mark> | OPTIONAL | No | *Condition* | <mark>Condition resource denoting the presence of Asthma in patient's record.</mark>

### Examples


```json
{
  "context": {
    "encounterId": "987654",
    "medication": {
      "resourceType": "Medication",
      "id": "DrugTLaba",
      "code": {
        "coding": [
          {
            "system": "http://anonymous.org/data/DrugTLaba",
            "code": "Laba",
            "display": "administer LABA"
          }
        ]
      },
      "subject": {
        "reference": "Patient/1677163",
        "display": "Link to patient"
      }
    },
    "previousAssessment": {
      "resourceType": "Bundle",
      "entry": [
        {
          "resourceType": "Observation",
          "id": "copd_group_prev",
          "status": "final",
          "code": {
            "coding": [
              {
                "system": "",
                "code": "",
                "display": "GOLD classification for chronic obstructive pulmonary disease (COPD)"
              }
            ]
          },
          "valueString": "B",
          "referenceRange": [
            {
              "low": {
                "value": "A"
              }
            },
            {
              "high": {
                "value": "D"
              }
            }
          ]
        },
        {
          "resourceType": "Observation",
          "id": "cat_score_prev",
          "status": "final",
          "valueInteger": 8,
          "code": {
            "coding": [
              {
                "system": "local-system",
                "code": "CAT_score_code",
                "display": "COPD Assessment Test (CAT) score"
              }
            ]
          }
        },
        {
          "resourceType": "Observation",
          "id": "mmrc_prev",
          "status": "final",
          "valueInteger": 1,
          "code": {
            "coding": [
              {
                "system": "",
                "code": "",
                "display": "modified Medical Research Council (mMRC) dyspnea scores"
              }
            ]
          },
          "referenceRange": [
            {
              "low": {
                "value": "0"
              }
            },
            {
              "high": {
                "value": "4"
              }
            }
          ]
        },
        {
          "resourceType": "Observation",
          "id": "copd_exacerbations_number_prev",
          "status": "final",
          "valueInteger": 0,
          "code": {
            "coding": [
              {
                "system": "",
                "code": "",
                "display": "Number of COPD exacerbations"
              }
            ]
          }
        }
      ]
    },
    "currentAssessment": {
      "resourceType": "Bundle",
      "entry": [
        {
          "resourceType": "Observation",
          "id": "copd_group_prev",
          "status": "final",
          "code": {
            "coding": [
              {
                "system": "",
                "code": "",
                "display": "GOLD classification for chronic obstructive pulmonary disease (COPD)"
              }
            ]
          },
          "valueString": "B",
          "referenceRange": [
            {
              "low": {
                "value": "A"
              }
            },
            {
              "high": {
                "value": "D"
              }
            }
          ]
        },
        {
          "resourceType": "Observation",
          "id": "cat_score",
          "status": "preliminary",
          "valueInteger": 7,
          "code": {
            "coding": [
              {
                "system": "local-system",
                "code": "CAT_score_code",
                "display": "COPD Assessment Test (CAT) score"
              }
            ]
          }
        },
        {
          "resourceType": "Observation",
          "id": "mmrc",
          "status": "preliminary",
          "valueInteger": 0,
          "code": {
            "coding": [
              {
                "system": "",
                "code": "",
                "display": "modified Medical Research Council (mMRC) dyspnea scores"
              }
            ]
          },
          "referenceRange": [
            {
              "low": {
                "value": "0"
              }
            },
            {
              "high": {
                "value": "4"
              }
            }
          ]
        },
        {
          "resourceType": "Observation",
          "id": "copd_exacerbations_number",
          "status": "preliminary",
          "valueInteger": 0,
          "code": {
            "coding": [
              {
                "system": "",
                "code": "",
                "display": "Number of COPD exacerbations"
              }
            ]
          }
        }
      ]
    }
  }
}
```

## Change Log

Version | Description
---- | ----
1.2 | FHIR resource parameters
