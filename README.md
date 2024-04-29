{"channels":{"destinations":{"documentId":"sanjkauu1d","output":"PAPER/DOCX","properties":{"a":"3","appearanceOwner":"A_BL001","c":"3","documentSubType":"BLNGDSC001","documentType":"BLNGDSL001","i":"3","ifwCategory":"Contract/Collateral letter","tenant":"T_BE001"},"templateName":"A-BLNCN01","type":"DOC_STREAM_DST"}},"identifiers":{"requestId":"acbbff8e-c74e-4ab5-aaa5-18de36d96358"},"metaData":{"costCenter":"50001256","entityCode":"3180","initiatingCI":"BusinesslendingGDS"},"payload":{"DocumentData":{"level":[{"inspireTransformObject":{"TypeOfDocument":"ContractLetter"},"level":[{"inspireTransformObject":{"TypeOfOperation":"New"},"level":[{"inspireTransformObject":{"TypeOfProduct":"Cash credit in Euro"},"level":[{}],"TypeOfProduct":"Cash credit in Euro"},{"inspireTransformObject":{"TypeOfProduct":"Cash credit in foreign currencies"},"level":[{}],"TypeOfProduct":"Cash credit in foreign currencies"},{"level":[{"inspireTransformObject":{"ContractSection":"Introduction"},"level":[{"inspireTransformObject":{"TypeOfOperationLev5":"Increase"},"level":[{}],"TypeOfOperationLev5":"Increase"}],"ContractSection":"Introduction"}]}],"TypeOfOperation":"New"}],"TypeOfDocument":"ContractLetter"}]}},"recipient":{"partyType":"BLNGD","preferredLanguage":"fr","type":"custom-recipient"}}





---------------------------------------------------

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
		"requestId": "acbbff8e-c74e-4ab5-aaa5-18de36d96358"
	},
	"metaData": {
		"costCenter": "50001256",
		"entityCode": "3180",
		"initiatingCI": "BusinesslendingGDS"
	},
	"payload": {
		"DocumentData": {
			"level1": [
				{
					"inspireTransformObject": {
						"TypeOfDocument": "ContractLetter"
					},
					"level2": [
						{
							"inspireTransformObject": {
								"TypeOfOperation": "New"
							},
							"level3": [
								{
									"inspireTransformObject": {
										"TypeOfProduct": "Cash credit in Euro"
									},
									"level3.1": [
										{}
									],
									"TypeOfProduct": "Cash credit in Euro"
								},
								{
									"inspireTransformObject": {
										"TypeOfProduct": "Cash credit in foreign currencies"
									},
									"level3.2": [
										{}
									],
									"TypeOfProduct": "Cash credit in foreign currencies"
								},
								{
									"level3.3": [
										{
											"inspireTransformObject": {
												"ContractSection": "Introduction"
											},
											"level3.3.1": [
												{
													"inspireTransformObject": {
														"TypeOfOperationLev5": "Increase"
													},
													"level3.3.1.1": [
														{}
													],
													"TypeOfOperationLev5": "Increase"
												}
											],
											"ContractSection": "Introduction"
										}
									]
								}
							],
							"TypeOfOperation": "New"
						}
					],
					"TypeOfDocument": "ContractLetter"
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
