<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Главная страница</title>
    
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    
<style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');

    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    html {
        scroll-behavior: smooth;
    }

    body.locked {
        overflow: hidden;
        height: 100vh;
    }

    body {
        font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
        line-height: 1.6;
        overflow-x: hidden;
        min-height: 100vh;
        position: relative;
        transition: background 0.6s ease, color 0.6s ease;
    }

    body.theme-light {
        color: #2d2d4a;
        background: #fff5ec;
    }

    body.theme-dark {
        color: #e0e0e0;
        background: #0a0a1a;
    }

    .bg-light {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -3;
        background: linear-gradient(-45deg, #ffecd2, #fcb69f, #a1c4fd, #c2e9fb, #ffd6e0, #ffb6c1);
        background-size: 400% 400%;
        animation: gradientShift 20s ease infinite;
        transition: opacity 0.6s ease;
    }

    body.theme-dark .bg-light {
        opacity: 0;
        pointer-events: none;
    }

    @keyframes gradientShift {
        0% { background-position: 0% 50%; }
        50% { background-position: 100% 50%; }
        100% { background-position: 0% 50%; }
    }

    .blob {
        position: fixed;
        border-radius: 50%;
        filter: blur(60px);
        opacity: 0.5;
        z-index: -2;
        pointer-events: none;
        transition: opacity 0.6s ease;
    }

    body.theme-dark .blob {
        opacity: 0;
    }

    .blob.blob1 {
        width: 400px;
        height: 400px;
        background: #ff9a9e;
        top: -100px;
        left: -100px;
        animation: blobFloat1 15s ease-in-out infinite;
    }

    .blob.blob2 {
        width: 500px;
        height: 500px;
        background: #a1c4fd;
        top: 30%;
        right: -150px;
        animation: blobFloat2 18s ease-in-out infinite;
    }

    .blob.blob3 {
        width: 350px;
        height: 350px;
        background: #ffb6c1;
        bottom: 10%;
        left: 20%;
        animation: blobFloat3 20s ease-in-out infinite;
    }

    .blob.blob4 {
        width: 300px;
        height: 300px;
        background: #b5d8ff;
        top: 60%;
        left: -100px;
        animation: blobFloat1 22s ease-in-out infinite reverse;
    }

    @keyframes blobFloat1 {
        0%, 100% { transform: translate(0, 0) scale(1); }
        33% { transform: translate(50px, 80px) scale(1.1); }
        66% { transform: translate(-30px, 40px) scale(0.95); }
    }

    @keyframes blobFloat2 {
        0%, 100% { transform: translate(0, 0) scale(1); }
        33% { transform: translate(-60px, 50px) scale(1.05); }
        66% { transform: translate(40px, -30px) scale(0.9); }
    }

    @keyframes blobFloat3 {
        0%, 100% { transform: translate(0, 0) scale(1); }
        50% { transform: translate(70px, -60px) scale(1.15); }
    }

    .bubbles {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        pointer-events: none;
        z-index: -1;
        overflow: hidden;
        transition: opacity 0.6s ease;
    }

    body.theme-dark .bubbles {
        opacity: 0;
        pointer-events: none;
    }

    .bubble {
        position: absolute;
        bottom: -100px;
        border-radius: 50%;
        background: radial-gradient(circle at 30% 30%, rgba(255,255,255,0.8), rgba(255,255,255,0.2));
        border: 1px solid rgba(255, 255, 255, 0.5);
        animation: rise linear infinite;
        box-shadow: inset 0 0 10px rgba(255, 255, 255, 0.5);
    }

    @keyframes rise {
        0% { transform: translateY(0) translateX(0); opacity: 0; }
        10% { opacity: 0.8; }
        90% { opacity: 0.8; }
        100% { transform: translateY(-110vh) translateX(100px); opacity: 0; }
    }

    #starCanvas {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: 0;
        pointer-events: none;
        opacity: 0;
        transition: opacity 0.6s ease;
    }

    body.theme-dark #starCanvas {
        opacity: 1;
    }

    .glow-orb {
        position: fixed;
        border-radius: 50%;
        filter: blur(80px);
        pointer-events: none;
        z-index: 0;
        opacity: 0;
        transition: opacity 0.6s ease;
    }

    body.theme-dark .glow-orb {
        opacity: 1;
    }

    .glow-orb.orb1 {
        width: 500px;
        height: 500px;
        background: rgba(120, 100, 255, 0.12);
        top: -100px;
        right: -100px;
        animation: float 8s ease-in-out infinite;
    }

    .glow-orb.orb2 {
        width: 400px;
        height: 400px;
        background: rgba(100, 200, 255, 0.1);
        bottom: 10%;
        left: -150px;
        animation: float 10s ease-in-out infinite reverse;
    }

    .glow-orb.orb3 {
        width: 350px;
        height: 350px;
        background: rgba(255, 130, 200, 0.08);
        top: 50%;
        right: -100px;
        animation: float 12s ease-in-out infinite 2s;
    }

    @keyframes float {
        0%, 100% { transform: translate(0, 0); }
        33% { transform: translate(30px, -30px); }
        66% { transform: translate(-20px, 20px); }
    }

    header {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        z-index: 100;
        transition: all 0.6s ease;
    }

    body.theme-light header {
        background: rgba(255, 255, 255, 0.25);
        backdrop-filter: blur(20px);
        -webkit-backdrop-filter: blur(20px);
        border-bottom: 1px solid rgba(255, 255, 255, 0.4);
        box-shadow: 0 4px 30px rgba(0, 0, 0, 0.05);
    }

    body.theme-dark header {
        background: rgba(10, 10, 26, 0.8);
        backdrop-filter: blur(20px);
        -webkit-backdrop-filter: blur(20px);
        border-bottom: 1px solid rgba(255, 255, 255, 0.08);
    }

    .header-inner {
        max-width: 1200px;
        margin: 0 auto;
        padding: 18px 40px;
        display: flex;
        gap: 20px;
        align-items: center;
        justify-content: space-between;
        flex-wrap: wrap;
    }

    .burger {
        display: none;
        flex-direction: column;
        gap: 5px;
        cursor: pointer;
        padding: 10px;
        z-index: 101;
    }

    .burger span {
        width: 25px;
        height: 3px;
        border-radius: 3px;
        transition: all 0.3s ease;
    }

    body.theme-light .burger span {
        background: #2d2d4a;
    }

    body.theme-dark .burger span {
        background: #fff;
    }

    .burger.active span:nth-child(1) {
        transform: rotate(45deg) translate(6px, 6px);
    }

    .burger.active span:nth-child(2) {
        opacity: 0;
    }

    .burger.active span:nth-child(3) {
        transform: rotate(-45deg) translate(6px, -6px);
    }

    nav {
        display: flex;
        gap: 16px;
        align-items: center;
        flex-wrap: wrap;
    }

    nav a {
        text-decoration: none;
        font-size: 15px;
        font-weight: 700;
        padding: 12px 26px;
        border-radius: 50px;
        transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
        position: relative;
        overflow: hidden;
        letter-spacing: 0.3px;
        cursor: pointer;
    }

    nav a::before {
        content: '';
        position: absolute;
        top: 0;
        left: -100%;
        width: 100%;
        height: 100%;
        background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
        transition: left 0.6s ease;
    }

    nav a:hover::before {
        left: 100%;
    }

    nav a:hover {
        transform: translateY(-3px) scale(1.05);
    }

    body.theme-light nav a {
        color: #fff;
        text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
    }

    body.theme-light nav a.tab1 {
        background: linear-gradient(135deg, #ff6a88 0%, #ff99ac 100%);
        box-shadow: 0 6px 20px rgba(255, 106, 136, 0.45);
    }

    body.theme-light nav a.tab2 {
        background: linear-gradient(135deg, #a18cd1 0%, #fbc2eb 100%);
        box-shadow: 0 6px 20px rgba(161, 140, 209, 0.45);
    }

    body.theme-light nav a.tab3 {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        box-shadow: 0 6px 20px rgba(102, 126, 234, 0.45);
    }

    body.theme-dark nav a {
        color: #fff;
        border: 1px solid rgba(255, 255, 255, 0.15);
        background: linear-gradient(135deg, rgba(255,255,255,0.08), rgba(255,255,255,0.02));
    }

    body.theme-dark nav a:nth-child(1) {
        border-color: rgba(120, 100, 255, 0.4);
        box-shadow: 0 0 15px rgba(120, 100, 255, 0.2);
    }

    body.theme-dark nav a:nth-child(2) {
        border-color: rgba(100, 200, 255, 0.4);
        box-shadow: 0 0 15px rgba(100, 200, 255, 0.2);
    }

    body.theme-dark nav a:nth-child(3) {
        border-color: rgba(255, 130, 200, 0.4);
        box-shadow: 0 0 15px rgba(255, 130, 200, 0.2);
    }

    .theme-toggle {
        width: 64px;
        height: 34px;
        border-radius: 50px;
        position: relative;
        cursor: pointer;
        transition: all 0.4s ease;
        flex-shrink: 0;
    }

    body.theme-light .theme-toggle {
        background: linear-gradient(135deg, #ffecd2, #fcb69f);
        box-shadow: inset 0 2px 6px rgba(0, 0, 0, 0.1), 0 0 15px rgba(252, 182, 159, 0.4);
    }

    body.theme-dark .theme-toggle {
        background: linear-gradient(135deg, #1a1a3e, #2d1b4e);
        box-shadow: inset 0 2px 6px rgba(0, 0, 0, 0.4), 0 0 15px rgba(120, 100, 255, 0.3);
    }

    .theme-toggle .toggle-knob {
        position: absolute;
        top: 3px;
        width: 28px;
        height: 28px;
        border-radius: 50%;
        transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 16px;
    }

    body.theme-light .toggle-knob {
        left: 3px;
        background: #fff;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
    }

    body.theme-dark .toggle-knob {
        left: 33px;
        background: #1a1a2e;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
    }

    .description {
        min-height: 100vh;
        padding: 140px 40px 120px;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        z-index: 2;
    }

    .description-inner {
        max-width: 800px;
        text-align: center;
        transition: all 0.6s ease;
    }

    body.theme-light .description-inner {
        background: rgba(255, 255, 255, 0.35);
        backdrop-filter: blur(15px);
        -webkit-backdrop-filter: blur(15px);
        padding: 60px 50px;
        border-radius: 30px;
        border: 1px solid rgba(255, 255, 255, 0.5);
        box-shadow: 0 20px 60px rgba(0, 0, 0, 0.08);
    }

    body.theme-dark .description-inner {
        background: transparent;
        padding: 60px 50px;
    }

    .description h2 {
        font-size: 56px;
        font-weight: 800;
        margin-bottom: 24px;
        letter-spacing: -2px;
        transition: all 0.6s ease;
    }

    body.theme-light .description h2 {
        background: linear-gradient(135deg, #ff6a88 0%, #a18cd1 50%, #667eea 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
    }

    body.theme-dark .description h2 {
        background: linear-gradient(135deg, #7864ff, #64c8ff, #ff82c8);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
    }

    .description p {
        font-size: 20px;
        line-height: 1.8;
        font-weight: 500;
        transition: all 0.6s ease;
    }

    body.theme-light .description p {
        color: #3d3d5c;
    }

    body.theme-dark .description p {
        color: rgba(255, 255, 255, 0.6);
    }

    .btn-start {
        display: inline-block;
        margin-top: 40px;
        padding: 18px 50px;
        font-size: 17px;
        font-weight: 700;
        font-family: inherit;
        color: #fff;
        border: none;
        border-radius: 50px;
        cursor: pointer;
        transition: all 0.4s ease;
        position: relative;
        overflow: hidden;
        letter-spacing: 0.5px;
    }

    body.theme-light .btn-start {
        background: linear-gradient(135deg, #ff6a88 0%, #a18cd1 100%);
        box-shadow: 0 8px 25px rgba(161, 140, 209, 0.4);
    }

    body.theme-dark .btn-start {
        background: linear-gradient(135deg, #7864ff, #64c8ff);
        box-shadow: 0 8px 25px rgba(120, 100, 255, 0.35);
    }

    .btn-start::before {
        content: '';
        position: absolute;
        top: 0;
        left: -100%;
        width: 100%;
        height: 100%;
        background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
        transition: left 0.6s ease;
    }

    .btn-start:hover::before {
        left: 100%;
    }

    .btn-start:hover {
        transform: translateY(-3px);
    }

    body.theme-light .btn-start:hover {
        box-shadow: 0 12px 35px rgba(161, 140, 209, 0.55);
    }

    body.theme-dark .btn-start:hover {
        box-shadow: 0 15px 40px rgba(120, 100, 255, 0.5);
    }

    .scroll-hint {
        margin-top: 40px;
        opacity: 0;
        transition: opacity 0.6s ease;
    }

    body.locked .scroll-hint {
        display: none;
    }

    body.theme-dark .scroll-hint {
        opacity: 1;
    }

    body.theme-light .scroll-hint {
        opacity: 0.7;
    }

    .scroll-hint span {
        display: inline-block;
        font-size: 13px;
        letter-spacing: 2px;
        text-transform: uppercase;
        font-weight: 600;
    }

    body.theme-light .scroll-hint span {
        color: rgba(61, 61, 92, 0.5);
    }

    body.theme-dark .scroll-hint span {
        color: rgba(255, 255, 255, 0.3);
    }

    .scroll-arrow {
        display: block;
        margin: 12px auto 0;
        width: 20px;
        height: 20px;
        transform: rotate(45deg);
        animation: bounce 2s ease infinite;
    }

    body.theme-light .scroll-arrow {
        border-right: 2px solid rgba(61, 61, 92, 0.4);
        border-bottom: 2px solid rgba(61, 61, 92, 0.4);
    }

    body.theme-dark .scroll-arrow {
        border-right: 2px solid rgba(255, 255, 255, 0.3);
        border-bottom: 2px solid rgba(255, 255, 255, 0.3);
    }

    @keyframes bounce {
        0%, 20%, 50%, 80%, 100% { transform: rotate(45deg) translateY(0); }
        40% { transform: rotate(45deg) translateY(8px); }
        60% { transform: rotate(45deg) translateY(4px); }
    }

    .wave-divider {
        position: relative;
        width: 100%;
        height: 120px;
        z-index: 2;
        margin-top: -1px;
    }

    .wave-divider svg {
        width: 100%;
        height: 100%;
        display: block;
    }

    .department-selector {
        min-height: 100vh;
        padding: 80px 40px 120px;
        max-width: 700px;
        margin: 0 auto;
        display: flex;
        flex-direction: column;
        justify-content: center;
        position: relative;
        z-index: 2;
    }

    .selector-card {
        padding: 60px 50px;
        border-radius: 30px;
        animation: fadeInUp 1s ease;
        transition: all 0.6s ease;
    }

    body.theme-light .selector-card {
        background: rgba(255, 255, 255, 0.45);
        backdrop-filter: blur(20px);
        -webkit-backdrop-filter: blur(20px);
        border: 1px solid rgba(255, 255, 255, 0.6);
        box-shadow: 0 20px 60px rgba(0, 0, 0, 0.1);
    }

    body.theme-dark .selector-card {
        background: rgba(255, 255, 255, 0.03);
        backdrop-filter: blur(10px);
        -webkit-backdrop-filter: blur(10px);
        border: 1px solid rgba(255, 255, 255, 0.08);
        box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
    }

    .department-selector h2 {
        font-size: 42px;
        margin-bottom: 12px;
        text-align: center;
        font-weight: 800;
        letter-spacing: -1px;
        transition: all 0.6s ease;
    }

    body.theme-light .department-selector h2 {
        background: linear-gradient(135deg, #a18cd1 0%, #ff6a88 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
    }

    body.theme-dark .department-selector h2 {
        background: linear-gradient(135deg, #64c8ff, #7864ff);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
    }

    .department-selector .subtitle {
        text-align: center;
        margin-bottom: 40px;
        font-size: 17px;
        font-weight: 500;
        transition: all 0.6s ease;
    }

    body.theme-light .department-selector .subtitle {
        color: #5a5a7a;
    }

    body.theme-dark .department-selector .subtitle {
        color: rgba(255, 255, 255, 0.45);
    }

    .select-wrapper {
        position: relative;
        margin-bottom: 24px;
    }

    select {
        width: 100%;
        padding: 20px 24px;
        font-size: 17px;
        font-family: inherit;
        border-radius: 16px;
        appearance: none;
        cursor: pointer;
        transition: all 0.3s ease;
        font-weight: 600;
    }

    body.theme-light select {
        border: 2px solid rgba(255, 255, 255, 0.6);
        background: rgba(255, 255, 255, 0.5);
        color: #2d2d4a;
    }

    body.theme-light select option {
        background: #fff;
        color: #2d2d4a;
    }

    body.theme-light select:focus {
        outline: none;
        border-color: #a18cd1;
        background: rgba(255, 255, 255, 0.8);
        box-shadow: 0 0 0 4px rgba(161, 140, 209, 0.2);
    }

    body.theme-dark select {
        border: 1px solid rgba(255, 255, 255, 0.12);
        background: rgba(255, 255, 255, 0.05);
        color: #fff;
        backdrop-filter: blur(10px);
    }

    body.theme-dark select option {
        background: #1a1a2e;
        color: #fff;
    }

    body.theme-dark select:focus {
        outline: none;
        border-color: rgba(120, 100, 255, 0.5);
        box-shadow: 0 0 30px rgba(120, 100, 255, 0.15);
    }

    .select-wrapper::after {
        content: '▼';
        position: absolute;
        right: 24px;
        top: 50%;
        transform: translateY(-50%);
        pointer-events: none;
        font-size: 12px;
        transition: color 0.6s ease;
    }

    body.theme-light .select-wrapper::after {
        color: #a18cd1;
    }

    body.theme-dark .select-wrapper::after {
        color: rgba(255, 255, 255, 0.4);
    }

    .btn-continue {
        width: 100%;
        padding: 20px;
        font-size: 17px;
        font-weight: 700;
        font-family: inherit;
        color: #fff;
        border: none;
        border-radius: 16px;
        cursor: pointer;
        transition: all 0.4s ease;
        position: relative;
        overflow: hidden;
        letter-spacing: 0.5px;
        margin-bottom: 16px;
    }

    body.theme-light .btn-continue {
        background: linear-gradient(135deg, #ff6a88 0%, #a18cd1 50%, #667eea 100%);
        background-size: 200% 200%;
        box-shadow: 0 8px 25px rgba(161, 140, 209, 0.4);
        text-shadow: 0 1px 2px rgba(0, 0, 0, 0.15);
        animation: btnGradient 5s ease infinite;
    }

    body.theme-dark .btn-continue {
        background: linear-gradient(135deg, #7864ff, #64c8ff);
        box-shadow: 0 8px 25px rgba(120, 100, 255, 0.35);
    }

    @keyframes btnGradient {
        0%, 100% { background-position: 0% 50%; }
        50% { background-position: 100% 50%; }
    }

    .btn-continue::before {
        content: '';
        position: absolute;
        top: 0;
        left: -100%;
        width: 100%;
        height: 100%;
        background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
        transition: left 0.6s ease;
    }

    .btn-continue:hover:not(:disabled)::before {
        left: 100%;
    }

    .btn-continue:hover:not(:disabled) {
        transform: translateY(-3px);
    }

    body.theme-light .btn-continue:hover:not(:disabled) {
        box-shadow: 0 12px 35px rgba(161, 140, 209, 0.55);
    }

    body.theme-dark .btn-continue:hover:not(:disabled) {
        box-shadow: 0 15px 40px rgba(120, 100, 255, 0.5);
    }

    body.theme-light .btn-continue:disabled {
        background: rgba(255, 255, 255, 0.4);
        color: rgba(45, 45, 74, 0.4);
        cursor: not-allowed;
        box-shadow: none;
        animation: none;
    }

    body.theme-dark .btn-continue:disabled {
        background: rgba(255, 255, 255, 0.08);
        color: rgba(255, 255, 255, 0.25);
        cursor: not-allowed;
        box-shadow: none;
    }

    .btn-back {
        width: 100%;
        padding: 16px;
        font-size: 15px;
        font-weight: 600;
        font-family: inherit;
        border: 2px solid;
        border-radius: 16px;
        cursor: pointer;
        transition: all 0.3s ease;
        background: transparent;
    }

    body.theme-light .btn-back {
        border-color: rgba(161, 140, 209, 0.4);
        color: #a18cd1;
    }

    body.theme-light .btn-back:hover {
        background: rgba(161, 140, 209, 0.1);
        border-color: #a18cd1;
    }

    body.theme-dark .btn-back {
        border-color: rgba(120, 100, 255, 0.4);
        color: #7864ff;
    }

    body.theme-dark .btn-back:hover {
        background: rgba(120, 100, 255, 0.1);
        border-color: #7864ff;
    }

    /* ===== БЛОК ЧАТА ===== */
    .chat-section {
        min-height: 100vh;
        padding: 120px 40px 80px;
        max-width: 1200px;
        margin: 0 auto;
        position: relative;
        z-index: 2;
        display: none;
    }

    .chat-section.visible {
        display: block;
        animation: fadeInUp 0.8s ease;
    }

    .chat-container {
        display: grid;
        grid-template-columns: 350px 1fr;
        gap: 30px;
        padding: 40px;
        border-radius: 30px;
        transition: all 0.6s ease;
        height: calc(100vh - 160px);
        max-height: 800px;
    }

    body.theme-light .chat-container {
        background: rgba(255, 255, 255, 0.45);
        backdrop-filter: blur(20px);
        -webkit-backdrop-filter: blur(20px);
        border: 1px solid rgba(255, 255, 255, 0.6);
        box-shadow: 0 20px 60px rgba(0, 0, 0, 0.1);
    }

    body.theme-dark .chat-container {
        background: rgba(255, 255, 255, 0.03);
        backdrop-filter: blur(10px);
        -webkit-backdrop-filter: blur(10px);
        border: 1px solid rgba(255, 255, 255, 0.08);
        box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
    }

    .person-card {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 30px;
        border-radius: 20px;
        transition: all 0.6s ease;
        overflow-y: auto;
    }

    body.theme-light .person-card {
        background: rgba(255, 255, 255, 0.6);
        border: 1px solid rgba(255, 255, 255, 0.8);
    }

    body.theme-dark .person-card {
        background: rgba(255, 255, 255, 0.05);
        border: 1px solid rgba(255, 255, 255, 0.1);
    }

    .person-photo {
        width: 150px;
        height: 150px;
        border-radius: 50%;
        margin-bottom: 20px;
        object-fit: cover;
        border: 4px solid;
        transition: all 0.6s ease;
        flex-shrink: 0;
    }

    body.theme-light .person-photo {
        border-color: #a18cd1;
        box-shadow: 0 8px 20px rgba(161, 140, 209, 0.3);
    }

    body.theme-dark .person-photo {
        border-color: #7864ff;
        box-shadow: 0 8px 20px rgba(120, 100, 255, 0.3);
    }

    .person-name {
        font-size: 22px;
        font-weight: 700;
        margin-bottom: 8px;
        text-align: center;
        transition: all 0.6s ease;
    }

    body.theme-light .person-name {
        color: #2d2d4a;
    }

    body.theme-dark .person-name {
        color: #fff;
    }

    .person-description {
        font-size: 14px;
        line-height: 1.6;
        text-align: center;
        transition: all 0.6s ease;
    }

    body.theme-light .person-description {
        color: #5a5a7a;
    }

    body.theme-dark .person-description {
        color: rgba(255, 255, 255, 0.6);
    }

    /* Правая часть чата - фиксированная высота */
    .chat-right {
        display: flex;
        flex-direction: column;
        height: 100%;
        overflow: hidden;
    }

    .chat-header {
        font-size: 24px;
        font-weight: 700;
        margin-bottom: 16px;
        transition: all 0.6s ease;
        flex-shrink: 0;
    }

    body.theme-light .chat-header {
        background: linear-gradient(135deg, #a18cd1 0%, #ff6a88 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
    }

    body.theme-dark .chat-header {
        background: linear-gradient(135deg, #64c8ff, #7864ff);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
    }

    .message-input-wrapper {
        margin-bottom: 16px;
        flex-shrink: 0;
    }

    .message-input {
        width: 100%;
        padding: 16px 20px;
        font-size: 15px;
        font-family: inherit;
        border-radius: 12px;
        resize: vertical;
        min-height: 80px;
        transition: all 0.3s ease;
    }

    body.theme-light .message-input {
        border: 2px solid rgba(255, 255, 255, 0.6);
        background: rgba(255, 255, 255, 0.5);
        color: #2d2d4a;
    }

    body.theme-light .message-input:focus {
        outline: none;
        border-color: #a18cd1;
        background: rgba(255, 255, 255, 0.8);
        box-shadow: 0 0 0 4px rgba(161, 140, 209, 0.2);
    }

    body.theme-dark .message-input {
        border: 1px solid rgba(255, 255, 255, 0.12);
        background: rgba(255, 255, 255, 0.05);
        color: #fff;
    }

    body.theme-dark .message-input:focus {
        outline: none;
        border-color: rgba(120, 100, 255, 0.5);
        box-shadow: 0 0 20px rgba(120, 100, 255, 0.15);
    }

    .btn-send {
        width: 100%;
        padding: 16px;
        font-size: 16px;
        font-weight: 700;
        font-family: inherit;
        color: #fff;
        border: none;
        border-radius: 12px;
        cursor: pointer;
        transition: all 0.3s ease;
        margin-bottom: 16px;
        flex-shrink: 0;
    }

    body.theme-light .btn-send {
        background: linear-gradient(135deg, #ff6a88 0%, #a18cd1 100%);
        box-shadow: 0 6px 20px rgba(161, 140, 209, 0.4);
    }

    body.theme-dark .btn-send {
        background: linear-gradient(135deg, #7864ff, #64c8ff);
        box-shadow: 0 6px 20px rgba(120, 100, 255, 0.35);
    }

    .btn-send:hover {
        transform: translateY(-2px);
    }

    body.theme-light .btn-send:hover {
        box-shadow: 0 10px 30px rgba(161, 140, 209, 0.55);
    }

    body.theme-dark .btn-send:hover {
        box-shadow: 0 10px 30px rgba(120, 100, 255, 0.5);
    }

    .messages-divider {
        height: 1px;
        margin: 16px 0;
        transition: all 0.6s ease;
        flex-shrink: 0;
    }

    body.theme-light .messages-divider {
        background: linear-gradient(90deg, transparent, rgba(161, 140, 209, 0.3), transparent);
    }

    body.theme-dark .messages-divider {
        background: linear-gradient(90deg, transparent, rgba(120, 100, 255, 0.3), transparent);
    }

    /* Список сообщений - ЕДИНСТВЕННЫЙ элемент со скроллом */
    .messages-list {
        flex: 1;
        overflow-y: auto;
        overflow-x: hidden;
        margin-bottom: 16px;
        -webkit-overflow-scrolling: touch;
        scroll-behavior: smooth;
    }

    .messages-list::-webkit-scrollbar {
        width: 6px;
    }

    body.theme-light .messages-list::-webkit-scrollbar-track {
        background: rgba(255, 255, 255, 0.3);
        border-radius: 10px;
    }

    body.theme-light .messages-list::-webkit-scrollbar-thumb {
        background: rgba(161, 140, 209, 0.5);
        border-radius: 10px;
    }

    body.theme-dark .messages-list::-webkit-scrollbar-track {
        background: rgba(255, 255, 255, 0.05);
        border-radius: 10px;
    }

    body.theme-dark .messages-list::-webkit-scrollbar-thumb {
        background: rgba(120, 100, 255, 0.5);
        border-radius: 10px;
    }

    .message-item {
        padding: 16px 20px;
        margin-bottom: 12px;
        border-radius: 12px;
        transition: all 0.6s ease;
        animation: fadeInUp 0.5s ease;
    }

    body.theme-light .message-item {
        background: rgba(255, 255, 255, 0.6);
        border: 1px solid rgba(255, 255, 255, 0.8);
    }

    body.theme-dark .message-item {
        background: rgba(255, 255, 255, 0.05);
        border: 1px solid rgba(255, 255, 255, 0.1);
    }

    .message-text {
        font-size: 15px;
        line-height: 1.6;
        transition: all 0.6s ease;
    }

    body.theme-light .message-text {
        color: #2d2d4a;
    }

    body.theme-dark .message-text {
        color: rgba(255, 255, 255, 0.85);
    }

    .message-time {
        font-size: 11px;
        margin-top: 6px;
        opacity: 0.5;
        font-style: italic;
    }

    .toggle-btn {
        width: 100%;
        padding: 12px;
        font-size: 14px;
        font-weight: 600;
        font-family: inherit;
        border: 2px solid;
        border-radius: 12px;
        cursor: pointer;
        transition: all 0.3s ease;
        background: transparent;
        margin-bottom: 12px;
        flex-shrink: 0;
    }

    body.theme-light .toggle-btn {
        border-color: rgba(161, 140, 209, 0.4);
        color: #a18cd1;
    }

    body.theme-light .toggle-btn:hover {
        background: rgba(161, 140, 209, 0.1);
        border-color: #a18cd1;
    }

    body.theme-dark .toggle-btn {
        border-color: rgba(120, 100, 255, 0.4);
        color: #7864ff;
    }

    body.theme-dark .toggle-btn:hover {
        background: rgba(120, 100, 255, 0.1);
        border-color: #7864ff;
    }

    .toggle-btn.active {
        background: rgba(161, 140, 209, 0.2) !important;
        border-color: #a18cd1 !important;
    }

    body.theme-dark .toggle-btn.active {
        background: rgba(120, 100, 255, 0.2) !important;
        border-color: #7864ff !important;
    }

    .hidden-section {
        display: none;
    }

    .info-content {
        max-height: 350px;
        overflow-y: auto;
        padding-right: 10px;
        margin-bottom: 12px;
        -webkit-overflow-scrolling: touch;
    }

    .info-content::-webkit-scrollbar {
        width: 6px;
    }

    body.theme-light .info-content::-webkit-scrollbar-track {
        background: rgba(255, 255, 255, 0.3);
        border-radius: 10px;
    }

    body.theme-light .info-content::-webkit-scrollbar-thumb {
        background: rgba(161, 140, 209, 0.5);
        border-radius: 10px;
    }

    body.theme-dark .info-content::-webkit-scrollbar-track {
        background: rgba(255, 255, 255, 0.05);
        border-radius: 10px;
    }

    body.theme-dark .info-content::-webkit-scrollbar-thumb {
        background: rgba(120, 100, 255, 0.5);
        border-radius: 10px;
    }

    .loading-spinner {
        text-align: center;
        padding: 20px;
        font-size: 14px;
        opacity: 0.6;
    }

    .toast {
        position: fixed;
        bottom: 40px;
        left: 50%;
        transform: translateX(-50%) translateY(100px);
        padding: 16px 32px;
        border-radius: 50px;
        font-size: 14px;
        font-weight: 500;
        z-index: 200;
        transition: transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1), opacity 0.3s ease;
        pointer-events: none;
        max-width: 90%;
        text-align: center;
        opacity: 0;
    }

    body.theme-light .toast {
        background: rgba(255, 255, 255, 0.95);
        backdrop-filter: blur(20px);
        border: 1px solid rgba(255, 255, 255, 0.5);
        color: #2d2d4a;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
    }

    body.theme-dark .toast {
        background: rgba(30, 30, 50, 0.95);
        backdrop-filter: blur(20px);
        border: 1px solid rgba(255, 255, 255, 0.15);
        color: #fff;
    }

    .toast.show {
        transform: translateX(-50%) translateY(0);
        opacity: 1;
    }

    .toast.hide {
        transform: translateX(-50%) translateY(100px);
        opacity: 0;
    }

    /* ===== ПЛАНШЕТЫ ===== */
    @media (max-width: 1024px) {
        .chat-container {
            grid-template-columns: 280px 1fr;
            gap: 20px;
            padding: 30px;
        }

        .person-photo {
            width: 120px;
            height: 120px;
        }

        .person-name {
            font-size: 20px;
        }
    }

    /* ===== МОБИЛЬНЫЕ ===== */
    @media (max-width: 768px) {
        .header-inner {
            padding: 14px 20px;
            gap: 12px;
        }

        .burger {
            display: flex;
        }

        nav {
            position: fixed;
            top: 0;
            right: -100%;
            width: 70%;
            max-width: 300px;
            height: 100vh;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 20px;
            transition: right 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
            z-index: 99;
        }

        body.theme-light nav {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            box-shadow: -5px 0 30px rgba(0, 0, 0, 0.1);
        }

        body.theme-dark nav {
            background: rgba(10, 10, 26, 0.95);
            backdrop-filter: blur(20px);
            box-shadow: -5px 0 30px rgba(0, 0, 0, 0.3);
        }

        nav.active {
            right: 0;
        }

        nav a {
            font-size: 16px;
            padding: 14px 30px;
            width: 80%;
            text-align: center;
        }

        .theme-toggle {
            width: 54px;
            height: 30px;
        }

        .theme-toggle .toggle-knob {
            width: 24px;
            height: 24px;
            font-size: 14px;
        }

        body.theme-dark .toggle-knob {
            left: 27px;
        }

        .description {
            padding: 100px 20px 60px;
        }

        .description-inner {
            padding: 40px 25px !important;
        }

        .description h2 {
            font-size: 36px;
            margin-bottom: 20px;
        }

        .description p {
            font-size: 17px;
        }

        .btn-start {
            padding: 16px 40px;
            font-size: 16px;
        }

        .department-selector {
            padding: 40px 20px 60px;
        }

        .selector-card {
            padding: 40px 25px !important;
        }

        .department-selector h2 {
            font-size: 30px;
        }

        .department-selector .subtitle {
            font-size: 15px;
            margin-bottom: 30px;
        }

        select {
            padding: 18px 20px;
            font-size: 16px;
        }

        .btn-continue {
            padding: 18px;
            font-size: 16px;
        }

        .btn-back {
            padding: 14px;
            font-size: 14px;
        }

        .wave-divider {
            height: 80px;
        }

        /* Мобильный чат */
        .chat-section {
            padding: 90px 16px 40px;
            min-height: 100vh;
            max-width: 100%;
        }

        .chat-container {
            grid-template-columns: 1fr;
            padding: 20px;
            gap: 16px;
            height: calc(100vh - 130px);
            display: flex;
            flex-direction: column;
        }

        .person-card {
            flex-shrink: 0;
            padding: 20px;
        }

        .person-photo {
            width: 100px;
            height: 100px;
            margin-bottom: 16px;
        }

        .person-name {
            font-size: 18px;
        }

        .person-description {
            font-size: 14px;
        }

        .chat-right {
            flex: 1;
            min-height: 0;
        }

        .chat-header {
            font-size: 20px;
        }

        .message-input {
            padding: 14px 16px;
            font-size: 16px;
            min-height: 80px;
        }

        .btn-send {
            padding: 14px;
            font-size: 15px;
        }

        .messages-list {
            flex: 1;
            min-height: 0;
        }

        .message-item {
            padding: 14px 16px;
        }

        .message-text {
            font-size: 14px;
        }

        .info-content {
            max-height: 250px;
        }

        .toast {
            bottom: auto;
            top: 80px;
            padding: 12px 20px;
            font-size: 13px;
            max-width: 85%;
        }
        
        .toast.show {
            transform: translateX(-50%) translateY(0);
        }
        
        .toast.hide {
            transform: translateX(-50%) translateY(-100px);
        }
    }

    /* ===== ОЧЕНЬ МАЛЕНЬКИЕ ЭКРАНЫ ===== */
    @media (max-width: 480px) {
        .header-inner {
            padding: 12px 16px;
        }

        nav {
            width: 85%;
        }

        nav a {
            font-size: 15px;
            padding: 12px 24px;
        }

        .description h2 {
            font-size: 32px;
        }

        .description p {
            font-size: 16px;
        }

        .btn-start {
            padding: 14px 36px;
            font-size: 15px;
        }

        .selector-card {
            padding: 30px 20px !important;
        }

        .department-selector h2 {
            font-size: 26px;
        }

        .chat-container {
            padding: 16px;
        }

        .person-card {
            padding: 16px;
        }

        .person-photo {
            width: 90px;
            height: 90px;
        }

        .person-name {
            font-size: 17px;
        }

        .chat-header {
            font-size: 18px;
        }

        .info-content {
            max-height: 200px;
        }
    }

    @keyframes fadeInUp {
        from { opacity: 0; transform: translateY(30px); }
        to { opacity: 1; transform: translateY(0); }
    }
</style>
</head>
<body class="theme-light locked">

    <div class="bg-light"></div>
    <div class="blob blob1"></div>
    <div class="blob blob2"></div>
    <div class="blob blob3"></div>
    <div class="blob blob4"></div>
    <div class="bubbles" id="bubbles"></div>

    <div class="glow-orb orb1"></div>
    <div class="glow-orb orb2"></div>
    <div class="glow-orb orb3"></div>
    <canvas id="starCanvas"></canvas>

    <header>
        <div class="header-inner">
            <div class="burger" id="burger">
                <span></span>
                <span></span>
                <span></span>
            </div>
            <nav id="nav">
                <a id="navStart" class="tab1">Начать</a>
                <a class="tab2 fake-tab" data-msg="Скоро будет доступно ✨">Вкладка 2</a>
                <a class="tab3 fake-tab" data-msg="Раздел в разработке 🚀">Вкладка 3</a>
            </nav>
            <div class="theme-toggle" id="themeToggle" title="Переключить тему">
                <div class="toggle-knob" id="toggleKnob">☀️</div>
            </div>
        </div>
    </header>

    <div class="description">
        <div class="description-inner">
            <h2>Добро пожаловать</h2>
            <p>Здесь будет ваш текст с описанием страницы. Можете добавить его позже или попросить меня сгенерировать. Этот блок занимает весь экран и находится сразу после шапки.</p>
            <button class="btn-start" id="startBtn">Начать →</button>
            <div class="scroll-hint">
                <span>Листайте вниз</span>
                <div class="scroll-arrow"></div>
            </div>
        </div>
    </div>

    <div class="wave-divider">
        <svg viewBox="0 0 1440 150" preserveAspectRatio="none">
            <path class="wave-path-1" d="M0,80 C360,150 720,0 1080,80 C1260,120 1380,100 1440,80 L1440,150 L0,150 Z"/>
            <path class="wave-path-2" d="M0,100 C360,40 720,140 1080,80 C1260,50 1380,90 1440,100 L1440,150 L0,150 Z"/>
        </svg>
    </div>

    <div class="department-selector" id="department-selector">
        <div class="selector-card">
            <h2>Выберите подразделение</h2>
            <p class="subtitle">После выбора вы перейдёте на соответствующую страницу</p>

            <div class="select-wrapper">
                <select id="departmentSelect">
                    <option value="">— Выберите подразделение —</option>
                    <option value="dept1">Подразделение 1</option>
                    <option value="dept2">Подразделение 2</option>
                    <option value="dept3">Подразделение 3</option>
                    <option value="dept4">Подразделение 4</option>
                </select>
            </div>

            <button class="btn-continue" id="continueBtn" disabled>Продолжить</button>
            <button class="btn-back" id="backBtn">← Вернуться назад</button>
        </div>
    </div>

    <div class="chat-section" id="chatSection1">
        <div class="chat-container">
            <div class="person-card">
                <img src="https://i.pinimg.com/736x/89/64/c4/8964c4609918bc8d43f288ad572d1e55.jpg" alt="Фото" class="person-photo">
                <div class="person-name">Левтер Татьяна Владимировна</div>
            </div>

            <div class="chat-right">
                <button class="toggle-btn" id="toggleInfo1">👤 Показать информацию о председателе</button>
                <div class="hidden-section" id="infoSection1">
                    <div class="person-card" style="margin-bottom: 16px;">
                        <div class="info-content">
                            <div class="person-description">Председатель профсоюза. Более 15 лет защищает права работников. Активно участвует в решении трудовых споров и улучшении условий труда. Имеет высшее юридическое образование. Награждён почётными грамотами за многолетний добросовестный труд.</div>
                        </div>
                    </div>
                    <button class="toggle-btn active" onclick="document.getElementById('infoSection1').classList.add('hidden-section'); document.getElementById('toggleInfo1').classList.remove('active'); document.getElementById('toggleInfo1').textContent='👤 Показать информацию о председателе'">Скрыть информацию</button>
                </div>

                <div class="chat-header">Пожелания и благодарности</div>

                <button class="toggle-btn" id="toggleChat1">💬 Открыть чат</button>
                <div class="hidden-section" id="chatInputSection1">
                    <div class="message-input-wrapper">
                        <textarea class="message-input" id="messageInput1" placeholder="Напишите ваше пожелание или благодарность..."></textarea>
                    </div>
                    <button class="btn-send" data-chat="1">Отправить сообщение</button>
                    <button class="toggle-btn active" onclick="document.getElementById('chatInputSection1').classList.add('hidden-section'); document.getElementById('toggleChat1').classList.remove('active'); document.getElementById('toggleChat1').textContent='💬 Открыть чат'">Скрыть чат</button>
                </div>

                <div class="messages-divider"></div>

                <div class="messages-list" id="messagesList1">
                    <div class="loading-spinner">Загрузка сообщений...</div>
                </div>

                <button class="btn-back back-from-chat" data-target="dept-selector">← Вернуться к выбору подразделения</button>
            </div>
        </div>
    </div>

    <div class="chat-section" id="chatSection2">
        <div class="chat-container">
            <div class="person-card">
                <img src="123.png" alt="Фото" class="person-photo">
                <div class="person-name">Петрова Анна Сергеевна</div>
            </div>

            <div class="chat-right">
                <button class="toggle-btn" id="toggleInfo2">👤 Показать информацию о председателе</button>
                <div class="hidden-section" id="infoSection2">
                    <div class="person-card" style="margin-bottom: 16px;">
                        <div class="info-content">
                            <div class="person-description">Заместитель председателя профсоюза. Курирует вопросы охраны труда и социальной защиты работников. Организует культурно-массовые мероприятия. Стаж работы в профсоюзе - 10 лет. Имеет благодарность от Федерации независимых профсоюзов России.</div>
                        </div>
                    </div>
                    <button class="toggle-btn active" onclick="document.getElementById('infoSection2').classList.add('hidden-section'); document.getElementById('toggleInfo2').classList.remove('active'); document.getElementById('toggleInfo2').textContent='👤 Показать информацию о председателе'">Скрыть информацию</button>
                </div>

                <div class="chat-header">Пожелания и благодарности</div>

                <button class="toggle-btn" id="toggleChat2">💬 Открыть чат</button>
                <div class="hidden-section" id="chatInputSection2">
                    <div class="message-input-wrapper">
                        <textarea class="message-input" id="messageInput2" placeholder="Напишите ваше пожелание или благодарность..."></textarea>
                    </div>
                    <button class="btn-send" data-chat="2">Отправить сообщение</button>
                    <button class="toggle-btn active" onclick="document.getElementById('chatInputSection2').classList.add('hidden-section'); document.getElementById('toggleChat2').classList.remove('active'); document.getElementById('toggleChat2').textContent='💬 Открыть чат'">Скрыть чат</button>
                </div>

                <div class="messages-divider"></div>

                <div class="messages-list" id="messagesList2">
                    <div class="loading-spinner">Загрузка сообщений...</div>
                </div>

                <button class="btn-back back-from-chat" data-target="dept-selector">← Вернуться к выбору подразделения</button>
            </div>
        </div>
    </div>

    <div class="toast" id="toast"></div>

    <script>
        // ===== FIREBASE КОНФИГУРАЦИЯ =====
        const firebaseConfig = {
  apiKey: "AIzaSyB5PeNgXrAPyW8qnfEvPSnB4qa_GCE1Dfo",
  authDomain: "wiki-profcom.firebaseapp.com",
  projectId: "wiki-profcom",
  storageBucket: "wiki-profcom.firebasestorage.app",
  messagingSenderId: "789513290691",
  appId: "1:789513290691:web:40e2ebc7b3611ac9b83376",
  measurementId: "G-5R4SZ4B19R"
};

        firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();

    // ===== УТИЛИТЫ =====
    function escapeHtml(text) {
        const div = document.createElement('div');
        div.textContent = text;
        return div.innerHTML;
    }

    function formatTimestamp(ts) {
        if (!ts) return '';
        const date = ts.toDate ? ts.toDate() : new Date(ts);
        const now = new Date();
        const diff = now - date;
        const minutes = Math.floor(diff / 60000);
        const hours = Math.floor(diff / 3600000);
        const days = Math.floor(diff / 86400000);

        if (minutes < 1) return 'только что';
        if (minutes < 60) return `${minutes} мин. назад`;
        if (hours < 24) return `${hours} ч. назад`;
        if (days < 7) return `${days} дн. назад`;
        return date.toLocaleDateString('ru-RU');
    }

    // ===== ЗАГРУЗКА СООБЩЕНИЙ ИЗ FIRESTORE =====
    function loadMessages(chatNum) {
        const list = document.getElementById('messagesList' + chatNum);
        const collectionName = 'messages' + chatNum;

        list.innerHTML = '<div class="loading-spinner">Загрузка сообщений...</div>';

        db.collection(collectionName)
            .orderBy('timestamp', 'desc')
            .onSnapshot((snapshot) => {
                list.innerHTML = '';

                if (snapshot.empty) {
                    list.innerHTML = '<div class="loading-spinner">Пока нет сообщений. Будьте первым!</div>';
                    return;
                }

                snapshot.forEach(doc => {
                    const data = doc.data();
                    const item = document.createElement('div');
                    item.className = 'message-item';

                    const timeStr = data.timestamp ? formatTimestamp(data.timestamp) : '';
                    const timeHtml = timeStr ? `<div class="message-time">${timeStr}</div>` : '';

                    item.innerHTML = `
                        <div class="message-text">${escapeHtml(data.text || '')}</div>
                        ${timeHtml}
                    `;
                    list.appendChild(item);
                });
            }, (error) => {
                console.error('Ошибка загрузки сообщений:', error);
                list.innerHTML = '<div class="loading-spinner">⚠️ Ошибка загрузки. Проверьте консоль (F12)</div>';
            });
        }

    // ===== ОТПРАВКА СООБЩЕНИЯ В FIRESTORE =====
    async function sendMessageToFirebase(chatNum, text) {
        const collectionName = 'messages' + chatNum;
        try {
            await db.collection(collectionName).add({
                text: text,
                timestamp: firebase.firestore.FieldValue.serverTimestamp()
            });
            return { success: true };
        } catch (error) {
            console.error('Ошибка отправки:', error);
            return { success: false, error: error.message };
        }
    }

    // ===== ЗАГРУЖАЕМ СООБЩЕНИЯ ПРИ СТАРТЕ =====
    loadMessages(1);
    loadMessages(2);

    // ===== ОСТАЛЬНАЯ ЛОГИКА САЙТА =====
    const burger = document.getElementById('burger');
    const nav = document.getElementById('nav');

    burger.addEventListener('click', () => {
        burger.classList.toggle('active');
        nav.classList.toggle('active');
    });

    nav.querySelectorAll('a').forEach(link => {
        link.addEventListener('click', () => {
            burger.classList.remove('active');
            nav.classList.remove('active');
        });
    });

    const body = document.body;
    const themeToggle = document.getElementById('themeToggle');
    const toggleKnob = document.getElementById('toggleKnob');

    const savedTheme = localStorage.getItem('theme') || 'light';
    setTheme(savedTheme);

    themeToggle.addEventListener('click', () => {
        const newTheme = body.classList.contains('theme-light') ? 'dark' : 'light';
        setTheme(newTheme);
        localStorage.setItem('theme', newTheme);
    });

    function setTheme(theme) {
        body.classList.remove('theme-light', 'theme-dark');
        body.classList.add('theme-' + theme);
        toggleKnob.textContent = theme === 'light' ? '☀️' : '🌙';
        updateWaveColors(theme);
    }

    function updateWaveColors(theme) {
        const path1 = document.querySelector('.wave-path-1');
        const path2 = document.querySelector('.wave-path-2');
        if (theme === 'light') {
            path1.setAttribute('fill', 'rgba(255, 255, 255, 0.3)');
            path2.setAttribute('fill', 'rgba(255, 255, 255, 0.5)');
        } else {
            path1.setAttribute('fill', 'rgba(120, 100, 255, 0.1)');
            path2.setAttribute('fill', 'rgba(100, 200, 255, 0.15)');
        }
    }
    updateWaveColors(savedTheme);

    const startBtn = document.getElementById('startBtn');
    const backBtn = document.getElementById('backBtn');
    const continueBtn = document.getElementById('continueBtn');
    const departmentSelect = document.getElementById('departmentSelect');
    const chatSection1 = document.getElementById('chatSection1');
    const chatSection2 = document.getElementById('chatSection2');

    function lockScroll() {
        body.classList.add('locked');
    }

    function unlockScroll() {
        body.classList.remove('locked');
    }

    function hideAllChats() {
        chatSection1.classList.remove('visible');
        chatSection2.classList.remove('visible');
    }

    // Кнопка "Начать" в навигации
    document.getElementById('navStart').addEventListener('click', (e) => {
        e.preventDefault();
        unlockScroll();
        setTimeout(() => {
            document.getElementById('department-selector').scrollIntoView({ behavior: 'smooth' });
            setTimeout(() => {
                lockScroll();
            }, 800);
        }, 100);
    });

    // Кнопка "Начать" на главной
    startBtn.addEventListener('click', () => {
        unlockScroll();
        setTimeout(() => {
            document.getElementById('department-selector').scrollIntoView({ behavior: 'smooth' });
            setTimeout(() => {
                lockScroll();
            }, 800);
        }, 100);
    });

    // Кнопка "Вернуться" из выбора подразделения
    backBtn.addEventListener('click', () => {
        unlockScroll();
        hideAllChats();
        setTimeout(() => {
            document.querySelector('.description').scrollIntoView({ behavior: 'smooth' });
            setTimeout(() => {
                lockScroll();
            }, 800);
        }, 100);
    });

    // Кнопки "Вернуться" из чатов
    document.querySelectorAll('.back-from-chat').forEach(btn => {
        btn.addEventListener('click', () => {
            unlockScroll();
            hideAllChats();
            setTimeout(() => {
                document.getElementById('department-selector').scrollIntoView({ behavior: 'smooth' });
                setTimeout(() => {
                    lockScroll();
                }, 800);
            }, 100);
        });
    });

    departmentSelect.addEventListener('change', () => {
        continueBtn.disabled = !departmentSelect.value;
    });

    // Кнопка "Продолжить" - переход к чату
    continueBtn.addEventListener('click', () => {
        if (!departmentSelect.value) return;

        unlockScroll();
        hideAllChats();

        if (departmentSelect.value === 'dept1') {
            chatSection1.classList.add('visible');
        } else if (departmentSelect.value === 'dept2') {
            chatSection2.classList.add('visible');
        } else {
            alert('Переход на страницу: ' + departmentSelect.value + '\n(ссылку настроим на следующем этапе)');
            lockScroll();
            return;
        }

        setTimeout(() => {
            const activeChat = departmentSelect.value === 'dept1' ? chatSection1 : chatSection2;
            activeChat.scrollIntoView({ behavior: 'smooth' });
            // Блокируем скролл страницы, но чат внутри прокручивается
            setTimeout(() => {
                lockScroll();
            }, 800);
        }, 100);
    });

    // ===== ОТПРАВКА СООБЩЕНИЙ =====
    document.querySelectorAll('.btn-send').forEach(btn => {
        btn.addEventListener('click', async () => {
            const chatNum = btn.dataset.chat;
            const input = document.getElementById('messageInput' + chatNum);
            const text = input.value.trim();

            if (!text) {
                showToast('Пожалуйста, введите сообщение');
                return;
            }

            const originalText = btn.textContent;
            btn.disabled = true;
            btn.textContent = 'Отправка...';

            const result = await sendMessageToFirebase(chatNum, text);

            if (result.success) {
                input.value = '';
                setTimeout(() => {
                    showToast('✅ Сообщение отправлено!');
                }, 300);
            } else {
                showToast('❌ Ошибка отправки');
            }

            btn.disabled = false;
            btn.textContent = originalText;
        });
    });

    // ===== TOGGLE КНОПКИ =====
    document.getElementById('toggleInfo1').addEventListener('click', function() {
        document.getElementById('infoSection1').classList.toggle('hidden-section');
        this.classList.toggle('active');
        this.textContent = this.classList.contains('active') ? '👤 Скрыть информацию' : '👤 Показать информацию о председателе';
    });

    document.getElementById('toggleChat1').addEventListener('click', function() {
        document.getElementById('chatInputSection1').classList.toggle('hidden-section');
        this.classList.toggle('active');
        this.textContent = this.classList.contains('active') ? '💬 Скрыть чат' : '💬 Открыть чат';
    });

    document.getElementById('toggleInfo2').addEventListener('click', function() {
        document.getElementById('infoSection2').classList.toggle('hidden-section');
        this.classList.toggle('active');
        this.textContent = this.classList.contains('active') ? '👤 Скрыть информацию' : '👤 Показать информацию о председателе';
    });

    document.getElementById('toggleChat2').addEventListener('click', function() {
        document.getElementById('chatInputSection2').classList.toggle('hidden-section');
        this.classList.toggle('active');
        this.textContent = this.classList.contains('active') ? '💬 Скрыть чат' : '💬 Открыть чат';
    });

    // ===== ПУЗЫРЬКИ =====
    function createBubbles() {
        const container = document.getElementById('bubbles');
        const count = 25;
        for (let i = 0; i < count; i++) {
            const bubble = document.createElement('div');
            bubble.className = 'bubble';
            const size = Math.random() * 40 + 10;
            bubble.style.width = size + 'px';
            bubble.style.height = size + 'px';
            bubble.style.left = Math.random() * 100 + '%';
            bubble.style.animationDuration = (Math.random() * 10 + 8) + 's';
            bubble.style.animationDelay = Math.random() * 10 + 's';
            container.appendChild(bubble);
        }
    }
    createBubbles();

    // ===== ЗВЁЗДЫ =====
    const canvas = document.getElementById('starCanvas');
    const ctx = canvas.getContext('2d');

    function resize() {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
    }
    resize();
    window.addEventListener('resize', resize);

    class Star {
        constructor() { this.reset(); }
        reset() {
            this.x = Math.random() * canvas.width;
            this.y = Math.random() * canvas.height - canvas.height;
            this.size = Math.random() * 2 + 0.5;
            this.speed = Math.random() * 1.5 + 0.3;
            this.opacity = Math.random() * 0.8 + 0.2;
            this.twinkleSpeed = Math.random() * 0.02 + 0.005;
            this.twinklePhase = Math.random() * Math.PI * 2;
            const colors = [[120,100,255],[100,200,255],[255,130,200],[255,255,255]];
            this.color = colors[Math.floor(Math.random() * colors.length)];
        }
        update() {
            this.y += this.speed;
            this.twinklePhase += this.twinkleSpeed;
            this.currentOpacity = this.opacity * (0.5 + 0.5 * Math.sin(this.twinklePhase));
            if (this.y > canvas.height + 10) { this.reset(); this.y = -10; }
        }
        draw() {
            const [r, g, b] = this.color;
            ctx.beginPath();
            ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
            ctx.fillStyle = `rgba(${r}, ${g}, ${b}, ${this.currentOpacity})`;
            ctx.fill();
            if (this.size > 1) {
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size * 3, 0, Math.PI * 2);
                ctx.fillStyle = `rgba(${r}, ${g}, ${b}, ${this.currentOpacity * 0.15})`;
                ctx.fill();
            }
        }
    }

    class ShootingStar {
        constructor() { this.reset(); }
        reset() {
            this.x = Math.random() * canvas.width;
            this.y = Math.random() * canvas.height * 0.5;
            this.length = Math.random() * 80 + 40;
            this.speed = Math.random() * 8 + 6;
            this.angle = Math.PI / 4 + (Math.random() - 0.5) * 0.3;
            this.opacity = 1;
            this.life = 0;
            this.maxLife = Math.random() * 40 + 20;
            this.active = false;
        }
        activate() { this.reset(); this.active = true; }
        update() {
            if (!this.active) return;
            this.x += Math.cos(this.angle) * this.speed;
            this.y += Math.sin(this.angle) * this.speed;
            this.life++;
            this.opacity = 1 - this.life / this.maxLife;
            if (this.life >= this.maxLife) this.active = false;
        }
        draw() {
            if (!this.active) return;
            const tailX = this.x - Math.cos(this.angle) * this.length;
            const tailY = this.y - Math.sin(this.angle) * this.length;
            const gradient = ctx.createLinearGradient(tailX, tailY, this.x, this.y);
            gradient.addColorStop(0, `rgba(255, 255, 255, 0)`);
            gradient.addColorStop(1, `rgba(255, 255, 255, ${this.opacity})`);
            ctx.beginPath();
            ctx.moveTo(tailX, tailY);
            ctx.lineTo(this.x, this.y);
            ctx.strokeStyle = gradient;
            ctx.lineWidth = 1.5;
            ctx.stroke();
            ctx.beginPath();
            ctx.arc(this.x, this.y, 2, 0, Math.PI * 2);
            ctx.fillStyle = `rgba(255, 255, 255, ${this.opacity})`;
            ctx.fill();
        }
    }

    const stars = Array.from({ length: 150 }, () => new Star());
    const shootingStars = Array.from({ length: 3 }, () => new ShootingStar());
    let shootingTimer = 0;

    function animate() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        stars.forEach(s => { s.update(); s.draw(); });
        shootingTimer++;
        if (shootingTimer > 120 + Math.random() * 200) {
            const inactive = shootingStars.find(s => !s.active);
            if (inactive) inactive.activate();
            shootingTimer = 0;
        }
        shootingStars.forEach(s => { s.update(); s.draw(); });
        requestAnimationFrame(animate);
    }
    animate();

    // ===== TOAST УВЕДОМЛЕНИЯ =====
    const toast = document.getElementById('toast');
    let toastTimeout = null;

    function showToast(message) {
        if (toastTimeout) {
            clearTimeout(toastTimeout);
            toastTimeout = null;
        }
        
        toast.textContent = message;
        toast.classList.remove('hide');
        toast.classList.add('show');
        
        const duration = window.innerWidth <= 768 ? 2000 : 3000;
        
        toastTimeout = setTimeout(() => {
            toast.classList.remove('show');
            toast.classList.add('hide');
            
            setTimeout(() => {
                if (!toast.classList.contains('show')) {
                    toast.classList.remove('hide');
                }
            }, 500);
        }, duration);
    }

    document.querySelectorAll('.fake-tab').forEach(tab => {
        tab.addEventListener('click', (e) => {
            e.preventDefault();
            showToast(tab.dataset.msg);
        });
    });
</script>

</body>
</html>
