<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Base64 to DOCX</title>
</head>
<body>
    <button id="downloadButton">Download DOCX</button>

    <script>
        document.getElementById('downloadButton').addEventListener('click', function() {
            // Dummy Base64 string
            const base64String = "UEsFBgAAAAAAAAAAAAAAAAAAAAAAAA==";

            // Decode Base64 string
            const binaryString = window.atob(base64String);
            const len = binaryString.length;
            const bytes = new Uint8Array(len);
            for (let i = 0; i < len; i++) {
                bytes[i] = binaryString.charCodeAt(i);
            }

            // Create a Blob object
            const blob = new Blob([bytes], { type: 'application/vnd.openxmlformats-officedocument.wordprocessingml.document' });

            // Create a link element
            const link = document.createElement('a');
            link.href = window.URL.createObjectURL(blob);
            link.download = 'dummy.docx';

            // Append the link to the body
            document.body.appendChild(link);

            // Trigger the download
            link.click();

            // Remove the link from the document
            document.body.removeChild(link);
        });
    </script>
</body>
</html>
