<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Desafio 30 Dias - Compromisso</title>
    
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Montserrat:wght@600;700&family=Courier+Prime&display=swap" rel="stylesheet">

    <style>
        :root {
            --primary: #6DD5A9;
            --primary-dark: #0C4437;
            --blue-save: #42A5F5;
            --neutral-bg: #F7F8FA;
            --white: #FFFFFF;
            --alert: #E57373;
            --shadow: 0px 4px 20px rgba(0,0,0,0.05);
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', sans-serif; -webkit-tap-highlight-color: transparent; }
        
        body { background-color: var(--neutral-bg); height: 100vh; display: flex; justify-content: center; color: #333; }
        
        #app-container {
            width: 100%; max-width: 414px; background: var(--white); height: 100%;
            position: relative; overflow-y: auto; display: flex; flex-direction: column;
            box-shadow: 0 0 50px rgba(0,0,0,0.1);
        }

        /* Typography & Components */
        h1, h2, h3 { font-family: 'Montserrat', sans-serif; color: var(--primary-dark); }
        h1 { font-size: 22px; margin-bottom: 5px; line-height: 1.2; }
        p { font-size: 14px; color: #666; line-height: 1.5; margin-bottom: 15px; }

        .btn {
            background: var(--primary); color: var(--primary-dark); width: 100%; height: 50px;
            border: none; border-radius: 12px; font-weight: 700; font-size: 16px; cursor: pointer;
            display: flex; align-items: center; justify-content: center; gap: 8px;
            box-shadow: 0 4px 10px rgba(109, 213, 169, 0.3); transition: 0.2s;
        }
        .btn:active { transform: scale(0.97); }
        .btn.secondary { background: #EFFFF8; border: 1px solid var(--primary); box-shadow: none; }
        .btn:disabled { background: #ccc; cursor: not-allowed; box-shadow: none; color: #666; }

        .card { background: white; border: 1px solid #eee; border-radius: 16px; padding: 16px; margin-bottom: 12px; box-shadow: var(--shadow); }
        input { width: 100%; height: 48px; border: 1px solid #ddd; border-radius: 10px; padding: 0 15px; font-size: 16px; margin-bottom: 15px; }

        /* Screens Transition */
        .screen { display: none; flex-direction: column; padding: 24px; height: 100%; animation: fade 0.4s; overflow-y: auto; padding-bottom: 90px; }
        .screen.active { display: flex; }
        @keyframes fade { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* --- ESTILOS DO ONBOARDING (NOVO) --- */
        .villain-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 20px; }
        .villain-card {
            border: 1px solid #ddd; border-radius: 12px; padding: 15px; text-align: center; cursor: pointer; transition: 0.2s;
        }
        .villain-card.selected {
            border-color: var(--primary); background: #EFFFF8; font-weight: bold; transform: scale(1.02); box-shadow: 0 4px 12px rgba(109, 213, 169, 0.2);
        }
        .villain-icon { font-size: 24px; margin-bottom: 5px; display: block; }

        /* Estilo do Contrato Digital */
        .contract-paper {
            background: #FFFDF0;
            border: 2px dashed #D4C4A8;
            padding: 24px;
            border-radius: 8px;
            margin: 20px 0;
            font-family: 'Courier Prime', monospace;
            font-size: 13px;
            line-height: 1.6;
            color: #444;
            position: relative;
        }
        .contract-paper strong { color: #000; border-bottom: 1px solid #aaa; }
        .signature-line {
            margin-top: 30px;
            border-bottom: 2px solid #333;
            height: 30px;
            font-family: 'Brush Script MT', cursive;
            font-size: 24px;
            color: #1a237e;
            display: flex;
            align-items: flex-end;
        }

        /* --- ESTILOS DO APP (MANTIDOS) --- */
        .cal-day {
            aspect-ratio: 1; border-radius: 8px; display: flex; flex-direction: column;
            align-items: center; justify-content: center; font-size: 12px; font-weight: bold;
            background: #f0f0f0; color: #999; border: 1px solid transparent; position: relative;
        }
        .cal-day span { font-size: 9px; font-weight: normal; margin-top: 2px; }
        .cal-day.green { background: #E3F2FD; color: #1565C0; border-color: #90CAF9; }
        .cal-day.orange { background: #FFF3E0; color: #EF6C00; border-color: #FFCC80; }
        .cal-day.red { background: #FFEBEE; color: #C62828; border-color: #EF9A9A; }
        .cal-day.active { background: white; color: var(--primary-dark); border: 2px dashed var(--primary); }
        
        .calendar-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 8px; margin-top: 15px; }

        .toggle-group { display: flex; flex-direction: column; gap: 8px; margin-bottom: 15px; }
        .toggle-btn { padding: 14px; border: 1px solid #ddd; border-radius: 10px; text-align: center; opacity: 0.7; cursor: pointer; font-weight: 500; transition: 0.2s; }
        .toggle-btn.active.blue { background: var(--blue-save); color: white; opacity: 1; border-color: var(--blue-save); font-weight: bold; }
        .toggle-btn.active.green { background: #E8F5E9; color: #2E7D32; opacity: 1; border-color: #2E7D32; font-weight: bold; }
        .toggle-btn.active.red { background: var(--alert); color: white; opacity: 1; border-color: #B71C1C; font-weight: bold; }

        .nav-bar {
            position: absolute; bottom: 0; width: 100%; height: 65px; background: white; border-top: 1px solid #eee;
            display: flex; justify-content: space-around; align-items: center; z-index: 99;
        }
        .nav-item { display: flex; flex-direction: column; align-items: center; font-size: 10px; color: #999; cursor: pointer; flex: 1; }
        .nav-item.active { color: var(--primary-dark); font-weight: bold; }
        .nav-fab { width: 48px; height: 48px; background: var(--primary); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 24px; margin-bottom: 25px; box-shadow: 0 4px 12px rgba(109, 213, 169, 0.4); color: var(--primary-dark); }
        
        .badge-grid { display: flex; gap: 10px; overflow-x: auto; padding: 5px; scrollbar-width: none; }
        .badge-item { min-width: 70px; height: 80px; background: #f9f9f9; border-radius: 10px; display: flex; flex-direction: column; align-items: center; justify-content: center; opacity: 0.5; filter: grayscale(1); padding: 5px; text-align: center; }
        .badge-item.unlocked { opacity: 1; filter: grayscale(0); background: #FFFDE7; border: 1px solid #FBC02D; }
        
        .rank-row { display: flex; justify-content: space-between; align-items: center; padding: 12px; border-bottom: 1px solid #f5f5f5; }
        .rank-row.highlight { background: #EFFFF8; border-radius: 8px; border: 1px solid var(--primary); }

    </style>
</head>
<body>

<div id="app-container">
    
    <section id="screen-intro" class="screen active" style="justify-content: center;">
        <div style="font-size: 60px; text-align: center; margin-bottom: 20px;">🛡️</div>
        <h1 style="text-align: center;">Bem-vindo ao Dia 0</h1>
        <h2 style="text-align: center; font-size: 16px; font-weight: normal; margin-top: 10px;">A Batalha contra a Impulsividade</h2>
        <p style="text-align: center; margin-top: 20px;">
            Sua missão é vencer o "vilão" dos gastos emocionais pelos próximos 30 dias. <br><br>
            Para começar, precisamos identificar seu inimigo.
        </p>
        <button class="btn" onclick="goToScreen('screen-villain')">Identificar meu Vilão</button>
    </section>

    <section id="screen-villain" class="screen">
        <h1>Quem é seu Vilão?</h1>
        <p>Escolha a categoria que representa seu maior gatilho de gasto impulsivo:</p>
        
        <div class="villain-grid">
            <div class="villain-card" onclick="selectVillain(this, 'Delivery')">
                <span class="villain-icon">🍕</span> Delivery
            </div>
            <div class="villain-card" onclick="selectVillain(this, 'Compras Online')">
                <span class="villain-icon">🛍️</span> Compras
            </div>
            <div class="villain-card" onclick="selectVillain(this, 'Apps Transporte')">
                <span class="villain-icon">🚗</span> Transporte
            </div>
            <div class="villain-card" onclick="selectVillain(this, 'Estética/Moda')">
                <span class="villain-icon">💅</span> Estética
            </div>
            <div class="villain-card" onclick="selectVillain(this, 'Mercado/Doces')">
                <span class="villain-icon">🍫</span> Mercado
            </div>
            <div class="villain-card" onclick="selectVillain(this, 'Jogos/Apps')">
                <span class="villain-icon">🎮</span> Jogos
            </div>
        </div>

        <div style="margin-top: 10px;">
            <label style="font-size: 12px; font-weight: bold;">Seu Nome de Jogador</label>
            <input type="text" id="setupName" placeholder="Ex: Viajante">
        </div>

        <button class="btn" onclick="goToScreen('screen-goal')">Continuar</button>
    </section>

    <section id="screen-goal" class="screen">
        <h1>Defina sua Meta</h1>
        <p>O pré-compromisso aumenta a chance de sucesso.</p>
        
        <div class="card">
            <label style="font-weight: bold; font-size: 14px;">Quanto você quer economizar?</label>
            <p style="font-size: 12px; margin-bottom: 5px;">Ao evitar seu vilão, quanto você quer juntar em 30 dias?</p>
            <input type="number" id="setupGoal" placeholder="Ex: 300,00" style="font-size: 20px; font-weight: bold; color: var(--primary-dark);">
            <div style="font-size: 11px; color: #888; background: #eee; padding: 8px; border-radius: 6px;">
                💡 Dica: Comece com uma meta realista (R$ 50 - R$ 200) para garantir sua primeira vitória.
            </div>
        </div>

        <button class="btn" onclick="prepareContract()">Gerar Contrato</button>
    </section>

    <section id="screen-contract" class="screen">
        <h1>Seu Compromisso</h1>
        <p>Para oficializar o desafio, assine digitalmente abaixo.</p>

        <div class="contract-paper">
            <div style="text-align: center; font-weight: bold; margin-bottom: 15px; text-transform: uppercase; border-bottom: 2px solid #D4C4A8; padding-bottom: 10px;">Termo de Compromisso<br>Desafio 30 Dias</div>
            
            Eu, <strong id="contract-name">Usuário</strong>, declaro estar ciente do desafio de enfrentar o vilão <strong id="contract-villain">Gasto</strong>.
            <br><br>
            Comprometo-me a vigiar meus impulsos e desenvolver autocontrole para atingir a meta de economia de <strong id="contract-goal">R$ 0,00</strong> ao final da jornada.
            
            <div class="signature-line" id="signature-place">
                <span style="color: #ccc; font-family: 'Inter'; font-size: 12px; align-self: center;">Aguardando assinatura...</span>
            </div>
        </div>

        <button class="btn" id="btn-sign" onclick="signContract()">Assinar Digitalmente ✍️</button>
    </section>

    <section id="screen-dashboard" class="screen">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
            <div>
                <h2 style="font-size: 18px;">Olá, <span id="dash-name">User</span></h2>
                <span style="font-size: 12px; color: #888;">Dia <span id="dash-day">1</span> de 30</span>
            </div>
            <div style="background: #E3F2FD; color: #1565C0; padding: 5px 12px; border-radius: 20px; font-weight: bold; font-size: 12px;">
                🏆 <span id="dash-points">0</span> pts
            </div>
        </div>

        <div class="card" style="margin-top: 5px; border-left: 5px solid var(--blue-save);">
            <div style="display: flex; justify-content: space-between; align-items: flex-end;">
                <div style="font-size: 11px; color: #666; font-weight: bold; text-transform: uppercase;">Progresso da Meta</div>
                <div style="font-size: 11px; color: #999;">Meta: R$ <span id="dash-goal-val">0</span></div>
            </div>
            
            <div style="font-size: 32px; font-weight: 800; color: var(--blue-save); margin: 5px 0;">
                R$ <span id="dash-current-net">0,00</span>
            </div>
            
            <div style="display: flex; justify-content: space-between; font-size: 11px; color: #666; margin-bottom: 5px;">
                <span>Acumulado</span>
                <span id="dash-percent">0%</span>
            </div>

            <div style="width: 100%; background: #eee; height: 8px; border-radius: 4px; overflow: hidden;">
                <div id="dash-progress-bar" style="width: 0%; background: var(--blue-save); height: 100%; transition: 1s;"></div>
            </div>
            
            <div style="margin-top: 8px; font-size: 10px; color: #888; text-align: center;">
                Foco: Combater <span id="dash-villain-display" style="font-weight: bold;">Vilão</span>
            </div>
        </div>

        <h3 style="font-size: 12px; margin-bottom: 8px;">CONQUISTAS</h3>
        <div class="badge-grid" id="badge-container"></div>

        <div class="card" style="text-align: center; padding: 12px; margin-top: 10px; cursor: pointer;" onclick="navTo('screen-calendar')">
            <span style="font-size: 20px;">📅</span> <span style="font-weight: bold; font-size: 12px;">Ver Calendário</span>
        </div>
    </section>

    <section id="screen-register" class="screen">
        <h2 style="text-align: center;">Dia <span id="reg-day">1</span></h2>
        
        <div class="card" style="margin-top: 15px;">
            <label style="font-size: 12px; font-weight: bold;">VALOR (R$)</label>
            <input type="number" id="reg-amount" placeholder="0,00" inputmode="decimal">
            
            <div class="toggle-group">
                <div id="btn-save" class="toggle-btn active blue" onclick="setEntryType('saved')">
                    💰 Economizei (Venci o Vilão)
                    <div style="font-size: 9px; font-weight: normal;">Soma na meta (+ Pontos)</div>
                </div>
                <div id="btn-imp" class="toggle-btn" onclick="setEntryType('impulse')">
                    😈 Gastei por Impulso
                    <div style="font-size: 9px; font-weight: normal;">Subtrai da meta (- Pontos)</div>
                </div>
                <div id="btn-plan" class="toggle-btn" onclick="setEntryType('planned')">
                    😇 Gasto Planejado
                    <div style="font-size: 9px; font-weight: normal;">Neutro (Apenas registro)</div>
                </div>
            </div>
            
            <button class="btn" onclick="addEntry()" style="font-size: 14px; height: 44px; margin-top: 10px;">Confirmar Registro</button>
        </div>

        <div id="today-list" style="margin-bottom: 20px;"></div>

        <div style="margin-top: auto;">
            <div style="text-align: right; margin-bottom: 10px; font-size: 13px;">
                Saldo do Dia: <strong>R$ <span id="today-net">0,00</span></strong>
            </div>
            <button class="btn secondary" onclick="finishDay()">Encerrar Dia ✅</button>
        </div>
    </section>

    <section id="screen-calendar" class="screen">
        <h2>Jornada Diária</h2>
        <div class="calendar-grid" id="cal-grid"></div>
        <div style="margin-top: 15px; font-size: 10px; color: #666; text-align: center;">
            <span style="color:#1565C0">🟦 Saldo Positivo</span> | <span style="color:#C62828">🟥 Saldo Negativo</span>
        </div>
    </section>

    <section id="screen-ranking" class="screen">
        <h2>Ranking (Pontos)</h2>
        <p style="font-size: 12px; color: #666; margin-bottom: 15px;">1 Ponto = R$ 1 Real de Saldo Líquido</p>
        <div id="ranking-list" style="background: white; border-radius: 12px; border: 1px solid #eee;"></div>
    </section>

    <nav class="nav-bar" id="navbar" style="display: none;">
        <div class="nav-item active" onclick="navTo('screen-dashboard', this)">🏠<br>Início</div>
        <div class="nav-fab" onclick="navTo('screen-register', null)">+</div>
        <div class="nav-item" onclick="navTo('screen-ranking', this)">🏆<br>Rank</div>
    </nav>

</div>

<script>
    // --- ESTADO & CONFIG --- //
    const BADGES = [
        { id: 'b1', icon: '🌱', name: 'Início', desc: 'Dia 1 OK' },
        { id: 'b2', icon: '🐷', name: 'Cofrinho', desc: 'Acumulou R$ 50' },
        { id: 'b3', icon: '🚀', name: 'Foguete', desc: '50% da Meta' },
        { id: 'b4', icon: '💎', name: 'Rico', desc: 'Meta Batida' }
    ];

    let bots = [
        { name: "Ana P.", points: 120 },
        { name: "Carlos", points: 45 },
        { name: "Julia Bot", points: 200 }
    ];

    let state = {
        started: false, // Novo flag para o onboarding
        name: 'Usuário',
        villain: '', // Categoria do vilão
        goal: 0,
        day: 1,
        
        totalSaved: 0,
        totalImpulse: 0,
        netBalance: 0,
        
        history: [],
        badges: [],

        todayLog: [],
        todaySaved: 0,
        todayImpulse: 0,
        todayPlanned: 0
    };

    // --- INIT --- //
    function init() {
        const saved = localStorage.getItem('eco_v5_day0');
        if (saved) {
            state = JSON.parse(saved);
            // Se já começou (passou do dia 0), vai pro dashboard
            if (state.started) {
                updateAll();
                navTo('screen-dashboard');
                document.getElementById('navbar').style.display = 'flex';
            }
        }
    }

    // --- LÓGICA DO DIA 0 (ONBOARDING) --- //
    function selectVillain(el, name) {
        document.querySelectorAll('.villain-card').forEach(c => c.classList.remove('selected'));
        el.classList.add('selected');
        state.villain = name;
    }

    function prepareContract() {
        state.name = document.getElementById('setupName').value || "Usuário";
        const g = parseFloat(document.getElementById('setupGoal').value);
        
        if (!state.villain) return alert("Por favor, selecione seu Vilão na tela anterior.");
        if (!g || g <= 0) return alert("Defina uma meta de economia válida.");
        
        state.goal = g;
        
        // Preenche o contrato visual
        document.getElementById('contract-name').innerText = state.name;
        document.getElementById('contract-villain').innerText = state.villain;
        document.getElementById('contract-goal').innerText = "R$ " + state.goal.toFixed(2);
        
        goToScreen('screen-contract');
    }

    function signContract() {
        const btn = document.getElementById('btn-sign');
        const signaturePlace = document.getElementById('signature-place');
        
        btn.disabled = true;
        btn.innerText = "Assinando...";
        
        setTimeout(() => {
            // Efeito de assinatura
            signaturePlace.innerHTML = `<span style="color:#1a237e; font-style:italic;">${state.name}</span>`;
            
            setTimeout(() => {
                alert("Compromisso firmado! Bem-vindo ao Dia 1.");
                state.started = true; // Marca onboarding como concluído
                save();
                updateAll();
                navTo('screen-dashboard');
                document.getElementById('navbar').style.display = 'flex';
            }, 800);
        }, 1000);
    }

    // --- NAVEGAÇÃO --- //
    function navTo(id, el) {
        document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        
        if (id === 'screen-calendar') renderCalendar();
        if (id === 'screen-ranking') renderRanking();
        
        if (el) {
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            el.classList.add('active');
        }
    }
    function goToScreen(id) { navTo(id); }

    // --- REGISTRO --- //
    let tempType = 'saved';

    function setEntryType(type) {
        tempType = type;
        document.querySelectorAll('.toggle-btn').forEach(b => b.className = 'toggle-btn');
        
        const map = { 'saved': 'btn-save', 'impulse': 'btn-imp', 'planned': 'btn-plan' };
        const colorMap = { 'saved': 'blue', 'impulse': 'red', 'planned': 'green' };
        
        const btn = document.getElementById(map[type]);
        btn.classList.add('active', colorMap[type]);
    }

    function addEntry() {
        const val = parseFloat(document.getElementById('reg-amount').value);
        if (!val || val <= 0) return alert("Digite um valor.");

        if (tempType === 'saved') state.todaySaved += val;
        if (tempType === 'impulse') state.todayImpulse += val;
        if (tempType === 'planned') state.todayPlanned += val;

        state.todayLog.push({ val, type: tempType });
        
        document.getElementById('reg-amount').value = '';
        renderTodayList();
        save();
    }

    function renderTodayList() {
        const list = document.getElementById('today-list');
        list.innerHTML = state.todayLog.map(i => {
            let color = i.type === 'saved' ? '#1565C0' : (i.type === 'impulse' ? '#C62828' : '#2E7D32');
            let signal = i.type === 'saved' ? '+' : '-';
            if(i.type === 'planned') signal = '';
            
            let label = i.type === 'saved' ? 'Economia' : (i.type === 'impulse' ? 'Impulso' : 'Planejado');
            
            return `<div style="display:flex; justify-content:space-between; padding:8px; border-bottom:1px solid #eee; font-size:13px;">
                <span style="color:${color};">${label}</span>
                <strong>${signal} R$ ${i.val.toFixed(2)}</strong>
            </div>`;
        }).join('');

        const netToday = state.todaySaved - state.todayImpulse;
        document.getElementById('today-net').innerText = netToday.toFixed(2);
        document.getElementById('today-net').style.color = netToday >= 0 ? 'var(--blue-save)' : 'var(--alert)';
    }

    // --- ENCERRAR DIA --- //
    function finishDay() {
        if (!confirm("Encerrar o dia?")) return;

        state.totalSaved += state.todaySaved;
        state.totalImpulse += state.todayImpulse;
        state.netBalance = state.totalSaved - state.totalImpulse;

        state.history.push({
            day: state.day,
            saved: state.todaySaved,
            impulse: state.todayImpulse,
            net: state.todaySaved - state.todayImpulse
        });

        // Bots update
        bots.forEach(b => { b.points += Math.floor(Math.random() * 40) - 10; });

        // Badges
        if(state.day === 1) unlockBadge('b1');
        if(state.netBalance >= 50) unlockBadge('b2');
        if(state.netBalance >= (state.goal / 2)) unlockBadge('b3');
        if(state.netBalance >= state.goal) unlockBadge('b4');

        state.day++;
        state.todayLog = [];
        state.todaySaved = 0;
        state.todayImpulse = 0;
        state.todayPlanned = 0;

        save();
        
        if (state.day > 30) {
            alert("PARABÉNS! VOCÊ VENCEU O DESAFIO!"); 
        } else {
            alert("Dia encerrado com sucesso.");
            updateAll();
            renderTodayList();
            navTo('screen-dashboard');
        }
    }

    function unlockBadge(id) {
        if(!state.badges.includes(id)) {
            state.badges.push(id);
            alert("Nova Conquista Desbloqueada! 🏆");
        }
    }

    // --- DASHBOARD UPDATE --- //
    function updateAll() {
        document.getElementById('dash-name').innerText = state.name;
        document.getElementById('dash-day').innerText = state.day;
        document.getElementById('reg-day').innerText = state.day;
        document.getElementById('dash-villain-display').innerText = state.villain;
        
        const current = state.netBalance;
        document.getElementById('dash-current-net').innerText = current.toFixed(2);
        document.getElementById('dash-goal-val').innerText = state.goal;
        
        let pct = (current / state.goal) * 100;
        if(pct < 0) pct = 0; 
        if(pct > 100) pct = 100;
        
        document.getElementById('dash-percent').innerText = Math.floor(pct) + "%";
        document.getElementById('dash-progress-bar').style.width = pct + "%";

        const points = Math.max(0, Math.floor(current));
        document.getElementById('dash-points').innerText = points;

        document.getElementById('badge-container').innerHTML = BADGES.map(b => `
            <div class="badge-item ${state.badges.includes(b.id)?'unlocked':''}">
                <div style="font-size:24px;">${b.icon}</div>
                <div style="font-size:9px; font-weight:bold;">${b.name}</div>
            </div>
        `).join('');
    }

    // --- CALENDÁRIO --- //
    function renderCalendar() {
        const grid = document.getElementById('cal-grid');
        grid.innerHTML = '';
        for(let i=1; i<=30; i++) {
            const h = state.history.find(x => x.day === i);
            let cls = '';
            let content = i;
            
            if(h) {
                if(h.net > 0) cls = 'green';
                else if(h.net < 0) cls = 'red';
                else cls = 'orange';
                content += `<br><span>R$${Math.round(h.net)}</span>`;
            } else if (i === state.day) {
                cls = 'active';
            }
            
            grid.innerHTML += `<div class="cal-day ${cls}">${content}</div>`;
        }
    }

    // --- RANKING --- //
    function renderRanking() {
        const myPoints = Math.max(0, Math.floor(state.netBalance));
        const list = [...bots, { name: state.name + " (Você)", points: myPoints, me: true }];
        
        list.sort((a,b) => b.points - a.points);
        
        document.getElementById('ranking-list').innerHTML = list.map((u, idx) => `
            <div class="rank-row ${u.me?'highlight':''}">
                <div style="display:flex; align-items:center; gap:10px;">
                    <b style="color:#999; font-size:12px;">${idx+1}</b>
                    <span>${u.name}</span>
                </div>
                <strong>${u.points} pts</strong>
            </div>
        `).join('');
    }

    function save() { localStorage.setItem('eco_v5_day0', JSON.stringify(state)); }

    init();

</script>
</body>
</html>
