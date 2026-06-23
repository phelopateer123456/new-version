<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            transition: background 0.5s ease;
        }

        body.dark-theme {
            background: linear-gradient(135deg, #0f0c29 0%, #302b63 50%, #24243e 100%);
        }

        .calculator {
            background: rgba(255, 255, 255, 0.95);
            padding: 2rem;
            border-radius: 2rem;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            width: 340px;
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.4);
            position: relative;
            animation: slideIn 0.5s ease-out;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(-20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .dark-theme .calculator {
            background: rgba(30, 30, 40, 0.95);
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: #ffffff;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
        }

        .controls {
            position: absolute;
            top: 1rem;
            left: 1rem;
            display: flex;
            gap: 0.5rem;
            z-index: 10;
        }

        .theme-toggle, .history-btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 0.6rem 1rem;
            border-radius: 0.8rem;
            cursor: pointer;
            font-size: 0.85rem;
            font-weight: 600;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
        }

        .history-btn {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            box-shadow: 0 4px 15px rgba(245, 87, 108, 0.4);
        }

        .theme-toggle:hover, .history-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(102, 126, 234, 0.6);
        }

        .theme-toggle:active, .history-btn:active {
            transform: translateY(0);
        }

        .dark-theme .theme-toggle {
            background: linear-gradient(135deg, #00c853 0%, #00e676 100%);
            box-shadow: 0 4px 15px rgba(0, 200, 83, 0.4);
        }

        .dark-theme .history-btn {
            background: linear-gradient(135deg, #8e44ad 0%, #9b59b6 100%);
            box-shadow: 0 4px 15px rgba(142, 68, 173, 0.4);
        }

        .display {
            width: 100%;
            height: 80px;
            border: none;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            color: #2d3436;
            text-align: right;
            padding: 1rem 1.5rem;
            font-size: 2.2rem;
            margin-bottom: 1.5rem;
            font-weight: 300;
            border-radius: 1rem;
            box-shadow: inset 0 2px 10px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
        }

        .display:focus {
            outline: none;
            box-shadow: inset 0 2px 10px rgba(102, 126, 234, 0.3);
        }

        .dark-theme .display {
            background: linear-gradient(135deg, #2d2d3d 0%, #1a1a2e 100%);
            color: #ffffff;
            box-shadow: inset 0 2px 10px rgba(0, 0, 0, 0.3);
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 0.7rem;
        }

        button {
            padding: 1.1rem;
            border: none;
            background: linear-gradient(135deg, #ffffff 0%, #f0f0f0 100%);
            color: #2d3436;
            font-size: 1.15rem;
            cursor: pointer;
            border-radius: 1rem;
            transition: all 0.2s ease;
            font-weight: 600;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            position: relative;
            overflow: hidden;
        }

        button::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.5);
            transform: translate(-50%, -50%);
            transition: width 0.6s, height 0.6s;
        }

        button:active::before {
            width: 300px;
            height: 300px;
        }

        .dark-theme button {
            background: linear-gradient(135deg, #3d3d4d 0%, #2d2d3d 100%);
            color: #ffffff;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
        }

        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
        }

        button:active {
            transform: translateY(-1px);
        }

        .dark-theme button:hover {
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.4);
        }

        .operator {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .dark-theme .operator {
            background: linear-gradient(135deg, #5a67d8 0%, #6b46c1 100%);
        }

        .equals {
            background: linear-gradient(135deg, #00c853 0%, #00e676 100%);
            color: white;
        }

        .dark-theme .equals {
            background: linear-gradient(135deg, #00a844 0%, #00c853 100%);
        }

        .clear {
            background: linear-gradient(135deg, #ff5252 0%, #ff1744 100%);
            color: white;
        }

        .dark-theme .clear {
            background: linear-gradient(135deg, #d32f2f 0%, #c62828 100%);
        }

        .special {
            background: linear-gradient(135deg, #f5f7fa 0%, #e8ecf1 100%);
            color: #667eea;
        }

        .dark-theme .special {
            background: linear-gradient(135deg, #3d3d4d 0%, #2d2d3d 100%);
            color: #667eea;
        }

        .zero {
            grid-column: span 2;
        }

        .history-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            justify-content: center;
            align-items: center;
            z-index: 1000;
            backdrop-filter: blur(5px);
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .history-content {
            background: white;
            padding: 2rem;
            border-radius: 1.5rem;
            max-width: 320px;
            width: 90%;
            max-height: 500px;
            overflow-y: auto;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            animation: slideUp 0.3s ease;
        }

        @keyframes slideUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .dark-theme .history-content {
            background: #2d2d3d;
            color: white;
        }

        .history-entry {
            padding: 0.8rem 0;
            border-bottom: 1px solid #eee;
            transition: background 0.2s ease;
        }

        .history-entry:hover {
            background: rgba(102, 126, 234, 0.1);
        }

        .dark-theme .history-entry {
            border-color: #444;
        }

        .dark-theme .history-entry:hover {
            background: rgba(102, 126, 234, 0.2);
        }

        .close-btn {
            float: right;
            cursor: pointer;
            padding: 0.5rem 1rem;
            background: #ff5252;
            color: white;
            border-radius: 0.5rem;
            border: none;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .close-btn:hover {
            background: #ff1744;
            transform: scale(1.05);
        }

        .history-content::-webkit-scrollbar {
            width: 8px;
        }

        .history-content::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }

        .history-content::-webkit-scrollbar-thumb {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 10px;
        }

        .dark-theme .history-content::-webkit-scrollbar-track {
            background: #1a1a2e;
        }

        @media (max-width: 400px) {
            .calculator {
                width: 95%;
                padding: 1.5rem;
            }
            
            button {
                padding: 0.9rem;
                font-size: 1rem;
            }

            .display {
                font-size: 1.8rem;
                height: 70px;
            }
        }

        @media (max-width: 360px) {
            .controls {
                position: relative;
                top: 0;
                left: 0;
                margin-bottom: 1rem;
                justify-content: center;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="controls">
            <button class="theme-toggle" id="themeToggle" title="Toggle Theme">🌙</button>
            <button class="history-btn" id="historyBtn" title="View History">🕒</button>
        </div>
        
        <div class="display" id="display">0</div>
        
        <div class="buttons">
            <button class="clear" data-action="clear">C</button>
            <button class="special" data-action="backspace">⌫</button>
            <button class="special" data-action="percent">%</button>
            <button class="operator" data-action="operator" data-value="/">÷</button>
            
            <button data-value="7">7</button>
            <button data-value="8">8</button>
            <button data-value="9">9</button>
            <button class="operator" data-action="operator" data-value="*">×</button>
            
            <button data-value="4">4</button>
            <button data-value="5">5</button>
            <button data-value="6">6</button>
            <button class="operator" data-action="operator" data-value="-">−</button>
            
            <button data-value="1">1</button>
            <button data-value="2">2</button>
            <button data-value="3">3</button>
            <button class="operator" data-action="operator" data-value="+">+</button>
            
            <button class="special zero" data-value="0">0</button>
            <button class="special" data-action="decimal">.</button>
            <button class="equals" data-action="calculate">=</button>
        </div>
    </div>

    <div class="history-modal" id="historyModal">
        <div class="history-content">
            <button class="close-btn" id="closeHistory">✕</button>
            <h2 style="margin-bottom: 1rem;">History</h2>
            <div id="historyList"></div>
        </div>
    </div>

    <script>
        let currentOperand = '0';
        let previousOperand = '';
        let operation = null;
        let shouldResetScreen = false;
        let history = JSON.parse(localStorage.getItem('calcHistory')) || [];

        const display = document.getElementById('display');
        const themeToggle = document.getElementById('themeToggle');
        const historyBtn = document.getElementById('historyBtn');
        const historyModal = document.getElementById('historyModal');
        const closeHistory = document.getElementById('closeHistory');
        const historyList = document.getElementById('historyList');

        function updateDisplay() {
            display.textContent = currentOperand;
            if (currentOperand.length > 10) {
                display.style.fontSize = '1.5rem';
            } else {
                display.style.fontSize = '';
            }
        }

        function appendNumber(number) {
            if (currentOperand === 'Error') clearAll();
            if (shouldResetScreen) {
                currentOperand = '';
                shouldResetScreen = false;
            }
            if (currentOperand === '0' && number !== '.') {
                currentOperand = number;
            } else {
                currentOperand += number;
            }
            updateDisplay();
        }

        function appendDecimal() {
            if (shouldResetScreen) {
                currentOperand = '0';
                shouldResetScreen = false;
            }
            if (!currentOperand.includes('.')) {
                currentOperand += '.';
            }
            updateDisplay();
        }

        function chooseOperation(op) {
            if (currentOperand === 'Error') return;
            if (previousOperand !== '') {
                calculate();
            }
            operation = op;
            previousOperand = currentOperand + ' ' + getOpSymbol(op);
            shouldResetScreen = true;
        }

        function getOpSymbol(op) {
            const symbols = { '/': '÷', '*': '×', '-': '−', '+': '+' };
            return symbols[op] || op;
        }

        function calculate() {
            if (operation === null || shouldResetScreen) return;
            
            let computation;
            const prev = parseFloat(previousOperand);
            const current = parseFloat(currentOperand);
            
            if (isNaN(prev) || isNaN(current)) return;

            switch (operation) {
                case '+': computation = prev + current; break;
                case '-': computation = prev - current; break;
                case '*': computation = prev * current; break;
                case '/': 
                    if (current === 0) {
                        currentOperand = 'Error';
                        updateDisplay();
                        previousOperand = '';
                        operation = null;
                        return;
                    }
                    computation = prev / current; 
                    break;
                default: return;
            }

            computation = Math.round(computation * 1e12) / 1e12;

            const historyEntry = `${previousOperand} ${currentOperand} = ${computation}`;
            history.unshift(historyEntry);
            if (history.length > 20) history.pop();
            localStorage.setItem('calcHistory', JSON.stringify(history));

            currentOperand = computation.toString();
            operation = null;
            previousOperand = '';
            shouldResetScreen = true;
            updateDisplay();
        }

        function clearAll() {
            currentOperand = '0';
            previousOperand = '';
            operation = null;
            updateDisplay();
        }

        function backspace() {
            if (currentOperand === 'Error') {
                clearAll();
                return;
            }
            currentOperand = currentOperand.slice(0, -1) || '0';
            updateDisplay();
        }

        function percent() {
            if (currentOperand === '0' || currentOperand === 'Error') return;
            currentOperand = (parseFloat(currentOperand) / 100).toString();
            updateDisplay();
        }

        document.querySelector('.buttons').addEventListener('click', (e) => {
            const btn = e.target.closest('button');
            if (!btn) return;

            const action = btn.dataset.action;
            const value = btn.dataset.value;

            if (!action) {
                appendNumber(value);
            } else {
                switch(action) {
                    case 'decimal': appendDecimal(); break;
                    case 'operator': chooseOperation(value); break;
                    case 'calculate': calculate(); break;
                    case 'clear': clearAll(); break;
                    case 'backspace': backspace(); break;
                    case 'percent': percent(); break;
                }
            }
        });

        document.addEventListener('keydown', (e) => {
            if (e.key >= '0' && e.key <= '9') appendNumber(e.key);
            if (e.key === '.') appendDecimal();
            if (e.key === '+' || e.key === '-' || e.key === '*' || e.key === '/') chooseOperation(e.key);
            if (e.key === 'Enter' || e.key === '=') {
                e.preventDefault();
                calculate();
            }
            if (e.key === 'Escape') clearAll();
            if (e.key === 'Backspace') backspace();
            if (e.key === '%') percent();
        });

        if (localStorage.getItem('theme') === 'dark') {
            document.body.classList.add('dark-theme');
            themeToggle.textContent = '☀️';
        }

        themeToggle.addEventListener('click', () => {
            document.body.classList.toggle('dark-theme');
            const isDark = document.body.classList.contains('dark-theme');
            themeToggle.textContent = isDark ? '☀️' : '🌙';
            localStorage.setItem('theme', isDark ? 'dark' : 'light');
        });

        function renderHistory() {
            historyList.innerHTML = '';
            if (history.length === 0) {
                historyList.innerHTML = '<p style="text-align:center; color:#888; padding: 1rem;">No history yet</p>';
                return;
            }
            history.forEach(entry => {
                const div = document.createElement('div');
                div.className = 'history-entry';
                div.textContent = entry;
                historyList.appendChild(div);
            });
        }

        historyBtn.addEventListener('click', () => {
            renderHistory();
            historyModal.style.display = 'flex';
        });

        closeHistory.addEventListener('click', () => {
            historyModal.style.display = 'none';
        });

        historyModal.addEventListener('click', (e) => {
            if (e.target === historyModal) {
                historyModal.style.display = 'none';
            }
        });

        updateDisplay();
    </script>
</body>
</html>
