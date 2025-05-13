<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1,user-scalable=no" />
<title>Calculadora de Conversão de Moedas</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');
  body {
    margin: 0;
    font-family: 'Poppins', sans-serif;
    background: #121212;
    color: #eee;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 600px;
    max-width: 350px;
    margin-left: auto;
    margin-right: auto;
    padding: 20px;
    user-select: none;
  }
  .container {
    background: #1f1f1f;
    border-radius: 16px;
    box-shadow: 0 6px 18px #00bfa5aa;
    padding: 24px;
    width: 100%;
    max-width: 350px;
  }
  h1 {
    font-weight: 600;
    font-size: 1.8rem;
    text-align: center;
    margin-bottom: 20px;
    color: #00bfa5;
  }
  label {
    display: block;
    font-weight: 600;
    margin: 12px 0 6px 0;
    font-size: 1rem;
    color: #00bfa5cc;
  }
  select, input[type="number"] {
    width: 100%;
    padding: 12px;
    font-size: 1.1rem;
    border: none;
    border-radius: 12px;
    background-color: #333;
    color: #eee;
    outline: none;
    box-sizing: border-box;
    transition: background-color 0.3s ease;
  }
  select:focus, input[type="number"]:focus {
    background-color: #444;
  }
  button {
    margin-top: 20px;
    width: 100%;
    background-color: #00bfa5;
    border: none;
    padding: 14px;
    font-weight: 700;
    font-size: 1.1rem;
    border-radius: 16px;
    color: #121212;
    cursor: pointer;
    transition: background-color 0.3s ease;
    user-select: none;
  }
  button:hover {
    background-color: #00afa0;
  }
  .result {
    margin-top: 20px;
    font-weight: 700;
    font-size: 1.3rem;
    text-align: center;
    padding: 16px;
    background: #00bfa51a;
    border-radius: 12px;
    color: #00bfa5;
    min-height: 48px;
    user-select: text;
  }
  @media (max-width: 380px) {
    body {
      padding: 10px;
    }
    .container {
      padding: 16px;
    }
  }
</style>
</head>
<body>
  <div class="container" role="main" aria-label="Calculadora de conversão de moedas">
    <h1>Conversor de Moedas</h1>
    <label for="inputAmount">Valor para converter</label>
    <input type="number" id="inputAmount" placeholder="Digite o valor" min="0" step="any" aria-describedby="inputAmountHelp" aria-required="true" />

    <label for="inputCurrency">De</label>
    <select id="inputCurrency" aria-required="true">
      <option value="USD">Dólar Americano (USD)</option>
      <option value="BRL">Real Brasileiro (BRL)</option>
      <option value="EUR">Euro (EUR)</option>
      <option value="JPY">Iene Japonês (JPY)</option>
      <option value="GBP">Libra Esterlina (GBP)</option>
      <option value="AUD">Dólar Australiano (AUD)</option>
      <option value="CAD">Dólar Canadense (CAD)</option>
      <option value="CHF">Franco Suíço (CHF)</option>
      <option value="CNY">Yuan Chinês (CNY)</option>
    </select>

    <label for="outputCurrency">Para</label>
    <select id="outputCurrency" aria-required="true">
      <option value="BRL">Real Brasileiro (BRL)</option>
      <option value="USD">Dólar Americano (USD)</option>
      <option value="EUR">Euro (EUR)</option>
      <option value="JPY">Iene Japonês (JPY)</option>
      <option value="GBP">Libra Esterlina (GBP)</option>
      <option value="AUD">Dólar Australiano (AUD)</option>
      <option value="CAD">Dólar Canadense (CAD)</option>
      <option value="CHF">Franco Suíço (CHF)</option>
      <option value="CNY">Yuan Chinês (CNY)</option>
    </select>

    <button id="convertBtn" aria-label="Converter moedas">Converter</button>

    <div class="result" aria-live="polite" aria-atomic="true" id="resultArea"></div>
  </div>
  <script>
    (function(){
      const rates = {
        USD: 1.00,            // base dólar americano
        BRL: 5.15,            // 1 USD = 5.15 BRL (exemplo)
        EUR: 0.92,
        JPY: 135.4,
        GBP: 0.78,
        AUD: 1.47,
        CAD: 1.36,
        CHF: 0.91,
        CNY: 7.24
      };

      const inputAmount = document.getElementById('inputAmount');
      const inputCurrency = document.getElementById('inputCurrency');
      const outputCurrency = document.getElementById('outputCurrency');
      const convertBtn = document.getElementById('convertBtn');
      const resultArea = document.getElementById('resultArea');

      function convertCurrency(amount, from, to){
        // Converter o valor para USD (base), depois para a moeda destino
        const amountInUSD = amount / rates[from];
        return amountInUSD * rates[to];
      }

      convertBtn.addEventListener('click', () => {
        const amount = parseFloat(inputAmount.value);
        if(isNaN(amount) || amount <= 0){
          resultArea.style.color = '#e74c3c';
          resultArea.textContent = 'Por favor, insira um valor válido maior que zero.';
          return;
        }
        const fromCur = inputCurrency.value;
        const toCur = outputCurrency.value;
        if(fromCur === toCur){
          resultArea.style.color = '#00bfa5';
          resultArea.textContent = `O valor é o mesmo: ${amount.toFixed(2)} ${toCur}`;
          return;
        }
        const converted = convertCurrency(amount, fromCur, toCur);
        const formatted = converted.toLocaleString('pt-BR', {minimumFractionDigits: 2, maximumFractionDigits: 2});
        resultArea.style.color = '#00bfa5';
        resultArea.textContent = `${amount.toFixed(2)} ${fromCur} = ${formatted} ${toCur}`;
      });

      // Também converter ao pressionar Enter no campo de valor
      inputAmount.addEventListener('keypress', (e) => {
        if(e.key === 'Enter'){
          convertBtn.click();
        }
      });
    })();
  </script>
</body>
</html>
