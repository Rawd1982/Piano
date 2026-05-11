<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prodigies Chromacolor Piano</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #1a1a1a;
            margin: 0;
            font-family: sans-serif;
        }

        #piano {
            display: flex;
            position: relative;
            background: #000;
            padding: 10px;
            border-radius: 8px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.8);
        }

        .key {
            cursor: pointer;
            user-select: none;
            transition: transform 0.05s, box-shadow 0.05s;
        }

        .key:active {
            transform: translateY(2px);
            filter: brightness(0.9);
        }

        /* White Key Styles */
        .white {
            width: 60px;
            height: 250px;
            border: 1px solid rgba(0,0,0,0.2);
            border-radius: 0 0 6px 6px;
            z-index: 1;
        }

        /* Black Key Styles */
        .black {
            width: 40px;
            height: 150px;
            background-color: #222;
            z-index: 2;
            margin-left: -20px;
            margin-right: -20px;
            border-radius: 0 0 4px 4px;
            box-shadow: 2px 2px 5px rgba(0,0,0,0.5);
        }

        /* Prodigies Chromacolor Mapping */
        .key[data-note^="C"] { background-color: #FF0000; } /* Red */
        .key[data-note^="D"] { background-color: #FF8000; } /* Orange */
        .key[data-note^="E"] { background-color: #FFFF00; } /* Yellow */
        .key[data-note^="F"] { background-color: #00FF00; } /* Green */
        .key[data-note^="G"] { background-color: #0000FF; } /* Blue */
        .key[data-note^="A"] { background-color: #8B00FF; } /* Purple */
        .key[data-note^="B"] { background-color: #FF00FF; } /* Pink */

        /* Override Black Keys to stay black regardless of note letter */
        .key.black { background-color: #222 !important; }

    </style>
</head>
<body>

    <div id="piano"></div>

    <script>
        const pianoContainer = document.getElementById('piano');
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

        // Sequence from A3 to E5
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
            
            osc.type = 'triangle'; 
            osc.frequency.setValueAtTime(frequency, audioCtx.currentTime);
            
            gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + 1.5);

            osc.connect(gain);
            gain.connect(audioCtx.destination);

            osc.start();
            osc.stop(audioCtx.currentTime + 1.5);
        }

        notes.forEach(note => {
            const key = document.createElement('div');
            key.className = `key ${note.t}`;
            key.dataset.note = note.n;

            const handlePress = (e) => {
                e.preventDefault();
                playNote(note.f);
            };

            key.addEventListener('mousedown', handlePress);
            key.addEventListener('touchstart', handlePress);

            pianoContainer.appendChild(key);
        });
    </script>
</body>
</html>
