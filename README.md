<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>WHITE VPN — копировать конфиги</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background: #f5f7fb;
            font-family: system-ui, -apple-system, 'Segoe UI', 'Roboto', 'Helvetica Neue', sans-serif;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        /* АНИМАЦИОННЫЙ ЭКРАН ВХОДА */
        .splash-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #0b1b2a;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            transition: opacity 0.6s ease-in-out, visibility 0.6s ease-in-out;
        }

        .splash-content {
            text-align: center;
            color: white;
            animation: fadeSlideUp 0.7s cubic-bezier(0.2, 0.9, 0.4, 1.1) forwards;
        }

        .splash-title {
            font-size: 3rem;
            font-weight: 700;
            letter-spacing: -0.5px;
            margin-bottom: 0.75rem;
            background: linear-gradient(135deg, #FFFFFF, #b9e2ff);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
        }

        .splash-welcome {
            font-size: 1.2rem;
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

        .splash-screen.hide {
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
        }

        /* ОСНОВНОЙ КОНТЕНТ */
        .main-app {
            width: 100%;
            max-width: 420px;
            margin: 0 auto;
            text-align: center;
            opacity: 0;
            transition: opacity 0.5s ease;
        }

        .main-app.visible {
            opacity: 1;
        }

        .logo {
            margin-bottom: 35px;
        }

        .logo h1 {
            font-size: 2rem;
            font-weight: 700;
            letter-spacing: -0.5px;
            background: linear-gradient(135deg, #1a3c5c, #2c6288);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            margin-bottom: 6px;
        }

        .logo p {
            color: #6c7f94;
            font-size: 0.85rem;
        }

        /* БОЛЬШАЯ КНОПКА КОПИРОВАТЬ */
        .copy-btn {
            background: #1e4663;
            border: none;
            width: 100%;
            max-width: 320px;
            margin: 0 auto 25px;
            padding: 24px 20px;
            border-radius: 64px;
            color: white;
            font-size: 1.7rem;
            font-weight: 700;
            font-family: inherit;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 10px 25px -5px rgba(30, 70, 99, 0.3);
            border: 1px solid rgba(255,255,255,0.2);
        }

        .copy-btn:active {
            transform: scale(0.97);
            background: #0f3550;
        }

        .copy-btn span {
            font-size: 1.9rem;
        }

        .copy-btn.loading {
            opacity: 0.7;
            cursor: wait;
            transform: none;
        }

        .message-area {
            background: #ffffff;
            border-radius: 28px;
            padding: 20px 18px;
            margin-top: 15px;
            box-shadow: 0 2px 12px rgba(0, 0, 0, 0.03);
            border: 1px solid #e9edf2;
            transition: all 0.2s;
            min-height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .info-text {
            font-size: 1rem;
            color: #2c3e4e;
            line-height: 1.4;
            font-weight: 450;
        }

        .success-message {
            background: #eafaf1;
            border-color: #c0e0d0;
        }

        .success-message .info-text {
            color: #1a5c3a;
        }

        .error-message {
            background: #fff0ef;
            border-color: #f5cfcc;
        }

        .error-message .info-text {
            color: #bc4e4e;
        }

        .footer-note {
            margin-top: 35px;
            font-size: 0.7rem;
            color: #8c9bb0;
            text-align: center;
            border-top: 1px solid #eef2f8;
            padding-top: 20px;
        }

        .retry-link {
            color: #2c6288;
            text-decoration: underline;
            cursor: pointer;
            font-weight: 500;
        }

        @media (max-width: 480px) {
            .copy-btn {
                padding: 20px 16px;
                font-size: 1.5rem;
            }
            .copy-btn span {
                font-size: 1.7rem;
            }
            .logo h1 {
                font-size: 1.8rem;
            }
            .splash-title {
                font-size: 2.3rem;
            }
            .splash-welcome {
                font-size: 1rem;
            }
        }
    </style>
</head>
<body>

    <!-- АНИМАЦИОННЫЙ ЭКРАН ВХОДА -->
    <div class="splash-screen" id="splashScreen">
        <div class="splash-content">
            <div class="splash-title">WHITE VPN</div>
            <div class="splash-welcome">
                Добро пожаловать
                <span class="splash-dot"></span><span class="splash-dot"></span>
            </div>
        </div>
    </div>

    <!-- ОСНОВНОЕ ПРИЛОЖЕНИЕ -->
    <div class="main-app" id="mainApp">
        <div class="logo">
            <h1>WHITE VPN</h1>
            <p>исходные конфигурации</p>
        </div>

        <button class="copy-btn" id="copyBigBtn">
            <span>📋</span> Копировать исходники
        </button>

        <div class="message-area" id="messageBox">
            <div class="info-text" id="messageText">
                Нажмите кнопку, чтобы скопировать CIDR‑список
            </div>
        </div>

        <div class="footer-note">
            🔄 Для обновления данных — перезагрузите страницу<br>
            📡 Источник: igareck/vpn-configs-for-russia
        </div>
    </div>

    <script>
        (function() {
            // URL исходного файла
            const SOURCE_URL = "https://raw.githubusercontent.com/igareck/vpn-configs-for-russia/main/WHITE-CIDR-RU-checked.txt";
            
            // Прокси-серверы для обхода CORS (несколько вариантов)
            const PROXIES = [
                "https://api.allorigins.win/raw?url=",
                "https://cors-anywhere.herokuapp.com/",
                "https://thingproxy.freeboard.io/fetch/"
            ];
            
            let cachedList = null;
            let isLoading = false;
            let currentProxyIndex = 0;
            
            // DOM элементы
            const splash = document.getElementById('splashScreen');
            const mainApp = document.getElementById('mainApp');
            const copyButton = document.getElementById('copyBigBtn');
            const messageBox = document.getElementById('messageBox');
            const messageTextEl = document.getElementById('messageText');
            
            // Анимация входа
            setTimeout(() => {
                if (splash) {
                    splash.classList.add('hide');
                    setTimeout(() => {
                        if (splash && splash.parentNode) {
                            splash.style.display = 'none';
                        }
                    }, 600);
                }
                mainApp.classList.add('visible');
            }, 1500);
            
            function setMessage(text, isError = false, isSuccess = false) {
                messageTextEl.innerText = text;
                messageBox.classList.remove('success-message', 'error-message');
                if (isSuccess) {
                    messageBox.classList.add('success-message');
                } else if (isError) {
                    messageBox.classList.add('error-message');
                }
            }
            
            function setButtonLoading(loading) {
                if (loading) {
                    copyButton.classList.add('loading');
                    copyButton.disabled = true;
                } else {
                    copyButton.classList.remove('loading');
                    copyButton.disabled = false;
                }
            }
            
            // Прямая загрузка без прокси (пытаемся первым делом)
            async function directFetch() {
                const controller = new AbortController();
                const timeoutId = setTimeout(() => controller.abort(), 8000);
                const response = await fetch(SOURCE_URL, {
                    signal: controller.signal,
                    mode: 'cors',
                    cache: 'no-store'
                });
                clearTimeout(timeoutId);
                if (!response.ok) throw new Error(`HTTP ${response.status}`);
                const text = await response.text();
                if (!text || text.trim() === "") throw new Error("Empty response");
                const trimmed = text.trim();
                if (trimmed.startsWith("<!DOCTYPE") || trimmed.startsWith("<html")) {
                    throw new Error("HTML response");
                }
                return text;
            }
            
            // Загрузка через прокси (по очереди)
            async function fetchViaProxy(proxyUrl) {
                const fullUrl = proxyUrl + encodeURIComponent(SOURCE_URL);
                const controller = new AbortController();
                const timeoutId = setTimeout(() => controller.abort(), 10000);
                const response = await fetch(fullUrl, {
                    signal: controller.signal,
                    cache: 'no-store'
                });
                clearTimeout(timeoutId);
                if (!response.ok) throw new Error(`Proxy HTTP ${response.status}`);
                const text = await response.text();
                if (!text || text.trim() === "") throw new Error("Empty proxy response");
                // Проверяем, не вернулся ли HTML с ошибкой
                const trimmed = text.trim();
                if (trimmed.startsWith("<!DOCTYPE") || trimmed.startsWith("<html") || trimmed.includes("error")) {
                    throw new Error("Proxy returned HTML");
                }
                return text;
            }
            
            // Основная функция загрузки с автоматическим переключением прокси
            async function fetchWhiteListWithFallback() {
                if (isLoading) return;
                isLoading = true;
                setButtonLoading(true);
                setMessage("⏳ Загрузка конфигураций...", false);
                
                // Сначала пробуем прямую загрузку
                try {
                    const data = await directFetch();
                    cachedList = data;
                    setMessage("✅ Список загружен! Нажмите «Копировать исходники»", false, true);
                    setButtonLoading(false);
                    isLoading = false;
                    return true;
                } catch (directError) {
                    console.log("Direct fetch failed:", directError.message);
                    // Пробуем прокси по очереди
                    for (let i = 0; i < PROXIES.length; i++) {
                        try {
                            setMessage(`⏳ Пробуем альтернативный канал (${i+1}/${PROXIES.length})...`, false);
                            const data = await fetchViaProxy(PROXIES[i]);
                            cachedList = data;
                            setMessage("✅ Список загружен! Нажмите «Копировать исходники»", false, true);
                            setButtonLoading(false);
                            isLoading = false;
                            return true;
                        } catch (proxyError) {
                            console.log(`Proxy ${i} failed:`, proxyError.message);
                            continue;
                        }
                    }
                    // Все прокси не сработали
                    throw new Error("Все способы загрузки не удались");
                }
            }
            
            // Альтернативная загрузка через JSONP-подобный fetch с дополнительным заголовком
            async function forceFetchWithRetry() {
                if (isLoading) return;
                isLoading = true;
                setButtonLoading(true);
                setMessage("🔄 Повторная попытка загрузки...", false);
                
                // Дополнительная попытка с no-cors (может не дать тело ответа, но пробуем)
                try {
                    const response = await fetch(SOURCE_URL, {
                        method: 'GET',
                        mode: 'no-cors',
                        cache: 'no-store'
                    });
                    // при no-cors response.ok может быть false, но мы не можем прочитать текст
                    // поэтому этот метод не подходит. Используем другой прокси
                    const altProxy = "https://corsproxy.io/?" + encodeURIComponent(SOURCE_URL);
                    const proxyResp = await fetch(altProxy, { cache: 'no-store' });
                    if (proxyResp.ok) {
                        const text = await proxyResp.text();
                        if (text && text.length > 50 && !text.includes("<html")) {
                            cachedList = text;
                            setMessage("✅ Список загружен! Нажмите «Копировать исходники»", false, true);
                            setButtonLoading(false);
                            isLoading = false;
                            return true;
                        }
                    }
                    throw new Error("Retry failed");
                } catch (e) {
                    console.error("Force fetch error:", e);
                    setMessage("❌ Ошибка загрузки. Проверьте интернет-соединение и перезагрузите страницу.\n\n💡 Совет: Попробуйте открыть страницу в другом браузере или отключить VPN/блокировщики.", true);
                    setButtonLoading(false);
                    isLoading = false;
                    return false;
                }
            }
            
            // Копирование в буфер
            async function copyToClipboard() {
                if (!cachedList) {
                    setMessage("⏳ Сначала загружаем данные...", false);
                    const success = await fetchWhiteListWithFallback();
                    if (!success || !cachedList) {
                        // Если не удалось, пробуем форсированную загрузку
                        await forceFetchWithRetry();
                        if (!cachedList) {
                            setMessage("❌ Не удалось загрузить список. Перезагрузите страницу или проверьте соединение.", true);
                            return;
                        }
                    }
                }
                
                if (!cachedList || cachedList.trim() === "") {
                    setMessage("❌ Список пуст. Попробуйте перезагрузить страницу.", true);
                    return;
                }
                
                // Пытаемся скопировать
                try {
                    await navigator.clipboard.writeText(cachedList);
                    setMessage(
                        "✨ Зайдите в приложение Happ, нажмите «+», добавить из буфера обмена ✨",
                        false,
                        true
                    );
                    
                    copyButton.style.transform = "scale(0.97)";
                    setTimeout(() => { copyButton.style.transform = ""; }, 150);
                    if (navigator.vibrate) navigator.vibrate(50);
                    
                } catch (err) {
                    console.warn("Clipboard error:", err);
                    // Fallback метод
                    let fallbackOk = false;
                    try {
                        const textarea = document.createElement('textarea');
                        textarea.value = cachedList;
                        textarea.style.position = 'fixed';
                        textarea.style.top = '-9999px';
                        textarea.style.left = '-9999px';
                        document.body.appendChild(textarea);
                        textarea.select();
                        textarea.setSelectionRange(0, 99999);
                        fallbackOk = document.execCommand('copy');
                        document.body.removeChild(textarea);
                    } catch (e) { /* ignore */ }
                    
                    if (fallbackOk) {
                        setMessage(
                            "✨ Зайдите в приложение Happ, нажмите «+», добавить из буфера обмена ✨",
                            false,
                            true
                        );
                    } else {
                        setMessage("❌ Не удалось скопировать. Попробуйте выделить текст вручную или обновите страницу.", true);
                    }
                }
            }
            
            // Обработчик кнопки
            copyButton.addEventListener('click', (e) => {
                e.preventDefault();
                copyToClipboard();
            });
            
            // Автоматическая загрузка при старте (с задержкой после анимации)
            setTimeout(() => {
                fetchWhiteListWithFallback().catch(err => {
                    console.error("Auto-load error:", err);
                    setMessage("⚠️ Не удалось автоматически загрузить данные. Нажмите кнопку для повторной попытки.", true);
                    setButtonLoading(false);
                    isLoading = false;
                });
            }, 1000);
            
            // Добавляем возможность ретрая по клику на сообщение об ошибке
            messageBox.addEventListener('click', (e) => {
                if (messageTextEl.innerText.includes("Ошибка") || messageTextEl.innerText.includes("Не удалось")) {
                    if (!isLoading) {
                        fetchWhiteListWithFallback();
                    }
                }
            });
        })();
    </script>
</body>
</html>
