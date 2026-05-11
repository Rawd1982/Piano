<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prodigies Chromacolor Piano (A3-E5)</title>
    <style>
        /* CSS - Styling the Piano */
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #2c3e50;
            margin: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            color: white;
        }

        h1 { margin-bottom: 20px; }

        #piano {
            display: flex;
            position: relative;
            background: #111;
            padding: 10px;
            border-radius: 10px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
        }

        .key {
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: flex-end;
            padding-bottom: 15px;
            user-select: none;
            transition: transform 0.1s, box-shadow 0.1s;
        }

        .key:active {
            transform: translateY(4px);
            box-shadow: inset 0 5px 10px rgba(0,0,0,0.3);
        }

        /* White Key Styles */
        .white {
            width: 60px;
            height: 250px;
            border: 1px solid #000;
            border-radius: 0 0 5px 5px;
            z-index: 1;
            font-size: 1.2rem;
            font-weight: bold;
        }

        /* Black Key Styles */
        .black {
            width: 40px;
            height: 150px;
            background-color: #333;
            z-index: 2;
            margin-left: -20px;
            margin-right: -20px;
            border-radius: 0 0 3px 3px;
            color: #ccc;
            font-size: 0.8rem;
        }

        /* Prodigies Chromacolor Mapping for White Keys */
        .key[data-note^="C"] { background-color: #FF0000; color: white; } /* Red */
        .key[data-note^="D"] { background-color: #FF8000; color: white; } /* Orange */
        .key[data-note^="E"] { background-color: #FFFF00; color: black; } /* Yellow */
        .key[data-note^="F"] { background-color: #00FF00; color: black; } /* Green */
        .key[data-note^="G"] { background-color: #0000FF; color: white; } /* Blue */
        .key[data-note^="A"] { background-color: #8B00FF; color: white; } /* Purple */
        .key[data-note^="B"] { background-color: #FF00FF; color: white; } /* Pink/Teal */

    </style>
</head>
<body>

    <h1>Prodigies Piano</h1>
    <div id="piano"></div>
    <p>Click the keys to play (A3 to E5)</p>

    <script>
        // JS - Logic and Audio
        const pianoContainer = document.getElementById('piano');
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

        const notes = [
            { n: 'A3', t: 'white', f: 220.00 }, { n: 'A#3', t: 'black', f: 233.08 },
            { n: 'B3', t: 'white', f: 246.94 }, { n: 'C4', t: 'white', f: 261.63 },
            { n: 'C#4', t: 'black', f: 277.18 }, { n: 'D4', t: 'white', f: 293.66 },
            { n: 'D#4', t: 'black', f: 311.13 }, { n: 'E4', t: 'white', f: 329.63 },
            { n: 'F4', t: 'white', f: 349.23 }, { n: 'F#4', t: 'black', f: 369.99 },
            { n: 'G4', t: 'white', f: 392.00 }, { n: 'G#4', t: 'black', f: 415.30 },
            { n: 'A4', t: 'white', f: 440.00 }, { n: 'A#4', t: 'black', f: 466.16 },
            { n: 'B4', t: 'white', f: 493.88 }, { n: 'C5', t: 'white', f: 523.25 },
            { n: 'C#5', t: 'black', f: 554.37 }, { n: 'D5', t: 'white', f: 587.33 },
            { n: 'D#5', t: 'black', f: 622.25 }, { n: 'E5', t: 'white', f: 659.25 }
        ];

        function playNote(frequency) {
            if (audioCtx.state === 'suspended') audioCtx.resume();
            
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            
            osc.type = 'triangle'; // Smoother sound than sine
            osc.frequency.setValueAtTime(frequency, audioCtx.currentTime);
            
            gain.gain.setValueAtTime(0.4, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + 1);

            osc.connect(gain);
            gain.connect(audioCtx.destination);

            osc.start();
            osc.stop(audioCtx.currentTime + 1);
        }

        notes.forEach(note => {
            const key = document.createElement('div');
            key.className = `key ${note.t}`;
            key.dataset.note = note.n;
            if (note.t === 'white') key.innerText = note.n;

            key.addEventListener('mousedown', () => playNote(note.f));
            // Add touch support for mobile
            key.addEventListener('touchstart', (e) => {
                e.preventDefault();
                playNote(note.f);
            });

            pianoContainer.appendChild(key);
        });
    </script>
</body>
</html>
