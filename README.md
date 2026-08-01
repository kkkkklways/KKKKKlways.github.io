<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>煎包套餐服务查询</title>
    <style>
        :root {
            --bg: #fdf6f0;
            --card-bg: #fffaf5;
            --text: #4a3728;
            --accent: #d4943a;
            --accent-hover: #b87a2e;
            --border: #e8d5c4;
            --success-bg: #f0f7e8;
            --success-text: #4a7c2e;
            --info-bg: #fef9e7;
            --info-text: #7a6d2e;
            --shadow: 0 4px 20px rgba(120, 80, 40, 0.08);
            --radius: 16px;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background: linear-gradient(160deg, #fef8f2 0%, #f9efe2 40%, #f5e6d3 100%);
            font-family: "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "Noto Sans SC", system-ui, -apple-system, sans-serif;
            padding: 20px;
            background-attachment: fixed;
        }

        /* 背景装饰 */
        body::before {
            content: '';
            position: fixed;
            top: -120px;
            right: -80px;
            width: 300px;
            height: 300px;
            background: radial-gradient(circle, rgba(255, 180, 100, 0.18) 0%, transparent 70%);
            border-radius: 50%;
            pointer-events: none;
            z-index: 0;
        }
        body::after {
            content: '';
            position: fixed;
            bottom: -100px;
            left: -60px;
            width: 260px;
            height: 260px;
            background: radial-gradient(circle, rgba(210, 150, 80, 0.12) 0%, transparent 70%);
            border-radius: 50%;
            pointer-events: none;
            z-index: 0;
        }

        .card {
            position: relative;
            z-index: 1;
            background: var(--card-bg);
            border-radius: var(--radius);
            box-shadow: var(--shadow), 0 1px 0 rgba(255, 255, 255, 0.6) inset;
            padding: 40px 36px 36px;
            max-width: 480px;
            width: 100%;
            text-align: center;
            border: 1px solid var(--border);
            transition: transform 0.2s ease, box-shadow 0.3s ease;
        }

        .card:hover {
            box-shadow: 0 8px 32px rgba(120, 80, 40, 0.12);
        }

        /* 图标区域 */
        .icon-wrap {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            width: 64px;
            height: 64px;
            border-radius: 50%;
            background: linear-gradient(135deg, #fde8c8, #fce4b8);
            margin-bottom: 20px;
            font-size: 34px;
            box-shadow: 0 2px 10px rgba(180, 120, 50, 0.15);
        }

        .title {
            font-size: 1.1rem;
            color: var(--text);
            line-height: 1.7;
            margin-bottom: 24px;
            font-weight: 500;
            letter-spacing: 0.02em;
        }

        .title .highlight {
            font-weight: 700;
            color: #b86d2c;
            background: linear-gradient(180deg, transparent 60%, #fde4c0 60%);
            padding: 0 2px;
        }

        /* 输入区域 */
        .input-group {
            display: flex;
            gap: 12px;
            justify-content: center;
            align-items: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .input-group input {
            width: 120px;
            padding: 12px 16px;
            border: 2px solid var(--border);
            border-radius: 10px;
            font-size: 1.05rem;
            text-align: center;
            background: #fffefc;
            color: var(--text);
            outline: none;
            transition: all 0.25s ease;
            font-family: inherit;
            letter-spacing: 0.04em;
        }

        .input-group input:focus {
            border-color: var(--accent);
            box-shadow: 0 0 0 4px rgba(212, 148, 58, 0.1);
            background: #fff;
        }

        .input-group input::placeholder {
            color: #c4b5a3;
            letter-spacing: 0;
        }

        .btn {
            padding: 12px 28px;
            background: var(--accent);
            color: #fff;
            border: none;
            border-radius: 10px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            letter-spacing: 0.03em;
            transition: all 0.25s ease;
            font-family: inherit;
            white-space: nowrap;
            box-shadow: 0 2px 8px rgba(180, 120, 40, 0.2);
        }

        .btn:hover {
            background: var(--accent-hover);
            box-shadow: 0 4px 16px rgba(160, 100, 30, 0.28);
            transform: translateY(-1px);
        }
        .btn:active {
            transform: scale(0.97);
            box-shadow: 0 1px 4px rgba(160, 100, 30, 0.2);
        }

        /* 提示信息 */
        .hint {
            font-size: 0.82rem;
            color: #b09a83;
            margin-bottom: 8px;
            letter-spacing: 0.02em;
        }

        /* 结果消息 */
        .result-msg {
            margin-top: 22px;
            padding: 16px 20px;
            border-radius: 10px;
            font-size: 1rem;
            font-weight: 500;
            line-height: 1.6;
            letter-spacing: 0.02em;
            display: none;
            animation: fadeSlideIn 0.35s ease-out;
            word-break: break-word;
        }

        .result-msg.show {
            display: block;
        }

        .result-msg.success {
            background: var(--success-bg);
            color: var(--success-text);
            border: 1px solid #d4e8c0;
        }

        .result-msg.info {
            background: var(--info-bg);
            color: var(--info-text);
            border: 1px solid #e8dca0;
        }

        .result-msg .contact-name {
            font-weight: 700;
            letter-spacing: 0.04em;
        }

        @keyframes fadeSlideIn {
            from {
                opacity: 0;
                transform: translateY(10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* 响应式 */
        @media (max-width: 420px) {
            .card {
                padding: 28px 20px 26px;
                border-radius: 14px;
            }
            .title {
                font-size: 0.98rem;
            }
            .input-group input {
                width: 100px;
                padding: 10px 12px;
                font-size: 0.95rem;
            }
            .btn {
                padding: 10px 20px;
                font-size: 0.95rem;
            }
            .result-msg {
                font-size: 0.92rem;
                padding: 14px 16px;
            }
        }
    </style>
</head>
<body>

    <div class="card">
        <!-- 小图标 -->
        <div class="icon-wrap">🥟</div>

        <!-- 询问文字 -->
        <p class="title">
            请问您是否已经通过付费<strong>一份煎包</strong>开通该套餐服务？<br>
            <span style="font-size:0.9rem;color:#8b6f55;">若开通请输入 <span class="highlight">1</span>，若没有，请输入 <span class="highlight">2</span></span>
        </p>

        <!-- 输入区 -->
        <div class="hint">请在下方输入数字 1 或 2</div>
        <div class="input-group">
            <input
            type="text"
            id="userInput"
            placeholder="输入 1 或 2"
            maxlength="1"
            autocomplete="off"
            inputmode="numeric"
            >
            <button class="btn" id="submitBtn">确 认</button>
        </div>

        <!-- 结果消息 -->
        <div class="result-msg" id="resultMsg"></div>
    </div>

    <script>
        (function() {
            // 获取DOM元素
            const inputEl = document.getElementById('userInput');
            const submitBtn = document.getElementById('submitBtn');
            const resultMsg = document.getElementById('resultMsg');

            /**
             * 显示结果消息
             * @param {string} message - 要显示的HTML内容
             * @param {'success'|'info'} type - 消息类型
             */
            function showResult(message, type) {
                // 先移除旧的类和内容，触发重绘以重新播放动画
                resultMsg.classList.remove('show', 'success', 'info');
                resultMsg.innerHTML = '';

                // 用微小延迟确保CSS过渡重新触发
                requestAnimationFrame(() => {
                    resultMsg.innerHTML = message;
                    resultMsg.classList.add(type);
                    requestAnimationFrame(() => {
                        resultMsg.classList.add('show');
                    });
                });
            }

            /**
             * 处理用户输入
             */
            function handleSubmit() {
                const rawValue = inputEl.value.trim();
                const value = rawValue.replace(/\s+/g, ''); // 去除所有空白字符

                // 清空输入框（可选，让界面更清爽）
                // inputEl.value = '';

                if (value === '1') {
                    showResult(
                        '✅ <strong>该服务已开通</strong>，感谢您对本人的支持！<br>祝您用餐愉快～ 🥟',
                        'success'
                    );
                } else if (value === '2') {
                    showResult(
                        '📋 若有意愿开通，请线下联系 <span class="contact-name">KKKKKlways</span>',
                        'info'
                    );
                } else {
                    // 无效输入提示
                    showResult(
                        '⚠️ 请输入有效的数字 <strong>1</strong> 或 <strong>2</strong>，请重试。',
                        'info'
                    );
                    // 让输入框聚焦方便重新输入
                    inputEl.focus();
                    inputEl.select();
                }
            }

            // 点击按钮提交
            submitBtn.addEventListener('click', handleSubmit);

            // 按下回车键提交
            inputEl.addEventListener('keydown', function(e) {
                if (e.key === 'Enter') {
                    e.preventDefault();
                    handleSubmit();
                }
            });

            // 输入时限制只能输入数字1或2（实时过滤，提升体验）
            inputEl.addEventListener('input', function(e) {
                // 保留原始值用于处理
                let val = inputEl.value;
                // 过滤掉非1和2的字符，但允许空字符串（方便用户删除后重新输入）
                let filtered = '';
                for (let char of val) {
                    if (char === '1' || char === '2') {
                        filtered += char;
                    }
                }
                // 只取第一个有效字符
                if (filtered.length > 1) {
                    filtered = filtered[0];
                }
                if (val !== filtered) {
                    inputEl.value = filtered;
                }
            });

            // 页面加载后自动聚焦输入框
            inputEl.focus();

            // 点击卡片其他区域时，如果输入框失焦，可以保持体验（不做额外处理）
            // 但为了方便，点击result消息区域不做什么
            console.log('🥟 煎包套餐服务查询页面已就绪');
        })();
    </script>

</body>
</html>
