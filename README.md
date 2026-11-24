```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mitos atau Fakta? — Edisi Rujak Bihun</title>
    <!--
        Aplikasi web mandiri satu file (self-contained) untuk pameran sejarah gastronomi Rujak Bihun.
        Tidak bergantung pada CDN eksternal, font eksternal, maupun jaringan internet:
        - Styling menggunakan CSS murni dan sistem utilitas terpadu.
        - Tipografi menggunakan tumpukan font sistem yang bersih dan mudah dibaca di mobile maupun desktop.
        - QR Code dihasilkan langsung oleh encoder QR Code murni JavaScript (Version 1-4, Byte Mode, ECC Level L/M)
          tanpa pustaka pihak ketiga atau gambar acak.
    -->
    <style>
        :root {
            --cream-50: #FDFBF7;
            --cream-100: #F8F3E8;
            --cream-200: #EFE5D1;
            --cream-300: #E3D3B5;
            --chili-500: #D94336;
            --chili-600: #C03226;
            --chili-700: #9E241A;
            --lime-500: #2E7D32;
            --lime-600: #236327;
            --lime-700: #1A4B1D;
            --turmeric-100: #FEF3C7;
            --turmeric-400: #F59E0B;
            --turmeric-500: #D97706;
            --turmeric-600: #B45309;
            --terra-900: #2D160A;
            --terra-800: #4A2511;
            --terra-700: #6B391E;
            --surface: #FFFFFF;
            --font-display: Georgia, "Times New Roman", "Liberation Serif", serif;
            --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            --font-mono: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;
        }

        *, *::before, *::after {
            box-sizing: border-box;
        }

        html, body {
            margin: 0;
            padding: 0;
            min-height: 100%;
        }

        body {
            font-family: var(--font-sans);
            background-color: var(--cream-50);
            background-image: radial-gradient(var(--cream-300) 1px, transparent 1px);
            background-size: 24px 24px;
            color: var(--terra-900);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            overflow-x: hidden;
            -webkit-tap-highlight-color: transparent;
        }

        .hidden {
            display: none !important;
        }

        .font-display {
            font-family: var(--font-display);
        }

        .font-mono-url {
            font-family: var(--font-mono);
            word-break: break-all;
        }

        .card-surface {
            background: var(--surface);
            border: 1px solid var(--cream-200);
            border-radius: 24px;
            box-shadow: 0 12px 30px -6px rgba(45, 22, 10, 0.10), 0 4px 12px -4px rgba(45, 22, 10, 0.05);
        }

        .btn-action {
            cursor: pointer;
            border: none;
            font-family: inherit;
            transition: transform 0.12s ease, background-color 0.16s ease, box-shadow 0.16s ease, border-color 0.16s ease;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .btn-action:active {
            transform: scale(0.97) translateY(1px);
        }

        .btn-primary {
            width: 100%;
            padding: 16px 22px;
            border-radius: 16px;
            background: linear-gradient(135deg, var(--chili-600), var(--chili-500));
            color: #FFFFFF;
            font-weight: 800;
            font-size: 1rem;
            box-shadow: 0 8px 20px -4px rgba(192, 50, 38, 0.35);
        }

        .btn-primary:hover {
            background: linear-gradient(135deg, var(--chili-700), var(--chili-600));
        }

        .btn-secondary {
            padding: 12px 16px;
            border-radius: 14px;
            background: var(--cream-100);
            color: var(--terra-900);
            border: 1px solid var(--cream-200);
            font-weight: 700;
            font-size: 0.875rem;
        }

        .btn-secondary:hover {
            background: var(--cream-200);
        }

        /* Header */
        .site-header {
            width: 100%;
            max-width: 640px;
            margin: 0 auto;
            padding: 18px 16px 8px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 12px;
        }

        .brand-wrap {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .brand-icon {
            width: 42px;
            height: 42px;
            border-radius: 14px;
            background: var(--chili-600);
            color: #FFFFFF;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 6px 14px -3px rgba(192, 50, 38, 0.35);
            flex-shrink: 0;
        }

        .brand-kicker {
            display: block;
            font-size: 0.6875rem;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 0.08em;
            color: var(--turmeric-600);
        }

        .brand-title {
            display: block;
            font-family: var(--font-display);
            font-weight: 700;
            font-size: 1.05rem;
            color: var(--terra-900);
            line-height: 1.2;
        }

        .icon-btn {
            width: 42px;
            height: 42px;
            border-radius: 12px;
            background: var(--cream-100);
            border: 1px solid var(--cream-200);
            color: var(--terra-800);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            flex-shrink: 0;
        }

        .icon-btn:hover {
            background: var(--cream-200);
        }

        /* Main container */
        .main-container {
            width: 100%;
            max-width: 640px;
            margin: 0 auto;
            padding: 12px 16px 24px;
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .start-card {
            padding: 24px;
        }

        @media (min-width: 640px) {
            .start-card {
                padding: 32px;
            }
        }

        .badge-pill {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            padding: 5px 12px;
            border-radius: 999px;
            background: var(--cream-100);
            border: 1px solid rgba(245, 158, 11, 0.45);
            color: var(--turmeric-600);
            font-size: 0.75rem;
            font-weight: 800;
            margin-bottom: 14px;
        }

        .hero-title {
            font-family: var(--font-display);
            font-size: 1.85rem;
            font-weight: 700;
            line-height: 1.2;
            color: var(--terra-900);
            margin: 0 0 10px;
        }

        @media (min-width: 640px) {
            .hero-title {
                font-size: 2.25rem;
            }
        }

        .hero-title span {
            color: var(--chili-600);
        }

        .hero-desc {
            margin: 0 0 20px;
            font-size: 0.9375rem;
            line-height: 1.6;
            color: var(--terra-800);
        }

        .ingredients-grid {
            display: grid;
            grid-template-columns: repeat(3, minmax(0, 1fr));
            gap: 10px;
            margin-bottom: 20px;
        }

        .ingredient-box {
            background: var(--cream-50);
            border: 1px solid var(--cream-200);
            border-radius: 16px;
            padding: 12px 8px;
            text-align: center;
        }

        .ingredient-emoji {
            font-size: 1.4rem;
            line-height: 1.2;
            margin-bottom: 4px;
        }

        .ingredient-name {
            font-size: 0.78rem;
            font-weight: 800;
            color: var(--terra-900);
        }

        .ingredient-sub {
            font-size: 0.6875rem;
            color: var(--terra-700);
            margin-top: 2px;
        }

        .session-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 12px;
            margin-bottom: 16px;
        }

        @media (min-width: 640px) {
            .session-grid {
                grid-template-columns: repeat(2, minmax(0, 1fr));
            }
        }

        .session-active-card {
            padding: 16px;
            border-radius: 18px;
            border: 2px solid var(--chili-600);
            background: rgba(217, 67, 54, 0.05);
        }

        .session-bank-card {
            padding: 16px;
            border-radius: 18px;
            border: 2px solid var(--cream-200);
            background: var(--cream-50);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .card-label-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 8px;
            margin-bottom: 4px;
        }

        .card-mini-title {
            font-size: 0.875rem;
            font-weight: 800;
            color: var(--terra-900);
        }

        .check-badge {
            width: 22px;
            height: 22px;
            border-radius: 999px;
            background: var(--chili-600);
            color: #FFFFFF;
            font-size: 0.75rem;
            font-weight: 800;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }

        .card-mini-desc {
            margin: 0;
            font-size: 0.75rem;
            line-height: 1.45;
            color: var(--terra-800);
        }

        .pool-counter {
            margin-top: 10px;
            font-size: 0.72rem;
            font-weight: 800;
            color: var(--turmeric-600);
        }

        .booth-toolbar {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: space-between;
            gap: 10px;
            background: var(--cream-100);
            border: 1px solid var(--cream-200);
            border-radius: 16px;
            padding: 11px 14px;
            margin-bottom: 12px;
            font-size: 0.78rem;
        }

        .booth-actions {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 12px;
        }

        .link-btn {
            background: none;
            border: none;
            padding: 2px 0;
            font-family: inherit;
            font-size: 0.78rem;
            font-weight: 800;
            cursor: pointer;
            text-decoration: underline;
            text-underline-offset: 3px;
        }

        .link-chili { color: var(--chili-600); }
        .link-lime { color: var(--lime-700); }
        .link-terra { color: var(--terra-900); }

        .option-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 12px;
            background: var(--cream-100);
            border: 1px solid var(--cream-200);
            border-radius: 16px;
            padding: 12px 14px;
            margin-bottom: 20px;
        }

        .option-title {
            font-size: 0.82rem;
            font-weight: 800;
            color: var(--terra-900);
        }

        .option-sub {
            font-size: 0.72rem;
            color: var(--terra-700);
            margin-top: 2px;
        }

        /* Custom Switch */
        .switch-wrap {
            position: relative;
            display: inline-block;
            width: 46px;
            height: 26px;
            flex-shrink: 0;
        }

        .switch-wrap input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .switch-slider {
            position: absolute;
            cursor: pointer;
            inset: 0;
            background-color: #CBD5E1;
            transition: 0.2s;
            border-radius: 999px;
        }

        .switch-slider::before {
            position: absolute;
            content: "";
            height: 20px;
            width: 20px;
            left: 3px;
            bottom: 3px;
            background-color: #FFFFFF;
            transition: 0.2s;
            border-radius: 50%;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
        }

        .switch-wrap input:checked + .switch-slider {
            background-color: var(--lime-500);
        }

        .switch-wrap input:checked + .switch-slider::before {
            transform: translateX(20px);
        }

        /* Quiz screen */
        .hud-card {
            padding: 16px;
            margin-bottom: 14px;
        }

        .hud-top {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 8px;
            flex-wrap: wrap;
            font-size: 0.78rem;
            font-weight: 800;
            margin-bottom: 10px;
        }

        .hud-pill-neutral {
            background: var(--cream-100);
            color: var(--terra-900);
            padding: 4px 12px;
            border-radius: 999px;
        }

        .hud-pill-score {
            background: rgba(46, 125, 50, 0.12);
            color: var(--lime-700);
            padding: 4px 12px;
            border-radius: 999px;
        }

        .hud-timer {
            color: var(--turmeric-600);
            font-weight: 800;
        }

        .hud-timer.urgent {
            color: var(--chili-600);
        }

        .progress-track {
            width: 100%;
            height: 8px;
            background: var(--cream-100);
            border-radius: 999px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, var(--turmeric-500), var(--chili-500));
            transition: width 0.3s ease;
        }

        .timer-track {
            width: 100%;
            height: 6px;
            background: #E5E7EB;
            border-radius: 999px;
            overflow: hidden;
            margin-top: 8px;
        }

        .timer-fill {
            height: 100%;
            width: 100%;
            background: var(--lime-500);
            transition: width 0.1s linear;
        }

        .timer-fill.urgent {
            background: var(--chili-500);
        }

        .question-box {
            padding: 24px;
            margin-bottom: 14px;
            border-width: 2px;
        }

        @media (min-width: 640px) {
            .question-box {
                padding: 32px;
            }
        }

        .topic-badge {
            display: inline-block;
            font-size: 0.72rem;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 0.06em;
            padding: 4px 10px;
            border-radius: 8px;
            background: rgba(245, 158, 11, 0.16);
            color: var(--turmeric-600);
            margin-bottom: 12px;
        }

        .statement-text {
            font-family: var(--font-display);
            font-size: 1.3rem;
            font-weight: 700;
            line-height: 1.4;
            color: var(--terra-900);
            margin: 0;
            min-height: 96px;
            display: flex;
            align-items: center;
        }

        @media (min-width: 640px) {
            .statement-text {
                font-size: 1.55rem;
            }
        }

        .choice-grid {
            display: grid;
            grid-template-columns: repeat(2, minmax(0, 1fr));
            gap: 12px;
            margin-bottom: 14px;
        }

        .btn-mitos {
            padding: 18px 14px;
            border-radius: 18px;
            background: var(--chili-600);
            color: #FFFFFF;
            font-weight: 800;
            font-size: 1.15rem;
            border-bottom: 4px solid var(--chili-700);
            flex-direction: column;
            box-shadow: 0 8px 18px -4px rgba(192, 50, 38, 0.28);
        }

        .btn-mitos:hover {
            background: var(--chili-700);
        }

        .btn-fakta {
            padding: 18px 14px;
            border-radius: 18px;
            background: var(--lime-500);
            color: #FFFFFF;
            font-weight: 800;
            font-size: 1.15rem;
            border-bottom: 4px solid var(--lime-700);
            flex-direction: column;
            box-shadow: 0 8px 18px -4px rgba(46, 125, 50, 0.28);
        }

        .btn-fakta:hover {
            background: var(--lime-600);
        }

        .feedback-box {
            padding: 20px;
            border-radius: 24px;
            border: 2px solid;
            box-shadow: 0 12px 28px -6px rgba(45, 22, 10, 0.12);
        }

        .feedback-correct {
            background: #ECFDF3;
            border-color: var(--lime-500);
        }

        .feedback-wrong {
            background: #FEF2F2;
            border-color: var(--chili-500);
        }

        .feedback-head {
            display: flex;
            align-items: flex-start;
            gap: 12px;
            margin-bottom: 12px;
        }

        .feedback-icon {
            width: 40px;
            height: 40px;
            border-radius: 14px;
            color: #FFFFFF;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
            flex-shrink: 0;
        }

        .feedback-correct .feedback-icon { background: var(--lime-500); }
        .feedback-wrong .feedback-icon { background: var(--chili-600); }

        .feedback-title {
            font-size: 1.05rem;
            font-weight: 800;
            line-height: 1.25;
        }

        .feedback-correct .feedback-title { color: var(--lime-700); }
        .feedback-wrong .feedback-title { color: var(--chili-700); }

        .feedback-sub {
            font-size: 0.78rem;
            font-weight: 700;
            color: var(--terra-800);
            margin-top: 2px;
        }

        .explanation-box {
            background: rgba(255, 255, 255, 0.94);
            border: 1px solid rgba(45, 22, 10, 0.08);
            border-radius: 16px;
            padding: 14px;
            margin-bottom: 14px;
        }

        .explanation-kicker {
            display: block;
            font-size: 0.6875rem;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 0.06em;
            color: var(--turmeric-600);
            margin-bottom: 4px;
        }

        .explanation-body {
            margin: 0;
            font-size: 0.875rem;
            line-height: 1.55;
            color: var(--terra-900);
        }

        .btn-next {
            width: 100%;
            padding: 14px 18px;
            border-radius: 16px;
            background: var(--terra-900);
            color: #FFFFFF;
            font-weight: 700;
            font-size: 0.95rem;
        }

        .btn-next:hover {
            background: var(--terra-800);
        }

        /* Result screen */
        .result-card {
            padding: 24px;
        }

        @media (min-width: 640px) {
            .result-card {
                padding: 32px;
            }
        }

        .rank-wrap {
            text-align: center;
            margin-bottom: 20px;
        }

        .rank-badge {
            width: 76px;
            height: 76px;
            margin: 0 auto 12px;
            border-radius: 24px;
            background: linear-gradient(135deg, var(--turmeric-400), var(--chili-500));
            color: #FFFFFF;
            font-size: 2.2rem;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 10px 20px -4px rgba(217, 119, 6, 0.35);
        }

        .rank-kicker {
            display: block;
            font-size: 0.7rem;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 0.08em;
            color: var(--turmeric-600);
            margin-bottom: 4px;
        }

        .rank-title {
            font-family: var(--font-display);
            font-size: 1.65rem;
            font-weight: 700;
            color: var(--terra-900);
            margin: 0 0 8px;
        }

        .rank-desc {
            margin: 0 auto;
            max-width: 440px;
            font-size: 0.875rem;
            line-height: 1.55;
            color: var(--terra-800);
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(3, minmax(0, 1fr));
            gap: 8px;
            background: var(--cream-50);
            border: 1px solid var(--cream-200);
            border-radius: 18px;
            padding: 14px 8px;
            margin-bottom: 20px;
            text-align: center;
        }

        .stat-label {
            font-size: 0.6875rem;
            font-weight: 800;
            text-transform: uppercase;
            color: var(--terra-700);
        }

        .stat-val-chili {
            font-size: 1.5rem;
            font-weight: 800;
            color: var(--chili-600);
            margin-top: 2px;
        }

        .stat-val-lime {
            font-size: 1.5rem;
            font-weight: 800;
            color: var(--lime-600);
            margin-top: 2px;
        }

        .stat-val-turmeric {
            font-size: 1.5rem;
            font-weight: 800;
            color: var(--turmeric-600);
            margin-top: 2px;
        }

        .review-list {
            max-height: 260px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 20px;
            padding-right: 4px;
        }

        .review-item {
            background: var(--cream-50);
            border: 1px solid var(--cream-200);
            border-radius: 16px;
            padding: 12px 14px;
        }

        .result-actions {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        @media (min-width: 640px) {
            .result-actions {
                flex-direction: row;
            }
            .result-actions .btn-primary {
                flex: 1;
            }
        }

        /* Modals */
        .modal-backdrop {
            position: fixed;
            inset: 0;
            z-index: 50;
            background: rgba(45, 22, 10, 0.62);
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 16px;
        }

        .modal-dialog {
            width: 100%;
            max-width: 640px;
            max-height: 88vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .modal-header {
            padding: 16px 20px;
            background: var(--cream-100);
            border-bottom: 1px solid var(--cream-200);
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 12px;
        }

        .modal-title {
            font-family: var(--font-display);
            font-size: 1.15rem;
            font-weight: 700;
            color: var(--terra-900);
            margin: 0;
        }

        .modal-sub {
            font-size: 0.75rem;
            color: var(--terra-700);
            margin: 2px 0 0;
        }

        .modal-body {
            padding: 18px 20px;
            overflow-y: auto;
            flex: 1;
        }

        .modal-footer {
            padding: 12px 20px;
            background: var(--cream-50);
            border-top: 1px solid var(--cream-200);
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 12px;
        }

        .qr-input-group {
            display: flex;
            gap: 8px;
            margin-top: 6px;
        }

        .qr-input {
            flex: 1;
            min-width: 0;
            padding: 10px 12px;
            border-radius: 12px;
            border: 1px solid var(--cream-300);
            background: var(--cream-50);
            font-family: inherit;
            font-size: 0.85rem;
            color: var(--terra-900);
        }

        .qr-preview-box {
            margin: 16px 0;
            padding: 20px;
            border-radius: 18px;
            background: var(--cream-50);
            border: 1px solid var(--cream-200);
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
        }

        .qr-frame {
            padding: 12px;
            background: #FFFFFF;
            border: 1px solid var(--cream-200);
            border-radius: 18px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }

        .qr-btn-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 10px;
        }

        @media (min-width: 640px) {
            .qr-btn-grid {
                grid-template-columns: repeat(2, minmax(0, 1fr));
            }
        }

        .btn-lime {
            padding: 12px 16px;
            border-radius: 14px;
            background: var(--lime-500);
            color: #FFFFFF;
            font-weight: 800;
            font-size: 0.85rem;
        }

        .btn-lime:hover {
            background: var(--lime-600);
        }

        /* Poster Mode */
        .poster-sheet {
            width: 100%;
            max-width: 580px;
            max-height: 92vh;
            overflow-y: auto;
            background: var(--cream-50);
            border: 2px solid var(--cream-200);
            border-radius: 24px;
            padding: 24px;
            text-align: center;
            box-shadow: 0 20px 40px rgba(45, 22, 10, 0.25);
        }

        .poster-toolbar {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 8px;
            flex-wrap: wrap;
            padding-bottom: 14px;
            margin-bottom: 18px;
            border-bottom: 1px solid var(--cream-200);
        }

        .poster-qr-card {
            display: inline-flex;
            flex-direction: column;
            align-items: center;
            background: #FFFFFF;
            border: 2px solid var(--cream-200);
            border-radius: 24px;
            padding: 20px;
            margin: 12px 0 20px;
        }

        .poster-scan-tag {
            margin-top: 10px;
            font-size: 0.72rem;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 0.06em;
            color: var(--lime-700);
            background: rgba(46, 125, 50, 0.12);
            padding: 4px 12px;
            border-radius: 999px;
        }

        .poster-steps {
            display: grid;
            grid-template-columns: repeat(3, minmax(0, 1fr));
            gap: 10px;
            text-align: left;
            margin-bottom: 16px;
        }

        .poster-step {
            background: #FFFFFF;
            border: 1px solid var(--cream-200);
            border-radius: 14px;
            padding: 10px;
        }

        .site-footer {
            width: 100%;
            max-width: 640px;
            margin: 0 auto;
            padding: 12px 16px 20px;
            text-align: center;
            font-size: 0.75rem;
            color: var(--terra-700);
        }

        @media print {
            body {
                background: #FFFFFF !important;
                background-image: none !important;
            }
            body.printing-poster .site-header,
            body.printing-poster .main-container > :not(#posterModal),
            body.printing-poster .site-footer,
            body.printing-poster .poster-toolbar {
                display: none !important;
            }
            body.printing-poster #posterModal {
                position: static !important;
                background: transparent !important;
                padding: 0 !important;
                display: block !important;
            }
            body.printing-poster .poster-sheet {
                max-width: 100% !important;
                max-height: none !important;
                box-shadow: none !important;
                border: 2px solid var(--cream-200) !important;
            }
        }
    </style>
</head>
<body>
    <!-- Header Pameran -->
    <header class="site-header">
        <div class="brand-wrap">
            <div class="brand-icon" aria-hidden="true">
                <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M3 11h18a1 1 0 0 1 1 1v.5a8.5 8.5 0 0 1-8.5 8.5h-3A8.5 8.5 0 0 1 2 12.5V12a1 1 0 0 1 1-1Z"></path>
                    <path d="M7 11c0-2.5 2-3.5 2-6"></path>
                    <path d="M12 11c0-2.5 2-3.5 2-6"></path>
                    <path d="M17 11c0-2.5 2-3.5 2-6"></path>
                </svg>
            </div>
            <div>
                <span class="brand-kicker">Pameran Sejarah Gastronomi</span>
                <span class="brand-title">Jejak Rasa Rujak Bihun</span>
            </div>
        </div>

        <button type="button" id="soundToggleBtn" onclick="toggleSound()" class="icon-btn" aria-label="Suara Efek" title="Aktifkan/Matikan Efek Suara">
            <span id="soundIconText" style="font-size:1.1rem;font-weight:800;">🔊</span>
        </button>
    </header>

    <!-- Kontainer Utama -->
    <main class="main-container">
        <!-- LAYAR 1: PEMBUKA -->
        <section id="startScreen" class="card-surface start-card">
            <div class="badge-pill">
                <span>🌶️</span>
                <span>Minigame Interaktif Stand • 100% Offline</span>
            </div>

            <h1 class="hero-title">
                Mitos atau Fakta? <span>— Edisi Rujak Bihun</span>
            </h1>
            <p class="hero-desc">
                Telusuri sejarah rujak bihun dari Prasasti Paradah (943 M), jaringan niaga bihun 1834–1836, akulturasi Tionghoa-Sunda, hingga dinamika gastrodiplomasi dan mobilitas pangan.
            </p>

            <div class="ingredients-grid">
                <div class="ingredient-box">
                    <div class="ingredient-emoji">🍜</div>
                    <div class="ingredient-name">Bihun &amp; Niaga</div>
                    <div class="ingredient-sub">1834 &amp; 1836</div>
                </div>
                <div class="ingredient-box">
                    <div class="ingredient-emoji">🌶️</div>
                    <div class="ingredient-name">Cabai &amp; Rempah</div>
                    <div class="ingredient-sub">Lintas Benua</div>
                </div>
                <div class="ingredient-box">
                    <div class="ingredient-emoji">🍋‍🟩</div>
                    <div class="ingredient-name">Varian &amp; Diplomasi</div>
                    <div class="ingredient-sub">Akulturasi Rasa</div>
                </div>
            </div>

            <div class="session-grid">
                <div class="session-active-card">
                    <div class="card-label-row">
                        <span class="card-mini-title">🎯 1 Sesi = 5 Soal Acak</span>
                        <span class="check-badge">✓</span>
                    </div>
                    <p class="card-mini-desc">Dipilih acak dari 26 pertanyaan dengan prioritas soal yang belum dimainkan.</p>
                </div>

                <div class="session-bank-card">
                    <div>
                        <div class="card-mini-title">📚 Bank 26 Pertanyaan</div>
                        <p class="card-mini-desc">Prasasti, jalur dagang, etnobotani, variasi daerah &amp; gastrodiplomasi.</p>
                    </div>
                    <div id="poolStatusText" class="pool-counter">Belum dimainkan: 26/26</div>
                </div>
            </div>

            <!-- Toolbar Penjaga Tenda -->
            <div class="booth-toolbar">
                <button type="button" onclick="resetPlayedPool()" class="link-btn link-chili">
                    🔄 Reset Riwayat
                </button>
                <div class="booth-actions">
                    <button type="button" onclick="toggleQrModal()" class="link-btn link-lime">
                        📱 QR Code Tenda
                    </button>
                    <button type="button" onclick="toggleBankModal()" class="link-btn link-terra">
                        📋 Ringkas Bank (26)
                    </button>
                </div>
            </div>

            <!-- Opsi Timer -->
            <div class="option-row">
                <div>
                    <div class="option-title">⏱️ Timer Tantangan (15 Detik/Soal)</div>
                    <div class="option-sub">Matikan jika ingin berdiskusi lebih santai di stand</div>
                </div>
                <label class="switch-wrap" aria-label="Aktifkan Timer 15 Detik">
                    <input type="checkbox" id="timerToggle" checked>
                    <span class="switch-slider"></span>
                </label>
            </div>

            <button type="button" onclick="startGame()" class="btn-action btn-primary">
                <span>Mulai Sesi (5 Soal Acak)</span>
                <span>→</span>
            </button>
        </section>

        <!-- LAYAR 2: PERMAINAN -->
        <section id="quizScreen" class="hidden">
            <div class="card-surface hud-card">
                <div class="hud-top">
                    <span id="questionCounter" class="hud-pill-neutral">Soal 1 dari 5</span>
                    <div style="display:flex;align-items:center;gap:12px;">
                        <span id="timerText" class="hud-timer">⏱️ <span id="secondsLeft">15</span>d</span>
                        <span class="hud-pill-score">Skor: <span id="currentScore">0</span></span>
                    </div>
                </div>
                <div class="progress-track">
                    <div id="progressBar" class="progress-fill"></div>
                </div>
                <div id="timerBarContainer" class="timer-track">
                    <div id="timerBar" class="timer-fill"></div>
                </div>
            </div>

            <div id="questionCard" class="card-surface question-box">
                <span id="topicBadge" class="topic-badge">Topik</span>
                <h2 id="statementText" class="statement-text">Memuat pernyataan...</h2>
            </div>

            <div id="answerButtons" class="choice-grid">
                <button type="button" onclick="submitAnswer('MITOS')" class="btn-action btn-mitos">
                    <span style="font-size:1.4rem;">❌</span>
                    <span>MITOS</span>
                </button>
                <button type="button" onclick="submitAnswer('FAKTA')" class="btn-action btn-fakta">
                    <span style="font-size:1.4rem;">✅</span>
                    <span>FAKTA</span>
                </button>
            </div>

            <div id="feedbackCard" class="hidden feedback-box">
                <div class="feedback-head">
                    <div id="feedbackIcon" class="feedback-icon">🎉</div>
                    <div>
                        <div id="feedbackTitle" class="feedback-title">Judul Feedback</div>
                        <div id="feedbackSubtitle" class="feedback-sub">Subjudul</div>
                    </div>
                </div>

                <div class="explanation-box">
                    <span class="explanation-kicker">📖 Catatan Sejarah Gastronomi</span>
                    <p id="explanationText" class="explanation-body"></p>
                </div>

                <button type="button" id="nextBtn" onclick="nextQuestion()" class="btn-action btn-next">
                    <span id="nextBtnText">Soal Berikutnya</span>
                    <span>→</span>
                </button>
            </div>
        </section>

        <!-- LAYAR 3: HASIL AKHIR -->
        <section id="resultScreen" class="hidden card-surface result-card">
            <div class="rank-wrap">
                <div id="rankEmoji" class="rank-badge">🏆</div>
                <span class="rank-kicker">Predikat Pengunjung Stand</span>
                <h2 id="rankTitle" class="rank-title">Duta Gastrodiplomasi</h2>
                <p id="rankDescription" class="rank-desc"></p>
            </div>

            <div class="stats-grid">
                <div>
                    <div class="stat-label">Total Skor</div>
                    <div id="finalScore" class="stat-val-chili">0</div>
                </div>
                <div style="border-left:1px solid var(--cream-200);border-right:1px solid var(--cream-200);">
                    <div class="stat-label">Jawaban Benar</div>
                    <div id="correctCount" class="stat-val-lime">0/5</div>
                </div>
                <div>
                    <div class="stat-label">Akurasi</div>
                    <div id="accuracyPercent" class="stat-val-turmeric">0%</div>
                </div>
            </div>

            <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:10px;">
                <span style="font-size:0.75rem;font-weight:800;text-transform:uppercase;letter-spacing:0.06em;color:var(--terra-900);">Ringkasan 5 Soal Sesi Ini</span>
                <span style="font-size:0.7rem;color:var(--terra-700);">Gulir untuk meninjau</span>
            </div>
            <div id="reviewList" class="review-list"></div>

            <div class="result-actions">
                <button type="button" onclick="startGame()" class="btn-action btn-primary">
                    <span>🔄 Main Sesi Baru (Acak 5 Soal)</span>
                </button>
                <button type="button" onclick="resetToStart()" class="btn-action btn-secondary">
                    Menu Awal
                </button>
            </div>
        </section>

        <!-- MODAL RINGKAS BANK 26 SOAL -->
        <div id="bankModal" class="hidden modal-backdrop" role="dialog" aria-modal="true" aria-labelledby="bankModalTitle">
            <div class="card-surface modal-dialog">
                <div class="modal-header">
                    <div>
                        <h3 id="bankModalTitle" class="modal-title">Ringkasan 26 Soal Penjaga Tenda</h3>
                        <p class="modal-sub">Kunci jawaban &amp; catatan sejarah rujak bihun</p>
                    </div>
                    <button type="button" onclick="toggleBankModal()" class="icon-btn" aria-label="Tutup Ringkasan Bank Soal">✕</button>
                </div>
                <div id="bankModalList" class="modal-body" style="display:flex;flex-direction:column;gap:10px;"></div>
                <div class="modal-footer">
                    <button type="button" onclick="resetPlayedPool(); renderBankModal();" class="link-btn link-chili">
                        Reset Riwayat Soal
                    </button>
                    <button type="button" onclick="toggleBankModal()" class="btn-action btn-secondary" style="padding:8px 16px;">
                        Tutup
                    </button>
                </div>
            </div>
        </div>

        <!-- MODAL QR CODE TENDA -->
        <div id="qrModal" class="hidden modal-backdrop" role="dialog" aria-modal="true" aria-labelledby="qrModalTitle">
            <div class="card-surface modal-dialog">
                <div class="modal-header">
                    <div>
                        <h3 id="qrModalTitle" class="modal-title">QR Code Stand Pameran</h3>
                        <p class="modal-sub">Encoder QR Code bawaan tanpa internet/CDN eksternal</p>
                    </div>
                    <button type="button" onclick="toggleQrModal()" class="icon-btn" aria-label="Tutup Panel QR Code">✕</button>
                </div>
                <div class="modal-body">
                    <label for="qrUrlInput" style="display:block;font-size:0.72rem;font-weight:800;text-transform:uppercase;letter-spacing:0.06em;color:var(--terra-800);">
                        URL Tautan Game untuk QR Code
                    </label>
                    <div class="qr-input-group">
                        <input
                            type="url"
                            id="qrUrlInput"
                            oninput="handleQrUrlChange()"
                            placeholder="https://contoh-pameran.id/rujak-bihun-quiz.html"
                            class="qr-input"
                        >
                        <button type="button" onclick="useCurrentPageUrl()" class="btn-action btn-secondary" style="white-space:nowrap;">
                            Pakai URL Ini
                        </button>
                    </div>
                    <p id="qrStatusNote" style="font-size:0.72rem;color:var(--terra-700);margin:8px 0 0;line-height:1.45;"></p>

                    <div class="qr-preview-box">
                        <div id="qrContainer" class="qr-frame"></div>
                        <div id="qrPreviewUrl" class="font-mono-url" style="font-size:0.72rem;color:var(--terra-700);margin-top:10px;"></div>
                    </div>

                    <div class="qr-btn-grid">
                        <button type="button" onclick="downloadQrPng()" class="btn-action btn-lime">
                            <span>⬇️ Unduh QR (PNG)</span>
                        </button>
                        <button type="button" onclick="openPosterMode()" class="btn-action btn-primary" style="padding:12px 16px;font-size:0.85rem;">
                            <span>🖼️ Mode Poster QR Tenda</span>
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- MODE POSTER QR TENDA -->
        <div id="posterModal" class="hidden modal-backdrop" role="dialog" aria-modal="true" aria-labelledby="posterTitle">
            <div class="poster-sheet">
                <div class="poster-toolbar">
                    <button type="button" onclick="closePosterMode(true)" class="btn-action btn-secondary" style="padding:8px 12px;font-size:0.78rem;">
                        ← Kembali ke QR
                    </button>
                    <div style="display:flex;align-items:center;gap:8px;">
                        <button type="button" onclick="printPosterStand()" class="btn-action btn-secondary" style="padding:8px 12px;font-size:0.78rem;background:var(--turmeric-100);border-color:var(--turmeric-400);color:var(--terra-900);">
                            🖨️ Cetak Poster
                        </button>
                        <button type="button" onclick="closePosterMode(false)" class="icon-btn" style="width:36px;height:36px;" aria-label="Tutup Mode Poster">✕</button>
                    </div>
                </div>

                <div class="badge-pill" style="margin-bottom:10px;">
                    <span>🌶️</span>
                    <span>Pameran Sejarah Gastronomi • Rujak Bihun</span>
                </div>

                <h2 id="posterTitle" class="hero-title" style="font-size:1.75rem;margin-bottom:8px;">
                    Mitos atau Fakta? <span>Edisi Rujak Bihun</span>
                </h2>
                <p class="hero-desc" style="font-size:0.85rem;margin-bottom:10px;">
                    Pindai kode QR di bawah untuk memainkan 5 soal acak seputar sejarah, jalur niaga, dan akulturasi rujak bihun.
                </p>

                <div class="poster-qr-card">
                    <div id="posterQrContainer"></div>
                    <span class="poster-scan-tag">Pindai untuk Mulai Main</span>
                </div>

                <div class="poster-steps">
                    <div class="poster-step">
                        <div style="font-size:0.75rem;font-weight:800;color:var(--chili-600);margin-bottom:2px;">1. Pindai QR</div>
                        <div style="font-size:0.6875rem;color:var(--terra-800);line-height:1.35;">Buka langsung di peramban ponsel tanpa instalasi.</div>
                    </div>
                    <div class="poster-step">
                        <div style="font-size:0.75rem;font-weight:800;color:var(--turmeric-600);margin-bottom:2px;">2. Jawab 5 Soal</div>
                        <div style="font-size:0.6875rem;color:var(--terra-800);line-height:1.35;">Pilih Mitos atau Fakta dan baca catatan sejarahnya.</div>
                    </div>
                    <div class="poster-step">
                        <div style="font-size:0.75rem;font-weight:800;color:var(--lime-700);margin-bottom:2px;">3. Cek Predikat</div>
                        <div style="font-size:0.6875rem;color:var(--terra-800);line-height:1.35;">Lihat skor &amp; predikat kuliner di akhir sesi.</div>
                    </div>
                </div>

                <div id="posterUrlCaption" class="font-mono-url" style="font-size:0.7rem;color:var(--terra-700);"></div>
            </div>
        </div>
    </main>

    <footer class="site-footer">
        Pameran Sejarah Gastronomi <strong>Rujak Bihun</strong> • Aplikasi Mandiri Tanpa CDN • 5 Soal Acak / Sesi dari 26 Bank Soal
    </footer>

    <script>
        // =========================================================================
        // 1. BANK 26 PERTANYAAN SEJARAH GASTRONOMI RUJAK BIHUN
        // =========================================================================
        const QUESTION_BANK = [
            {
                id: 1,
                topic: "Prasasti Paradah (943 M)",
                statement: "Istilah 'rurujak' dan 'angrujak' tercatat dalam Prasasti Paradah tahun 943 M pada masa Mataram Kuno.",
                answer: "FAKTA",
                explanation: "Prasasti Paradah (943 M) mencatat istilah 'rurujak' dan 'angrujak' sebagai jejak leksikal awal terkait pengolahan/pemotongan bahan pangan pada masa Mataram Kuno."
            },
            {
                id: 2,
                topic: "Makna Awal Rujak",
                statement: "Pada tahun 943 M di Prasasti Paradah, kata 'rurujak' sudah merujuk secara khusus pada bihun kuah kencur.",
                answer: "MITOS",
                explanation: "Istilah 'rurujak' pada Prasasti Paradah (943 M) berasal dari konteks kuliner Jawa Kuno jauh sebelum perjumpaan dengan bihun dan racikan rujak bihun modern."
            },
            {
                id: 3,
                topic: "Akulturasi Tionghoa-Sunda",
                statement: "Rujak bihun mencerminkan pertemuan dan akulturasi antara tradisi bihun Tionghoa serta cita rasa maupun bumbu lokal seperti kencur (cikur) di Sunda.",
                answer: "FAKTA",
                explanation: "Perpaduan bahan berbasis tradisi Tionghoa (bihun) dan aromatik lokal seperti kencur (cikur) memperlihatkan dinamika akulturasi kuliner di Nusantara."
            },
            {
                id: 4,
                topic: "Akar Tradisi Bihun",
                statement: "Bihun berakar dari tradisi olahan mi berbahan tepung beras dalam budaya kuliner Tionghoa.",
                answer: "FAKTA",
                explanation: "Bihun merupakan bagian dari tradisi kuliner Tionghoa yang kemudian menyebar dan beradaptasi bersama jaringan perdagangan dan migrasi di Asia."
            },
            {
                id: 5,
                topic: "Jalur Niaga 1834 (Makau–Singapura)",
                statement: "Catatan perdagangan merekam pengiriman bihun dari Makau ke Singapura pada tahun 1834.",
                answer: "FAKTA",
                explanation: "Rekaman perdagangan tahun 1834 menunjukkan arus distribusi bihun dari Makau ke Singapura sebagai simpul niaga maritim Asia."
            },
            {
                id: 6,
                topic: "Jalur Niaga 1836 (Batavia–Semarang)",
                statement: "Pada tahun 1836, catatan perdagangan menunjukkan pengiriman bihun ke Batavia dan Semarang.",
                answer: "FAKTA",
                explanation: "Pada 1836, jaringan niaga membawa bihun ke pelabuhan Batavia dan Semarang, memperkuat konektivitas pangan di pesisir utara Jawa."
            },
            {
                id: 7,
                topic: "Kronologi Bihun Batavia & Semarang",
                statement: "Bihun baru pertama kali dikenal di Batavia dan Semarang setelah tahun 1950.",
                answer: "MITOS",
                explanation: "Catatan perdagangan sudah merekam pengiriman bihun ke Batavia dan Semarang pada tahun 1836, setelah jalur Makau–Singapura pada 1834."
            },
            {
                id: 8,
                topic: "Kencur / Cikur",
                statement: "Kencur (cikur) merupakan rimpang beraroma khas yang berperan dalam racikan rujak bihun sekaligus dikenal dalam pengobatan tradisional.",
                answer: "FAKTA",
                explanation: "Kencur (Kaempferia galanga) memberi karakter aroma hangat-segar pada bumbu sekaligus lama dimanfaatkan sebagai bahan jamu dan pengobatan tradisional."
            },
            {
                id: 9,
                topic: "Bawang Putih & Jalur Sebar",
                statement: "Bawang putih diperkirakan berasal dari kawasan Asia Tengah sebelum menyebar ke berbagai wilayah termasuk Nusantara.",
                answer: "FAKTA",
                explanation: "Bawang putih (Allium sativum) berasal dari Asia Tengah dan menyebar luas melalui jalur perdagangan kuno hingga menjadi bumbu dasar kuliner."
            },
            {
                id: 10,
                topic: "Etnobotani & Kias",
                statement: "Dalam kajian etnobotani Jawa Barat, bawang putih juga berkaitan dengan praktik kepercayaan lokal seperti 'kias'.",
                answer: "FAKTA",
                explanation: "Selain sebagai bumbu dan obat tradisional, bawang putih dalam tradisi Sunda juga muncul dalam praktik simbolik/kepercayaan lokal seperti 'kias' (penangkal)."
            },
            {
                id: 11,
                topic: "Fungsi Tunggal Bawang Putih",
                statement: "Bawang putih hanya berfungsi sebagai bumbu tumisan dan tidak pernah memiliki makna simbolik atau etnobotani lokal.",
                answer: "MITOS",
                explanation: "Kajian etnobotani menunjukkan bawang putih memiliki dimensi kuliner, pengobatan, sekaligus simbolik dalam praktik budaya masyarakat."
            },
            {
                id: 12,
                topic: "Jeruk Limau",
                statement: "Jeruk limau memberikan kesegaran dan aroma sitrus yang menyeimbangkan rasa pedas, gurih, atau manis pada rujak bihun.",
                answer: "FAKTA",
                explanation: "Perasan dan aroma jeruk limau menjadi penyeimbang rasa sekaligus penguat kesegaran pada berbagai racikan rujak."
            },
            {
                id: 13,
                topic: "Asal Cabai Abad ke-16",
                statement: "Cabai berasal dari Benua Amerika dan masuk ke jaringan perdagangan Asia sejak abad ke-16.",
                answer: "FAKTA",
                explanation: "Cabai (Capsicum) berasal dari Benua Amerika dan menyebar ke Asia sejak abad ke-16 melalui rute pelayaran dan perdagangan global."
            },
            {
                id: 14,
                topic: "Asal-Usul Cabai",
                statement: "Cabai adalah tanaman asli Nusantara yang sudah tumbuh di Jawa sejak zaman purba.",
                answer: "MITOS",
                explanation: "Cabai berasal dari Benua Amerika dan baru tiba di Asia pada abad ke-16, lalu beradaptasi cepat hingga menjadi bagian penting rasa kuliner Nusantara."
            },
            {
                id: 15,
                topic: "Garam & Situs Zhongba",
                statement: "Situs Zhongba di lembah Sungai Yangtze, Tiongkok, merupakan jejak penting arkeologi produksi garam pada masa lalu.",
                answer: "FAKTA",
                explanation: "Situs Zhongba dikenal dalam kajian arkeologi sebagai pusat produksi garam kuno yang menopang pengawetan dan cita rasa pangan."
            },
            {
                id: 16,
                topic: "Peran Garam dalam Sejarah Pangan",
                statement: "Garam tidak memiliki peran penting dalam sejarah perdagangan dan pengawetan bahan pangan sebelum abad ke-20.",
                answer: "MITOS",
                explanation: "Sejak masa kuno, garam adalah komoditas vital untuk mengawetkan bahan pangan, mendukung perjalanan jauh, dan membentuk cita rasa."
            },
            {
                id: 17,
                topic: "Kitab Mustika Rasa (1960/1967)",
                statement: "Kitab 'Mustika Rasa' (1960/1967) merupakan dokumentasi resmi resep kuliner dari berbagai daerah di Indonesia.",
                answer: "FAKTA",
                explanation: "Disusun pada 1960-an dan diterbitkan 1967 oleh Departemen Pertanian, Mustika Rasa menghimpun beragam resep daerah sebagai arsip kuliner Nusantara."
            },
            {
                id: 18,
                topic: "Varian Rujak Bihun Lampung",
                statement: "Di Lampung, penyajian rujak bihun dikenal dengan tambahan irisan mentimun dan tahu cokelat.",
                answer: "FAKTA",
                explanation: "Varian rujak bihun di Lampung memadukan bihun dengan mentimun segar dan tahu cokelat yang memberi kekayaan tekstur."
            },
            {
                id: 19,
                topic: "Varian Rujak Bihun Madura",
                statement: "Di Madura, varian rujak bihun kerap disajikan dengan bumbu petis dan potongan tahu putih.",
                answer: "FAKTA",
                explanation: "Adaptasi di Madura menonjolkan bumbu petis yang gurih-khas berpadu dengan potongan tahu putih dan bihun."
            },
            {
                id: 20,
                topic: "Varian Rujak Bihun Surabaya",
                statement: "Di Surabaya, rujak bihun sering dilengkapi dengan taoge dan kerupuk.",
                answer: "FAKTA",
                explanation: "Penambahan taoge dan kerupuk pada varian Surabaya menghadirkan sensasi renyah dan segar dalam satu sajian."
            },
            {
                id: 21,
                topic: "Satu Resep Baku Tunggal",
                statement: "Rujak bihun memiliki satu resep tunggal yang identik di seluruh daerah tanpa adaptasi lokal.",
                answer: "MITOS",
                explanation: "Rujak bihun berkembang secara kontekstual di tiap daerah (seperti Lampung, Madura, Surabaya, dan Sunda) sesuai bahan dan selera setempat."
            },
            {
                id: 22,
                topic: "Mobilitas Pangan",
                statement: "Perspektif mobilitas melihat bahan pangan, resep, dan manusia terus bergerak melintasi ruang dan jaringan perdagangan.",
                answer: "FAKTA",
                explanation: "Makanan bergerak melalui mobilitas pedagang, perantau, pelaut, dan jalur distribusi sehingga membentuk pertemuan rasa lintas wilayah."
            },
            {
                id: 23,
                topic: "Mediascape Digital",
                statement: "Konsep 'mediascape' membuat cerita dan variasi rujak bihun daerah menyebar luas melampaui batas lokasi asalnya melalui media.",
                answer: "FAKTA",
                explanation: "Arus media sosial dan konten digital ('mediascape') mempercepat penyebaran narasi kuliner daerah ke khalayak yang lebih luas."
            },
            {
                id: 24,
                topic: "Gastrodiplomasi",
                statement: "Gastrodiplomasi memanfaatkan daya tarik makanan dan cerita budayanya untuk membangun pemahaman lintas masyarakat.",
                answer: "FAKTA",
                explanation: "Melalui pengalaman rasa dan narasi sejarah yang membumi, gastrodiplomasi mendekatkan budaya suatu masyarakat kepada audiens yang lebih luas."
            },
            {
                id: 25,
                topic: "Soft Power Kuliner",
                statement: "Konsep 'soft power' dalam gastronomi hanya bekerja lewat perjanjian politik formal dan tidak relevan untuk pameran budaya.",
                answer: "MITOS",
                explanation: "Soft power bekerja melalui daya tarik budaya, cerita sejarah, dan keterlibatan publik—termasuk lewat pameran kuliner seperti rujak bihun."
            },
            {
                id: 26,
                topic: "Peta Sejarah Semangkuk Rujak",
                statement: "Semangkuk rujak bihun dapat dibaca sebagai pertemuan sejarah global dan lokal: dari bahan lintas benua hingga racikan daerah.",
                answer: "FAKTA",
                explanation: "Dari bihun jalur dagang Asia, cabai asal Amerika, bumbu dan rempah lokal, hingga variasi daerah—rujak bihun adalah arsip hidup akulturasi."
            }
        ];

        // =========================================================================
        // 2. STATE APLIKASI (5 SOAL ACAK PER SESI + PRIORITAS SOAL BELUM DIMAINKAN)
        // =========================================================================
        const QUESTIONS_PER_SESSION = 5;
        const TIME_PER_QUESTION = 15;

        let activeQuestions = [];
        let playedIds = new Set();
        let currentIndex = 0;
        let score = 0;
        let userHistory = [];
        let isAnswered = false;
        let soundEnabled = true;
        let timeLeft = TIME_PER_QUESTION;
        let timerInterval = null;
        let useTimer = true;
        let audioCtx = null;

        function playTone(type) {
            if (!soundEnabled) return;
            try {
                const AudioContextClass = window.AudioContext || window.webkitAudioContext;
                if (!AudioContextClass) return;
                if (!audioCtx) {
                    audioCtx = new AudioContextClass();
                }
                if (audioCtx.state === 'suspended') {
                    audioCtx.resume();
                }

                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);

                const now = audioCtx.currentTime;
                if (type === 'correct') {
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(523.25, now);
                    osc.frequency.setValueAtTime(659.25, now + 0.09);
                    osc.frequency.setValueAtTime(783.99, now + 0.18);
                    gain.gain.setValueAtTime(0.14, now);
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.35);
                    osc.start(now);
                    osc.stop(now + 0.35);
                } else if (type === 'wrong') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(220, now);
                    osc.frequency.linearRampToValueAtTime(165, now + 0.25);
                    gain.gain.setValueAtTime(0.11, now);
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.3);
                    osc.start(now);
                    osc.stop(now + 0.3);
                } else {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(440, now);
                    gain.gain.setValueAtTime(0.08, now);
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.08);
                    osc.start(now);
                    osc.stop(now + 0.08);
                }
            } catch (err) {
                // Abaikan jika audio diblokir oleh peramban
            }
        }

        function toggleSound() {
            soundEnabled = !soundEnabled;
            document.getElementById('soundIconText').textContent = soundEnabled ? '🔊' : '🔇';
            if (soundEnabled) playTone('click');
        }

        function shuffleArray(array) {
            const arr = array.slice();
            for (let i = arr.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                const temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
            return arr;
        }

        function updatePoolStatus() {
            const unplayedCount = QUESTION_BANK.filter(function (q) {
                return !playedIds.has(q.id);
            }).length;
            const statusEl = document.getElementById('poolStatusText');
            if (statusEl) {
                statusEl.textContent = 'Belum dimainkan: ' + unplayedCount + '/' + QUESTION_BANK.length;
            }
        }

        function resetPlayedPool() {
            playedIds.clear();
            playTone('click');
            updatePoolStatus();
        }

        function pickSessionQuestions(count) {
            const unplayedPool = shuffleArray(QUESTION_BANK.filter(function (q) {
                return !playedIds.has(q.id);
            }));

            let selected = [];
            if (unplayedPool.length >= count) {
                selected = unplayedPool.slice(0, count);
            } else {
                selected = unplayedPool.slice();
                const needed = count - selected.length;
                playedIds.clear();
                const fallbackPool = shuffleArray(QUESTION_BANK.filter(function (q) {
                    return !selected.some(function (s) { return s.id === q.id; });
                }));
                selected = selected.concat(fallbackPool.slice(0, needed));
            }

            selected.forEach(function (q) {
                playedIds.add(q.id);
            });
            updatePoolStatus();
            return selected;
        }

        function startGame() {
            playTone('click');
            useTimer = document.getElementById('timerToggle').checked;
            activeQuestions = pickSessionQuestions(QUESTIONS_PER_SESSION);
            currentIndex = 0;
            score = 0;
            userHistory = [];

            document.getElementById('startScreen').classList.add('hidden');
            document.getElementById('resultScreen').classList.add('hidden');
            document.getElementById('quizScreen').classList.remove('hidden');

            document.getElementById('timerText').classList.toggle('hidden', !useTimer);
            document.getElementById('timerBarContainer').classList.toggle('hidden', !useTimer);

            renderQuestion();
        }

        function renderQuestion() {
            isAnswered = false;
            clearInterval(timerInterval);

            const q = activeQuestions[currentIndex];
            const total = activeQuestions.length;

            document.getElementById('questionCounter').textContent = 'Soal ' + (currentIndex + 1) + ' dari ' + total;
            document.getElementById('currentScore').textContent = String(score);
            document.getElementById('progressBar').style.width = ((currentIndex / total) * 100) + '%';

            document.getElementById('topicBadge').textContent = q.topic;
            document.getElementById('statementText').textContent = '"' + q.statement + '"';

            document.getElementById('answerButtons').classList.remove('hidden');
            document.getElementById('feedbackCard').classList.add('hidden');

            if (useTimer) {
                startTimer();
            }
        }

        function startTimer() {
            timeLeft = TIME_PER_QUESTION;
            const secondsEl = document.getElementById('secondsLeft');
            const barEl = document.getElementById('timerBar');
            const timerTextEl = document.getElementById('timerText');

            secondsEl.textContent = String(timeLeft);
            barEl.style.width = '100%';
            barEl.classList.remove('urgent');
            timerTextEl.classList.remove('urgent');

            const startTime = Date.now();
            const durationMs = TIME_PER_QUESTION * 1000;

            timerInterval = setInterval(function () {
                if (isAnswered) {
                    clearInterval(timerInterval);
                    return;
                }

                const elapsed = Date.now() - startTime;
                const remaining = Math.max(0, durationMs - elapsed);
                const sec = Math.ceil(remaining / 1000);
                const pct = (remaining / durationMs) * 100;

                secondsEl.textContent = String(sec);
                barEl.style.width = pct + '%';

                if (sec <= 5) {
                    barEl.classList.add('urgent');
                    timerTextEl.classList.add('urgent');
                }

                if (remaining <= 0) {
                    clearInterval(timerInterval);
                    submitAnswer('TIMEOUT');
                }
            }, 100);
        }

        function submitAnswer(choice) {
            if (isAnswered) return;
            isAnswered = true;
            clearInterval(timerInterval);

            const q = activeQuestions[currentIndex];
            const isCorrect = (choice === q.answer);
            const isTimeout = (choice === 'TIMEOUT');

            if (isCorrect) {
                score += 100;
                playTone('correct');
            } else {
                playTone('wrong');
            }

            userHistory.push({
                question: q,
                userChoice: choice,
                isCorrect: isCorrect
            });

            document.getElementById('currentScore').textContent = String(score);
            document.getElementById('progressBar').style.width = (((currentIndex + 1) / activeQuestions.length) * 100) + '%';
            document.getElementById('answerButtons').classList.add('hidden');

            const feedbackCard = document.getElementById('feedbackCard');
            const feedbackIcon = document.getElementById('feedbackIcon');
            const feedbackTitle = document.getElementById('feedbackTitle');
            const feedbackSubtitle = document.getElementById('feedbackSubtitle');
            const explanationText = document.getElementById('explanationText');
            const nextBtnText = document.getElementById('nextBtnText');

            explanationText.textContent = q.explanation;

            if (isCorrect) {
                feedbackCard.className = 'feedback-box feedback-correct';
                feedbackIcon.textContent = '🎉';
                feedbackTitle.textContent = 'Tepat! Ini adalah ' + q.answer + '.';
                feedbackSubtitle.textContent = '+100 Poin Sejarah Gastronomi';
            } else {
                feedbackCard.className = 'feedback-box feedback-wrong';
                feedbackIcon.textContent = isTimeout ? '⏰' : '💡';
                feedbackTitle.textContent = isTimeout
                    ? 'Waktu Habis! Jawabannya ' + q.answer + '.'
                    : 'Kurang Tepat! Ini adalah ' + q.answer + '.';
                feedbackSubtitle.textContent = 'Simak fakta di balik pernyataan ini:';
            }

            nextBtnText.textContent = (currentIndex === activeQuestions.length - 1)
                ? 'Lihat Hasil & Predikat'
                : 'Soal Berikutnya';

            feedbackCard.classList.remove('hidden');
        }

        function nextQuestion() {
            playTone('click');
            if (currentIndex < activeQuestions.length - 1) {
                currentIndex++;
                renderQuestion();
            } else {
                showResults();
            }
        }

        function showResults() {
            document.getElementById('quizScreen').classList.add('hidden');
            document.getElementById('resultScreen').classList.remove('hidden');

            const total = activeQuestions.length;
            const correctTotal = userHistory.filter(function (item) { return item.isCorrect; }).length;
            const accuracy = Math.round((correctTotal / total) * 100);

            document.getElementById('finalScore').textContent = String(score);
            document.getElementById('correctCount').textContent = correctTotal + '/' + total;
            document.getElementById('accuracyPercent').textContent = accuracy + '%';

            const rankEmoji = document.getElementById('rankEmoji');
            const rankTitle = document.getElementById('rankTitle');
            const rankDesc = document.getElementById('rankDescription');

            if (accuracy >= 80) {
                rankEmoji.textContent = '🏆';
                rankTitle.textContent = 'Duta Gastrodiplomasi';
                rankDesc.textContent = 'Luar biasa! Kamu menguasai sejarah mobilitas bahan, akulturasi rasa, dan diplomasi kuliner di balik rujak bihun.';
            } else if (accuracy >= 50) {
                rankEmoji.textContent = '🧭';
                rankTitle.textContent = 'Penjelajah Rasa Nusantara';
                rankDesc.textContent = 'Bagus sekali! Sebagian besar jejak sejarah rujak bihun sudah kamu kenali. Coba sesi berikutnya untuk soal-soal baru!';
            } else {
                rankEmoji.textContent = '🥣';
                rankTitle.textContent = 'Pencicip Sejarah Pemula';
                rankDesc.textContent = 'Terima kasih sudah bermain! Banyak kisah lintas benua dan variasi daerah menarik di balik semangkuk rujak bihun.';
            }

            const reviewList = document.getElementById('reviewList');
            reviewList.innerHTML = '';

            userHistory.forEach(function (item, idx) {
                const statusColor = item.isCorrect ? 'color:var(--lime-700);' : 'color:var(--chili-700);';
                const statusLabel = item.isCorrect ? '✓ Benar' : '✕ Keliru';
                const el = document.createElement('div');
                el.className = 'review-item';
                el.innerHTML =
                    '<div style="display:flex;align-items:center;justify-content:space-between;gap:8px;margin-bottom:4px;font-size:0.72rem;font-weight:800;">' +
                        '<span style="color:var(--terra-700);">Soal #' + (idx + 1) + ' • Kunci: ' + escapeHtml(item.question.answer) + '</span>' +
                        '<span style="' + statusColor + '">' + statusLabel + '</span>' +
                    '</div>' +
                    '<div style="font-size:0.8rem;font-weight:700;color:var(--terra-900);margin-bottom:4px;">"' + escapeHtml(item.question.statement) + '"</div>' +
                    '<div style="font-size:0.74rem;color:var(--terra-800);line-height:1.45;">' + escapeHtml(item.question.explanation) + '</div>';
                reviewList.appendChild(el);
            });
        }

        function resetToStart() {
            playTone('click');
            clearInterval(timerInterval);
            updatePoolStatus();
            document.getElementById('resultScreen').classList.add('hidden');
            document.getElementById('quizScreen').classList.add('hidden');
            document.getElementById('startScreen').classList.remove('hidden');
        }

        function escapeHtml(str) {
            return String(str)
                .replace(/&/g, '&amp;')
                .replace(/</g, '&lt;')
                .replace(/>/g, '&gt;')
                .replace(/"/g, '&quot;');
        }

        // =========================================================================
        // 3. MODAL RINGKAS 26 SOAL PENJAGA TENDA
        // =========================================================================
        function toggleBankModal() {
            playTone('click');
            const modal = document.getElementById('bankModal');
            const opening = modal.classList.contains('hidden');
            if (opening) {
                renderBankModal();
                modal.classList.remove('hidden');
            } else {
                modal.classList.add('hidden');
            }
        }

        function renderBankModal() {
            const listEl = document.getElementById('bankModalList');
            listEl.innerHTML = '';

            QUESTION_BANK.forEach(function (q, idx) {
                const badgeStyle = q.answer === 'FAKTA'
                    ? 'background:#DCFCE7;color:#166534;border:1px solid #86EFAC;'
                    : 'background:#FEE2E2;color:#991B1B;border:1px solid #FCA5A5;';
                const playedText = playedIds.has(q.id) ? 'Sudah Dimainkan' : 'Belum Dimainkan';

                const card = document.createElement('div');
                card.className = 'review-item';
                card.innerHTML =
                    '<div style="display:flex;align-items:center;justify-content:space-between;gap:8px;flex-wrap:wrap;margin-bottom:6px;">' +
                        '<span style="font-size:0.74rem;font-weight:800;color:var(--turmeric-600);">#' + (idx + 1) + ' • ' + escapeHtml(q.topic) + '</span>' +
                        '<div style="display:flex;align-items:center;gap:6px;">' +
                            '<span style="font-size:0.68rem;font-weight:700;color:var(--terra-700);">' + playedText + '</span>' +
                            '<span style="font-size:0.68rem;font-weight:800;padding:2px 8px;border-radius:999px;' + badgeStyle + '">' + escapeHtml(q.answer) + '</span>' +
                        '</div>' +
                    '</div>' +
                    '<div style="font-size:0.82rem;font-weight:700;color:var(--terra-900);margin-bottom:4px;">' + escapeHtml(q.statement) + '</div>' +
                    '<div style="font-size:0.74rem;color:var(--terra-800);line-height:1.45;">' + escapeHtml(q.explanation) + '</div>';
                listEl.appendChild(card);
            });
        }

        // =========================================================================
        // 4. QR CODE ENCODER MURNI JAVASCRIPT (TANPA CDN / TANPA PUSTAKA LUAR)
        //    Mendukung Byte Mode pada QR Version 1 hingga 4, ECC Level L.
        // =========================================================================
        const PureQRCode = (function () {
            // Tabel kapasitas Byte Mode (ECC Level L) untuk Version 1..4
            // Setiap entri: [version, size, totalCodewords, ecCodewords, dataCapacity]
            const VERSION_SPECS = [
                { version: 1, size: 21, totalCW: 26, ecCW: 7,  dataCap: 17 },
                { version: 2, size: 25, totalCW: 44, ecCW: 10, dataCap: 32 },
                { version: 3, size: 29, totalCW: 70, ecCW: 15, dataCap: 53 },
                { version: 4, size: 33, totalCW: 100, ecCW: 20, dataCap: 78 }
            ];

            // Posisi alignment pattern untuk Version 2..4
            const ALIGNMENT_CENTERS = {
                2: [6, 18],
                3: [6, 22],
                4: [6, 26]
            };

            // Galois Field GF(256) dengan primitive polynomial 0x11D (285)
            const GF_EXP = new Uint8Array(512);
            const GF_LOG = new Uint8Array(256);
            (function initGF() {
                let x = 1;
                for (let i = 0; i < 255; i++) {
                    GF_EXP[i] = x;
                    GF_LOG[x] = i;
                    x <<= 1;
                    if (x & 0x100) {
                        x ^= 0x11D;
                    }
                }
                for (let i = 255; i < 512; i++) {
                    GF_EXP[i] = GF_EXP[i - 255];
                }
            })();

            function gfMul(a, b) {
                if (a === 0 || b === 0) return 0;
                return GF_EXP[(GF_LOG[a] + GF_LOG[b]) % 255];
            }

            function getGeneratorPoly(ecLength) {
                let poly = [1];
                for (let i = 0; i < ecLength; i++) {
                    const factor = [1, GF_EXP[i]];
                    const next = new Array(poly.length + 1).fill(0);
                    for (let aIdx = 0; aIdx < poly.length; aIdx++) {
                        for (let bIdx = 0; bIdx < factor.length; bIdx++) {
                            next[aIdx + bIdx] ^= gfMul(poly[aIdx], factor[bIdx]);
                        }
                    }
                    poly = next;
                }
                return poly;
            }

            function computeECC(dataCodewords, ecLength) {
                const gen = getGeneratorPoly(ecLength);
                const ecc = new Array(ecLength).fill(0);
                for (let i = 0; i < dataCodewords.length; i++) {
                    const factor = dataCodewords[i] ^ ecc[0];
                    ecc.shift();
                    ecc.push(0);
                    for (let j = 0; j < ecLength; j++) {
                        ecc[j] ^= gfMul(gen[j + 1], factor);
                    }
                }
                return ecc;
            }

            function encodeUtf8Bytes(text) {
                if (typeof TextEncoder !== 'undefined') {
                    return Array.from(new TextEncoder().encode(text));
                }
                const utf8 = unescape(encodeURIComponent(text));
                const bytes = [];
                for (let i = 0; i < utf8.length; i++) {
                    bytes.push(utf8.charCodeAt(i));
                }
                return bytes;
            }

            function selectSpec(byteLength) {
                for (let i = 0; i < VERSION_SPECS.length; i++) {
                    if (byteLength <= VERSION_SPECS[i].dataCap) {
                        return VERSION_SPECS[i];
                    }
                }
                return null;
            }

            function buildCodewords(bytes, spec) {
                const bits = [];
                function pushBits(val, count) {
                    for (let i = count - 1; i >= 0; i--) {
                        bits.push((val >>> i) & 1);
                    }
                }

                // Mode indicator untuk Byte Mode = 0100 (4)
                pushBits(4, 4);
                // Character count indicator (8 bit untuk Version 1..9)
                pushBits(bytes.length, 8);
                // Data bytes
                for (let i = 0; i < bytes.length; i++) {
                    pushBits(bytes[i], 8);
                }

                const maxDataBits = spec.dataCap * 8;
                // Terminator (hingga 4 bit nol)
                const terminatorLen = Math.min(4, maxDataBits - bits.length);
                pushBits(0, terminatorLen);

                // Pad ke kelipatan 8 bit
                while (bits.length % 8 !== 0) {
                    bits.push(0);
                }

                const dataCodewords = [];
                for (let i = 0; i < bits.length; i += 8) {
                    let byteVal = 0;
                    for (let b = 0; b < 8; b++) {
                        byteVal = (byteVal << 1) | bits[i + b];
                    }
                    dataCodewords.push(byteVal);
                }

                // Pad codewords 0xEC dan 0x11 bergantian
                let padToggle = true;
                while (dataCodewords.length < spec.dataCap) {
                    dataCodewords.push(padToggle ? 0xEC : 0x11);
                    padToggle = !padToggle;
                }

                const ecc = computeECC(dataCodewords, spec.ecCW);
                return dataCodewords.concat(ecc);
            }

            function placeFinderPattern(matrix, reserved, top, left) {
                const size = matrix.length;
                for (let r = -1; r <= 7; r++) {
                    for (let c = -1; c <= 7; c++) {
                        const rr = top + r;
                        const cc = left + c;
                        if (rr < 0 || rr >= size || cc < 0 || cc >= size) continue;
                        const isBorder = (r === -1 || r === 7 || c === -1 || c === 7);
                        const isOuter = (r === 0 || r === 6 || c === 0 || c === 6);
                        const isCenter = (r >= 2 && r <= 4 && c >= 2 && c <= 4);
                        matrix[rr][cc] = (!isBorder && (isOuter || isCenter)) ? 1 : 0;
                        reserved[rr][cc] = true;
                    }
                }
            }

            function placeAlignmentPattern(matrix, reserved, centerR, centerC) {
                for (let r = -2; r <= 2; r++) {
                    for (let c = -2; c <= 2; c++) {
                        const rr = centerR + r;
                        const cc = centerC + c;
                        const isOuter = (Math.abs(r) === 2 || Math.abs(c) === 2);
                        const isCenter = (r === 0 && c === 0);
                        matrix[rr][cc] = (isOuter || isCenter) ? 1 : 0;
                        reserved[rr][cc] = true;
                    }
                }
            }

            function reserveFormatAreas(reserved, size) {
                for (let i = 0; i < 8; i++) {
                    if (i !== 6) {
                        reserved[8][i] = true;
                        reserved[i][8] = true;
                    }
                    reserved[8][size - 1 - i] = true;
                    reserved[size - 1 - i][8] = true;
                }
                reserved[8][8] = true;
                // Dark module selalu (4 * version + 9, 8) -> untuk v1..4 berada pada (size - 8, 8)
                reserved[size - 8][8] = true;
            }

            function placeDarkModule(matrix, size) {
                matrix[size - 8][8] = 1;
            }

            function evalMaskCondition(maskId, r, c) {
                switch (maskId) {
                    case 0: return (r + c) % 2 === 0;
                    case 1: return r % 2 === 0;
                    case 2: return c % 3 === 0;
                    case 3: return (r + c) % 3 === 0;
                    case 4: return (Math.floor(r / 2) + Math.floor(c / 3)) % 2 === 0;
                    case 5: return ((r * c) % 2) + ((r * c) % 3) === 0;
                    case 6: return ((((r * c) % 2) + ((r * c) % 3)) % 2) === 0;
                    case 7: return ((((r + c) % 2) + ((r * c) % 3)) % 2) === 0;
                    default: return (r + c) % 2 === 0;
                }
            }

            // Format string 15-bit untuk ECC Level L (01) + Mask (0..7)
            function getFormatBits(maskId) {
                const formatData = (1 << 3) | maskId; // 5-bit: 01xxx
                let rem = formatData << 10;
                const poly = 0x537; // generator x^10 + x^8 + x^5 + x^4 + x^2 + x + 1
                for (let bit = 14; bit >= 10; bit--) {
                    if ((rem >>> bit) & 1) {
                        rem ^= poly << (bit - 10);
                    }
                }
                const combined = ((formatData << 10) | rem) ^ 0x5412;
                return combined & 0x7FFF;
            }

            function placeFormatInfo(matrix, size, maskId) {
                const bits = getFormatBits(maskId);
                // Bit 14..0 diurutkan dari MSB ke LSB pada posisi standar
                const getBit = function (idxFromMsb) {
                    return (bits >>> (14 - idxFromMsb)) & 1;
                };

                // Sekitar finder kiri atas
                const aroundTopLeftRow = [0, 1, 2, 3, 4, 5, 7, 8, 8, 8, 8, 8, 8, 8, 8];
                const aroundTopLeftCol = [8, 8, 8, 8, 8, 8, 8, 8, 7, 5, 4, 3, 2, 1, 0];
                for (let i = 0; i < 15; i++) {
                    matrix[aroundTopLeftRow[i]][aroundTopLeftCol[i]] = getBit(i);
                }

                // Sekitar finder kanan atas & bawah kiri
                for (let i = 0; i < 8; i++) {
                    matrix[8][size - 1 - i] = getBit(i);
                }
                for (let i = 8; i < 15; i++) {
                    matrix[size - 15 + i][8] = getBit(i);
                }
            }

            function scoreMatrix(matrix) {
                const size = matrix.length;
                let penalty = 0;

                // Aturan 1: runs horizontal & vertikal >= 5
                for (let r = 0; r < size; r++) {
                    let runColor = matrix[r][0];
                    let runLen = 1;
                    for (let c = 1; c < size; c++) {
                        if (matrix[r][c] === runColor) {
                            runLen++;
                        } else {
                            if (runLen >= 5) penalty += 3 + (runLen - 5);
                            runColor = matrix[r][c];
                            runLen = 1;
                        }
                    }
                    if (runLen >= 5) penalty += 3 + (runLen - 5);
                }

                for (let c = 0; c < size; c++) {
                    let runColor = matrix[0][c];
                    let runLen = 1;
                    for (let r = 1; r < size; r++) {
                        if (matrix[r][c] === runColor) {
                            runLen++;
                        } else {
                            if (runLen >= 5) penalty += 3 + (runLen - 5);
                            runColor = matrix[r][c];
                            runLen = 1;
                        }
                    }
                    if (runLen >= 5) penalty += 3 + (runLen - 5);
                }

                // Aturan 2: blok 2x2 seragam
                for (let r = 0; r < size - 1; r++) {
                    for (let c = 0; c < size - 1; c++) {
                        const v = matrix[r][c];
                        if (v === matrix[r][c + 1] && v === matrix[r + 1][c] && v === matrix[r + 1][c + 1]) {
                            penalty += 3;
                        }
                    }
                }

                return penalty;
            }

            function createMatrix(text) {
                const bytes = encodeUtf8Bytes(text);
                const spec = selectSpec(bytes.length);
                if (!spec) {
                    return null;
                }

                const size = spec.size;
                const codewords = buildCodewords(bytes, spec);

                const baseMatrix = Array.from({ length: size }, function () {
                    return new Array(size).fill(0);
                });
                const reserved = Array.from({ length: size }, function () {
                    return new Array(size).fill(false);
                });

                placeFinderPattern(baseMatrix, reserved, 0, 0);
                placeFinderPattern(baseMatrix, reserved, 0, size - 7);
                placeFinderPattern(baseMatrix, reserved, size - 7, 0);

                if (ALIGNMENT_CENTERS[spec.version]) {
                    const centers = ALIGNMENT_CENTERS[spec.version];
                    for (let rIdx = 0; rIdx < centers.length; rIdx++) {
                        for (let cIdx = 0; cIdx < centers.length; cIdx++) {
                            const cr = centers[rIdx];
                            const cc = centers[cIdx];
                            const overlapsFinder =
                                (cr <= 8 && cc <= 8) ||
                                (cr <= 8 && cc >= size - 9) ||
                                (cr >= size - 9 && cc <= 8);
                            if (!overlapsFinder) {
                                placeAlignmentPattern(baseMatrix, reserved, cr, cc);
                            }
                        }
                    }
                }

                reserveFormatAreas(reserved, size);
                placeDarkModule(baseMatrix, size);

                // Tempatkan bit data secara zigzag ke atas & ke bawah
                const dataBits = [];
                for (let i = 0; i < codewords.length; i++) {
                    for (let b = 7; b >= 0; b--) {
                        dataBits.push((codewords[i] >>> b) & 1);
                    }
                }

                let bitPtr = 0;
                let goingUp = true;
                for (let col = size - 1; col >= 1; col -= 2) {
                    if (col === 6) col = 5; // Lewati timing column vertikal ke-6
                    for (let step = 0; step < size; step++) {
                        const row = goingUp ? (size - 1 - step) : step;
                        for (let offset = 0; offset < 2; offset++) {
                            const c = col - offset;
                            if (!reserved[row][c]) {
                                const bitVal = bitPtr < dataBits.length ? dataBits[bitPtr++] : 0;
                                baseMatrix[row][c] = bitVal;
                            }
                        }
                    }
                    goingUp = !goingUp;
                }

                // Pilih mask terbaik dari 8 pola mask
                let bestMatrix = null;
                let bestPenalty = Infinity;

                for (let maskId = 0; maskId < 8; maskId++) {
                    const candidate = baseMatrix.map(function (row) { return row.slice(); });
                    for (let r = 0; r < size; r++) {
                        for (let c = 0; c < size; c++) {
                            if (!reserved[r][c] && evalMaskCondition(maskId, r, c)) {
                                candidate[r][c] ^= 1;
                            }
                        }
                    }
                    placeFormatInfo(candidate, size, maskId);
                    const penalty = scoreMatrix(candidate);
                    if (penalty < bestPenalty) {
                        bestPenalty = penalty;
                        bestMatrix = candidate;
                    }
                }

                return bestMatrix;
            }

            function renderToContainer(containerEl, text, pixelSize) {
                if (!containerEl) return;
                containerEl.innerHTML = '';

                const matrix = createMatrix(text);
                if (!matrix) {
                    const warn = document.createElement('div');
                    warn.style.cssText = 'max-width:220px;font-size:0.75rem;color:var(--chili-700);font-weight:700;padding:12px;';
                    warn.textContent = 'URL terlalu panjang untuk QR standar (maks. ~70 karakter). Gunakan tautan yang lebih ringkas.';
                    containerEl.appendChild(warn);
                    return;
                }

                const count = matrix.length;
                const quietZone = 2; // Margin quiet zone minimal di sekeliling QR
                const totalCells = count + quietZone * 2;
                const cellPx = pixelSize / totalCells;

                const svgNS = 'http://www.w3.org/2000/svg';
                const svg = document.createElementNS(svgNS, 'svg');
                svg.setAttribute('width', String(pixelSize));
                svg.setAttribute('height', String(pixelSize));
                svg.setAttribute('viewBox', '0 0 ' + pixelSize + ' ' + pixelSize);
                svg.setAttribute('role', 'img');
                svg.setAttribute('aria-label', 'Kode QR Tautan Game Rujak Bihun');
                svg.style.display = 'block';

                const bg = document.createElementNS(svgNS, 'rect');
                bg.setAttribute('x', '0');
                bg.setAttribute('y', '0');
                bg.setAttribute('width', String(pixelSize));
                bg.setAttribute('height', String(pixelSize));
                bg.setAttribute('fill', '#FFFFFF');
                svg.appendChild(bg);

                for (let r = 0; r < count; r++) {
                    for (let c = 0; c < count; c++) {
                        if (matrix[r][c] === 1) {
                            const rect = document.createElementNS(svgNS, 'rect');
                            const x = ((c + quietZone) * cellPx).toFixed(2);
                            const y = ((r + quietZone) * cellPx).toFixed(2);
                            const w = (cellPx + 0.35).toFixed(2);
                            rect.setAttribute('x', x);
                            rect.setAttribute('y', y);
                            rect.setAttribute('width', w);
                            rect.setAttribute('height', w);
                            rect.setAttribute('fill', '#2D160A');
                            svg.appendChild(rect);
                        }
                    }
                }

                containerEl.appendChild(svg);
            }

            return {
                createMatrix: createMatrix,
                renderToContainer: renderToContainer
            };
        })();

        // =========================================================================
        // 5. KONTROL QR CODE, UNDUH PNG & MODE POSTER TENDA PAMERAN
        // =========================================================================
        function getDefaultExhibitionUrl() {
            try {
                const href = window.location.href || '';
                if (href.startsWith('http://') || href.startsWith('https://')) {
                    return href.split('#')[0];
                }
            } catch (err) {
                // Abaikan jika lokasi tidak dapat diakses
            }
            return 'https://pameran-sejarah.id/rujak-bihun';
        }

        function resolveQrUrl(rawValue) {
            const trimmed = (rawValue || '').trim();
            if (!trimmed) {
                return getDefaultExhibitionUrl();
            }
            if (/^https?:\/\//i.test(trimmed)) {
                return trimmed;
            }
            return 'https://' + trimmed;
        }

        function updateAllQrViews(urlText) {
            const previewEl = document.getElementById('qrPreviewUrl');
            const posterCaptionEl = document.getElementById('posterUrlCaption');
            const statusNoteEl = document.getElementById('qrStatusNote');

            if (previewEl) previewEl.textContent = urlText;
            if (posterCaptionEl) posterCaptionEl.textContent = urlText;

            const matrix = PureQRCode.createMatrix(urlText);
            if (statusNoteEl) {
                if (!matrix) {
                    statusNoteEl.textContent = 'Panjang tautan melebihi kapasitas QR ringkas (maks. ~70 karakter). Pendekkan URL agar QR dapat dibentuk.';
                } else if (urlText.startsWith('file://')) {
                    statusNoteEl.textContent = 'Catatan: Alamat masih lokal (file://). Masukkan tautan publik/HTTP agar mudah dipindai kamera ponsel pengunjung.';
                } else {
                    statusNoteEl.textContent = 'QR Code siap dipindai langsung menggunakan kamera ponsel pengunjung (100% lokal tanpa CDN).';
                }
            }

            PureQRCode.renderToContainer(document.getElementById('qrContainer'), urlText, 196);
            PureQRCode.renderToContainer(document.getElementById('posterQrContainer'), urlText, 224);
        }

        function handleQrUrlChange() {
            const inputEl = document.getElementById('qrUrlInput');
            const resolved = resolveQrUrl(inputEl ? inputEl.value : '');
            updateAllQrViews(resolved);
        }

        function useCurrentPageUrl() {
            playTone('click');
            const inputEl = document.getElementById('qrUrlInput');
            const url = getDefaultExhibitionUrl();
            if (inputEl) {
                inputEl.value = url;
            }
            updateAllQrViews(url);
        }

        function toggleQrModal() {
            playTone('click');
            const modal = document.getElementById('qrModal');
            const isOpening = modal.classList.contains('hidden');
            if (isOpening) {
                const inputEl = document.getElementById('qrUrlInput');
                if (inputEl && !inputEl.value.trim()) {
                    inputEl.value = getDefaultExhibitionUrl();
                }
                updateAllQrViews(resolveQrUrl(inputEl ? inputEl.value : ''));
                modal.classList.remove('hidden');
            } else {
                modal.classList.add('hidden');
            }
        }

        function openPosterMode() {
            playTone('click');
            handleQrUrlChange();
            document.getElementById('qrModal').classList.add('hidden');
            document.getElementById('posterModal').classList.remove('hidden');
        }

        function closePosterMode(returnToQrModal) {
            playTone('click');
            document.body.classList.remove('printing-poster');
            document.getElementById('posterModal').classList.add('hidden');
            if (returnToQrModal) {
                document.getElementById('qrModal').classList.remove('hidden');
            }
        }

        function printPosterStand() {
            playTone('click');
            handleQrUrlChange();
            document.body.classList.add('printing-poster');
            window.print();
        }

        function downloadQrPng() {
            playTone('click');
            handleQrUrlChange();
            const inputEl = document.getElementById('qrUrlInput');
            const urlText = resolveQrUrl(inputEl ? inputEl.value : '');
            const matrix = PureQRCode.createMatrix(urlText);
            if (!matrix) return;

            const exportSize = 512;
            const canvas = document.createElement('canvas');
            canvas.width = exportSize;
            canvas.height = exportSize;
            const ctx = canvas.getContext('2d');
            if (!ctx) return;

            const count = matrix.length;
            const quietZone = 3;
            const totalCells = count + quietZone * 2;
            const cellPx = exportSize / totalCells;

            ctx.fillStyle = '#FFFFFF';
            ctx.fillRect(0, 0, exportSize, exportSize);

            ctx.fillStyle = '#2D160A';
            for (let r = 0; r < count; r++) {
                for (let c = 0; c < count; c++) {
                    if (matrix[r][c] === 1) {
                        const x = Math.round((c + quietZone) * cellPx);
                        const y = Math.round((r + quietZone) * cellPx);
                        const w = Math.ceil(cellPx);
                        ctx.fillRect(x, y, w, w);
                    }
                }
            }

            try {
                const dataUrl = canvas.toDataURL('image/png');
                const link = document.createElement('a');
                link.download = 'qr-mitos-atau-fakta-rujak-bihun.png';
                link.href = dataUrl;
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            } catch (err) {
                // Abaikan jika pengunduhan dibatasi lingkungan
            }
        }

        // Inisialisasi awal saat halaman dibuka
        updatePoolStatus();
        const initialUrlInput = document.getElementById('qrUrlInput');
        if (initialUrlInput && !initialUrlInput.value.trim()) {
            initialUrlInput.value = getDefaultExhibitionUrl();
        }
        updateAllQrViews(resolveQrUrl(initialUrlInput ? initialUrlInput.value : ''));
    </script>
</body>
</html>
```