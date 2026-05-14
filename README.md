<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Simple Calculator</title>

  <style>
    body{
      display:flex;
      justify-content:center;
      align-items:center;
      height:100vh;
      background:#121212;
      font-family:Arial;
    }

    .calculator{
      background:#1e1e1e;
      padding:20px;
      border-radius:15px;
      box-shadow:0 0 15px rgba(0,0,0,0.5);
    }

    #display{
      width:100%;
      height:60px;
      margin-bottom:15px;
      font-size:28px;
      text-align:right;
      padding-right:10px;
      border:none;
      border-radius:10px;
      background:#2d2d2d;
      color:white;
    }

    .buttons{
      display:grid;
      grid-template-columns:repeat(4,70px);
      gap:10px;
    }

    button{
      height:60px;
      font-size:22px;
      border:none;
      border-radius:10px;
      cursor:pointer;
      background:#333;
      color:white;
      transition:0.2s;
    }

    button:hover{
      background:#555;
    }

    .equal{
      background:#ff9500;
    }

    .equal:hover{
      background:#e68900;
    }
  </style>
</head>
<body>

  <div class="calculator">
    <input type="text" id="display" disabled />

    <div class="buttons">
      <button onclick="clearDisplay()">C</button>
      <button onclick="append('/')">/</button>
      <button onclick="append('*')">*</button>
      <button onclick="append('-')">-</button>

      <button onclick="append('7')">7</button>
      <button onclick="append('8')">8</button>
      <button onclick="append('9')">9</button>
      <button onclick="append('+')">+</button>

      <button onclick="append('4')">4</button>
      <button onclick="append('5')">5</button>
      <button onclick="append('6')">6</button>
      <button onclick="calculate()" class="equal">=</button>

      <button onclick="append('1')">1</button>
      <button onclick="append('2')">2</button>
      <button onclick="append('3')">3</button>
      <button onclick="append('.')">.</button>

      <button onclick="append('0')" style="grid-column: span 4;">0</button>
    </div>
  </div>

  <script>
    let display = document.getElementById("display");

    function append(value){
      display.value += value;
    }

    function clearDisplay(){
      display.value = "";
    }

    function calculate(){
      try{
        display.value = eval(display.value);
      }
      catch{
        display.value = "Error";
      }
    }
  </script>

</body>
</html>
