<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Générateur de mots de passe sécurisé</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      margin: 0;
      padding: 0;
      text-align: center;
    }
    header, section {
      margin: 2rem auto;
      max-width: 600px;
      background: #fff;
      border-radius: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      padding: 2rem;
    }
    h1 {
      color: #333;
    }
    button, input[type="number"] {
      padding: 0.5rem 1rem;
      margin: 0.5rem;
      border-radius: 5px;
      border: none;
      cursor: pointer;
    }
    button {
      background: #007BFF;
      color: #fff;
    }
    button:hover {
      background: #0056b3;
    }
    input[type="checkbox"] {
      margin: 0.5rem;
    }
    #passwordOutput {
      font-size: 1.2rem;
      font-weight: bold;
      margin: 1rem 0;
      word-break: break-all;
    }
    textarea {
      width: 100%;
      height: 150px;
      margin-top: 1rem;
      padding: 0.5rem;
    }
  </style>
</head>
<body>

  <!-- Page d'accueil -->
  <section id="homePage">
    <h1>Bienvenue sur le générateur de mots de passe sécurisé</h1>
    <p>Créez des mots de passe solides et sauvegardez-les facilement !</p>
    <button onclick="showGeneratorPage()">Commencer</button>
  </section>

  <!-- Page de génération -->
  <section id="generatorPage" style="display:none;">
    <h1>Générateur de mots de passe</h1>
    <div>
      <label>Longueur :
        <input type="number" id="length" min="4" max="50" value="12">
      </label>
    </div>
    <div>
      <label><input type="checkbox" id="uppercase" checked> Majuscules</label>
      <label><input type="checkbox" id="lowercase" checked> Minuscules</label>
      <label><input type="checkbox" id="numbers" checked> Chiffres</label>
      <label><input type="checkbox" id="symbols" checked> Symboles</label>
    </div>
    <button onclick="generatePassword()">Générer</button>
    <div id="passwordOutput"></div>
    <button onclick="savePassword()">Enregistrer dans le bloc-notes</button>

    <h2>Bloc-notes</h2>
    <textarea id="notepad" placeholder="Vos mots de passe sauvegardés..."></textarea>
    <button onclick="saveNotepad()">Sauvegarder le bloc-notes</button>
    <button onclick="clearNotepad()">Vider le bloc-notes</button>
  </section>

  <script>
    function showGeneratorPage() {
      document.getElementById('homePage').style.display = 'none';
      document.getElementById('generatorPage').style.display = 'block';
      loadNotepad();
    }

    function generatePassword() {
      const length = parseInt(document.getElementById('length').value);
      const includeUppercase = document.getElementById('uppercase').checked;
      const includeLowercase = document.getElementById('lowercase').checked;
      const includeNumbers = document.getElementById('numbers').checked;
      const includeSymbols = document.getElementById('symbols').checked;

      const uppercase = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
      const lowercase = "abcdefghijklmnopqrstuvwxyz";
      const numbers = "0123456789";
      const symbols = "!@#$%^&*()_+{}[]:;,.<>?";

      let charset = "";
      if (includeUppercase) charset += uppercase;
      if (includeLowercase) charset += lowercase;
      if (includeNumbers) charset += numbers;
      if (includeSymbols) charset += symbols;

      let password = "";
      for (let i = 0; i < length; i++) {
        const randomIndex = Math.floor(Math.random() * charset.length);
        password += charset[randomIndex];
      }

      document.getElementById('passwordOutput').textContent = password;
    }

    function savePassword() {
      const password = document.getElementById('passwordOutput').textContent;
      if (password) {
        const notepad = document.getElementById('notepad');
        notepad.value += password + "\n";
        saveNotepad();
        alert("Mot de passe enregistré !");
      }
    }

    function saveNotepad() {
      const content = document.getElementById('notepad').value;
      localStorage.setItem('notepad', content);
    }

    function loadNotepad() {
      const saved = localStorage.getItem('notepad');
      if (saved) {
        document.getElementById('notepad').value = saved;
      }
    }

    function clearNotepad() {
      if (confirm("Voulez-vous vraiment vider le bloc-notes ?")) {
        document.getElementById('notepad').value = "";
        saveNotepad();
      }
    }
  </script>

</body>
</html>
