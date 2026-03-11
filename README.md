index.html.
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Porquim IA - Acesso Restrito</title>
    <style>
        :root { 
            --bg-overlay: rgba(248, 250, 252, 0.92); 
            --card: #ffffff; 
            --text: #1e293b; 
            --primary: #3b82f6; 
            --income: #10b981; 
            --expense: #ef4444; 
            --border: #e2e8f0; 
        }
        
        body { 
            font-family: 'Inter', sans-serif; 
            color: var(--text); 
            margin: 0; 
            padding: 20px; 
            display: flex; 
            justify-content: center; 
            align-items: center;
            min-height: 100vh; 
            background-color: #f8fafc; 
            background-image: linear-gradient(var(--bg-overlay), var(--bg-overlay)), url('https://images.unsplash.com/photo-1620714223084-8fcacc6dfd8d?q=80&w=2070&auto=format&fit=crop'); 
            background-size: cover; background-position: center; background-attachment: fixed; 
        }

        .container { width: 100%; max-width: 450px; background: rgba(255, 255, 255, 0.7); border-radius: 20px; padding: 25px; backdrop-filter: blur(12px); box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1); border: 1px solid rgba(255,255,255,0.3); }
        
        /* TELAS */
        .page { display: none; }
        .page.active { display: block; }

        header { text-align: center; margin-bottom: 30px; }
        h1 { font-size: 2rem; color: #1e3a8a; margin: 0; font-weight: 800; }
        p.subtitle { color: #64748b; font-size: 0.9rem; margin-top: 5px; }

        input, select, button { width: 100%; padding: 14px; margin-bottom: 15px; border: 1px solid var(--border); border-radius: 10px; box-sizing: border-box; font-size: 1rem; transition: 0.3s; }
        input:focus { border-color: var(--primary); outline: none; box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1); }
        
        button { background: var(--primary); color: white; border: none; font-weight: 700; cursor: pointer; }
        button:hover { transform: translateY(-1px); filter: brightness(1.1); }
        .btn-link { background: none; color: var(--primary); padding: 5px; font-size: 0.85rem; margin-top: -5px; }

        /* Estilos do Financeiro (Resumidos) */
        .summary { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 20px; }
        .card { background: white; padding: 12px; border-radius: 12px; border: 1px solid var(--border); }
        .card.full { grid-column: 1 / -1; border-top: 4px solid var(--primary); }
        .day-group { background: #f1f5f9; padding: 8px 12px; border-radius: 8px; margin: 20px 0 10px 0; display: flex; justify-content: space-between; font-size: 0.8rem; }
        .transaction-item { background: white; padding: 12px; border-radius: 10px; margin-bottom: 8px; display: flex; align-items: center; border-left: 5px solid #ddd; font-size: 0.9rem; }
        
        .user-info { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; padding: 10px; background: rgba(59, 130, 246, 0.1); border-radius: 10px; }
        .user-info span { font-weight: 700; color: #1e3a8a; }
        .logout-btn { width: auto; padding: 5px 10px; margin: 0; background: #ef4444; font-size: 0.7rem; }
    </style>
</head>
<body>

<div class="container">
    
    <div id="page-login" class="page active">
        <header>
            <h1>🐷 Porquim IA</h1>
            <p class="subtitle">Faça login para gerenciar sua loja</p>
        </header>
        <input type="email" id="login-email" placeholder="Seu e-mail">
        <input type="password" id="login-pass" placeholder="Sua senha">
        <button onclick="handleLogin()">Entrar no Sistema</button>
        <button class="btn-link" onclick="showPage('page-register')">Não tem conta? Criar agora</button>
    </div>

    <div id="page-register" class="page">
        <header>
            <h1>Criar Conta</h1>
            <p class="subtitle">Rápido, fácil e profissional</p>
        </header>
        <input type="text" id="reg-name" placeholder="Nome Completo / Nome da Loja">
        <input type="email" id="reg-email" placeholder="Melhor e-mail">
        <input type="password" id="reg-pass" placeholder="Crie uma senha">
        <button onclick="handleRegister()">Finalizar Cadastro</button>
        <button class="btn-link" onclick="showPage('page-login')">Já tenho conta</button>
    </div>

    <div id="page-app" class="page">
        <div class="user-info">
            <div>Olá, <span id="display-name">...</span>!</div>
            <button class="logout-btn" onclick="logout()">Sair</button>
        </div>

        <div class="summary">
            <div class="card full"><label>Saldo Total</label><p id="total-balance">R$ 0,00</p></div>
            <div class="card"><p style="color: var(--income); font-size: 1rem;" id="total-income">R$ 0,00</p></div>
            <div class="card"><p style="color: var(--expense); font-size: 1rem;" id="total-expense">R$ 0,00</p></div>
        </div>

        <div style="background: white; padding: 15px; border-radius: 12px; margin-bottom: 20px; border: 1px solid var(--border);">
            <input type="text" id="desc" placeholder="O que vendeu/comprou?">
            <div class="row">
                <input type="text" id="amount" placeholder="Valor">
                <input type="date" id="date">
            </div>
            <div class="row">
                <select id="type">
                    <option value="entrada">Entrada 📥</option>
                    <option value="saida">Saída 📤</option>
                </select>
                <button onclick="saveTransaction()" style="margin:0">Lançar</button>
            </div>
        </div>

        <div style="display:flex; justify-content: space-between; align-items:center; margin-bottom:10px;">
            <strong style="font-size:0.8rem">HISTÓRICO</strong>
            <input type="month" id="month-filter" style="width:auto; margin:0; padding:5px;" onchange="renderUI()">
        </div>
        <div id="transaction-list"></div>
        
        <div style="margin-top:20px; display:flex; gap:5px;">
            <button onclick="exportData()" style="background:#64748b; font-size:0.7rem">Backup</button>
        </div>
    </div>

</div>

<script>
    // --- LÓGICA DE NAVEGAÇÃO E LOGIN ---
    let currentUser = JSON.parse(localStorage.getItem('porquim_user')) || null;
    let transactions = JSON.parse(localStorage.getItem('porquim_ia_db')) || [];

    function showPage(pageId) {
        document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
        document.getElementById(pageId).classList.add('active');
    }

    function handleRegister() {
        const name = document.getElementById('reg-name').value;
        const email = document.getElementById('reg-email').value;
        const pass = document.getElementById('reg-pass').value;

        if(!name || !email || !pass) return alert("Preencha todos os campos!");

        const newUser = { name, email, pass };
        localStorage.setItem('porquim_user', JSON.stringify(newUser));
        alert("Conta criada com sucesso! Agora faça login.");
        showPage('page-login');
    }

    function handleLogin() {
        const email = document.getElementById('login-email').value;
        const pass = document.getElementById('login-pass').value;
        const savedUser = JSON.parse(localStorage.getItem('porquim_user'));

        if(savedUser && savedUser.email === email && savedUser.pass === pass) {
            currentUser = savedUser;
            initApp();
        } else {
            alert("E-mail ou senha incorretos!");
        }
    }

    function logout() {
        currentUser = null;
        showPage('page-login');
    }

    // --- LÓGICA DO APP (REUTILIZADA) ---
    function initApp() {
        document.getElementById('display-name').innerText = currentUser.name;
        document.getElementById('month-filter').value = new Date().toISOString().substring(0, 7);
        document.getElementById('date').valueAsDate = new Date();
        showPage('page-app');
        renderUI();
    }

    function saveTransaction() {
        const desc = document.getElementById('desc').value;
        const amountStr = document.getElementById('amount').value;
        const amount = parseFloat(amountStr.replace(',', '.')) || 0;
        const date = document.getElementById('date').value;
        const type = document.getElementById('type').value;

        if(!desc || amount <= 0 || !date) return alert("Dados inválidos!");

        transactions.push({ id: Date.now(), desc, amount, date, type });
        localStorage.setItem('porquim_ia_db', JSON.stringify(transactions));
        
        document.getElementById('desc').value = '';
        document.getElementById('amount').value = '';
        renderUI();
    }

    function renderUI() {
        const listEl = document.getElementById('transaction-list');
        const filterVal = document.getElementById('month-filter').value;
        listEl.innerHTML = '';
        let mInc = 0, mExp = 0;

        const filtered = transactions.filter(t => t.date.startsWith(filterVal));
        const grouped = {};
        
        filtered.forEach(t => {
            if (!grouped[t.date]) grouped[t.date] = { items: [], in: 0, out: 0 };
            grouped[t.date].items.push(t);
            if (t.type === 'entrada') { mInc += t.amount; grouped[t.date].in += t.amount; }
            else { mExp += t.amount; grouped[t.date].out += t.amount; }
        });

        Object.keys(grouped).sort((a,b) => new Date(b) - new Date(a)).forEach(date => {
            const d = grouped[date];
            listEl.innerHTML += `<div class="day-group"><strong>📅 ${date.split('-').reverse().join('/')}</strong><span>R$ ${(d.in-d.out).toLocaleString('pt-BR')}</span></div>`;
            d.items.forEach(t => {
                const isInc = t.type === 'entrada';
                listEl.innerHTML += `<div class="transaction-item" style="border-left-color: ${isInc ? 'var(--income)' : 'var(--expense)'}">
                    <div style="flex:1"><strong>${t.desc}</strong></div>
                    <div style="font-weight:800; color:${isInc ? 'var(--income)' : 'var(--expense)'}">R$ ${t.amount.toLocaleString('pt-BR')}</div>
                </div>`;
            });
        });

        document.getElementById('total-income').innerText = `Entradas: R$ ${mInc.toLocaleString('pt-BR')}`;
        document.getElementById('total-expense').innerText = `Saídas: R$ ${mExp.toLocaleString('pt-BR')}`;
        document.getElementById('total-balance').innerText = `R$ ${(mInc - mExp).toLocaleString('pt-BR')}`;
    }

    function exportData() {
        const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(transactions));
        const dlAnchorElem = document.createElement('a');
        dlAnchorElem.setAttribute("href", dataStr);
        dlAnchorElem.setAttribute("download", "backup_porquim.json");
        dlAnchorElem.click();
    }

    // Verifica se já está logado ao abrir
    if(currentUser) initApp();
</script>
</body>
</html>
