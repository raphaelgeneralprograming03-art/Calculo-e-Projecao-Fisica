
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Memorial de Cálculo e Projeção Física - Sistema DSMAC/ATR (Rafael Silva)</title>
    <style>
        :root {
            --bg-color: #07090f;
            --panel-bg: #0d131f;
            --accent-green: #10b981;
            --accent-amber: #f59e0b;
            --accent-red: #ef4444;
            --accent-blue: #3b82f6;
            --text-main: #f3f4f6;
            --text-muted: #9ca3af;
            --border-color: #1e293b;
        }
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Courier New', Courier, monospace;
        }
        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            padding: 15px;
        }
        header {
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            padding: 15px 20px;
            margin-bottom: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-radius: 6px;
        }
        header h1 {
            font-size: 1.05rem;
            color: var(--accent-green);
            letter-spacing: 1px;
        }
        .badge {
            background: rgba(16, 185, 129, 0.12);
            color: var(--accent-green);
            border: 1px solid var(--accent-green);
            padding: 6px 12px;
            border-radius: 4px;
            font-size: 0.75rem;
            font-weight: bold;
        }
        .main-container {
            display: grid;
            grid-template-columns: 1fr 420px;
            gap: 15px;
            flex: 1;
        }
        @media(max-width: 1100px) {
            .main-container {
                grid-template-columns: 1fr;
            }
        }
        .panel {
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        .panel-title {
            font-size: 0.85rem;
            color: var(--accent-amber);
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 8px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        .overview-box {
            font-size: 0.76rem;
            line-height: 1.4;
            color: var(--text-muted);
            background: #090d16;
            padding: 10px;
            border-radius: 4px;
            border-left: 3px solid var(--accent-blue);
        }
        .formula-box {
            font-size: 0.74rem;
            background: #04060a;
            border: 1px solid var(--border-color);
            padding: 10px;
            border-radius: 4px;
            color: var(--accent-green);
            white-space: pre-wrap;
            line-height: 1.35;
        }
        .canvas-wrapper {
            position: relative;
            background: #020408;
            border: 1px solid var(--border-color);
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 4px;
            flex: 1;
            min-height: 300px;
        }
        canvas {
            display: block;
            cursor: crosshair;
        }
        .controls-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
        }
        button {
            background: #1e293b;
            color: var(--text-main);
            border: 1px solid var(--border-color);
            padding: 9px 12px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.78rem;
            transition: all 0.2s;
        }
        button:hover {
            background: #334155;
            border-color: var(--accent-green);
            color: var(--accent-green);
        }
        button.active {
            background: rgba(16, 185, 129, 0.2);
            border-color: var(--accent-green);
            color: var(--accent-green);
        }
        .slider-group {
            display: flex;
            flex-direction: column;
            gap: 4px;
            font-size: 0.75rem;
        }
        .slider-group label {
            display: flex;
            justify-content: space-between;
            color: var(--text-muted);
        }
        input[type="range"] {
            width: 100%;
            accent-color: var(--accent-amber);
            cursor: pointer;
        }
        .telemetry-box {
            background: #04060a;
            border: 1px solid var(--border-color);
            padding: 10px;
            border-radius: 4px;
            font-size: 0.78rem;
            display: flex;
            flex-direction: column;
            gap: 5px;
        }
        .telemetry-row {
            display: flex;
            justify-content: space-between;
        }
        .serial-console {
            background: #020305;
            color: var(--accent-green);
            border: 1px solid var(--border-color);
            padding: 8px;
            border-radius: 4px;
            font-size: 0.69rem;
            height: 90px;
            overflow-y: auto;
            white-space: pre-wrap;
        }
    </style>
</head>
<body>

    <header>
        <div>
            <h1>VISÃO GERAL DO APLICATIVO & MEMORIAL DE CÁLCULO</h1>
            <p style="font-size: 0.7rem; color: var(--text-muted);">Simulação de Correção Vetorial ATR & Controlador PID de Borda Local</p>
        </div>
        <div class="badge">SISTEMA: RAFAEL SILVA</div>
    </header>

    <div class="main-container">
        <!-- Coluna Esquerda: Visão Geral e Fórmulas do Memorial -->
        <div class="panel">
            <div class="panel-title">Visão Geral do Aplicativo & Memorial Descritivo</div>
            <div class="overview-box">
                <strong>Visão Geral:</strong> Este aplicativo simula a camada de processamento de alta velocidade de bordo desenvolvida por Rafael Silva. O sistema processa o rastreamento visual tridimensional/óptico por correlação de pixels, convertendo o erro de enquadramento instantâneo em comandos de atuação servomecânica de precisão nanométrica.
            </div>

            <div class="panel-title" style="margin-top: 4px;">Fórmula de Correção Vetorial (ATR Local & PID)</div>
            <div class="formula-box">E(x) = X_alvo - X_centro
E(y) = Y_alvo - Y_centro

w(t) = Kp*E(t) + Ki*∫E(t)dt + Kd*(dE(t)/dt)

[Edge Processing] Taxa Mínima: 60 FPS
[Latencia Tática] T_latencia < 16ms (Zero-Lag)</div>

            <div class="panel-title" style="margin-top: 4px;">Ajustes de Ganho PID em Tempo Real</div>
            <div class="slider-group">
                <label><span>Ganho Proporcional (Kp):</span> <span id="lblKp">0.15</span></label>
                <input type="range" id="rangeKp" min="0.05" max="0.40" step="0.01" value="0.15">
            </div>
            <div class="slider-group">
                <label><span>Ganho Integral (Ki):</span> <span id="lblKi">0.01</span></label>
                <input type="range" id="rangeKi" min="0.00" max="0.05" step="0.002" value="0.01">
            </div>
            <div class="slider-group">
                <label><span>Ganho Derivativo (Kd):</span> <span id="lblKd">0.05</span></label>
                <input type="range" id="rangeKd" min="0.01" max="0.15" step="0.005" value="0.05">
            </div>

            <div class="controls-grid" style="margin-top: 5px;">
                <button id="btnEvasive" class="active">Manobra Evasiva: Ativa</button>
                <button id="btnReset">Resetar Posição do Alvo</button>
            </div>
        </div>

        <!-- Coluna Direita: Simulador Canvas & Telemetria PID -->
        <div class="panel">
            <div class="panel-title">Simulador Gráfico (Canvas 2D - 60 FPS)</div>
            <div class="canvas-wrapper">
                <canvas id="simCanvas" width="520" height="300"></canvas>
            </div>

            <div class="panel-title">Telemetria & Desempenho Físico</div>
            <div class="telemetry-box">
                <div class="telemetry-row"><span>Erro Espacial E(x, y):</span> <span id="valError" style="color:var(--accent-amber)">X: 0 px | Y: 0 px</span></div>
                <div class="telemetry-row"><span>Velocidade Angular w(t):</span> <span id="valOmega" style="color:var(--accent-green)">0.00 rad/s</span></div>
                <div class="telemetry-row"><span>Taxa de Processamento:</span> <span style="color:var(--accent-green)">60.0 FPS (Borda Local)</span></div>
                <div class="telemetry-row"><span>Latencia Tática (T_lat):</span> <span id="valLatency" style="color:var(--accent-green)">12.4 ms (&lt; 16ms)</span></div>
            </div>

            <div class="panel-title">Log de Processamento do Barramento</div>
            <div class="serial-console" id="serialConsole">[INIT] Placa de borda inicializada a 60 FPS.
[PID] Controladores Kp, Ki, Kd calibrados com sucesso.
[ATR] Rastreamento óptico por correlação de pixels ativo.</div>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('simCanvas');
        const ctx = canvas.getContext('2d');

        // Configurações do PID e Estado
        let Kp = 0.15;
        let Ki = 0.01;
        let Kd = 0.05;
        let evasiveActive = true;

        // Alvo e Sistema de Seguimento
        let target = { x: 380, y: 150, vx: 2.2, vy: 1.5, radius: 10 };
        let tracker = { x: 260, y: 150 };

        // Variáveis de Estado do PID
        let integralX = 0, integralY = 0;
        let prevErrorX = 0, prevErrorY = 0;

        // Elementos da UI
        const rangeKp = document.getElementById('rangeKp');
        const rangeKi = document.getElementById('rangeKi');
        const rangeKd = document.getElementById('rangeKd');
        const lblKp = document.getElementById('lblKp');
        const lblKi = document.getElementById('lblKi');
        const lblKd = document.getElementById('lblKd');
        const btnEvasive = document.getElementById('btnEvasive');
        const btnReset = document.getElementById('btnReset');
        const valError = document.getElementById('valError');
        const valOmega = document.getElementById('valOmega');
        const valLatency = document.getElementById('valLatency');
        const serialConsole = document.getElementById('serialConsole');

        function logMsg(msg) {
            serialConsole.textContent += "\n" + msg;
            serialConsole.scrollTop = serialConsole.scrollHeight;
        }

        rangeKp.addEventListener('input', (e) => { Kp = parseFloat(e.target.value); lblKp.textContent = Kp.toFixed(2); });
        rangeKi.addEventListener('input', (e) => { Ki = parseFloat(e.target.value); lblKi.textContent = Ki.toFixed(3); });
        rangeKd.addEventListener('input', (e) => { Kd = parseFloat(e.target.value); lblKd.textContent = Kd.toFixed(3); });

        btnEvasive.addEventListener('click', () => {
            evasiveActive = !evasiveActive;
            btnEvasive.classList.toggle('active', evasiveActive);
            logMsg(evasiveActive ? "[MANOBRA] Alvo executando desvios cinéticos evasivos." : "[MANOBRA] Alvo operando em trajetória linear constante.");
        });

        btnReset.addEventListener('click', () => {
            target.x = 320 + (Math.random() - 0.5) * 100;
            target.y = 150 + (Math.random() - 0.5) * 80;
            integralX = 0; integralY = 0;
            logMsg("[SYSTEM] Posições resetadas no espaço tridimensional.");
        });

        function update() {
            // Movimentação evasiva do alvo
            if (evasiveActive) {
                target.vx += (Math.random() - 0.5) * 0.4;
                target.vy += (Math.random() - 0.5) * 0.4;
                target.vx = Math.max(-3.5, Math.min(3.5, target.vx));
                target.vy = Math.max(-3.5, Math.min(3.5, target.vy));
            }
            target.x += target.vx;
            target.y += target.vy;

            // Limites do canvas para o alvo
            if (target.x < 30 || target.x > canvas.width - 30) target.vx *= -1;
            if (target.y < 30 || target.y > canvas.height - 30) target.vy *= -1;

            // Centro óptico da tela (X_centro, Y_centro)
            const centerX = canvas.width / 2;
            const centerY = canvas.height / 2;

            // Cálculo do Erro Vetorial E(x, y)
            const errorX = target.x - tracker.x;
            const errorY = target.y - tracker.y;

            // Cálculo do PID para velocidade angular w(t)
            integralX += errorX;
            integralY += errorY;
            const derivativeX = errorX - prevErrorX;
            const derivativeY = errorY - prevErrorY;

            const omegaX = Kp * errorX + Ki * integralX + Kd * derivativeX;
            const omegaY = Kp * errorY + Ki * integralY + Kd * derivativeY;

            prevErrorX = errorX;
            prevErrorY = errorY;

            // Atualização da posição do rastreador mecânico baseada no PID
            tracker.x += omegaX;
            tracker.y += omegaY;

            // Telemetria visual
            valError.textContent = `X: ${Math.round(errorX)} px | Y: ${Math.round(errorY)} px`;
            const omegaMagnitude = Math.sqrt(omegaX * omegaX + omegaY * omegaY).toFixed(2);
            valOmega.textContent = `${omegaMagnitude} rad/s`;

            // Simulação de latência flutuante ultra-baixa (< 16ms)
            const currentLatency = (12.0 + Math.random() * 3.5).toFixed(1);
            valLatency.textContent = `${currentLatency} ms (< 16ms)`;
        }

        function draw() {
            ctx.fillStyle = '#020408';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Grade tática
            ctx.strokeStyle = '#101826';
            ctx.lineWidth = 1;
            for (let x = 0; x < canvas.width; x += 40) {
                ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
            }
            for (let y = 0; y < canvas.height; y += 40) {
                ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
            }

            // Cruz central óptica (X_centro, Y_centro)
            ctx.strokeStyle = 'rgba(16, 185, 129, 0.4)';
            ctx.lineWidth = 1;
            ctx.beginPath();
            ctx.moveTo(canvas.width/2, 0); ctx.lineTo(canvas.width/2, canvas.height);
            ctx.moveTo(0, canvas.height/2); ctx.lineTo(canvas.width, canvas.height/2);
            ctx.stroke();

            // Desenhar Alvo Móvel
            ctx.fillStyle = '#f59e0b';
            ctx.beginPath();
            ctx.arc(target.x, target.y, target.radius, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillStyle = '#ffffff';
            ctx.font = '9px Courier New';
            ctx.fillText("ALVO ATR", target.x - 22, target.y - 14);

            // Desenhar Rastreador / Visão Óptica Ajustada pelo PID
            ctx.strokeStyle = '#10b981';
            ctx.lineWidth = 2;
            ctx.strokeRect(tracker.x - 22, tracker.y - 22, 44, 44);

            ctx.fillStyle = '#10b981';
            ctx.fillText("MECANISMO PID", tracker.x - 30, tracker.y + 32);

            // Linha vetorial de erro E(x, y)
            ctx.strokeStyle = 'rgba(245, 158, 11, 0.6)';
            ctx.setLineDash([3, 3]);
            ctx.beginPath();
            ctx.moveTo(tracker.x, tracker.y);
            ctx.lineTo(target.x, target.y);
            ctx.stroke();
            ctx.setLineDash([]);
        }

        function loop() {
            update();
            draw();
            requestAnimationFrame(loop);
        }

        requestAnimationFrame(loop);
    </script>
</body>
</html>
