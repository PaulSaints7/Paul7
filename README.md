<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Fernaditor</title>
  <link rel="icon" href="icone.png" />
  <link rel="apple-touch-icon" href="icone.png" />
  <meta name="theme-color" content="#C8A2C8" />
  <link rel="manifest" href="manifest.json" />
  <link href="https://fonts.googleapis.com/css2?family=Dancing+Script&display=swap" rel="stylesheet">
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: 'Dancing Script', cursive;
      background-image: url('https://i.imgur.com/ZT6Ymz9.jpg');
      background-size: cover;
      background-repeat: no-repeat;
      background-attachment: fixed;
      background-position: center;
      color: #333;
    }
    .container {
      max-width: 90%;
      margin: 20px auto;
      background: rgba(255, 255, 255, 0.9);
      padding: 20px;
      border-radius: 20px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    textarea {
      width: 96%;
      height: 300px;
      border: 2px solid #C8A2C8;
      border-radius: 10px;
      padding: 10px;
      font-size: 18px;
      resize: vertical;
      background-color: #FFF0F5;
    }
    button {
      font-family: 'Dancing Script', cursive;
      background-color: #98FF98;
      border: none;
      padding: 10px 20px;
      margin: 10px 5px;
      border-radius: 10px;
      cursor: pointer;
      font-size: 18px;
      transition: background 0.3s;
    }
    button:hover {
      background-color: #C8A2C8;
      color: white;
    }
    input[type="file"] {
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Fernaditor 💐</h1>
    <textarea id="editor" placeholder="Escreva seu texto aqui..."></textarea><br>
    <button onclick="copiarTexto()">Copiar Texto</button>
    <button onclick="gerarPDF()">Gerar PDF</button>
    <button onclick="lerTexto()">Voz alta</button>
    <button onclick="baixarHTML()">📥 Baixar HTML</button><br>
    <label for="pdfInput">📂 Abrir PDF </label>
    <input type="file" id="pdfInput" accept="application/pdf" onchange="abrirPDFExternamente(event)">
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
  <script>
    function copiarTexto() {
      const texto = document.getElementById("editor");
      texto.select();
      document.execCommand("copy");
      alert("Texto copiado com sucesso!");
    }

    function gerarPDF() {
      const elemento = document.getElementById("editor");
      const opt = {
        margin: 1,
        filename: 'meu_texto.pdf',
        image: { type: 'jpeg', quality: 0.98 },
        html2canvas: { scale: 2 },
        jsPDF: { unit: 'in', format: 'letter', orientation: 'portrait' }
      };
      html2pdf().from(elemento.value).set(opt).save();
    }

    function lerTexto() {
      const texto = document.getElementById("editor").value;
      const sintese = window.speechSynthesis;
      const fala = new SpeechSynthesisUtterance(texto);
      fala.lang = "pt-BR";
      sintese.speak(fala);
    }

    function abrirPDFExternamente(event) {
      const file = event.target.files[0];
      if (file && file.type === "application/pdf") {
        const fileURL = URL.createObjectURL(file);
        window.open(fileURL, '_blank');
      } else {
        alert("Por favor, selecione um arquivo PDF válido.");
      }
    }

    function baixarHTML() {
      const htmlContent = document.documentElement.outerHTML;
      const blob = new Blob([htmlContent], { type: 'text/html' });
      const link = document.createElement('a');
      link.href = URL.createObjectURL(blob);
      link.download = 'editor_florido.html';
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    }
  </script>
</body>
</html>
