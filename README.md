# SimpleCalculator
This is a responsive and interactive web-based calculator designed using HTML, CSS, and JavaScript. It features a modern, clean UI with enhanced usability through animations, keyboard support, and a dark mode toggle. It performs basic arithmetic operations like addition, subtraction, multiplication, and division.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Simple Calculator By JayeshKarale</title>
  <style>
    :root {
      --bg-color: #ffffff;
      --btn-color: #f0f0f0;
      --btn-hover: #dcdcdc;
      --text-color: #333;
      --operator-color: #ffd166;
      --operator-hover: #fcbf49;
      --equal-color: #06d6a0;
      --equal-hover: #05c292;
      --clear-color: #ff6b6b;
      --clear-hover: #ff4d4d;
      --delete-color: #6c757d;
      --delete-hover: #5a6268;
    }

    body.dark {
      --bg-color: #2b2d42;
      --btn-color: #3a3c58;
      --btn-hover: #4e5170;
      --text-color: #f8f8f8;
      --operator-color: #ffbe0b;
      --operator-hover: #faa307;
      --equal-color: #00f5d4;
      --equal-hover: #06d6a0;
      --clear-color: #ef233c;
      --clear-hover: #d90429;
      --delete-color: #adb5bd;
      --delete-hover: #ced4da;
    }

    body {
      background: var(--bg-color);
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      height: 100vh;
      margin: 0;
      transition: background 0.3s ease;
    }

    .calculator {
      background: var(--bg-color);
      padding: 2rem;
      border-radius: 16px;
      box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
      width: 320px;
      transition: background 0.3s ease;
    }

    .display {
      background: var(--btn-color);
      border-radius: 10px;
      padding: 1rem;
      text-align: right;
      font-size: 2rem;
      color: var(--text-color);
      margin-bottom: 1rem;
      overflow-x: auto;
      transition: background 0.3s ease, color 0.3s ease;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 0.75rem;
    }

    .button {
      background: var(--btn-color);
      border: none;
      border-radius: 12px;
      padding: 1rem;
      font-size: 1.2rem;
      cursor: pointer;
      color: var(--text-color);
      transition: background 0.2s ease, transform 0.1s ease;
    }

    .button:hover {
      background: var(--btn-hover);
    }

    .button:active {
      transform: scale(0.95);
    }

    .button.operator {
      background: var(--operator-color);
    }

    .button.operator:hover {
      background: var(--operator-hover);
    }

    .button.equal {
      background: var(--equal-color);
      color: black;
    }

    .button.equal:hover {
      background: var(--equal-hover);
    }

    .button.clear {
      background: var(--clear-color);
      color: white;
    }

    .button.clear:hover {
      background: var(--clear-hover);
    }

    .button.delete {
      background: var(--delete-color);
      color: white;
    }

    .button.delete:hover {
      background: var(--delete-hover);
    }

    .footer {
      margin-top: 20px;
      font-size: 14px;
      color: var(--text-color);
    }

    .toggle-mode {
      margin-bottom: 15px;
      padding: 8px 14px;
      background: var(--btn-color);
      border: none;
      border-radius: 8px;
      cursor: pointer;
      color: var(--text-color);
      font-weight: bold;
      transition: background 0.3s ease;
    }

    .toggle-mode:hover {
      background: var(--btn-hover);
    }
  </style>
</head>
<body>

<button class="toggle-mode" onclick="toggleDarkMode()">🌙 Toggle Dark Mode</button>

<div class="calculator">
  <div class="display" id="display">0</div>
  <div class="buttons">
    <button class="button clear">C</button>
    <button class="button delete">⌫</button>
    <button class="button">.</button>
    <button class="button operator">/</button>
    
    <button class="button">7</button>
    <button class="button">8</button>
    <button class="button">9</button>
    <button class="button operator">*</button>
    
    <button class="button">4</button>
    <button class="button">5</button>
    <button class="button">6</button>
    <button class="button operator">-</button>
    
    <button class="button">1</button>
    <button class="button">2</button>
    <button class="button">3</button>
    <button class="button operator">+</button>
    
    <button class="button" style="grid-column: span 2;">0</button>
    <button class="button equal" style="grid-column: span 2;">=</button>
  </div>
</div>

<div class="footer">
  Created by <strong>Jayesh Karale</strong>
</div>

<script>
  const display = document.getElementById('display');
  const buttons = document.querySelectorAll('.button');
  let currentInput = '';

  function updateDisplay() {
    display.textContent = currentInput || '0';
  }

  buttons.forEach(button => {
    button.addEventListener('click', () => {
      const value = button.textContent;

      if (button.classList.contains('clear')) {
        currentInput = '';
      } else if (button.classList.contains('delete')) {
        currentInput = currentInput.slice(0, -1);
      } else if (button.classList.contains('equal')) {
        try {
          currentInput = eval(currentInput).toString();
        } catch {
          currentInput = 'Error';
        }
      } else {
        if (currentInput === 'Error') {
          currentInput = value;
        } else {
          currentInput += value;
        }
      }
      updateDisplay();
    });
  });

  // Keyboard input support
  document.addEventListener('keydown', (e) => {
    const allowedKeys = '0123456789.+-*/';
    if (allowedKeys.includes(e.key)) {
      if (currentInput === 'Error') currentInput = '';
      currentInput += e.key;
    } else if (e.key === 'Backspace') {
      currentInput = currentInput.slice(0, -1);
    } else if (e.key === 'Enter') {
      try {
        currentInput = eval(currentInput).toString();
      } catch {
        currentInput = 'Error';
      }
    } else if (e.key.toLowerCase() === 'c') {
      currentInput = '';
    }
    updateDisplay();
  });

  // Dark mode toggle
  function toggleDarkMode() {
    document.body.classList.toggle('dark');
  }
</script>

</body>
</html>

