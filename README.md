# Criador-de-senha <!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de Senhas Seguro</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #0f172a;
            color: #f8fafc;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            background-color: #1e293b;
            padding: 2rem;
            border-radius: 12px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
            width: 100%;
            max-width: 450px;
        }

        h1 {
            font-size: 1.8rem;
            margin-bottom: 1.5rem;
            text-align: center;
            color: #38bdf8;
        }

        .result-container {
            background-color: #0f172a;
            padding: 1rem;
            border-radius: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
            border: 1px solid #334155;
        }

        #password {
            font-family: 'Courier New', Courier, monospace;
            font-size: 1.2rem;
            font-weight: bold;
            letter-spacing: 1px;
            word-break: break-all;
            color: #e2e8f0;
        }

        .btn-copy {
            background-color: #38bdf8;
            border: none;
            color: #0f172a;
            padding: 0.5rem 1rem;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.2s;
        }

        .btn-copy:hover {
            background-color: #7dd3fc;
        }

        .option {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1rem;
        }

        label {
            color: #cbd5e1;
        }

        input[type="number"] {
            background-color: #0f172a;
            border: 1px solid #334155;
            color: #fff;
            padding: 0.4rem;
            border-radius: 6px;
            width: 60px;
            text-align: center;
        }

        input[type="checkbox"] {
            accent-color: #38bdf8;
            width: 18px;
            height: 18px;
        }

        .btn-generate {
            width: 100%;
            background-color: #10b981;
            color: white;
            border: none;
            padding: 1rem;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            margin-top: 1rem;
            transition: background 0.2s;
        }

        .btn-generate:hover {
            background-color: #059669;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Gerador de Senhas</h1>
        
        <div class="result-container">
            <span id="password">Clique em Gerar</span>
            <button class="btn-copy" id="clipboard">Copiar</button>
        </div>

        <div class="options">
            <div class="option">
                <label for="length">Comprimento da senha</label>
                <input type="number" id="length" min="4" max="32" value="12">
            </div>
            <div class="option">
                <label for="uppercase">Incluir Letras Maiúsculas</label>
                <input type="checkbox" id="uppercase" checked>
            </div>
            <div class="option">
                <label for="lowercase">Incluir Letras Minúsculas</label>
                <input type="checkbox" id="lowercase" checked>
            </div>
            <div class="option">
                <label for="numbers">Incluir Números</label>
                <input type="checkbox" id="numbers" checked>
            </div>
            <div class="option">
                <label for="symbols">Incluir Símbolos</label>
                <input type="checkbox" id="symbols" checked>
            </div>
        </div>

        <button class="btn-generate" id="generate">Gerar Senha</button>
    </div>

    <script>
        const passwordEl = document.getElementById('password');
        const lengthEl = document.getElementById('length');
        const uppercaseEl = document.getElementById('uppercase');
        const lowercaseEl = document.getElementById('lowercase');
        const numbersEl = document.getElementById('numbers');
        const symbolsEl = document.getElementById('symbols');
        const generateEl = document.getElementById('generate');
        const clipboardEl = document.getElementById('clipboard');

        const randomFunc = {
            lower: () => String.fromCharCode(Math.floor(Math.random() * 26) + 97),
            upper: () => String.fromCharCode(Math.floor(Math.random() * 26) + 65),
            number: () => String.fromCharCode(Math.floor(Math.random() * 10) + 48),
            symbol: () => {
                const symbols = '!@#$%^&*(){}[]=<>/,.';
                return symbols[Math.floor(Math.random() * symbols.length)];
            }
        };

        generateEl.addEventListener('click', () => {
            const length = +lengthEl.value;
            const hasLower = lowercaseEl.checked;
            const hasUpper = uppercaseEl.checked;
            const hasNumber = numbersEl.checked;
            const hasSymbol = symbolsEl.checked;

            passwordEl.innerText = generatePassword(hasLower, hasUpper, hasNumber, hasSymbol, length);
        });

        clipboardEl.addEventListener('click', () => {
            const password = passwordEl.innerText;
            if (!password || password === 'Clique em Gerar') return;
            
            navigator.clipboard.writeText(password);
            const originalText = clipboardEl.innerText;
            clipboardEl.innerText = 'Copiado!';
            setTimeout(() => clipboardEl.innerText = originalText, 2000);
        });

        function generatePassword(lower, upper, number, symbol, length) {
            let generatedPassword = '';
            const typesCount = lower + upper + number + symbol;
            const typesArr = [{ lower }, { upper }, { number }, { symbol }].filter(item => Object.values(item)[0]);

            if (typesCount === 0) return '';

            for (let i = 0; i < length; i += typesCount) {
                typesArr.forEach(type => {
                    const funcName = Object.keys(type)[0];
                    generatedPassword += randomFunc[funcName]();
                });
            }

            return generatedPassword.slice(0, length).split('').sort(() => Math.random() - 0.5).join('');
        }
    </script>
</body>
</html>
