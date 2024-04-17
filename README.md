private static JsonNode removeInspireTransformVar(JsonNode node) {
        if (node.isObject()) {
            ObjectNode objectNode = (ObjectNode) node;
            ObjectNode modifiedNode = objectNode.objectNode();
            Iterator<String> fieldNames = objectNode.fieldNames();
            while (fieldNames.hasNext()) {
                String fieldName = fieldNames.next();
                JsonNode fieldValue = objectNode.get(fieldName);
                if (fieldValue.isArray()) {
                    modifiedNode.set(fieldName, removeInspireTransformVar(fieldValue));
                } else if (fieldValue.isObject()) {
                    modifiedNode.set(fieldName, removeInspireTransformVar(fieldValue));
                } else if (!fieldName.equals("inspireTransformVar")) {
                    modifiedNode.set(fieldName, fieldValue);
                } else {
                    fieldValue.fields().forEachRemaining(entry -> modifiedNode.set(entry.getKey(), entry.getValue()));
                }
            }
            return modifiedNode;
        } else if (node.isArray()) {
            ArrayNode arrayNode = (ArrayNode) node;
            ArrayNode modifiedArrayNode = arrayNode.arrayNode();
            arrayNode.elements().forEachRemaining(element -> modifiedArrayNode.add(removeInspireTransformVar(element)));
            return modifiedArrayNode;
        } else {
            return node;
        }
    }
