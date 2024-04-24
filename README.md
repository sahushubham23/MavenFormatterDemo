import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

public class Main {
    public static void main(String[] args) {
        String jsonString = "{ \"level\": [ { \"level\": [ { \"level\": [ { \"level\": [ {} ], \"TypeOfProduct\": \"Cash credit in Euro\" }, { \"level\": [ {} ], \"TypeOfProduct\": \"Cash credit in foreign currencies\" }, { \"level\": [ { \"level\": [ { \"level\": [ {} ], \"TypeOfOperationLev5\": \"Increase\" } ], \"ContractSection\": \"Introduction\" } ] } ], \"TypeOfOperation\": \"New\" } ], \"TypeOfDocument\": \"ContractLetter\" } ] }";

        try {
            JSONObject jsonObject = new JSONObject(jsonString);
            JSONObject variables = new JSONObject();
            JSONArray arrays = new JSONArray();

            // Traverse JSON object to separate variables and arrays
            extractVariablesAndArrays(jsonObject, variables, arrays);

            // Reconstruct JSON with variables first and arrays second
            JSONObject newJsonObject = new JSONObject();
            newJsonObject.put("variables", variables);
            newJsonObject.put("arrays", arrays);

            System.out.println(newJsonObject.toString());
        } catch (JSONException e) {
            e.printStackTrace();
        }
    }

    private static void extractVariablesAndArrays(JSONObject jsonObject, JSONObject variables, JSONArray arrays) {
        jsonObject.keySet().forEach(key -> {
            Object value = jsonObject.opt(key);
            if (value instanceof JSONArray) {
                JSONArray jsonArray = new JSONArray();
                ((JSONArray) value).forEach(innerObject -> {
                    JSONObject innerVariables = new JSONObject();
                    JSONArray innerArrays = new JSONArray();
                    extractVariablesAndArrays((JSONObject) innerObject, innerVariables, innerArrays);
                    JSONObject newObject = new JSONObject();
                    mergeJSONObject(newObject, innerVariables);
                    mergeJSONObject(newObject, innerArrays);
                    String dynamicVariableName = getDynamicVariableName(newObject);
                    if (dynamicVariableName != null) {
                        Object level = newObject.remove("level");
                        newObject.put(dynamicVariableName, level);
                    }
                    jsonArray.put(newObject);
                });
                variables.put(key, jsonArray);
            } else if (value instanceof JSONObject) {
                extractVariablesAndArrays((JSONObject) value, variables, arrays);
            } else {
                variables.put(key, value);
            }
        });
    }

    private static void mergeJSONObject(JSONObject target, JSONObject source) {
        source.keySet().forEach(key -> {
            Object value = source.opt(key);
            try {
                target.put(key, value);
            } catch (JSONException e) {
                e.printStackTrace();
            }
        });
    }

    private static void mergeJSONObject(JSONObject target, JSONArray source) {
        for (int i = 0; i < source.length(); i++) {
            try {
                Object obj = source.get(i);
                if (obj instanceof JSONObject) {
                    JSONObject subObject = (JSONObject) obj;
                    subObject.keySet().forEach(subKey -> {
                        Object subValue = subObject.opt(subKey);
                        try {
                            target.put(subKey, subValue);
                        } catch (JSONException e) {
                            e.printStackTrace();
                        }
                    });
                } else {
                    target.put(Integer.toString(i), obj);
                }
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
    }

    private static String getDynamicVariableName(JSONObject object) {
        String dynamicVariableName = null;
        for (String key : object.keySet()) {
            if (!key.equals("level")) {
                dynamicVariableName = key;
                break;
            }
        }
        return dynamicVariableName;
    }
}
