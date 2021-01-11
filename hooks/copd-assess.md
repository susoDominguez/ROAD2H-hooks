# <mark>`copd-assess`</mark>

| Metadata | Value
| ---- | ----
| specificationVersion | 2.1
| hookVersion | 2,0
| hookMaturity | [0 - Draft](../../specification/1.0/#hook-maturity-model)

## Workflow

<mark>The `copd-assess` hook is triggered when the practitioner is assessing the severity of the airflow limitation on the current patient to adjust medication, if needed.</mark>

## Context
<mark>The context contains pairs of measurements of CAT score and mMRC dyspnoea scale, one taken on the current encounter and another as stored on the previous visit. Similarly for the number of exacerbations. Additionally, the context contains information on whether asthma is present along with the previous diagnosis, that is, the identified COPD group and active medication.</mark>

Field | Optionality | Prefetch Token | Type | Description
----- | -------- | ---- | ---- | ----
<mark>`encounterId`</mark> | OPTIONAL | No | *string* | <mark>identifier of current encounter</mark>
<mark>`medication`</mark> | OPTIONAL | No | *object* | <mark>COPD medication currently active. Omission of this resource suggests  patient has just been newly diagnosed with COPD at this encounter.</mark>
<mark>`previousAssessment`</mark> | OPTIONAL | No | *object* | <mark>FHIR Bundle of Observations in 'final' state representing  COPD group, CAT score, mMRC dyspnoea scale and number of exacerbations as recorded on the previous COPD-related encounter. Omission of the bundle resource entirely suggests  patient has just been newly diagnosed with COPD at this encounter.</mark>
<mark>`currentAssessment`</mark> | REQUIRED | No | *object* | <mark>FHIR Bundle of Observations in 'preliminary' state representing  CAT score, mMRC dyspnoea scale and number of exacerbations as measured at the current encounter.</mark>
<mark>`asthma`</mark> | OPTIONAL | No | *object* | <mark>Condition resource denoting the presence of Asthma in patient's record.</mark>

### Examples


```json
{
  "hookInstance": "d1577c69-dfbe-44ad-ba6d-3e05e953b2ea",
  "fhirServer": "https://example.org/fhir",
  "hook": "copd-assess",
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
                "system": "http://snomed.info/sct",
                "code": "13645005",
                "display": "Chronic airflow limitation"
              }
            ]
          },
          "valueQuantity": {
            "value": "B",
            "system": "http://unitsofmeasure.org",
            "code": "{score}"
          },
          "effectiveDateTime": "2018-03-11T16:07:54+00:00",
            "referenceRange": [
              {
                "high": {
                  "value": "D",
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "type": {
                  "text": "Very Severe Chronic airflow limitation"
                }
              },
              {
                "high": {
                  "value": "C",
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "type": {
                  "text": "Severe Chronic airflow limitation"
                }
              },
              {
                "low": {
                  "value": "B",
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "type": {
                  "text": "Moderate Chronic airflow limitation"
                }
              },
              {
                "low": {
                  "value": "A",
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "type": {
                  "text": "Mild Chronic airflow limitation"
                }
              }
            ]
        },
        {
          "resourceType": "Observation",
          "id": "cat_score_prev",
          "status": "final",
          "valueQuantity": {
            "value": 8,
            "system": "http://unitsofmeasure.org",
            "code": "{score}"
          },
          "code": {
            "coding": [
              {
                "system": "local-system",
                "code": "CAT_score_code",
                "display": "COPD Assessment Test (CAT) score"
              }
            ]
          },
          "effectiveDateTime": "2018-03-11T16:07:54+00:00",
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
          "id": "mmrc_prev",
          "status": "final",
          "valueQuantity": {
            "value": 0,
            "system": "http://unitsofmeasure.org",
            "code": "{score}"
          },
          "effectiveDateTime": "2018-03-11T16:07:54+00:00",
          "code": {
            "coding": [
              {
                "system": "local-system",
                "code": "codedValue",
                "display": "modified Medical Research Council (mMRC) dyspnea score"
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
          },
          "effectiveDateTime": "2018-03-11T16:07:54+00:00",
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
        }
      ]
    },
    "currentAssessment": {
      "resourceType": "Bundle",
      "entry": [
        {
          "resourceType": "Observation",
          "id": "copd_group_curr",
          "status": "preliminary",
          "code": {
            "coding": [
              {
                "system": "http://snomed.info/sct",
                "code": "13645005",
                "display": "Chronic airflow limitation"
              }
            ]
          },
          "valueQuantity": {
            "value": "B",
            "system": "http://unitsofmeasure.org",
            "code": "{score}"
          },
          "effectiveDateTime": "2018-03-11T16:07:54+00:00",
          "referenceRange": [
              {
                "high": {
                  "value": "D",
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "type": {
                  "text": "Very Severe Chronic airflow limitation"
                }
              },
              {
                "high": {
                  "value": "C",
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "type": {
                  "text": "Severe Chronic airflow limitation"
                }
              },
              {
                "low": {
                  "value": "B",
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "type": {
                  "text": "Moderate Chronic airflow limitation"
                }
              },
              {
                "low": {
                  "value": "A",
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "type": {
                  "text": "Mild Chronic airflow limitation"
                }
              }
            ]
        },
        {
          "resourceType": "Observation",
          "id": "cat_score_curr",
          "status": "preliminary",
          "valueQuantity": {
            "value": 8,
            "system": "http://unitsofmeasure.org",
            "code": "{score}"
          },
          "code": {
            "coding": [
              {
                "system": "local-system",
                "code": "CAT_score_code",
                "display": "COPD Assessment Test (CAT) score"
              }
            ]
          },
          "effectiveDateTime": "2018-03-11T16:07:54+00:00",
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
          "id": "mmrc_curr",
          "status": "preliminary",
          "valueQuantity": {
            "value": 0,
            "system": "http://unitsofmeasure.org",
            "code": "{score}"
          },
          "effectiveDateTime": "2018-03-11T16:07:54+00:00",
          "code": {
            "coding": [
              {
                "system": "local-system",
                "code": "codedValue",
                "display": "modified Medical Research Council (mMRC) dyspnea score"
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
          "id": "copd_exacerbations_number_curr",
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
          },
          "effectiveDateTime": "2018-03-11T16:07:54+00:00",
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
        }
      ]
    }
  }
}

```

## Change Log

Version | Description
---- | ----
2.1 | FHIR resource parameters
