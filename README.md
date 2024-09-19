import java.io.*;
import java.util.regex.*;
import java.util.*;

public class ExtractData {

    public static void main(String[] args) {
        String filePath = "/mnt/data/file-xXTbWZv4LmwOMyWFRqlphBsZ";  // Path to the file
        List<String> extractedData = new ArrayList<>();
        
        // Regular expression to detect lines with 32 stars, variable name, and 33 stars
        String patternString = "(\\*{32})(.*?)\\*{33}";
        Pattern pattern = Pattern.compile(patternString);

        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = reader.readLine()) != null) {
                Matcher matcher = pattern.matcher(line);
                
                // Check if the line matches the pattern
                if (matcher.find()) {
                    // Extract the variable name from between the stars
                    String variableName = matcher.group(2).trim();
                    
                    // Extract the content/data after the 33 stars
                    String remainingData = line.substring(matcher.end()).trim();
                    
                    // Build and store the extracted data
                    StringBuilder blockData = new StringBuilder();
                    blockData.append("Variable Name: ").append(variableName).append("\n");
                    blockData.append("Data: ").append(remainingData).append("\n");

                    // Store the block
                    extractedData.add(blockData.toString());
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Output all extracted blocks
        if (extractedData.isEmpty()) {
            System.out.println("No blocks were found.");
        } else {
            for (String data : extractedData) {
                System.out.println("Extracted Block:");
                System.out.println(data);
            }
        }
    }
}
