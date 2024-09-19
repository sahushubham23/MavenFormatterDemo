import java.io.*;
import java.util.*;
import java.util.regex.*;

public class ExtractData {

    public static void main(String[] args) {
        String filePath = "/mnt/data/file-xXTbWZv4LmwOMyWFRqlphBsZ";  // Path to your file
        List<String> extractedData = new ArrayList<>();

        // Regular expression to detect lines with 32 stars, variable name, and 33 stars
        String patternString = "(\\*{32})(.*?)\\*{33}";
        Pattern pattern = Pattern.compile(patternString);

        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;
            StringBuilder blockData = new StringBuilder();  // To store the content of each block
            boolean isInsideBlock = false;  // To track if we are currently inside a block
            String variableName = "";  // To store the current variable name
            
            while ((line = reader.readLine()) != null) {
                Matcher matcher = pattern.matcher(line);
                
                // If the pattern is found, we are at the start of a new block
                if (matcher.find()) {
                    // If we were already inside a block, save the previous block
                    if (isInsideBlock) {
                        extractedData.add("Variable Name: " + variableName + "\n" + "Data:\n" + blockData.toString());
                    }

                    // Start a new block: reset blockData and capture the new variable name
                    blockData = new StringBuilder();
                    variableName = matcher.group(2).trim();  // Extract variable name

                    // Append any data that might exist on the same line after the 33 stars
                    String remainingData = line.substring(matcher.end()).trim();
                    if (!remainingData.isEmpty()) {
                        blockData.append(remainingData).append("\n");
                    }

                    // Mark that we are inside a block now
                    isInsideBlock = true;
                } 
                // If we are inside a block and there's no new pattern, append the line to the block data
                else if (isInsideBlock) {
                    blockData.append(line).append("\n");
                }
            }

            // After the last block, we need to add its data since there is no further 32-star delimiter
            if (isInsideBlock) {
                extractedData.add("Variable Name: " + variableName + "\n" + "Data:\n" + blockData.toString());
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
