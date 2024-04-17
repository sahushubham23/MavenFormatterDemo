private static void removeInspireTransformVar(JsonNode node) {
        if (node.isObject()) {
            Iterator<String> fieldNames = node.fieldNames();
            while (fieldNames.hasNext()) {
                String fieldName = fieldNames.next();
                JsonNode fieldValue = node.get(fieldName);
                if (fieldValue.isObject()) {
                    removeInspireTransformVar(fieldValue);
                } else if (fieldValue.isArray()) {
                    for (JsonNode element : fieldValue) {
                        removeInspireTransformVar(element);
                    }
                }
                if (fieldName.equals("inspireTransformVar")) {
                    ((ObjectNode) node).remove(fieldName);
                }
            }
        }
    }
