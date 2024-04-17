private static void removeInspireTransformVar(JSONObject jsonObject) {
        for (String key : jsonObject.keySet()) {
            Object value = jsonObject.get(key);
            if (value instanceof JSONObject) {
                JSONObject innerObject = (JSONObject) value;
                if (innerObject.has("inspireTransformVar")) {
                    JSONObject inspireTransformVar = innerObject.getJSONObject("inspireTransformVar");
                    innerObject.remove("inspireTransformVar");
                    for (String innerKey : inspireTransformVar.keySet()) {
                        innerObject.put(innerKey, inspireTransformVar.get(innerKey));
                    }
                }
                removeInspireTransformVar(innerObject);
            } else if (value instanceof JSONArray) {
                JSONArray jsonArray = (JSONArray) value;
                for (int i = 0; i < jsonArray.length(); i++) {
                    removeInspireTransformVar(jsonArray.getJSONObject(i));
                }
            }
        }
    }
}
