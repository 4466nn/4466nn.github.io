# 4466nn.github.io
  ＃index.html
  <!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>现代网页设计 | 图标与链接</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: #333;
            line-height: 1.6;
            padding: 20px;
            min-height: 100vh;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
        
        header {
            text-align: center;
            padding: 40px 20px;
            color: white;
            margin-bottom: 30px;
        }
        
        header h1 {
            font-size: 2.8rem;
            margin-bottom: 15px;
            text-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        
        header p {
            font-size: 1.2rem;
            max-width: 600px;
            margin: 0 auto;
            opacity: 0.9;
        }
        
        .cards-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 25px;
            margin-bottom: 40px;
        }
        
        .card {
            background: white;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
            transition: transform 0.3s, box-shadow 0.3s;
        }
        
        .card:hover {
            transform: translateY(-10px);
            box-shadow: 0 15px 30px rgba(0,0,0,0.15);
        }
        
        .card-header {
            padding: 20px;
            background: #4a00e0;
            color: white;
            display: flex;
            align-items: center;
        }
        
        .card-header i {
            font-size: 1.8rem;
            margin-right: 15px;
        }
        
        .card-body {
            padding: 25px;
        }
        
        .card-body h3 {
            margin-bottom: 15px;
            color: #4a00e0;
        }
        
        .card-body p {
            margin-bottom: 20px;
            color: #555;
        }
        
        .btn {
            display: inline-block;
            background: #4a00e0;
            color: white;
            padding: 10px 20px;
            border-radius: 30px;
            text-decoration: none;
            font-weight: 600;
            transition: all 0.3s;
            border: 2px solid #4a00e0;
        }
        
        .btn:hover {
            background: transparent;
            color: #4a00e0;
        }
        
        .btn-outline {
            background: transparent;
            color: #4a00e0;
        }
        
        .btn-outline:hover {
            background: #4a00e0;
            color: white;
        }
        
        .social-links {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin: 40px 0;
        }
        
        .social-link {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 60px;
            height: 60px;
            background: white;
            border-radius: 50%;
            color: #4a00e0;
            font-size: 1.5rem;
            text-decoration: none;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }
        
        .social-link:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
            color: white;
        }
        
        .social-link:nth-child(1):hover { background: #3b5998; }
        .social-link:nth-child(2):hover { background: #1da1f2; }
        .social-link:nth-child(3):hover { background: #e1306c; }
        .social-link:nth-child(4):hover { background: #0077b5; }
        .social-link:nth-child(5):hover { background: #333; }
        
        footer {
            text-align: center;
            padding: 30px;
            color: white;
            margin-top: 40px;
            border-top: 1px solid rgba(255,255,255,0.2);
        }
        
        .footer-links {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin: 20px 0;
            flex-wrap: wrap;
        }
        
        .footer-links a {
            color: white;
            text-decoration: none;
            transition: opacity 0.3s;
        }
        
        .footer-links a:hover {
            opacity: 0.8;
            text-decoration: underline;
        }
        
        @media (max-width: 768px) {
            .cards-container {
                grid-template-columns: 1fr;
            }
            
            header h1 {
                font-size: 2.2rem;
            }
            
            .social-links {
                flex-wrap: wrap;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1><i class="fas fa-code"></i> 现代网页设计</h1>
            <p>探索图标与链接在网页设计中的运用</p>
        </header>
        
        <div class="cards-container">
            <div class="card">
                <div class="card-header">
                    <i class="fas fa-palette"></i>
                    <h2>设计服务</h2>
                </div>
                <div class="card-body">
                    <h3>UI/UX 设计</h3>
                    <p>我们提供专业的用户界面和用户体验设计服务，帮助您创建直观且吸引人的数字产品。</p>
                    <a href="#" class="btn">了解更多 <i class="fas fa-arrow-right"></i></a>
                </div>
            </div>
            
            <div class="card">
                <div class="card-header">
                    <i class="fas fa-laptop-code"></i>
                    <h2>网页开发</h2>
                </div>
                <div class="card-body">
                    <h3>响应式网站</h3>
                    <p>使用最新技术构建响应式网站，确保在所有设备上都能提供完美的浏览体验。</p>
                    <a href="#" class="btn">查看案例 <i class="fas fa-external-link-alt"></i></a>
                </div>
            </div>
            
            <div class="card">
                <div class="card-header">
                    <i class="fas fa-rocket"></i>
                    <h2>品牌推广</h2>
                </div>
                <div class="card-body">
                    <h3>数字营销</h3>
                    <p>通过全面的数字营销策略提升您的在线影响力，吸引更多潜在客户。</p>
                    <a href="https://github.com/4466nn/4466nn.github.io/tree/7a16ca82fb21206b05f23ccb23dedcb97f2cae61/%E6%B8%B8%E6%88%8F" class="btn btn-outline">获取方案 <i class="fas fa-chart-line"></i></a>
                </div>
            </div>
        </div>
        
        <div class="social-links">
            <a href="#" class="social-link" title="Facebook">
                <i class="fab fa-facebook-f"></i>
            </a>
            <a href="#" class="social-link" title="Twitter">
                <i class="fab fa-twitter"></i>
            </a>
            <a href="#" class="social-link" title="Instagram">
                <i class="fab fa-instagram"></i>
            </a>
            <a href="#" class="social-link" title="LinkedIn">
                <i class="fab fa-linkedin-in"></i>
            </a>
            <a href="#" class="social-link" title="GitHub">
                <i class="fab fa-github"></i>
            </a>
        </div>
        
        <footer>
            <div class="footer-links">
                <a href="#"><i class="fas fa-home"></i> 首页</a>
                <a href="#"><i class="fas fa-user"></i> 关于我们</a>
                <a href="#"><i class="fas fa-concierge-bell"></i> 服务</a>
                <a href="#"><i class="fas fa-images"></i> 作品集</a>
                <a href="#"><i class="fas fa-envelope"></i> 联系我们</a>
            </div>
            <p>&copy; 2023 现代网页设计 | 使用图标和链接增强用户体验</p>
        </footer>
    </div>
</body>
</html>
