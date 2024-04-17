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
                    if (node instanceof ObjectNode) {
                        JsonNode inspireTransformVar = node.get(fieldName);
                        ObjectNode objectNode = (ObjectNode) node;
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
    }
