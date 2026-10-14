# notebook-English-Russian
This is my first repository. I’m just trying to create a convenient dictionary for learning. If anyone needs it, feel free to download and improve it
[vocab.html](https://github.com/user-attachments/files/33435329/vocab.html)

<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Мой словарик (бар + программирование)</title>
  <style>
    :root {
      --bg: #f4f6f9;
      --card: #ffffff;
      --primary: #3b82f6;
      --text-main: #1f2937;
      --text-muted: #6b7280;
      --border: #e5e7eb;
      --radius: 12px;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      background-color: var(--bg);
      color: var(--text-main);
      margin: 0;
      padding: 20px;
      line-height: 1.5;
    }

    .container {
      max-width: 960px;
      margin: 0 auto;
    }

    h1 {
      text-align: center;
      font-size: 24px;
      color: var(--text-main);
      margin-bottom: 24px;
    }

    /* Панель добавления */
    .add-panel {
      background: var(--card);
      padding: 20px;
      border-radius: var(--radius);
      box-shadow: 0 4px 12px rgba(0,0,0,0.04);
      display: grid;
      grid-template-columns: 1fr 1fr 1fr auto;
      gap: 12px;
      align-items: end;
      margin-bottom: 30px;
    }

    @media (max-width: 768px) {
      .add-panel {
        grid-template-columns: 1fr;
        grid-template-rows: auto auto auto auto;
      }
    }

    label {
      display: block;
      font-size: 13px;
      font-weight: 600;
      color: var(--text-muted);
      margin-bottom: 6px;
    }

    input, select {
      width: 100%;
      padding: 10px 12px;
      border: 1px solid var(--border);
      border-radius: 8px;
      font-size: 15px;
      box-sizing: border-box;
    }

    button.add-btn {
      background: var(--primary);
      color: white;
      border: none;
      padding: 14px;
      border-radius: 8px;
      font-size: 16px;
      font-weight: 600;
      cursor: pointer;
      transition: background 0.2s;
      height: 48px;
    }

    button.add-btn:hover {
      background: #2563eb;
    }

    /* Таблица */
    table {
      width: 100%;
      background: var(--card);
      border-collapse: collapse;
      border-radius: var(--radius);
      overflow: hidden;
      box-shadow: 0 4px 12px rgba(0,0,0,0.03);
    }

    th {
      text-align: left;
      padding: 14px 16px;
      background: #f9fafb;
      color: var(--text-muted);
      font-size: 13px;
      font-weight: 600;
      border-bottom: 1px solid var(--border);
    }

    td {
      padding: 14px 16px;
      border-bottom: 1px solid var(--border);
      vertical-align: top;
    }

    tr:last-child td {
      border-bottom: none;
    }

    .word-col {
      font-weight: 700;
      color: #111827;
      font-size: 16px;
    }

    .trans-col {
      color: #374151;
      font-size: 15px;
    }

    .example-col {
      color: var(--text-muted);
      font-style: italic;
      font-size: 14px;
      line-height: 1.6;
    }

    /* Адаптив для телефона */
    @media (max-width: 640px) {
      table, thead, tbody, th, td, tr {
        display: block;
      }
      thead {
        position: absolute;
        left: -9999px;
      }
      tr {
        margin-bottom: 16px;
        border: 1px solid var(--border);
        border-radius: 8px;
        padding: 12px;
        background: var(--card);
      }
      td {
        border: none;
        border-bottom: 1px dashed var(--border);
        padding-bottom: 8px;
        margin-bottom: 8px;
        text-align: left;
      }
      td:last-child {
        border-bottom: none;
      }
      td::before {
        content: attr(data-label);
        font-weight: 700;
        color: var(--primary);
        display: block;
        font-size: 12px;
        margin-bottom: 4px;
        text-transform: uppercase;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Мой словарик (бар + программирование)</h1>

    <!-- Панель добавления -->
    <div class="add-panel">
      <div>
        <label for="word">Слово (англ.)</label>
        <input type="text" id="word" placeholder="trendy" oninput="autoTranscribe()" />
      </div>
      <div>
        <label for="trans">Транскрипция (рус.)</label>
        <input type="text" id="trans" placeholder="тренди" />
      </div>
      <div>
        <label for="example">Пример фразы</label>
        <input type="text" id="example" placeholder="This bar is trendy." />
      </div>
      <button class="add-btn" onclick="addWord()">Добавить</button>
    </div>

    <!-- Таблица: слово | транскрипция | пример -->
    <table>
      <thead>
        <tr>
          <th>Слово (англ.)</th>
          <th>Транскрипция (рус.)</th>
          <th>Пример фразы</th>
        </tr>
      </thead>
      <tbody id="tableBody"></tbody>
    </table>
  </div>

  <script>
    // Начальная база слов (бар + программирование)
    const initialWords = [
      // Бар / общение
      { word: "vibe", trans: "вайб", example: "I like the vibe of this place." },
      { word: "trendy", trans: "тренди", example: "This bar is so trendy, everyone comes here." },
      { word: "crowd", trans: "крауд", example: "The crowd is pretty cool tonight." },
      { word: "to hit it off", trans: "ту хит ит оф", example: "We really hit it off right away." },
      { word: "chill", trans: "чил", example: "Let’s just chill and talk." },
      { word: "ambiance", trans: "эмбиэнс", example: "I love the ambiance here, it’s not too loud." },
      { word: "to spark a conversation", trans: "ту спарк э конверсэйшн", example: "I’m trying to spark a conversation with that guy." },
      { word: "approachable", trans: "эпроучэбл", example: "He looks really approachable, I’ll say hi." },
      { word: "to break the ice", trans: "ту брейк зэ айс", example: "A simple ‘Hi, how’s it going?’ helps break the ice." },
      { word: "to be into something", trans: "ту би инту самсинг", example: "What are you into? Music? Travel?" },
      { word: "laid-back", trans: "лейд-бэк", example: "I prefer laid-back bars where you can just chat." },
      { word: "to run into someone", trans: "ту ран инту самван", example: "It’s great to run into you here!" },
      { word: "spot", trans: "спот", example: "This is my favorite spot in the city." },
      { word: "to get to know someone", trans: "ту гет ту ноу самван", example: "I’d love to get to know you better." },
      { word: "good call", trans: "гуд кол", example: "Coming here was a good call." },

      // Программирование (продвинутые)
      { word: "algorithm", trans: "алгоритм", example: "We need to optimize this algorithm for speed." },
      { word: "debug", trans: "дебаг", example: "I spent two hours trying to debug this function." },
      { word: "framework", trans: "фреймворк", example: "Our team uses a popular frontend framework." },
      { word: "repository", trans: "репозиторий", example: "Push the changes to the main repository." },
      { word: "deployment", trans: "деплоймент", example: "The deployment to production went smoothly." },
      { word: "scalability", trans: "скейлабилити", example: "This architecture has great scalability for growth." },
      { word: "latency", trans: "лэтэнси", example: "High latency makes the app feel slow for users." },
      { word: "authentication", trans: "оутентикейшн", example: "Authentication is handled by our central service." },
      { word: "legacy code", trans: "леджеси код", example: "We’re slowly refactoring the legacy code." },
      { word: "refactor", trans: "рефактор", example: "Time to refactor this module before adding new features." },
      { word: "constraint", trans: "констрэинт", example: "There are strict time constraints on this task." },
      { word: "concurrency", trans: "конкэрэнси", example: "Concurrency makes this part of the system tricky." },
      { word: "pipeline", trans: "пайплайн", example: "Our CI/CD pipeline runs tests automatically." },
      { word: "microservice", trans: "микросервис", example: "Each microservice handles a specific business domain." },
      { word: "cache", trans: "кэш", example: "We added a cache layer to improve performance." },
      { word: "endpoint", trans: "эндпоинт", example: "Call the API endpoint to get the latest data." },
      { word: "validation", trans: "вэлидейшн", example: "Input validation is critical for security." },
      { word: "iteration", trans: "итэрэйшн", example: "After several iterations, we got the design right." },
      { word: "optimization", trans: "оптимайзэйшн", example: "Database optimization reduced query time significantly." },
      { word: "integration", trans: "интегрэйшн", example: "Integration with the payment gateway took a week." }
    ];

    let words = JSON.parse(localStorage.getItem('vocab_words')) || initialWords;

    function autoTranscribe() {
      const word = document.getElementById('word').value.trim().toLowerCase();
      if (!word) {
        document.getElementById('trans').value = '';
        return;
      }

      let trans = word;
      trans = trans.replace(/th/g, 'с').replace(/sh/g, 'ш').replace(/ch/g, 'ч');
      trans = trans.replace(/ph/g, 'ф').replace(/qu/g, 'кв').replace(/ck/g, 'к');
      
      trans = trans
        .replace(/a/g, 'а').replace(/e/g, 'э').replace(/i/g, 'и')
        .replace(/o/g, 'о').replace(/u/g, 'у').replace(/y/g, 'й');
      
      trans = trans
        .replace(/b/g, 'б').replace(/c/g, 'к').replace(/d/g, 'д')
        .replace(/f/g, 'ф').replace(/g/g, 'г').replace(/h/g, 'х')
        .replace(/j/g, 'дж').replace(/k/g, 'к').replace(/l/g, 'л')
        .replace(/m/g, 'м').replace(/n/g, 'н').replace(/p/g, 'п')
        .replace(/q/g, 'к').replace(/r/g, 'р').replace(/s/g, 'с')
        .replace(/t/g, 'т').replace(/v/g, 'в').replace(/w/g, 'в')
        .replace(/x/g, 'кс').replace(/z/g, 'з');

      document.getElementById('trans').value = trans;
    }

    function addWord() {
      const word = document.getElementById('word').value.trim();
      const trans = document.getElementById('trans').value.trim();
      const example = document.getElementById('example').value.trim();

      if (!word) return alert('Введите английское слово');

      words.push({ word, trans, example });
      saveAndRender();
      clearInputs();
    }

    function clearInputs() {
      document.getElementById('word').value = '';
      document.getElementById('trans').value = '';
      document.getElementById('example').value = '';
    }

    function saveAndRender() {
      localStorage.setItem('vocab_words', JSON.stringify(words));
      renderTable();
    }

    function renderTable() {
      const tbody = document.getElementById('tableBody');
      tbody.innerHTML = '';

      words.slice().reverse().forEach(w => {
        const row = `
          <tr>
            <td data-label="Слово" class="word-col">${escapeHtml(w.word)}</td>
            <td data-label="Транскрипция" class="trans-col">${escapeHtml(w.trans)}</td>
            <td data-label="Пример" class="example-col">${escapeHtml(w.example)}</td>
          </tr>
        `;
        tbody.innerHTML += row;
      });
    }

    function escapeHtml(str) {
      return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    }

    renderTable();
  </script>
</body>
</html>
