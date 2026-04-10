<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover" />
  <title>Family Banker</title>
  <style>
    :root {
      --bg: #f4f6fb;
      --card: #ffffff;
      --text: #1d2433;
      --muted: #647089;
      --border: #dbe2ef;
      --accent: #3563e9;
      --accent-2: #1f9d6a;
      --danger: #d64545;
      --warning: #e68a00;
      --shadow: 0 10px 24px rgba(31, 48, 86, 0.08);
      --radius: 18px;
    }


    * { box-sizing: border-box; }
    html, body {
      margin: 0;
      padding: 0;
      background: var(--bg);
      color: var(--text);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    }
    body { min-height: 100vh; }
    button, input, select { font: inherit; }
    .app { max-width: 1000px; margin: 0 auto; padding: 18px 14px 40px; }
    .topbar {
      display: flex; justify-content: space-between; align-items: center; gap: 12px; margin-bottom: 16px;
    }
    .title h1 { margin: 0; font-size: 1.75rem; }
    .title p { margin: 4px 0 0; color: var(--muted); }
    .actions { display: flex; gap: 8px; flex-wrap: wrap; }
    .btn {
      border: 0; background: var(--accent); color: #fff; padding: 12px 14px; border-radius: 14px; cursor: pointer;
      box-shadow: var(--shadow); font-weight: 700;
    }
    .btn.secondary { background: #fff; color: var(--text); border: 1px solid var(--border); box-shadow: none; }
    .btn.success { background: var(--accent-2); }
    .btn.danger { background: var(--danger); }
    .btn.warning { background: var(--warning); }
    .btn.ghost { background: transparent; color: var(--accent); border: 1px solid var(--border); box-shadow: none; }
    .btn.small { padding: 10px 11px; border-radius: 12px; font-size: 0.95rem; }
    .btn:disabled { opacity: 0.5; cursor: not-allowed; }
    .panel {
      background: var(--card); border: 1px solid var(--border); border-radius: var(--radius); box-shadow: var(--shadow);
      padding: 16px;
    }
    .hidden { display: none !important; }


    .setup-header { display: flex; justify-content: space-between; gap: 12px; align-items: end; margin-bottom: 14px; }
    .setup-controls, .inline-fields {
      display: grid; grid-template-columns: 1fr 1fr; gap: 10px;
    }
    .field { display: flex; flex-direction: column; gap: 6px; }
    .field label { font-size: 0.92rem; color: var(--muted); font-weight: 700; }
    .field input, .field select {
      width: 100%; padding: 12px 14px; border-radius: 12px; border: 1px solid var(--border); background: #fff;
    }
    .players-setup-list { display: grid; gap: 12px; margin-top: 14px; }
    .setup-player-row {
      display: grid; grid-template-columns: minmax(0, 1.8fr) 92px 92px 54px; gap: 10px; align-items: end;
      padding: 12px; border: 1px solid var(--border); border-radius: 16px; background: #fbfcff;
    }
    .emoji-input { text-align: center; }
    .color-swatch {
      width: 16px; height: 16px; border-radius: 999px; display: inline-block; margin-right: 8px; vertical-align: middle; border: 1px solid rgba(0,0,0,.08);
    }


    .players-grid {
      display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 14px; margin-top: 16px;
    }
    .player-card {
      position: relative; overflow: hidden; background: var(--card); border: 1px solid var(--border); border-radius: 20px;
      box-shadow: var(--shadow); padding: 16px; cursor: pointer;
    }
    .player-card::before {
      content: ""; position: absolute; inset: 0 auto 0 0; width: 8px; background: var(--card-accent, var(--accent));
    }
    .player-top { display: flex; justify-content: space-between; gap: 8px; align-items: center; }
    .avatar {
      width: 46px; height: 46px; border-radius: 999px; display: grid; place-items: center; font-size: 1.2rem; font-weight: 700;
      background: rgba(53, 99, 233, 0.12); color: var(--accent);
    }
    .player-name { font-weight: 800; font-size: 1.1rem; }
    .player-balance { margin-top: 18px; font-size: 2rem; font-weight: 800; letter-spacing: -0.02em; }
    .player-meta { color: var(--muted); margin-top: 8px; font-size: 0.95rem; }


    .detail-view { margin-top: 16px; }
    .detail-header { display: flex; justify-content: space-between; align-items: flex-start; gap: 12px; margin-bottom: 14px; }
    .detail-header h2 { margin: 0; font-size: 1.4rem; }
    .balance-hero { font-size: 2.4rem; font-weight: 800; margin-top: 8px; }
    .quick-grid {
      display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 10px; margin-top: 12px;
    }
    .section-title { margin: 18px 0 10px; color: var(--muted); font-size: 0.92rem; font-weight: 800; letter-spacing: 0.03em; text-transform: uppercase; }
    .history-list { display: grid; gap: 8px; margin-top: 10px; }
    .history-item {
      display: flex; justify-content: space-between; gap: 10px; align-items: center; border: 1px solid var(--border); border-radius: 14px; padding: 12px 14px;
    }
    .history-label { font-weight: 700; }
    .history-time { color: var(--muted); font-size: 0.9rem; }
    .amount.plus { color: var(--accent-2); font-weight: 800; }
    .amount.minus { color: var(--danger); font-weight: 800; }


    .transfer-box, .custom-box { display: grid; gap: 10px; margin-top: 10px; }
    .footer-note { margin-top: 18px; color: var(--muted); font-size: 0.92rem; text-align: center; }
    .pill {
      display: inline-flex; align-items: center; gap: 8px; padding: 8px 12px; border: 1px solid var(--border); border-radius: 999px; color: var(--muted); font-size: .92rem; background: #fff;
    }


    @media (max-width: 720px) {
      .topbar, .detail-header, .setup-header { flex-direction: column; align-items: stretch; }
      .setup-controls, .inline-fields { grid-template-columns: 1fr; }
      .setup-player-row { grid-template-columns: 1fr; }
      .quick-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }
      .balance-hero { font-size: 2rem; }
      .player-balance { font-size: 1.8rem; }
    }
  </style>
</head>
<body>
  <div class="app">
    <div class="topbar">
      <div class="title">
        <h1>Family Banker</h1>
        <p>Local Monopoly-style money tracker for one banker, one device.</p>
      </div>
      <div class="actions">
        <button id="newGameBtn" class="btn secondary hidden">New Game</button>
        <button id="resetBtn" class="btn danger hidden">Reset Balances</button>
      </div>
    </div>


    <section id="setupPanel" class="panel">
      <div class="setup-header">
        <div>
          <h2 style="margin:0;">Set up players</h2>
          <p style="color: var(--muted); margin: 6px 0 0;">Enter names, adjust the starting balance, and add or remove players before you begin.</p>
        </div>
        <div class="pill"><span id="setupCount">0 players</span></div>
      </div>


      <div class="setup-controls">
        <div class="field">
          <label for="startingBalance">Starting balance</label>
          <input id="startingBalance" type="number" min="0" step="1" value="1500" />
        </div>
        <div class="field">
          <label>&nbsp;</label>
          <div style="display:flex; gap:10px; flex-wrap:wrap;">
            <button id="addPlayerBtn" class="btn success" type="button">Add Player</button>
            <button id="loadSavedBtn" class="btn secondary hidden" type="button">Load Saved Game</button>
          </div>
        </div>
      </div>


      <div id="setupPlayersList" class="players-setup-list"></div>


      <div style="display:flex; gap:10px; flex-wrap:wrap; margin-top: 14px;">
        <button id="startGameBtn" class="btn">Start Game</button>
      </div>
    </section>


    <section id="dashboard" class="hidden">
      <div class="players-grid" id="playersGrid"></div>
      <div class="footer-note">Tap a player card to update money, transfer funds, or view that player's history.</div>
    </section>


    <section id="detailView" class="detail-view hidden panel"></section>
  </div>


  <script>
    const STORAGE_KEY = 'family-banker-save-v3';
    const palette = ['#3563e9', '#1f9d6a', '#e68a00', '#8b5cf6', '#d64545', '#0ea5b7', '#f43f5e', '#22c55e'];
    const emojis = ['😀','😎','🤠','🦁','🚀','⭐','🐯','🎯'];


    let state = {
      startingBalance: 1500,
      players: [],
      selectedPlayerId: null,
      gameStarted: false,
    };


    let draftPlayers = [];


    const setupPanel = document.getElementById('setupPanel');
    const dashboard = document.getElementById('dashboard');
    const playersGrid = document.getElementById('playersGrid');
    const detailView = document.getElementById('detailView');
    const setupPlayersList = document.getElementById('setupPlayersList');
    const setupCount = document.getElementById('setupCount');
    const startingBalanceInput = document.getElementById('startingBalance');
    const startGameBtn = document.getElementById('startGameBtn');
    const addPlayerBtn = document.getElementById('addPlayerBtn');
    const resetBtn = document.getElementById('resetBtn');
    const newGameBtn = document.getElementById('newGameBtn');
    const loadSavedBtn = document.getElementById('loadSavedBtn');


    function makeId() {
      return Math.random().toString(36).slice(2, 9);
    }


    function pounds(n) {
      return '£' + Number(n).toLocaleString('en-GB');
    }


    function nowString() {
      return new Date().toLocaleTimeString([], { hour: 'numeric', minute: '2-digit' });
    }


    function escapeHtml(str) {
      return String(str)
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#039;');
    }


    function saveState() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
    }


    function loadState() {
      const raw = localStorage.getItem(STORAGE_KEY);
      if (!raw) return false;
      try {
        const parsed = JSON.parse(raw);
        if (!parsed || !Array.isArray(parsed.players)) return false;
        state = parsed;
        return true;
      } catch {
        return false;
      }
    }


    function makeDraftPlayer(index = draftPlayers.length) {
      return {
        id: makeId(),
        name: `Player ${index + 1}`,
        emoji: emojis[index % emojis.length],
        color: palette[index % palette.length],
      };
    }


    function resetDraftPlayers(count = 4) {
      draftPlayers = Array.from({ length: count }, (_, i) => makeDraftPlayer(i));
      renderSetupPlayers();
    }


    function addDraftPlayer() {
      draftPlayers.push(makeDraftPlayer(draftPlayers.length));
      renderSetupPlayers();
    }


    function removeDraftPlayer(id) {
      if (draftPlayers.length <= 2) {
        alert('Keep at least 2 players in the game.');
        return;
      }
      draftPlayers = draftPlayers.filter(player => player.id !== id);
      renderSetupPlayers();
    }


    function renderSetupPlayers() {
      setupPlayersList.innerHTML = '';
      setupCount.textContent = `${draftPlayers.length} ${draftPlayers.length === 1 ? 'player' : 'players'}`;


      draftPlayers.forEach((player, index) => {
        const row = document.createElement('div');
        row.className = 'setup-player-row';
        row.innerHTML = `
          <div class="field">
            <label for="name-${player.id}">Player ${index + 1} name</label>
            <input id="name-${player.id}" type="text" value="${escapeHtml(player.name)}" placeholder="Player ${index + 1}" />
          </div>
          <div class="field">
            <label for="emoji-${player.id}">Emoji</label>
            <input id="emoji-${player.id}" class="emoji-input" type="text" maxlength="2" value="${escapeHtml(player.emoji)}" />
          </div>
          <div class="field">
            <label for="color-${player.id}">Color</label>
            <select id="color-${player.id}">
              ${palette.map(color => `<option value="${color}" ${color === player.color ? 'selected' : ''}>${color}</option>`).join('')}
            </select>
          </div>
          <button class="btn danger small remove-player-btn" type="button" data-id="${player.id}">✕</button>
        `;
        setupPlayersList.appendChild(row);
      });


      setupPlayersList.querySelectorAll('.remove-player-btn').forEach(btn => {
        btn.addEventListener('click', () => removeDraftPlayer(btn.dataset.id));
      });
    }


    function syncDraftFromInputs() {
      draftPlayers = draftPlayers.map((player, index) => {
        const nameInput = document.getElementById(`name-${player.id}`);
        const emojiInput = document.getElementById(`emoji-${player.id}`);
        const colorInput = document.getElementById(`color-${player.id}`);
        return {
          ...player,
          name: (nameInput?.value || '').trim() || `Player ${index + 1}`,
          emoji: (emojiInput?.value || '').trim() || emojis[index % emojis.length],
          color: colorInput?.value || palette[index % palette.length],
        };
      });
    }


    function createPlayersFromSetup() {
      syncDraftFromInputs();
      const start = Number(startingBalanceInput.value);
      if (Number.isNaN(start) || start < 0) {
        alert('Enter a valid starting balance.');
        return false;
      }
      if (draftPlayers.length < 2) {
        alert('Add at least 2 players.');
        return false;
      }
      state.startingBalance = start;
      state.players = draftPlayers.map((draft, index) => ({
        id: makeId(),
        name: draft.name,
        balance: start,
        color: draft.color,
        emoji: draft.emoji || emojis[index % emojis.length],
        history: [{ label: 'Game started', amount: 0, type: 'neutral', time: nowString() }],
      }));
      state.selectedPlayerId = null;
      state.gameStarted = true;
      saveState();
      return true;
    }


    function showGameUI() {
      setupPanel.classList.add('hidden');
      dashboard.classList.remove('hidden');
      resetBtn.classList.remove('hidden');
      newGameBtn.classList.remove('hidden');
      renderPlayers();
      if (state.selectedPlayerId) renderDetail(state.selectedPlayerId);
      else detailView.classList.add('hidden');
    }


    function showSetupUI() {
      setupPanel.classList.remove('hidden');
      dashboard.classList.add('hidden');
      detailView.classList.add('hidden');
      resetBtn.classList.add('hidden');
      newGameBtn.classList.add('hidden');
      loadSavedBtn.textContent = 'Load Saved Game';
      if (localStorage.getItem(STORAGE_KEY)) loadSavedBtn.classList.remove('hidden');
      else loadSavedBtn.classList.add('hidden');
      startingBalanceInput.value = state.startingBalance || 1500;
      if (!draftPlayers.length) resetDraftPlayers(4);
      else renderSetupPlayers();
    }


    function renderPlayers() {
      playersGrid.innerHTML = '';
      state.players.forEach(player => {
        const card = document.createElement('div');
        card.className = 'player-card';
        card.style.setProperty('--card-accent', player.color);
        card.innerHTML = `
          <div class="player-top">
            <div>
              <div class="player-name">${escapeHtml(player.name)}</div>
              <div class="player-meta">Tap to manage money</div>
            </div>
            <div class="avatar" style="background:${player.color}1a; color:${player.color};">${escapeHtml(player.emoji)}</div>
          </div>
          <div class="player-balance">${pounds(player.balance)}</div>
        `;
        card.addEventListener('click', () => {
          state.selectedPlayerId = player.id;
          saveState();
          renderDetail(player.id);
        });
        playersGrid.appendChild(card);
      });
    }


    function renderDetail(playerId) {
      const player = state.players.find(p => p.id === playerId);
      if (!player) return;
      detailView.classList.remove('hidden');
      const playerOptions = state.players
        .filter(p => p.id !== player.id)
        .map(p => `<option value="${p.id}">${escapeHtml(p.name)}</option>`)
        .join('');


      detailView.innerHTML = `
        <div class="detail-header">
          <div>
            <button class="btn ghost small" id="backToPlayers">← Back to players</button>
            <h2 style="margin-top:12px;">${escapeHtml(player.name)}</h2>
            <div class="balance-hero">${pounds(player.balance)}</div>
          </div>
          <div class="avatar" style="width:56px; height:56px; font-size:1.4rem; background:${player.color}1a; color:${player.color};">${escapeHtml(player.emoji)}</div>
        </div>


        <div class="section-title">Add money</div>
        <div class="quick-grid">
          ${[10,20,50,100,200,500].map(v => `<button class="btn small success delta-btn" data-player="${player.id}" data-amount="${v}">+${v}</button>`).join('')}
        </div>


        <div class="section-title">Subtract money</div>
        <div class="quick-grid">
          ${[10,20,50,100,200,500].map(v => `<button class="btn small danger delta-btn" data-player="${player.id}" data-amount="-${v}">-${v}</button>`).join('')}
        </div>


        <div class="section-title">Custom amount</div>
        <div class="custom-box">
          <div class="inline-fields">
            <div class="field">
              <label for="customAmount">Amount</label>
              <input id="customAmount" type="number" min="1" step="1" placeholder="75" />
            </div>
            <div class="field">
              <label>&nbsp;</label>
              <div style="display:flex; gap:10px; flex-wrap:wrap;">
                <button class="btn success" id="customAddBtn">Add Amount</button>
                <button class="btn danger" id="customSubtractBtn">Subtract Amount</button>
              </div>
            </div>
          </div>
        </div>


        <div class="section-title">Transfer money</div>
        <div class="transfer-box">
          <div class="inline-fields">
            <div class="field">
              <label for="transferTo">Transfer to</label>
              <select id="transferTo">${playerOptions}</select>
            </div>
            <div class="field">
              <label for="transferAmount">Amount</label>
              <input id="transferAmount" type="number" min="1" step="1" placeholder="100" />
            </div>
          </div>
          <div style="display:flex; gap:10px; flex-wrap:wrap;">
            <button class="btn" id="transferBtn">Send Money</button>
            <button class="btn secondary" id="collect200Btn">Collect £200</button>
            <button class="btn secondary" id="pay50Btn">Pay £50</button>
          </div>
        </div>


        <div class="section-title">History</div>
        <div class="history-list">
          ${player.history.length ? player.history.slice().reverse().map(item => `
            <div class="history-item">
              <div>
                <div class="history-label">${escapeHtml(item.label)}</div>
                <div class="history-time">${escapeHtml(item.time || '')}</div>
              </div>
              <div class="amount ${item.type === 'plus' ? 'plus' : item.type === 'minus' ? 'minus' : ''}">
                ${item.amount > 0 ? '+' : item.amount < 0 ? '' : ''}${item.amount ? pounds(item.amount) : ''}
              </div>
            </div>
          `).join('') : '<div class="history-item"><div>No history yet</div></div>'}
        </div>
      `;


      document.getElementById('backToPlayers').addEventListener('click', () => {
        state.selectedPlayerId = null;
        saveState();
        detailView.classList.add('hidden');
      });


      detailView.querySelectorAll('.delta-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          applyDelta(player.id, Number(btn.dataset.amount));
        });
      });


      document.getElementById('collect200Btn').addEventListener('click', () => applyDelta(player.id, 200, 'Collected £200'));
      document.getElementById('pay50Btn').addEventListener('click', () => applyDelta(player.id, -50, 'Paid £50'));
      document.getElementById('customAddBtn').addEventListener('click', () => {
        const amount = Number(document.getElementById('customAmount').value);
        if (!amount || amount <= 0) {
          alert('Enter a valid custom amount.');
          return;
        }
        applyDelta(player.id, amount, `Added ${pounds(amount)}`);
      });
      document.getElementById('customSubtractBtn').addEventListener('click', () => {
        const amount = Number(document.getElementById('customAmount').value);
        if (!amount || amount <= 0) {
          alert('Enter a valid custom amount.');
          return;
        }
        applyDelta(player.id, -amount, `Subtracted ${pounds(amount)}`);
      });
      document.getElementById('transferBtn').addEventListener('click', () => {
        const toId = document.getElementById('transferTo').value;
        const amount = Number(document.getElementById('transferAmount').value);
        if (!toId || !amount || amount <= 0) {
          alert('Enter a valid transfer amount.');
          return;
        }
        transferMoney(player.id, toId, amount);
      });
    }


    function applyDelta(playerId, amount, customLabel) {
      const player = state.players.find(p => p.id === playerId);
      if (!player) return;
      player.balance += amount;
      player.history.push({
        label: customLabel || (amount >= 0 ? 'Added money' : 'Subtracted money'),
        amount,
        type: amount >= 0 ? 'plus' : 'minus',
        time: nowString(),
      });
      saveState();
      renderPlayers();
      renderDetail(playerId);
    }


    function transferMoney(fromId, toId, amount) {
      const from = state.players.find(p => p.id === fromId);
      const to = state.players.find(p => p.id === toId);
      if (!from || !to) return;
      from.balance -= amount;
      to.balance += amount;
      from.history.push({ label: `Sent to ${to.name}`, amount: -amount, type: 'minus', time: nowString() });
      to.history.push({ label: `Received from ${from.name}`, amount: amount, type: 'plus', time: nowString() });
      saveState();
      renderPlayers();
      renderDetail(fromId);
    }


    function resetBalances() {
      const ok = confirm(`Reset all player balances back to ${pounds(state.startingBalance)}?`);
      if (!ok) return;
      state.players = state.players.map(player => ({
        ...player,
        balance: state.startingBalance,
        history: [...(player.history || []), { label: `Balances reset to ${pounds(state.startingBalance)}`, amount: 0, type: 'neutral', time: nowString() }],
      }));
      saveState();
      renderPlayers();
      if (state.selectedPlayerId && state.players.some(p => p.id === state.selectedPlayerId)) {
        renderDetail(state.selectedPlayerId);
      } else {
        detailView.classList.add('hidden');
      }
    }


    function startNewGame() {
      const ok = confirm('Start a new game setup? This will take you back to setup. Your current saved game will only be replaced when you tap Start Game on the setup screen.');
      if (!ok) return;


      state.selectedPlayerId = null;
      detailView.classList.add('hidden');


      draftPlayers = state.players.length
        ? state.players.map((p, i) => ({
            id: makeId(),
            name: p.name || `Player ${i + 1}`,
            emoji: p.emoji || emojis[i % emojis.length],
            color: p.color || palette[i % palette.length],
          }))
        : Array.from({ length: 4 }, (_, i) => makeDraftPlayer(i));


      startingBalanceInput.value = state.startingBalance || 1500;
      renderSetupPlayers();
      showSetupUI();
    }


    addPlayerBtn.addEventListener('click', addDraftPlayer);
    startGameBtn.addEventListener('click', () => {
      if (createPlayersFromSetup()) showGameUI();
    });
    resetBtn.addEventListener('click', resetBalances);
    newGameBtn.addEventListener('click', startNewGame);
    loadSavedBtn.addEventListener('click', () => {
      if (loadState()) {
        draftPlayers = [];
        showGameUI();
      } else {
        alert('No saved game was found on this device.');
        loadSavedBtn.classList.add('hidden');
      }
    });


    if (loadState() && state.gameStarted) {
      loadSavedBtn.classList.remove('hidden');
      showGameUI();
    } else {
      resetDraftPlayers(4);
      showSetupUI();
    }
  </script>
</body>
</html>

