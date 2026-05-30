<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HalimaEntreprise App</title>
    
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="HalimaApp">
    <link rel="manifest" href="data:application/manifest+json,{%22name%22:%22HalimaEntreprise%20App%22,%22short_name%22:%22HalimaApp%22,%22start_url%22:%22.%22,%22display%22:%22standalone%22,%22background_color%22:%22%23f8fafc%22,%22theme_color%22:%22%230284c7%22}">

    <style>
        :root { --primary: #0284c7; --bg: #f8fafc; --text: #1e293b; }
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background: var(--bg); color: var(--text); margin: 0; padding: 10px; padding-top: env(safe-area-inset-top); }
        .container { max-width: 100%; margin: 0 auto; background: white; padding: 15px; border-radius: 8px; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
        h1, h2 { color: #0f172a; margin-top: 0; font-size: 1.2rem; }
        .grid { display: grid; grid-template-columns: 1fr; gap: 10px; margin-bottom: 15px; }
        @media(min-width: 600px) { .grid { grid-template-columns: 1fr 1fr; } }
        .form-group { display: flex; flex-direction: column; margin-bottom: 10px; }
        label { font-weight: 600; margin-bottom: 3px; font-size: 12px; color: #64748b; }
        input, select { padding: 12px; border: 1px solid #cbd5e1; border-radius: 6px; font-size: 14px; background: #fff; -webkit-appearance: none; }
        .table-responsive { overflow-x: auto; margin-top: 15px; }
        
        table { width: 100%; border-collapse: collapse; min-width: 500px; margin-bottom: 15px; }
        th, td { padding: 12px; border: 1px solid #cbd5e1; text-align: left; font-size: 14px; }
        th { background: #f1f5f9; font-weight: bold; color: #0f172a; }
        input.table-input { width: 95%; padding: 8px; border: 1px solid #cbd5e1; border-radius: 4px; font-size: 14px; }
        .row-total { font-weight: bold; color: #0f172a; }
        
        .btn { background: var(--primary); color: white; border: none; padding: 14px; border-radius: 6px; cursor: pointer; font-weight: 600; font-size: 14px; width: 100%; margin-top: 10px; display: block; text-align: center; }
        .btn-danger { background: #ef4444; padding: 6px 10px; font-size: 12px; width: auto; margin: 0; }
        .totals { margin-top: 20px; background: #f8fafc; padding: 15px; border-radius: 6px; border: 1px solid #e2e8f0; }
        .total-row { display: flex; justify-content: space-between; margin-bottom: 8px; font-size: 14px; }
        .total-row.grand-total { font-weight: bold; font-size: 16px; border-top: 2px solid #cbd5e1; padding-top: 8px; color: #0f172a; }
        @media print {
            body { background: white; padding: 0; }
            .container { box-shadow: none; padding: 0; }
            .no-print, button { display: none !important; }
            input, select { border: none; background: transparent; padding: 0; font-size: 14px; color: black; pointer-events: none; }
        }
    </style>
</head>
<body>

<div class="container">
    <h1 class="no-print" style="text-align: center; color: var(--primary); font-size: 1.4rem; margin-bottom: 20px;">HalimaEntreprise</h1>
    
    <div class="grid">
        <div>
            <h2>Votre Entreprise</h2>
            <div class="form-group"><label>Nom</label><input type="text" value="HalimaEntreprise"></div>
            <div class="form-group"><label>Adresse & Contact</label><input type="text" placeholder="Votre adresse et téléphone"></div>
        </div>
        <div>
            <h2>Client & Chantier</h2>
            <div class="form-group"><label>Client</label><input type="text" placeholder="Nom du client"></div>
            <div class="form-group"><label>Chantier</label><input type="text" value="Chantier sanitaire"></div>
        </div>
    </div>

    <h2>Détail des Prestations</h2>
    <div class="table-responsive">
        <table id="itemsTable">
            <thead>
                <tr>
                    <th>Désignation</th>
                    <th style="width: 60px;">Qté</th>
                    <th style="width: 110px;">Prix Unitaire HT (€)</th>
                    <th style="width: 110px;">Prix HT (€)</th>
                    <th class="no-print" style="width: 70px;">Action</th>
                </tr>
            </thead>
            <tbody id="tableBody">
                <tr>
                    <td><input type="text" class="table-input" placeholder="Ex: Pose de la tuyauterie"></td>
                    <td><input type="number" class="table-input qte" value="1" oninput="calculateTotals()"></td>
                    <td><input type="number" class="table-input pu" value="0" oninput="calculateTotals()"></td>
                    <td><span class="row-total">0.00</span> €</td>
                    <td class="no-print"><button class="btn btn-danger" onclick="deleteRow(this)">Suppr</button></td>
                </tr>
            </tbody>
        </table>
    </div>
    
    <!-- Bouton d'ajout configuré -->
    <button type="button" class="btn no-print" style="background: #64748b; margin-bottom: 15px;" onclick="addRow()">+ Ajouter une ligne au tableau</button>

    <div class="totals">
        <div class="total-row"><span>Total Général HT :</span><span id="totalDevis">0.00 €</span></div>
        <div class="total-row">
            <span>TVA :</span>
            <select id="tvaRate" onchange="calculateTotals()" style="padding: 2px; font-size: 13px;">
                <option value="20">20 % (Normal)</option>
                <option value="10" selected>10 % (Rénovation)</option>
                <option value="5.5">5.5 % (Énergie)</option>
            </select>
        </div>
        <div class="total-row grand-total"><span>Total TTC :</span><span id="grandTotal">0.00 €</span></div>
    </div>

    <button type="button" class="btn no-print" onclick="window.print()">Créer le PDF / Imprimer</button>
</div>

<script>
    function addRow() {
        const tbody = document.getElementById('tableBody');
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td><input type="text" class="table-input" placeholder="Nouvelle prestation"></td>
            <td><input type="number" class="table-input qte" value="1" oninput="calculateTotals()"></td>
            <td><input type="number" class="table-input pu" value="0" oninput="calculateTotals()"></td>
            <td><span class="row-total">0.00</span> €</td>
            <td class="no-print"><button class="btn btn-danger" onclick="deleteRow(this)">Suppr</button></td>
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
        const tvaRate = parseFloat(document.getElementById('tvaRate').value) / 100;
        let globalHT = 0;

        rows.forEach(row => {
            const qteInput = row.querySelector('.qte');
            const puInput = row.querySelector('.pu');
            
            if(qteInput && puInput) {
                const qte = parseFloat(qteInput.value) || 0;
                const pu = parseFloat(puInput.value) || 0;
                const totalLigne = qte * pu;
                globalHT += totalLigne;
                row.querySelector('.row-total').innerText = totalLigne.toFixed(2);
            }
        });

        const tvaAmount = globalHT * tvaRate;
        const grandTotal = globalHT + tvaAmount;

        document.getElementById('totalDevis').innerText = globalHT.toFixed(2) + " €";
        document.getElementById('grandTotal').innerText = grandTotal.toFixed(2) + " €";
    }
</script>
</body>
</html>
