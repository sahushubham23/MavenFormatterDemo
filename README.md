    private static JsonNode removeInspireTransformVar(JsonNode node) {
        if (node.isObject()) {
            ObjectNode objectNode = (ObjectNode) node;
            ObjectNode modifiedNode = objectNode.objectNode();
            objectNode.fields().forEachRemaining(entry -> {
                String fieldName = entry.getKey();
                JsonNode fieldValue = entry.getValue();
                if (!fieldName.equals("inspireTransformVar")) {
                    modifiedNode.set(fieldName, removeInspireTransformVar(fieldValue));
                } else {
                    fieldValue.fields().forEachRemaining(inspireEntry -> modifiedNode.set(inspireEntry.getKey(), inspireEntry.getValue()));
                }
            });
            return modifiedNode;
        } else if (node.isArray()) {
            ObjectNode modifiedArrayNode = node.arrayNode();
            node.forEach(element -> modifiedArrayNode.add(removeInspireTransformVar(element)));
            return modifiedArrayNode;
        } else {
            return node;
        }
    }
