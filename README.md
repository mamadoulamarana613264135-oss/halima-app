<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HalimaEntreprise App</title>
    
    <!-- Configuration Mode Application Mobile -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="HalimaApp">
    <link rel="manifest" href="data:application/manifest+json,{%22name%22:%22HalimaEntreprise%20App%22,%22short_name%22:%22HalimaApp%22,%22start_url%22:%22.%22,%22display%22:%22standalone%22,%22background_color%22:%22%23f8fafc%22,%22theme_color%22:%22%230284c7%22}">

    <style>
        :root { --primary: #0284c7; --primary-hover: #0369a1; --bg: #f8fafc; --text: #1e293b; }
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background: var(--bg); color: var(--text); margin: 0; padding: 10px; padding-top: env(safe-area-inset-top); }
        .container { max-width: 100%; margin: 0 auto; background: white; padding: 15px; border-radius: 8px; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
        h1, h2 { color: #0f172a; margin-top: 0; font-size: 1.2rem; }
        .grid { display: grid; grid-template-columns: 1fr; gap: 10px; margin-bottom: 15px; }
        @media(min-width: 600px) { .grid { grid-template-columns: 1fr 1fr; } }
        .form-group { display: flex; flex-direction: column; margin-bottom: 10px; }
        label { font-weight: 600; margin-bottom: 3px; font-size: 12px; color: #64748b; }
        input, select { padding: 12px; border: 1px solid #cbd5e1; border-radius: 6px; font-size: 14px; background: #fff; -webkit-appearance: none; }
        .table-responsive { overflow-x: auto; margin-top: 15px; }
        table { width: 100%; border-collapse: collapse; min-width: 500px; }
        th, td { padding: 10px; border: 1px solid #e2e8f0; text-align: left; font-size: 13px; }
        th { background: #f1f5f9; }
        input.table-input { width: 90%; padding: 5px; border: 1px solid #cbd5e1; border-radius: 4px; font-size: 13px; }
        .btn { background: var(--primary); color: white; border: none; padding: 14px; border-radius: 6px; cursor: pointer; font-weight: 600; font-size: 14px; width: 100%; margin-top: 10px; box-shadow: 0 2px 4px rgba(2, 132, 199, 0.2); }
        .btn:hover { background: var(--primary-hover); }
        .btn-danger { background: #ef4444; padding: 5px 10px; width: auto; font-size: 12px; box-shadow: none; }
        .btn-danger:hover { background: #dc2626; }
        .totals { margin-top: 20px; background: #f8fafc; padding: 15px; border-radius: 6px; border: 1px solid #e2e8f0; }
        .total-row { display: flex; justify-content: space-between; margin-bottom: 8px; font-size: 14px; }
        .total-row.grand-total { font-weight: bold; font-size: 16px; border-top: 2px solid #cbd5e1; padding-top: 8px; color: #0f172a; }
        .sit-col { display: none; }
        @media print {
            body { background: white; padding: 0; }
            .container { box-shadow: none; padding: 0; }
            .no-print, button, .btn-danger, th:last-child, td:last-child { display: none !important; }
            input, select { border: none; background: transparent; padding: 0; font-size: 14px; color: black; pointer-events: none; }
        }
    </style>
</head>
<body>

<div class="container">
    <h1 class="no-print" style="text-align: center; color: var(--primary); font-size: 1.4rem; margin-bottom: 20px;">HalimaEntreprise</h1>
    
    <!-- Infos Entreprise & Client -->
    <div class="grid">
        <div>
            <h2>Votre Entreprise</h2>
            <div class="form-group"><label>Nom de l'entreprise</label><input type="text" value="HalimaEntreprise"></div>
            <div class="form-group"><label>Adresse professionnelle</label><input type="text" placeholder="Votre adresse"></div>
            <div class="form-group"><label>Téléphone</label><input type="tel" placeholder="Votre numéro"></div>
            <div class="form-group"><label>E-mail</label><input type="email" placeholder="Votre email"></div>
        </div>
        <div>
            <h2>Client & Chantier</h2>
            <div class="form-group"><label>Nom du Client</label><input type="text" placeholder="Nom du client"></div>
            <div class="form-group"><label>Adresse du client</label><input type="text" placeholder="Adresse du client"></div>
            <div class="form-group"><label>Nom du Chantier</label><input type="text" value="Chantier sanitaire"></div>
        </div>
    </div>

    <!-- Infos Document -->
    <div class="grid">
        <div class="form-group">
            <label>Type de document</label>
            <select id="docType" onchange="toggleSituationColumn()">
                <option value="devis" selected>Devis Initial</option>
                <option value="situation">Facture de Situation (Avancement)</option>
            </select>
        </div>
        <div class="form-group">
            <label>Numéro de document</label>
            <input type="text" id="docNum" value="DEV-2026-001">
        </div>
    </div>

    <!-- Tableau de travaux vide au démarrage -->
    <h2>Détail des Prestations</h2>
    <div class="table-responsive">
        <table id="itemsTable">
            <thead>
                <tr>
                    <th>Désignation</th>
                    <th style="width: 50px;">Qté</th>
                    <th style="width: 90px;">P.U. HT (€)</th>
                    <th class="sit-col" style="width: 70px;">Avancement (%)</th>
                    <th style="width: 100px;">Prix HT (€)</th>
                    <th class="no-print" style="width: 40px;">Action</th>
                </tr>
            </thead>
            <tbody id="tableBody">
                <!-- Les lignes s'ajouteront ici avec le bouton -->
            </tbody>
        </table>
    </div>
    
    <button class="btn no-print" style="background: #64748b;" onclick="addRow()">+ Ajouter une ligne de prestation</button>

    <!-- Section des calculs et totaux -->
    <div class="totals">
        <div class="total-row"><span>Total Général Devis HT :</span><span id="totalDevis">0.00 €</span></div>
        <div class="total-row situation-view" style="display:none; font-weight: bold; color: var(--primary);"><span>Total Situation Actuelle HT :</span><span id="totalSituation">0.00 €</span></div>
        <div class="total-row">
            <span>TVA :</span>
            <select id="tvaRate" onchange="calculateTotals()" style="padding: 2px; font-size: 13px;">
                <option value="20">20 % (Normal)</option>
                <option value="10" selected>10 % (Rénovation)</option>
                <option value="5.5">5.5 % (Énergie)</option>
            </select>
        </div>
        <div class="total-row situation-view" style="display:none;"><span>TVA sur Situation :</span><span id="tvaAmount">0.00 €</span></div>
        <div class="total-row grand-total"><span id="lblTotal">Total TTC :</span><span id="grandTotal">0.00 €</span></div>
    </div>

    <!-- Bouton d'action principal -->
    <button class="btn no-print" onclick="window.print()">Créer le PDF / Imprimer</button>
</div>

<script>
    if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('data:text/javascript,self.addEventListener(%22fetch%22,function(e){})');
    }

    // Ajouter une première ligne vide automatiquement au chargement de l'application
    window.onload = function() {
        addRow();
    };

    function toggleSituationColumn() {
        const type = document.getElementById('docType').value;
        const sitCols = document.querySelectorAll('.sit-col');
        const sitViews = document.querySelectorAll('.situation-view');
        const docNum = document.getElementById('docNum');
        
        if (type === 'situation') {
            sitCols.forEach(el => el.style.display = 'table-cell');
            sitViews.forEach(el => el.style.display = 'flex');
            docNum.value = "SIT-2026-001";
            document.getElementById('lblTotal').innerText = "Total Situation TTC :";
        } else {
            sitCols.forEach(el => el.style.display = 'none');
            sitViews.forEach(el => el.style.display = 'none');
            docNum.value = "DEV-2026-001";
            document.getElementById('lblTotal').innerText = "Total TTC :";
        }
        calculateTotals();
    }

    function addRow() {
        const tbody = document.getElementById('tableBody');
        const isSituation = document.getElementById('docType').value === 'situation';
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td><input type="text" class="table-input" placeholder="Ex: Pose lavabo, peinture..."></td>
            <td><input type="number" class="table-input qte" value="1" oninput="calculateTotals()"></td>
            <td><input type="number" class="table-input pu" value="0" oninput="calculateTotals()"></td>
            <td class="sit-col" style="${isSituation ? 'display:table-cell;' : ''}"><input type="number" class="table-input avancement" value="100" min="0" max="100" oninput="calculateTotals()"></td>
            <td class="row-total">0.00</td>
            <td class="no-print"><button class="btn btn-danger" onclick="deleteRow(this)">X</button></td>
        `;
        tbody.appendChild(tr);
        calculateTotals();
    }

    function deleteRow(btn) {
        btn.closest('tr').remove();
        calculateTotals();
    }

    function calculateTotals() {
        const rows = document.querySelectorAll('#tableBody tr');
        const type = document.getElementById('docType').value;
        const tvaRate = parseFloat(document.getElementById('tvaRate').value) / 100;
        
        let globalDevisHT = 0;
        let globalSituationHT = 0;

        rows.forEach(row => {
