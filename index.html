<!doctype html>
<html lang="hi">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>8-Field Secure QR</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
  <script src="https://unpkg.com/html5-qrcode"></script>
  <style>
    :root { color-scheme: light; font-family: Arial, sans-serif; background: #f1f5f9; color: #172033; }
    * { box-sizing: border-box; }
    body { max-width: 980px; margin: 0 auto; padding: 24px 16px 48px; }
    h1 { margin: 0 0 8px; font-size: 1.7rem; }
    .intro { color: #475569; line-height: 1.5; margin: 0 0 20px; }
    .card { background: #fff; border: 1px solid #dbe2ea; border-radius: 14px; padding: 20px; margin: 16px 0; box-shadow: 0 3px 14px #0f172a0b; }
    h2 { margin: 0 0 16px; font-size: 1.2rem; }
    .fields { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 12px; }
    label { display: block; font-size: .9rem; font-weight: 700; }
    input { display: block; width: 100%; margin-top: 6px; padding: 11px 12px; border: 1px solid #cbd5e1; border-radius: 8px; font: inherit; }
    input:focus { outline: 2px solid #93c5fd; border-color: #2563eb; }
    button { border: 0; border-radius: 8px; background: #2563eb; color: #fff; font-weight: 700; font-size: .95rem; padding: 11px 15px; cursor: pointer; }
    button:hover { background: #1d4ed8; }
    button.secondary { background: #475569; }
    .actions { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 16px; }
    #qrcode { display: flex; justify-content: center; margin: 18px auto 0; min-height: 10px; }
    #reader { width: min(100%, 440px); margin: 16px auto; }
    .status { min-height: 1.4em; margin: 12px 0 0; font-weight: 700; color: #334155; }
    .hint { font-size: .88rem; color: #475569; line-height: 1.5; margin: 12px 0 0; }
    .warning { border-left: 4px solid #f59e0b; padding: 10px 12px; background: #fffbeb; color: #713f12; }
    #decodedView[hidden] { display: none; }
    .result-fields { display: grid; grid-template-columns: 1fr; gap: clamp(18px, 2.8vw, 32px); }
    .result-fields label { font-size: clamp(1.5rem, 4.1vw, 2.8rem); font-weight: 400; }
    .result-fields input { height: clamp(62px, 9.3vw, 102px); margin-top: clamp(12px, 2vw, 20px); border: 2px solid #999; border-radius: 10px; color: #111; background: #fff; font-size: clamp(1.1rem, 3.1vw, 2rem); }
    body.decoded-mode { max-width: 1080px; padding: 12px clamp(20px, 4vw, 48px) 36px; background: #fff; }
    body.decoded-mode > h1, body.decoded-mode > .intro, body.decoded-mode > .card:not(#decodedView) { display: none !important; }
    body.decoded-mode #decodedView { margin: 0; padding: 8px 0; border: 0; border-radius: 0; box-shadow: none; background: #fff; }
    @media (max-width: 620px) { body { padding: 16px 10px 32px; } .card { padding: 16px; } .fields { grid-template-columns: 1fr; } }
  </style>
</head>
<body>
  <h1>8-Field Secure QR</h1>
  <p class="intro">सभी 8 बॉक्स भरकर encrypted QR बनाएँ। Scan करने के लिए यही page खोलें, वही passphrase दें और camera से QR पढ़ें।</p>

  <section class="card">
    <h2>1. आठ बॉक्स भरें और QR बनाएँ</h2>
    <div id="generatorFields" class="fields"></div>
    <label style="margin-top:14px">Secret passphrase (QR बनाने और scan करने, दोनों समय वही phrase डालें)
      <input id="generatePassphrase" type="password" autocomplete="new-password" minlength="8" placeholder="कम से कम 8 अक्षर" required>
    </label>
    <button type="button" style="margin-top:14px" onclick="generateSecureQR()">आठ बॉक्स का Secure QR बनाएँ</button>
    <p id="generateStatus" class="status" role="status"></p>
    <div id="qrcode" aria-label="Generated encrypted QR code"></div>
  </section>

  <section class="card">
    <h2>2. Scan करके आठों बॉक्स में डेटा दिखाएँ</h2>
    <label>वही Secret passphrase
      <input id="scanPassphrase" type="password" autocomplete="off" minlength="8" placeholder="QR बनाते समय वाला passphrase" required>
    </label>
    <div class="actions">
      <button type="button" onclick="startScanner()">कैमरा स्कैनर चालू करें</button>
      <button type="button" class="secondary" onclick="stopScanner()">स्कैनर बंद करें</button>
    </div>
    <div id="reader"></div>
    <label>या QR से मिला text यहाँ paste करें
      <input id="manualPayload" type="text" autocomplete="off" placeholder="SQR8:...">
    </label>
    <button type="button" class="secondary" style="margin-top:10px" onclick="decryptManualPayload()">Paste किए QR को जाँचें</button>
    <p id="scanStatus" class="status" role="status"></p>
  </section>

  <section id="decodedView" class="card" hidden aria-label="Scanned QR data">
    <div id="scanFields" class="result-fields" aria-live="polite"></div>
    <button type="button" class="secondary" style="margin-top:22px" onclick="location.reload()">दूसरा QR स्कैन करें</button>
  </section>

  <p class="hint warning">मोबाइल के सामान्य camera scanner में encrypted text दिख सकता है, लेकिन असली field values नहीं। Values देखने के लिए इस page के scanner और सही passphrase की ज़रूरत है। किसी scanner में “कुछ भी न दिखे” यह client-side QR में संभव नहीं; उसके लिए authenticated server/token व्यवस्था चाहिए।</p>

  <script>
    'use strict';
    const APP_MARKER = 'SHIV-NIRMAL-QR';
    const QR_PREFIX = 'SQR8:';
    const FIELD_LABELS = ['UD Number', 'Name', 'Year Of Birth', 'Dis type', 'Per of Dis', 'Date of issue', 'Valid Upto', 'Aad Number'];
    const FIELD_COUNT = FIELD_LABELS.length;
    const PBKDF2_ITERATIONS = 210000;
    const encoder = new TextEncoder();
    const decoder = new TextDecoder();
    let scanner = null;
    let scannerRunning = false;
    let handledScan = false;

    function makeFields(containerId, prefix, readonly) {
      const container = document.getElementById(containerId);
      for (let i = 1; i <= FIELD_COUNT; i++) {
        const label = document.createElement('label');
        label.textContent = FIELD_LABELS[i - 1];
        const input = document.createElement('input');
        input.id = `${prefix}_field${i}`;
        input.type = 'text';
        input.autocomplete = 'off';
        input.placeholder = readonly ? FIELD_LABELS[i - 1] : `${FIELD_LABELS[i - 1]} भरें`;
        if (i === 8 && !readonly) input.placeholder = 'XXXXXXXX0000 (masked Aad Number)';
        if (readonly) input.readOnly = true;
        label.append(input);
        container.append(label);
      }
    }
    makeFields('generatorFields', 'gen', false);
    makeFields('scanFields', 'scan', true);

    function randomBytes(length) {
      return crypto.getRandomValues(new Uint8Array(length));
    }
    function toBase64Url(bytes) {
      let binary = '';
      for (let i = 0; i < bytes.length; i += 0x8000) {
        binary += String.fromCharCode(...bytes.subarray(i, i + 0x8000));
      }
      return btoa(binary).replace(/\+/g, '-').replace(/\//g, '_').replace(/=+$/g, '');
    }
    function fromBase64Url(value) {
      if (!/^[A-Za-z0-9_-]+$/.test(value)) throw new Error('QR payload format is invalid');
      const base64 = value.replace(/-/g, '+').replace(/_/g, '/') + '='.repeat((4 - value.length % 4) % 4);
      const binary = atob(base64);
      return Uint8Array.from(binary, char => char.charCodeAt(0));
    }
    async function deriveAesKey(passphrase, salt) {
      const material = await crypto.subtle.importKey('raw', encoder.encode(passphrase), 'PBKDF2', false, ['deriveKey']);
      return crypto.subtle.deriveKey(
        { name: 'PBKDF2', salt, iterations: PBKDF2_ITERATIONS, hash: 'SHA-256' },
        material,
        { name: 'AES-GCM', length: 256 },
        false,
        ['encrypt', 'decrypt']
      );
    }
    function requireCrypto() {
      if (!window.crypto?.subtle || !window.isSecureContext) {
        throw new Error('Encryption/camera features ke liye page ko HTTPS par kholen.');
      }
    }

    async function generateSecureQR() {
      const status = document.getElementById('generateStatus');
      const output = document.getElementById('qrcode');
      output.replaceChildren();
      status.textContent = '';
      try {
        requireCrypto();
        const passphrase = document.getElementById('generatePassphrase').value;
        if (passphrase.length < 8) throw new Error('Passphrase कम से कम 8 अक्षर का रखें।');
        const fields = Array.from({ length: FIELD_COUNT }, (_, i) => document.getElementById(`gen_field${i + 1}`).value.trim());
        if (fields.length !== FIELD_COUNT || fields.some(value => !value)) throw new Error('QR बनाने से पहले सभी 8 बॉक्स भरें।');
        fields[7] = maskAadNumber(fields[7]);
        if (!window.QRCode) throw new Error('QR generator load नहीं हुआ; internet connection जाँचें।');

        status.textContent = 'डेटा encrypt हो रहा है…';
        const salt = randomBytes(16);
        const iv = randomBytes(12);
        const key = await deriveAesKey(passphrase, salt);
        const plaintext = encoder.encode(JSON.stringify({ app: APP_MARKER, version: 1, fieldCount: FIELD_COUNT, fieldNames: FIELD_LABELS, fields }));
        const ciphertext = new Uint8Array(await crypto.subtle.encrypt({ name: 'AES-GCM', iv }, key, plaintext));
        const packet = { version: 1, salt: toBase64Url(salt), iv: toBase64Url(iv), data: toBase64Url(ciphertext) };
        const payload = QR_PREFIX + toBase64Url(encoder.encode(JSON.stringify(packet)));

        new QRCode(output, { text: payload, width: 260, height: 260, colorDark: '#000000', colorLight: '#ffffff', correctLevel: QRCode.CorrectLevel.H });
        status.textContent = '✅ Encrypted QR तैयार है। इसमें ठीक 8 बॉक्स का data है।';
      } catch (error) {
        status.textContent = `❌ ${error.message || 'QR नहीं बन पाया।'}`;
      }
    }

    function maskAadNumber(value) {
      const digits = value.replace(/\D/g, '');
      if (/^\d{12}$/.test(digits)) return `XXXXXXXX${digits.slice(-4)}`;
      if (/^[Xx*]{8}\s*\d{4}$/.test(value)) return `XXXXXXXX${digits.slice(-4)}`;
      throw new Error('Aad Number में 12 digits दें; QR में केवल आख़िरी 4 digits दिखेंगे (XXXXXXXX0000)।');
    }

    function clearScanFields() {
      for (let i = 1; i <= FIELD_COUNT; i++) document.getElementById(`scan_field${i}`).value = '';
    }
    async function decryptPayload(payload) {
      const status = document.getElementById('scanStatus');
      clearScanFields();
      try {
        requireCrypto();
        const passphrase = document.getElementById('scanPassphrase').value;
        if (passphrase.length < 8) throw new Error('पहले सही passphrase डालें।');
        if (!payload.startsWith(QR_PREFIX)) throw new Error('यह इस 8-box system का QR नहीं है।');
        if (payload.length > 12000) throw new Error('QR payload सीमा से बड़ा है।');
        const packet = JSON.parse(decoder.decode(fromBase64Url(payload.slice(QR_PREFIX.length))));
        if (packet.version !== 1) throw new Error('QR version स्वीकार नहीं है।');
        const salt = fromBase64Url(packet.salt);
        const iv = fromBase64Url(packet.iv);
        const ciphertext = fromBase64Url(packet.data);
        if (salt.length !== 16 || iv.length !== 12 || ciphertext.length < 17 || ciphertext.length > 8000) throw new Error('QR payload invalid है।');

        status.textContent = 'QR decrypt और validate हो रहा है…';
        const key = await deriveAesKey(passphrase, salt);
        const plaintext = await crypto.subtle.decrypt({ name: 'AES-GCM', iv }, key, ciphertext);
        const record = JSON.parse(decoder.decode(plaintext));
        if (record.app !== APP_MARKER || record.version !== 1 || record.fieldCount !== FIELD_COUNT || JSON.stringify(record.fieldNames) !== JSON.stringify(FIELD_LABELS) || !Array.isArray(record.fields) || record.fields.length !== FIELD_COUNT || record.fields.some(value => typeof value !== 'string' || !value) || !/^XXXXXXXX\d{4}$/.test(record.fields[7])) {
          throw new Error('इस QR में ठीक 8 valid boxes नहीं हैं।');
        }
        record.fields.forEach((value, i) => { document.getElementById(`scan_field${i + 1}`).value = value; });
        document.getElementById('decodedView').hidden = false;
        document.body.classList.add('decoded-mode');
        status.textContent = '✅ सही 8-box QR decrypt हो गया; सभी बॉक्स भर दिए गए।';
      } catch (error) {
        clearScanFields();
        status.textContent = `❌ Data नहीं दिखाया गया: ${error.message || 'QR invalid है या passphrase गलत है।'}`;
      }
    }
    function decryptManualPayload() {
      decryptPayload(document.getElementById('manualPayload').value.trim());
    }

    async function startScanner() {
      const status = document.getElementById('scanStatus');
      if (scannerRunning) return;
      if (!window.Html5Qrcode) {
        status.textContent = '❌ QR scanner library load नहीं हुई; internet connection जाँचें।';
        return;
      }
      if (!window.isSecureContext) {
        status.textContent = '❌ Camera scanner चलाने के लिए इस page को HTTPS पर खोलें।';
        return;
      }
      try {
        scanner = new Html5Qrcode('reader');
        handledScan = false;
        scannerRunning = true;
        status.textContent = 'Camera permission दें और QR को frame में रखें।';
        await scanner.start({ facingMode: 'environment' }, { fps: 10, qrbox: { width: 250, height: 250 } }, async decodedText => {
          if (handledScan) return;
          handledScan = true;
          await stopScanner();
          await decryptPayload(decodedText);
        }, () => {});
      } catch (error) {
        scannerRunning = false;
        status.textContent = `❌ Camera शुरू नहीं हुआ: ${error.message || error}`;
      }
    }
    async function stopScanner() {
      if (scanner && scannerRunning) {
        try { await scanner.stop(); } catch (error) { console.warn('Scanner stop:', error); }
        try { scanner.clear(); } catch (_) {}
      }
      scannerRunning = false;
      scanner = null;
    }
  </script>
</body>
</html>
