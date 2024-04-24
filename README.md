import org.json.JSONArray;
import org.json.JSONObject;

public class JsonTransformer {
    public static void transform(JSONObject json) {
        if (json.has("level")) {
            JSONArray levelArray = json.getJSONArray("level");
            for (int i = 0; i < levelArray.length(); i++) {
                JSONObject levelObject = levelArray.getJSONObject(i);
                if (levelObject.has("inspireTransformObject")) {
                    JSONObject inspireObject = levelObject.getJSONObject("inspireTransformObject");
                    for (String key : inspireObject.keySet()) {
                        levelObject.put(key, inspireObject.get(key));
                    }
                    levelObject.remove("inspireTransformObject");
                }
                if (levelObject.has("level")) {
                    transform(levelObject);
                }
            }
        }
    }
}

JSONObject json = new JSONObject(yourJsonString);
JsonTransformer.transform(json);
System.out.println(json.toString());
