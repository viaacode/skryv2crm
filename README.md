# skryv2crm

Integration between Skryv and CRM

## Introduction

This interface tranfers company and contact data from Skryv to CRM.

In Skryv there are 3 webhooks configured:
- milestone
- process
- document

Every webhooks sends a json message to a different Mule endpoint
- https://services-qas.viaa.be/skryv/event/milestone
- https://services-qas.viaa.be/skryv/event/process
- https://services-qas.viaa.be/skryv/event/document

We use OAUTH authentication:
- Authenticatie methode: Oauth-Client-Credentials
- IdP token endpoint: https://oas-qas.viaa.be/token
- Client ID : <......>
- Client Secret : <......>

## Overview

Excel sheet : "Integratie Teamleader-Skryv Metadatamapping" beschrijft alle scenarios met de CRM field mapping.

Link : https://docs.google.com/spreadsheets/d/1-orV084eUbagKI1eyIHj-Tdqneh5vJN45_ZNFvri4Ug/edit#gid=1102311688


| Skryv          | CRM           | Action  |
| -------------- |:-------------:| -------:|
| ITV + SWO + SA | Company       | Update  |
| ITV Contact    | Company       | Update  |
|                | Contact       | Link    |
|                | Contact       | Create  |

## Scenarios

### 1 - Opgestuurd naar CP (proces ITV opgestart) - Start event (start proces)

Method	POST 		http://localhost:10009/skryv/event/process

Header	Content-Type	application/json
        x-skryv-event   process

Body

```json
{  
   "action":"created",
   "dossier":{  
      "id":"b43283bf-8cf9-4ece-91dd-701b47b1837f",
      "label":"TEST 2 17042018",
      "externalId":"OR-j960b2x",
      "description":"",
      "dossierDefinition":"90d24d34-b5b3-4942-8504-b6d76dd86ccb",
      "createdAt":"2018-04-17T07:38:10.000+0000",
      "updatedAt":"2018-04-17T07:38:10.000+0000",
      "creator":"33aeb47c-d027-1036-8f5f-d369bf7ac4f1",
      "active":true,
      "communications":[  

      ]
   },
   "process":{  
      "id":"337028",
      "businessKey":"b43283bf-8cf9-4ece-91dd-701b47b1837f",
      "processDefinitionId":"Intentieverklaring_v2:5:331612",
      "processDefinitionKey":"Intentieverklaring_v2",
      "startTime":"2018-04-17T07:41:23.000+0000",
      "startActivityId":"StartEvent_10e3ss5"
   }
}
```

### 2 - Ingevuld door CP, en goedgekeurd door VIAA - Milestone "Geen interesse"

Method	POST 		http://localhost:10009/skryv/event/milestone

Header	Content-Type	application/json
        x-skryv-event   milestone

Body

```json
{  
   "action":"reached",
   "dossier":{  
      "id":"b43283bf-8cf9-4ece-91dd-701b47b1837f",
      "label":"TEST 2 17042018",
      "externalId":"OR-j960b2x",
      "description":"",
      "dossierDefinition":"90d24d34-b5b3-4942-8504-b6d76dd86ccb",
      "createdAt":"2018-04-17T07:38:10.000+0000",
      "updatedAt":"2018-04-17T07:38:10.000+0000",
      "creator":"33aeb47c-d027-1036-8f5f-d369bf7ac4f1",
      "active":true,
      "communications":[  

      ]
   },
   "milestone":{  
      "id":"159bbb84-544b-46ed-9a5a-c348ed5e95db",
      "dossierId":"b43283bf-8cf9-4ece-91dd-701b47b1837f",
      "key":"IntermediateThrowEvent_DS_ITV_geeninteressesamenwerkingVIAA",
      "status":"Geen interesse",
      "timestamp":"2018-04-17T08:01:18.063Z"
   }
}
```

### 3 - Ingevuld door CP, en goedgekeurd door VIAA - Milestone "Misschien later samenwerking"

Method	POST 		http://localhost:10009/skryv/event/milestone

Header	Content-Type	application/json
        x-skryv-event   milestone

Body

```json
{  
   "action":"reached",
   "dossier":{  
      "id":"5724b881-b0a4-4760-8587-0092c2946a9f",
      "label":"TEST 3 17042018",
      "externalId":"OR-j960b2x",
      "description":"",
      "dossierDefinition":"90d24d34-b5b3-4942-8504-b6d76dd86ccb",
      "createdAt":"2018-04-17T08:10:01.000+0000",
      "updatedAt":"2018-04-17T08:10:01.000+0000",
      "creator":"33aeb47c-d027-1036-8f5f-d369bf7ac4f1",
      "active":true,
      "communications":[  

      ]
   },
   "milestone":{  
      "id":"dfd0523b-d9ba-4d8a-bf32-3e8decd0f539",
      "dossierId":"5724b881-b0a4-4760-8587-0092c2946a9f",
      "key":"IntermediateThrowEvent_DS_ITV_misschienlatersamenwerkingVIAA",
      "status":"Misschien later samenwerking",
      "timestamp":"2018-04-17T08:11:25.580Z"
   }
}
```

### 4 - Ingevuld door CP, en goedgekeurd door VIAA - Milestone "Akkoord en opstart"

Method	POST 		http://localhost:10009/skryv/event/milestone

Header	Content-Type	application/json
        x-skryv-event   milestone

Body

```json
{  
   "action":"reached",
   "dossier":{  
      "id":"16f897d4-937b-466e-8f70-f48b7514f09e",
      "label":"TEST 4 17042018",
      "externalId":"OR-j38kk4p",
      "description":"",
      "dossierDefinition":"90d24d34-b5b3-4942-8504-b6d76dd86ccb",
      "createdAt":"2018-04-17T08:14:48.000+0000",
      "updatedAt":"2018-04-17T08:14:48.000+0000",
      "creator":"33aeb47c-d027-1036-8f5f-d369bf7ac4f1",
      "active":true,
      "communications":[  

      ]
   },
   "milestone":{  
      "id":"df3384f0-e91b-4f31-b074-4cc906bf09f4",
      "dossierId":"16f897d4-937b-466e-8f70-f48b7514f09e",
      "key":"IntermediateThrowEvent_DS_ITV_akkoordITVenopstart",
      "status":"Akkoord en opstart",
      "timestamp":"2018-04-17T08:16:09.728Z"
   }
}
```

### 5 - Ingevuld door CP, en goedgekeurd door VIAA - Milestone "Akkoord, geen opstart"

Method	POST 		http://localhost:10009/skryv/event/milestone

Header	Content-Type	application/json
        x-skryv-event   milestone

Body

```json
{  
   "action":"reached",
   "dossier":{  
      "id":"29bb7dfb-5c8a-4b05-bb7a-346cad95dfd2",
      "label":"TEST 5 17042018",
      "externalId":"OR-j38kk4p",
      "description":"",
      "dossierDefinition":"90d24d34-b5b3-4942-8504-b6d76dd86ccb",
      "createdAt":"2018-04-17T08:18:25.000+0000",
      "updatedAt":"2018-04-17T08:18:25.000+0000",
      "creator":"33aeb47c-d027-1036-8f5f-d369bf7ac4f1",
      "active":true,
      "communications":[  

      ]
   },
   "milestone":{  
      "id":"ca8257a8-fb69-4506-99d1-a4ce5ca1be15",
      "dossierId":"29bb7dfb-5c8a-4b05-bb7a-346cad95dfd2",
      "key":"IntermediateThrowEvent_DS_ITV_akkoordITVgeenopstart",
      "status":"Akkoord, geen opstart",
      "timestamp":"2018-04-17T08:19:53.418Z"
   }
}
```

### 6 - Ingevuld door CP, en goedgekeurd door VIAA - Milestone "Interesse, niet akkoord met SWO"

Method	POST 		http://localhost:10009/skryv/event/milestone

Header	Content-Type	application/json
        x-skryv-event   milestone

Body

```json
{  
   "action":"reached",
   "dossier":{  
      "id":"7a32b5f3-e63c-4519-ad90-d8cf850554dc",
      "label":"TEST 6 17042018",
      "externalId":"OR-j960b2x",
      "description":"",
      "dossierDefinition":"90d24d34-b5b3-4942-8504-b6d76dd86ccb",
      "createdAt":"2018-04-17T08:21:41.000+0000",
      "updatedAt":"2018-04-17T08:21:41.000+0000",
      "creator":"33aeb47c-d027-1036-8f5f-d369bf7ac4f1",
      "active":true,
      "communications":[  

      ]
   },
   "milestone":{  
      "id":"cc357c99-e779-4466-9cfb-e67a5333e222",
      "dossierId":"7a32b5f3-e63c-4519-ad90-d8cf850554dc",
      "key":"IntermediateThrowEvent_DS_ITV_interesse_nietakkoordSWO",
      "status":"Interesse, niet akkoord SWO",
      "timestamp":"2018-04-17T08:23:13.340Z"
   }
}
```

### 7a - Ingevuld door CP, en goedgekeurd door VIAA - Milestone "Interesse, niet akkoord met SWO"

Method	POST 		http://localhost:10009/skryv/event/process

Header	Content-Type	application/json
        x-skryv-event   process

Body

```json
{  
   "action":"created",
   "dossier":{  
      "id":"db2f78a0-f709-44ca-b626-4d31ec8af8f1",
      "label":"TEST 17042018",
      "externalId":"OR-j960b2x",
      "description":"",
      "dossierDefinition":"90d24d34-b5b3-4942-8504-b6d76dd86ccb",
      "createdAt":"2018-04-17T07:33:09.000+0000",
      "updatedAt":"2018-04-17T07:33:09.000+0000",
      "creator":"33aeb47c-d027-1036-8f5f-d369bf7ac4f1",
      "active":true,
      "communications":[  

      ]
   },
   "process":{  
      "id":"337717",
      "businessKey":"db2f78a0-f709-44ca-b626-4d31ec8af8f1",
      "processDefinitionId":"service_agreement:5:331616",
      "processDefinitionKey":"service_agreement",
      "startTime":"2018-04-17T08:33:18.000+0000",
      "startActivityId":"StartEvent_SA"
   }
}
```

### 7b - Niet akkoord met SWO - Document "Validatie SWO documenten"

Method	POST 		http://localhost:10009/skryv/event/process

Header	Content-Type	application/json
        x-skryv-event   process

Body

```json
{  
   "action":"created",
   "dossier":{  
      "id":"db2f78a0-f709-44ca-b626-4d31ec8af8f1",
      "label":"TEST 17042018",
      "externalId":"OR-j960b2x",
      "description":"",
      "dossierDefinition":"90d24d34-b5b3-4942-8504-b6d76dd86ccb",
      "createdAt":"2018-04-17T07:33:09.000+0000",
      "updatedAt":"2018-04-17T07:33:09.000+0000",
      "creator":"33aeb47c-d027-1036-8f5f-d369bf7ac4f1",
      "active":true,
      "communications":[  

      ]
   },
   "process":{  
      "id":"337717",
      "businessKey":"db2f78a0-f709-44ca-b626-4d31ec8af8f1",
      "processDefinitionId":"service_agreement:5:331616",
      "processDefinitionKey":"service_agreement",
      "startTime":"2018-04-17T08:33:18.000+0000",
      "startActivityId":"StartEvent_SA"
   }
}
```
### 8 - Opstarten process - Start event (start proces)

Method	POST 		http://localhost:10009/skryv/event/document

Header	Content-Type	application/json
        x-skryv-event   document

Body

```json
{  
   "action":"created",
   "dossier":{  
      "id":"db2f78a0-f709-44ca-b626-4d31ec8af8f1",
      "label":"TEST 17042018",
      "externalId":"OR-j960b2x",
      "description":"",
      "dossierDefinition":"90d24d34-b5b3-4942-8504-b6d76dd86ccb",
      "createdAt":"2018-04-17T07:33:09.000+0000",
      "updatedAt":"2018-04-17T07:33:09.000+0000",
      "creator":"33aeb47c-d027-1036-8f5f-d369bf7ac4f1",
      "active":true,
      "communications":[  

      ]
   },
   "process":{  
      "id":"337717",
      "businessKey":"db2f78a0-f709-44ca-b626-4d31ec8af8f1",
      "processDefinitionId":"service_agreement:5:331616",
      "processDefinitionKey":"service_agreement",
      "startTime":"2018-04-17T08:33:18.000+0000",
      "startActivityId":"StartEvent_SA"
   }
}
```
### 9 - Getekend opgeladen en goedgekeurd door VIAA - Eind event (einde proces)	

Method	POST 		http://localhost:10009/skryv/event/document

Header	Content-Type	application/json
        x-skryv-event   document

Body

```json
{
  "action": "updated",
  "dossier": {
    "id": "24db155a-7001-4544-b8ec-dd6d61d061ea",
    "label": "test archief B",
    "externalId": "OR-z892h1h",
    "description": "",
    "dossierDefinition": "90d24d34-b5b3-4942-8504-b6d76dd86ccb",
    "createdAt": "2018-05-16T11:36:15.000+0000",
    "updatedAt": "2018-05-16T11:36:15.000+0000",
    "creator": "33aeb47c-d027-1036-8f5f-d369bf7ac4f1",
    "active": true,
    "communications": []
  },
  "document": {
    "id": "82594fd5-830d-4eaa-9cb5-171b4cf868c4",
    "version": 2,
    "definition": "14e0553d-ceef-4950-b8fc-3feea7baf0c1",
    "definitionLabel": "Wederzijds getekende Service Agreement",
    "definitionKey": "getekende_service_agreement",
    "readOnly": false,
    "document": {
      "value": {
        "algemene_contactgegevens_organisatie": {
          "naam_instelling": {
            "CP": "ADVN",
            "CP lowercase /naamgeving logo = cplowercase.png": "advn",
            "CP-ID": "OR-4x54g1p",
            "SubCPs": "",
            "Adres": "Lange Leemstraat 26, 2018 Antwerpen",
            "Straat en nummer": "Lange Leemstraat 26",
            "Postcode": 2018,
            "Stad": "Antwerpen",
            "Provincie": "Antwerpen",
            "BTW-nummer": "BE0431.020.884",
            "Telefoonnummer": "+32 3 225 18 37",
            "Website": "http://www.advn.be",
            "Sector": "Cultureel Erfgoed",
            "Type Organisatie": "Archief"
          },
          "type_organisatie": "Archief",
          "sector": "Cultureel Erfgoed",
          "adres": "Lange Leemstraat 26, 2018 Antwerpen",
          "btwnummer": "BE0431.020.884",
          "telefoonnummer": "+32 3 225 18 37",
          "website": "http://www.advn.be",
          "cpid": "OR-4x54g1p"
        },
        "contactpersoon_service_agreement": {
          "voornaam": "tine",
          "familienaam": "philips",
          "emailadres": "tine.philips@viaa.be"
        },
        "logo_content_partner": {
          "logocustomisatie_op_proxy_gewenst": {
            "selectedOption": "neen"
          }
        },
        "viaa_contactpersoon_service_agreement": {
          "voornaam_1": {
            "Voornaam": "Eva",
            "Familienaam": "Verdoodt",
            "Emailadres": "eva.verdoodt@viaa.be",
            "Telefoonnummer": "+32 494 34 19 36"
          },
          "familienaam_1": "Verdoodt",
          "emailadres_1": "eva.verdoodt@viaa.be",
          "telefoonnummer_1": "+32 494 34 19 36"
        },
        "dragers": [
          {
            "type_drager": {
              "Drager": "Betacam SP",
              "Type": "Video",
              "Wave": "Wave 1",
              "Gemiddelde duurtijd per drager (in uur)": 0.7
            },
            "kwantificatie": {
              "aantal_aangeleverde_objecten": 7,
              "gemiddelde_duurtijd_per_drager_uur": 0.7,
              "totaal_aantal_uur": 4.9,
              "gbh": 50
            },
            "type": "Video",
            "digitalisatiegolf": "Wave 1",
            "archiefformaat_van_het_dataobject": {
              "archiefformaat__codec": "jpeg2000",
              "archiefformaat__container": "MXF",
              "archiefformaat__specs": "120 Mbps"
            },
            "proxyformaat_van_het_dataobject": {
              "proxyformaat__codec": "h264",
              "proxyformaat__container": "mp4",
              "proxyformaat__specs": "1.5 mbit/sec"
            }
          }
        ],
        "totale_grootte_van_de_collectie_tb": 0.25,
        "geschatte_grootte_in_tb_1": 0.25,
        "geef_hieronder_aan_op_welke_manier_de_metadata_aangeleverd_zal_worden_door_de_cp": {
          "selectedOption": "een_xml_volgens_het_viaa_metadata_schema"
        },
        "naast_titel_korte_beschrijving_en_duurtijd_staat_de_cp_viaa_ook_toe_om_de_premis_beschrijvende_en_technische_metadata_publiek_beschikbaar_te_maken": {
          "selectedOption": "ja_1"
        },
        "de_cp_wenst_een_kopie_te_ontvangen_van_het_archiefformaat_na_digitalisering_en_opname_in_het_archief": {
          "selectedOption": "neen_2"
        },
        "aanleverschema_1": {
          "id": "attachment_aanleverschema_1",
          "base": "6721109c-cde0-495f-898a-a107072ef48a",
          "contentType": "image/png",
          "size": 52610,
          "name": "Schermafbeelding 2018-05-07 om 11.50.03.png"
        },
        "de_cp_heeft_kennis_genomen_van_het_aanleverschema_en_gaat_hiermee_akkoord": {
          "selectedOption": "ja_3"
        },
        "getekende_service_agreement_cp_version": {
          "id": "attachment_getekende_service_agreement_cp_version",
          "base": "5f72d07b-9e83-4063-a319-b880fee499f7",
          "contentType": "image/png",
          "size": 52610,
          "name": "Schermafbeelding 2018-05-07 om 11.50.03.png"
        },
        "voorbehouden_voor_de_content_partner": {},
        "voorbehouden_voor_viaa": {},
        "subcps": {
          "naam_van_de_subcps": ""
        },
        "getekende_service_agreement_1": {
          "id": "attachment_getekende_service_agreement_1",
          "base": "02f13490-5eee-4d72-8f63-df85b8f427da",
          "contentType": "image/png",
          "size": 52610,
          "name": "Schermafbeelding 2018-05-07 om 11.50.03.png"
        }
      },
      "lastSavedSnapshot": {
        "algemene_contactgegevens_organisatie": {
          "naam_instelling": {
            "CP": "ADVN",
            "CP lowercase /naamgeving logo = cplowercase.png": "advn",
            "CP-ID": "OR-4x54g1p",
            "SubCPs": "",
            "Adres": "Lange Leemstraat 26, 2018 Antwerpen",
            "Straat en nummer": "Lange Leemstraat 26",
            "Postcode": 2018,
            "Stad": "Antwerpen",
            "Provincie": "Antwerpen",
            "BTW-nummer": "BE0431.020.884",
            "Telefoonnummer": "+32 3 225 18 37",
            "Website": "http://www.advn.be",
            "Sector": "Cultureel Erfgoed",
            "Type Organisatie": "Archief"
          },
          "type_organisatie": "Archief",
          "sector": "Cultureel Erfgoed",
          "adres": "Lange Leemstraat 26, 2018 Antwerpen",
          "btwnummer": "BE0431.020.884",
          "telefoonnummer": "+32 3 225 18 37",
          "website": "http://www.advn.be",
          "cpid": "OR-4x54g1p"
        },
        "contactpersoon_service_agreement": {
          "voornaam": "tine",
          "familienaam": "philips",
          "emailadres": "tine.philips@viaa.be"
        },
        "logo_content_partner": {
          "logocustomisatie_op_proxy_gewenst": {
            "selectedOption": "neen"
          }
        },
        "viaa_contactpersoon_service_agreement": {
          "voornaam_1": {
            "Voornaam": "Eva",
            "Familienaam": "Verdoodt",
            "Emailadres": "eva.verdoodt@viaa.be",
            "Telefoonnummer": "+32 494 34 19 36"
          },
          "familienaam_1": "Verdoodt",
          "emailadres_1": "eva.verdoodt@viaa.be",
          "telefoonnummer_1": "+32 494 34 19 36"
        },
        "dragers": [
          {
            "type_drager": {
              "Drager": "Betacam SP",
              "Type": "Video",
              "Wave": "Wave 1",
              "Gemiddelde duurtijd per drager (in uur)": 0.7
            },
            "kwantificatie": {
              "aantal_aangeleverde_objecten": 7,
              "gemiddelde_duurtijd_per_drager_uur": 0.7,
              "totaal_aantal_uur": 4.9,
              "gbh": 50
            },
            "type": "Video",
            "digitalisatiegolf": "Wave 1",
            "archiefformaat_van_het_dataobject": {
              "archiefformaat__codec": "jpeg2000",
              "archiefformaat__container": "MXF",
              "archiefformaat__specs": "120 Mbps"
            },
            "proxyformaat_van_het_dataobject": {
              "proxyformaat__codec": "h264",
              "proxyformaat__container": "mp4",
              "proxyformaat__specs": "1.5 mbit/sec"
            }
          }
        ],
        "totale_grootte_van_de_collectie_tb": 0.25,
        "geschatte_grootte_in_tb_1": 0.25,
        "geef_hieronder_aan_op_welke_manier_de_metadata_aangeleverd_zal_worden_door_de_cp": {
          "selectedOption": "een_xml_volgens_het_viaa_metadata_schema"
        },
        "naast_titel_korte_beschrijving_en_duurtijd_staat_de_cp_viaa_ook_toe_om_de_premis_beschrijvende_en_technische_metadata_publiek_beschikbaar_te_maken": {
          "selectedOption": "ja_1"
        },
        "de_cp_wenst_een_kopie_te_ontvangen_van_het_archiefformaat_na_digitalisering_en_opname_in_het_archief": {
          "selectedOption": "neen_2"
        },
        "aanleverschema_1": {
          "id": "attachment_aanleverschema_1",
          "base": "6721109c-cde0-495f-898a-a107072ef48a",
          "contentType": "image/png",
          "size": 52610,
          "name": "Schermafbeelding 2018-05-07 om 11.50.03.png"
        },
        "de_cp_heeft_kennis_genomen_van_het_aanleverschema_en_gaat_hiermee_akkoord": {
          "selectedOption": "ja_3"
        },
        "getekende_service_agreement_cp_version": {
          "id": "attachment_getekende_service_agreement_cp_version",
          "base": "5f72d07b-9e83-4063-a319-b880fee499f7",
          "contentType": "image/png",
          "size": 52610,
          "name": "Schermafbeelding 2018-05-07 om 11.50.03.png"
        },
        "voorbehouden_voor_de_content_partner": {},
        "voorbehouden_voor_viaa": {},
        "subcps": {
          "naam_van_de_subcps": ""
        }
      },
      "hasUnsavedChanges": true
    },
    "createdAt": "2018-05-16T11:39:42.000+0000",
    "updatedAt": "2018-05-16T11:40:05.000+0000",
    "links": [
      {
        "resourceType": "DOSSIER",
        "resourceId": "24db155a-7001-4544-b8ec-dd6d61d061ea",
        "linkType": "LINK"
      },
      {
        "resourceType": "PROCESS",
        "resourceId": "368411",
        "linkType": "SCOPE"
      }
    ]
  }
}
```





