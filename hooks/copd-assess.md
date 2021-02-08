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
    "patientId": "1677163", 
    "medication": {
      "resourceType": "Medication",
      "id": "current_medication",
      "code": {
        "coding": [
          {
            "system": "http://anonymous.org/data/DrugTLaba",
            "code": "Laba",
            "display": "administer medication containing LABA"
          }
        ]
      }
    },
    "previousAssessment": {
      "resourceType": "Bundle",
      "entry": [
        {
          "resource": {
            "resourceType": "Observation", 
            "id": "copd_group_prev",
            "status": "final",
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "1097861000000108",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group"
                }
              ]
            },
            "valueCodeableConcept": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "1097881000000104",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group B"
                }
              ]
            },
            "referenceRange": [
              {
                "low": {
                  "system": "http://snomed.info/sct",
                  "code": "1097871000000101",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group A"
                }
              },
              {
                "low": {
                  "system": "http://snomed.info/sct",
                  "code": "1097881000000104",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group B"
                }
              },
              {
                "high": {
                  "system": "http://snomed.info/sct",
                  "code": "1097891000000102",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group C"
                }
              },
              {
                "high": {
                  "system": "http://snomed.info/sct",
                  "code": "1097901000000101",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group D"
                }
              }
            ]
          }
        },
        {
          "resource": {
            "resourceType": "Observation",
            "id": "cat_score_prev",
            "status": "final",
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "446660005",
                  "display": "Chronic obstructive pulmonary disease assessment test score"
                }
              ]
            },
            "valueInteger": 10,
            "referenceRange": [
              {
                "low": {
                  "value": 0,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "high": {
                  "value": 9,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                }
              },
              {
                "low": {
                  "value": 10,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "high": {
                  "value": 20,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                }
              },
              {
                "low": {
                  "value": 21,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "high": {
                  "value": 30,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                }
              },
              {
                "low": {
                  "value": 31,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "high": {
                  "value": 40,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                }
              }
            ]
          }
        },
        {
          "resource": {
            "resourceType": "Observation",
            "id": "mmrc_prev",
            "status": "final",
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "450755002",
                  "display": "Medical Research Council Dyspnoea scale score"
                }
              ]
            },
            "valueInteger": 2,
            "referenceRange": [
              {
                "low": {
                  "value": 0,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "no breathlessness"
              },
              {
                "low": {
                  "value": 1,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "breathless when hurrying or walking up a hill"
              },
              {
                "high": {
                  "value": 2,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "breathless when walking slower than people of same age or has to stop when walking"
              },
              {
                "high": {
                  "value": 3,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "Stops for breath after walking 100 yards, or after a few minutes on level ground"
              },
              {
                "high": {
                  "value": 4,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "Too breathless to leave the house, or breathless when dressing"
              }
            ]
          }
        },
        {
          "resource": {
            "resourceType": "Observation",
            "id": "copd_exacerbations_number_prev",
            "status": "final",
            "valueInteger": 0,
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "723245007",
                  "display": "Number of chronic obstructive pulmonary disease exacerbations in past year"
                }
              ]
            }
          }
        }
      ]
    },
    "currentAssessment": {
      "resourceType": "Bundle",
      "entry": [
        {
          "resource": {
            "resourceType": "Observation",
            "id": "cat_score_curr",
            "status": "preliminary",
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "446660005",
                  "display": "Chronic obstructive pulmonary disease assessment test score"
                }
              ]
            },
            "valueInteger": 8,
            "referenceRange": [
              {
                "low": {
                  "value": 0,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "high": {
                  "value": 9,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "Low impact level"
              },
              {
                "low": {
                  "value": 10,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "high": {
                  "value": 20,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "Medium impact level"
              },
              {
                "low": {
                  "value": 21,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "high": {
                  "value": 30,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "High impact level"
              },
              {
                "low": {
                  "value": 31,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "high": {
                  "value": 40,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "Very high impact level"
              }
            ]
          }
        },
        {
          "resource": {
            "resourceType": "Observation",
            "id": "mmrc_curr",
            "status": "preliminary",
            "valueInteger": 1,
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "450755002",
                  "display": "Medical Research Council Dyspnoea scale score"
                }
              ]
            },
            "referenceRange": [
              {
                "low": {
                  "value": 0,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "no breathlessness"
              },
              {
                "low": {
                  "value": 1,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "breathless when hurrying or walking up a hill"
              },
              {
                "high": {
                  "value": 2,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "breathless when walking slower than people of same age or has to stop when walking"
              },
              {
                "high": {
                  "value": 3,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "Stops for breath after walking 100 yards, or after a few minutes on level ground"
              },
              {
                "high": {
                  "value": 4,
                  "system": "http://unitsofmeasure.org",
                  "code": "{score}"
                },
                "text": "Too breathless to leave the house, or breathless when dressing"
              }
            ]
          }
        },
        {
          "resource": {
            "resourceType": "Observation",
            "id": "copd_exacerbations_number_curr",
            "status": "preliminary",
            "valueInteger": 0,
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "723245007",
                  "display": "Number of chronic obstructive pulmonary disease exacerbations in past year"
                }
              ]
            }
          }
        }
      ]
    },
    "asthma": {
      "resourceType": "Condition",
      "id": "asthma",
      "code": {
        "coding": [
          {
            "system": "http://snomed.info/sct",
            "code": "195967001",
            "display": "Asthma"
          }
        ]
      },
      "subject": {
        "reference": "Patient/1677163"
      }
    }
  }
}



```

## Change Log

Version | Description
---- | ----
2.1 | FHIR resource parameters
