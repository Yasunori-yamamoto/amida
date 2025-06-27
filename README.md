<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>あみだくじ</title>
  <style>
    canvas { border: 1px solid #000; }
    #result { margin-top: 10px; font-size: 1.2em; }
  </style>
</head>
<body>
  <h1>あみだくじ</h1>
  <canvas id="amida" width="400" height="400"></canvas><br>
  <button onclick="startAmida()">あみだスタート！</button>
  <div id="result"></div>

  <script>
    const canvas = document.getElementById('amida');
    const ctx = canvas.getContext('2d');
    const num = 4; // 人数
    const lines = [];

    function drawAmida() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      const width = canvas.width;
      const height = canvas.height;
      const margin = width / (num + 1);

      // 縦線
      for (let i = 1; i <= num; i++) {
        ctx.beginPath();
        ctx.moveTo(margin * i, 0);
        ctx.lineTo(margin * i, height);
        ctx.stroke();
      }

      // ランダム横線
      lines.length = 0;
      for (let y = 20; y < height - 20; y += 40) {
        const idx = Math.floor(Math.random() * (num - 1)) + 1;
        const x = margin * idx;
        ctx.beginPath();
        ctx.moveTo(x, y);
        ctx.lineTo(x + margin, y);
        ctx.stroke();
        lines.push({ x: idx, y });
      }
    }

    function startAmida() {
      let pos = 1; // 左から2番目からスタート（例）
      const margin = canvas.width / (num + 1);
      let x = margin * pos;
      let y = 0;

      ctx.beginPath();
      ctx.moveTo(x, y);

      for (let h = 0; h < canvas.height; h += 1) {
        const line = lines.find(l => Math.abs(l.y - y) < 2 && (l.x === pos || l.x === pos - 1));
        if (line) {
          if (line.x === pos) pos++;
          else if (line.x === pos - 1) pos--;
          x = margin * pos;
        }
        ctx.lineTo(x, y);
        y++;
      }

      ctx.strokeStyle = 'red';
      ctx.stroke();
      ctx.strokeStyle = 'black';

      document.getElementById('result').textContent = `結果: ${pos} 番目！`;
    }

    drawAmida();
  </script>
</body>
</html>
