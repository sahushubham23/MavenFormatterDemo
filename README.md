import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.node.ArrayNode;
import com.fasterxml.jackson.databind.node.ObjectNode;

public class JsonTransformer {

    public static void main(String[] args) throws Exception {
        String json = "{\"level\":[{\"TypeOfDocument\":\"ContractLetter\",\"level\":[{\"TypeOfOperation\":\"New\",\"level\":[{\"TypeOfProduct\":\"Cash credit in Euro\",\"level\":[{}]},{\"TypeOfProduct\":\"Cash credit in foreign currencies\",\"level\":[{}]},{\"level\":[{\"ContractSection\":\"Introduction\",\"level\":[{\"TypeOfOperationLev5\":\"Increase\",\"level\":[{}]}]}]}]}]}]}";
        String transformedJson = transformJson(json);
        System.out.println(transformedJson);
    }

    public static String transformJson(String json) throws Exception {
        ObjectMapper mapper = new ObjectMapper();
        JsonNode root = mapper.readTree(json);
        transformNode(root, 1);
        return mapper.writeValueAsString(root);
    }

    private static void transformNode(JsonNode node, int level) {
        if (node.isObject()) {
            ObjectNode objectNode = (ObjectNode) node;
            JsonNode levelNode = node.get("level");
            if (levelNode != null) {
                objectNode.remove("level");
                objectNode.put("level" + level, transformObject(levelNode, level + 1));
            }
        }
    }

    private static JsonNode transformObject(JsonNode node, int level) {
        if (node.isArray()) {
            ArrayNode arrayNode = (ArrayNode) node;
            for (JsonNode child : arrayNode) {
                transformNode(child, level);
            }
        }
        return node;
    }
}
