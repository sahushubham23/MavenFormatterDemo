public static void transformJson(JSONObject jsonObject) {
    if (jsonObject.has("level")) {
        JSONArray levelArray = jsonObject.getJSONArray("level");
        for (int i = 0; i < levelArray.length(); i++) {
            JSONObject levelObject = levelArray.getJSONObject(i);
            if (levelObject.has("inspireTransformObject")) {
                JSONObject inspireTransformObject = levelObject.getJSONObject("inspireTransformObject");
                levelObject.remove("inspireTransformObject");
                Iterator<String> keys = inspireTransformObject.keys();
                while (keys.hasNext()) {
                    String key = keys.next();
                    levelObject.put(key, inspireTransformObject.get(key));
                }
            }
            if (levelObject.has("level")) {
                JSONArray nestedLevelArray = levelObject.getJSONArray("level");
                for (int j = 0; j < nestedLevelArray.length(); j++) {
                    JSONObject nestedLevelObject = nestedLevelArray.getJSONObject(j);
                    transformJson(nestedLevelObject);
                }
            }
        }
    }
}
