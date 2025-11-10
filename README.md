<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>软件系统 - 电脑版</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'PingFang SC', 'Microsoft YaHei', sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        
        :root {
            --primary: #6C63FF;
            --secondary: #4A44B5;
            --accent: #FF6584;
            --light: #F8F9FA;
            --dark: #2D3047;
            --gray: #8A8D93;
            --animation-speed: 0.4s;
        }
        
        body {
            background: transparent;
            min-height: 100vh;
            padding: 20px;
            color: var(--light);
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
            overflow-x: hidden;
            transition: background 0.5s ease;
        }
        
        /* 背景层 */
        .background-layer {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, rgba(26, 42, 108, 0.7), rgba(74, 68, 181, 0.7), rgba(108, 99, 255, 0.7));
            z-index: -2;
            transition: background 0.5s ease;
        }
        
        /* 玻璃拟态效果层 */
        .glass-layer {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            z-index: -1;
        }
        
        .container {
            width: 100%;
            max-width: 1400px;
            margin: 0 auto;
            position: relative;
            overflow: hidden;
        }
        
        /* 页面容器 */
        .page-container {
            position: relative;
            width: 100%;
            transition: transform 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
        }
        
        .page {
            width: 100%;
            transition: opacity 0.3s ease;
        }
        
        .page.hidden {
            display: none;
        }
        
        /* 电脑状态栏 */
        .desktop-status-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 20px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 10px;
            margin-bottom: 20px;
            backdrop-filter: blur(10px);
            font-size: 0.9rem;
            transition: all 0.3s ease;
            width: 100%;
        }
        
        .status-left {
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .status-center {
            font-weight: bold;
            font-size: 1.1rem;
        }
        
        .status-right {
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .battery {
            width: 20px;
            height: 10px;
            border: 1px solid white;
            border-radius: 2px;
            position: relative;
        }
        
        .battery::after {
            content: '';
            position: absolute;
            right: -3px;
            top: 2px;
            width: 2px;
            height: 4px;
            background: white;
            border-radius: 0 1px 1px 0;
        }
        
        .battery-level {
            height: 100%;
            width: 70%;
            background: white;
            border-radius: 1px;
        }
        
        /* 横向表头 */
        .header-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            padding: 25px 40px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            backdrop-filter: blur(15px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .header-left {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .logo {
            width: 60px;
            height: 60px;
            border-radius: 12px;
            background: var(--primary);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 28px;
            color: white;
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
        }
        
        .title-group h1 {
            font-size: 2.5rem;
            background: linear-gradient(to right, #fff, #e0e0ff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        .subtitle {
            font-size: 1.1rem;
            opacity: 0.9;
        }
        
        .header-right {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .year-badge {
            background: var(--accent);
            color: white;
            padding: 10px 20px;
            border-radius: 20px;
            font-size: 1rem;
            font-weight: bold;
        }
        
        /* 紧凑的应用图标 - 电脑版布局 */
        .apps-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 25px;
            margin-bottom: 40px;
        }
        
        .app-item {
            background: rgba(255, 255, 255, 0.15);
            border-radius: 18px;
            padding: 25px 15px;
            text-align: center;
            transition: all 0.3s ease;
            cursor: pointer;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        
        .app-item:hover {
            transform: translateY(-8px);
            background: rgba(255, 255, 255, 0.25);
            box-shadow: 0 15px 25px rgba(0, 0, 0, 0.3);
        }
        
        .app-icon {
            width: 70px;
            height: 70px;
            border-radius: 18px;
            margin: 0 auto 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 28px;
            color: white;
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
        }
        
        .app-name {
            font-size: 1rem;
            font-weight: 600;
        }
        
        .add-app-item {
            background: rgba(255, 255, 255, 0.1);
            border: 2px dashed rgba(255, 255, 255, 0.4);
        }
        
        .add-app-item:hover {
            background: rgba(255, 255, 255, 0.2);
            border-color: rgba(255, 255, 255, 0.6);
        }
        
        .add-icon {
            width: 70px;
            height: 70px;
            border-radius: 50%;
            margin: 0 auto 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 32px;
            color: white;
            background: rgba(255, 255, 255, 0.2);
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
        }
        
        /* 电脑版弹出窗口 */
        .modal-section {
            background: rgba(255, 255, 255, 0.1);
            padding: 30px;
            border-radius: 20px;
            margin-top: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(15px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            width: 600px;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0.9);
            z-index: 10;
            max-height: 80vh;
            overflow-y: auto;
            opacity: 0;
            visibility: hidden;
            transition: all var(--animation-speed) cubic-bezier(0.25, 0.46, 0.45, 0.94);
        }
        
        .active-section {
            transform: translate(-50%, -50%) scale(1);
            opacity: 1;
            visibility: visible;
        }
        
        h2 {
            margin-bottom: 20px;
            text-align: center;
            font-size: 1.8rem;
            color: white;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 500;
        }
        
        input, select {
            width: 100%;
            padding: 12px 15px;
            border-radius: 10px;
            border: none;
            background: rgba(255, 255, 255, 0.9);
            font-size: 1rem;
            transition: all 0.3s ease;
        }
        
        input:focus, select:focus {
            outline: none;
            box-shadow: 0 0 0 2px var(--primary);
        }
        
        .button-group {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-top: 25px;
        }
        
        button {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 10px;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        button:hover {
            background: var(--secondary);
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
        }
        
        .back-btn {
            background: var(--gray);
        }
        
        .back-btn:hover {
            background: #6c757d;
        }
        
        footer {
            text-align: center;
            margin-top: 40px;
            padding: 20px;
            font-size: 0.9rem;
            opacity: 0.8;
            width: 100%;
        }
        
        /* 搜索框样式 */
        .search-container {
            display: flex;
            align-items: center;
            background: rgba(255, 255, 255, 0.15);
            border-radius: 10px;
            padding: 10px 20px;
            margin-bottom: 25px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            width: 100%;
            max-width: 500px;
            margin-left: auto;
            margin-right: auto;
        }
        
        .search-container i {
            color: rgba(255, 255, 255, 0.7);
            margin-right: 10px;
        }
        
        .search-container input {
            background: transparent;
            border: none;
            color: white;
            padding: 8px 0;
            width: 100%;
        }
        
        .search-container input::placeholder {
            color: rgba(255, 255, 255, 0.7);
        }
        
        .search-container input:focus {
            box-shadow: none;
        }
        
        /* 电脑底部导航栏 */
        .desktop-nav {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 30px;
            padding: 15px 0;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            margin-top: 20px;
            backdrop-filter: blur(10px);
            width: 100%;
        }
        
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
            font-size: 0.9rem;
            cursor: pointer;
            padding: 10px 20px;
            border-radius: 10px;
            transition: all 0.3s ease;
        }
        
        .nav-item:hover {
            background: rgba(255, 255, 255, 0.1);
        }
        
        .nav-item i {
            font-size: 1.4rem;
        }
        
        /* 分页样式 */
        .pagination {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin: 20px 0;
        }
        
        .page-btn {
            background: rgba(255, 255, 255, 0.15);
            color: white;
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .page-btn.active {
            background: var(--primary);
        }
        
        .page-btn:hover:not(.active) {
            background: rgba(255, 255, 255, 0.25);
        }
        
        /* 遮罩层 */
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(5px);
            z-index: 5;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease, visibility 0.3s ease;
        }
        
        .overlay.active {
            opacity: 1;
            visibility: visible;
        }
        
        /* 设置项样式 */
        .settings-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 0;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .settings-item:last-child {
            border-bottom: none;
        }
        
        .settings-item label {
            margin-bottom: 0;
            flex: 1;
        }
        
        .settings-item input[type="color"],
        .settings-item select {
            width: auto;
            min-width: 100px;
        }
        
        .settings-item input[type="file"] {
            background: rgba(255, 255, 255, 0.2);
            color: white;
            padding: 8px;
        }
        
        .settings-item input[type="file"]::file-selector-button {
            background: var(--primary);
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 5px;
            cursor: pointer;
        }
        
        @media (max-width: 1024px) {
            .container {
                max-width: 95%;
            }
            
            .apps-container {
                grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
                gap: 20px;
            }
        }
        
        @media (max-width: 768px) {
            .header-row {
                flex-direction: column;
                gap: 15px;
                text-align: center;
            }
            
            .apps-container {
                grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
                gap: 15px;
            }
            
            h1 {
                font-size: 1.8rem;
            }
            
            .button-group {
                flex-direction: column;
            }
            
            .modal-section {
                width: 90%;
                padding: 20px;
            }
        }
    </style>
</head>
<body>
    <!-- 背景层 -->
    <div class="background-layer" id="backgroundLayer"></div>
    <div class="glass-layer"></div>
    
    <!-- 遮罩层 -->
    <div class="overlay" id="overlay"></div>
    
    <div class="container">
        <!-- 电脑状态栏 -->
        <div class="desktop-status-bar" id="desktopStatusBar">
            <div class="status-left">
                <i class="fas fa-signal"></i>
                <span id="current-time">--:--</span>
            </div>
            <div class="status-center">应用系统 - 电脑版</div>
            <div class="status-right">
                <i class="fas fa-wifi"></i>
                <div class="battery">
                    <div class="battery-level"></div>
                </div>
            </div>
        </div>
        
        <!-- 页面容器 -->
        <div class="page-container" id="pageContainer">
            <!-- 主页面 -->
            <div class="page" id="mainPage">
                <div class="header-row">
                    <div class="header-left">
                        <div class="logo">
                            <i class="fas fa-rocket"></i>
                        </div>
                        <div class="title-group">
                            <h1>我的应用系统</h1>
                            <div class="subtitle">一站式访问您最喜爱的应用</div>
                        </div>
                    </div>
                    <div class="header-right">
                        <div class="year-badge">2025 最新版</div>
                    </div>
                </div>
                
                <!-- 搜索框 -->
                <div class="search-container">
                    <i class="fas fa-search"></i>
                    <input type="text" id="searchInput" placeholder="搜索应用...">
                </div>
                
                <!-- 应用图标容器 -->
                <div class="apps-container" id="appsContainer">
                    <!-- 应用图标将通过JavaScript动态生成 -->
                </div>
                
                <!-- 分页控件 -->
                <div class="pagination" id="pagination">
                    <!-- 分页按钮将通过JavaScript动态生成 -->
                </div>
                
                <!-- 电脑底部导航栏 -->
                <div class="desktop-nav">
                    <div class="nav-item" data-page="main">
                        <i class="fas fa-home"></i>
                        <span>首页</span>
                    </div>
                    <div class="nav-item" id="searchNav">
                        <i class="fas fa-search"></i>
                        <span>搜索</span>
                    </div>
                    <div class="nav-item" id="addAppNav">
                        <i class="fas fa-plus-circle"></i>
                        <span>添加应用</span>
                    </div>
                    <div class="nav-item" id="settingsNav">
                        <i class="fas fa-cog"></i>
                        <span>系统设置</span>
                    </div>
                </div>
            </div>
        </div>
        
        
    </div>
    
    <!-- 添加应用的表单部分 - 居中弹出 -->
    <div class="modal-section" id="addAppSection">
        <div class="desktop-status-bar">
            <div class="status-left">
                <i class="fas fa-arrow-left" id="backBtnAdd"></i>
                <span id="add-page-time">--:--</span>
            </div>
            <div class="status-center">添加应用</div>
            <div class="status-right">
                <i class="fas fa-wifi"></i>
                <div class="battery">
                    <div class="battery-level"></div>
                </div>
            </div>
        </div>
        
        <div style="padding: 20px;">
            <h2>添加新应用</h2>
            <div class="form-group">
                <label for="appName">应用名称</label>
                <input type="text" id="appName" placeholder="例如：微信">
            </div>
            <div class="form-group">
                <label for="appUrl">应用链接</label>
                <input type="text" id="appUrl" placeholder="例如：https://weixin.qq.com">
            </div>
            <div class="form-group">
                <label for="appColor">图标颜色</label>
                <input type="color" id="appColor" value="#6C63FF">
            </div>
            <div class="button-group">
                <button id="backBtn" class="back-btn">
                    <i class="fas fa-arrow-left"></i> 返回
                </button>
                <button id="addAppBtn">
                    <i class="fas fa-plus"></i> 添加应用
                </button>
            </div>
        </div>
    </div>
    
    <!-- 设置页面 - 居中弹出 -->
    <div class="modal-section" id="settingsSection">
        <div class="desktop-status-bar">
            <div class="status-left">
                <i class="fas fa-arrow-left" id="backBtnSettings"></i>
                <span id="settings-page-time">--:--</span>
            </div>
            <div class="status-center">系统设置</div>
            <div class="status-right">
                <i class="fas fa-wifi"></i>
                <div class="battery">
                    <div class="battery-level"></div>
                </div>
            </div>
        </div>
        
        <div style="padding: 20px;">
            <h2>系统设置</h2>
            
            <div class="settings-item">
                <label for="backgroundType">背景类型</label>
                <select id="backgroundType">
                    <option value="gradient">渐变背景</option>
                    <option value="image">自定义图片</option>
                    <option value="solid">纯色背景</option>
                </select>
            </div>
            
            <div class="settings-item" id="backgroundImageItem">
                <label for="backgroundImage">背景图片</label>
                <input type="file" id="backgroundImage" accept="image/*">
            </div>
            
            <div class="settings-item" id="backgroundColorItem">
                <label for="backgroundColor">背景颜色</label>
                <input type="color" id="backgroundColor" value="#1a2a6c">
            </div>
            
            <div class="settings-item">
                <label for="statusBarStyle">状态栏样式</label>
                <select id="statusBarStyle">
                    <option value="dark">深色</option>
                    <option value="light">浅色</option>
                    <option value="auto">自动</option>
                </select>
            </div>
            
            <div class="settings-item">
                <label for="appLimit">每页应用数量</label>
                <select id="appLimit">
                    <option value="12">12个</option>
                    <option value="16">16个</option>
                    <option value="20" selected>20个</option>
                    <option value="24">24个</option>
                </select>
            </div>
            
            <div class="settings-item">
                <label for="animationSpeed">动画速度</label>
                <select id="animationSpeed">
                    <option value="slow">慢</option>
                    <option value="normal" selected>正常</option>
                    <option value="fast">快</option>
                </select>
            </div>
            
            <div class="button-group">
                <button id="resetSettings" class="back-btn">
                    <i class="fas fa-undo"></i> 恢复默认
                </button>
                <button id="saveSettings">
                    <i class="fas fa-save"></i> 保存设置
                </button>
            </div>
        </div>
    </div>

    <script>
        // 初始应用数据
        const initialApps = [
            { name: "快手", url: "https://www.kuaishou.com", color: "#FF5000" },
            { name: "抖音", url: "https://www.douyin.com", color: "#000000" },
            { name: "QQ音乐", url: "https://y.qq.com", color: "#31C27C" },
            { name: "微信", url: "https://weixin.qq.com", color: "#07C160" },
            { name: "支付宝", url: "https://www.alipay.com", color: "#1677FF" },
            { name: "淘宝", url: "https://www.taobao.com", color: "#FF4400" },
            { name: "微博", url: "https://weibo.com", color: "#E6162D" },
            { name: "知乎", url: "https://www.zhihu.com", color: "#0084FF" },
            { name: "百度", url: "https://www.baidu.com", color: "#2932E1" },
            { name: "京东", url: "https://www.jd.com", color: "#E33333" },
            { name: "网易云音乐", url: "https://music.163.com", color: "#E60026" },
            { name: "腾讯视频", url: "https://v.qq.com", color: "#1E6BFF" },
            { name: "爱奇艺", url: "https://www.iqiyi.com", color: "#00BE06" },
            { name: "B站", url: "https://www.bilibili.com", color: "#FB7299" },
            { name: "钉钉", url: "https://www.dingtalk.com", color: "#0086FF" }
        ];
        
        // 默认设置
        const defaultSettings = {
            backgroundType: 'gradient',
            backgroundImage: null,
            backgroundColor: '#1a2a6c',
            statusBarStyle: 'dark',
            appLimit: 20,
            animationSpeed: 'normal'
        };
        
        // 从本地存储获取应用数据，如果没有则使用初始数据
        let apps = JSON.parse(localStorage.getItem('myApps')) || initialApps;
        
        // 从本地存储获取设置，如果没有则使用默认设置
        let settings = JSON.parse(localStorage.getItem('appSettings')) || defaultSettings;
        
        // DOM元素
        const appsContainer = document.getElementById('appsContainer');
        const addAppBtn = document.getElementById('addAppBtn');
        const backBtn = document.getElementById('backBtn');
        const backBtnAdd = document.getElementById('backBtnAdd');
        const backBtnSettings = document.getElementById('backBtnSettings');
        const searchInput = document.getElementById('searchInput');
        const currentTime = document.getElementById('current-time');
        const addPageTime = document.getElementById('add-page-time');
        const settingsPageTime = document.getElementById('settings-page-time');
        const mainPage = document.getElementById('mainPage');
        const addAppSection = document.getElementById('addAppSection');
        const settingsSection = document.getElementById('settingsSection');
        const overlay = document.getElementById('overlay');
        const addAppNav = document.getElementById('addAppNav');
        const settingsNav = document.getElementById('settingsNav');
        const searchNav = document.getElementById('searchNav');
        const backgroundLayer = document.getElementById('backgroundLayer');
        const desktopStatusBar = document.getElementById('desktopStatusBar');
        const pagination = document.getElementById('pagination');
        
        // 设置相关的DOM元素
        const backgroundType = document.getElementById('backgroundType');
        const backgroundImage = document.getElementById('backgroundImage');
        const backgroundColor = document.getElementById('backgroundColor');
        const statusBarStyle = document.getElementById('statusBarStyle');
        const appLimit = document.getElementById('appLimit');
        const animationSpeed = document.getElementById('animationSpeed');
        const resetSettingsBtn = document.getElementById('resetSettings');
        const saveSettingsBtn = document.getElementById('saveSettings');
        const backgroundImageItem = document.getElementById('backgroundImageItem');
        const backgroundColorItem = document.getElementById('backgroundColorItem');
        
        // 分页相关变量
        let currentPage = 1;
        let appsPerPage = parseInt(settings.appLimit);
        let filteredApps = apps;
        
        // 更新时间的函数
        function updateTime() {
            const now = new Date();
            const hours = now.getHours().toString().padStart(2, '0');
            const minutes = now.getMinutes().toString().padStart(2, '0');
            const timeString = `${hours}:${minutes}`;
            
            currentTime.textContent = timeString;
            addPageTime.textContent = timeString;
            settingsPageTime.textContent = timeString;
        }
        
        // 初始化时间并设置定时器
        updateTime();
        setInterval(updateTime, 1000);
        
        // 应用设置
        function applySettings() {
            // 应用背景设置
            if (settings.backgroundType === 'gradient') {
                backgroundLayer.style.background = 'linear-gradient(135deg, rgba(26, 42, 108, 0.7), rgba(74, 68, 181, 0.7), rgba(108, 99, 255, 0.7))';
                backgroundImageItem.style.display = 'none';
                backgroundColorItem.style.display = 'none';
            } else if (settings.backgroundType === 'image' && settings.backgroundImage) {
                backgroundLayer.style.background = `url(${settings.backgroundImage}) center/cover no-repeat`;
                backgroundImageItem.style.display = 'flex';
                backgroundColorItem.style.display = 'none';
            } else if (settings.backgroundType === 'solid') {
                backgroundLayer.style.background = settings.backgroundColor;
                backgroundImageItem.style.display = 'none';
                backgroundColorItem.style.display = 'flex';
            }
            
            // 应用状态栏样式
            if (settings.statusBarStyle === 'light') {
                desktopStatusBar.style.color = '#000';
                desktopStatusBar.style.background = 'rgba(255, 255, 255, 0.7)';
            } else if (settings.statusBarStyle === 'dark') {
                desktopStatusBar.style.color = '#fff';
                desktopStatusBar.style.background = 'rgba(0, 0, 0, 0.3)';
            } else {
                // 自动模式，根据背景亮度决定
                const bgColor = settings.backgroundColor || '#1a2a6c';
                const rgb = parseInt(bgColor.substring(1), 16);
                const r = (rgb >> 16) & 0xff;
                const g = (rgb >> 8) & 0xff;
                const b = (rgb >> 0) & 0xff;
                const brightness = (r * 299 + g * 587 + b * 114) / 1000;
                
                if (brightness > 128) {
                    desktopStatusBar.style.color = '#000';
                    desktopStatusBar.style.background = 'rgba(255, 255, 255, 0.7)';
                } else {
                    desktopStatusBar.style.color = '#fff';
                    desktopStatusBar.style.background = 'rgba(0, 0, 0, 0.3)';
                }
            }
            
            // 应用动画速度
            const speedMap = {
                'slow': '0.6s',
                'normal': '0.4s',
                'fast': '0.2s'
            };
            
            document.documentElement.style.setProperty('--animation-speed', speedMap[settings.animationSpeed] || '0.4s');
            
            // 更新每页应用数量
            appsPerPage = parseInt(settings.appLimit);
            
            // 重新渲染应用
            renderApps();
        }
        
        // 保存设置到本地存储
        function saveSettings() {
            localStorage.setItem('appSettings', JSON.stringify(settings));
            applySettings();
        }
        
        // 渲染分页控件
        function renderPagination() {
            const totalPages = Math.ceil(filteredApps.length / appsPerPage);
            pagination.innerHTML = '';
            
            if (totalPages <= 1) return;
            
            for (let i = 1; i <= totalPages; i++) {
                const pageBtn = document.createElement('button');
                pageBtn.className = `page-btn ${i === currentPage ? 'active' : ''}`;
                pageBtn.textContent = i;
                pageBtn.addEventListener('click', () => {
                    currentPage = i;
                    renderApps();
                });
                pagination.appendChild(pageBtn);
            }
        }
        
        // 渲染应用图标
        function renderApps() {
            appsContainer.innerHTML = '';
            
            // 计算当前页的应用
            const startIndex = (currentPage - 1) * appsPerPage;
            const endIndex = Math.min(startIndex + appsPerPage, filteredApps.length);
            const currentApps = filteredApps.slice(startIndex, endIndex);
            
            currentApps.forEach((app, index) => {
                const appElement = document.createElement('div');
                appElement.className = 'app-item';
                appElement.onclick = () => window.open(app.url, '_blank');
                
                // 创建图标，使用应用名称的第一个字符
                const iconChar = app.name.charAt(0);
                
                appElement.innerHTML = `
                    <div class="app-icon" style="background-color: ${app.color}">
                        ${iconChar}
                    </div>
                    <div class="app-name">${app.name}</div>
                `;
                
                appsContainer.appendChild(appElement);
            });
            
            // 如果当前页应用数量未达到每页限制，并且不是在搜索状态下，添加"添加应用"按钮
            if (currentApps.length < appsPerPage && filteredApps.length === apps.length) {
                const addAppElement = document.createElement('div');
                addAppElement.className = 'app-item add-app-item';
                addAppElement.onclick = showAddAppSection;
                
                addAppElement.innerHTML = `
                    <div class="add-icon">
                        <i class="fas fa-plus"></i>
                    </div>
                    <div class="app-name">添加应用</div>
                `;
                
                appsContainer.appendChild(addAppElement);
            }
            
            // 渲染分页控件
            renderPagination();
        }
        
        // 显示添加应用部分
        function showAddAppSection() {
            addAppSection.classList.add('active-section');
            overlay.classList.add('active');
            document.body.style.overflow = 'hidden';
        }
        
        // 隐藏添加应用部分
        function hideAddAppSection() {
            addAppSection.classList.remove('active-section');
            overlay.classList.remove('active');
            document.body.style.overflow = 'auto';
        }
        
        // 显示设置部分
        function showSettingsSection() {
            settingsSection.classList.add('active-section');
            overlay.classList.add('active');
            document.body.style.overflow = 'hidden';
        }
        
        // 隐藏设置部分
        function hideSettingsSection() {
            settingsSection.classList.remove('active-section');
            overlay.classList.remove('active');
            document.body.style.overflow = 'auto';
        }
        
        // 搜索功能
        function performSearch() {
            const searchTerm = searchInput.value.toLowerCase();
            if (searchTerm) {
                filteredApps = apps.filter(app => 
                    app.name.toLowerCase().includes(searchTerm)
                );
            } else {
                filteredApps = apps;
            }
            
            currentPage = 1;
            renderApps();
        }
        
        // 添加新应用
        addAppBtn.addEventListener('click', function() {
            const appName = document.getElementById('appName').value.trim();
            const appUrl = document.getElementById('appUrl').value.trim();
            const appColor = document.getElementById('appColor').value;
            
            if (!appName || !appUrl) {
                alert('请填写应用名称和链接！');
                return;
            }
            
            // 确保URL格式正确
            let formattedUrl = appUrl;
            if (!formattedUrl.startsWith('http://') && !formattedUrl.startsWith('https://')) {
                formattedUrl = 'https://' + formattedUrl;
            }
            
            // 添加新应用到数组
            apps.push({
                name: appName,
                url: formattedUrl,
                color: appColor
            });
            
            // 保存到本地存储
            localStorage.setItem('myApps', JSON.stringify(apps));
            
            // 重新渲染应用
            filteredApps = apps;
            currentPage = Math.ceil(apps.length / appsPerPage);
            renderApps();
            
            // 清空表单
            document.getElementById('appName').value = '';
            document.getElementById('appUrl').value = '';
            
            // 隐藏添加应用部分
            hideAddAppSection();
            
            alert('应用添加成功！');
        });
        
        // 返回按钮事件
        backBtn.addEventListener('click', hideAddAppSection);
        backBtnAdd.addEventListener('click', hideAddAppSection);
        backBtnSettings.addEventListener('click', hideSettingsSection);
        
        // 导航栏按钮事件
        addAppNav.addEventListener('click', showAddAppSection);
        settingsNav.addEventListener('click', showSettingsSection);
        searchNav.addEventListener('click', () => {
            searchInput.focus();
        });
        
        // 搜索输入事件
        searchInput.addEventListener('input', performSearch);
        
        // 遮罩层点击事件
        overlay.addEventListener('click', function() {
            hideAddAppSection();
            hideSettingsSection();
        });
        
        // 设置相关事件
        backgroundType.addEventListener('change', function() {
            settings.backgroundType = this.value;
            
            if (this.value === 'image') {
                backgroundImageItem.style.display = 'flex';
                backgroundColorItem.style.display = 'none';
            } else if (this.value === 'solid') {
                backgroundImageItem.style.display = 'none';
                backgroundColorItem.style.display = 'flex';
            } else {
                backgroundImageItem.style.display = 'none';
                backgroundColorItem.style.display = 'none';
            }
        });
        
        backgroundImage.addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    settings.backgroundImage = e.target.result;
                };
                reader.readAsDataURL(file);
            }
        });
        
        backgroundColor.addEventListener('change', function() {
            settings.backgroundColor = this.value;
        });
        
        statusBarStyle.addEventListener('change', function() {
            settings.statusBarStyle = this.value;
        });
        
        appLimit.addEventListener('change', function() {
            settings.appLimit = this.value;
            appsPerPage = parseInt(this.value);
            currentPage = 1;
            renderApps();
        });
        
        animationSpeed.addEventListener('change', function() {
            settings.animationSpeed = this.value;
        });
        
        resetSettingsBtn.addEventListener('click', function() {
            if (confirm('确定要恢复默认设置吗？')) {
                settings = {...defaultSettings};
                
                // 更新UI
                backgroundType.value = settings.backgroundType;
                backgroundColor.value = settings.backgroundColor;
                statusBarStyle.value = settings.statusBarStyle;
                appLimit.value = settings.appLimit;
                animationSpeed.value = settings.animationSpeed;
                
                saveSettings();
                alert('设置已恢复为默认值！');
            }
        });
        
        saveSettingsBtn.addEventListener('click', function() {
            saveSettings();
            alert('设置已保存！');
            hideSettingsSection();
        });
        
        // 初始化设置UI
        function initializeSettingsUI() {
            backgroundType.value = settings.backgroundType;
            backgroundColor.value = settings.backgroundColor;
            statusBarStyle.value = settings.statusBarStyle;
            appLimit.value = settings.appLimit;
            animationSpeed.value = settings.animationSpeed;
            
            if (settings.backgroundType === 'image') {
                backgroundImageItem.style.display = 'flex';
                backgroundColorItem.style.display = 'none';
            } else if (settings.backgroundType === 'solid') {
                backgroundImageItem.style.display = 'none';
                backgroundColorItem.style.display = 'flex';
            } else {
                backgroundImageItem.style.display = 'none';
                backgroundColorItem.style.display = 'none';
            }
        }
        
        // 初始化页面
        function initializePage() {
            initializeSettingsUI();
            applySettings();
            renderApps();
        }
        
        // 启动应用
        initializePage();
    </script>
</body>
</html>
