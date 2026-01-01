# Gta-san-andreas-informa-es 
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GTA San Andreas Hub - Grove Street</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        /* Fallback de cores caso o Tailwind falhe */
        :root { --gta-green: #22c55e; --gta-dark: #09090b; }
        body { background-color: var(--gta-dark); color: #f4f4f5; font-family: 'Inter', sans-serif; margin: 0; }
        .text-green-500 { color: var(--gta-green) !important; }
        .bg-green-600 { background-color: #16a34a !important; }
        .gta-font { font-style: italic; text-transform: uppercase; font-weight: 900; letter-spacing: -0.05em; }
        
        .tab-content { display: none; }
        .tab-content.active { display: block; animation: fadeIn 0.5s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        .sidebar { transition: transform 0.3s ease; background: #000; border-right: 1px solid rgba(22, 163, 74, 0.2); }
        @media (max-width: 768px) { .sidebar.closed { transform: translateX(-100%); } }
        
        .nav-btn { cursor: pointer; border: none; background: transparent; color: #71717a; width: 100%; text-align: left; transition: 0.2s; }
        .nav-btn.active { color: var(--gta-green); background: rgba(22, 163, 74, 0.1); border-right: 4px solid var(--gta-green); }
        
        .card-gta { background: #18181b; border: 1px solid #27272a; border-radius: 1.5rem; overflow: hidden; margin-bottom: 1rem; }
    </style>
</head>
<body>
    <nav id="sidebar" class="fixed inset-y-0 left-0 z-50 w-72 sidebar closed md:translate-x-0">
        <div class="p-10">
            <h1 class="text-4xl gta-font text-green-500">GTA <span class="text-white">SA</span></h1>
            <p class="text-[10px] uppercase tracking-widest text-zinc-500 font-bold">Grove Street Hub</p>
        </div>
        <div class="flex flex-col py-6">
            <button onclick="showTab('inicio', this)" class="nav-btn active flex items-center gap-4 px-10 py-5 text-xs font-black uppercase tracking-widest">
                <i data-lucide="info"></i> Início
            </button>
            <button onclick="showTab('personagens', this)" class="nav-btn flex items-center gap-4 px-10 py-5 text-xs font-black uppercase tracking-widest">
                <i data-lucide="users"></i> Personagens
            </button>
            <button onclick="showTab('cheats', this)" class="nav-btn flex items-center gap-4 px-10 py-5 text-xs font-black uppercase tracking-widest">
                <i data-lucide="shield-check"></i> Cheats
            </button>
            <button onclick="showTab('veiculos', this)" class="nav-btn flex items-center gap-4 px-10 py-5 text-xs font-black uppercase tracking-widest">
                <i data-lucide="car"></i> Veículos
            </button>
        </div>
    </nav>

    <button onclick="toggleMenu()" class="fixed bottom-6 right-6 z-[60] w-16 h-16 bg-green-600 rounded-full md:hidden flex items-center justify-center shadow-2xl text-white">
        <i data-lucide="menu"></i>
    </button>

    <main class="md:ml-72 p-6 md:p-16 max-w-6xl mx-auto min-h-screen">
        <section id="inicio" class="tab-content active">
            <h2 class="text-6xl gta-font text-green-500 mb-8">Início</h2>
            <div class="relative bg-zinc-900 rounded-[2rem] overflow-hidden min-h-[400px] flex flex-col justify-end p-8 border border-zinc-800">
                <img src="https://images.alphacoders.com/441/441614.jpg" class="absolute inset-0 w-full h-full object-cover opacity-40">
                <div class="relative z-10">
                    <h3 class="text-4xl gta-font text-green-500">RESPEITO É TUDO</h3>
                    <p class="text-zinc-300 mt-2 max-w-md">Bem-vindo ao portal definitivo da Grove Street. O código funciona e o estilo é verde!</p>
                </div>
            </div>
        </section>

        <section id="personagens" class="tab-content">
            <h2 class="text-5xl gta-font text-green-500 mb-8">Personagens</h2>
            <div id="chars-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6"></div>
        </section>

        <section id="cheats" class="tab-content">
            <h2 class="text-5xl gta-font text-green-500 mb-8">Cheats</h2>
            <input type="text" oninput="filterCheats(this.value)" placeholder="Procurar código..." class="w-full bg-zinc-900 border-2 border-zinc-800 rounded-2xl p-6 mb-8 text-white outline-none focus:border-green-600 font-bold">
            <div id="cheats-grid" class="grid grid-cols-1 sm:grid-cols-2 gap-4"></div>
        </section>

        <section id="veiculos" class="tab-content">
            <h2 class="text-5xl gta-font text-green-500 mb-8">Veículos</h2>
            <div id="vehicles-list" class="space-y-4"></div>
        </section>
    </main>

    <script>
        const characters = [{"id":"cj","name":"Carl Johnson (CJ)","role":"Protagonista","description":"O herói relutante que volta para salvar sua família e sua vizinhança.","personality":"Leal à Grove Street, determinado e o personagem mais versátil da franquia.","image":"https://oyster.ignimgs.com/mediawiki/apis.ign.com/grand-theft-auto-san-andreas/d/d0/Cj.jpg"},{"id":"big-smoke","name":"Melvin \"Big Smoke\" Harris","role":"Antagonista","description":"Um dos membros mais graduados da Grove Street que esconde segredos sombrios.","personality":"Carismático, ambicioso e famoso por seu apetite lendário.","image":"https://oyster.ignimgs.com/mediawiki/apis.ign.com/grand-theft-auto-san-andreas/4/43/Bigsmoke.jpg"},{"id":"sweet","name":"Sean \"Sweet\" Johnson","role":"Líder da Grove Street","description":"Irmão mais velho de CJ e o coração moral da gangue Grove Street Families.","personality":"Rígido, focado na lealdade e orgulhoso de suas raízes.","image":"https://oyster.ignimgs.com/mediawiki/apis.ign.com/grand-theft-auto-san-andreas/7/70/Sweet.jpg"},{"id":"ryder","name":"Lance \"Ryder\" Wilson","role":"Antagonista","description":"Um gênio incompreendido (em sua própria mente) e amigo de infância de CJ.","personality":"Imprevisível, viciado em substâncias e sempre pronto para um assalto.","image":"https://oyster.ignimgs.com/mediawiki/apis.ign.com/grand-theft-auto-san-andreas/a/a5/Ryder.jpg"},{"id":"tenpenny","name":"Frank Tenpenny","role":"Vilão Principal","description":"Oficial corrupto da C.R.A.S.H. que controla Los Santos através do medo.","personality":"Sociopata, manipulador e uma das figuras mais odiadas do estado.","image":"https://oyster.ignimgs.com/mediawiki/apis.ign.com/grand-theft-auto-san-andreas/c/c5/Tenpenny.jpg"},{"id":"cesar","name":"Cesar Vialpando","role":"Aliado Leal","description":"Líder dos Varrios Los Aztecas e o melhor amigo que CJ faz fora de sua gangue.","personality":"Honrado, apaixonado por carros e extremamente protetor de Kendl.","image":"https://oyster.ignimgs.com/mediawiki/apis.ign.com/grand-theft-auto-san-andreas/4/4c/Cesar.jpg"},{"id":"catalina","name":"Catalina","role":"Antagonista/Ex-Namorada","description":"A prima explosiva de Cesar que leva CJ em uma onda de crimes pelo campo.","personality":"Violenta, instável e completamente insana.","image":"https://oyster.ignimgs.com/mediawiki/apis.ign.com/grand-theft-auto-san-andreas/1/1d/Catalina.jpg"},{"id":"truth","name":"The Truth","role":"Mentor Excêntrico","description":"O hippie paranoico que introduz CJ aos segredos do governo no deserto.","personality":"Pacifista (quase sempre), teórico da conspiração e mestre do disfarce.","image":"https://oyster.ignimgs.com/mediawiki/apis.ign.com/grand-theft-auto-san-andreas/f/f3/Thetruth.jpg"}];
        const cheats = [{"code":"HESOYAM","effect":"Vida, Colete e $250.000","category":"Vida"},{"code":"BAGUVIX","effect":"Saúde Infinita (exceto explosões)","category":"Vida"},{"code":"CVWKXAM","effect":"Oxigênio Infinito (mergulho)","category":"Vida"},{"code":"AEDUWNV","effect":"Nunca sentir fome","category":"Vida"},{"code":"BTCDBCB","effect":"CJ Gordo","category":"Vida"},{"code":"JYSDSOD","effect":"Músculos no Máximo","category":"Vida"},{"code":"KVGYZQK","effect":"CJ Magrelo","category":"Vida"},{"code":"OGXSDAG","effect":"Respeito no Máximo","category":"Vida"},{"code":"EHIBXQS","effect":"Sex Appeal no Máximo","category":"Vida"},{"code":"LXGIWYL","effect":"Pacote de Armas 1 (Thugs Tools)","category":"Armas"},{"code":"KJKSZPJ","effect":"Pacote de Armas 2 (Professional Tools)","category":"Armas"},{"code":"UZUMYMW","effect":"Pacote de Armas 3 (Nutter Tools)","category":"Armas"},{"code":"WANRLTW","effect":"Munição Infinita","category":"Armas"},{"code":"NCSGDAG","effect":"Nível Hitman em todas as armas","category":"Armas"},{"code":"ASNAEB","effect":"Limpar Nível de Procurado","category":"Polícia"},{"code":"AEZAKMI","effect":"Nunca ser procurado","category":"Polícia"},{"code":"OSRBLHH","effect":"Aumentar 2 estrelas","category":"Polícia"},{"code":"LJSPQK","effect":"Nível de Procurado 6 estrelas","category":"Polícia"},{"code":"ROCKETMAN","effect":"Jetpack","category":"Veículos"},{"code":"AIWPRTON","effect":"Tanque Rhino","category":"Veículos"},{"code":"CQZIJMB","effect":"Bloodring Banger","category":"Veículos"},{"code":"JQNTDMH","effect":"Rancher","category":"Veículos"},{"code":"PDNEJOH","effect":"Racecar","category":"Veículos"},{"code":"VPJTQWV","effect":"Racecar 2","category":"Veículos"},{"code":"AQTBCODX","effect":"Romero (Carro funerário)","category":"Veículos"},{"code":"KRIJEBR","effect":"Stretch (Limusine)","category":"Veículos"},{"code":"UBHYZHQ","effect":"Trashmaster (Caminhão de lixo)","category":"Veículos"},{"code":"RZHSUEW","effect":"Caddy (Carrinho de golfe)","category":"Veículos"},{"code":"AKJJYGLC","effect":"Quadriciclo","category":"Veículos"},{"code":"EEGCYXT","effect":"Dozer (Escavadeira)","category":"Veículos"},{"code":"URKQSRK","effect":"Stunt Plane","category":"Veículos"},{"code":"AGBDLCID","effect":"Monster Truck","category":"Veículos"},{"code":"OHDUDE","effect":"Hunter (Helicóptero)","category":"Veículos"},{"code":"JUMPJET","effect":"Hydra (Jato)","category":"Veículos"},{"code":"KGGGDKP","effect":"Vortex (Hovercraft)","category":"Veículos"},{"code":"RIPAZHA","effect":"Carros voadores","category":"Veículos"},{"code":"CPKTNWT","effect":"Explodir todos os carros","category":"Veículos"},{"code":"XICWMW","effect":"Carro invisível","category":"Veículos"},{"code":"PGGOMOY","effect":"Direção perfeita","category":"Veículos"},{"code":"ZEIVGWC","effect":"Sinal verde sempre","category":"Veículos"},{"code":"LLQPFBN","effect":"Tráfego rosa","category":"Veículos"},{"code":"IOWDLAC","effect":"Tráfego preto","category":"Veículos"},{"code":"AFSNMSMW","effect":"Barcos voadores","category":"Veículos"},{"code":"AFZLLQLL","effect":"Céu limpo","category":"Clima"},{"code":"ICIKPYH","effect":"Tempo muito ensolarado","category":"Clima"},{"code":"ALNSFMZO","effect":"Tempo nublado","category":"Clima"},{"code":"AUIFRVQS","effect":"Tempo chuvoso","category":"Clima"},{"code":"CFVFGMJ","effect":"Tempo com neblina","category":"Clima"},{"code":"YSOHNUL","effect":"Tempo passa mais rápido","category":"Clima"},{"code":"PPGWJHT","effect":"Gameplay acelerada","category":"Clima"},{"code":"LIYOAAY","effect":"Gameplay em câmera lenta","category":"Clima"},{"code":"OFVIAC","effect":"Céu laranja (21:00 fixo)","category":"Clima"},{"code":"XJVSNAJ","effect":"Sempre meia-noite","category":"Clima"},{"code":"AJLOJYQY","effect":"Pedestres se atacam com tacos de golfe","category":"Clima"},{"code":"BAGOWPG","effect":"Caça à recompensa (pedestres te atacam)","category":"Clima"},{"code":"FOOOXFT","effect":"Todos os pedestres armados","category":"Clima"},{"code":"CIKGCGX","effect":"Festa na praia","category":"Clima"},{"code":"BEKKNQV","effect":"Imã de prostitutas","category":"Clima"},{"code":"IOJUFZN","effect":"Modo Revolta (Riots)","category":"Clima"}];
        const vehicles = [{"name":"Infernus","category":"Carro","speed":"Muito Alta","curiosity":"O carro mais rápido do jogo."},{"name":"NRG-500","category":"Moto","speed":"Extrema","curiosity":"A moto de corrida definitiva."},{"name":"Hydra","category":"Avião","speed":"Super Sônica","curiosity":"Jato de combate com decolagem vertical."},{"name":"Hunter","category":"Helicóptero","speed":"Alta","curiosity":"Helicóptero de ataque militar."},{"name":"Squalo","category":"Barco","speed":"Média/Alta","curiosity":"Lancha esportiva veloz."}];

        function showTab(id, btn) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            btn.classList.add('active');
            if(window.innerWidth < 768) toggleMenu();
        }

        function toggleMenu() {
            document.getElementById('sidebar').classList.toggle('closed');
        }

        const charGrid = document.getElementById('chars-grid');
        characters.forEach(c => {
            charGrid.innerHTML += `
                <div class="card-gta">
                    <img src="${c.image}" class="w-full h-48 object-cover">
                    <div class="p-6">
                        <h4 class="text-green-500 gta-font text-2xl">${c.name}</h4>
                        <p class="text-zinc-500 text-[10px] font-bold uppercase tracking-widest mb-2">${c.role}</p>
                        <p class="text-zinc-400 text-sm">${c.description}</p>
                    </div>
                </div>
            `;
        });

        function renderCheats(list) {
            const grid = document.getElementById('cheats-grid');
            grid.innerHTML = '';
            list.forEach(c => {
                grid.innerHTML += `
                    <div class="bg-zinc-900 p-6 rounded-2xl border border-zinc-800">
                        <p class="font-bold text-zinc-100 mb-3">${c.effect}</p>
                        <div class="bg-black p-4 rounded-xl text-green-500 gta-font text-2xl text-center border border-green-900/20">${c.code}</div>
                    </div>
                `;
            });
        }
        renderCheats(cheats);

        function filterCheats(val) {
            const f = cheats.filter(c => c.code.toLowerCase().includes(val.toLowerCase()) || c.effect.toLowerCase().includes(val.toLowerCase()));
            renderCheats(f);
        }

        const vList = document.getElementById('vehicles-list');
        vehicles.forEach(v => {
            vList.innerHTML += `
                <div class="bg-zinc-900 p-6 rounded-2xl border border-zinc-800 flex justify-between items-center">
                    <div>
                        <h4 class="text-xl gta-font text-white">${v.name}</h4>
                        <p class="text-zinc-500 text-xs">${v.category} • ${v.curiosity}</p>
                    </div>
                    <div class="text-green-500 font-black bg-black px-4 py-2 rounded-full border border-green-900/20">${v.speed}</div>
                </div>
            `;
        });

        lucide.createIcons();
    </script>
</body>
</html>
