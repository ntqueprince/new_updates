
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>✨ Update JSON Generator ✨</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Google Font -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Poppins', sans-serif; }
    .fade-in { animation: fadeIn 0.8s ease forwards; }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(30px); }
      to   { opacity: 1; transform: translateY(0); }
    }
    .ripple {
      position: relative;
      overflow: hidden;
    }
    .ripple::after {
      content: "";
      position: absolute;
      border-radius: 50%;
      width: 100px;
      height: 100px;
      background: rgba(255, 255, 255, 0.4);
      top: 50%;
      left: 50%;
      pointer-events: none;
      transform: translate(-50%, -50%) scale(0);
      opacity: 0;
      transition: transform 0.6s, opacity 1s;
    }
    .ripple:active::after {
      transform: translate(-50%, -50%) scale(2);
      opacity: 1;
      transition: 0s;
    }
  </style>
</head>
<body class="min-h-screen flex items-center justify-center bg-gradient-to-br from-green-100 to-blue-200 p-4">

  <div class="bg-white rounded-3xl shadow-2xl p-8 max-w-xl w-full fade-in relative overflow-hidden">

    <!-- Decorative blurred circles -->
    <div class="absolute w-48 h-48 bg-purple-300 opacity-20 rounded-full -top-20 -left-20 blur-3xl"></div>
    <div class="absolute w-56 h-56 bg-blue-400 opacity-20 rounded-full -bottom-24 -right-24 blur-3xl"></div>

    <h1 class="text-3xl font-bold text-purple-600 text-center mb-6">✨ Update JSON Generator ✨</h1>

    <label class="block text-gray-700 mb-2 font-semibold">Select Date:</label>
    <input id="dateInput" type="date" class="w-full p-3 border border-purple-300 rounded-xl focus:ring-2 focus:ring-purple-400 mb-5">

    <label class="block text-gray-700 mb-2 font-semibold">Enter Update Text:</label>
    <textarea id="updateInput" rows="6" placeholder="Example: If customer comes for odometer update in DIGIT, these 4 things need to be captured: odometer reading, engraved chassis number, 360-degree view, engine compartment." class="w-full p-3 border border-purple-300 rounded-xl focus:ring-2 focus:ring-purple-400 mb-5 resize-y"></textarea>

    <button onclick="generateJSON()" class="w-full bg-gradient-to-r from-purple-500 to-indigo-600 text-white font-bold py-3 rounded-full shadow-lg ripple hover:shadow-xl mb-5 flex items-center justify-center gap-2">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/>
      </svg>
      Generate JSON
    </button>

    <label class="block text-gray-700 mb-2 font-semibold">Generated JSON Output:</label>
    <textarea id="jsonOutput" readonly rows="4" class="w-full p-3 bg-gray-100 border border-purple-200 rounded-xl font-mono whitespace-pre-wrap break-words mb-4"></textarea>

    <button onclick="copyToClipboard()" class="w-full bg-gradient-to-r from-blue-500 to-teal-500 text-white font-bold py-3 rounded-full shadow-lg ripple hover:shadow-xl flex items-center justify-center gap-2">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8l6 6v4"/>
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 16h2a2 2 0 002-2V10m-4 6v6M8 20h8"/>
      </svg>
      Copy to Clipboard
    </button>

  </div>

  <!-- Toast Message -->
  <div id="toast" class="fixed bottom-6 left-1/2 transform -translate-x-1/2 bg-green-500 text-white px-4 py-2 rounded-full shadow-lg hidden"></div>

  <script>
    // Set today's date as default
    document.getElementById("dateInput").value = new Date().toISOString().split('T')[0];

    // Toast notification
    function showToast(message, type = 'success') {
      const toast = document.getElementById("toast");
      toast.textContent = message;
      toast.className = `fixed bottom-6 left-1/2 transform -translate-x-1/2 text-white px-4 py-2 rounded-full shadow-lg bg-${type === 'success' ? 'green' : type === 'error' ? 'red' : 'blue'}-500`;
      toast.style.display = 'block';
      setTimeout(() => { toast.style.display = 'none'; }, 3000);
    }

    // Generate JSON function
    function generateJSON() {
      const date = document.getElementById("dateInput").value;
      const updateText = document.getElementById("updateInput").value;

      if (!date) {
        showToast("Please select a date!", 'error');
        return;
      }

      if (!updateText.trim()) {
        showToast("Please enter update text!", 'error');
        return;
      }

      // Clean and normalize text
      let cleanedUpdate = updateText.replace(/(\r\n|\n|\r)/g, ' ');
      cleanedUpdate = cleanedUpdate.replace(/\s\s+/g, ' ').trim();
      cleanedUpdate = cleanedUpdate.replace(/"/g, '\\"');

      const jsonOutput = `{ date: "${date}", update: "${cleanedUpdate}" }`;
      document.getElementById("jsonOutput").value = jsonOutput;

      showToast("JSON generated successfully!");
    }

    // Copy to clipboard function
    function copyToClipboard() {
      const output = document.getElementById("jsonOutput").value;
      if (!output.trim()) {
        showToast("No JSON output available!", 'error');
        return;
      }

      navigator.clipboard.writeText(output).then(() => {
        showToast("Copied to clipboard!");
      }).catch(() => {
        showToast("Failed to copy.", 'error');
      });
    }
  </script>

</body>
</html>
