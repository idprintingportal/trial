<!DOCTYPE html>
<html lang="mr">
<head>
  <meta charset="UTF-8">
  <title>AgriStack PVC Card Generator — Template Aligned</title>
  <!-- QRCode.js Library CDN -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    body {
      background-color: #eceff1;
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    h1 {
      margin-bottom: 20px;
      color: #1b5e20;
    }
    .main-container {
      display: flex;
      gap: 30px;
      flex-wrap: wrap;
      justify-content: center;
      max-width: 1250px;
    }
    
    /* Live Editor Form */
    .editor-form {
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      width: 420px;
      max-height: 88vh;
      overflow-y: auto;
    }
    .editor-form h2 {
      font-size: 15px;
      margin-top: 12px;
      margin-bottom: 10px;
      color: #2e7d32;
      border-bottom: 2px solid #a5d6a7;
      padding-bottom: 4px;
    }
    .form-group {
      margin-bottom: 8px;
    }
    .form-group label {
      display: block;
      font-size: 11px;
      font-weight: bold;
      margin-bottom: 2px;
      color: #333;
    }
    .form-group input {
      width: 100%;
      padding: 5px 8px;
      font-size: 12px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }

    /* Dynamic Table Input Section */
    .land-row-input {
      display: flex;
      gap: 4px;
      margin-bottom: 5px;
    }
    .land-row-input input {
      padding: 4px;
      font-size: 11px;
      border: 1px solid #ccc;
      border-radius: 3px;
    }
    .btn-action {
      padding: 5px 10px;
      background: #1b5e20;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 11px;
      font-weight: bold;
      margin-top: 5px;
    }
    .btn-remove {
      background: #c62828;
      padding: 2px 6px;
      font-size: 10px;
    }

    /* Standard CR80 PVC Card Dimensions (3.375in x 2.125in) */
    .cards-wrapper {
      display: flex;
      flex-direction: column;
      gap: 20px;
    }
    
    .pvc-card {
      width: 323.5px; /* ~85.6mm */
      height: 204px;   /* ~53.98mm */
      border: 1px solid #b0bec5;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.15);
      position: relative;
      overflow: hidden;
      padding: 10px;
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
    }

    /* Front & Back Background Templates */
    #frontCard {
      background-image: linear-gradient(145deg,rgba(255,255,255,.9),rgba(250,250,232,.65)); 
    }
    #backCard {
      background-image: linear-gradient(145deg,rgba(255,255,255,.9),rgba(247,248,224,.6)); 
    }

    /* FIXED: Vertical Dates Positioning (Separated from table margin) */
    .vertical-date-front {
      position: absolute;
      left: 6px;
      bottom: 25px;
      transform: rotate(-90deg);
      transform-origin: left bottom;
      font-size: 7.5px;
      font-weight: bold;
      color: #000;
      white-space: nowrap;
      z-index: 10;
    }

    .vertical-date-back {
      position: absolute;
      left: 6px;
      bottom: 25px;
      transform: rotate(-90deg);
      transform-origin: left bottom;
      font-size: 7.5px;
      font-weight: bold;
      color: #000;
      white-space: nowrap;
      z-index: 10;
    }

    /* Front Card Elements */
    .photo-box {
      position: absolute;
      top: 45px;
      left: 30px;
      width: 70px;
      height: 86px;
      border: 1px solid #90a4ae;
      border-radius: 4px;
      background: #e0e0e0;
      overflow: hidden;
    }
    .photo-box img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .info-details-front {
      position: absolute;
      top: 45px;
      left: 108px;
      font-size: 9px;
      line-height: 1.35;
      color: #000;
      font-weight: 600;
    }

    .qr-box {
      position: absolute;
      top: 80px;
      right: 18px;
      width: 60px;
      height: 60px;
      background: #fff;
      padding: 2px;
      border: 1px solid #ccc;
    }
    .qr-box img {
      width: 100% !important;
      height: 100% !important;
    }

    .farmer-id-disp {
      position: absolute;
      bottom: 26px;
      left: 0;
      width: 100%;
      text-align: center;
      font-weight: bold;
      font-size: 12.5px;
      color: #1b5e20;
    }

    /* Back Card Address Position */
    .disp-address-text {
      position: absolute;
      top: 42px;
      left: 38px;
      right: 15px;
      font-size: 8px;
      font-weight: 600;
      color: #000;
      line-height: 1.2;
    }

    /* FIXED: Table Position (Clear Gap from Vertical Date Text) */
    .agri-table-overlay {
      position: absolute;
      top: 80px;
      left: 38px; /* Safe Margin from left edge */
      width: 270px;
      border-collapse: collapse;
      font-size: 7.5px;
      text-align: center;
      background: transparent;
    }
    .agri-table-overlay td {
      padding: 3px 1px;
      font-weight: bold;
      color: #000;
      border: 0.5px solid #ccc;
      background: #fff; /* Ensures clear visibility */
    }

    .print-btn {
      margin-top: 20px;
      padding: 10px 24px;
      background: #2e7d32;
      color: white;
      border: none;
      border-radius: 5px;
      font-size: 14px;
      cursor: pointer;
      font-weight: bold;
    }

    @media print {
      body * { visibility: hidden; }
      .cards-wrapper, .cards-wrapper * { visibility: visible; }
      .cards-wrapper { position: absolute; left: 0; top: 0; }
      .editor-form, .print-btn, h1 { display: none; }
    }

    /* Exact PVC preview and print bounds; artwork and overlaid data are separate. */
    .pvc-card { width:323.5px; height:204px; flex-shrink:0; padding:0;
      box-shadow:0 4px 10px rgba(0,0,0,.12); border-radius:9px; isolation:isolate; }
    .pvc-card::before { content:""; position:absolute; pointer-events:none;
      inset:0; background:radial-gradient(ellipse at 12% 104%,rgba(156,183,101,.27),transparent 37%),
      radial-gradient(ellipse at 99% 91%,rgba(183,195,114,.24),transparent 33%); z-index:0; }
    .template-heading { position:absolute; left:3px;right:3px;top:3px;height:31px;
      border-bottom:1px solid #333;display:flex;align-items:center;justify-content:center;
      gap:7px;z-index:1;pointer-events:none; }
    .template-logo { position:absolute;left:7px;top:8px;font-size:11px;font-weight:bold;color:#23672f;letter-spacing:-.5px; }
    .template-logo em {font-style:normal;color:#ef7a1a}
    .heading-text {text-align:center;line-height:1.05;padding-left:17px;}
    .heading-text .mr {display:block;font-size:9px;font-weight:700;color:#cf4e36;}
    .heading-text .en {display:block;font-family:Georgia,serif;font-size:15px;font-weight:bold;color:#214c2c;}
    .seal {position:absolute;right:11px;top:0;width:30px;height:30px;border-radius:50%;background:#090909;}
    /* Dates rotate around their own center so they cannot protrude through card edges. */
    .vertical-date-front,.vertical-date-back { left:2px;top:59px;bottom:auto;
      width:117px;height:10px;line-height:10px;transform-origin:top left;
      transform:translate(1px,117px) rotate(-90deg);text-align:center;
      font-size:7px;font-weight:700;z-index:3;white-space:nowrap; }
    .photo-box {top:46px;left:16px;width:71px;height:88px;border:1px solid #75869b;
      background:#e4ebf2;border-radius:4px;z-index:1;}
    .photo-box img {object-fit:cover;}
    .info-details-front {top:45px;left:94px;right:90px;font-size:8.2px;
      line-height:1.46;z-index:2;overflow-wrap:anywhere;}
    .info-details-front .info-line {display:grid;grid-template-columns:51px minmax(0,1fr);gap:2px;}
    .info-details-front .info-line .key {font-weight:750;}
    .qr-box {top:89px;right:18px;width:70px;height:70px;padding:2px;z-index:2;border:1px solid #999;}
    .qr-box img,.qr-box canvas {display:block;width:64px!important;height:64px!important;}
    .farmer-id-disp {bottom:19px;left:11px;width:calc(100% - 22px);
      font-family:Georgia,serif;font-size:14px;line-height:1.2;white-space:nowrap;
      text-align:center;z-index:2;}
    .card-foot-rule {position:absolute;bottom:18px;left:4px;right:4px;
      border-top:2px solid #c78229;z-index:1;}
    .disp-address-text {left:19px;right:17px;top:37px;line-height:1.18;
      font-family:Georgia,serif;font-size:8.5px;z-index:2;max-height:28px;overflow:hidden;}
    .agri-table-overlay {left:16px;top:65px;width:292px;max-width:calc(100% - 31px);
      table-layout:fixed; border-collapse:collapse; font-size:7.6px;z-index:2;
      background:white;}
    .agri-table-overlay th {background:#aad0c0;color:#13503d;
      border:.6px solid #333;padding:4px 1px;font-weight:bold;white-space:nowrap;}
    .agri-table-overlay td {background:rgba(255,255,255,.96);border:.6px solid #444;
      padding:3px 1px;overflow-wrap:anywhere;line-height:1.1;}
    .agri-table-overlay th:nth-child(1){width:11%}
    .agri-table-overlay th:nth-child(2){width:16%}
    .agri-table-overlay th:nth-child(3){width:17%}
    .agri-table-overlay th:nth-child(4){width:20%}
    .agri-table-overlay th:nth-child(5){width:12%}
    .agri-table-overlay th:nth-child(6){width:12%}
    .agri-table-overlay th:nth-child(7){width:12%}
    .table-notice {display:none;font-size:12px;color:#9b251e;font-weight:bold;max-width:380px;}
    .photo-empty {position:absolute;inset:0;display:flex;align-items:center;justify-content:center;
      font-weight:700;font-size:11px;color:#77879d;}
    #displayPhoto:not([src]) {display:none;}
    .photo-box:has(#displayPhoto[src]) .photo-empty {display:none;}
    @page {size:auto;margin:8mm;}
    @media print {
      body{padding:0;background:white!important;}
      .cards-wrapper{position:static!important;display:flex;gap:6mm;}
      .pvc-card {width:85.6mm;height:54mm;border:.15mm solid #9c9c9c;
        border-radius:2mm;box-shadow:none;break-inside:avoid;
        print-color-adjust:exact;-webkit-print-color-adjust:exact;}
      .cards-wrapper{position:absolute!important;left:0;top:0;}
    }
  </style>
</head>
<body>

  <h1>AgriStack Card Generator</h1>

  <div class="main-container">
    <!-- Live Form Controls -->
    <div class="editor-form">
      <h2>Front Side Details</h2>
      <div class="form-group">
        <label>Approval Date (Vertical Text):</label>
        <input type="text" id="inputApprovalDate" value="Approval Date : 07/10/2026">
      </div>
      <div class="form-group">
        <label>Upload Photo:</label>
        <input type="file" id="inputPhoto" accept="image/*">
      </div>
      <div class="form-group">
        <label>Name (Marathi):</label>
        <input type="text" id="inputNameMr" value="रमेश बाबुराव पाटील">
      </div>
      <div class="form-group">
        <label>Name (English):</label>
        <input type="text" id="inputNameEn" value="Ramesh Baburao Patil">
      </div>
      <div class="form-group">
        <label>Gender:</label>
        <input type="text" id="inputGender" value="Male">
      </div>
      <div class="form-group">
        <label>DOB:</label>
        <input type="text" id="inputDob" value="15/08/1980">
      </div>
      <div class="form-group">
        <label>Area:</label>
        <input type="text" id="inputArea" value="Rural">
      </div>
      <div class="form-group">
        <label>Aadhaar No:</label>
        <input type="text" id="inputAadhaar" value="[Aadhaar Redacted]">
      </div>
      <div class="form-group">
        <label>Mobile No:</label>
        <input type="text" id="inputMobile" value="9876543210">
      </div>
      <div class="form-group">
        <label>Farmer ID:</label>
        <input type="text" id="inputFarmerId" value="9876 5432 1010">
      </div>

      <h2>Back Side Details</h2>
      <div class="form-group">
        <label>Download Date (Vertical Text):</label>
        <input type="text" id="inputDownloadDate" value="Download Date : 07/10/2026">
      </div>
      <div class="form-group">
        <label>Address:</label>
        <input type="text" id="inputAddress" value="A/P. Pachora, Tal. Pachora, Dist. Jalgaon - 424201">
      </div>

      <h2>Agriculture Table (Add Multiple Rows)</h2>
      <div id="landRowsContainer">
        <!-- Default Initial Row -->
        <div class="land-row-input">
          <input type="text" placeholder="State" value="MH" style="width: 35px;">
          <input type="text" placeholder="District" value="Jalgaon" style="width: 55px;">
          <input type="text" placeholder="Sub Dist" value="Pachora" style="width: 55px;">
          <input type="text" placeholder="Village" value="Pachora" style="width: 55px;">
          <input type="text" placeholder="S.No" value="102" style="width: 35px;">
          <input type="text" placeholder="Khata" value="4" style="width: 35px;">
          <input type="text" placeholder="Area" value="0.500" style="width: 45px;">
        </div>
      </div>
      <button class="btn-action" onclick="addLandRow()">+ Add Row</button>
    </div>

    <!-- PVC Card Previews -->
    <div class="cards-wrapper">
      <!-- FRONT CARD -->
      <div class="pvc-card" id="frontCard">
        <div class="template-heading" aria-hidden="true"><div class="template-logo">Agri<em>Stack</em></div><div class="heading-text"><span class="mr">शेतकरी वैयक्तिक ओळखपत्र</span><span class="en">Farmer Personal Identity Card</span></div><div class="seal"></div></div>
        <!-- Vertical Approval Date -->
        <div class="vertical-date-front" id="dispApprovalDate">Approval Date : 07/10/2026</div>

        <div class="photo-box">
          <span class="photo-empty">PHOTO</span><img id="displayPhoto" alt="Photo">
        </div>
        
        <div class="info-details-front">
          <div class="info-line"><span class="key">नाव</span><span id="dispNameMr">रमेश बाबुराव पाटील</span></div>
          <div class="info-line"><span class="key">Name</span><span id="dispNameEn">Ramesh Baburao Patil</span></div>
          <div class="info-line"><span class="key">Gender</span><span id="dispGender">Male</span></div>
          <div class="info-line"><span class="key">DOB</span><span id="dispDob">15/08/1980</span></div>
          <div class="info-line"><span class="key">Area</span><span id="dispArea">Rural</span></div>
          <div class="info-line"><span class="key">Aadhaar</span><span id="dispAadhaar">[Aadhaar Redacted]</span></div>
          <div class="info-line"><span class="key">Mobile</span><span id="dispMobile">9876543210</span></div>
        </div>

        <!-- Dynamic QR Code -->
        <div class="qr-box" id="qrcode"></div>

        <div class="farmer-id-disp">
          Farmer ID : <span id="dispFarmerId">9876 5432 1010</span>
        </div>
      </div>

      <!-- BACK CARD -->
      <div class="pvc-card" id="backCard">
        <div class="template-heading" aria-hidden="true"><div class="template-logo">Agri<em>Stack</em></div><div class="heading-text"><span class="mr">शेतीची माहिती</span><span class="en">Information about agriculture</span></div><div class="seal"></div></div>
        <!-- Vertical Download Date -->
        <div class="vertical-date-back" id="dispDownloadDate">Download Date : 07/10/2026</div>

        <div class="disp-address-text">
          <b>Address:</b> <span id="dispAddress">A/P. Pachora, Tal. Pachora, Dist. Jalgaon - 424201</span>
        </div>

        <!-- Dynamic Agriculture Table -->
        <table class="agri-table-overlay" id="agriTableDisplay">
          <thead><tr><th>State</th><th>District</th><th>Sub Dist</th><th>Village</th><th>S.No</th><th>Khata</th><th>Area (Ha)</th></tr></thead>
          <tbody id="agriTableBody"></tbody>
        </table>
        <div class="card-foot-rule" aria-hidden="true"></div>
      </div>
    </div>
  </div>

  <p class="table-notice" id="tableNotice" role="alert"></p>
  <button class="print-btn" onclick="window.print()">Print Cards</button>

  <script>
    // Form Elements
    const inputApprovalDate = document.getElementById('inputApprovalDate');
    const inputNameMr = document.getElementById('inputNameMr');
    const inputNameEn = document.getElementById('inputNameEn');
    const inputGender = document.getElementById('inputGender');
    const inputDob = document.getElementById('inputDob');
    const inputArea = document.getElementById('inputArea');
    const inputAadhaar = document.getElementById('inputAadhaar');
    const inputMobile = document.getElementById('inputMobile');
    const inputFarmerId = document.getElementById('inputFarmerId');
    const inputPhoto = document.getElementById('inputPhoto');

    const inputDownloadDate = document.getElementById('inputDownloadDate');
    const inputAddress = document.getElementById('inputAddress');
    const qrcodeContainer = document.getElementById('qrcode');

    // Dynamic QR Code Generator
    function updateQRCode() {
      const qrData = `Farmer ID: ${inputFarmerId.value}\nName: ${inputNameEn.value}\nDOB: ${inputDob.value}\nGender: ${inputGender.value}\nMobile: ${inputMobile.value}\nAddress: ${inputAddress.value}`;
      qrcodeContainer.innerHTML = "";
      new QRCode(qrcodeContainer, {
        text: qrData,
        width: 64,
        height: 64,
        correctLevel: QRCode.CorrectLevel.M
      });
    }

    // Photo Upload
    inputPhoto.addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(event) {
          document.getElementById('displayPhoto').src = event.target.result;
        };
        reader.readAsDataURL(file);
      }
    });

    // Dynamic Land Table Rows
    function addLandRow() {
      const container = document.getElementById('landRowsContainer');
      if (container.querySelectorAll('.land-row-input').length >= 5) { alert('PVC कार्ड में अधिकतम 5 पंक्तियाँ फिट होंगी।'); return; }
      const rowDiv = document.createElement('div');
      rowDiv.className = 'land-row-input';
      rowDiv.innerHTML = `
        <input type="text" placeholder="State" value="MH" style="width: 35px;">
        <input type="text" placeholder="District" value="Jalgaon" style="width: 55px;">
        <input type="text" placeholder="Sub Dist" value="Pachora" style="width: 55px;">
        <input type="text" placeholder="Village" value="Pachora" style="width: 55px;">
        <input type="text" placeholder="S.No" value="102" style="width: 35px;">
        <input type="text" placeholder="Khata" value="4" style="width: 35px;">
        <input type="text" placeholder="Area" value="0.500" style="width: 45px;">
        <button class="btn-action btn-remove" onclick="removeRow(this)">X</button>
      `;
      container.appendChild(rowDiv);
      bindRowEvents();
      renderLandTable();
    }

    function removeRow(btn) {
      btn.parentElement.remove();
      renderLandTable();
    }

    function renderLandTable() {
      const container = document.getElementById('landRowsContainer');
      const rows = container.getElementsByClassName('land-row-input');
      const tableDisplay = document.getElementById('agriTableBody');
      tableDisplay.replaceChildren();

      Array.from(rows).forEach(row => {
        const inputs = row.getElementsByTagName('input');
        const tr = document.createElement('tr');
        Array.from(inputs).forEach(input => {
          const td = document.createElement('td');
          td.textContent = input.value;
          tr.appendChild(td);
        });
        tableDisplay.appendChild(tr);
      });
    }

    function bindRowEvents() {
      const inputs = document.querySelectorAll('#landRowsContainer input');
      inputs.forEach(input => {
        input.removeEventListener('input', renderLandTable);
        input.addEventListener('input', renderLandTable);
      });
    }

    // Static Fields Sync
    function bindSync() {
      document.getElementById('dispApprovalDate').innerText = inputApprovalDate.value;
      document.getElementById('dispNameMr').innerText = inputNameMr.value;
      document.getElementById('dispNameEn').innerText = inputNameEn.value;
      document.getElementById('dispGender').innerText = inputGender.value;
      document.getElementById('dispDob').innerText = inputDob.value;
      document.getElementById('dispArea').innerText = inputArea.value;
      document.getElementById('dispAadhaar').innerText = inputAadhaar.value;
      document.getElementById('dispMobile').innerText = inputMobile.value;
      document.getElementById('dispFarmerId').innerText = inputFarmerId.value;
      document.getElementById('dispDownloadDate').innerText = inputDownloadDate.value;
      document.getElementById('dispAddress').innerText = inputAddress.value;

      updateQRCode();
    }

    [inputApprovalDate, inputNameMr, inputNameEn, inputGender, inputDob, inputArea, inputAadhaar, inputMobile, inputFarmerId, inputDownloadDate, inputAddress].forEach(elem => {
      elem.addEventListener('input', bindSync);
    });

    // Prevent cropped row content or layout spill in print. 
    window.addEventListener('beforeprint', () => {
      const t = document.getElementById('agriTableDisplay');
      const card = document.getElementById('backCard');
      if (t.getBoundingClientRect().bottom > card.getBoundingClientRect().bottom - 12) {
        alert('Agriculture table is too long for the PVC card. Please shorten entries or remove rows before printing.');
      }
    });

    // Initial Trigger
    bindSync();
    bindRowEvents();
    renderLandTable();
  </script>
</body>
</html>
