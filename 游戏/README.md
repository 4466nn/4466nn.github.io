<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>优化版Python代码执行系统</title>
    <script src="https://cdn.jsdelivr.net/pyodide/v0.23.4/full/pyodide.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.16/codemirror.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.16/codemirror.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.16/mode/python/python.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a2a6c, #2c3e50);
            color: #333;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            background-color: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.4);
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(to right, #2c3e50, #3498db);
            color: white;
            padding: 25px 30px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }
        
        header::before {
            content: "";
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0) 70%);
            transform: rotate(30deg);
        }
        
        h1 {
            font-size: 2.8rem;
            margin-bottom: 10px;
            position: relative;
        }
        
        .subtitle {
            font-size: 1.3rem;
            opacity: 0.9;
            position: relative;
        }
        
        .main-content {
            display: flex;
            flex-wrap: wrap;
            padding: 25px;
            gap: 25px;
        }
        
        .editor-section, .output-section {
            flex: 1;
            min-width: 300px;
        }
        
        .section-title {
            font-size: 1.5rem;
            margin-bottom: 15px;
            color: #2c3e50;
            display: flex;
            align-items: center;
            gap: 10px;
            padding-bottom: 10px;
            border-bottom: 2px solid #f0f0f0;
        }
        
        .section-title i {
            color: #3498db;
        }
        
        .code-editor-container {
            border: 1px solid #ddd;
            border-radius: 10px;
            overflow: hidden;
            margin-bottom: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
            position: relative;
        }
        
        .editor-header {
            background: linear-gradient(to right, #f8f9fa, #e9ecef);
            padding: 10px 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #ddd;
        }
        
        .editor-title {
            font-weight: bold;
            color: #495057;
        }
        
        .editor-actions {
            display: flex;
            gap: 8px;
        }
        
        .CodeMirror {
            height: 350px; /* 缩小编辑器高度 */
            font-size: 16px;
            line-height: 1.5;
        }
        
        .controls {
            display: flex;
            gap: 12px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        
        button {
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        .run-btn {
            background: linear-gradient(to right, #27ae60, #2ecc71);
            color: white;
            flex: 1;
        }
        
        .run-btn:hover {
            background: linear-gradient(to right, #219653, #27ae60);
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(39, 174, 96, 0.3);
        }
        
        .clear-btn {
            background: linear-gradient(to right, #e74c3c, #c0392b);
            color: white;
        }
        
        .clear-btn:hover {
            background: linear-gradient(to right, #c0392b, #a93226);
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(231, 76, 60, 0.3);
        }
        
        .clear-editor-btn {
            background: linear-gradient(to right, #f39c12, #e67e22);
            color: white;
        }
        
        .clear-editor-btn:hover {
            background: linear-gradient(to right, #e67e22, #d35400);
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(243, 156, 18, 0.3);
        }
        
        .examples-btn {
            background: linear-gradient(to right, #3498db, #2980b9);
            color: white;
        }
        
        .examples-btn:hover {
            background: linear-gradient(to right, #2980b9, #2471a3);
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(52, 152, 219, 0.3);
        }
        
        /* 高级输出框样式 */
        .output-container {
            background-color: #1e272e;
            color: #f5f6fa;
            border-radius: 10px;
            overflow: hidden;
            min-height: 200px;
            max-height: 350px; /* 缩小输出框高度 */
            font-family: 'Courier New', monospace;
            white-space: pre-wrap;
            box-shadow: inset 0 0 15px rgba(0, 0, 0, 0.5);
            margin-bottom: 20px;
            position: relative;
            border: 1px solid #34495e;
        }
        
        .output-header {
            background: linear-gradient(to right, #2c3e50, #34495e);
            padding: 12px 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #4a6572;
        }
        
        .output-title {
            font-weight: bold;
            color: #ecf0f1;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .output-actions {
            display: flex;
            gap: 8px;
        }
        
        .fullscreen-btn {
            background: #3498db;
            color: white;
            padding: 6px 10px;
            border-radius: 4px;
            font-size: 0.9rem;
        }
        
        .fullscreen-btn:hover {
            background: #2980b9;
        }
        
        .output-content {
            padding: 15px;
            max-height: 270px; /* 调整内容高度 */
            overflow-y: auto;
        }
        
        .output-content::-webkit-scrollbar {
            width: 8px;
        }
        
        .output-content::-webkit-scrollbar-track {
            background: #2c3e50;
            border-radius: 4px;
        }
        
        .output-content::-webkit-scrollbar-thumb {
            background: #3498db;
            border-radius: 4px;
        }
        
        .output-content::-webkit-scrollbar-thumb:hover {
            background: #2980b9;
        }
        
        .output-line {
            margin-bottom: 5px;
            line-height: 1.4;
        }
        
        .output-line.error {
            color: #e74c3c;
        }
        
        .output-line.success {
            color: #2ecc71;
        }
        
        .output-line.info {
            color: #3498db;
        }
        
        .input-section {
            margin-top: 20px;
        }
        
        .input-section label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #2c3e50;
        }
        
        .input-section textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-family: 'Courier New', monospace;
            resize: vertical;
            min-height: 80px;
            box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.05);
        }
        
        .status {
            padding: 15px;
            border-radius: 8px;
            margin-top: 20px;
            text-align: center;
            font-weight: bold;
            background-color: #f8f9fa;
            border: 1px solid #e9ecef;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        
        .loading {
            background-color: #fff3cd;
            color: #856404;
            border-color: #ffeaa7;
        }
        
        .success {
            background-color: #d1ecf1;
            color: #0c5460;
            border-color: #bee5eb;
        }
        
        .error {
            background-color: #f8d7da;
            color: #721c24;
            border-color: #f5c6cb;
        }
        
        .examples-panel {
            display: none;
            background-color: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
            border: 1px solid #e9ecef;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
        }
        
        .examples-panel h3 {
            margin-bottom: 15px;
            color: #2c3e50;
            padding-bottom: 10px;
            border-bottom: 1px solid #dee2e6;
        }
        
        .example-item {
            padding: 15px;
            background-color: white;
            border-radius: 8px;
            margin-bottom: 12px;
            cursor: pointer;
            transition: all 0.2s ease;
            border: 1px solid #e9ecef;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        
        .example-item:hover {
            background-color: #e9f7fe;
            border-color: #3498db;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(52, 152, 219, 0.2);
        }
        
        .example-icon {
            font-size: 1.5rem;
            color: #3498db;
        }
        
        .example-content {
            flex: 1;
        }
        
        .example-title {
            font-weight: bold;
            margin-bottom: 5px;
            color: #2c3e50;
        }
        
        .example-desc {
            font-size: 0.9rem;
            color: #7f8c8d;
        }
        
        footer {
            text-align: center;
            padding: 20px;
            color: #7f8c8d;
            font-size: 0.9rem;
            border-top: 1px solid #e9ecef;
            background-color: #f8f9fa;
        }
        
        /* 移动端样式 */
        @media (max-width: 768px) {
            .main-content {
                flex-direction: column;
            }
            
            h1 {
                font-size: 2.2rem;
            }
            
            .controls {
                flex-direction: column;
                order: -1; /* 将按钮移到顶部 */
                margin-bottom: 20px;
                width: 100%;
            }
            
            button {
                width: 100%;
                justify-content: center;
            }
            
            .mobile-controls {
                display: flex;
                flex-direction: column;
                gap: 12px;
                width: 100%;
            }
            
            .editor-section {
                width: 100%;
            }
        }
        
        /* 全屏模式样式 */
        .output-fullscreen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1000;
            max-height: 100% !important;
            border-radius: 0;
        }
        
        .output-fullscreen .output-content {
            max-height: calc(100vh - 60px) !important;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1><i class="fas fa-code"></i> 优化版Python代码执行系统</h1>
            <div class="subtitle">在浏览器中直接运行Python代码，支持丰富的库和高级功能</div>
        </header>
        
        <div class="main-content">
            <div class="editor-section">
                <!-- 移动端按钮位置 -->
                <div class="mobile-controls">
                    <div class="controls">
                        <button class="run-btn" id="run-btn">
                            <i class="fas fa-play-circle"></i> 运行代码
                        </button>
                        <button class="clear-btn" id="clear-btn">
                            <i class="fas fa-trash-alt"></i> 清除输出
                        </button>
                        <button class="examples-btn" id="examples-btn">
                            <i class="fas fa-book"></i> 代码示例
                        </button>
                    </div>
                </div>
                
                <div class="section-title">
                    <i class="fas fa-edit"></i> 代码编辑器
                </div>
                
                <div class="code-editor-container">
                    <div class="editor-header">
                        <div class="editor-title">
                            <i class="fas fa-file-code"></i> main.py
                        </div>
                        <div class="editor-actions">
                            <button class="clear-editor-btn" id="clear-editor-btn" title="清空编辑器">
                                <i class="fas fa-eraser"></i>
                            </button>
                        </div>
                    </div>
                    <div class="code-editor">
                        <textarea id="code" placeholder="在这里输入您的Python代码..."># 欢迎使用优化版Python代码执行系统
# 这是一个计算斐波那契数列的示例

def fibonacci(n):
    if n <= 1:
        return n
    else:
        return fibonacci(n-1) + fibonacci(n-2)

print("斐波那契数列前10项:")
for i in range(10):
    print(f"F({i}) = {fibonacci(i)}")</textarea>
                    </div>
                </div>
                
                <div class="examples-panel" id="examples-panel">
                    <h3><i class="fas fa-lightbulb"></i> Python代码示例</h3>
                    <div class="example-item" data-example="fibonacci">
                        <div class="example-icon">
                            <i class="fas fa-calculator"></i>
                        </div>
                        <div class="example-content">
                            <div class="example-title">斐波那契数列</div>
                            <div class="example-desc">计算斐波那契数列前N项</div>
                        </div>
                    </div>
                    <div class="example-item" data-example="prime">
                        <div class="example-icon">
                            <i class="fas fa-sort-numeric-up"></i>
                        </div>
                        <div class="example-content">
                            <div class="example-title">质数判断</div>
                            <div class="example-desc">判断一个数是否为质数</div>
                        </div>
                    </div>
                    <div class="example-item" data-example="sort">
                        <div class="example-icon">
                            <i class="fas fa-sort-amount-down"></i>
                        </div>
                        <div class="example-content">
                            <div class="example-title">排序算法</div>
                            <div class="example-desc">实现冒泡排序算法</div>
                        </div>
                    </div>
                    <div class="example-item" data-example="math">
                        <div class="example-icon">
                            <i class="fas fa-square-root-alt"></i>
                        </div>
                        <div class="example-content">
                            <div class="example-title">数学计算</div>
                            <div class="example-desc">使用math库进行数学计算</div>
                        </div>
                    </div>
                    <div class="example-item" data-example="file">
                        <div class="example-icon">
                            <i class="fas fa-file-alt"></i>
                        </div>
                        <div class="example-content">
                            <div class="example-title">文件操作模拟</div>
                            <div class="example-desc">模拟文件读写操作</div>
                        </div>
                    </div>
                    <div class="example-item" data-example="graphics">
                        <div class="example-icon">
                            <i class="fas fa-chart-bar"></i>
                        </div>
                        <div class="example-content">
                            <div class="example-title">图形绘制</div>
                            <div class="example-desc">使用turtle库绘制图形</div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="output-section">
                <div class="section-title">
                    <i class="fas fa-terminal"></i> 运行结果
                </div>
                
                <div class="output-container" id="output-container">
                    <div class="output-header">
                        <div class="output-title">
                            <i class="fas fa-desktop"></i> 控制台输出
                        </div>
                        <div class="output-actions">
                            <button class="fullscreen-btn" id="fullscreen-btn" title="全屏显示">
                                <i class="fas fa-expand"></i> 全屏
                            </button>
                            <button class="clear-btn" id="clear-output-btn" title="清除输出">
                                <i class="fas fa-trash-alt"></i>
                            </button>
                        </div>
                    </div>
                    <div class="output-content" id="output">
                        <div class="output-line info">系统就绪，点击"运行代码"执行Python程序</div>
                    </div>
                </div>
                
                <div class="input-section">
                    <label for="stdin">
                        <i class="fas fa-keyboard"></i> 标准输入 (stdin) - 每行一个输入
                    </label>
                    <textarea id="stdin" placeholder="如果需要输入数据，请在此输入..."></textarea>
                </div>
                
                <div class="status" id="status">
                    <i class="fas fa-check-circle"></i>
                    <span>系统就绪，点击"运行代码"执行Python程序</span>
                </div>
            </div>
        </div>
        
        <footer>
            <p><i class="fas fa-code-branch"></i> 基于Pyodide构建 | 支持Python 3.9 | 常用库: math, random, datetime, json, re等</p>
        </footer>
    </div>

    <script>
        // 初始化代码编辑器
        const editor = CodeMirror.fromTextArea(document.getElementById('code'), {
            mode: 'python',
            theme: 'default',
            lineNumbers: true,
            indentUnit: 4,
            indentWithTabs: false,
            lineWrapping: true,
            autofocus: true,
            matchBrackets: true,
            autoCloseBrackets: true
        });
        
        let pyodide;
        let isLoading = false;
        let isFullscreen = false;
        
        // 初始化Pyodide
        async function initializePyodide() {
            if (isLoading) return;
            
            isLoading = true;
            updateStatus("正在加载Pyodide环境，请稍候...", "loading");
            
            try {
                pyodide = await loadPyodide({
                    indexURL: "https://cdn.jsdelivr.net/pyodide/v0.23.4/full/"
                });
                
                updateStatus("Pyodide加载完成！现在可以运行Python代码了。", "success");
                document.getElementById('run-btn').disabled = false;
            } catch (error) {
                updateStatus("加载Pyodide时出错: " + error.message, "error");
            }
            
            isLoading = false;
        }
        
        // 更新状态信息
        function updateStatus(message, type) {
            const statusEl = document.getElementById('status');
            const icon = statusEl.querySelector('i');
            
            // 移除所有类
            statusEl.className = 'status';
            icon.className = '';
            
            // 添加新类
            statusEl.classList.add(type);
            
            // 设置图标
            if (type === 'loading') {
                icon.className = 'fas fa-spinner fa-spin';
            } else if (type === 'success') {
                icon.className = 'fas fa-check-circle';
            } else if (type === 'error') {
                icon.className = 'fas fa-exclamation-circle';
            }
            
            statusEl.querySelector('span').textContent = message;
        }
        
        // 运行Python代码
        async function runPythonCode() {
            if (!pyodide) {
                updateStatus("Pyodide尚未加载完成，请稍后再试。", "error");
                return;
            }
            
            const code = editor.getValue();
            const stdin = document.getElementById('stdin').value;
            const outputEl = document.getElementById('output');
            
            if (!code.trim()) {
                updateStatus("请输入Python代码", "error");
                return;
            }
            
            updateStatus("正在执行代码...", "loading");
            
            try {
                // 清空输出区域
                outputEl.innerHTML = '';
                
                // 设置标准输入
                if (stdin) {
                    const inputLines = stdin.split('\n');
                    let inputIndex = 0;
                    
                    pyodide.setStdin({
                        stdin: () => {
                            if (inputIndex < inputLines.length) {
                                return inputLines[inputIndex++] + '\n';
                            }
                            return null;
                        }
                    });
                }
                
                // 捕获输出
                let hasOutput = false;
                
                pyodide.setStdout({
                    batched: (text) => {
                        hasOutput = true;
                        addOutputLine(text, 'normal');
                    }
                });
                
                pyodide.setStderr({
                    batched: (text) => {
                        hasOutput = true;
                        addOutputLine(text, 'error');
                    }
                });
                
                // 执行代码
                await pyodide.runPythonAsync(code);
                
                if (!hasOutput) {
                    addOutputLine("程序执行完成，但没有输出。", 'info');
                } else {
                    updateStatus("代码执行成功！", "success");
                }
            } catch (error) {
                addOutputLine(error.toString(), 'error');
                updateStatus("代码执行出错，请检查代码语法。", "error");
            }
        }
        
        // 添加输出行
        function addOutputLine(text, type) {
            const outputEl = document.getElementById('output');
            const line = document.createElement('div');
            line.className = `output-line ${type}`;
            line.textContent = text;
            outputEl.appendChild(line);
            
            // 自动滚动到底部
            outputEl.scrollTop = outputEl.scrollHeight;
        }
        
        // 清除输出
        function clearOutput() {
            document.getElementById('output').innerHTML = '<div class="output-line info">输出已清除</div>';
            document.getElementById('stdin').value = '';
            updateStatus("输出已清除", "success");
        }
        
        // 清空编辑器
        function clearEditor() {
            editor.setValue('');
            updateStatus("编辑器已清空", "success");
        }
        
        // 显示/隐藏代码示例面板
        function toggleExamples() {
            const panel = document.getElementById('examples-panel');
            panel.style.display = panel.style.display === 'block' ? 'none' : 'block';
        }
        
        // 切换全屏模式
        function toggleFullscreen() {
            const outputContainer = document.getElementById('output-container');
            const fullscreenBtn = document.getElementById('fullscreen-btn');
            const icon = fullscreenBtn.querySelector('i');
            
            if (!isFullscreen) {
                outputContainer.classList.add('output-fullscreen');
                icon.className = 'fas fa-compress';
                fullscreenBtn.innerHTML = '<i class="fas fa-compress"></i> 退出全屏';
                isFullscreen = true;
            } else {
                outputContainer.classList.remove('output-fullscreen');
                icon.className = 'fas fa-expand';
                fullscreenBtn.innerHTML = '<i class="fas fa-expand"></i> 全屏';
                isFullscreen = false;
            }
        }
        
        // 加载代码示例
        function loadExample(exampleName) {
            const examples = {
                fibonacci: `# 斐波那契数列示例
def fibonacci(n):
    if n <= 1:
        return n
    else:
        return fibonacci(n-1) + fibonacci(n-2)

print("斐波那契数列前10项:")
for i in range(10):
    print(f"F({i}) = {fibonacci(i)}")`,
                
                prime: `# 质数判断示例
def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True

# 测试质数判断
test_numbers = [2, 3, 4, 17, 25, 29, 100]
for num in test_numbers:
    if is_prime(num):
        print(f"{num} 是质数")
    else:
        print(f"{num} 不是质数")`,
                
                sort: `# 冒泡排序示例
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr

# 测试排序
numbers = [64, 34, 25, 12, 22, 11, 90]
print("排序前:", numbers)
sorted_numbers = bubble_sort(numbers.copy())
print("排序后:", sorted_numbers)`,
                
                math: `# 数学计算示例
import math

# 计算圆的面积和周长
radius = 5
area = math.pi * radius ** 2
circumference = 2 * math.pi * radius

print(f"半径为 {radius} 的圆:")
print(f"面积: {area:.2f}")
print(f"周长: {circumference:.2f}")

# 计算三角函数
angle_degrees = 45
angle_radians = math.radians(angle_degrees)
print(f"\\n{angle_degrees}度的三角函数值:")
print(f"sin: {math.sin(angle_radians):.4f}")
print(f"cos: {math.cos(angle_radians):.4f}")
print(f"tan: {math.tan(angle_radians):.4f}")`,
                
                file: `# 文件操作模拟示例
# 注意：在浏览器环境中，我们模拟文件操作

# 模拟写入文件
file_content = """这是第一行
这是第二行
这是第三行"""

print("文件内容:")
print(file_content)

# 模拟读取文件
lines = file_content.split('\\n')
print("\\n逐行读取文件:")
for i, line in enumerate(lines, 1):
    print(f"第{i}行: {line}")

# 模拟追加内容
new_content = "这是新添加的内容"
file_content += '\\n' + new_content
print("\\n追加内容后的文件:")
print(file_content)`,
                
                graphics: `# 图形绘制示例 (使用turtle库)
import turtle

# 创建一个画布和画笔
screen = turtle.Screen()
screen.bgcolor("white")
pen = turtle.Turtle()
pen.speed(2)

# 绘制一个正方形
for _ in range(4):
    pen.forward(100)
    pen.right(90)

# 绘制一个圆形
pen.penup()
pen.goto(150, 0)
pen.pendown()
pen.circle(50)

# 绘制一个三角形
pen.penup()
pen.goto(-150, 0)
pen.pendown()
for _ in range(3):
    pen.forward(100)
    pen.left(120)

print("图形绘制完成！")
print("注意：在浏览器中，turtle图形可能无法显示，但代码可以正常运行。")`
            };
            
            if (examples[exampleName]) {
                editor.setValue(examples[exampleName]);
                document.getElementById('examples-panel').style.display = 'none';
                updateStatus(`已加载"${exampleName}"示例`, "success");
            }
        }
        
        // 事件监听
        document.getElementById('run-btn').addEventListener('click', runPythonCode);
        document.getElementById('clear-btn').addEventListener('click', clearOutput);
        document.getElementById('clear-editor-btn').addEventListener('click', clearEditor);
        document.getElementById('clear-output-btn').addEventListener('click', clearOutput);
        document.getElementById('examples-btn').addEventListener('click', toggleExamples);
        document.getElementById('fullscreen-btn').addEventListener('click', toggleFullscreen);
        
        // 为示例项添加点击事件
        document.querySelectorAll('.example-item').forEach(item => {
            item.addEventListener('click', () => {
                loadExample(item.getAttribute('data-example'));
            });
        });
        
        // 初始化
        initializePyodide();
    </script>
</body>
</html>
