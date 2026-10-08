<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>AgriStack Farmer Identity Card Generator (PVC Size)</title>
  <!-- QRCode.js Library CDN for Automatic QR Generation -->
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
      max-width: 1200px;
    }
    
    /* Live Editor Form */
    .editor-form {
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      width: 380px;
    }
    .editor-form h2 {
      font-size: 15px;
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

    /* Standard CR80 PVC Card Dimensions (3.375in x 2.125in) */
    .cards-wrapper {
      display: flex;
      flex-direction: column;
      gap: 20px;
    }
    
    .pvc-card {
      width: 323.5px; /* ~85.6mm at 96 DPI */
      height: 204px;   /* ~53.98mm at 96 DPI */
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

    /* Set Background Images for Front and Back Templates */
    #frontCard {
      /* Front Background Image Template */
      background-image: url('front-bg.png'); 
    }

    #backCard {
      /* Back Background Image Template */
      background-image: url('back-bg.png'); 
    }

    /* Overlay positioning for Front Card */
    .photo-box {
      position: absolute;
      top: 45px;
      left: 20px;
      width: 75px;
      height: 90px;
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
      left: 105px;
      font-size: 9.5px;
      line-height: 1.35;
      color: #000;
      font-weight: 600;
    }

    .qr-box {
      position: absolute;
      top: 82px;
      right: 15px;
      width: 65px;
      height: 65px;
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
      bottom: 28px;
      left: 0;
      width: 100%;
      text-align: center;
      font-weight: bold;
      font-size: 13px;
      color: #1b5e20;
    }

    /* Overlay positioning for Back Card */
    .disp-address-text {
      position: absolute;
      top: 42px;
      left: 20px;
      right: 20px;
      font-size: 8.5px;
      font-weight: 600;
      color: #000;
      line-height: 1.2;
    }

    .agri-table-overlay {
      position: absolute;
      top: 80px;
      left: 16px;
      width: 290px;
      border-collapse: collapse;
      font-size: 8px;
      text-align: center;
    }
    .agri-table-overlay td {
      padding: 3px 2px;
      font-weight: bold;
      color: #000;
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

  <h1>AgriStack Identity Card Live Editor</h1>

  <div class="main-container">
    <!-- Live Form Controls -->
    <div class="editor-form">
      <h2>Farmer Details (Front)</h2>
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

      <h2>Land & Address Details (Back)</h2>
      <div class="form-group">
        <label>Address:</label>
        <input type="text" id="inputAddress" value="A/P. Pachora, Tal. Pachora, Dist. Jalgaon - 424201">
      </div>
      <div class="form-group">
        <label>State / District / Sub Dist / Village:</label>
        <input type="text" id="inputLocDetails" value="MH / Jalgaon / Pachora / Pachora">
      </div>
      <div class="form-group">
        <label>Survey No / Khata / Area (Ha):</label>
        <input type="text" id="inputLandDetails" value="102 / 4 / 0.500">
      </div>
    </div>

    <!-- PVC Card Previews -->
    <div class="cards-wrapper">
      <!-- FRONT CARD -->
      <div class="pvc-card" id="frontCard">
        <div class="photo-box">
          <img id="displayPhoto" src="https://via.placeholder.com/75x90?text=PHOTO" alt="Photo">
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

        <!-- Automatic Live QR Code -->
        <div class="qr-box" id="qrcode"></div>

        <div class="farmer-id-disp">
          Farmer ID : <span id="dispFarmerId">9876 5432 1010</span>
        </div>
      </div>

      <!-- BACK CARD -->
      <div class="pvc-card" id="backCard">
        <div class="disp-address-text">
          <b>Address:</b> <span id="dispAddress">A/P. Pachora, Tal. Pachora, Dist. Jalgaon - 424201</span>
        </div>

        <table class="agri-table-overlay">
          <tr>
            <td id="dispState" style="width: 38px;">MH</td>
            <td id="dispDist" style="width: 48px;">Jalgaon</td>
            <td id="dispSubDist" style="width: 48px;">Pachora</td>
            <td id="dispVillage" style="width: 48px;">Pachora</td>
            <td id="dispSNo" style="width: 32px;">102</td>
            <td id="dispKhata" style="width: 32px;">4</td>
            <td id="dispAreaHa" style="width: 44px;">0.500</td>
          </tr>
        </table>
      </div>
    </div>
  </div>

  <button class="print-btn" onclick="window.print()">Print PVC Cards</button>

  <script>
    // Getting Elements
    const inputNameMr = document.getElementById('inputNameMr');
    const inputNameEn = document.getElementById('inputNameEn');
    const inputGender = document.getElementById('inputGender');
    const inputDob = document.getElementById('inputDob');
    const inputArea = document.getElementById('inputArea');
    const inputAadhaar = document.getElementById('inputAadhaar');
    const inputMobile = document.getElementById('inputMobile');
    const inputFarmerId = document.getElementById('inputFarmerId');
    const inputAddress = document.getElementById('inputAddress');
    const inputLocDetails = document.getElementById('inputLocDetails');
    const inputLandDetails = document.getElementById('inputLandDetails');
    const inputPhoto = document.getElementById('inputPhoto');

    const qrcodeContainer = document.getElementById('qrcode');

    // Generate Dynamic QR Code
    function updateQRCode() {
      const qrData = `Farmer ID: ${inputFarmerId.value}\nName: ${inputNameEn.value}\nDOB: ${inputDob.value}\nGender: ${inputGender.value}\nMobile: ${inputMobile.value}\nAddress: ${inputAddress.value}`;
      qrcodeContainer.innerHTML = "";
      new QRCode(qrcodeContainer, {
        text: qrData,
        width: 65,
        height: 65,
        correctLevel: QRCode.CorrectLevel.M
      });
    }

    // Photo Reader
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

    // Real-time Text Synchronizer
    function bindSync() {
      document.getElementById('dispNameMr').innerText = inputNameMr.value;
      document.getElementById('dispNameEn').innerText = inputNameEn.value;
      document.getElementById('dispGender').innerText = inputGender.value;
      document.getElementById('dispDob').innerText = inputDob.value;
      document.getElementById('dispArea').innerText = inputArea.value;
      document.getElementById('dispAadhaar').innerText = inputAadhaar.value;
      document.getElementById('dispMobile').innerText = inputMobile.value;
      document.getElementById('dispFarmerId').innerText = inputFarmerId.value;
      document.getElementById('dispAddress').innerText = inputAddress.value;

      // Location fields sync
      const loc = inputLocDetails.value.split('/');
      document.getElementById('dispState').innerText = loc[0] ? loc[0].trim() : '';
      document.getElementById('dispDist').innerText = loc[1] ? loc[1].trim() : '';
      document.getElementById('dispSubDist').innerText = loc[2] ? loc[2].trim() : '';
      document.getElementById('dispVillage').innerText = loc[3] ? loc[3].trim() : '';

      // Land fields sync
      const land = inputLandDetails.value.split('/');
      document.getElementById('dispSNo').innerText = land[0] ? land[0].trim() : '';
      document.getElementById('dispKhata').innerText = land[1] ? land[1].trim() : '';
      document.getElementById('dispAreaHa').innerText = land[2] ? land[2].trim() : '';

      updateQRCode();
    }

    // Attach Event Listeners
    [inputNameMr, inputNameEn, inputGender, inputDob, inputArea, inputAadhaar, inputMobile, inputFarmerId, inputAddress, inputLocDetails, inputLandDetails].forEach(elem => {
      elem.addEventListener('input', bindSync);
    });

    // Initial Execution
    bindSync();
  </script>
</body>
</html>
