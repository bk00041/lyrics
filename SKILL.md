name: bilingual-lyrics-breakdown
description: Formats bilingual song lyrics with line-by-line translations and interactive slang/cultural explanation boxes using a dark-mode mobile-first layout.
instructions: |
  When requested to generate a lyrics translation page or break down a song for language learning, output a complete, standalone HTML document following this exact template, structure, and styling.

  ### Design & Layout Specifications
  1. Palette: Dark mode by default using CSS root variables.
     - `--bg-color: #0d0f12`
     - `--card-bg: #161a22`
     - `--accent-color: #ff2a5f`
     - `--accent-hover: #ff537c`
     - `--text-main: #f0f3f8`
     - `--text-sub: #9da7b8`
     - `--info-bg: #1c2230`
     - `--info-border: #3b82f6`
     - `--info-text: #dbeafe`
  2. Structure:
     - Header (`<header>`): Center-aligned title (`<h1>`) in accent color and subtitle (`<p class="subtitle">`) with artist information.
     - Instruction banner (`<div class="instruction">`): Styled pill banner giving user tips (e.g., "Tap the blue ℹ️ icon next to lines for slang and cultural explanations.").
     - Song Sections (`<div class="verse-block">`): Card layout grouping song sections (Intro, Verse, Chorus, Outro) with section titles (`<div class="section-title">`).
     - Line Pairs (`<div class="line-pair">`):
       - `.es-line`: Target/Original language line (bold, primary text color).
       - `.en-line`: English translation (secondary text color).
       - Interactive Info Button (`<button class="info-btn" onclick="toggleNote(this)" aria-label="Explanation">`): SVG info icon embedded inside `.en-line` when a slang, cultural, or idiomatic note exists.
       - Explanation Box (`<div class="explanation-box">`): Hidden expandable box below `.en-line` with custom left-border accenting and strong category labels (e.g., `<strong>Slang Breakdown:</strong>`, `<strong>Cultural Note:</strong>`).
  3. JavaScript:
     - Include the light toggle script `toggleNote(btn)` that targets the nearest `.line-pair` and toggles the `.active` class on `.explanation-box`.

  ### HTML Template Structure

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{SONG_TITLE} - {ARTIST_NAME} (Lyrics & Meaning)</title>
    <style>
      :root {
        --bg-color: #0d0f12;
        --card-bg: #161a22;
        --accent-color: #ff2a5f;
        --accent-hover: #ff537c;
        --text-main: #f0f3f8;
        --text-sub: #9da7b8;
        --info-bg: #1c2230;
        --info-border: #3b82f6;
        --info-text: #dbeafe;
      }

      * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
      }

      body {
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        background-color: var(--bg-color);
        color: var(--text-main);
        line-height: 1.5;
        padding: 20px 15px 60px;
      }

      .container {
        max-width: 650px;
        margin: 0 auto;
      }

      header {
        text-align: center;
        margin-bottom: 30px;
        padding-bottom: 20px;
        border-bottom: 1px solid #232936;
      }

      h1 {
        color: var(--accent-color);
        font-size: 2rem;
        margin-bottom: 5px;
        letter-spacing: -0.5px;
      }

      .subtitle {
        color: var(--text-sub);
        font-size: 1rem;
      }

      .instruction {
        text-align: center;
        font-size: 0.85rem;
        color: var(--text-sub);
        margin-bottom: 25px;
        background: rgba(255, 42, 95, 0.1);
        padding: 8px 12px;
        border-radius: 20px;
        display: inline-block;
        width: 100%;
      }

      .verse-block {
        background: var(--card-bg);
        border-radius: 12px;
        padding: 18px;
        margin-bottom: 20px;
        border: 1px solid #222836;
      }

      .section-title {
        color: var(--accent-color);
        font-size: 0.85rem;
        text-transform: uppercase;
        letter-spacing: 1px;
        margin-bottom: 12px;
        font-weight: 700;
      }

      .line-pair {
        margin-bottom: 14px;
      }

      .line-pair:last-child {
        margin-bottom: 0;
      }

      .es-line {
        font-weight: 600;
        font-size: 1.05rem;
        color: var(--text-main);
      }

      .en-line {
        font-size: 0.95rem;
        color: var(--text-sub);
        display: flex;
        align-items: center;
        gap: 6px;
        margin-top: 2px;
      }

      .info-btn {
        background: none;
        border: none;
        cursor: pointer;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        padding: 2px;
        border-radius: 50%;
        color: #3b82f6;
        transition: transform 0.2s, color 0.2s;
      }

      .info-btn:hover {
        color: #60a5fa;
        transform: scale(1.15);
      }

      .info-btn svg {
        width: 18px;
        height: 18px;
        fill: currentColor;
      }

      .explanation-box {
        display: none;
        background-color: var(--info-bg);
        border-left: 3px solid var(--info-border);
        color: var(--info-text);
        font-size: 0.88rem;
        padding: 10px 14px;
        margin-top: 8px;
        border-radius: 0 8px 8px 0;
        animation: fadeIn 0.25s ease-in-out;
      }

      .explanation-box.active {
        display: block;
      }

      @keyframes fadeIn {
        from { opacity: 0; transform: translateY(-4px); }
        to { opacity: 1; transform: translateY(0); }
      }
    </style>
  </head>
  <body>

    <div class="container">
      <header>
        <h1>{SONG_TITLE}</h1>
        <p class="subtitle">{ARTIST_NAME}</p>
      </header>

      <div class="instruction">
        💡 Tap the blue ℹ️ icon next to lines for slang and cultural explanations.
      </div>

      <!-- SECTION BLOCK EXAMPLE -->
      <div class="verse-block">
        <div class="section-title">{SECTION_NAME}</div>
        
        <div class="line-pair">
          <div class="es-line">{ORIGINAL_LINE}</div>
          <div class="en-line">
            {TRANSLATED_LINE}
            <button class="info-btn" onclick="toggleNote(this)" aria-label="Explanation">
              <svg viewBox="0 0 24 24"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1 15h-2v-6h2v6zm0-8h-2V7h2v2z"/></svg>
            </button>
          </div>
          <div class="explanation-box">
            <strong>{NOTE_TYPE}:</strong> {NOTE_CONTENT}
          </div>
        </div>
      </div>

    </div>

    <script>
      function toggleNote(btn) {
        const linePair = btn.closest('.line-pair');
        const box = linePair.querySelector('.explanation-box');
        
        if (box) {
          box.classList.toggle('active');
        }
      }
    </script>
  </body>
  </html>
