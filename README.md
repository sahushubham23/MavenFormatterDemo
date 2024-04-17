    private static void removeInspireTransformVar(JsonNode node) {
        if (node.isObject()) {
            ObjectNode objectNode = (ObjectNode) node;
            if (objectNode.has("inspireTransformVar")) {
                JsonNode inspireTransformVar = objectNode.get("inspireTransformVar");
                objectNode.remove("inspireTransformVar");
                inspireTransformVar.fields().forEachRemaining(entry -> objectNode.set(entry.getKey(), entry.getValue()));
            }
            objectNode.fields().forEachRemaining(entry -> removeInspireTransformVar(entry.getValue()));
        } else if (node.isArray()) {
            node.forEach(Main::removeInspireTransformVar);
        }
    }
