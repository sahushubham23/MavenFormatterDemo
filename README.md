import java.io.*;
import java.util.*;

public class ExtractData {

    public static void main(String[] args) {
        String filePath = "/mnt/data/file-xXTbWZv4LmwOMyWFRqlphBsZ";  // Adjust to your file path
        List<String> extractedData = new ArrayList<>();

        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;
            boolean isInsideBlock = false;
            StringBuilder blockData = new StringBuilder();
            int starCount = 0;
            String variableName = "";

            while ((line = reader.readLine()) != null) {
                // Check for starting pattern with 32 stars
                if (line.startsWith("********************************") && line.length() == 32) {
                    // Starting a new block
                    isInsideBlock = true;
                    blockData = new StringBuilder(); // Reset block data
                    starCount = 32;
                    continue; // Move to the next line to get the variable name
                }

                // If we are inside a block, collect data
                if (isInsideBlock) {
                    // Check if this is the variable line
                    if (line.matches(".*[a-zA-Z]\\d+[a-zA-Z]\\d+.*")) {
                        variableName = line.trim();
                        blockData.append(variableName).append("\n");
                    } else if (line.startsWith("*********************************") && line.length() == 33) {
                        // Closing 33 stars block
                        starCount = 33;
                        isInsideBlock = false;
                        blockData.append(line).append("\n");
                        extractedData.add(blockData.toString()); // Store the block
                    } else {
                        // Append the content between stars
                        blockData.append(line).append("\n");
                    }
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Print all extracted blocks (or process further)
        for (String data : extractedData) {
            System.out.println(data);
        }
    }
}
