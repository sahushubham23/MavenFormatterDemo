{
	"level": [
		{
			"inspireTransformObject": {
				"TypeOfDocument": "ContractLetter"
			},
			"level": [
				{
					"inspireTransformObject": {
						"TypeOfOperation": "New"
					},
					"level": [
						{
							"inspireTransformObject": {
								"TypeOfProduct": "Cash credit in Euro"
							},
							"level": [
								{}
							],
							"TypeOfProduct": "Cash credit in Euro"
						},
						{
							"inspireTransformObject": {
								"TypeOfProduct": "Cash credit in foreign currencies"
							},
							"level": [
								{}
							],
							"TypeOfProduct": "Cash credit in foreign currencies"
						},
						{
							"level": [
								{
									"inspireTransformObject": {
										"ContractSection": "Introduction"
									},
									"level": [
										{
											"inspireTransformObject": {
												"TypeOfOperationLev5": "Increase"
											},
											"level": [
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





---------------------------------------------------

{
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
							"level4": [
								{}
							],
							"TypeOfProduct": "Cash credit in Euro"
						},
						{
							"inspireTransformObject": {
								"TypeOfProduct": "Cash credit in foreign currencies"
							},
							"level5": [
								{}
							],
							"TypeOfProduct": "Cash credit in foreign currencies"
						},
						{
							"level6": [
								{
									"inspireTransformObject": {
										"ContractSection": "Introduction"
									},
									"level7": [
										{
											"inspireTransformObject": {
												"TypeOfOperationLev5": "Increase"
											},
											"level8": [
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


----------------------

import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.node.ObjectNode;

import java.io.IOException;
import java.util.Iterator;

public class JsonTransformer {
    public static void main(String[] args) throws IOException {
        ObjectMapper mapper = new ObjectMapper();
        String json = "{\n" +
                "  \"level\": [\n" +
                "    {\n" +
                "      \"inspireTransformObject\": {\n" +
                "        \"TypeOfDocument\": \"ContractLetter\"\n" +
                "      },\n" +
                "      \"level\": [\n" +
                "        {\n" +
                "          \"inspireTransformObject\": {\n" +
                "            \"TypeOfOperation\": \"New\"\n" +
                "          },\n" +
                "          \"level\": [\n" +
                "            {\n" +
                "              \"inspireTransformObject\": {\n" +
                "                \"TypeOfProduct\": \"Cash credit in Euro\"\n" +
                "              },\n" +
                "              \"level\": [\n" +
                "                {\n" +
                "                  \"inspireTransformObject\": {\n" +
                "                    \"ContractSection\": \"Introduction\"\n" +
                "                  },\n" +
                "                  \"level\": [\n" +
                "                    {\n" +
                "                      \"inspireTransformObject\": {\n" +
                "                        \"TypeOfOperationLev5\": \"Increase\"\n" +
                "                      },\n" +
                "                      \"level\": [\n" +
                "                        {}\n" +
                "                      ]\n" +
                "                    }\n" +
                "                  ]\n" +
                "                }\n" +
                "              ]\n" +
                "            },\n" +
                "            {\n" +
                "              \"inspireTransformObject\": {\n" +
                "                \"TypeOfProduct\": \"Cash credit in foreign currencies\"\n" +
                "              },\n" +
                "              \"level\": [\n" +
                "                {}\n" +
                "              ]\n" +
                "            },\n" +
                "            {\n" +
                "              \"level\": [\n" +
                "                {\n" +
                "                  \"inspireTransformObject\": {\n" +
                "                    \"ContractSection\": \"Introduction\"\n" +
                "                  },\n" +
                "                  \"level\": [\n" +
                "                    {\n" +
                "                      \"inspireTransformObject\": {\n" +
                "                        \"TypeOfOperationLev5\": \"Increase\"\n" +
                "                      },\n" +
                "                      \"level\": [\n" +
                "                        {}\n" +
                "                      ]\n" +
                "                    }\n" +
                "                  ]\n" +
                "                }\n" +
                "              ]\n" +
                "            }\n" +
                "          ]\n" +
                "        }\n" +
                "      ]\n" +
                "    }\n" +
                "  ]\n" +
                "}";
        JsonNode root = mapper.readTree(json);
        transformJson(root, 1);
        System.out.println(mapper.writerWithDefaultPrettyPrinter().writeValueAsString(root));
    }

    private static void transformJson(JsonNode node, int level) {
        if (node.isArray()) {
            for (JsonNode child : node) {
                transformJson(child, level);
            }

        } else if (node.isObject()) {
            ObjectNode objectNode = (ObjectNode) node;
            if (objectNode.has("inspireTransformObject")) {
                ObjectNode inspireTransformObject = (ObjectNode) objectNode.get("inspireTransformObject");
                Iterator<String> fieldNames = inspireTransformObject.fieldNames();
                while (fieldNames.hasNext()) {
                    String fieldName = fieldNames.next();
                    objectNode.put(fieldName, inspireTransformObject.get(fieldName).asText());
                }
                objectNode.remove("inspireTransformObject");
            }
            if (objectNode.has("level")) {
                JsonNode levelNode = objectNode.get("level");
                if (levelNode.size() > 0) {
                    objectNode.set("level" + level, levelNode);
                    objectNode.remove("level");
                    transformJson(objectNode.get("level" + level), level + 1);
                }
            }
        }
    }
    
}


