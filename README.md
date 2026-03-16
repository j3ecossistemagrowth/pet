<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de ROI — Marketing Pet</title>
    <style>
        /* RESET E BASE */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background-color: #0f1117;
            color: #e2e8f0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        /* CONTAINER PRINCIPAL */
        .app-container {
            width: 100%;
            max-width: 1000px;
            background: #1a1d27;
            border-radius: 16px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4);
            overflow: hidden;
            border: 1px solid #2d313d;
        }

        /* CABEÇALHO */
        header {
            padding: 30px;
            border-bottom: 1px solid #2d313d;
            background: #161922;
        }

        header h1 {
            font-size: 24px;
            font-weight: 700;
            color: #ffffff;
            margin-bottom: 6px;
        }

        header p {
            font-size: 14px;
            color: #94a3b8;
        }

        /* CONTEÚDO (GRID) */
        .content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 0;
        }

        @media (max-width: 768px) {
            .content {
                grid-template-columns: 1fr;
            }
        }

        /* COLUNA DE ENTRADAS */
        .inputs-section {
            padding: 30px;
            background: #1a1d27;
            border-right: 1px solid #2d313d;
        }

        .input-group {
            margin-bottom: 24px;
        }

        .input-group label {
            display: block;
            font-size: 13px;
            font-weight: 600;
            color: #94a3b8;
            margin-bottom: 8px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .input-wrapper {
            position: relative;
        }

        .input-wrapper span.prefix {
            position: absolute;
            left: 14px;
            top: 50%;
            transform: translateY(-50%);
            color: #64748b;
            font-weight: 500;
        }

        input[type="number"], input[type="text"] {
            width: 100%;
            background: #0f1117;
            border: 1px solid #334155;
            border-radius: 8px;
            padding: 12px 14px 12px 40px;
            color: #fff;
            font-size: 16px;
            outline: none;
            transition: border-color 0.2s;
        }

        input[type="number"]:focus {
            border-color: #22c55e;
        }

        /* SLIDER PERSONALIZADO */
        .slider-container {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-top: 10px;
        }

        input[type="range"] {
            flex-grow: 1;
            cursor: pointer;
            accent-color: #22c55e;
        }

        .percent-display {
            font-weight: 700;
            color: #22c55e;
            font-size: 18px;
            min-width: 45px;
        }

        /* COLUNA DE RESULTADOS */
        .results-section {
            padding: 30px;
            background: #161922;
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
        }

        .results-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 16px;
        }

        .result-card {
            background: #1a1d27;
            padding: 20px;
            border-radius: 12px;
            border: 1px solid #2d313d;
            transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275), border-color 0.3s;
        }

        .result-card.full-width {
            grid-column: span 2;
        }

        .result-card h3 {
            font-size: 12px;
            color: #94a3b8;
            margin-bottom: 10px;
            text-transform: uppercase;
        }

        .result-value {
            font-size: 22px;
            font-weight: 700;
            color: #fff;
            transition: color 0.3s;
        }

        /* DINÂMICA DE CORES ROI */
        .roi-positive { color: #22c55e; }
        .roi-neutral { color: #eab308; }
        .roi-negative { color: #ef4444; }

        /* MENSAGEM DE ERRO/AVISO */
        #empty-warning {
            grid-column: span 2;
            padding: 40px;
            text-align: center;
            color: #64748b;
            font-style: italic;
            border: 2px dashed #2d313d;
            border-radius: 12px;
        }

        /* RODAPÉ */
        footer {
            padding: 20px 30px;
            background: #11141d;
            border-top: 1px solid #2d313d;
            text-align: center;
        }

        footer p {
            font-size: 11px;
            color: #475569;
        }

        /* ANIMAÇÃO DE NÚMEROS */
        .updating {
            transform: scale(1.05);
        }
    </style>
</head>
<body>

<div class="app-container">
    <header>
        <h1>Calculadora de ROI — Marketing Pet</h1>
        <p>Simule o retorno do seu investimento em tráfego pago</p>
    </header>

    <div class="content">
        <!-- ENTRADAS -->
        <section class="inputs-section">
            <div class="input-group">
                <label for="faturamento">Faturamento Mensal Atual</label>
                <div class="input-wrapper">
                    <span class="prefix">R$</span>
                    <input type="number" id="faturamento" placeholder="30.000" oninput="calculate()">
                </div>
            </div>

            <div class="input-group">
                <label for="investimento">Investimento em Tráfego</label>
                <div class="input-wrapper">
                    <span class="prefix">R$</span>
                    <input type="number" id="investimento" placeholder="1.500" oninput="calculate()">
                </div>
            </div>

            <div class="input-group">
                <label for="cpl">CPL Médio (Custo por Lead)</label>
                <div class="input-wrapper">
                    <span class="prefix">R$</span>
                    <input type="number" id="cpl" value="8" step="0.50" oninput="calculate()">
                </div>
            </div>

            <div class="input-group">
                <label for="conversao">Taxa de Conversão: <span id="conversao-label">10%</span></label>
                <div class="slider-container">
                    <input type="range" id="conversao" min="1" max="30" value="10" oninput="updateSliderLabel(); calculate();">
                    <div class="percent-display"><span id="conversao-val">10</span>%</div>
                </div>
            </div>

            <div class="input-group">
                <label for="ticket">Ticket Médio (Venda)</label>
                <div class="input-wrapper">
                    <span class="prefix">R$</span>
                    <input type="number" id="ticket" placeholder="180" oninput="calculate()">
                </div>
            </div>
        </section>

        <!-- RESULTADOS -->
        <section class="results-section">
            <div id="results-container" class="results-grid" style="display: none;">
                <div class="result-card">
                    <h3>Novos Clientes</h3>
                    <div id="res-clientes" class="result-value">0</div>
                </div>

                <div class="result-card">
                    <h3>Payback Est.</h3>
                    <div id="res-payback" class="result-value">0 dias</div>
                </div>

                <div class="result-card full-width">
                    <h3>Receita Adicional</h3>
                    <div id="res-receita" class="result-value">R$ 0,00</div>
                </div>

                <div class="result-card full-width">
                    <h3>ROI Projetado</h3>
                    <div id="res-roi" class="result-value">0%</div>
                </div>
            </div>

            <div id="empty-warning">
                Insira um valor de investimento para simular o retorno.
            </div>
        </section>
    </div>

    <footer>
        <p>Valores estimados com base em benchmarks do setor pet brasileiro. O sucesso real depende da qualidade do atendimento interno e operação de vendas.</p>
    </footer>
</div>

<script>
    function formatCurrency(value) {
        return new Intl.NumberFormat('pt-BR', {
            style: 'currency',
            currency: 'BRL',
        }).format(value);
    }

    function updateSliderLabel() {
        const val = document.getElementById('conversao').value;
        document.getElementById('conversao-val').innerText = val;
        document.getElementById('conversao-label').innerText = val + '%';
    }

    function calculate() {
        // Obter valores
        const investimento = parseFloat(document.getElementById('investimento').value) || 0;
        const cpl = parseFloat(document.getElementById('cpl').value) || 0;
        const conversaoPercent = parseFloat(document.getElementById('conversao').value) || 0;
        const ticket = parseFloat(document.getElementById('ticket').value) || 0;

        const resultsGrid = document.getElementById('results-container');
        const emptyWarning = document.getElementById('empty-warning');

        if (investimento <= 0) {
            resultsGrid.style.display = 'none';
            emptyWarning.style.display = 'block';
            return;
        }

        resultsGrid.style.display = 'grid';
        emptyWarning.style.display = 'none';

        // Lógica de cálculo
        const leads = cpl > 0 ? (investimento / cpl) : 0;
        const novosClientes = Math.floor(leads * (conversaoPercent / 100));
        const receitaAdicional = novosClientes * ticket;
        const lucroBrutoMidia = receitaAdicional - investimento;
        
        let roi = 0;
        if (investimento > 0) {
            roi = (lucroBrutoMidia / investimento) * 100;
        }

        let paybackDias = 0;
        if (receitaAdicional > 0) {
            paybackDias = Math.ceil((investimento / receitaAdicional) * 30);
        }

        // Atualizar UI com animações
        updateValue('res-clientes', novosClientes);
        updateValue('res-receita', formatCurrency(receitaAdicional));
        updateValue('res-roi', roi.toFixed(0) + '%');
        updateValue('res-payback', paybackDias + (paybackDias === 1 ? ' dia' : ' dias'));

        // Lógica de cores para ROI
        const roiElement = document.getElementById('res-roi');
        roiElement.classList.remove('roi-positive', 'roi-neutral', 'roi-negative');
        if (roi >= 200) {
            roiElement.classList.add('roi-positive');
        } else if (roi >= 100) {
            roiElement.classList.add('roi-neutral');
        } else {
            roiElement.classList.add('roi-negative');
        }
    }

    function updateValue(id, newValue) {
        const el = document.getElementById(id);
        if (el.innerText !== String(newValue)) {
            el.classList.add('updating');
            el.innerText = newValue;
            setTimeout(() => el.classList.remove('updating'), 300);
        }
    }

    // Inicialização
    window.onload = calculate;
</script>

</body>
</html>
