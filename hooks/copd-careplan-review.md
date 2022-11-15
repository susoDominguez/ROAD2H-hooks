# <mark>`copd-careplan-review`</mark>

| Metadata | Value
| ---- | ----
| specificationVersion | 2.2
| hookVersion | 1.3
| hookMaturity | [0 - Draft](../../specification/1.0/#hook-maturity-model)

## Workflow

<mark>The `copd-careplan-review` hook fires when a practitioner has assessed the COPD symptom severity case of the patient, that is, assigning a GOLD 2017 COPD group (A, B, C, D) and selecting one or more COPD drug types or type combinations for CDS. The CDS Service then provides a collection of patient-centered care plan proposals, based on the GOLD 2017 COPD guideline, where each proposal contains a collection of conflict-free clinical recommendations which take into account the practitioner's choices -GOLD COPD group and treatment pathways-, immunization, lifestyle -e.g., smoking status- and comorbidities present in the patient's record, in particular cardiovascular and chronic kidney diseases, which could potentially interfere with the COPD treatment.</mark>

## Context

<mark>The context contains the GOLD 20017 COPD group selected by the clinician, information of the current COPD assessment and the collection of one or more COPd drug types or type combinations selectected by the pulmonologist for CDS, alongside another collection including the initial personalised COPD drug types suggested by the COPD CDS system, and taken from GOLD 2017 guideline, as part of the response card for the same patient and for the hook 'copd-assess'. This CDS-suggested collection of treatments is submitted as input to the argumentation engine to apply as potential alternative treatments when conflict arises. Additionally, there is some lifestyle and demographics information as well as comorbidities and immunization status data.</mark>

Field | Optionality | Prefetch Token | Type | Description
----- | -------- | ---- | ---- | ----
<mark>`encounterId`</mark> | REQUIRED | No | *string* | <mark>identifier of current encounter.</mark>
<mark>`patientId`</mark> | REQUIRED | No | *string* | <mark>identifier of current patient.</mark>
<mark>`birthDate`</mark> | REQUIRED | No | *string* | <mark>date of birth of patient, used to identify whether Pneumococcal vaccine should be suggested for patients of 65 years of age or older.</mark>
<mark>`smokingStatus`</mark> | REQUIRED | No | *object* | <mark>FHIR Observation which identifies current patient as either a regular smoker or not.</mark>
<mark>`comorbidities`</mark> | OPTIONAL | No | *object* | <mark>FHIR Bundle of Conditions representing CKD or CVD diagnosis which are currently active in the patient's record.</mark>
<mark>`immunizationStatus`</mark> | REQUIRED | No | *object* | <mark>FHIR Bundle of Immunizations denoting whether the patient has taken the annual influenza vaccine, or the pneumococcal vaccine if 65 years of age or older.</mark>
<mark>`copdAssessment`</mark> | REQUIRED | No | *object* | <mark>FHIR Bundle composed of Observation and a Medication Bundle, representing the identified GOLD 2017 COPD group and the clinician-selected COPD drug types -implemented as Medication resources- being taking into consideration to treat the patient, respectively.</mark>
<mark>`suggestedTreatmentsByCdsService`</mark> | OPTIONAL | No | *object* | <mark>FHIR Bundle of Medications as suggested by the COPD CDS service. This object is fed to the conflict resolution engine to provide alternative treatments to the selected ones in case conflicts arise with the selected choices.</mark>

### Example


```json
{
    "hookInstance": "d1577c69-dfbe-44ad-ba6d-3e05e953b2ea",
    "hook": "copd-careplan-review",
    "context": {
        "encounterId": "285064-0",
        "patientId": "1677163",
        "birthDate": "1960-08-03T00:00:00+01:00",
        "smokingStatus": {
            "resourceType": "Observation",
            "id": "smoking_status",
            "status": "final",
            "code": {
                "coding": [
                    {
                        "system": "http://snomed.info/sct",
                        "code": "229819007",
                        "display": "Tobacco use and exposure (observable entity)"
                    }
                ]
            },
            "valueCodeableConcept": {
                "coding": [
                    {
                        "system": "http://snomed.info/sct",
                        "code": "77176002",
                        "display": "Smoker"
                    }
                ]
            },
            "referenceRange": [
                {
                    "low": {
                        "system": "http://snomed.info/sct",
                        "code": "8392000",
                        "display": "Non-smoker"
                    }
                },
                {
                    "high": {
                        "system": "http://snomed.info/sct",
                        "code": "77176002",
                        "display": "Smoker"
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
                        "id": "ht",
                        "code": {
                            "coding": [
                                {
                                    "system": "http://snomed.info/sct",
                                    "code": "38341003",
                                    "display": "Hypertensive disorder"
                                }
                            ]
                        },
                        "subject": {
                            "reference": "Patient/1677163"
                        }
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
                        "status": "not-done",
                        "vaccineCode": {
                            "coding": [
                                {
                                    "system": "http://snomed.info/sct",
                                    "code": "1181000221105",
                                    "display": "Vaccine product containing only Influenza virus antigen"
                                }
                            ]
                        },
                        "occurrenceDateTime": "2019-10-26T18:00:00+01:00"
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
                                    "display": "Pneumococcal vaccine"
                                }
                            ]
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
                                    "code": "1097861000000108",
                                    "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group"
                                }
                            ],
                            "text": "COPD group"
                        },
                        "valueCodeableConcept": {
                            "coding": [
                                {
                                    "system": "http://snomed.info/sct",
                                    "code": "1097901000000101",
                                    "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group D"
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
                        "resourceType": "Bundle",
                        "id": "selected_treatments",
                        "type": "collection",
                        "entry": [
                            {
                                "resource": {
                                    "resourceType": "Medication",
                                    "id": "DrugTLama",
                                    "code": {
                                        "coding": [
                                            {
                                                "code": "Lama",
                                                "display": "administer medication containing a combination of LAMA"
                                            }
                                        ]
                                    }
                                }
                            },
                            {
                                "resource": {
                                    "resourceType": "Medication",
                                    "id": "DrugCatLabaLamaIcs",
                                    "code": {
                                        "coding": [
                                            {
                                                "code": "LabaLamaIcs",
                                                "display": "administer medication containing a combination of LABA + LAMA + ICS"
                                            }
                                        ]
                                    }
                                }
                            },
                            {
                                "resource": {
                                    "resourceType": "Medication",
                                    "id": "DrugCatLabaIcs",
                                    "code": {
                                        "coding": [
                                            {
                                                "code": "LabaIcs",
                                                "display": "administer medication containing a combination of LABA + ICS"
                                            }
                                        ]
                                    }
                                }
                            },
                            {
                                "resource": {
                                    "resourceType": "Medication",
                                    "id": "DrugCatLabaLama",
                                    "code": {
                                        "coding": [
                                            {
                                                "code": "LabaLama",
                                                "display": "administer medication containing a combination of LABA + LAMA"
                                            }
                                        ]
                                    }
                                }
                            }
                        ]
                    }
                }
            ]
        },
        "suggestedTreatmentsByCdsService": {
            "resourceType": "Bundle",
            "id": "suggested_treatments",
            "type": "collection",
            "entry": [
                {
                    "resource": {
                        "resourceType": "Medication",
                        "id": "DrugTLama",
                        "code": {
                            "coding": [
                                {
                                    "code": "Lama",
                                    "display": "administer medication containing a combination of LAMA"
                                }
                            ]
                        }
                    }
                },
                {
                    "resource": {
                        "resourceType": "Medication",
                        "id": "DrugCatLabaLamaIcs",
                        "code": {
                            "coding": [
                                {
                                    "code": "LabaLamaIcs",
                                    "display": "administer medication containing a combination of LABA + LAMA + ICS"
                                }
                            ]
                        }
                    }
                },
                {
                    "resource": {
                        "resourceType": "Medication",
                        "id": "DrugCatLabaIcs",
                        "code": {
                            "coding": [
                                {
                                    "code": "LabaIcs",
                                    "display": "administer medication containing a combination of LABA + ICS"
                                }
                            ]
                        }
                    }
                },
                {
                    "resource": {
                        "resourceType": "Medication",
                        "id": "DrugCatLabaLama",
                        "code": {
                            "coding": [
                                {
                                    "code": "LabaLama",
                                    "display": "administer medication containing a combination of LABA + LAMA"
                                }
                            ]
                        }
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
2.1 | tokenised approach
