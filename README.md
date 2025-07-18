# Tiranga-Predictor-
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tiranga Color Predictor</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; background: #f2f2f2; }
    h1 { color: #333; }
    input, button { padding: 10px; font-size: 16px; margin-top: 10px; width: 100%; }
    .result { background: #fff; padding: 15px; margin-top: 20px; border-radius: 8px; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
  </style>
</head>
<body>
  <h1>Tiranga Color Predictor</h1>
  <p>अपनी पिछली रंगों की हिस्ट्री दर्ज करें (जैसे: Green,Red,Yellow,Green):</p>
  <input type="text" id="colorInput" placeholder="Enter colors separated by comma" />
  <button onclick="predictColor()">Predict Next Color</button>
  <div class="result" id="output"></div>

  <script>
    function predictColor() {
      const input = document.getElementById("colorInput").value;
      const colors = input.split(',').map(c => c.trim().toLowerCase());
      const validColors = ['green', 'red', 'yellow'];

      const count = { green: 0, red: 0, yellow: 0 };
      colors.forEach(color => {
        if (validColors.includes(color)) count[color]++;
      });

      const total = colors.length;
      if (total === 0) {
        document.getElementById("output").innerHTML = "कृपया वैध रंग दर्ज करें।";
        return;
      }

      const probs = Object.fromEntries(
        Object.entries(count).map(([color, val]) => [
          color,
          { count: val, probability: ((val / total) * 100).toFixed(2) + '%' }
        ])
      );

      const likely = Object.entries(count).sort((a, b) => b[1] - a[1])[0][0];

      document.getElementById("output").innerHTML = `
        <strong>Color Frequency:</strong><br>
        🟩 Green: ${count.green} (${probs.green.probability})<br>
        🟥 Red: ${count.red} (${probs.red.probability})<br>
        🟨 Yellow: ${count.yellow} (${probs.yellow.probability})<br>
        <br>
        <strong>Next Likely Color:</strong> ${likely.charAt(0).toUpperCase() + likely.slice(1)}
      `;
    }
  </script>
</body>
</html>
