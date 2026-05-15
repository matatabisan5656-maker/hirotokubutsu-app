[index.html](https://github.com/user-attachments/files/27800426/index.html)
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#0f172a">
    <title>拾得物台帳</title>
    <link rel="manifest" href="data:application/json,{}">
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700;900&display=swap"
        rel="stylesheet">
    <style>
        *,
        *::before,
        *::after {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        html,
        body {
            height: 100%;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'Noto Sans JP', -apple-system, sans-serif;
            background: linear-gradient(160deg, #0a0f1e 0%, #101b35 50%, #0d1a2d 100%);
            color: #e2e8f0;
            min-height: 100vh;
            min-height: 100dvh;
            overflow-x: hidden;
        }

        /* ── Screen System ── */
        .screen {
            display: none;
            min-height: 100vh;
            min-height: 100dvh;
            flex-direction: column;
        }

        .screen.active {
            display: flex;
            animation: screenIn 0.35s cubic-bezier(0.22, 1, 0.36, 1) forwards;
        }

        @keyframes screenIn {
            from {
                opacity: 0;
                transform: translateY(18px) scale(0.98);
            }

            to {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }

        /* ── Header ── */
        .app-header {
            display: flex;
            align-items: center;
            padding: 14px 20px;
            gap: 10px;
            background: rgba(10, 15, 30, 0.85);
            border-bottom: 1px solid rgba(255, 255, 255, 0.06);
            position: sticky;
            top: 0;
            z-index: 10;
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
        }

        .app-header h1 {
            font-size: 17px;
            font-weight: 700;
            color: #f1f5f9;
        }

        .back-btn {
            background: none;
            border: none;
            color: #94a3b8;
            font-size: 22px;
            cursor: pointer;
            padding: 6px 10px 6px 0;
            transition: color 0.15s;
        }

        .back-btn:hover {
            color: #e2e8f0;
        }

        /* ── Buttons ── */
        .btn {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            padding: 17px 24px;
            border: none;
            border-radius: 18px;
            font-family: inherit;
            font-size: 16px;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.2s cubic-bezier(0.22, 1, 0.36, 1);
            width: 100%;
            position: relative;
            overflow: hidden;
        }

        .btn::after {
            content: '';
            position: absolute;
            inset: 0;
            border-radius: inherit;
            opacity: 0;
            transition: opacity 0.2s;
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.15), transparent);
        }

        .btn:hover::after {
            opacity: 1;
        }

        .btn:active {
            transform: scale(0.96);
        }

        .btn:disabled {
            opacity: 0.5;
            pointer-events: none;
        }

        .btn-primary {
            background: linear-gradient(135deg, #3b82f6 0%, #1d4ed8 100%);
            color: #fff;
            box-shadow: 0 6px 30px rgba(59, 130, 246, 0.35), inset 0 1px 0 rgba(255, 255, 255, 0.15);
        }

        .btn-success {
            background: linear-gradient(135deg, #10b981 0%, #047857 100%);
            color: #fff;
            box-shadow: 0 6px 30px rgba(16, 185, 129, 0.3), inset 0 1px 0 rgba(255, 255, 255, 0.15);
        }

        .btn-secondary {
            background: rgba(255, 255, 255, 0.06);
            color: #cbd5e1;
            border: 1px solid rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(8px);
        }

        .btn-danger {
            background: linear-gradient(135deg, #ef4444 0%, #b91c1c 100%);
            color: #fff;
            box-shadow: 0 6px 30px rgba(239, 68, 68, 0.3);
        }

        .btn-lg {
            padding: 20px 28px;
            font-size: 18px;
            border-radius: 20px;
        }

        /* ── Forms ── */
        .form-group {
            margin-bottom: 16px;
        }

        .form-label {
            display: block;
            font-size: 11px;
            font-weight: 700;
            color: #64748b;
            text-transform: uppercase;
            letter-spacing: 1.2px;
            margin-bottom: 8px;
        }

        .form-input,
        .form-textarea {
            width: 100%;
            padding: 14px 16px;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 14px;
            color: #f1f5f9;
            font-family: inherit;
            font-size: 15px;
            transition: all 0.2s;
            -webkit-appearance: none;
            appearance: none;
        }

        .form-input:focus,
        .form-textarea:focus {
            outline: none;
            border-color: #3b82f6;
            background: rgba(59, 130, 246, 0.06);
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.12);
        }

        input[type="date"]::-webkit-calendar-picker-indicator,
        input[type="time"]::-webkit-calendar-picker-indicator {
            filter: invert(0.6);
        }

        .form-textarea {
            min-height: 75px;
            resize: vertical;
        }

        .form-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
        }

        /* ── Setup Screen ── */
        #screen-setup {
            align-items: center;
            justify-content: center;
            padding: 40px 24px;
            gap: 24px;
            text-align: center;
        }

        .setup-icon {
            font-size: 64px;
            filter: drop-shadow(0 8px 24px rgba(59, 130, 246, 0.3));
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {

            0%,
            100% {
                transform: translateY(0);
            }

            50% {
                transform: translateY(-8px);
            }
        }

        .setup-card {
            background: rgba(255, 255, 255, 0.04);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 24px;
            padding: 28px;
            width: 100%;
            max-width: 400px;
            backdrop-filter: blur(12px);
        }

        .setup-title {
            font-size: 22px;
            font-weight: 900;
            color: #f1f5f9;
            margin-bottom: 8px;
        }

        .setup-desc {
            font-size: 13px;
            color: #64748b;
            margin-bottom: 20px;
            line-height: 1.7;
        }

        .setup-url-input {
            width: 100%;
            padding: 14px;
            margin-bottom: 14px;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.12);
            border-radius: 14px;
            color: #f1f5f9;
            font-family: inherit;
            font-size: 13px;
            resize: none;
        }

        .setup-url-input:focus {
            outline: none;
            border-color: #3b82f6;
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.12);
        }

        /* ── Home Screen ── */
        #screen-home {
            align-items: center;
            justify-content: center;
            padding: 40px 24px;
            gap: 28px;
            text-align: center;
        }

        .home-logo {
            font-size: 80px;
            filter: drop-shadow(0 8px 32px rgba(59, 130, 246, 0.4));
            animation: float 3s ease-in-out infinite;
        }

        .home-title {
            font-size: 28px;
            font-weight: 900;
            color: #f1f5f9;
            line-height: 1.3;
        }

        .home-subtitle {
            font-size: 14px;
            color: #475569;
            margin-top: 8px;
            line-height: 1.6;
        }

        .home-buttons {
            width: 100%;
            max-width: 400px;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .home-footer {
            font-size: 11px;
            color: #1e3a5f;
            margin-top: 4px;
            letter-spacing: 0.5px;
        }

        /* ── Preview Screen ── */
        #screen-preview {
            background: #000;
        }

        #preview-img {
            width: 100%;
            flex: 1;
            object-fit: contain;
            max-height: 72vh;
        }

        .preview-actions {
            padding: 20px;
            display: flex;
            gap: 12px;
            background: linear-gradient(to top, rgba(0, 0, 0, 0.95), rgba(0, 0, 0, 0.7));
        }

        .preview-actions .btn {
            flex: 1;
        }

        /* ── Analyzing Screen ── */
        #screen-analyzing {
            align-items: center;
            justify-content: center;
            gap: 32px;
            padding: 40px;
            text-align: center;
        }

        .analyzing-wrap {
            position: relative;
            width: 120px;
            height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .analyzing-ring {
            position: absolute;
            width: 120px;
            height: 120px;
            border: 4px solid rgba(59, 130, 246, 0.1);
            border-top-color: #3b82f6;
            border-right-color: #60a5fa;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        .analyzing-ring2 {
            position: absolute;
            width: 88px;
            height: 88px;
            border: 3px solid rgba(59, 130, 246, 0.05);
            border-bottom-color: #818cf8;
            border-radius: 50%;
            animation: spin 1.6s linear infinite reverse;
        }

        .analyzing-ring3 {
            position: absolute;
            width: 56px;
            height: 56px;
            border: 2px solid rgba(139, 92, 246, 0.08);
            border-left-color: #a78bfa;
            border-radius: 50%;
            animation: spin 2s linear infinite;
        }

        @keyframes spin {
            to {
                transform: rotate(360deg);
            }
        }

        .analyzing-icon {
            font-size: 32px;
            animation: pulse 1.8s ease-in-out infinite;
            z-index: 1;
        }

        @keyframes pulse {

            0%,
            100% {
                transform: scale(1);
            }

            50% {
                transform: scale(1.15);
            }
        }

        .analyzing-title {
            font-size: 24px;
            font-weight: 900;
            color: #f1f5f9;
        }

        .analyzing-sub {
            font-size: 14px;
            color: #475569;
            line-height: 1.8;
        }

        .analyzing-dots::after {
            content: '';
            animation: dots 1.5s steps(4) infinite;
        }

        @keyframes dots {
            0% {
                content: '';
            }

            25% {
                content: '.';
            }

            50% {
                content: '..';
            }

            75% {
                content: '...';
            }
        }

        /* ── Form Screen ── */
        #screen-form {
            overflow-y: auto;
        }

        .form-photo-preview {
            width: 100%;
            max-height: 200px;
            object-fit: cover;
            border-bottom: 1px solid rgba(255, 255, 255, 0.06);
        }

        .form-body {
            padding: 20px 20px 40px;
        }

        .ai-badge {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            background: linear-gradient(135deg, rgba(59, 130, 246, 0.1), rgba(139, 92, 246, 0.1));
            border: 1px solid rgba(59, 130, 246, 0.2);
            border-radius: 24px;
            padding: 6px 16px;
            font-size: 12px;
            color: #818cf8;
            margin-bottom: 22px;
            font-weight: 600;
        }

        /* ── Success Screen ── */
        #screen-success {
            align-items: center;
            justify-content: center;
            gap: 28px;
            padding: 40px;
            text-align: center;
        }

        .success-icon {
            font-size: 96px;
            animation: bounceIn 0.6s cubic-bezier(0.36, 0.07, 0.19, 0.97) forwards;
        }

        @keyframes bounceIn {
            0% {
                transform: scale(0);
                opacity: 0;
            }

            55% {
                transform: scale(1.2);
            }

            75% {
                transform: scale(0.92);
            }

            100% {
                transform: scale(1);
                opacity: 1;
            }
        }

        .success-title {
            font-size: 28px;
            font-weight: 900;
            color: #10b981;
        }

        .success-item-name {
            font-size: 20px;
            color: #f1f5f9;
            font-weight: 700;
            margin-top: 6px;
        }

        .success-row-info {
            font-size: 13px;
            color: #475569;
            margin-top: 4px;
        }

        .success-btn-group {
            width: 100%;
            max-width: 400px;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        /* ── List Screen ── */
        #screen-list { overflow-y: auto; }

        .list-filter-bar {
            display: flex; gap: 6px; padding: 10px 16px 6px;
            flex-wrap: wrap; align-items: center;
        }
        .list-filter-btn {
            padding: 5px 14px; border-radius: 20px;
            border: 1px solid rgba(255,255,255,0.12);
            background: rgba(255,255,255,0.05); color: #94a3b8;
            font-family: inherit; font-size: 12px; font-weight: 600;
            cursor: pointer; transition: all 0.2s; white-space: nowrap;
        }
        .list-filter-btn.active { background: #3b82f6; border-color: #3b82f6; color: #fff; }

        .list-stats {
            padding: 4px 16px 8px;
            font-size: 11px; color: #475569;
            display: flex; gap: 12px; flex-wrap: wrap;
        }
        .list-stats span { display: flex; align-items: center; gap: 4px; }
        .stat-dot { width: 7px; height: 7px; border-radius: 50%; display: inline-block; }

        .list-table-wrap { overflow-x: auto; padding: 0 0 32px; }
        .list-table {
            width: 100%; border-collapse: collapse; font-size: 12px;
            min-width: 560px;
        }
        .list-table thead th {
            padding: 10px 12px; text-align: left;
            background: rgba(10,15,30,0.98);
            border-bottom: 1px solid rgba(255,255,255,0.1);
            color: #64748b; font-size: 10px; font-weight: 700;
            text-transform: uppercase; letter-spacing: 0.6px; white-space: nowrap;
        }
        .list-table tbody tr { border-bottom: 1px solid rgba(255,255,255,0.04); }
        .list-table tbody tr:hover { background: rgba(255,255,255,0.02); }
        .list-table tbody tr.tr-returned { opacity: 0.65; }
        .list-table td { padding: 10px 12px; vertical-align: middle; color: #cbd5e1; }
        .list-table td.td-item { font-weight: 700; color: #f1f5f9; }
        .list-table td.td-thumb { width: 52px; padding: 6px 8px; }
        .list-table td.td-thumb img {
            width: 44px; height: 44px; object-fit: cover;
            border-radius: 8px; display: block;
        }
        .list-table td.td-thumb .no-img {
            width: 44px; height: 44px; border-radius: 8px;
            background: rgba(255,255,255,0.04);
            display: flex; align-items: center; justify-content: center;
            font-size: 18px; color: #1e3a5f;
        }

        .ret-btn {
            padding: 3px 10px; font-size: 10px; font-weight: 700;
            border: 1px solid rgba(139,92,246,0.3);
            background: rgba(139,92,246,0.08); color: #a78bfa;
            border-radius: 10px; cursor: pointer; white-space: nowrap;
        }

        /* 旧カードUIのスタイルは残す（他で使うため） */
        .item-thumb { width: 68px; height: 68px; object-fit: cover; border-radius: 12px; flex-shrink: 0; background: rgba(255,255,255,0.03); }
        .item-thumb-ph { width: 68px; height: 68px; border-radius: 12px; flex-shrink: 0; background: rgba(255,255,255,0.04); display: flex; align-items: center; justify-content: center; font-size: 26px; color: #1e3a5f; }

        .status-badge { font-size: 10px; font-weight: 700; padding: 3px 10px; border-radius: 20px; white-space: nowrap; display: inline-block; }

        .status-done {
            background: rgba(16, 185, 129, 0.1);
            color: #34d399;
            border: 1px solid rgba(16, 185, 129, 0.2);
        }

        .status-pending {
            background: rgba(234, 179, 8, 0.1);
            color: #fbbf24;
            border: 1px solid rgba(234, 179, 8, 0.2);
        }

        .status-error {
            background: rgba(239, 68, 68, 0.1);
            color: #f87171;
            border: 1px solid rgba(239, 68, 68, 0.2);
        }

        .status-returned {
            background: rgba(139, 92, 246, 0.1);
            color: #a78bfa;
            border: 1px solid rgba(139, 92, 246, 0.2);
        }

        .list-state {
            text-align: center;
            padding: 64px 20px;
            color: #334155;
            font-size: 15px;
        }

        .list-spinner {
            width: 44px;
            height: 44px;
            border: 3px solid rgba(59, 130, 246, 0.1);
            border-top-color: #3b82f6;
            border-radius: 50%;
            animation: spin 0.9s linear infinite;
            margin: 0 auto 16px;
        }

        .list-count {
            font-size: 12px;
            color: #334155;
            padding: 0 16px 4px;
        }

        .list-empty-icon {
            font-size: 48px;
            margin-bottom: 12px;
            opacity: 0.4;
        }

        /* ── Hidden file input ── */
        #camera-input {
            position: absolute;
            width: 0;
            height: 0;
            opacity: 0;
        }

        /* ── Toast ── */
        .toast {
            position: fixed;
            bottom: 32px;
            left: 50%;
            transform: translateX(-50%) translateY(100px);
            background: rgba(30, 40, 60, 0.95);
            color: #f1f5f9;
            padding: 14px 24px;
            border-radius: 16px;
            font-size: 14px;
            font-weight: 600;
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.4);
            z-index: 100;
            opacity: 0;
            transition: all 0.35s cubic-bezier(0.22, 1, 0.36, 1);
        }

        .toast.show {
            opacity: 1;
            transform: translateX(-50%) translateY(0);
        }

        /* ── Error banner ── */
        .error-banner {
            background: rgba(239, 68, 68, 0.08);
            border: 1px solid rgba(239, 68, 68, 0.2);
            border-radius: 14px;
            padding: 14px 16px;
            margin: 16px 20px 0;
            color: #f87171;
            font-size: 13px;
            line-height: 1.6;
            display: none;
        }

        .error-banner.show {
            display: block;
            animation: screenIn 0.3s ease;
        }
    </style>
</head>

<body>

    <!-- ── 初回設定画面 ── -->
    <div id="screen-setup" class="screen active">
        <div class="setup-icon">📦</div>
        <div class="setup-card">
            <div class="setup-title">⚙️ 初回設定</div>
            <div class="setup-desc">
                GASのデプロイURLを入力してください。<br>
                一度設定すると次回から不要です。
            </div>
            <textarea class="setup-url-input" id="gas-url-input" rows="3"
                placeholder="https://script.google.com/macros/s/XXXX/exec"></textarea>
            <button class="btn btn-primary" onclick="saveGasUrl()">✅ 設定して始める</button>
        </div>
        <div style="font-size:12px; color:#1e3a5f;">設定したURLはこのデバイスに保存されます</div>
    </div>

    <!-- ── ホーム画面 ── -->
    <div id="screen-home" class="screen">
        <div class="home-logo">📦</div>
        <div>
            <div class="home-title">拾得物台帳</div>
            <div class="home-subtitle">写真を撮るだけで自動記録<br>AI が品名・特徴を解析します</div>
        </div>
        <div class="home-buttons">
            <button class="btn btn-primary btn-lg" onclick="openCamera()">📷 &nbsp;撮影して登録</button>
            <button class="btn btn-secondary" onclick="showList()">📋 &nbsp;登録一覧を見る</button>
            <button class="btn btn-secondary" onclick="showScreen('setup')" style="font-size:13px; padding:12px;">
                ⚙️ URL設定を変更
            </button>
        </div>
        <div class="home-footer">Powered by Gemini AI ✨</div>
    </div>

    <!-- ── プレビュー画面 ── -->
    <div id="screen-preview" class="screen">
        <img id="preview-img" src="" alt="撮影した画像">
        <div class="preview-actions">
            <button class="btn btn-secondary" onclick="showScreen('home')">✕ やり直す</button>
            <button class="btn btn-primary" onclick="startAnalysis()">🤖 AI解析スタート</button>
        </div>
    </div>

    <!-- ── 解析中画面 ── -->
    <div id="screen-analyzing" class="screen">
        <div class="analyzing-wrap">
            <div class="analyzing-ring"></div>
            <div class="analyzing-ring2"></div>
            <div class="analyzing-ring3"></div>
            <div class="analyzing-icon">🤖</div>
        </div>
        <div class="analyzing-title">AI解析中<span class="analyzing-dots"></span></div>
        <div class="analyzing-sub">Gemini AI が画像を読み取っています<br>しばらくお待ちください（10〜30秒）</div>
    </div>

    <!-- ── フォーム画面 ── -->
    <div id="screen-form" class="screen">
        <div class="app-header">
            <button class="back-btn" onclick="showScreen('home')">←</button>
            <h1>内容を確認・編集</h1>
        </div>
        <img id="form-photo" src="" alt="登録画像" class="form-photo-preview">
        <div id="form-error" class="error-banner"></div>
        <div class="form-body">
            <div class="ai-badge">✨ AIが自動入力（修正できます）</div>
            <div class="form-row">
                <div class="form-group">
                    <label class="form-label">📅 日付</label>
                    <input type="date" id="f-date" class="form-input">
                </div>
                <div class="form-group">
                    <label class="form-label">⏰ 時間</label>
                    <input type="time" id="f-time" class="form-input">
                </div>
            </div>
            <div class="form-group">
                <label class="form-label">📦 拾得物（品名）</label>
                <input type="text" id="f-item" class="form-input" placeholder="例: 財布、スマートフォン">
            </div>
            <div class="form-group">
                <label class="form-label">📍 場所</label>
                <input type="text" id="f-location" class="form-input" placeholder="例: 1Fエントランス">
            </div>
            <div class="form-group">
                <label class="form-label">👤 拾得者</label>
                <input type="text" id="f-person" class="form-input" placeholder="担当者名">
            </div>
            <div class="form-group">
                <label class="form-label">📝 備考</label>
                <textarea id="f-notes" class="form-textarea" placeholder="その他の特記事項"></textarea>
            </div>
            <button class="btn btn-success btn-lg" onclick="submitForm()" id="submit-btn">
                ✅ &nbsp;スプレッドシートに登録
            </button>
        </div>
    </div>

    <!-- ── 完了画面 ── -->
    <div id="screen-success" class="screen">
        <div class="success-icon">✅</div>
        <div>
            <div class="success-title">登録完了！</div>
            <div class="success-item-name" id="success-item-name"></div>
            <div class="success-row-info" id="success-row-info"></div>
        </div>
        <div class="success-btn-group">
            <button class="btn btn-primary btn-lg" onclick="openCamera()">📷 &nbsp;続けて登録</button>
            <button class="btn btn-secondary" onclick="showList()">📋 &nbsp;一覧を見る</button>
            <button class="btn btn-secondary" onclick="showScreen('home')">🏠 &nbsp;ホームへ</button>
        </div>
    </div>

    <!-- ── 一覧画面 ── -->
    <div id="screen-list" class="screen">
        <div class="app-header">
            <button class="back-btn" onclick="showScreen('home')">←</button>
            <h1>拾得物一覧</h1>
        </div>
        <div class="list-filter-bar">
            <button class="list-filter-btn active" id="lf-all"  onclick="setListFilter('all')">すべて</button>
            <button class="list-filter-btn"        id="lf-keep" onclick="setListFilter('keep')">保管中</button>
            <button class="list-filter-btn"        id="lf-ret"  onclick="setListFilter('ret')">返却済み</button>
        </div>
        <div class="list-stats" id="list-stats"></div>
        <div id="list-state-area"></div>
        <div class="list-table-wrap" id="list-table-wrap" style="display:none;">
            <table class="list-table">
                <thead>
                    <tr>
                        <th>写真</th>
                        <th>日付</th>
                        <th>品名</th>
                        <th>場所</th>
                        <th>拾得者</th>
                        <th>状態</th>
                        <th></th>
                    </tr>
                </thead>
                <tbody id="list-tbody"></tbody>
            </table>
        </div>
    </div>

    <!-- ── 返却確認モーダル ── -->
    <div id="return-modal" style="display:none; position:fixed; inset:0; background:rgba(0,0,0,0.7); z-index:200; align-items:center; justify-content:center; padding:24px;">
        <div style="background:#1e293b; border:1px solid rgba(255,255,255,0.1); border-radius:24px; padding:28px; width:100%; max-width:380px;">
            <div style="font-size:32px; text-align:center; margin-bottom:12px;">↩️</div>
            <div style="font-size:18px; font-weight:700; color:#f1f5f9; text-align:center; margin-bottom:6px;">返却処理</div>
            <div id="return-item-name" style="font-size:14px; color:#64748b; text-align:center; margin-bottom:20px;"></div>
            <div style="margin-bottom:16px;">
                <label style="display:block; font-size:11px; font-weight:700; color:#64748b; text-transform:uppercase; letter-spacing:1.2px; margin-bottom:8px;">📅 返却日</label>
                <input type="date" id="return-date-input" class="form-input">
            </div>
            <div style="display:flex; gap:12px;">
                <button class="btn btn-secondary" style="flex:1;" onclick="closeReturnModal()">キャンセル</button>
                <button class="btn btn-success" style="flex:1;" onclick="confirmReturn()">✅ 返却済みにする</button>
            </div>
        </div>
    </div>

    <input type="file" id="camera-input" accept="image/*" capture="environment">
    <div class="toast" id="toast"></div>

    <script>
        // ── GAS API URL ──
        const GAS_URL_KEY = 'gasApiUrl';

        // GASから配信されている場合 → 現在のURLをAPIエンドポイントとして自動設定
        const SELF_URL = window.location.href.split('?')[0];
        const IS_GAS   = SELF_URL.includes('script.google.com');

        let GAS_URL = IS_GAS
            ? SELF_URL
            : (localStorage.getItem(GAS_URL_KEY) || '');

        // ── 起動処理 ──
        window.addEventListener('load', function () {
            if (GAS_URL) showScreen('home');
            else showScreen('setup');
        });

        function saveGasUrl() {
            const url = document.getElementById('gas-url-input').value.trim();
            if (!url || !url.startsWith('https://script.google.com')) {
                showToast('⚠️ 正しいGASのURLを入力してください');
                return;
            }
            GAS_URL = url;
            localStorage.setItem(GAS_URL_KEY, url);
            showToast('✅ URLを保存しました');
            showScreen('home');
        }

        // ── グローバル状態 ──
        const state = { base64: null, mime: 'image/jpeg', aiResult: null, driveFileId: null };

        function showScreen(name) {
            document.querySelectorAll('.screen').forEach(function (s) { s.classList.remove('active'); });
            document.getElementById('screen-' + name).classList.add('active');
            window.scrollTo(0, 0);
        }

        function openCamera() {
            var input = document.getElementById('camera-input');
            input.value = '';
            input.click();
        }

        document.getElementById('camera-input').addEventListener('change', function (e) {
            var file = e.target.files[0];
            if (!file) return;
            resizeImage(file, 1280, function (dataUrl) {
                state.base64 = dataUrl.split(',')[1];
                state.mime = 'image/jpeg';
                document.getElementById('preview-img').src = dataUrl;
                document.getElementById('form-photo').src = dataUrl;
                showScreen('preview');
            });
        });

        function resizeImage(file, maxPx, callback) {
            var reader = new FileReader();
            reader.onload = function (e) {
                var img = new Image();
                img.onload = function () {
                    var w = img.width, h = img.height;
                    if (w > maxPx || h > maxPx) {
                        if (w > h) { h = Math.round(h * maxPx / w); w = maxPx; }
                        else { w = Math.round(w * maxPx / h); h = maxPx; }
                    }
                    var canvas = document.createElement('canvas');
                    canvas.width = w; canvas.height = h;
                    canvas.getContext('2d').drawImage(img, 0, 0, w, h);
                    callback(canvas.toDataURL('image/jpeg', 0.82));
                };
                img.src = e.target.result;
            };
            reader.readAsDataURL(file);
        }

        // ── GAS API 呼び出し ──
        function callGasApi(payload) {
            return fetch(GAS_URL, {
                method: 'POST',
                body: JSON.stringify(payload),
                headers: { 'Content-Type': 'text/plain;charset=utf-8' }
            }).then(function (res) {
                if (!res.ok) throw new Error('HTTP ' + res.status);
                return res.json();
            });
        }

        // ── AI解析（Drive保存+Gemini） → フォーム表示 ──
        function startAnalysis() {
            showScreen('analyzing');
            var now  = new Date();
            var date = now.toISOString().split('T')[0];
            var time = now.toTimeString().slice(0, 5);

            callGasApi({
                action:      'analyze',
                imageBase64: state.base64,
                imageMime:   state.mime
            }).then(function (result) {
                if (!result.success) {
                    showToast('❌ エラー: ' + (result.error || '不明なエラー'));
                    showScreen('home');
                    return;
                }
                state.driveFileId = result.driveFileId;
                state.aiResult    = result.aiResult;

                var ai = result.aiResult || {};
                document.getElementById('f-date').value     = date;
                document.getElementById('f-time').value     = time;
                document.getElementById('f-item').value     = ai.item     || '';
                document.getElementById('f-location').value = ai.location || '';
                document.getElementById('f-person').value   = '';
                document.getElementById('f-notes').value    = ai.notes    || '';
                document.getElementById('form-error').classList.remove('show');
                showScreen('form');
            }).catch(function (err) {
                showToast('❌ 通信エラー: ' + err.message);
                showScreen('home');
            });
        }

        // ── フォーム送信（全データをGASに送信してスプレッドシートへ保存） ──
        function submitForm() {
            var btn = document.getElementById('submit-btn');
            btn.disabled = true;
            btn.textContent = '📤 送信中...';

            var errorEl = document.getElementById('form-error');
            errorEl.classList.remove('show');

            callGasApi({
                action:      'submit',
                driveFileId: state.driveFileId,
                imageBase64: state.driveFileId ? null : state.base64,
                imageMime:   state.mime,
                date:        document.getElementById('f-date').value,
                time:        document.getElementById('f-time').value,
                location:    document.getElementById('f-location').value,
                person:      document.getElementById('f-person').value,
                item:        document.getElementById('f-item').value,
                notes:       document.getElementById('f-notes').value
            }).then(function (result) {
                btn.disabled = false;
                btn.innerHTML = '✅ &nbsp;スプレッドシートに登録';
                if (!result.success) {
                    errorEl.textContent = '❌ ' + (result.error || 'エラーが発生しました');
                    errorEl.classList.add('show');
                    return;
                }
                document.getElementById('success-item-name').textContent = result.item || '（品名なし）';
                document.getElementById('success-row-info').textContent = '行 ' + result.row + ' に登録されました';
                showScreen('success');
            }).catch(function (err) {
                btn.disabled = false;
                btn.innerHTML = '✅ &nbsp;スプレッドシートに登録';
                errorEl.textContent = '❌ 通信エラー: ' + err.message;
                errorEl.classList.add('show');
            });
        }

        // ── 一覧表示 ──
        var allListItems = [];
        var listFilter   = 'all';

        function showList() {
            showScreen('list');
            document.getElementById('list-state-area').innerHTML =
                '<div class="list-state"><div class="list-spinner"></div>読み込み中...</div>';
            document.getElementById('list-table-wrap').style.display = 'none';
            document.getElementById('list-stats').innerHTML = '';

            callGasApi({ action: 'list' }).then(function (result) {
                if (!result.success || !result.items) {
                    document.getElementById('list-state-area').innerHTML =
                        '<div class="list-state">❌ データ取得に失敗しました</div>';
                    return;
                }
                allListItems = result.items || [];
                renderList();
            }).catch(function (err) {
                document.getElementById('list-state-area').innerHTML =
                    '<div class="list-state">❌ ' + escHtml(err.message) + '</div>';
            });
        }

        function setListFilter(f) {
            listFilter = f;
            ['all','keep','ret'].forEach(function(k) {
                document.getElementById('lf-' + k).classList.toggle('active', k === f);
            });
            renderList();
        }

function renderList() {
            var items = allListItems.filter(function(it) {
                if (listFilter === 'keep') return !it.status || !it.status.includes('返却');
                if (listFilter === 'ret')  return it.status && it.status.includes('返却');
                return true;
            });

            // ── 追加: 現金の計算 ──
            var totalCash = 0;
            items.forEach(function(it) {
                totalCash += extractCashAmount(it.item || '', it.notes || '');
            });

            var total    = allListItems.length;
            var keeping  = allListItems.filter(function(it) { return !it.status || !it.status.includes('返却'); }).length;
            var returned = allListItems.filter(function(it) { return it.status && it.status.includes('返却'); }).length;

            // 🌟修正ポイント: 表示枠（list-stats）が無い場合は自動で作るエラー回避
            var statsArea = document.getElementById('list-stats');
            if (!statsArea) {
                statsArea = document.createElement('div');
                statsArea.id = 'list-stats';
                var wrap = document.getElementById('list-table-wrap');
                if (wrap) wrap.parentNode.insertBefore(statsArea, wrap);
            }

            // ── 変更: 統計情報に「現金合計」と「一括削除ボタン」を追加 ──
            if (statsArea) {
                statsArea.innerHTML = total === 0 ? '' :
                    '<div style="margin-bottom:10px;">' +
                    '<span><span class="stat-dot" style="background:#64748b"></span>合計 ' + total + '件</span>' +
                    '<span><span class="stat-dot" style="background:#fbbf24"></span>保管中 ' + keeping + '件</span>' +
                    '<span><span class="stat-dot" style="background:#a78bfa"></span>返却済み ' + returned + '件</span>' +
                    '</div>' +
                    '<div style="padding: 10px; background: rgba(10, 15, 30, 0.95); border: 1px solid rgba(255,255,255,0.1); display:flex; justify-content:space-between; align-items:center; border-radius: 8px; margin-bottom: 10px;">' +
                    '<span style="color:#34d399; font-weight:bold;">💰 現金合計：' + totalCash.toLocaleString() + ' 円</span>' +
                    '<button id="bulk-delete-btn" class="ret-btn" style="background:#ef4444; color:white; border:none; display:none; margin:0;" onclick="confirmBulkDelete()"></button>' +
                    '</div>';
            }

            // ── 追加: テーブルのヘッダー（一番上）にチェックボックスを自動生成 ──
            var theadTr = document.querySelector('#list-table-wrap thead tr') || document.querySelector('thead tr');
            if (theadTr && !document.getElementById('select-all')) {
                var thCheck = document.createElement('th');
                thCheck.style.width = '40px';
                thCheck.style.textAlign = 'center';
                thCheck.innerHTML = '<input type="checkbox" id="select-all" onclick="toggleSelectAll(this)">';
                theadTr.insertBefore(thCheck, theadTr.firstChild);
            }

            if (allListItems.length === 0) {
                document.getElementById('list-table-wrap').style.display = 'none';
                document.getElementById('list-state-area').innerHTML =
                    '<div class="list-state"><div style="font-size:40px;opacity:.4;margin-bottom:10px">📭</div>まだ登録がありません</div>';
                return;
            }
            if (items.length === 0) {
                document.getElementById('list-table-wrap').style.display = 'none';
                document.getElementById('list-state-area').innerHTML =
                    '<div class="list-state">該当なし</div>';
                return;
            }

            document.getElementById('list-state-area').innerHTML = '';
            document.getElementById('list-table-wrap').style.display = 'block';
            
            // ── 変更: 各行の先頭にチェックボックスを追加 ──
            document.getElementById('list-tbody').innerHTML = items.map(function(it) {
                var isRet = it.status && it.status.includes('返却');
                var statusClass = isRet ? 'status-returned'
                    : (it.status && it.status.includes('完了'))  ? 'status-done'
                    : (it.status && it.status.includes('エラー')) ? 'status-error'
                    : 'status-pending';
                var thumb = it.image
                    ? '<img src="' + escHtml(it.image) + '" alt="" loading="lazy">'
                    : '<div class="no-img">📷</div>';
                var retBtn = isRet ? ''
                    : '<button class="ret-btn" onclick="returnItem(' + it.row + ',\'' +
                      escHtml(it.sheetName) + '\',\'' + escHtml(it.item || '') + '\')">↩️ 返却</button>';
                
                return '<tr class="' + (isRet ? 'tr-returned' : '') + '">' +
                    '<td style="text-align:center; vertical-align:middle;"><input type="checkbox" class="delete-checkbox" data-sheet="' + escHtml(it.sheetName) + '" data-row="' + it.row + '" onchange="toggleBulkDeleteBtn()"></td>' +
                    '<td class="td-thumb">' + thumb + '</td>' +
                    '<td style="white-space:nowrap">' + escHtml((it.date || '') + ' ' + (it.time || '')) + '</td>' +
                    '<td class="td-item">' + escHtml(it.item || '---') + '</td>' +
                    '<td>' + escHtml(it.location || '---') + '</td>' +
                    '<td>' + escHtml(it.person || '---') + '</td>' +
                    '<td><span class="status-badge ' + statusClass + '">' + escHtml(it.status || '---') + '</span></td>' +
                    '<td>' + retBtn + '</td>' +
                    '</tr>';
            }).join('');
        }

        // ── 返却処理 ──
        var _returnRow = null, _returnSheet = null;
        function returnItem(row, sheetName, itemName) {
            _returnRow   = row;
            _returnSheet = sheetName;
            document.getElementById('return-item-name').textContent = itemName || '（品名なし）';
            var today = new Date().toISOString().split('T')[0];
            document.getElementById('return-date-input').value = today;
            var modal = document.getElementById('return-modal');
            modal.style.display = 'flex';
        }
        function closeReturnModal() {
            document.getElementById('return-modal').style.display = 'none';
        }
        function confirmReturn() {
            var returnDate = document.getElementById('return-date-input').value;
            closeReturnModal();
            callGasApi({
                action:     'updateStatus',
                row:        _returnRow,
                sheetName:  _returnSheet,
                status:     '返却済み ✅',
                returnDate: returnDate
            }).then(function(result) {
                if (result.success) {
                    showToast('✅ 返却済みにしました');
                    showList();
                } else {
                    showToast('❌ エラー: ' + (result.error || '更新失敗'));
                }
            }).catch(function(err) {
                showToast('❌ 通信エラー: ' + err.message);
            });
        }

        // ── ユーティリティ ──
        function escHtml(s) {
            var d = document.createElement('div');
            d.textContent = s;
            return d.innerHTML;
        }

        function showToast(msg) {
            var t = document.getElementById('toast');
            t.textContent = msg;
            t.classList.add('show');
            setTimeout(function () { t.classList.remove('show'); }, 3000);
        }
// ── 追加機能セット（現金計算・一括削除） ──
        document.head.insertAdjacentHTML('beforeend', `<style>.delete-checkbox { width: 18px; height: 18px; cursor: pointer; accent-color: #ef4444; }</style>`);

        function extractCashAmount(itemStr, notesStr) {
            var combined = (itemStr + " " + notesStr).toLowerCase();
            if (combined.includes("ic") || combined.includes("suica") || combined.includes("pasmo") || 
                combined.includes("nanaco") || combined.includes("waon") || combined.includes("edy")) {
                return 0;
            }
            var regex = /([0-9０-９,，]+)\s*円/g;
            var match;
            var total = 0;
            while ((match = regex.exec(combined)) !== null) {
                var numStr = match[1].replace(/[０-９]/g, function(s) {
                    return String.fromCharCode(s.charCodeAt(0) - 0xFEE0);
                }).replace(/[,，]/g, ''); 
                var val = parseInt(numStr, 10);
                if (!isNaN(val)) total += val;
            }
            return total;
        }

        function toggleBulkDeleteBtn() {
            var checkboxes = document.querySelectorAll('.delete-checkbox:checked');
            var btn = document.getElementById('bulk-delete-btn');
            var selectAllCb = document.getElementById('select-all');
            if (selectAllCb) {
                selectAllCb.checked = (checkboxes.length > 0 && checkboxes.length === document.querySelectorAll('.delete-checkbox').length);
            }
            if (checkboxes.length > 0) {
                btn.style.display = 'inline-flex';
                btn.innerHTML = '🗑 選択した ' + checkboxes.length + ' 件を削除';
            } else {
                btn.style.display = 'none';
            }
        }

        function toggleSelectAll(mainCb) {
            const cbs = document.querySelectorAll('.delete-checkbox');
            cbs.forEach(cb => cb.checked = mainCb.checked);
            toggleBulkDeleteBtn();
        }

        function confirmBulkDelete() {
            var checkboxes = document.querySelectorAll('.delete-checkbox:checked');
            if (checkboxes.length === 0) return;
            if (!confirm('選択した ' + checkboxes.length + ' 件のデータを一括削除します。\n本当によろしいですか？')) return;
            
            var targets = [];
            checkboxes.forEach(function(cb) {
                targets.push({ sheetName: cb.getAttribute('data-sheet'), row: cb.getAttribute('data-row') });
            });
            
            var btn = document.getElementById('bulk-delete-btn');
            btn.disabled = true;
            btn.innerHTML = '削除中...';
            
            callGasApi({ action: 'bulkDelete', targets: targets }).then(function(res) {
                if (res.success) {
                    showToast('✅ 一括削除が完了しました');
                    showList(); 
                } else {
                    showToast('❌ エラー: ' + res.error);
                }
            }).catch(function(err) {
                showToast('❌ 通信エラー: ' + err.message);
            }).finally(function() {
                btn.disabled = false;
                btn.style.display = 'none';
            });
        }
    </script>
</body>

</html>
