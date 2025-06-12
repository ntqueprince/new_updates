
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Update JSON Generator</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts - Inter for modern look -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #e0f7fa 0%, #c4e4e9 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .container {
            background-color: #ffffff;
            padding: 32px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            max-width: 600px;
            width: 100%;
            text-align: center;
            border: 2px solid #a78bfa; /* Light purple border */
        }
        .input-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #4b5563; /* Dark gray */
            text-align: left;
        }
        .input-group input[type="date"],
        .input-group textarea {
            width: 100%;
            padding: 12px 16px;
            border: 2px solid #d1d5db; /* Light gray border */
            border-radius: 10px; /* More rounded corners */
            font-size: 1rem;
            color: #374151; /* Darker gray text */
            transition: all 0.3s ease;
            box-sizing: border-box; /* Include padding in width calculation */
        }
        .input-group input[type="date"]:focus,
        .input-group textarea:focus {
            outline: none;
            border-color: #8b5cf6; /* Purple on focus */
            box-shadow: 0 0 0 3px rgba(139, 92, 246, 0.2); /* Soft purple shadow */
        }
        .input-group textarea {
            min-height: 120px; /* Increased height for better text input */
            resize: vertical; /* Allow vertical resizing */
        }
        button {
            background-color: #8b5cf6; /* Purple button */
            color: white;
            padding: 12px 24px;
            border: none;
            border-radius: 10px; /* More rounded corners */
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 12px rgba(139, 92, 246, 0.3);
            display: inline-flex; /* For icon alignment */
            align-items: center;
            justify-content: center;
        }
        button:hover {
            background-color: #7c3aed; /* Darker purple on hover */
            transform: translateY(-2px);
            box-shadow: 0 6px 16px rgba(139, 92, 246, 0.4);
        }
        #outputCode {
            background-color: #f3f4f6; /* Lighter gray background */
            border: 2px solid #e5e7eb; /* Light gray border */
            border-radius: 10px; /* More rounded corners */
            padding: 16px;
            font-family: 'SFMono-Regular', Consolas, 'Liberation Mono', Menlo, monospace; /* Monospace font */
            font-size: 0.95rem;
            color: #1f2937; /* Dark text */
            min-height: 100px;
            resize: vertical;
            overflow-x: auto; /* Allow horizontal scrolling for long lines */
            white-space: pre-wrap; /* Preserve whitespace and wrap text */
            text-align: left;
            word-break: break-all; /* Break long words */
        }
        /* Custom message box for copy confirmation */
        .message-box {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #10b981; /* Green */
            color: white;
            padding: 12px 20px;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
            font-size: 0.9rem;
            z-index: 1000;
            opacity: 0;
            transition: opacity 0.3s ease-in-out;
        }
        .message-box.show {
            opacity: 1;
        }

        /* Responsive adjustments */
        @media (max-width: 480px) {
            .container {
                padding: 20px;
            }
            .input-group input[type="date"],
            .input-group textarea,
            button {
                font-size: 0.9rem;
                padding: 10px 15px;
            }
            #outputCode {
                font-size: 0.85rem;
            }
        }
    </style>
</head>
<body class="selection:bg-purple-200 selection:text-purple-800">
    <div class="container">
        <h1 class="text-3xl font-bold text-gray-800 mb-8">Update JSON Generator</h1>

        <div class="input-group mb-6">
            <label for="updateDate" class="mb-2">Select Date:</label>
            <input type="date" id="updateDate" class="mb-4">
        </div>

        <div class="input-group mb-6">
            <label for="updateText" class="mb-2">Enter Update Text:</label>
            <textarea id="updateText" class="mb-4" placeholder="e.g., Unmasked KYC documents (Aadhar and PAN card) are needed for KYC in Reliance..."></textarea>
        </div>

        <button id="generateBtn" class="mb-8">
            Generate JSON 
            <svg class="w-5 h-5 ml-2 -mr-1" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M7 2a2 2 0 00-2 2v12a2 2 0 002 2h6a2 2 0 002-2V4a2 2 0 00-2-2H7zm3 14a1 1 0 100-2 1 1 0 000 2z" clip-rule="evenodd"></path></svg>
        </button>

        <div class="input-group">
            <label for="outputCode" class="mb-2">Generated JSON Output:</label>
            <textarea id="outputCode" readonly class="w-full text-sm font-mono p-4 bg-gray-100 rounded-lg border border-gray-300"></textarea>
        </div>
        
        <button id="copyBtn" class="mt-4 px-6 py-2 bg-blue-500 hover:bg-blue-600 focus:outline-none focus:ring-4 focus:ring-blue-300 active:bg-blue-700 shadow-md transform transition-all duration-150 ease-in-out">
            Copy to Clipboard 
            <svg class="w-5 h-5 ml-2 -mr-1" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M8 3a1 1 0 011-1h2a1 1 0 110 2H9a1 1 0 01-1-1z"></path><path d="M6 3a2 2 0 00-2 2v11a2 2 0 002 2h8a2 2 0 002-2V5a2 2 0 00-2-2 3 3 0 01-3 3H9a3 3 0 01-3-3z"></path></svg>
        </button>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const updateDateInput = document.getElementById('updateDate');
            const updateTextInput = document.getElementById('updateText');
            const generateBtn = document.getElementById('generateBtn');
            const outputCodeArea = document.getElementById('outputCode');
            const copyBtn = document.getElementById('copyBtn');

            // Set today's date as default
            const today = new Date();
            const year = today.getFullYear();
            const month = String(today.getMonth() + 1).padStart(2, '0'); // Month is 0-indexed
            const day = String(today.getDate()).padStart(2, '0');
            updateDateInput.value = `${year}-${month}-${day}`;

            generateBtn.addEventListener('click', function() {
                const selectedDate = updateDateInput.value;
                const updateText = updateTextInput.value.trim();

                if (!selectedDate) {
                    showMessage('Please select a date!', 'error');
                    return;
                }
                if (!updateText) {
                    showMessage('Please enter the update text!', 'error');
                    return;
                }

                // Escape double quotes in the update text to ensure valid JSON
                const escapedUpdateText = updateText.replace(/"/g, '\\"');

                // Generate the JSON string
                const jsonOutput = `{ date: "${selectedDate}", update: "${escapedUpdateText}" }`;
                outputCodeArea.value = jsonOutput;
                showMessage('JSON generated successfully!', 'info');
            });

            copyBtn.addEventListener('click', function() {
                outputCodeArea.select();
                try {
                    // Use document.execCommand('copy') as navigator.clipboard.writeText() might not work in some iframe environments
                    const successful = document.execCommand('copy');
                    if (successful) {
                        showMessage('Copied to clipboard!', 'success');
                    } else {
                        showMessage('Failed to copy. Please copy manually.', 'error');
                    }
                } catch (err) {
                    console.error('Failed to copy text: ', err);
                    showMessage('Failed to copy. Please copy manually.', 'error');
                }
            });

            // Custom message box function
            function showMessage(message, type = "info") {
                const messageBox = document.createElement("div");
                messageBox.className = "message-box";
                if (type === "error") {
                    messageBox.style.backgroundColor = "#dc2626"; /* Red */
                } else if (type === "success") {
                    messageBox.style.backgroundColor = "#10b981"; /* Green */
                } else {
                    messageBox.style.backgroundColor = "#2563eb"; /* Blue */
                }
                messageBox.textContent = message;
                document.body.appendChild(messageBox);

                // Show with transition
                setTimeout(() => {
                    messageBox.classList.add('show');
                }, 10); // Small delay for CSS transition to work

                // Hide after 3 seconds
                setTimeout(() => {
                    messageBox.classList.remove('show');
                    messageBox.addEventListener("transitionend", () => messageBox.remove());
                }, 3000);
            }
        });
    </script>
</body>
</html>
