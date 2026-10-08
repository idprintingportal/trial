<!DOCTYPE html>
<html lang="mr">
<head>
  <meta charset="UTF-8">
  <title>AgriStack PVC Card Generator — Premium Farm Design</title>
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

    /* Centered title lanes with a farmer-themed AgriStack corner logo. */
    .template-heading {left:4px;right:4px;top:4px;height:32px;display:block;}
    .template-logo {
      position:absolute;left:9px;top:9px;width:74px;height:18px;z-index:2;
      display:flex;align-items:flex-end;gap:1px;white-space:nowrap;
      font-weight:800;letter-spacing:-.45px;
    }
    .template-logo .logo-word.agri {color:#2f6d35;font-size:10px;line-height:1;}
    .template-logo .logo-word.stack {color:#ef7a1a;font-size:10px;line-height:1;}
    .template-logo .logo-sprout {
      position:relative;display:inline-block;width:11px;height:16px;flex:0 0 11px;
      margin:0 1px 0 -1px;
    }
    .template-logo .logo-sprout::before {
      content:"";position:absolute;left:5px;bottom:0;width:1.6px;height:15px;
      background:linear-gradient(to top,#4f7d2b,#89b94d);border-radius:2px;
      box-shadow:0 0 0 .2px rgba(0,0,0,.12);
    }
    .template-logo .logo-sprout::after {
      content:"";position:absolute;left:0;top:1px;width:11px;height:13px;
      background:
        radial-gradient(ellipse at 33% 27%, #9ac54a 0 34%, transparent 36%),
        radial-gradient(ellipse at 73% 42%, #76a93b 0 34%, transparent 36%),
        radial-gradient(ellipse at 28% 70%, #6fa144 0 32%, transparent 35%),
        radial-gradient(ellipse at 77% 76%, #8ebf54 0 32%, transparent 35%);
      transform:rotate(-8deg);
      opacity:.98;
    }
    .heading-text {
      position:absolute;left:50%;top:1px;transform:translateX(-50%);
      width:210px;max-width:calc(100% - 112px);height:27px;padding:0;
      display:flex;flex-direction:column;align-items:center;justify-content:center;
      overflow:hidden;white-space:nowrap;text-align:center;
    }
    .heading-text .mr {font-size:9px;line-height:12px;white-space:nowrap;}
    .heading-text .en {font-size:12.1px;line-height:14px;letter-spacing:-.25px;white-space:nowrap;}
    #backCard .heading-text .en {font-size:11.4px;letter-spacing:-.2px;}
    .seal {display:none !important; background:transparent !important; border:none !important; box-shadow:none !important;}
    .disp-address-text {left:19px;right:15px;top:40px;min-height:19px;
      max-height:26px;overflow-wrap:anywhere;overflow:hidden;}
    /* Seven columns must fit entirely inside the card; never scroll on PVC. */
    .agri-table-overlay {left:17px;top:73px;width:289px;max-width:289px;
      min-width:0;table-layout:fixed;border-spacing:0;overflow:visible;font-size:7px;}
    .agri-table-overlay th,.agri-table-overlay td {box-sizing:border-box;min-width:0;
      overflow:hidden;overflow-wrap:anywhere;word-break:normal;white-space:normal;
      text-overflow:clip;line-height:1.15;padding:4px 1px;text-align:center;}
    .agri-table-overlay th {font-size:7px;line-height:1.12;padding:5px 1px;}
    .agri-table-overlay td {font-size:7.3px;}
    .agri-table-overlay th:nth-child(1){width:12%}
    .agri-table-overlay th:nth-child(2){width:17%}
    .agri-table-overlay th:nth-child(3){width:17%}
    .agri-table-overlay th:nth-child(4){width:18%}
    .agri-table-overlay th:nth-child(5){width:12%}
    .agri-table-overlay th:nth-child(6){width:11%}
    .agri-table-overlay th:nth-child(7){width:13%}
    .card-foot-rule {bottom:17px;left:12px;right:12px;}
    .farmer-id-disp {bottom:18px;font-size:13px;left:12px;width:calc(100% - 24px);text-align:center;}
    .vertical-date-front,.vertical-date-back {left:3px;}
    /* Print an A4 sheet with two CR80-size cards side-by-side (front left, back right). */
    @page {size:A4 portrait;margin:10mm;}
    @media print {
      html,body {
        width:auto!important;height:auto!important;margin:0!important;padding:0!important;
        background:white!important;overflow:visible!important;
      }
      body * {visibility:visible!important;}
      h1,.editor-form,.print-btn,.table-notice {display:none!important;}
      body {
        display:block!important;
        background:white!important;
      }
      .main-container {
        display:block!important;
        width:100%!important;
        max-width:none!important;
        margin:0 auto!important;
        padding:0!important;
      }
      .cards-wrapper {
        position:static!important;
        display:flex!important;
        flex-direction:row!important;
        justify-content:center!important;
        align-items:flex-start!important;
        gap:8mm!important;
        margin:0 auto!important;
        padding:0!important;
        width:100%!important;
        overflow:visible!important;
      }
      .pvc-card {
        width:85.6mm!important;
        height:54mm!important;
        flex:0 0 85.6mm!important;
        border:.15mm solid #aaa!important;
        box-shadow:none!important;
        break-inside:avoid!important;
        page-break-inside:avoid!important;
        overflow:hidden!important;
        print-color-adjust:exact!important;
        -webkit-print-color-adjust:exact!important;
      }
      .agri-table-overlay {overflow:visible!important;}
    }

    /* PVC back-table: no scroll and exact seven-column printed grid. */
    #backCard .disp-address-text {top:39px;left:18px;right:12px;font-family:Georgia,serif;
      font-size:8.6px;line-height:1.22;max-height:31px;overflow:visible;}
    #backCard .agri-table-overlay {top:76px;left:16px;width:291px;max-width:291px;
      display:table!important;table-layout:fixed;border-collapse:collapse;
      overflow:visible!important;background:#fff;border:1px solid #202020;
      font-family:Georgia,serif;font-size:8.1px;}
    #backCard .agri-table-overlay th {background:#a7cfc0;color:#164d3b;
      border:1px solid #262626;padding:4px 1px;font-size:8.2px;
      line-height:1.1;font-weight:700;overflow-wrap:normal;white-space:nowrap;}
    #backCard .agri-table-overlay td {background:#fff;color:#161616;
      border:1px solid #262626;padding:5px 1px;font-size:8.4px;
      line-height:1.15;font-weight:700;white-space:normal;overflow-wrap:anywhere;
      word-break:normal;text-align:center;vertical-align:middle;}
    #backCard .agri-table-overlay th:nth-child(1){width:11%}
    #backCard .agri-table-overlay th:nth-child(2){width:16%}
    #backCard .agri-table-overlay th:nth-child(3){width:17%}
    #backCard .agri-table-overlay th:nth-child(4){width:19%}
    #backCard .agri-table-overlay th:nth-child(5){width:12%}
    #backCard .agri-table-overlay th:nth-child(6){width:11%}
    #backCard .agri-table-overlay th:nth-child(7){width:14%}
    #backCard .agri-table-overlay.table-compact td {padding:3px 1px;font-size:7.6px;line-height:1.08;}
    #backCard .agri-table-overlay.table-compact th {padding:3px 1px;font-size:7.7px;}
    #backCard .agri-table-overlay.table-dense td {padding:2px 1px;font-size:6.7px;line-height:1.05;}
    #backCard .agri-table-overlay.table-dense th {padding:2px 1px;font-size:7px;}
    #landRowsContainer {max-width:100%;overflow-x:auto;}
    #tableNotice {display:none;margin-top:10px;color:#a32920;max-width:420px;font-size:12px;font-weight:bold;}
    #tableNotice:not(:empty) {display:block;}
    @media print {
      #backCard .agri-table-overlay {overflow:visible!important;}
    }

    /* Permanent bilingual personal-use notice; stays centered inside the PVC back-card footer. */
    #backCard .card-foot-rule { bottom:24px; left:12px; right:12px; }
    #backCard .personal-use-note {
      position:absolute; left:50%; transform:translateX(-50%); width:286px; max-width:calc(100% - 24px);
      bottom:4px; z-index:3; color:#c53022; font-size:5.9px; font-weight:700;
      line-height:1.18; text-align:center; white-space:normal; pointer-events:none;
    }
    #backCard .personal-use-note span { display:block; }
    @media print {
      #backCard .personal-use-note { color:#c53022!important; print-color-adjust:exact; -webkit-print-color-adjust:exact; }
    }

    /* Premium farm identity: crisp vector logo and quiet wheat artwork. */
    .pvc-card { background-color:#fcfcf4; }
    #frontCard { background-image:linear-gradient(158deg,#fff 0%,#fffef8 57%,#eff3de 100%); }
    #backCard { background-image:linear-gradient(158deg,#fff 0%,#fdfdf8 54%,#edf3dc 100%); }
    .agri-card-art { position:absolute;inset:0;width:100%;height:100%;z-index:0;
      pointer-events:none;opacity:.26; }
    .template-heading { left:6px;right:6px;top:4px;height:33px;
      border-bottom:1px solid #456647; }
    .template-logo { left:3px;top:3px;width:49px;height:25px;display:flex;align-items:center;
      gap:0;padding:0;white-space:nowrap;z-index:3; }
    .template-logo .wheat-mark {display:block;flex:0 0 15px;width:15px;height:23px;}
    .template-logo .logo-type {display:flex;align-items:baseline;letter-spacing:-.55px;
      font-family:Georgia,'Segoe UI',serif;font-size:9.4px;font-weight:900;line-height:1;}
    .template-logo .logo-agri {color:#2b6a35;}
    .template-logo .logo-stack {color:#d57b23;}
    .heading-text {position:absolute;left:50%;transform:translateX(-50%);top:1px;
      width:212px;max-width:none;height:27px;overflow:visible;padding:0;
      display:flex;flex-direction:column;align-items:center;justify-content:center;
      text-align:center;white-space:nowrap;}
    .heading-text .mr { font-size:9.2px;line-height:12px; }
    .heading-text .en {font-size:12.1px;line-height:14px;letter-spacing:-.25px;}
    #backCard .heading-text .en {font-size:11.3px;letter-spacing:-.2px;}
    .farmer-id-disp {left:12px;right:12px;width:auto;text-align:center;
      bottom:18px;font-variant-numeric:tabular-nums;}
    #backCard .card-foot-rule {left:12px;right:12px;border-top-color:#bf8c40;}
    #backCard .personal-use-note {left:50%;transform:translateX(-50%);
      width:calc(100% - 26px);max-width:297px;text-align:center;}
    @media print {
      .agri-card-art {opacity:.26!important;print-color-adjust:exact!important;
        -webkit-print-color-adjust:exact!important;}
      #frontCard,#backCard {print-color-adjust:exact!important;
        -webkit-print-color-adjust:exact!important;}
    }

    /* TEN MATCHED FRONT + BACK AGRICULTURE THEMES: same variant always on both. */
    .cards-wrapper {
      --theme-top:#fffef7;--theme-bottom:#eef3d9;--theme-accent:#d0a24f;
      --theme-table:#a6d0c0;--theme-ink:#1e5a36;--theme-sub:#c44c39;
      --theme-rule:#c58e35;--theme-art:#718d42;
    }
    .cards-wrapper[data-theme="wheat"] { --theme-top:#fffef7; --theme-bottom:#eef3d9;
      --theme-accent:#d0a24f; --theme-table:#a6d0c0; --theme-ink:#1e5a36;
      --theme-sub:#c44c39; --theme-rule:#c58e35; --theme-art:#718d42; }
    .cards-wrapper[data-theme="paddy"] { --theme-top:#ffffff; --theme-bottom:#e0f1df;
      --theme-accent:#6c9b63; --theme-table:#a8d6bd; --theme-ink:#235c3c;
      --theme-sub:#ba633c; --theme-rule:#71a366; --theme-art:#5c9567; }
    .cards-wrapper[data-theme="cotton"] { --theme-top:#fffefa; --theme-bottom:#f0edf1;
      --theme-accent:#ac8aa1; --theme-table:#b7d2c6; --theme-ink:#53415e;
      --theme-sub:#b45b63; --theme-rule:#947d9a; --theme-art:#ba98b5; }
    .cards-wrapper[data-theme="sugarcane"] { --theme-top:#fefff9; --theme-bottom:#e0edd5;
      --theme-accent:#6a8f50; --theme-table:#b9d8a7; --theme-ink:#355a2a;
      --theme-sub:#ae7038; --theme-rule:#7b9e45; --theme-art:#719d5c; }
    .cards-wrapper[data-theme="sunflower"] { --theme-top:#fffef8; --theme-bottom:#fff1d6;
      --theme-accent:#d99c38; --theme-table:#d6d2a6; --theme-ink:#645322;
      --theme-sub:#be6539; --theme-rule:#e0a23f; --theme-art:#ca9830; }
    .cards-wrapper[data-theme="orchard"] { --theme-top:#fffefa; --theme-bottom:#ebf4d8;
      --theme-accent:#7a9c4c; --theme-table:#b5d3a5; --theme-ink:#405e2b;
      --theme-sub:#a9583e; --theme-rule:#c49c43; --theme-art:#8dac5b; }
    .cards-wrapper[data-theme="terrace"] { --theme-top:#fefffc; --theme-bottom:#e8f3eb;
      --theme-accent:#6b9d86; --theme-table:#accfc3; --theme-ink:#2a6153;
      --theme-sub:#bb674d; --theme-rule:#689e85; --theme-art:#68a997; }
    .cards-wrapper[data-theme="organic"] { --theme-top:#fffefd; --theme-bottom:#eaf4e5;
      --theme-accent:#78a179; --theme-table:#c2d8b1; --theme-ink:#29583e;
      --theme-sub:#aa6352; --theme-rule:#81a75c; --theme-art:#81ae83; }
    .cards-wrapper[data-theme="monsoon"] { --theme-top:#fdfeff; --theme-bottom:#e6f0f6;
      --theme-accent:#7b9cad; --theme-table:#b6d2d7; --theme-ink:#2d5568;
      --theme-sub:#be6651; --theme-rule:#729cac; --theme-art:#779db8; }
    .cards-wrapper[data-theme="millet"] { --theme-top:#fffefa; --theme-bottom:#f3ecd8;
      --theme-accent:#ad8c5e; --theme-table:#d2c19d; --theme-ink:#684e34;
      --theme-sub:#b85f36; --theme-rule:#bc9655; --theme-art:#ab9568; }

    .cards-wrapper[data-theme] .pvc-card {
      background-color:var(--theme-top)!important;
      background-image:linear-gradient(164deg,var(--theme-top) 0%,#fffefa 46%,var(--theme-bottom) 100%)!important;
    }
    .cards-wrapper[data-theme] .pvc-card::before {
      background:radial-gradient(ellipse at 10% 97%,var(--theme-bottom),transparent 41%),
      radial-gradient(ellipse at 92% 100%,var(--theme-bottom),transparent 45%);
      opacity:.9;
    }
    .cards-wrapper[data-theme] .agri-card-art {color:var(--theme-art);opacity:.18!important;}
    .cards-wrapper[data-theme] .template-heading {border-bottom-color:var(--theme-ink);}
    .cards-wrapper[data-theme] .heading-text .en {color:var(--theme-ink);}
    .cards-wrapper[data-theme] .heading-text .mr {color:var(--theme-sub);}
    .cards-wrapper[data-theme] .logo-agri {color:var(--theme-ink);}
    .cards-wrapper[data-theme] .logo-stack {color:var(--theme-rule);}
    .cards-wrapper[data-theme] .farmer-id-disp {color:var(--theme-ink);}
    .cards-wrapper[data-theme] .agri-table-overlay th {background:var(--theme-table)!important;color:var(--theme-ink)!important;}
    .cards-wrapper[data-theme] .card-foot-rule {border-top-color:var(--theme-rule)!important;}
    .cards-wrapper[data-theme] .photo-box {border-color:var(--theme-art);}
    .cards-wrapper[data-theme] .pvc-card .personal-use-note {color:#bb392c!important;}
    /* Logo stays in its own left lane while titles remain mathematically centered. */
    .cards-wrapper[data-theme] .template-logo {left:2px;top:3px;width:51px;}
    .cards-wrapper[data-theme] .heading-text {
      left:50%;transform:translateX(-50%);width:220px;max-width:none;
      overflow:hidden;text-align:center;
    }
    .cards-wrapper[data-theme] .heading-text .en {font-size:11.8px;letter-spacing:-.42px;}
    .cards-wrapper[data-theme] #backCard .heading-text .en {font-size:11.25px;letter-spacing:-.42px;}
    .cards-wrapper[data-theme] .info-details-front .info-line {grid-template-columns:42px minmax(0,1fr);gap:2px;}
    .cards-wrapper[data-theme] .farmer-id-disp {left:12px;right:12px;width:auto;text-align:center;}
    .cards-wrapper[data-theme] #backCard .personal-use-note {left:50%;transform:translateX(-50%);text-align:center;}
    .theme-picker {background:white;border:1px solid #d1dacd;border-radius:10px;padding:13px;
      margin-bottom:14px;max-width:1220px;width:100%;box-shadow:0 2px 8px rgba(0,0,0,.05);}
    .theme-picker h2{font-size:15px;color:#245a35;margin-bottom:4px;text-align:center;}
    .theme-picker p{font-size:11px;color:#60726b;text-align:center;margin-bottom:11px;}
    .theme-grid{display:grid;grid-template-columns:repeat(5,minmax(0,1fr));gap:8px;}
    .theme-option{border:1px solid #cbd5ca;border-radius:8px;padding:7px 6px;
      display:flex;align-items:center;gap:8px;min-width:0;min-height:48px;
      cursor:pointer;background:#fff;color:#233b2d;text-align:left;font-size:11px;font-weight:700;
      transition:border-color .15s,box-shadow .15s;}
    .theme-option .theme-thumb{width:46px;height:30px;flex:none;border-radius:5px;
      border:1px solid #bec9bd;position:relative;overflow:hidden;
      background:linear-gradient(155deg,var(--swatch-top) 15%,var(--swatch-bottom) 100%);}
    .theme-option .theme-thumb::before{content:"";position:absolute;inset:3px 3px auto 3px;height:3px;
      border-bottom:1px solid var(--swatch-ink);opacity:.8;}
    .theme-option .theme-thumb::after{content:"";position:absolute;bottom:-5px;right:-1px;
      width:27px;height:19px;border:2px solid var(--swatch-art);opacity:.42;
      border-radius:90% 0 0 0;transform:rotate(-12deg);}
    .theme-option:is(:hover,:focus-visible){outline:0;border-color:#49794e;box-shadow:0 0 0 2px rgba(65,119,69,.14);}
    .theme-option[aria-pressed="true"]{border:2px solid #246b3e;padding:6px 5px;box-shadow:0 0 0 2px rgba(45,117,61,.15);}
    .theme-option .theme-title{line-height:1.25;min-width:0;}
    @media (max-width:950px){.theme-grid{grid-template-columns:repeat(3,minmax(0,1fr));}}
    @media (max-width:590px){.theme-grid{grid-template-columns:repeat(2,minmax(0,1fr));}}
    @media print {
      .theme-picker{display:none!important;}
      .cards-wrapper[data-theme] .pvc-card,.cards-wrapper[data-theme] .agri-table-overlay th{
        -webkit-print-color-adjust:exact!important;print-color-adjust:exact!important;}
    }

    /* Back side: address expands naturally. Table is positioned immediately after
       its measured text height; no fixed height, clipped lines, or horizontal slider. */
    #backCard .disp-address-text {
      height:auto!important;
      min-height:0!important;
      max-height:none!important;
      overflow:visible!important;
      white-space:normal!important;
      overflow-wrap:anywhere;
      word-break:normal;
    }
    #backCard .agri-table-overlay {
      /* The actual top is set from the address element's rendered height. */
      margin:0!important;
      min-width:0!important;
      overflow:visible!important;
    }

  </style>
</head>
<body>

  <svg xmlns="http://www.w3.org/2000/svg" width="0" height="0" aria-hidden="true" focusable="false" style="position:absolute;pointer-events:none;overflow:hidden"><defs>
<symbol id="art-wheat" viewBox="0 0 323.5 204"><g fill="none" stroke="currentColor" stroke-width="1.4"><path d="M0 198 Q76 180 162 198 T324 191 M0 204 Q86 189 172 204 T324 197 M10 204 C15 177 23 148 39 130 M23 204 C32 175 45 151 52 140 M314 204 C303 175 302 150 282 137"/></g><g fill="currentColor"><ellipse cx="34" cy="141" rx="3.3" ry="8" transform="rotate(27 34 141)"/><ellipse cx="26" cy="148" rx="3.3" ry="7" transform="rotate(-34 26 148)"/><ellipse cx="39" cy="154" rx="3.3" ry="7" transform="rotate(35 39 154)"/><ellipse cx="20" cy="161" rx="3.3" ry="7" transform="rotate(-40 20 161)"/><ellipse cx="285" cy="142" rx="3.4" ry="8" transform="rotate(-25 285 142)"/><ellipse cx="295" cy="153" rx="3.2" ry="7" transform="rotate(35 295 153)"/><ellipse cx="278" cy="155" rx="3.2" ry="7" transform="rotate(-30 278 155)"/></g></symbol>
<symbol id="art-paddy" viewBox="0 0 323.5 204"><g fill="none" stroke="currentColor" stroke-width="1.5"><path d="M0 165 Q79 141 163 162 T324 157 M0 177 Q83 150 169 175 T324 171 M0 189 Q80 167 175 189 T324 181 M0 201 Q87 179 172 202 T324 193"/><path d="M21 203 Q28 176 29 149 M31 204Q42 173 43 146 M295 204Q281 166 289 143 M305 204 Q302 174 301 147"/></g><g fill="currentColor"><path d="M29 156Q14 148 17 135Q29 139 29 156 M29 165Q44 149 48 137Q31 140 29 165 M42 156Q51 142 59 137Q53 153 42 156 M287 151Q270 140 269 130Q285 131 287 151 M296 159Q306 144 314 143Q312 158 296 159"/></g></symbol>
<symbol id="art-cotton" viewBox="0 0 323.5 204"><g fill="none" stroke="currentColor" stroke-width="1.6"><path d="M12 204 Q35 166 45 154 M318 204Q297 163 285 156 M35 196Q25 176 18 176 M47 184Q63 174 69 164 M288 185Q277 168 263 167"/></g><g fill="currentColor" opacity=".75"><circle cx="44" cy="150" r="9"/><circle cx="34" cy="149" r="7"/><circle cx="52" cy="144" r="7"/><circle cx="44" cy="138" r="7"/><circle cx="283" cy="146" r="9"/><circle cx="274" cy="144" r="7"/><circle cx="291" cy="140" r="7"/><circle cx="282" cy="133" r="7"/></g><g fill="currentColor"><path d="M27 190Q11 180 9 173Q22 173 27 190 M51 182Q61 165 70 162Q70 175 51 182 M296 183Q315 172 318 164Q304 165 296 183"/></g></symbol>
<symbol id="art-sugarcane" viewBox="0 0 323.5 204"><g fill="none" stroke="currentColor" stroke-width="2"><path d="M18 204 L23 145 M31 204 L38 133 M49 204 L54 145 M288 204L281 139 M302 204L305 151 M317 204 L313 144"/></g><g fill="currentColor"><path d="M23 163Q1 133 0 116Q24 130 23 163 M38 150Q65 115 76 109Q60 142 38 150 M36 175Q8 164 0 151Q23 152 36 175 M54 165Q72 151 84 147Q72 166 54 165 M281 160Q266 136 246 127Q259 152 281 160 M304 177Q320 153 324 144Q312 151 304 177 M286 181Q265 161 254 161Q269 181 286 181"/></g></symbol>
<symbol id="art-sunflower" viewBox="0 0 323.5 204"><g fill="none" stroke="currentColor" stroke-width="1.6"><path d="M22 204Q32 169 41 157 M309 204 Q300 166 291 156 M41 189 Q12 169 5 173 M291 180Q316 167 324 168"/></g><g fill="currentColor"><circle cx="42" cy="151" r="7"/><circle cx="291" cy="152" r="7"/><g transform="translate(42 151)"><ellipse cx="0" cy="-14" rx="4" ry="7"/><ellipse cx="0" cy="14" rx="4" ry="7"/><ellipse cx="-14" cy="0" rx="7" ry="4"/><ellipse cx="14" cy="0" rx="7" ry="4"/><ellipse cx="10" cy="-10" rx="4" ry="6" transform="rotate(45 10 -10)"/><ellipse cx="-10" cy="10" rx="4" ry="6" transform="rotate(45 -10 10)"/><ellipse cx="10" cy="10" rx="4" ry="6" transform="rotate(-45 10 10)"/><ellipse cx="-10" cy="-10" rx="4" ry="6" transform="rotate(-45 -10 -10)"/></g><g transform="translate(291 152)"><ellipse cx="0" cy="-14" rx="4" ry="7"/><ellipse cx="0" cy="14" rx="4" ry="7"/><ellipse cx="-14" cy="0" rx="7" ry="4"/><ellipse cx="14" cy="0" rx="7" ry="4"/><ellipse cx="10" cy="-10" rx="5" ry="6"/><ellipse cx="-10" cy="10" rx="5" ry="6"/><ellipse cx="10" cy="10" rx="5" ry="6"/><ellipse cx="-10" cy="-10" rx="5" ry="6"/></g></g></symbol>
<symbol id="art-orchard" viewBox="0 0 323.5 204"><g fill="none" stroke="currentColor" stroke-width="2"><path d="M36 204V168 M291 204V164 M36 176L22 157 M36 179L52 161 M291 174L274 152 M291 172L309 154 M0 199Q85 178 170 197 T324 194"/></g><g fill="currentColor" opacity=".75"><circle cx="22" cy="161" r="13"/><circle cx="39" cy="149" r="16"/><circle cx="54" cy="163" r="13"/><circle cx="277" cy="152" r="13"/><circle cx="295" cy="147" r="16"/><circle cx="309" cy="160" r="13"/></g><g fill="#caa355"><ellipse cx="24" cy="165" rx="3" ry="4"/><ellipse cx="45" cy="159" rx="3" ry="4"/><ellipse cx="293" cy="163" rx="3" ry="4"/></g></symbol>
<symbol id="art-terrace" viewBox="0 0 323.5 204"><g fill="none" stroke="currentColor" stroke-width="1.6"><path d="M0 138Q54 128 96 139T185 139T324 136 M0 151Q60 142 119 151T238 150T324 149 M0 165 Q69 153 142 165T278 161T324 164 M0 178 Q75 166 164 176T324 177 M0 193Q79 182 172 191T324 187 M0 204Q90 193 185 203T324 198"/></g><g fill="currentColor" opacity=".5"><path d="M0 177Q80 164 162 178T324 174V187Q240 173 154 190T0 189Z"/><path d="M0 197Q99 182 178 199T324 192V204H0Z"/></g></symbol>
<symbol id="art-organic" viewBox="0 0 323.5 204"><g fill="none" stroke="currentColor" stroke-width="1.4"><path d="M4 204 Q18 164 56 131 M2 194Q33 178 64 180 M320 204 Q298 168 262 129 M324 188Q301 178 261 182 M0 201Q80 184 164 199T324 194"/></g><g fill="currentColor"><path d="M19 166 Q-1 159 3 146Q21 149 19 166 M32 153Q25 132 37 125Q44 139 32 153 M44 143Q46 121 62 119Q60 136 44 143 M27 177Q43 157 59 158Q52 175 27 177 M298 166Q317 154 320 140Q300 143 298 166 M283 148Q278 133 263 124Q266 142 283 148 M289 184Q268 162 251 165Q265 183 289 184"/></g></symbol>
<symbol id="art-monsoon" viewBox="0 0 323.5 204"><g fill="none" stroke="currentColor" stroke-width="1.4"><path d="M0 177Q71 156 149 176T324 175 M0 192Q76 171 163 190T324 188 M0 204Q97 187 171 205T324 199"/><path d="M18 203L21 174 M39 204L37 178 M296 204L297 175 M309 204L310 177"/></g><g fill="currentColor"><path d="M17 169Q4 162 0 149Q18 153 17 169 M39 174Q49 156 60 153Q52 172 39 174 M294 167Q274 151 267 143Q288 147 294 167"/><path d="M237 140l-2 7 M252 138l-2 7 M268 141l-2 7 M281 137l-2 7" stroke="currentColor" stroke-width="1.8"/></g><g fill="currentColor" opacity=".5"><path d="M230 126Q237 116 245 122Q258 109 268 120Q283 114 290 128Q287 136 278 135H240Q232 135 230 126Z"/></g></symbol>
<symbol id="art-millet" viewBox="0 0 323.5 204"><g fill="none" stroke="currentColor" stroke-width="1.4"><path d="M14 204 Q22 159 26 127 M35 204 Q44 168 49 136 M310 204Q303 158 290 127 M287 204Q283 170 267 139 M0 202Q89 181 175 199T324 192"/></g><g fill="currentColor"><ellipse cx="26" cy="135" rx="6" ry="16" transform="rotate(11 26 135)"/><ellipse cx="49" cy="145" rx="5" ry="14" transform="rotate(19 49 145)"/><ellipse cx="289" cy="137" rx="6" ry="16" transform="rotate(-15 289 137)"/><ellipse cx="268" cy="148" rx="5" ry="13" transform="rotate(-20 268 148)"/><path d="M21 183Q4 171 1 162Q19 166 21 183 M36 180Q51 163 61 162Q54 179 36 180 M301 183Q322 169 324 157Q308 169 301 183"/></g></symbol>
</defs></svg>
  <h1>AgriStack Card Generator</h1>

  <section class="theme-picker" aria-labelledby="themePickerHeading">
    <h2 id="themePickerHeading">Agriculture Card Templates · 10 Matching Sets</h2>
    <p>Choose one design — Front and Back update together (प्रिंट में भी वही डिज़ाइन रहेगा).</p>
    <div class="theme-grid" id="themeGrid" role="group" aria-label="Select agriculture card design"></div>
  </section>

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
    <div class="cards-wrapper" data-theme="wheat">
      <!-- FRONT CARD -->
      <div class="pvc-card" id="frontCard">
        <svg class="agri-card-art" viewBox="0 0 323.5 204" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg" aria-hidden="true"><use href="#art-wheat"></use></svg>
        <div class="template-heading" aria-hidden="true"><div class="template-logo"><svg class="wheat-mark" viewBox="0 0 19 27" xmlns="http://www.w3.org/2000/svg" aria-hidden="true"><path d="M9.5 25V4" fill="none" stroke="#547b31" stroke-width="1.5" stroke-linecap="round"/><g fill="#e3b44e" stroke="#b78b2f" stroke-width=".35"><ellipse cx="5.5" cy="8.8" rx="2.5" ry="4" transform="rotate(-33 5.5 8.8)"/><ellipse cx="13.5" cy="8.8" rx="2.5" ry="4" transform="rotate(33 13.5 8.8)"/><ellipse cx="5.1" cy="14.3" rx="2.55" ry="4.2" transform="rotate(-38 5.1 14.3)"/><ellipse cx="13.9" cy="14.3" rx="2.55" ry="4.2" transform="rotate(38 13.9 14.3)"/><ellipse cx="9.5" cy="3.8" rx="2.2" ry="4"/></g><g fill="#79a84c"><path d="M9 23C4.7 23.3 3 19 2.4 17.3c4-.1 6.7 1.7 6.6 5.7Z"/><path d="M10.2 21c3.6-4.8 5.3-5.1 7-5.1-.6 3.7-2.9 6.1-7 6.5Z"/></g></svg><span class="logo-type"><span class="logo-agri">Agri</span><span class="logo-stack">Stack</span></span></div><div class="heading-text"><span class="mr">शेतकरी वैयक्तिक ओळखपत्र</span><span class="en">Farmer Personal Identity Card</span></div><div class="seal"></div></div>
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
        <svg class="agri-card-art" viewBox="0 0 323.5 204" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg" aria-hidden="true"><use href="#art-wheat"></use></svg>
        <div class="template-heading" aria-hidden="true"><div class="template-logo"><svg class="wheat-mark" viewBox="0 0 19 27" xmlns="http://www.w3.org/2000/svg" aria-hidden="true"><path d="M9.5 25V4" fill="none" stroke="#547b31" stroke-width="1.5" stroke-linecap="round"/><g fill="#e3b44e" stroke="#b78b2f" stroke-width=".35"><ellipse cx="5.5" cy="8.8" rx="2.5" ry="4" transform="rotate(-33 5.5 8.8)"/><ellipse cx="13.5" cy="8.8" rx="2.5" ry="4" transform="rotate(33 13.5 8.8)"/><ellipse cx="5.1" cy="14.3" rx="2.55" ry="4.2" transform="rotate(-38 5.1 14.3)"/><ellipse cx="13.9" cy="14.3" rx="2.55" ry="4.2" transform="rotate(38 13.9 14.3)"/><ellipse cx="9.5" cy="3.8" rx="2.2" ry="4"/></g><g fill="#79a84c"><path d="M9 23C4.7 23.3 3 19 2.4 17.3c4-.1 6.7 1.7 6.6 5.7Z"/><path d="M10.2 21c3.6-4.8 5.3-5.1 7-5.1-.6 3.7-2.9 6.1-7 6.5Z"/></g></svg><span class="logo-type"><span class="logo-agri">Agri</span><span class="logo-stack">Stack</span></span></div><div class="heading-text"><span class="mr">शेतीची माहिती</span><span class="en">Information about agriculture</span></div><div class="seal"></div></div>
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
        <div class="personal-use-note" aria-label="Card for personal use only, not government issued">
          <span>* टीप: हे कार्ड केवळ वैयक्तिक वापरासाठी आहे; हे सरकारी कार्ड नाही.</span>
          <span>This card is for personal use not for Govt. issue card.</span>
        </div>
      </div>
    </div>
  </div>

  <p class="table-notice" id="tableNotice" role="alert"></p>
  <button class="print-btn" onclick="printCards()">Print Cards</button>

  <script>

    // Each of the 10 agriculture templates is a matched FRONT + BACK pair.
    const AGRI_THEMES = [
      {id:'wheat',label:'01 · Golden Wheat',top:'#fffef7',bottom:'#eef3d9',ink:'#1e5a36',art:'#718d42'},
      {id:'paddy',label:'02 · Emerald Paddy',top:'#ffffff',bottom:'#e0f1df',ink:'#235c3c',art:'#5c9567'},
      {id:'cotton',label:'03 · Cotton Blossom',top:'#fffefa',bottom:'#f0edf1',ink:'#53415e',art:'#ba98b5'},
      {id:'sugarcane',label:'04 · Sugarcane Green',top:'#fefff9',bottom:'#e0edd5',ink:'#355a2a',art:'#719d5c'},
      {id:'sunflower',label:'05 · Sunflower Gold',top:'#fffef8',bottom:'#fff1d6',ink:'#645322',art:'#ca9830'},
      {id:'orchard',label:'06 · Mango Orchard',top:'#fffefa',bottom:'#ebf4d8',ink:'#405e2b',art:'#8dac5b'},
      {id:'terrace',label:'07 · Terrace Fields',top:'#fefffc',bottom:'#e8f3eb',ink:'#2a6153',art:'#68a997'},
      {id:'organic',label:'08 · Organic Leaves',top:'#fffefd',bottom:'#eaf4e5',ink:'#29583e',art:'#81ae83'},
      {id:'monsoon',label:'09 · Monsoon Crops',top:'#fdfeff',bottom:'#e6f0f6',ink:'#2d5568',art:'#779db8'},
      {id:'millet',label:'10 · Millet Heritage',top:'#fffefa',bottom:'#f3ecd8',ink:'#684e34',art:'#ab9568'},
    ];
    const cardsWrapper = document.querySelector('.cards-wrapper');
    const themeGrid = document.getElementById('themeGrid');
    AGRI_THEMES.forEach(theme => {
      const button = document.createElement('button');
      button.type = 'button';
      button.className = 'theme-option';
      button.dataset.theme = theme.id;
      button.setAttribute('aria-pressed', theme.id === 'wheat' ? 'true' : 'false');
      const swatch = document.createElement('span');
      swatch.className = 'theme-thumb';
      swatch.setAttribute('aria-hidden','true');
      swatch.style.setProperty('--swatch-top',theme.top);
      swatch.style.setProperty('--swatch-bottom',theme.bottom);
      swatch.style.setProperty('--swatch-ink',theme.ink);
      swatch.style.setProperty('--swatch-art',theme.art);
      const title = document.createElement('span');
      title.className = 'theme-title';
      title.textContent = theme.label;
      button.append(swatch,title);
      button.addEventListener('click',() => selectAgriTheme(theme.id));
      themeGrid.append(button);
    });
    function selectAgriTheme(id) {
      const theme = AGRI_THEMES.find(t => t.id === id);
      if (!theme) return;
      cardsWrapper.dataset.theme = theme.id;
      document.querySelectorAll('.agri-card-art use').forEach(use => {
        use.setAttribute('href', '#art-' + theme.id);
      });
      themeGrid.querySelectorAll('.theme-option').forEach(button => {
        button.setAttribute('aria-pressed',button.dataset.theme === theme.id ? 'true' : 'false');
      });
    }

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
      if (container.querySelectorAll('.land-row-input').length >= 5) { alert('एक कार्ड पर अधिकतम 5 rows रखी जा सकती हैं।'); return; }
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
      requestAnimationFrame(fitLandTable);
    }

    function fitLandTable() {
      const table = document.getElementById('agriTableDisplay');
      const card = document.getElementById('backCard');
      const address = card.querySelector('.disp-address-text');
      const notice = document.getElementById('tableNotice');
      // Follow the actual address height, including any newly wrapped lines.
      // Since both elements are positioned relative to the same card, their
      // top offsets remain reliable in both preview and A4 print layouts.
      const addressBottom = address.offsetTop + address.offsetHeight;
      const gapAfterAddress = 4; // px; intentional small clear gap
      table.style.top = `${Math.ceil(addressBottom + gapAfterAddress)}px`;

      table.classList.remove('table-compact', 'table-dense');
      // The bilingual note and footer rule at the bottom remain unobstructed.
      const availableBottom = card.getBoundingClientRect().top + 175;
      if (table.getBoundingClientRect().bottom > availableBottom) table.classList.add('table-compact');
      if (table.getBoundingClientRect().bottom > availableBottom) table.classList.add('table-dense');
      const fits = table.getBoundingClientRect().bottom <= availableBottom;
      notice.textContent = fits ? '' : 'Address और Agriculture Table एक PVC कार्ड में फिट नहीं हो रहे हैं। कृपया पता या rows छोटी करें।';
      return fits;
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
      // Reposition table on every address edit (no fixed table top).
      requestAnimationFrame(fitLandTable);
      updateQRCode();
    }

    [inputApprovalDate, inputNameMr, inputNameEn, inputGender, inputDob, inputArea, inputAadhaar, inputMobile, inputFarmerId, inputDownloadDate, inputAddress].forEach(elem => {
      elem.addEventListener('input', bindSync);
    });

    function printCards() {
      if (!fitLandTable()) return;
      window.print();
    }

    // Prevent cropped row content or layout spill in print. 
    window.addEventListener('beforeprint', () => {
      const t = document.getElementById('agriTableDisplay');
      const card = document.getElementById('backCard');
      if (!fitLandTable()) {
        alert('टेबल PVC कार्ड में फिट नहीं हो रही है। कृपया कुछ rows हटाएँ या लंबे नाम छोटे करें।');
      }
    });

    // Recheck after fonts are loaded, resizing, or print media changes.
    window.addEventListener('resize', () => requestAnimationFrame(fitLandTable));
    if (document.fonts && document.fonts.ready) {
      document.fonts.ready.then(() => requestAnimationFrame(fitLandTable));
    }

    // Initial Trigger
    bindSync();
    bindRowEvents();
    renderLandTable();
  </script>
</body>
</html>
