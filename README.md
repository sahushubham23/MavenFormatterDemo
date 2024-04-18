{
    "channels": {
        "destinations": {
            "documentId": "sanjkauu1d",
            "output": "PAPER/DOCX",
            "properties": {
                "a": "3",
                "appearanceOwner": "A_BL001",
                "c": "3",
                "documentSubType": "BLNGDSC001",
                "documentType": "BLNGDSL001",
                "i": "3",
                "ifwCategory": "Contract/Collateral letter",
                "tenant": "T_BE001"
            },
            "templateName": "A-BLNCN01",
            "type": "DOC_STREAM_DST"
        }
    },
    "identifiers": {
        "requestId": "4025a880-81ca-4a10-8814-64d7243fc528"
    },
    "metaData": {
        "costCenter": "50001256",
        "entityCode": "3180",
        "initiatingCI": "BusinesslendingGDS"
    },
    "payload": {
        "documentData": {
            "level": [
                {
                    "inspireObject": {
                        "TypeOfDocument": "ContractLetter"
                    },
                    "level": [
                        {
                            "inspireObject": {
                                "TypeOfOperation": "New"
                            },
                            "level": [
                                {
                                    "inspireObject": {
                                        "TypeOfProduct": "Cash credit in Euro"
                                    },
                                    "level": [
                                        {}
                                    ]
                                },
                                {
                                    "inspireObject": {
                                        "TypeOfProduct": "Cash credit in foreign currencies"
                                    },
                                    "level": [
                                        {}
                                    ]
                                },
                                {
                                    "level": [
                                        {
                                            "inspireObject": {
                                                "ContractSection": "Introduction"
                                            },
                                            "level": [
                                                {
                                                    "inspireObject": {
                                                        "TypeOfOperationLev5": "Increase"
                                                    },
                                                    "level": [
                                                        {}
                                                    ]
                                                }
                                            ]
                                        }
                                    ]
                                }
                            ]
                        }
                    ]
                }
            ]
        }
    },
    "recipient": {
        "partyType": "BLNGD",
        "preferredLanguage": "fr",
        "type": "custom-recipient"
    }
}
