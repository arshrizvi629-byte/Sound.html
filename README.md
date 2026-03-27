<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Viral Soundboard</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Exo+2:wght@300;400;600&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Exo 2', sans-serif;
            background: #0a0a0a;
            min-height: 100vh;
            padding: 20px;
            overflow-x: hidden;
            position: relative;
        }

        /* Animated background particles */
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                radial-gradient(circle at 20% 80%, rgba(120,119,198,0.3) 0%, transparent 50%),
                radial-gradient(circle at 80% 20%, rgba(255,119,198,0.3) 0%, transparent 50%),
                radial-gradient(circle at 40% 40%, rgba(120,219,255,0.2) 0%, transparent 50%);
            animation: bgShift 20s ease-in-out infinite;
            z-index: -2;
        }

        @keyframes bgShift {
            0%, 100% { transform: scale(1) rotate(0deg); }
            50% { transform: scale(1.1) rotate(180deg); }
        }

        /* Floating particles */
        .particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }

        .particle {
            position: absolute;
            background: linear-gradient(45deg, #00f5ff, #ff00ff);
            border-radius: 50%;
            animation: float 6s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px) rotate(0deg); opacity: 0.6; }
            50% { transform: translateY(-20px) rotate(180deg); opacity: 1; }
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }

        h1 {
            text-align: center;
            background: linear-gradient(45deg, #00f5ff, #ff00ff, #00ff88, #ffaa00);
            background-size: 300% 300%;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            font-family: 'Orbitron', monospace;
            font-size: 4rem;
            font-weight: 900;
            margin-bottom: 40px;
            text-shadow: 0 0 30px rgba(0, 245, 255, 0.5);
            animation: titleGlow 3s ease-in-out infinite alternate;
            letter-spacing: 5px;
        }

        @keyframes titleGlow {
            from { filter: drop-shadow(0 0 20px #00f5ff); }
            to { filter: drop-shadow(0 0 40px #ff00ff); }
        }

        .search-box {
            width: 100%;
            max-width: 500px;
            margin: 0 auto 40px;
            display: block;
            position: relative;
        }

        .search-box input {
            width: 100%;
            padding: 20px 25px;
            border: 2px solid transparent;
            border-radius: 50px;
            font-size: 1.1rem;
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(20px);
            color: #fff;
            outline: none;
            transition: all 0.3s ease;
            font-family: 'Exo 2', sans-serif;
        }

        .search-box input::placeholder {
            color: rgba(255, 255, 255, 0.6);
        }

        .search-box input:focus {
            border-color: #00f5ff;
            box-shadow: 0 0 30px rgba(0, 245, 255, 0.4);
            background: rgba(255, 255, 255, 0.1);
        }

        .search-box::before {
            content: '🔍';
            position: absolute;
            right: 20px;
            top: 50%;
            transform: translateY(-50%);
            color: #00f5ff;
            font-size: 1.2rem;
        }

        .category-filter {
            text-align: center;
            margin-bottom: 40px;
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 15px;
        }

        .category-btn {
            background: rgba(255,255,255,0.05);
            backdrop-filter: blur(10px);
            border: 2px solid rgba(0, 245, 255, 0.3);
            border-radius: 25px;
            padding: 12px 25px;
            color: #fff;
            cursor: pointer;
            transition: all 0.3s ease;
            font-family: 'Orbitron', monospace;
            font-weight: 600;
            position: relative;
            overflow: hidden;
        }

        .category-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(0, 245, 255, 0.4), transparent);
            transition: left 0.5s;
        }

        .category-btn:hover::before {
            left: 100%;
        }

        .category-btn.active, .category-btn:hover {
            background: linear-gradient(45deg, #00f5ff, #0099ff);
            border-color: #00f5ff;
            box-shadow: 0 0 25px rgba(0, 245, 255, 0.6);
            transform: translateY(-3px);
        }

        .sound-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 25px;
            padding: 20px 0;
        }

        .sound-btn {
            background: linear-gradient(145deg, rgba(255,255,255,0.1), rgba(255,255,255,0.05));
            backdrop-filter: blur(20px);
            border: 2px solid rgba(0, 245, 255, 0.3);
            border-radius: 25px;
            padding: 25px;
            font-size: 1.1rem;
            font-weight: 600;
            color: #fff;
            cursor: pointer;
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            position: relative;
            overflow: hidden;
            font-family: 'Exo 2', sans-serif;
            text-align: center;
        }

        .sound-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
            transition: left 0.6s;
        }

        .sound-btn:hover::before {
            left: 100%;
        }

        .sound-btn:hover {
            transform: translateY(-10px) scale(1.05);
            border-color: #00f5ff;
            box-shadow: 
                0 20px 40px rgba(0, 245, 255, 0.3),
                0 0 40px rgba(0, 245, 255, 0.2);
        }

        .sound-btn:active {
            transform: translateY(-5px) scale(1.03);
        }

        .sound-btn.playing {
            background: linear-gradient(45deg, #00f5ff, #ff00ff);
            border-color: #ff00ff;
            animation: futuristicPulse 0.8s infinite;
            box-shadow: 
                0 0 50px rgba(0, 245, 255, 0.8),
                0 0 100px rgba(255, 0, 255, 0.6),
                inset 0 0 20px rgba(255, 255, 255, 0.3);
        }

        @keyframes futuristicPulse {
            0% { 
                transform: scale(1) rotate(0deg);
                box-shadow: 
                    0 0 50px rgba(0, 245, 255, 0.8),
                    0 0 100px rgba(255, 0, 255, 0.6);
            }
            50% { 
                transform: scale(1.1) rotate(5deg);
                box-shadow: 
                    0 0 70px rgba(0, 245, 255, 1),
                    0 0 140px rgba(255, 0, 255, 0.8);
            }
            100% { 
                transform: scale(1) rotate(0deg);
                box-shadow: 
                    0 0 50px rgba(0, 245, 255, 0.8),
                    0 0 100px rgba(255, 0, 255, 0.6);
            }
        }

        /* Loading spinner */
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(0, 245, 255, 0.3);
            border-radius: 50%;
            border-top-color: #00f5ff;
            animation: spin 1s ease-in-out infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        @media (max-width: 768px) {
            .sound-grid {
                grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
                gap: 20px;
            }
            
            h1 {
                font-size: 2.5rem;
                letter-spacing: 2px;
            }

            .category-filter {
                flex-direction: column;
                align-items: center;
            }
        }

        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }

        ::-webkit-scrollbar-track {
            background: rgba(255,255,255,0.05);
        }

        ::-webkit-scrollbar-thumb {
            background: linear-gradient(#00f5ff, #ff00ff);
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <div class="particles" id="particles"></div>
    
    <div class="container">
        <h1>🔮 VIRAL SOUNDBOARD 🔮</h1>
        
        <div class="search-box">
            <input type="text" id="searchInput" placeholder="Search viral sounds...">
        </div>

        <div class="category-filter">
            <button class="category-btn active" data-category="all">ALL</button>
            <button class="category-btn" data-category="memes">MEMES</button>
            <button class="category-btn" data-category="tiktok">TIKTOK</button>
            <button class="category-btn" data-category="funny">FUNNY</button>
            <button class="category-btn" data-category="effects">EFFECTS</button>
        </div>

        <div class="sound-grid" id="soundGrid">
            <!-- Sounds will be populated by JavaScript -->
        </div>
    </div>

    <script>
        // Create floating particles
        function createParticles() {
            const particlesContainer = document.getElementById('particles');
            for (let i = 0; i < 20; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.left = Math.random() * 100 + '%';
                particle.style.top = Math.random() * 100 + '%';
                particle.style.width = particle.style.height = (Math.random() * 6 + 2) + 'px';
                particle.style.animationDelay = Math.random() * 6 + 's';
                particle.style.animationDuration = (Math.random() * 3 + 4) + 's';
                particlesContainer.appendChild(particle);
            }
        }

        const sounds = [
            { name: "Ohio 💀", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "memes" },
            { name: "Skibidi 🚽", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "tiktok" },
            { name: "Rizzler 😎", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "memes" },
            { name: "GYATT 💥", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "tiktok" },
            { name: "Fanum Tax 🤑", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "memes" },
            { name: "Sigma 🗿", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "memes" },
            { name: "Mewing 😤", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "tiktok" },
            { name: "Airhorn 📣", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "effects" },
            { name: "Dramatic Chipmunk 🐿️", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "funny" },
            { name: "Vine Boom 💣", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "memes" },
            { name: "Sad Affleck 😢", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "memes" },
            { name: "Cash Register 🤑", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "effects" },
            { name: "Phone Whoosh 📱", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "tiktok" },
            { name: "Cartoon Boing 🪙", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "funny" },
            { name: "Epic Fail 😭", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "funny" },
            { name: "Level Up 🎮", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "effects" },
            { name: "Troll Face 😂", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "memes" },
            { name: "Dolphin Squeak 🐬", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "funny" },
            { name: "Magic Whoosh ✨", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "effects" },
            { name: "Perfect 💯", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "tiktok" },
            { name: "No Cap 🙅‍♂️", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "memes" },
            { name: "Bet 🤝", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "tiktok" },
            { name: "Sus 😳", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "memes" },
            { name: "Yeet 🚀", file: "https://www.soundjay.com/misc/sounds/bell-ringing-05.wav", category: "funny" }
        ];

        let currentAudio = null;
        let allButtons = [];

        function createSoundButtons() {
            const grid = document.getElementById('soundGrid');
            grid.innerHTML = '';

            sounds.forEach((sound, index) => {
                const btn = document.createElement('button');
                btn.className = 'sound-btn';
                btn.dataset.sound = index;
                btn.dataset.category = sound.category;
                btn.innerHTML = sound.name;
                btn.onclick = () => playSound(sound, btn);
                grid.appendChild(btn);
                allButtons.push(btn);
            });
        }

        function playSound(sound, button) {
            if (currentAudio) {
                currentAudio.pause();
                currentAudio.currentTime = 0;
            }

            allButtons.forEach(btn => btn.classList.remove('playing'));

            currentAudio = new Audio(sound.file);
            currentAudio.volume = 0.7;
            currentAudio.play();

            button.classList.add('playing');

            currentAudio.onended = () => {
                button.classList.remove('playing');
            };
        }

        // Search functionality
        document.getElementById('searchInput').addEventListener('input', (e) => {
            const query = e.target.value.toLowerCase();
            allButtons.forEach(btn => {
                const name = btn.textContent.toLowerCase();
                btn.style.display = name.includes(query) ? 'block' : 'none';
            });
        });

        // Category filter
        document.querySelectorAll('.category-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.category
