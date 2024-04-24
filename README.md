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
        for (String key : jsonObject.keySet()) {
            Object value = jsonObject.get(key);
            if (value instanceof JSONArray) {
                for (int i = 0; i < ((JSONArray) value).length(); i++) {
                    JSONObject innerObject = ((JSONArray) value).getJSONObject(i);
                    JSONObject innerVariables = new JSONObject();
                    JSONArray innerArrays = new JSONArray();
                    extractVariablesAndArrays(innerObject, innerVariables, innerArrays);
                    arrays.put(new JSONObject().put(key, innerArrays));
                    mergeJSONObject(variables, innerVariables);
                }
            } else if (value instanceof JSONObject) {
                extractVariablesAndArrays((JSONObject) value, variables, arrays);
            } else {
                variables.put(key, value);
            }
        }
    }

    private static void mergeJSONObject(JSONObject target, JSONObject source) {
        for (String key : source.keySet()) {
            target.put(key, source.get(key));
        }
    }
}
