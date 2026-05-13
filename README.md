<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>White VPN · Список конфигураций</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #f2f4f8;
            font-family: system-ui, -apple-system, 'Segoe UI', 'Roboto', 'Helvetica Neue', sans-serif;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        /* ---------- АНИМАЦИОННЫЙ ЭКРАН (SPLASH) ---------- */
        .splash-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #0b1b2a;   /* тёмный спокойный фон */
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            transition: opacity 0.6s ease-in-out, visibility 0.6s ease-in-out;
            backdrop-filter: blur(0px);
        }

        .splash-content {
            text-align: center;
            color: white;
            transform: translateY(0);
            animation: fadeSlideUp 0.7s cubic-bezier(0.2, 0.9, 0.4, 1.1) forwards;
        }

        .splash-title {
            font-size: 3.2rem;
            font-weight: 700;
            letter-spacing: -0.5px;
            margin-bottom: 0.75rem;
            background: linear-gradient(135deg, #FFFFFF, #b9e2ff);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            text-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .splash-welcome {
            font-size: 1.35rem;
            font-weight: 400;
            opacity: 0.9;
            border-top: 1px solid rgba(255,255,255,0.25);
            display: inline-block;
            padding-top: 0.7rem;
            margin-top: 0.3rem;
        }

        .splash-dot {
            display: inline-block;
            width: 8px;
            height: 8px;
            background: #7bc5ff;
            border-radius: 50%;
            margin: 0 4px;
            animation: pulse 1.4s infinite;
        }

        @keyframes fadeSlideUp {
            0% {
                opacity: 0;
                transform: translateY(18px);
            }
            100% {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes pulse {
            0%, 100% { opacity: 0.4; transform: scale(1);}
            50% { opacity: 1; transform: scale(1.3);}
        }

        /* скрытый splash */
        .splash-screen.hide {
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
        }

        /* ---------- ОСНОВНОЙ КОНТЕНТ (появляется после анимации) ---------- */
        .main-app {
            width: 100%;
            max-width: 1100px;
            margin: 20px auto;
            padding: 16px 20px 30px;
            opacity: 0;
            transition: opacity 0.5s ease;
        }

        .main-app.visible {
            opacity: 1;
        }

        /* простой дизайн: карточка, минимум теней */
        .card {
            background: #ffffff;
            border-radius: 28px;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.03), 0 2px 6px rgba(0, 0, 0, 0.05);
            overflow: hidden;
            border: 1px solid #eef2f5;
        }

        /* заголовок */
        .simple-header {
            background: #fefefe;
            padding: 1.5rem 2rem 0.8rem 2rem;
            border-bottom: 1px solid #eef2f8;
        }

        .simple-header h2 {
            font-size: 1.6rem;
            font-weight: 600;
            color: #1f3b4c;
            margin: 0;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .simple-header p {
            color: #5c6f87;
            margin-top: 6px;
            font-size: 0.85rem;
        }

        /* панель кнопок */
        .action-bar {
            padding: 1rem 2rem;
            background: #fafcff;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            gap: 15px;
            border-bottom: 1px solid #eef2f5;
        }

        .buttons {
            display: flex;
            gap: 12px;
            flex-wrap: wrap;
        }

        button {
            border: none;
            background: #ffffff;
            border: 1px solid #dce3ec;
            padding: 8px 20px;
            border-radius: 50px;
            font-weight: 500;
            font-size: 0.9rem;
            cursor: pointer;
            transition: 0.2s;
            font-family: inherit;
            display: inline-flex;
            align-items: center;
            gap: 6px;
            background: white;
            color: #2c4b6e;
        }

        button.primary-btn {
            background: #1e4663;
            border-color: #1e4663;
            color: white;
        }

        button.primary-btn:hover {
            background: #0f3550;
            transform: translateY(-1px);
        }

        button.secondary-btn {
            background: #f2f5f9;
            border-color: #cdd9ed;
        }

        button.secondary-btn:hover {
            background: #e9eef5;
        }

        button:active {
            transform: translateY(1px);
        }

        .status-badge {
            font-size: 0.8rem;
            background: #edf2f9;
            padding: 6px 14px;
            border-radius: 40px;
            color: #2a577b;
        }

        .status-badge.error {
            background: #ffe6e5;
            color: #bc4e4e;
        }

        .status-badge.success {
            background: #e0f2e9;
            color: #1a6840;
        }

        /* текстовое поле */
        .preview-area {
            padding: 1.2rem 2rem 2rem 2rem;
        }

        .info-line {
            display: flex;
            justify-content: space-between;
            margin-bottom: 12px;
            font-size: 0.7rem;
            text-transform: uppercase;
            font-weight: 600;
            color: #5b7a99;
            letter-spacing: 0.4px;
        }

        .counter {
            background: #eef2fa;
            padding: 2px 12px;
            border-radius: 30px;
            font-family: monospace;
        }

        textarea {
            width: 100%;
            border: 1px solid #e2e8f0;
            border-radius: 20px;
            padding: 16px 18px;
            font-family: 'SF Mono', 'Fira Code', monospace;
            font-size: 13px;
            line-height: 1.45;
            background: #fefefe;
            resize: vertical;
            outline: none;
            color: #1e2f3a;
        }

        textarea:focus {
            border-color: #bdd4ec;
            background: #ffffff;
        }

        .footer-meta {
            margin-top: 18px;
            text-align: center;
            font-size: 0.7rem;
            color: #7b8ba3;
            border-top: 1px solid #ecf0f5;
            padding-top: 18px;
        }

        code {
            background: #f0f3f8;
            padding: 2px 6px;
            border-radius: 12px;
            font-size: 0.7rem;
        }

        @media (max-width: 640px) {
            .simple-header h2 { font-size: 1.3rem; }
            .action-bar { flex-direction: column; align-items: stretch; }
            .buttons { justify-content: center; }
            .status-badge { text-align: center; }
            .splash-title { font-size: 2.2rem; }
        }
    </style>
</head>
<body>

<!-- АНИМАЦИОННЫЙ ЭКРАН ПРИВЕТСТВИЯ -->
<div class="splash-screen" id="splashScreen">
    <div class="splash-content">
        <div class="splash-title">WHITE VPN</div>
        <div class="splash-welcome">
            Добро пожаловать
            <span class="splash-dot"></span><span class="splash-dot"></span>
        </div>
    </div>
</div>

<!-- ОСНОВНОЕ ПРИЛОЖЕНИЕ (изначально скрыто) -->
<div class="main-app" id="mainApp">
    <div class="card">
        <div class="simple-header">
            <h2>📋 Белые CIDR / списки</h2>
            <p>Конфигурации из репозитория igareck/vpn-configs-for-russia</p>
        </div>
        <div class="action-bar">
            <div class="buttons">
                <button id="fetchBtn" class="primary-btn">
                    🌐 Загрузить список
                </button>
                <button id="copyBtn" class="secondary-btn" disabled>
                    📋 Копировать всё
                </button>
            </div>
            <div id="statusMsg" class="status-badge">
                Готово
            </div>
        </div>
        <div class="preview-area">
            <div class="info-line">
                <span>📄 Содержимое WHITE-CIDR-RU-checked.txt</span>
                <span class="counter" id="lineCounter">— элементов</span>
            </div>
            <textarea id="dataTextarea" rows="12" readonly placeholder="Нажмите «Загрузить список»&#10;Данные будут взяты с GitHub (raw.githubusercontent.com)"></textarea>
            <div class="footer-meta">
                🔗 Источник: 
                <code>raw.githubusercontent.com/igareck/vpn-configs-for-russia/main/WHITE-CIDR-RU-checked.txt</code>
                <br>✅ Кнопка копирует все строки (CIDR/ссылки) в буфер обмена
            </div>
        </div>
    </div>
</div>

<script>
    (function() {
        // ----- Элементы -----
        const splash = document.getElementById('splashScreen');
        const mainApp = document.getElementById('mainApp');
        
        // ----- Логика анимации: показываем приветствие 1.5 сек, потом исчезает и появляется контент -----
        // Длительность анимации + задержка, чтобы юзер успел прочитать
        setTimeout(() => {
            if (splash) {
                splash.classList.add('hide');   // плавное исчезновение
                // После окончания анимации удаляем сплеш из DOM (необязательно, но аккуратно)
                setTimeout(() => {
                    if (splash && splash.parentNode) {
                        splash.style.display = 'none';
                    }
                }, 600);
            }
            // Показываем основной блок с плавным появлением
            mainApp.classList.add('visible');
        }, 1500);  // 1.5 секунды приветствия — просто и элегантно
        
        // ----- ОСНОВНАЯ ЛОГИКА ЗАГРУЗКИ / КОПИРОВАНИЯ (как просили) -----
        const fetchBtn = document.getElementById('fetchBtn');
        const copyBtn = document.getElementById('copyBtn');
        const textarea = document.getElementById('dataTextarea');
        const statusDiv = document.getElementById('statusMsg');
        const lineCounterSpan = document.getElementById('lineCounter');
        
        let currentRawContent = "";     // храним последний загруженный текст
        let isLoading = false;
        
        // вспомогательные функции UI
        function setStatus(text, isError = false, isSuccess = false) {
            statusDiv.innerHTML = text;
            statusDiv.classList.remove('error', 'success');
            if (isError) {
                statusDiv.classList.add('error');
            } else if (isSuccess) {
                statusDiv.classList.add('success');
            }
        }
        
        function updateLineCounter(content) {
            if (!content || content.trim() === "") {
                lineCounterSpan.innerText = "0 строк";
                return;
            }
            const lines = content.split(/\r?\n/);
            const nonEmpty = lines.filter(l => l.trim().length > 0).length;
            lineCounterSpan.innerText = `${nonEmpty} строк (всего ${lines.length})`;
        }
        
        function setTextareaContent(content) {
            textarea.value = content;
            currentRawContent = content;
            updateLineCounter(content);
            if (content && content.trim().length > 0) {
                copyBtn.disabled = false;
            } else {
                copyBtn.disabled = true;
            }
        }
        
        function setFetchLoading(loading) {
            isLoading = loading;
            if (loading) {
                fetchBtn.disabled = true;
                fetchBtn.innerHTML = `⏳ Загрузка...`;
            } else {
                fetchBtn.disabled = false;
                fetchBtn.innerHTML = `🌐 Загрузить список`;
            }
        }
        
        // Функция загрузки с github raw
        async function loadWhiteList() {
            if (isLoading) return;
            const url = "https://raw.githubusercontent.com/igareck/vpn-configs-for-russia/main/WHITE-CIDR-RU-checked.txt";
            setFetchLoading(true);
            setStatus("⏳ Загрузка данных...", false, false);
            
            try {
                const controller = new AbortController();
                const timeoutId = setTimeout(() => controller.abort(), 12000);
                const response = await fetch(url, { signal: controller.signal, cache: "no-store" });
                clearTimeout(timeoutId);
                
                if (!response.ok) throw new Error(`Ошибка ${response.status}`);
                const rawText = await response.text();
                
                if (rawText === undefined || rawText === null) throw new Error("Пустой ответ");
                setTextareaContent(rawText);
                const kb = (rawText.length / 1024).toFixed(1);
                setStatus(`✅ Загружено (${kb} KB)`, false, true);
                
                // если в тексте странный html - предупреждение
                if (rawText.trim().startsWith("<") && rawText.includes("<!DOCTYPE")) {
                    setStatus("⚠️ Загружен странный формат, но данные скопированы как есть", true, false);
                }
            } catch (err) {
                console.error(err);
                let errorMsg = "";
                if (err.name === "AbortError") errorMsg = "❌ Таймаут, проверьте интернет";
                else if (err.message.includes("fetch")) errorMsg = "❌ Сетевая ошибка / CORS?";
                else errorMsg = `❌ Ошибка: ${err.message}`;
                setStatus(errorMsg, true, false);
                if (!currentRawContent) {
                    setTextareaContent("// Не удалось загрузить список. Нажмите еще раз.\n// Проверьте доступ к raw.githubusercontent.com");
                } else {
                    // оставляем старый контент, но кнопка копирования активна если есть контент
                    if (!currentRawContent || currentRawContent.trim() === "") copyBtn.disabled = true;
                    else copyBtn.disabled = false;
                }
            } finally {
                setFetchLoading(false);
            }
        }
        
        // Копирование в буфер
        async function copyAllText() {
            let contentToCopy = textarea.value;
            if (!contentToCopy || contentToCopy.trim() === "") {
                setStatus("📭 Нет данных — сначала загрузите список", true, false);
                return;
            }
            try {
                await navigator.clipboard.writeText(contentToCopy);
                setStatus("✅ Скопировано! Весь список в буфере", false, true);
                // подсветка кнопки
                copyBtn.style.transform = "scale(0.96)";
                setTimeout(() => { copyBtn.style.transform = ""; }, 150);
                setTimeout(() => {
                    if (statusDiv.innerText.includes("Скопировано")) {
                        if (currentRawContent) setStatus(`✅ Данные загружены (${(currentRawContent.length/1024).toFixed(1)} KB)`, false, true);
                        else setStatus("Готово", false, false);
                    }
                }, 2000);
            } catch (err) {
                // fallback для старых браузеров
                let fallbackSuccess = false;
                try {
                    const fakeArea = document.createElement('textarea');
                    fakeArea.value = contentToCopy;
                    document.body.appendChild(fakeArea);
                    fakeArea.select();
                    fallbackSuccess = document.execCommand('copy');
                    document.body.removeChild(fakeArea);
                } catch(e) { /* тихо */ }
                if (fallbackSuccess) {
                    setStatus("✅ Скопировано (резервный метод)", false, true);
                } else {
                    setStatus("❌ Не удалось скопировать, разрешите доступ к буферу", true, false);
                }
            }
        }
        
        // Предзагрузка (опционально: при появлении основного окна, сделаем автоматическую загрузку)
        // но чтобы не мешать анимации, подождем пока mainApp станет видимым + небольшая задержка
        let autoLoaded = false;
        function autoLoadWhenReady() {
            if (autoLoaded) return;
            autoLoaded = true;
            // небольшая пауза, чтобы всё отрисовалось (после сплеша)
            setTimeout(() => {
                loadWhiteList();
            }, 400);
        }
        
        // Отслеживаем появление mainApp с классом visible — тогда триггерим авто-загрузку (удобно)
        const observer = new MutationObserver((mutations) => {
            mutations.forEach((mutation) => {
                if (mutation.attributeName === 'class') {
                    if (mainApp.classList.contains('visible') && !autoLoaded) {
                        autoLoadWhenReady();
                    }
                }
            });
        });
        observer.observe(mainApp, { attributes: true });
        // если mainApp уже видим до таймера (но visible появляется через 1.5 сек), проверим сразу
        if (mainApp.classList.contains('visible')) {
            autoLoadWhenReady();
        } else {
            // подстраховка, если visible появится до инициализации обсервера?
            // всё ок
        }
        
        // Кнопка загрузки ручная
        fetchBtn.addEventListener('click', () => {
            loadWhiteList();
        });
        
        copyBtn.addEventListener('click', () => {
            copyAllText();
        });
        
        // Инициализируем текст по умолчанию (типа заглушка)
        setTextareaContent("// Нажмите «Загрузить список», чтобы получить актуальные CIDR / ссылки.\n// Источник: WHITE-CIDR-RU-checked.txt\n// Репозиторий: igareck/vpn-configs-for-russia");
        copyBtn.disabled = true;
        
        // Также если после ошибки, кнопка копирования должна проверять состояние
        // дополнительно сделаем, чтобы после загрузки авто- или ручной, статус обновлялся
    })();
</script>
</body>
</html>
