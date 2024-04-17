private static void removeInspireTransformVar(JsonNode node) {
        if (node.isObject()) {
            Iterator<String> fieldNames = node.fieldNames();
            while (fieldNames.hasNext()) {
                String fieldName = fieldNames.next();
                JsonNode fieldValue = node.get(fieldName);
                if (fieldValue.isObject() && fieldValue.has("inspireTransformVar")) {
                    JsonNode inspireTransformVar = fieldValue.get("inspireTransformVar");
                    Iterator<String> inspireFieldNames = inspireTransformVar.fieldNames();
                    while (inspireFieldNames.hasNext()) {
                        String inspireFieldName = inspireFieldNames.next();
                        JsonNode inspireFieldValue = inspireTransformVar.get(inspireFieldName);
                        ((ObjectNode) fieldValue).put(inspireFieldName, inspireFieldValue);
                    }
                    ((ObjectNode) fieldValue).remove("inspireTransformVar");
                }
                removeInspireTransformVar(fieldValue);
            }
        } else if (node.isArray()) {
            for (JsonNode element : node) {
                removeInspireTransformVar(element);
            }
        }
    }
