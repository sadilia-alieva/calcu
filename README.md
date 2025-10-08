<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Калькулятор</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }
        
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            padding: 20px;
        }
        
        .calculator {
            width: 100%;
            max-width: 400px;
            background-color: #2c2c2c;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        }
        
        .display {
            background-color: #1a1a1a;
            color: white;
            padding: 25px 20px;
            text-align: right;
            font-size: 2.5rem;
            min-height: 120px;
            word-wrap: break-word;
            overflow-wrap: break-word;
        }
        
        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1px;
            background-color: #3c3c3c;
        }
        
        button {
            border: none;
            padding: 20px 10px;
            font-size: 1.5rem;
            cursor: pointer;
            transition: all 0.2s;
            background-color: #4a4a4a;
            color: white;
        }
        
        button:active {
            background-color: #5a5a5a;
            transform: scale(0.95);
        }
        
        .operator {
            background-color: #ff9500;
            color: white;
        }
        
        .operator:active {
            background-color: #ffaa33;
        }
        
        .equals {
            background-color: #ff9500;
            color: white;
            grid-column: span 2;
        }
        
        .clear {
            background-color: #a6a6a6;
            color: black;
        }
        
        .clear:active {
            background-color: #b6b6b6;
        }
        
        .zero {
            grid-column: span 2;
        }
        
        @media (max-width: 480px) {
            .display {
                font-size: 2rem;
                padding: 20px 15px;
                min-height: 100px;
            }
            
            button {
                padding: 18px 8px;
                font-size: 1.3rem;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display" id="display">0</div>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button class="clear" onclick="deleteLast()">⌫</button>
            <button class="operator" onclick="appendToDisplay('/')">/</button>
            <button class="operator" onclick="appendToDisplay('*')">×</button>
            
            <button onclick="appendToDisplay('7')">7</button>
            <button onclick="appendToDisplay('8')">8</button>
            <button onclick="appendToDisplay('9')">9</button>
            <button class="operator" onclick="appendToDisplay('-')">-</button>
            
            <button onclick="appendToDisplay('4')">4</button>
            <button onclick="appendToDisplay('5')">5</button>
            <button onclick="appendToDisplay('6')">6</button>
            <button class="operator" onclick="appendToDisplay('+')">+</button>
            
            <button onclick="appendToDisplay('1')">1</button>
            <button onclick="appendToDisplay('2')">2</button>
            <button onclick="appendToDisplay('3')">3</button>
            <button class="equals" onclick="calculate()" rowspan="2">=</button>
            
            <button class="zero" onclick="appendToDisplay('0')">0</button>
            <button onclick="appendToDisplay('.')">.</button>
        </div>
    </div>

    <script>
        let display = document.getElementById('display');
        let currentInput = '0';
        let shouldResetDisplay = false;
        
        function updateDisplay() {
            display.textContent = currentInput;
        }
        
        function appendToDisplay(value) {
            if (currentInput === '0' || shouldResetDisplay) {
                currentInput = value;
                shouldResetDisplay = false;
            } else {
                currentInput += value;
            }
            updateDisplay();
        }
        
        function clearDisplay() {
            currentInput = '0';
            updateDisplay();
        }
        
        function deleteLast() {
            if (currentInput.length > 1) {
                currentInput = currentInput.slice(0, -1);
            } else {
                currentInput = '0';
            }
            updateDisplay();
        }
        
        function calculate() {
            try {
                // Заменяем символ × на * для вычислений
                let expression = currentInput.replace(/×/g, '*');
                let result = eval(expression);
                
                // Ограничиваем количество знаков после запятой
                if (Number.isFinite(result)) {
                    currentInput = parseFloat(result.toPrecision(12)).toString();
                } else {
                    currentInput = 'Ошибка';
                }
            } catch (error) {
                currentInput = 'Ошибка';
            }
            shouldResetDisplay = true;
            updateDisplay();
        }
        
        // Инициализация
        updateDisplay();
    </script>
</body>
</html>
