<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prodigies Chromacolor Piano (C4-G5)</title>
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
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 25px 60px rgba(0,0,0,0.9);
        }

        .key {
            cursor: pointer;
            user-select: none;
            transition: transform 0.05s, filter 0.05s;
        }

        .key:active {
            transform: translateY(3px);
            filter: brightness(0.8);
        }

        /* White Key Styles */
        .white {
            width: 65px;
            height: 280px;
            border: 1px solid rgba(0,0,0,0.3);
            border-radius: 0 0 8px 8px;
            z-index: 1;
        }

        /* Black Key Styles */
        .black {
            width: 44px;
            height: 170px;
            background-color: #222 !important; /* Force Black */
            z-index: 2;
            margin-left: -22px;
            margin-right: -22px;
            border-radius: 0 0 5px 5px;
            box-shadow: 3px 3px 6px rgba(0,0,0,0.6);
        }

        /* Prodigies Chromacolor Mapping (White Keys Only) */
        .white[data-note^="C"] { background-color: #FF0000; } /* Red */
        .white[data-note^="D"] { background-color: #FF8000; } /* Orange */
        .white[data-note^="E"] { background-color: #FFFF00; } /* Yellow */
        .white[data-note^="F"] { background-color: #00FF00; } /* Green */
        .white[data-note^="G"] { background-color: #008080; } /* Teal */
        .white[data-note^="A"] { background-color: #8B00FF; } /* Purple */
        .white[data-note^="B"] { background-color: #FF00FF; } /* Pink */

    </style>
</head>
<body>

    <div id="piano"></div>

    <script>
        const pianoContainer = document.getElementById('piano');
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

        // Specific range: C4 to E5
        const notes = [
            { n: 'C4', t: 'white', f: 261.63 }, { n: 'C#4', t: 'black', f: 277.18 },
            { n: 'D4', t: 'white', f: 293.66 }, { n: 'D#4', t: 'black', f: 311.13 },
            { n: 'E4', t: 'white', f: 329.63 },
            { n: 'F4', t: 'white', f: 349.23 }, { n: 'F#4', t: 'black', f: 369.99 },
            { n: 'G4', t: 'white', f: 392.00 }, { n: 'G#4', t: 'black', f: 415.30 },
            { n: 'A4', t: 'white', f: 440.00 }, { n: 'A#4', t: 'black', f: 466.16 },
            { n: 'B4', t: 'white', f: 493.88 },
            { n: 'C5', t: 'white', f: 523.25 }, { n: 'C#5', t: 'black', f: 554.37 },
            { n: 'D5', t: 'white', f: 587.33 }, { n: 'D#5', t: 'black', f: 622.25 },
            { n: 'E5', t: 'white', f: 659.25 },
          
        ];

        function playNote(frequency) {
            if (audioCtx.state === 'suspended') audioCtx.resume();
            
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            
            osc.type = 'triangle'; 
            osc.frequency.setValueAtTime(frequency, audioCtx.currentTime);
            
            gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + 1.2);

            osc.connect(gain);
            gain.connect(audioCtx.destination);

            osc.start();
            osc.stop(audioCtx.currentTime + 1.2);
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
