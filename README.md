private static void removeInspireTransformVar(JsonNode node) {
        if (node.isObject()) {
            ObjectNode objectNode = (ObjectNode) node;
            Iterator<String> fieldNames = objectNode.fieldNames();
            while (fieldNames.hasNext()) {
                String fieldName = fieldNames.next();
                JsonNode fieldValue = objectNode.get(fieldName);
                if (fieldValue.isObject()) {
                    removeInspireTransformVar(fieldValue);
                } else if (fieldValue.isArray()) {
                    for (JsonNode element : fieldValue) {
                        removeInspireTransformVar(element);
                    }
                }
                if (fieldName.equals("inspireTransformVar")) {
                    JsonNode inspireTransformVar = objectNode.get(fieldName);
                    Iterator<String> inspireFieldNames = inspireTransformVar.fieldNames();
                    while (inspireFieldNames.hasNext()) {
                        String inspireFieldName = inspireFieldNames.next();
                        JsonNode inspireFieldValue = inspireTransformVar.get(inspireFieldName);
                        objectNode.set(inspireFieldName, inspireFieldValue);
                    }
                    objectNode.remove(fieldName);
                }
            }
        }
    }
