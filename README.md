<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>AgriStack Card Generator (No Overlap Layout)</title>
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
      background-image: url('front-bg.png'); 
    }
    #backCard {
      background-image: url('back-bg.png'); 
    }

    /* Both vertical dates stay inside the safe print area. */
    .vertical-date-front, .vertical-date-back {
      position: absolute;
      left: 11px;
      top: 41px;
      height: 139px;
      max-width: 12px;
      writing-mode: vertical-rl;
      transform: rotate(180deg);
      font-size: 7px;
      line-height: 1.1;
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

    /* Fixed back-side viewport: rows scale inside the PVC edges. */
    .agri-table-area {
      position: absolute;
      top: 83px;
      left: 38px;
      right: 15px;
      bottom: 12px;
      overflow: hidden;
    }
    .agri-table-overlay {
      width: 100%;
      table-layout: fixed;
      border-collapse: collapse;
      font-size: 7.5px;
      text-align: center;
      background: transparent;
      transform-origin: top left;
    }
    .agri-table-overlay td {
      padding: 3px 1px;
      font-weight: bold;
      color: #000;
      border: 0.5px solid #ccc;
      background: #fff;
      overflow-wrap: anywhere;
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
        <!-- Vertical Approval Date -->
        <div class="vertical-date-front" id="dispApprovalDate">Approval Date : 07/10/2026</div>

        <div class="photo-box">
          <img id="displayPhoto" src="https://via.placeholder.com/70x86?text=PHOTO" alt="Photo">
        </div>
        
        <div class="info-details-front">
          <div><span id="dispNameMr">रमेश बाबुराव पाटील</span></div>
          <div><span id="dispNameEn">Ramesh Baburao Patil</span></div>
          <div><span id="dispGender">Male</span></div>
          <div><span id="dispDob">15/08/1980</span></div>
          <div><span id="dispArea">Rural</span></div>
          <div><span id="dispAadhaar">[Aadhaar Redacted]</span></div>
          <div><span id="dispMobile">9876543210</span></div>
        </div>

        <!-- Dynamic QR Code -->
        <div class="qr-box" id="qrcode"></div>

        <div class="farmer-id-disp">
          Farmer ID : <span id="dispFarmerId">9876 5432 1010</span>
        </div>
      </div>

      <!-- BACK CARD -->
      <div class="pvc-card" id="backCard">
        <!-- Vertical Download Date -->
        <div class="vertical-date-back" id="dispDownloadDate">Download Date : 07/10/2026</div>

        <div class="disp-address-text">
          <b>Address:</b> <span id="dispAddress">A/P. Pachora, Tal. Pachora, Dist. Jalgaon - 424201</span>
        </div>

        <!-- Dynamic Agriculture Table -->
        <div class="agri-table-area" id="agriTableArea">
          <table class="agri-table-overlay" id="agriTableDisplay">
            <!-- Rows render dynamically -->
          </table>
        </div>
      </div>
    </div>
  </div>

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
        width: 60,
        height: 60,
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

    function fitLandTable() {
      const area = document.getElementById('agriTableArea');
      const table = document.getElementById('agriTableDisplay');
      table.style.transform = 'none';
      table.style.width = '100%';
      const naturalHeight = table.offsetHeight;
      if (!naturalHeight) return;
      const scale = Math.min(1, area.clientHeight / naturalHeight);
      // Resize width inversely so that the scaled table still fills the back panel.
      table.style.width = `${100 / scale}%`;
      table.style.transform = `scale(${scale})`;
    }

    function renderLandTable() {
      const rows = document.querySelectorAll('#landRowsContainer .land-row-input');
      const tableDisplay = document.getElementById('agriTableDisplay');
      tableDisplay.replaceChildren();
      rows.forEach(row => {
        const tr = document.createElement('tr');
        row.querySelectorAll('input').forEach(input => {
          const td = document.createElement('td');
          td.textContent = input.value;
          tr.appendChild(td);
        });
        tableDisplay.appendChild(tr);
      });
      fitLandTable();
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

    // Initial Trigger
    bindSync();
    bindRowEvents();
    renderLandTable();
    window.addEventListener('resize', fitLandTable);
    window.addEventListener('beforeprint', fitLandTable);
  </script>
</body>
</html>
