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
        "requestId": "0ca2aa83-a39e-4066-b618-98c51995dd3d"
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
                    "inspireTransformVar": {
                        "TypeOfOperation": "New"
                    },
                    "level": [
                        {
                            "inspireTransformVar": {
                                "TypeOfProduct": "Advances in Euro",
                                "TypeOfOperation": "Expenses in Euro"
                            },
                            "level": [
                                {
                                    "inspireTransformVar": {
                                        "TypeOfOperation": "New"
                                    },
                                    "level": [
                                        {}
                                    ]
                                },
                                {
                                    "inspireTransformVar": {
                                        "TypeOfOperation": "New"
                                    },
                                    "level": [
                                        {
                                            "inspireTransformVar": {
                                                "TypeOfOperation": "New"
                                            }
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
