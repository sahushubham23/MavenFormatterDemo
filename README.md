private static void removeInspireObject(JsonNode node) {
        if (node.isObject()) {
            ObjectNode objectNode = (ObjectNode) node;
            Iterator<String> fieldNames = objectNode.fieldNames();
            while (fieldNames.hasNext()) {
                String fieldName = fieldNames.next();
                JsonNode fieldValue = objectNode.get(fieldName);
                if (fieldValue.isObject() && fieldValue.has("inspireObject")) {
                    // Extract the inspireObject contents and add as individual fields
                    JsonNode inspireObject = fieldValue.get("inspireObject");
                    objectNode.remove(fieldName);
                    Iterator<String> inspireFieldNames = inspireObject.fieldNames();
                    while (inspireFieldNames.hasNext()) {
                        String inspireFieldName = inspireFieldNames.next();
                        JsonNode inspireFieldValue = inspireObject.get(inspireFieldName);
                        objectNode.set(inspireFieldName, inspireFieldValue);
                    }
                } else if (fieldValue.isObject() || fieldValue.isArray()) {
                    removeInspireObject(fieldValue);
                }
            }
        } else if (node.isArray()) {
            for (JsonNode childNode : node) {
                removeInspireObject(childNode);
            }
        }
    }
