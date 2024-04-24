    public static void transformJson(JSONObject jsonObject) {
        if (jsonObject.has("level")) {
            JSONArray levelArray = jsonObject.getJSONArray("level");
            for (int i = 0; i < levelArray.length(); i++) {
                JSONObject levelObject = levelArray.getJSONObject(i);
                if (levelObject.has("inspireTransformObject")) {
                    JSONObject inspireTransformObject = levelObject.getJSONObject("inspireTransformObject");
                    levelObject.remove("inspireTransformObject");
                    levelObject.putAll(inspireTransformObject);
                }
                if (levelObject.has("level")) {
                    transformJson(levelObject);
                }
            }
        }
    }

JSONObject json = new JSONObject(yourJsonString);
JsonTransformer.transform(json);
System.out.println(json.toString());
