<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Interactive Spirograph</title>
</head>
<body>
  <h2>Spirograph Drawing</h2>
  <label for="R">Outer Radius (R):</label>
  <input type="number" id="R" value="150" min="50" max="200"><br>
  
  <label for="r">Inner Radius (r):</label>
  <input type="number" id="r" value="50" min="10" max="150"><br>
  
  <label for="O">Offset (O):</label>
  <input type="number" id="O" value="10" min="5" max="50"><br>
  
  <button onclick="startSpirograph()">Start Spirograph</button>
  
  <canvas id="spirographCanvas" width="600" height="600"></canvas>

  <script>
    let canvas = document.getElementById("spirographCanvas");
    let ctx = canvas.getContext("2d");
    let t = 0;
    let R, r, O;

    function startSpirograph() {
      // Clear canvas
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      t = 0; // reset t to restart drawing
      
      // Get user inputs
      R = parseInt(document.getElementById("R").value);
      r = parseInt(document.getElementById("r").value);
      O = parseInt(document.getElementById("O").value);

      // Start the drawing
      drawSpirograph();
    }

    function getPosition(t) {
      // Spirograph equations
      const x = (R + r) * Math.cos(t) - (r + O) * Math.cos(((R + r) / r) * t);
      const y = (R + r) * Math.sin(t) - (r + O) * Math.sin(((R + r) / r) * t);
      return { x, y };
    }

    function drawSpirograph() {
      ctx.beginPath();
      let { x, y } = getPosition(t);
      ctx.moveTo(300 + x, 300 + y); // Offset for center of canvas
      
      const interval = setInterval(() => {
        t += 0.05; // Increase t to draw in small steps

        const { x, y } = getPosition(t);
        ctx.lineTo(300 + x, 300 + y);

        // Optional color change every cycle
        if (Math.floor(t * 10) % 20 === 0) {
          ctx.strokeStyle = '#' + Math.floor(Math.random() * 16777215).toString(16);
          ctx.stroke();
          ctx.beginPath();
          ctx.moveTo(300 + x, 300 + y);
        }

        ctx.stroke();

        // Stop condition when full cycle completes
        if (t > Math.PI * 2 * r / gcd(R, r)) {
          clearInterval(interval);
        }
      }, 20);
    }

    // Helper function for full cycle stop condition
    function gcd(a, b) {
      return b == 0 ? a : gcd(b, a % b);
    }
  </script>
</body>
</html>
