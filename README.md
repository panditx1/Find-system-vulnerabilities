<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SecureScan - Web Vulnerability Scanner</title>
    <style>
        :root {
            --primary: #2c3e50;
            --secondary: #3498db;
            --danger: #e74c3c;
            --warning: #f39c12;
            --success: #2ecc71;
            --light: #ecf0f1;
            --dark: #34495e;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f5f7fa;
            color: var(--dark);
            line-height: 1.6;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        header {
            background-color: var(--primary);
            color: white;
            padding: 20px 0;
            text-align: center;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }

        header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }

        header p {
            opacity: 0.8;
        }

        .dashboard {
            display: grid;
            grid-template-columns: 250px 1fr;
            gap: 20px;
            margin-top: 20px;
        }

        .sidebar {
            background-color: white;
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
        }

        .sidebar h3 {
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid #eee;
        }

        .scan-types {
            list-style: none;
        }

        .scan-types li {
            padding: 10px 0;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .scan-types li:hover {
            color: var(--secondary);
        }

        .scan-types li.active {
            color: var(--secondary);
            font-weight: bold;
        }

        .main-content {
            background-color: white;
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
        }

        .scan-form {
            margin-bottom: 30px;
        }

        .input-group {
            margin-bottom: 15px;
        }

        .input-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
        }

        .input-group input, 
        .input-group select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 16px;
        }

        .btn {
            background-color: var(--secondary);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s ease;
        }

        .btn:hover {
            background-color: #2980b9;
        }

        .btn-danger {
            background-color: var(--danger);
        }

        .btn-danger:hover {
            background-color: #c0392b;
        }

        .scan-results {
            margin-top: 20px;
        }

        .result-card {
            background-color: #f9f9f9;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 15px;
            border-left: 4px solid var(--warning);
        }

        .result-card.critical {
            border-left-color: var(--danger);
        }

        .result-card.high {
            border-left-color: var(--danger);
        }

        .result-card.medium {
            border-left-color: var(--warning);
        }

        .result-card.low {
            border-left-color: var(--secondary);
        }

        .result-card.info {
            border-left-color: var(--success);
        }

        .result-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }

        .result-title {
            font-weight: 600;
        }

        .result-severity {
            padding: 3px 10px;
            border-radius: 4px;
            font-size: 12px;
            font-weight: bold;
            color: white;
        }

        .severity-critical {
            background-color: var(--danger);
        }

        .severity-high {
            background-color: #e67e22;
        }

        .severity-medium {
            background-color: var(--warning);
        }

        .severity-low {
            background-color: var(--secondary);
        }

        .severity-info {
            background-color: var(--success);
        }

        .result-description {
            margin-bottom: 10px;
        }

        .result-solution {
            font-size: 14px;
            background-color: #f1f1f1;
            padding: 10px;
            border-radius: 4px;
        }

        .progress-container {
            margin: 20px 0;
            height: 10px;
            background-color: #eee;
            border-radius: 5px;
            overflow: hidden;
        }

        .progress-bar {
            height: 100%;
            background-color: var(--secondary);
            width: 0%;
            transition: width 1s ease;
        }

        .status-message {
            padding: 10px;
            border-radius: 4px;
            margin-bottom: 15px;
            text-align: center;
            display: none;
        }

        .status-running {
            background-color: #ffeaa7;
            color: #d35400;
        }

        .status-complete {
            background-color: #a8e6cf;
            color: #27ae60;
        }

        .status-error {
            background-color: #ffcdd2;
            color: #c62828;
        }

        .scan-history {
            margin-top: 30px;
        }

        .history-table {
            width: 100%;
            border-collapse: collapse;
        }

        .history-table th, 
        .history-table td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }

        .history-table th {
            background-color: #f1f1f1;
            font-weight: 600;
        }

        .history-table tr:hover {
            background-color: #f9f9f9;
        }

        .badge {
            padding: 3px 8px;
            border-radius: 4px;
            font-size: 12px;
            font-weight: bold;
            color: white;
        }

        .badge-success {
            background-color: var(--success);
        }

        .badge-warning {
            background-color: var(--warning);
        }

        .badge-danger {
            background-color: var(--danger);
        }

        @media (max-width: 768px) {
 .dashboard {
                grid-template-columns: 1fr;
            }

            .sidebar {
                margin-bottom: 20px;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>SecureScan</h1>
        <p>Your web vulnerability scanner</p>
    </header>
    <div class="container">
        <div class="dashboard">
            <div class="sidebar">
                <h3>Scan Types</h3>
                <ul class="scan-types">
                    <li class="active">Full Scan</li>
                    <li>Quick Scan</li>
                    <li>Custom Scan</li>
                </ul>
            </div>
            <div class="main-content">
                <div class="scan-form">
                    <div class="input-group">
                        <label for="url">Target URL</label>
                        <input type="text" id="url" placeholder="Enter the URL to scan">
                    </div>
                    <div class="input-group">
                        <label for="scan-type">Select Scan Type</label>
                        <select id="scan-type">
                            <option value="full">Full Scan</option>
                            <option value="quick">Quick Scan</option>
                            <option value="custom">Custom Scan</option>
                        </select>
                    </div>
                    <button class="btn" id="start-scan">Start Scan</button>
                </div>
                <div class="progress-container" id="progress-container">
                    <div class="progress-bar" id="progress-bar"></div>
                </div>
                <div class="status-message" id="status-message"></div>
                <div class="scan-results" id="scan-results"></div>
                <div class="scan-history">
                    <h3>Scan History</h3>
                    <table class="history-table">
                        <thead>
                            <tr>
                                <th>Date</th>
                                <th>URL</th>
                                <th>Scan Type</th>
                                <th>Status</th>
                            </tr>
                        </thead>
                        <tbody id="history-body">
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
    <script>
        document.getElementById('start-scan').addEventListener('click', function() {
            const url = document.getElementById('url').value;
            const scanType = document.getElementById('scan-type').value;
            const progressBar = document.getElementById('progress-bar');
            const statusMessage = document.getElementById('status-message');
            const historyBody = document.getElementById('history-body');
            let progress = 0;

            statusMessage.style.display = 'block';
            statusMessage.className = 'status-running';
            statusMessage.innerText = 'Scan is running...';

            const interval = setInterval(() => {
                progress += 10;
                progressBar.style.width = progress + '%';

                if (progress >= 100) {
                    clearInterval(interval);
                    statusMessage.className = 'status-complete';
                    statusMessage.innerText = 'Scan complete!';
                    const newRow = `<tr><td>${new Date().toLocaleString()}</td><td>${url}</td><td>${scanType}</td><td>Completed</td></tr>`;
                    historyBody.insertAdjacentHTML('beforeend', newRow);
                }
            }, 1000);
        });
    </script>
</body>
</html>
