# hangman-samisk
<!DOCTYPE html>
<html lang="no">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hangman Samegilli</title>
    
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            text-align: center;
            margin: 0;
            padding: 20px;
        }

        h1 {
            color: #333;
        }

        .gallows {
            font-size: 20px;
            margin-bottom: 20px;
            white-space: pre-line; /* For å vise line-breaks */
        }

        .word {
            font-size: 24px;
            letter-spacing: 5px;
        }

        input {
            padding: 10px;
            font-size: 16px;
            margin-top: 10px;
        }

        button {
            padding: 10px 20px;
            background-color: #007BFF;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        button:hover {
            background-color: #0056b3;
        }

        p {
            font-size: 18px;
        }
    </style>
</head>
<body>
    <h1>Spille Hangmana på samisk!</h1>
    
    <!-- Hangman galgen -->
    <div class="gallows" id="gallows"></div>
    
    <!-- Det hemmelige ordet -->
    <div class="word" id="word"></div>
    
    <p>Gjett en bokstav:</p>
    
    <!-- Input-felt for å gjette bokstaver -->
    <input type="text" id="letterInput" maxlength="1">
    <button onclick="guessLetter()">Gjett!</button>
    
    <p id="result"></p>

    <script>
        // Liste over ord spilleren kan gjette
        const ordListe = ["eadni", "sámegiella", "ihkku", "disdat", "beana", "guovdageaidnu", "divri"];
        let hemmeligOrd = ordListe[Math.floor(Math.random() * ordListe.length)];
        let korrekteGjetninger = Array(hemmeligOrd.length).fill("_");
        let gjettedeBokstaver = [];
        let antallForsok = 6;

        // Hangman stadier
        const galgeStadier = [
            `
               -----
               |   |
                   |
                   |
                   |
                   |
            _______|__
            `,
            `
               -----
               |   |
               O   |
                   |
                   |
                   |
            _______|__
            
               -----
               |   |
               O   |
               |   |
                   |
                   |
            _______|__
           
               -----
               |   |
               O   |
              /|   |
                   |
                   |
            _______|__
           
               -----
               |   |
               O   |
              /|\  |
                   |
                   |
            _______|__
            
               -----
               |   |
               O   |
              /|\  |
              /    |
                   |
            _______|__
          
               -----
               |   |
               O   |
              /|\  |
              / \  |
                   |
            _______|__
            `
        ];

        // Viser status for spillet
        function visSpillet() {
            document.getElementById("gallows").innerText = galgeStadier[6 - antallForsok];
            document.getElementById("word").innerText = korrekteGjetninger.join(" ");
            document.getElementById("result").innerText = `Du har ${antallForsok} forsøk igjen.`;
        }

        // Når spilleren gjetter en bokstav
        function guessLetter() {
            const letterInput = document.getElementById("letterInput").value.toLowerCase();
            document.getElementById("letterInput").value = ""; // Tøm input-feltet

            if (!letterInput || letterInput.length !== 1 || !/^[a-zæøå]+$/.test(letterInput)) {
                alert("Vennligst gjett en gyldig bokstav.");
                return;
            }

            if (gjettedeBokstaver.includes(letterInput)) {
                alert("Du har allerede gjettet denne bokstaven.");
                return;
            }

            gjettedeBokstaver.push(letterInput);

            if (hemmeligOrd.includes(letterInput)) {
                for (let i = 0; i < hemmeligOrd.length; i++) {
                    if (hemmeligOrd[i] === letterInput) {
                        korrekteGjetninger[i] = letterInput;
                    }
                }
            } else {
                antallForsok--;
            }

            visSpillet();
            sjekkVinner();
        }

        // Sjekk om spilleren har vunnet eller tapt
        function sjekkVinner() {
            if (!korrekteGjetninger.includes("_")) {
                document.getElementById("result").innerHTML = `<span style="color:green;">Riktig! Ordet var: ${hemmeligOrd}.</span>`;
                document.getElementById("letterInput").disabled = true;
            } else if (antallForsok === 0) {
                document.getElementById("result").innerHTML = `<span style="color:red;">Du tapte! Ordet var: ${hemmeligOrd}.</span>`;
                document.getElementById("letterInput").disabled = true;
            }
        }

        // Start spillet
        visSpillet();
    </script>
</body>
</html>
