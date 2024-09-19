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
            
            while ((line = reader.readLine()) != null) {
                // Check for starting pattern with exactly 32 stars
                if (line.trim().equals("********************************")) {
                    isInsideBlock = true; // Start of a new block
                    blockData = new StringBuilder(); // Reset the block data for new block
                    blockData.append(line).append("\n"); // Add the 32-star line to the block
                    continue; // Go to the next line to get content
                }
                
                // If we're inside a block, collect the content
                if (isInsideBlock) {
                    // If we find the closing 33 stars, end the block
                    if (line.trim().equals("*********************************")) {
                        blockData.append(line).append("\n"); // Add the 33-star line to the block
                        extractedData.add(blockData.toString()); // Save the block
                        isInsideBlock = false; // End block
                    } else {
                        // Collect lines between stars
                        blockData.append(line).append("\n");
                    }
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Print all extracted blocks (or process further)
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
