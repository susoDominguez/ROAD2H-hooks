# <mark>`copd-careplan-select`</mark>

| Metadata | Value
| ---- | ----
| specificationVersion | 2.1
| hookVersion | 1.3
| hookMaturity | [0 - Draft](../../specification/1.0/#hook-maturity-model)

## Workflow

<mark>The `copd-careplan-select` hook fires when a practitioner has assessed the COPD case of the patient, assigning a GOLD COPD group (A, B, C, D) along with a selection of potential COPD medication treatments. The CDS Service then provides a collection of personalised care plans based on GOLD COPD 2017 where each of which contains non-conflictive clinical recommendations taking into account practitioner/patient choices -GOLD COPD group and medications-,immunization, lifestyle -e.g., smoking status- and comorbidities present in the patient's record which could potentially interfere with the COPD treatment.</mark>

## Context

<mark>The context contains the GOLD COPD group selected by the clinician, a list of selected COPD-related medications the practitioner is considering</mark>

Field | Optionality | Prefetch Token | Type | Description
----- | -------- | ---- | ---- | ----
<mark>`encounterId`</mark> | OPTIONAL | No | *string* | <mark>identifier of current encounter</mark>
<mark>`birthDate`</mark> | REQUIRED | No | *string* | <mark>date of birth of patient, used to identify whether Pneumococcal vaccine should be suggested for patients of 65 years of age or older</mark>
<mark>`smokingStatus`</mark> | REQUIRED | No | *object* | <mark>Observation which identifies current patient as either a regular smoker or not</mark>
<mark>`comorbidities`</mark> | OPTIONAL | No | *object* | <mark>FHIR Bundle of Conditions representing CKD or CVD diagnosis which are present in the patient's record</mark>
<mark>`immunizationStatus`</mark> | REQUIRED | No | *object* | <mark>FHIR Bundle of Immunizations denoting whether the patient has taken the annual influenza vaccine or the pneumococcal vaccine</mark>
<mark>`copdAssessment`</mark> | REQUIRED | No | *object* | <mark>FHIR Bundle of Observation and CarePlan representing the identified GOLD COPD group for the former, and COPD treatments -implemented as Medication resources- suitable for the identified group, for the latter. Observation and Medication resources have `preliminary` status</mark>
<mark>`preferences`</mark> | OPTIONAL | No | *array* | <mark>The FHIR id(s) of the preferred  medication(s) from the initial selection made by the practitioner.
The preferences field references FHIR resources from the CarePlan in the copdAssessment Bundle. For example, Medication/DrugTLaba .</mark>

### Example


```json
{
  "hookInstance": "d1577c69-dfbe-44ad-ba6d-3e05e953b2ea",
  "fhirServer": "https://example.org/fhir",
  "hook": "copd-careplan-select",
  "context": {
    "encounterId": "1234", 
    "birthDate": "1970-10-03T00:00:00+01:00", 
    "smokingStatus": {
      "resourceType": "Observation",
      "id": "smoking_status",
      "status": "final",
      "code": {
        "coding": [
          {
            "system": "https://www.snomed.org/",
            "code": "229819007",
            "display": "Tobacco use and exposure (observable entity)"
          }
        ],
        "text": "Smoking status" 
      },
      "valueCodeableConcept": {
        "coding": [
          {
            "system": "http://snomed.info/sct",
            "code": "8392000",
            "display": "Non-smoker (finding)"
          }
        ],
        "text": "Non-smoker"
      },
      "referenceRange": [
        {
          "low": {
            "system": "http://snomed.info/sct",
            "code": "8392000",
            "display": "Non-smoker (finding)"
          },
          "type": {
            "text": "Non-smoker"
          }
        },
        {
          "high": {
            "system": "http://snomed.info/sct",
            "code": "77176002",
            "display": "Smoker (finding)"
          },
          "type": {
            "text": "Smoker"
          }
        }
      ]
    },
    "comorbidities": {
      "resourceType": "Bundle",
      "id": "comorbiditiesBundle",
      "type": "collection",
      "entry": [
        {
          "resource": {
            "resourceType": "Condition",
            "id": "cvd",
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "49601007",
                  "display": "Disorder of cardiovascular system (disorder)"
                }
              ],
              "text": "Cvd"
            },
            "subject": {
              "reference": "Patient/1677163"
            },
            "recordedDate": "2020-10-26T18:00:00+01:00"
          }
        },
        {
          "resource": {
            "resourceType": "Condition",
            "id": "ckd",
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "709044004",
                  "display": "Chronic kidney disease (disorder)"
                }
              ],
              "text": "Ckd"
            },
            "subject": {
              "reference": "Patient/1677163"
            },
            "recordedDate": "2020-10-26T18:00:00+01:00"
          }
        }
      ]
    },
    "immunizationStatus": {
      "resourceType": "Bundle",
      "id": "immunizationStatusBundle",
      "type": "collection",
      "entry": [
        {
          "resource": {
            "resourceType": "Immunization",
            "id": "influenza_immunization",
            "status": "completed",
            "vaccineCode": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "1181000221105",
                  "display": "Vaccine product containing only Influenza virus antigen (medicinal product)"
                }
              ],
              "text": "FLU vaccine"
            },
            "occurrenceDateTime": "2020-10-26T18:00:00+01:00"
          }
        },
        {
          "resource": {
            "resourceType": "Immunization",
            "id": "pneumococcal_immunization",
            "status": "not-done",
            "vaccineCode": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "333598008",
                  "display": "Pneumococcal vaccine (product)"
                }
              ],
              "text": "Pneumococcal vaccine"
            }
          }
        }
      ]
    },
    "copdAssessment": {
      "resourceType": "Bundle",
      "id": "copdAssessmentBundle",
			"type": "collection",
      "entry": [
        {
          "resource": {
            "resourceType": "Observation",
            "id": "copd_group_curr",
            "status": "preliminary",
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "13645005",
                  "display": "Chronic obstructive lung disease (disorder)"
                }
              ],
              "text": "COPD"
            },
            "valueCodeableConcept": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "313296004",
                  "display": "Mild chronic obstructive pulmonary disease (disorder)"
                }
              ],
              "text": "Copd group A"
            },
            "effectiveDateTime": "2018-03-11T16:07:54+00:00",
            "referenceRange": [
              {
                "low": {
                  "system": "http://snomed.info/sct",
                  "code": "313296004",
                  "display": "Mild chronic obstructive pulmonary disease (disorder)"
                },
                "type": {
                  "text": "Copd group A"
                }
              },
              {
                "low": {
                  "system": "http://snomed.info/sct",
                  "code": "313297008",
                  "display": "Moderate chronic obstructive pulmonary disease (disorder)"
                },
                "type": {
                  "text": "Copd group B"
                }
              },
              {
                "high": {
                  "system": "http://snomed.info/sct",
                  "code": "313299006",
                  "display": "Severe chronic obstructive pulmonary disease (disorder)"
                },
                "type": {
                  "text": "Copd group C"
                }
              },
              {
                "high": {
                  "system": "http://snomed.info/sct",
                  "code": "135836000",
                  "display": "End stage chronic obstructive pulmonary disease (disorder)"
                },
                "type": {
                  "text": "Copd group D"
                }
              }
            ]
          }
        },
        {
          "resourceType": "CarePlan",
          "id": "selected_treatments",
          "status": "active",
          "intent": "proposal",
          "subject": {
            "reference": "Patient/1677163",
            "display": "Link to patient"
          },
          "contained": [
            {
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
              }
            },
            {
              "resourceType": "Medication",
              "id": "DrugTLama",
              "code": {
                "coding": [
                  {
                    "system": "http://anonymous.org/data/DrugTLama",
                    "code": "Lama",
                    "display": "administer LAMA"
                  }
                ]
              }
            },
            {
              "resourceType": "Medication",
              "id": "DrugCatLabaLama",
              "code": {
                "coding": [
                  {
                    "system": "http://anonymous.org/data/DrugCatLabaLama",
                    "code": "LabaLama",
                    "display": "administer a combination of LABA and LAMA"
                  }
                ]
              }
            }
          ]
        }
      ]
    },
    "preferences": ["Medication/DrugTLaba"] 
  }
}


```

## Change Log

Version | Description
---- | ----
2.0 | tokenised approach
