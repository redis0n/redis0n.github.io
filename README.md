<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>White CIDR Helper — копирование списка</title>
    <style>
        * {
            box-sizing: border-box;
        }

        body {
            background: #f0f4f8;
            font-family: system-ui, 'Segoe UI', 'Roboto', 'Inter', 'Helvetica Neue', sans-serif;
            margin: 0;
            padding: 24px 20px;
            color: #0a2540;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 32px;
            box-shadow: 0 20px 35px -12px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            transition: all 0.2s ease;
        }

        .header {
            background: #111c2e;
            color: white;
            padding: 1.8rem 2rem;
            border-bottom: 3px solid #2b6e9e;
        }

        .header h1 {
            margin: 0 0 0.3rem;
            font-weight: 600;
            font-size: 1.8rem;
            letter-spacing: -0.3px;
        }

        .header p {
            margin: 0;
            opacity: 0.8;
            font-size: 0.95rem;
        }

        .badge {
            display: inline-block;
            background: #2b6e9e30;
            backdrop-filter: blur(2px);
            border-radius: 40px;
            padding: 0.2rem 0.8rem;
            font-size: 0.75rem;
            font-family: monospace;
            margin-top: 12px;
            color: #c9e9ff;
        }

        .controls {
            padding: 1.2rem 2rem;
            background: #f9fafc;
            border-bottom: 1px solid #e4e9f0;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            gap: 15px;
        }

        .btn-group {
            display: flex;
            gap: 12px;
            flex-wrap: wrap;
        }

        button {
            background: white;
            border: 1px solid #cbd5e1;
            padding: 10px 20px;
            font-size: 0.9rem;
            font-weight: 500;
            border-radius: 60px;
            cursor: pointer;
            transition: 0.2s;
            font-family: inherit;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            box-shadow: 0 1px 1px rgba(0,0,0,0.02);
        }

        button i {
            font-style: normal;
            font-weight: 600;
            font-size: 1.1rem;
        }

        button.primary {
            background: #0f3b5c;
            border-color: #0f3b5c;
            color: white;
            box-shadow: 0 2px 5px rgba(15,59,92,0.2);
        }

        button.primary:hover {
            background: #1c4e74;
            transform: translateY(-1px);
            border-color: #1c4e74;
        }

        button.secondary {
            background: #eef2ff;
            border-color: #bfd9f0;
            color: #155a8a;
        }

        button.secondary:hover {
            background: #e2eaff;
            border-color: #8bb3d0;
        }

        button:active {
            transform: translateY(1px);
        }

        .status {
            font-size: 0.85rem;
            background: #eef2fa;
            padding: 6px 14px;
            border-radius: 40px;
            color: #1f6392;
            font-weight: 500;
        }

        .status.error {
            background: #ffe6e5;
            color: #b13e3e;
        }

        .status.success {
            background: #e1f7e8;
            color: #1e7640;
        }

        .content-area {
            padding: 1.6rem 2rem 2rem;
        }

        .info-bar {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            flex-wrap: wrap;
            margin-bottom: 1rem;
            gap: 10px;
        }

        .counter {
            background: #eef2ff;
            border-radius: 32px;
            padding: 4px 15px;
            font-size: 0.8rem;
            font-weight: 500;
            font-family: monospace;
        }

        .preview-header {
            font-size: 0.75rem;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            color: #5a6e8a;
            font-weight: 600;
        }

        .list-container {
            border: 1px solid #e2e8f0;
            border-radius: 20px;
            background: #ffffff;
            overflow: hidden;
            transition: 0.1s;
        }

        textarea {
            width: 100%;
            border: none;
            padding: 18px 20px;
            font-family: 'SF Mono', 'JetBrains Mono', 'Fira Code', monospace;
            font-size: 13px;
            line-height: 1.45;
            resize: vertical;
            background: #fefefe;
            color: #1e2f3e;
            outline: none;
            white-space: pre-wrap;
            word-break: break-all;
        }

        textarea:focus {
            background: #ffffff;
        }

        .footer-note {
            margin-top: 20px;
            font-size: 0.7rem;
            text-align: center;
            color: #6c7e97;
            border-top: 1px solid #eef2f5;
            padding-top: 18px;
        }

        .spinner {
            display: inline-block;
            width: 14px;
            height: 14px;
            border: 2px solid rgba(255,255,255,0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 0.8s linear infinite;
            margin-right: 6px;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        @media (max-width: 640px) {
            body { padding: 16px; }
            .controls { flex-direction: column; align-items: stretch; }
            .btn-group { justify-content: center; }
            .status { text-align: center; }
            .header h1 { font-size: 1.5rem; }
        }

        .copy-icon {
            font-size: 1.1rem;
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h1>📋 White CIDR / RU список</h1>
        <p>Автоматическая загрузка с официального raw-репозитория</p>
        <div class="badge">🔗 источник: igareck/vpn-configs-for-russia</div>
    </div>

    <div class="controls">
        <div class="btn-group">
            <button id="fetchBtn" class="primary">
                🌐 Загрузить ссылки / CIDR
            </button>
            <button id="copyBtn" class="secondary" disabled>
                📋 Копировать всё одной кнопкой
            </button>
        </div>
        <div id="statusMsg" class="status">
            ⚡ Готов к загрузке
        </div>
    </div>

    <div class="content-area">
        <div class="info-bar">
            <div class="preview-header">📄 Актуальные записи (строки из WHITE-CIDR-RU-checked.txt)</div>
            <div class="counter" id="lineCounter">— элементов</div>
        </div>
        <div class="list-container">
            <textarea id="dataTextarea" rows="12" readonly placeholder="Нажмите «Загрузить», чтобы получить список из репозитория...&#10;Файл содержит белые CIDR-сети или ссылки (конфигурации) для России."></textarea>
        </div>
        <div class="footer-note">
            ⚡ Сырые данные парсятся напрямую с 
            <code>https://raw.githubusercontent.com/igareck/vpn-configs-for-russia/main/WHITE-CIDR-RU-checked.txt</code>
            <br>✅ Кнопка «Копировать» скопирует всё содержимое в буфер обмена (каждая строка — как в исходнике)
        </div>
    </div>
</div>

<script>
    (function() {
        // ---------- DOM elements ----------
        const fetchBtn = document.getElementById('fetchBtn');
        const copyBtn = document.getElementById('copyBtn');
        const textarea = document.getElementById('dataTextarea');
        const statusDiv = document.getElementById('statusMsg');
        const lineCounterSpan = document.getElementById('lineCounter');

        // ---------- состояние ----------
        let currentRawContent = "";       // храним последний загруженный текст (RAW)
        let isLoading = false;

        // ---------- вспомогательные функции UI ----------
        function setStatus(text, isError = false, isSuccess = false) {
            statusDiv.innerHTML = text;
            statusDiv.classList.remove('error', 'success');
            if (isError) {
                statusDiv.classList.add('error');
            } else if (isSuccess) {
                statusDiv.classList.add('success');
            } else {
                // обычный нейтральный статус
                statusDiv.classList.remove('error', 'success');
            }
        }

        function updateLineCounter(content) {
            if (!content || content.trim() === "") {
                lineCounterSpan.innerText = "0 элементов";
                return;
            }
            // считаем количество непустых строк (как полезные строки, но показываем общее количество строк, исключая пустые в конце)
            const lines = content.split(/\r?\n/);
            let nonEmpty = lines.filter(line => line.trim().length > 0).length;
            lineCounterSpan.innerText = `${nonEmpty} строка(и) / ${lines.length} всего строк`;
        }

        // обновить содержимое и счетчики
        function setTextareaContent(content) {
            textarea.value = content;
            currentRawContent = content;
            updateLineCounter(content);
            // если есть контент и он не пустой — активируем кнопку копирования
            if (content && content.trim().length > 0) {
                copyBtn.disabled = false;
            } else {
                copyBtn.disabled = true;
            }
        }

        // показать индикатор загрузки на кнопке fetch
        function setFetchLoading(loading) {
            isLoading = loading;
            if (loading) {
                fetchBtn.disabled = true;
                fetchBtn.innerHTML = `<span class="spinner"></span> Загрузка...`;
            } else {
                fetchBtn.disabled = false;
                fetchBtn.innerHTML = `🌐 Загрузить ссылки / CIDR`;
            }
        }

        // основная функция загрузки с raw github
        async function loadWhiteListFromGitHub() {
            if (isLoading) return;
            const url = "https://raw.githubusercontent.com/igareck/vpn-configs-for-russia/main/WHITE-CIDR-RU-checked.txt";
            setFetchLoading(true);
            setStatus("⏳ Загрузка данных...", false, false);
            
            try {
                // Используем fetch с таймаутом для надёжности
                const controller = new AbortController();
                const timeoutId = setTimeout(() => controller.abort(), 15000); // 15 секунд таймаут
                
                const response = await fetch(url, {
                    signal: controller.signal,
                    cache: "no-cache",
                    headers: { "Cache-Control": "no-cache" }
                });
                clearTimeout(timeoutId);
                
                if (!response.ok) {
                    throw new Error(`Ошибка HTTP ${response.status}: ${response.statusText}`);
                }
                
                const rawText = await response.text();
                // Проверка на пустой или подозрительно короткий контент (меньше 3 символов), но белый список может быть коротким?
                if (rawText === null || rawText === undefined) {
                    throw new Error("Получен пустой ответ от сервера");
                }
                
                // сохраняем оригинальный текст как есть (сохраняем переносы строк)
                setTextareaContent(rawText);
                setStatus(`✅ Загружено успешно (${(rawText.length / 1024).toFixed(1)} KB)`, false, true);
                
                // дополнительная проверка: если в тексте есть <html или похоже на ошибку github (но хтмл обычно не возвращается на raw)
                if (rawText.trim().startsWith("<!DOCTYPE") || rawText.trim().startsWith("<html")) {
                    setStatus("⚠️ Предупреждение: получен HTML вместо текста. Возможно, ссылка недоступна или редирект.", true, false);
                }
                
            } catch (err) {
                console.error("Fetch error:", err);
                let errorMsg = "";
                if (err.name === "AbortError") {
                    errorMsg = "❌ Таймаут загрузки. Проверьте интернет или повторите попытку.";
                } else if (err.message.includes("Failed to fetch") || err.message.includes("NetworkError")) {
                    errorMsg = "❌ Сетевая ошибка. Не удалось связаться с GitHub (CORS/блокировка?). Проверьте соединение.";
                } else {
                    errorMsg = `❌ Ошибка: ${err.message}`;
                }
                setStatus(errorMsg, true, false);
                // в случае ошибки сбрасываем textarea до предыдущего состояния? Лучше показать ошибку, но не сбрасывать существующий контент.
                // но если ранее не было контента, показываем пустое поле с подсказкой
                if (!currentRawContent) {
                    setTextareaContent("// Не удалось загрузить список. Нажмите повторить позже.\n// Проверьте доступ к raw.githubusercontent.com");
                }
                // все равно кнопка копирования должна быть disabled если нет валидного контента.
                if (!currentRawContent || currentRawContent.trim() === "") {
                    copyBtn.disabled = true;
                } else {
                    // если старый контент был, но сбой, не блокируем копирование предыдущего
                    copyBtn.disabled = false;
                }
            } finally {
                setFetchLoading(false);
            }
        }

        // ---------- функция копирования в буфер обмена ----------
        async function copyToClipboard() {
            // берем актуальный текст из textarea (на случай ручного редактирования? но textarea readonly,
            // тем не менее используем currentRawContent или value — они синхронны)
            let textToCopy = textarea.value;
            if (!textToCopy || textToCopy.trim() === "") {
                setStatus("📭 Нечего копировать: список пуст. Сначала загрузите данные.", true, false);
                return;
            }
            
            // сохраняем оригинальный статус для восстановления
            const originalStatusText = statusDiv.innerText;
            const hadErrorClass = statusDiv.classList.contains('error');
            
            try {
                await navigator.clipboard.writeText(textToCopy);
                setStatus("✅ Скопировано! Весь список в буфере обмена.", false, true);
                // кратковременное выделение кнопки
                copyBtn.style.transform = "scale(0.97)";
                setTimeout(() => { copyBtn.style.transform = ""; }, 150);
                // через 2 секунды вернуть предыдущий статус, если он не был ошибкой? но лучше показать что успех, затем сбросить через 2.5 сек
                setTimeout(() => {
                    if (statusDiv.innerText.includes("Скопировано")) {
                        if (currentRawContent) {
                            setStatus(`✅ Данные загружены (${(currentRawContent.length / 1024).toFixed(1)} KB)`, false, true);
                        } else {
                            setStatus("✨ Готово. Нажмите загрузить для обновления", false, false);
                        }
                    }
                }, 2500);
            } catch (err) {
                console.error("Clipboard error:", err);
                let fallbackMessage = "❌ Не удалось скопировать. Возможно, недостаточно прав.";
                if (err.message) fallbackMessage += ` (${err.message})`;
                setStatus(fallbackMessage, true, false);
                
                // дополнительная fallback-попытка для старых браузеров
                if (document.execCommand) {
                    try {
                        const textareaTemp = document.createElement('textarea');
                        textareaTemp.value = textToCopy;
                        document.body.appendChild(textareaTemp);
                        textareaTemp.select();
                        const success = document.execCommand('copy');
                        document.body.removeChild(textareaTemp);
                        if (success) {
                            setStatus("✅ Скопировано (fallback метод). Список в буфере.", false, true);
                            setTimeout(() => {
                                if (statusDiv.innerText.includes("fallback")) {
                                    setStatus(`📋 Готово: список актуален`, false, false);
                                }
                            }, 2000);
                        } else {
                            setStatus("❌ Копирование не поддерживается в вашем браузере.", true, false);
                        }
                    } catch (fallbackErr) {
                        setStatus("❌ Ошибка копирования даже через execCommand", true, false);
                    }
                }
            }
        }

        // ---------- предзагрузка (опционально при старте) ----------
        // Можно при загрузке страницы автоматически подтянуть данные,
        // чтобы пользователь сразу видел список. Но по условию "он берёт все ссылки вручную с ... и вставляет их"
        // я сделаю опциональную авто-загрузку при первом открытии (удобно)
        // однако если хотим оставить только по кнопке, но обычно удобно загрузить по умолчанию.
        // Для комфорта при первом визите попробуем загрузить данные автоматически.
        let autoLoaded = false;
        
        async function initialAutoLoad() {
            if (autoLoaded) return;
            autoLoaded = true;
            // выполняем загрузку, но без блокировки интерфейса
            await loadWhiteListFromGitHub();
        }
        
        // Добавим небольшую задержку для инициализации, чтобы не перегружать индикатор
        setTimeout(() => {
            initialAutoLoad().catch(e => console.warn("auto load minor:", e));
        }, 200);
        
        // обработчики кнопок
        fetchBtn.addEventListener('click', () => {
            loadWhiteListFromGitHub();
        });
        
        copyBtn.addEventListener('click', () => {
            copyToClipboard();
        });
        
        // Дополнительная проверка: если текст уже загружен автоматически, активировать кнопку копирования (после загрузки)
        // и при ручной загрузке всё обновится.
        // также при клике на копирование, если текст пустой, показываем предупреждение.
        
        // инициализация начального состояния (без данных)
        setTextareaContent("// Нажмите «Загрузить», чтобы получить актуальный список CIDR / ссылок.\n// Источник: WHITE-CIDR-RU-checked.txt\n// Репозиторий: igareck/vpn-configs-for-russia");
        copyBtn.disabled = true;
        
        // добавим дополнительный эффект при копировании
        window.addEventListener('load', () => {
            // ничего лишнего
        });
        
        // если вдруг пользователь вручную захочет обновить - ок
        // также повторная загрузка перезапишет текстовое поле.
        // и для наглядности - подсказка, что можно копировать.
    })();
</script>
</body>
</html>
