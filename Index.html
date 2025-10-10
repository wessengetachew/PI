<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modular Sieve: π and ζ(2n) Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            color: #fff;
            min-height: 100vh;
            padding: 20px;
        }
        
        .tooltip {
            position: relative;
            display: inline-block;
            cursor: help;
            color: #ffd700;
            margin-left: 5px;
            font-weight: bold;
        }
        
        .tooltip .tooltiptext {
            visibility: hidden;
            width: 300px;
            background-color: rgba(0, 0, 0, 0.95);
            color: #fff;
            text-align: left;
            border-radius: 8px;
            padding: 15px;
            position: absolute;
            z-index: 1000;
            bottom: 125%;
            left: 50%;
            margin-left: -150px;
            opacity: 0;
            transition: opacity 0.3s;
            font-size: 0.9em;
            line-height: 1.4;
            border: 1px solid #ffd700;
            box-shadow: 0 5px 20px rgba(0,0,0,0.5);
        }
        
        .tooltip:hover .tooltiptext {
            visibility: visible;
            opacity: 1;
        }
        
        .progress-container {
            width: 100%;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            overflow: hidden;
            margin: 15px 0;
            display: none;
        }
        
        .progress-bar {
            height: 25px;
            background: linear-gradient(90deg, #4ecdc4, #44a08d);
            width: 0%;
            transition: width 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.9em;
            font-weight: bold;
        }
        
        .export-buttons {
            display: flex;
            gap: 10px;
            margin-top: 15px;
            flex-wrap: wrap;
        }
        
        .export-btn {
            padding: 8px 15px;
            background: linear-gradient(45deg, #44a08d, #4ecdc4);
            border: none;
            border-radius: 6px;
            color: white;
            cursor: pointer;
            font-size: 0.9em;
            flex: 1;
            min-width: 120px;
        }
        
        .export-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(68, 160, 141, 0.4);
        }
        
        .presets {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 10px;
            margin-bottom: 15px;
        }
        
        .preset-btn {
            padding: 10px;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: white;
            cursor: pointer;
            font-size: 0.85em;
            transition: all 0.3s;
        }
        
        .preset-btn:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: scale(1.05);
        }
        
        .formula-display {
            background: rgba(0, 0, 0, 0.3);
            padding: 15px;
            border-radius: 8px;
            margin: 15px 0;
            font-family: 'Courier New', monospace;
            font-size: 1.1em;
            text-align: center;
            border: 1px solid rgba(255, 215, 0, 0.3);
        }
        
        .comparison-table {
            width: 100%;
            margin-top: 20px;
            border-collapse: collapse;
        }
        
        .comparison-table th,
        .comparison-table td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .comparison-table th {
            background: rgba(255, 215, 0, 0.2);
            font-weight: bold;
        }
        
        .convergence-chart {
            margin-top: 30px;
            background: rgba(255, 255, 255, 0.05);
            padding: 25px;
            border-radius: 15px;
        }
        
        .tour-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 9999;
            display: none;
            align-items: center;
            justify-content: center;
        }
        
        .tour-content {
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            padding: 30px;
            border-radius: 15px;
            max-width: 600px;
            border: 2px solid #ffd700;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
        }
        
        .tour-buttons {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }
        
        .tour-btn {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
        }
        
        .help-icon {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 50px;
            height: 50px;
            background: linear-gradient(45deg, #ffd700, #ffed4e);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 24px;
            font-weight: bold;
            color: #1e3c72;
            box-shadow: 0 5px 20px rgba(255, 215, 0, 0.4);
            transition: transform 0.3s;
            z-index: 1000;
        }
        
        .help-icon:hover {
            transform: scale(1.1);
        }
        
        .keyboard-shortcuts {
            font-size: 0.85em;
            opacity: 0.7;
            margin-top: 10px;
        }
        
        .error-message {
            background: rgba(255, 107, 107, 0.2);
            border: 1px solid #ff6b6b;
            padding: 15px;
            border-radius: 8px;
            margin: 15px 0;
            display: none;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
        }
        
        h1 {
            text-align: center;
            font-size: 2.5em;
            margin-bottom: 10px;
            background: linear-gradient(45deg, #ffd700, #ffed4e);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .subtitle {
            text-align: center;
            font-size: 1.1em;
            opacity: 0.9;
            margin-bottom: 30px;
        }
        
        .controls {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .control-group {
            background: rgba(255, 255, 255, 0.05);
            padding: 20px;
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .control-group h3 {
            margin-bottom: 15px;
            color: #ffd700;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
        }
        
        input, select, button {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            margin-bottom: 15px;
            background: rgba(255, 255, 255, 0.9);
            color: #333;
        }
        
        button {
            background: linear-gradient(45deg, #ff6b6b, #ee5a52);
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(238, 90, 82, 0.3);
        }
        
        .results {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }
        
        .result-card {
            background: rgba(255, 255, 255, 0.08);
            padding: 25px;
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .result-card h4 {
            color: #ffd700;
            margin-bottom: 15px;
            font-size: 1.3em;
        }
        
        .value {
            font-family: 'Courier New', monospace;
            font-size: 1.2em;
            background: rgba(0, 0, 0, 0.3);
            padding: 10px;
            border-radius: 8px;
            margin: 10px 0;
            word-break: break-all;
        }
        
        .error-info {
            font-size: 0.9em;
            opacity: 0.8;
            margin-top: 10px;
        }
        
        .gap-analysis {
            margin-top: 30px;
            background: rgba(255, 255, 255, 0.05);
            padding: 25px;
            border-radius: 15px;
        }
        
        .gap-analysis h3 {
            color: #ffd700;
            margin-bottom: 20px;
        }
        
        .gap-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
        }
        
        .gap-item {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            text-align: center;
        }
        
        .gap-value {
            font-size: 1.1em;
            font-weight: bold;
            color: #4ecdc4;
        }
        
        .channel-analysis {
            margin-top: 30px;
            background: rgba(255, 255, 255, 0.05);
            padding: 25px;
            border-radius: 15px;
            max-height: 600px;
            overflow-y: auto;
        }
        
        .channel-analysis::-webkit-scrollbar {
            width: 8px;
        }
        
        .channel-analysis::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 4px;
        }
        
        .channel-analysis::-webkit-scrollbar-thumb {
            background: rgba(255, 215, 0, 0.5);
            border-radius: 4px;
        }
        
        .channel-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
            gap: 10px;
            max-height: 400px;
            overflow-y: auto;
            padding-right: 10px;
        }
        
        .channel-grid::-webkit-scrollbar {
            width: 8px;
        }
        
        .channel-grid::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 4px;
        }
        
        .channel-grid::-webkit-scrollbar-thumb {
            background: rgba(255, 215, 0, 0.5);
            border-radius: 4px;
        }
        
        .channel-grid::-webkit-scrollbar-thumb:hover {
            background: rgba(255, 215, 0, 0.7);
        }
        
        .channel-item {
            background: rgba(255, 255, 255, 0.1);
            padding: 12px;
            border-radius: 8px;
            text-align: center;
            font-size: 0.9em;
        }
        
        .loading {
            text-align: center;
            font-style: italic;
            opacity: 0.7;
        }
        
        .chart-container {
            margin-top: 30px;
            background: rgba(255, 255, 255, 0.05);
            padding: 25px;
            border-radius: 15px;
        }
        
        .chart-container h3 {
            color: #ffd700;
            margin-bottom: 20px;
            text-align: center;
        }
        
        #channelChart {
            width: 100%;
            height: 400px;
            max-height: 400px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
        }
        
        .chart-legend {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 15px;
            margin-top: 15px;
            font-size: 0.9em;
            max-height: 200px;
            overflow-y: auto;
            padding: 10px;
        }
        
        .chart-legend::-webkit-scrollbar {
            width: 6px;
        }
        
        .chart-legend::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 3px;
        }
        
        .chart-legend::-webkit-scrollbar-thumb {
            background: rgba(255, 215, 0, 0.5);
            border-radius: 3px;
        }
        
        .legend-item {
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .legend-color {
            width: 12px;
            height: 12px;
            border-radius: 2px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Modular Sieve Calculator</h1>
        <div class="subtitle">Computing π and ζ(2n) via Gap-Class and Residue-Channel Decompositions</div>
        
        <div class="error-message" id="error-message"></div>
        
        <div class="presets">
            <button class="preset-btn" onclick="loadPreset('quick')">Quick Demo<br>(ε=0.01)</button>
            <button class="preset-btn" onclick="loadPreset('accurate')">High Accuracy<br>(ε=0.0001)</button>
            <button class="preset-btn" onclick="loadPreset('ultra')">Ultra Precise<br>(ε=0.00001)</button>
            <button class="preset-btn" onclick="loadPreset('compare')">Compare All<br>Methods</button>
        </div>
        
        <div class="keyboard-shortcuts">
            ⌨️ Shortcuts: <strong>Ctrl+Enter</strong> Calculate | <strong>Ctrl+E</strong> Export | <strong>Ctrl+H</strong> Help
        </div>
        
        <div class="controls">
            <div class="control-group">
                <h3>Target Accuracy</h3>
                <label for="epsilon">Relative Error (ε):</label>
                <input type="number" id="epsilon" value="0.001" step="0.0001" min="0.0001" max="0.1">
                
                <label for="constant">Constant to Compute:</label>
                <select id="constant">
                    <option value="pi">π (from ζ(2))</option>
                    <option value="zeta4">ζ(4)</option>
                    <option value="zeta6">ζ(6)</option>
                    <option value="zeta8">ζ(8)</option>
                    <option value="zeta10">ζ(10)</option>
                </select>
            </div>
            
            <div class="control-group">
                <h3>Computation Method
                    <span class="tooltip">ℹ️
                        <span class="tooltiptext">
                            <strong>Standard:</strong> Classic Euler product<br>
                            <strong>Gap-Class:</strong> Group primes by gap sizes (p_{n+1} - p_n)<br>
                            <strong>Residue Channels:</strong> Group primes by residue mod k<br>
                            <strong>Combined:</strong> Both decompositions
                        </span>
                    </span>
                </h3>
                <label for="method">Decomposition:</label>
                <select id="method">
                    <option value="standard">Standard Euler Product</option>
                    <option value="gap">Gap-Class Analysis</option>
                    <option value="residue">Residue Channels</option>
                    <option value="both">Gap + Residue Combined</option>
                </select>
                
                <div id="modulus-control" style="display: none;">
                    <label for="modulus">Modulus (k):
                        <span class="tooltip">ℹ️
                            <span class="tooltiptext">
                                Primes are grouped by their remainder when divided by k. Common choices:<br>
                                <strong>k=6:</strong> Primes ≡ 1,5 (mod 6)<br>
                                <strong>k=12:</strong> Primes ≡ 1,5,7,11 (mod 12)<br>
                                <strong>k=30:</strong> 8 residue classes (Euler's φ(30)=8)<br>
                                <strong>Higher k:</strong> More refined decomposition
                            </span>
                        </span>
                    </label>
                    <input type="number" id="modulus" value="30" min="3" max="210" step="1">
                    <div style="font-size: 0.85em; opacity: 0.8; margin-top: 5px;" id="phi-info"></div>
                </div>
                
                <div class="progress-container" id="progress-container">
                    <div class="progress-bar" id="progress-bar">0%</div>
                </div>
                
                <button onclick="compute()">Calculate</button>
                
                <div class="export-buttons">
                    <button class="export-btn" onclick="exportCSV()">📊 Export CSV</button>
                    <button class="export-btn" onclick="exportJSON()">📄 Export JSON</button>
                    <button class="export-btn" onclick="exportImage()">🖼️ Save Chart</button>
                </div>
            </div>
        </div>
        
        <div id="results" class="results" style="display: none;">
            <div class="result-card">
                <h4>Computed Value</h4>
                <div id="computed-value" class="value"></div>
                <div id="error-bound" class="error-info"></div>
                <div id="prime-count" class="error-info"></div>
            </div>
            
            <div class="result-card">
                <h4>Exact Reference</h4>
                <div id="exact-value" class="value"></div>
                <div id="actual-error" class="error-info"></div>
            </div>
        </div>
        
        <div id="intermediate-section" class="gap-analysis" style="display: none;">
            <h3>Mathematical Structure 
                <span class="tooltip">ℹ️
                    <span class="tooltiptext">
                        Shows the exact mathematical form of the truncated product before transformation. This reveals what constant you're actually computing!
                    </span>
                </span>
            </h3>
            <div class="result-card" style="background: rgba(255, 215, 0, 0.1); border: 2px solid rgba(255, 215, 0, 0.3);">
                <h4>Intermediate Value</h4>
                <div id="intermediate-product" class="value"></div>
                <div id="intermediate-formula" class="error-info" style="margin-top: 15px; font-size: 1.05em;"></div>
                <div id="intermediate-explanation" class="error-info" style="margin-top: 10px; line-height: 1.6;"></div>
            </div>
        </div>
        
        <div id="gap-analysis" class="gap-analysis" style="display: none;">
            <h3>Gap-Class Decomposition</h3>
            <div id="gap-grid" class="gap-grid"></div>
        </div>
        
        <div id="channel-analysis" class="channel-analysis" style="display: none;">
            <h3 id="channel-title">Residue Channels</h3>
            <div id="channel-grid" class="channel-grid"></div>
        </div>
        
        <div id="chart-section" class="chart-container" style="display: none;">
            <h3 id="chart-title">Residue Channel Contributions</h3>
            <canvas id="channelChart"></canvas>
            <div class="chart-legend" id="chartLegend"></div>
        </div>
        
        <div id="convergence-section" class="convergence-chart" style="display: none;">
            <h3>Convergence Analysis</h3>
            <canvas id="convergenceChart"></canvas>
        </div>
        
        <div id="comparison-section" class="gap-analysis" style="display: none;">
            <h3>Method Comparison</h3>
            <table class="comparison-table" id="comparison-table"></table>
        </div>
    </div>
    
    <div class="help-icon" onclick="showTour()" aria-label="Help and Tutorial">?</div>
    
    <div class="tour-overlay" id="tour-overlay">
        <div class="tour-content">
            <h2 style="color: #ffd700; margin-bottom: 15px;">Welcome to Modular Sieve Calculator!</h2>
            <div id="tour-text"></div>
            <div class="tour-buttons">
                <button class="tour-btn" style="background: #666; color: white;" onclick="closeTour()">Skip</button>
                <button class="tour-btn" style="background: #ffd700; color: #1e3c72;" onclick="nextTourStep()">Next</button>
            </div>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
    <script>
        // Global state
        let computationCache = {};
        let currentResults = null;
        let tourStep = 0;
        
        const tourSteps = [
            {
                title: "Mathematical Foundation",
                text: "This calculator uses the <strong>Euler Product Formula</strong> to compute mathematical constants:<br><br>ζ(s) = ∏<sub>p prime</sub> (1 - p<sup>-s</sup>)<sup>-1</sup><br><br>We can compute π and ζ(2n) by truncating this infinite product."
            },
            {
                title: "Accuracy Control",
                text: "Set your desired <strong>relative error (ε)</strong> to control accuracy. The calculator automatically determines how many primes are needed to guarantee this error bound."
            },
            {
                title: "Decomposition Methods",
                text: "<strong>Gap-Class:</strong> Groups primes by their spacing<br><strong>Residue Channels:</strong> Groups primes by their remainder mod 30<br><br>These reveal hidden structure in prime distribution!"
            },
            {
                title: "Export & Analysis",
                text: "Export your results as CSV or JSON, save charts as images, and compare different methods side-by-side. Use keyboard shortcuts for faster workflow!"
            }
        ];
        
        // Keyboard shortcuts
        document.addEventListener('keydown', function(e) {
            if (e.ctrlKey || e.metaKey) {
                if (e.key === 'Enter') {
                    e.preventDefault();
                    compute();
                } else if (e.key === 'e') {
                    e.preventDefault();
                    exportJSON();
                } else if (e.key === 'h') {
                    e.preventDefault();
                    showTour();
                }
            }
        });
        
        // Tour functions
        function showTour() {
            tourStep = 0;
            document.getElementById('tour-overlay').style.display = 'flex';
            updateTourContent();
        }
        
        function closeTour() {
            document.getElementById('tour-overlay').style.display = 'none';
        }
        
        function nextTourStep() {
            tourStep++;
            if (tourStep >= tourSteps.length) {
                closeTour();
            } else {
                updateTourContent();
            }
        }
        
        function updateTourContent() {
            const step = tourSteps[tourStep];
            document.getElementById('tour-text').innerHTML = `
                <h3 style="color: #4ecdc4; margin-bottom: 10px;">${step.title}</h3>
                <p style="line-height: 1.6;">${step.text}</p>
                <p style="margin-top: 15px; opacity: 0.7; font-size: 0.9em;">Step ${tourStep + 1} of ${tourSteps.length}</p>
            `;
        }
        
        // Preset configurations
        function loadPreset(type) {
            const epsilonInput = document.getElementById('epsilon');
            const methodSelect = document.getElementById('method');
            
            switch(type) {
                case 'quick':
                    epsilonInput.value = 0.01;
                    methodSelect.value = 'standard';
                    break;
                case 'accurate':
                    epsilonInput.value = 0.0001;
                    methodSelect.value = 'residue';
                    break;
                case 'ultra':
                    epsilonInput.value = 0.00001;
                    methodSelect.value = 'both';
                    break;
                case 'compare':
                    epsilonInput.value = 0.001;
                    compareAllMethods();
                    return;
            }
            compute();
        }
        
        // Input validation
        function validateInputs() {
            const epsilon = parseFloat(document.getElementById('epsilon').value);
            const errors = [];
            
            if (isNaN(epsilon) || epsilon <= 0) {
                errors.push('Epsilon must be a positive number');
            }
            if (epsilon > 0.1) {
                errors.push('Epsilon too large (max 0.1) - results may be inaccurate');
            }
            if (epsilon < 0.00001) {
                errors.push('Epsilon too small (min 0.00001) - computation may take very long');
            }
            
            if (errors.length > 0) {
                showError(errors.join('<br>'));
                return false;
            }
            
            hideError();
            return true;
        }
        
        function showError(message) {
            const errorDiv = document.getElementById('error-message');
            errorDiv.innerHTML = `<strong>⚠️ Error:</strong> ${message}`;
            errorDiv.style.display = 'block';
        }
        
        function hideError() {
            document.getElementById('error-message').style.display = 'none';
        }
        
        // Progress indicator
        function updateProgress(percent, message = '') {
            const container = document.getElementById('progress-container');
            const bar = document.getElementById('progress-bar');
            container.style.display = 'block';
            bar.style.width = percent + '%';
            bar.textContent = message || percent + '%';
        }
        
        function hideProgress() {
            document.getElementById('progress-container').style.display = 'none';
        }
        
        // Update formula display
        function updateFormulaDisplay() {
            const constantType = document.getElementById('constant').value;
            const formulaDiv = document.getElementById('formula-display');
            
            let formula = '';
            if (constantType === 'pi') {
                formula = 'π = √(6 · ∏<sub>p</sub>(1-p<sup>-2</sup>)<sup>-1</sup>)';
            } else {
                const n = parseInt(constantType.replace('zeta', '')) / 2;
                formula = `ζ(${2*n}) = ∏<sub>p</sub>(1-p<sup>-${2*n}</sup>)<sup>-1</sup>`;
            }
            
            formulaDiv.innerHTML = formula;
        }
        
        // Listen for constant change
        document.getElementById('constant').addEventListener('change', updateFormulaDisplay);
        
        // Listen for method change to show/hide modulus control
        document.getElementById('method').addEventListener('change', function() {
            const method = this.value;
            const modulusControl = document.getElementById('modulus-control');
            
            if (method === 'residue' || method === 'both') {
                modulusControl.style.display = 'block';
                updatePhiInfo();
            } else {
                modulusControl.style.display = 'none';
            }
        });
        
        // Listen for modulus change
        document.getElementById('modulus').addEventListener('input', updatePhiInfo);
        
        // Update phi(k) information
        function updatePhiInfo() {
            const k = parseInt(document.getElementById('modulus').value);
            const phi = eulerPhi(k);
            const coprimes = getCoprimesTo(k);
            
            document.getElementById('phi-info').innerHTML = 
                `φ(${k}) = ${phi} residue classes: {${coprimes.slice(0, 10).join(', ')}${coprimes.length > 10 ? ', ...' : ''}}`;
        }
        
        // Compute Euler's totient function φ(n)
        function eulerPhi(n) {
            let result = n;
            let p = 2;
            
            while (p * p <= n) {
                if (n % p === 0) {
                    while (n % p === 0) {
                        n = Math.floor(n / p);
                    }
                    result -= Math.floor(result / p);
                }
                p++;
            }
            
            if (n > 1) {
                result -= Math.floor(result / n);
            }
            
            return result;
        }
        
        // Get all numbers coprime to k
        function getCoprimesTo(k) {
            const coprimes = [];
            for (let a = 1; a < k; a++) {
                if (gcd(a, k) === 1) {
                    coprimes.push(a);
                }
            }
            return coprimes;
        }
        
        // Greatest common divisor
        function gcd(a, b) {
            while (b !== 0) {
                const temp = b;
                b = a % b;
                a = temp;
            }
            return a;
        }
        // Sieve of Eratosthenes optimized for our needs
        function sieveOfEratosthenes(limit) {
            if (limit < 2) return [];
            
            const isPrime = new Array(limit + 1).fill(true);
            isPrime[0] = isPrime[1] = false;
            
            for (let i = 2; i * i <= limit; i++) {
                if (isPrime[i]) {
                    for (let j = i * i; j <= limit; j += i) {
                        isPrime[j] = false;
                    }
                }
            }
            
            const primes = [];
            for (let i = 2; i <= limit; i++) {
                if (isPrime[i]) primes.push(i);
            }
            return primes;
        }
        
        // Compute gap classes: gap = p_next - p_current
        function computeGapClasses(primes) {
            const gapClasses = {};
            
            for (let i = 0; i < primes.length - 1; i++) {
                const gap = primes[i + 1] - primes[i];
                if (!gapClasses[gap]) gapClasses[gap] = [];
                // Store the current prime (the one before the gap)
                gapClasses[gap].push(primes[i]);
            }
            
            // Add the last prime to a special category since it has no gap after it
            const lastPrime = primes[primes.length - 1];
            if (!gapClasses['last']) gapClasses['last'] = [];
            gapClasses['last'].push(lastPrime);
            
            return gapClasses;
        }
        
        // Compute residue channels for any modulus k
        function computeResidueChannels(primes, modulus) {
            const coprimes = getCoprimesTo(modulus);
            const channels = {};
            
            // Initialize channels
            coprimes.forEach(a => channels[a] = []);
            
            // Get small primes that divide the modulus (need special handling)
            const smallPrimes = [];
            let tempMod = modulus;
            for (let p = 2; p <= modulus; p++) {
                if (tempMod % p === 0) {
                    smallPrimes.push(p);
                    while (tempMod % p === 0) {
                        tempMod = Math.floor(tempMod / p);
                    }
                }
            }
            
            // Classify primes by residue
            primes.forEach(p => {
                if (!smallPrimes.includes(p)) {
                    const residue = p % modulus;
                    if (channels[residue] !== undefined) {
                        channels[residue].push(p);
                    }
                }
            });
            
            return {
                channels: channels,
                smallPrimes: smallPrimes.filter(p => primes.includes(p)),
                coprimes: coprimes
            };
        }
        
        // Compute Y cutoff for given epsilon and constant type
        function computeCutoff(epsilon, constantType) {
            if (constantType === 'pi') {
                return Math.ceil(1 + 1 / Math.log(1 + epsilon));
            } else {
                const n = parseInt(constantType.replace('zeta', '')) / 2;
                const power = 1 / (2 * n - 1);
                return Math.ceil(Math.pow(2 / ((2 * n - 1) * Math.log(1 + epsilon)), power));
            }
        }
        
        // Compute truncated product
        function computeTruncatedProduct(primes, exponent) {
            let product = 1;
            for (const p of primes) {
                product *= 1 / (1 - Math.pow(p, -exponent));
            }
            return product;
        }
        
        // Exact values for reference
        const exactValues = {
            'pi': Math.PI,
            'zeta4': Math.PI ** 4 / 90,
            'zeta6': Math.PI ** 6 / 945,
            'zeta8': Math.PI ** 8 / 9450,
            'zeta10': Math.PI ** 10 / 93555
        };
        
        function compute() {
            const epsilon = parseFloat(document.getElementById('epsilon').value);
            const constantType = document.getElementById('constant').value;
            const method = document.getElementById('method').value;
            
            // Show loading
            document.getElementById('results').style.display = 'block';
            document.getElementById('computed-value').innerHTML = '<div class="loading">Computing...</div>';
            
            setTimeout(() => {
                try {
                    // Compute required cutoff
                    const Y = computeCutoff(epsilon, constantType);
                    const primes = sieveOfEratosthenes(Y - 1);
                    
                    let computedValue;
                    let intermediateValue;
                    let analysisHtml = '';
                    
                    if (constantType === 'pi') {
                        // π = √(6 * ζ(2))
                        const zetaProduct = computeTruncatedProduct(primes, 2);
                        intermediateValue = zetaProduct;
                        computedValue = Math.sqrt(6 * zetaProduct);
                        
                        // Show intermediate structure
                        showIntermediateStructure(zetaProduct, primes, constantType);
                    } else {
                        // ζ(2n) = ∏(1-p^-2n)^-1
                        const n = parseInt(constantType.replace('zeta', '')) / 2;
                        computedValue = computeTruncatedProduct(primes, 2 * n);
                        intermediateValue = computedValue;
                        
                        showIntermediateStructure(computedValue, primes, constantType);
                    }
                    
                    // Display results
                    document.getElementById('computed-value').textContent = computedValue.toFixed(12);
                    document.getElementById('exact-value').textContent = exactValues[constantType].toFixed(12);
                    
                    const actualError = Math.abs(computedValue - exactValues[constantType]) / exactValues[constantType];
                    document.getElementById('actual-error').innerHTML = `Actual relative error: ${(actualError * 100).toFixed(6)}%`;
                    document.getElementById('error-bound').innerHTML = `Guaranteed error ≤ ${(epsilon * 100).toFixed(4)}%`;
                    document.getElementById('prime-count').innerHTML = `Using ${primes.length} primes up to ${Y-1}`;
                    
                    // Method-specific analysis
                    if (method === 'gap' || method === 'both') {
                        showGapAnalysis(primes, constantType);
                    } else {
                        document.getElementById('gap-analysis').style.display = 'none';
                    }
                    
                    if (method === 'residue' || method === 'both') {
                        showResidueAnalysis(primes, constantType);
                    } else {
                        document.getElementById('channel-analysis').style.display = 'none';
                        document.getElementById('chart-section').style.display = 'none';
                    }
                    
                } catch (error) {
                    document.getElementById('computed-value').innerHTML = `<span style="color: #ff6b6b;">Error: ${error.message}</span>`;
                }
            }, 100);
        }
        
        function showGapAnalysis(primes, constantType) {
            const gapClasses = computeGapClasses(primes);
            const exponent = constantType === 'pi' ? 2 : parseInt(constantType.replace('zeta', ''));
            
            let html = '';
            const sortedGaps = Object.keys(gapClasses)
                .filter(g => g !== 'last')
                .map(Number)
                .sort((a, b) => a - b);
            
            // Show gap 1 separately (only prime 2)
            if (gapClasses[1]) {
                const gap1Primes = gapClasses[1];
                const contribution = computeTruncatedProduct(gap1Primes, exponent);
                const logContrib = Math.log(contribution);
                
                html += `
                    <div class="gap-item" style="background: rgba(255, 215, 0, 0.2); border: 2px solid #ffd700;">
                        <div><strong>Gap 1</strong></div>
                        <div style="font-size: 0.85em; color: #ffd700;">Only prime 2 → 3</div>
                        <div class="gap-value">R₁ = ${contribution.toFixed(6)}</div>
                        <div style="font-size: 0.8em; opacity: 0.8;">${gap1Primes.length} prime: [${gap1Primes.join(', ')}]</div>
                        <div style="font-size: 0.8em; opacity: 0.8;">log R = ${logContrib.toFixed(4)}</div>
                    </div>
                `;
            }
            
            // Show even gaps
            for (const gap of sortedGaps.filter(g => g !== 1).slice(0, 11)) {
                const gapPrimes = gapClasses[gap];
                const contribution = computeTruncatedProduct(gapPrimes, exponent);
                const logContrib = Math.log(contribution);
                
                // Show first few primes in this gap class
                const samplePrimes = gapPrimes.slice(0, 5);
                const primeList = samplePrimes.join(', ') + (gapPrimes.length > 5 ? ', ...' : '');
                
                html += `
                    <div class="gap-item">
                        <div><strong>Gap ${gap}</strong></div>
                        <div style="font-size: 0.75em; opacity: 0.7; margin: 3px 0;">p where p' - p = ${gap}</div>
                        <div class="gap-value">R_${gap} = ${contribution.toFixed(6)}</div>
                        <div style="font-size: 0.8em; opacity: 0.8;">${gapPrimes.length} primes</div>
                        <div style="font-size: 0.75em; opacity: 0.6; margin-top: 3px;">[${primeList}]</div>
                        <div style="font-size: 0.8em; opacity: 0.8;">log R = ${logContrib.toFixed(4)}</div>
                    </div>
                `;
            }
            
            document.getElementById('gap-grid').innerHTML = html;
            document.getElementById('gap-analysis').style.display = 'block';
        }
        
        function showIntermediateStructure(intermediateValue, primes, constantType) {
            document.getElementById('intermediate-section').style.display = 'block';
            document.getElementById('intermediate-product').textContent = intermediateValue.toFixed(12);
            
            let formulaHtml = '';
            let explanationHtml = '';
            
            if (constantType === 'pi') {
                // For pi, show ζ(2) approximation
                const primeList = primes.length <= 5 ? 
                    `{${primes.join(', ')}}` : 
                    `{${primes.slice(0, 3).join(', ')}, ..., ${primes[primes.length-1]}}`;
                
                formulaHtml = `<strong>ζ(2) ≈ ∏<sub>p∈${primeList}</sub> (1-p<sup>-2</sup>)<sup>-1</sup></strong>`;
                
                // Calculate what this equals exactly for small cases
                if (primes.length === 1 && primes[0] === 2) {
                    const exactForm = '4/3';
                    explanationHtml = `
                        With only prime <strong>p=2</strong>:<br>
                        ζ(2) ≈ 1/(1-2<sup>-2</sup>) = 1/(1-1/4) = <strong>${exactForm}</strong><br>
                        Therefore: π ≈ √(6 · 4/3) = √8 = <strong>2√2</strong>
                    `;
                } else if (primes.length === 2 && primes[0] === 2 && primes[1] === 3) {
                    const product1 = 1/(1-1/4);
                    const product2 = 1/(1-1/9);
                    const total = product1 * product2;
                    explanationHtml = `
                        With primes <strong>{2, 3}</strong>:<br>
                        ζ(2) ≈ [1/(1-1/4)] · [1/(1-1/9)] = (4/3) · (9/8) = <strong>${total.toFixed(6)}</strong><br>
                        Therefore: π ≈ √(6 · ${total.toFixed(4)}) ≈ <strong>${Math.sqrt(6*total).toFixed(6)}</strong>
                    `;
                } else {
                    explanationHtml = `
                        Using ${primes.length} primes up to ${primes[primes.length-1]}, the truncated Euler product gives:<br>
                        ζ(2) ≈ <strong>${intermediateValue.toFixed(12)}</strong><br>
                        Then: π = √(6 · ζ(2)) ≈ √(6 · ${intermediateValue.toFixed(6)}) ≈ <strong>${Math.sqrt(6*intermediateValue).toFixed(12)}</strong>
                    `;
                }
            } else {
                // For ζ(2n)
                const n = parseInt(constantType.replace('zeta', ''));
                const primeList = primes.length <= 5 ? 
                    `{${primes.join(', ')}}` : 
                    `{${primes.slice(0, 3).join(', ')}, ..., ${primes[primes.length-1]}}`;
                
                formulaHtml = `<strong>ζ(${n}) ≈ ∏<sub>p∈${primeList}</sub> (1-p<sup>-${n}</sup>)<sup>-1</sup></strong>`;
                
                if (primes.length === 1 && primes[0] === 2) {
                    const denom = 1 - Math.pow(2, -n);
                    explanationHtml = `
                        With only prime <strong>p=2</strong>:<br>
                        ζ(${n}) ≈ 1/(1-2<sup>-${n}</sup>) = <strong>${intermediateValue.toFixed(12)}</strong>
                    `;
                } else {
                    explanationHtml = `
                        Using ${primes.length} primes, the truncated Euler product directly gives:<br>
                        ζ(${n}) ≈ <strong>${intermediateValue.toFixed(12)}</strong>
                    `;
                }
            }
            
            document.getElementById('intermediate-formula').innerHTML = formulaHtml;
            document.getElementById('intermediate-explanation').innerHTML = explanationHtml;
        }
        
        function showResidueAnalysis(primes, constantType) {
            const modulus = parseInt(document.getElementById('modulus').value);
            const exponent = constantType === 'pi' ? 2 : parseInt(constantType.replace('zeta', ''));
            
            const result = computeResidueChannels(primes, modulus);
            const channels = result.channels;
            const smallPrimes = result.smallPrimes;
            const coprimes = result.coprimes;
            
            // Update titles
            document.getElementById('channel-title').textContent = `Residue Channels (mod ${modulus})`;
            document.getElementById('chart-title').textContent = `Residue Channel Contributions ℤ_a(s;${modulus})`;
            
            let html = '';
            const channelData = [];
            
            // Handle small primes that divide the modulus
            if (smallPrimes.length > 0) {
                const smallProduct = computeTruncatedProduct(smallPrimes, exponent);
                const gridSpan = Math.min(coprimes.length, 4);
                
                html += `
                    <div class="channel-item" style="grid-column: span ${gridSpan}; background: rgba(255, 215, 0, 0.2);">
                        <div>Prime Divisors of ${modulus}</div>
                        <div style="font-weight: bold;">${smallProduct.toFixed(6)}</div>
                        <div style="font-size: 0.8em;">{${smallPrimes.join(', ')}}</div>
                    </div>
                `;
            }
            
            // Display each residue channel
            for (const a of coprimes) {
                const channelPrimes = channels[a];
                let contribution = 1;
                
                if (channelPrimes.length > 0) {
                    contribution = computeTruncatedProduct(channelPrimes, exponent);
                    
                    // Show sample primes for this channel
                    const sampleSize = 3;
                    const samplePrimes = channelPrimes.slice(0, sampleSize);
                    const primeDisplay = samplePrimes.join(', ') + 
                        (channelPrimes.length > sampleSize ? ', ...' : '');
                    
                    html += `
                        <div class="channel-item">
                            <div>≡ ${a} (mod ${modulus})</div>
                            <div style="font-weight: bold; color: #4ecdc4;">${contribution.toFixed(6)}</div>
                            <div style="font-size: 0.75em;">${channelPrimes.length} primes</div>
                            <div style="font-size: 0.7em; opacity: 0.6; margin-top: 3px;">[${primeDisplay}]</div>
                        </div>
                    `;
                } else {
                    html += `
                        <div class="channel-item" style="opacity: 0.5;">
                            <div>≡ ${a} (mod ${modulus})</div>
                            <div>1.0000</div>
                            <div style="font-size: 0.8em;">0 primes</div>
                        </div>
                    `;
                }
                
                channelData.push({
                    residue: a,
                    contribution: contribution,
                    primeCount: channelPrimes.length,
                    logContribution: Math.log(contribution)
                });
            }
            
            document.getElementById('channel-grid').innerHTML = html;
            document.getElementById('channel-analysis').style.display = 'block';
            
            // Create chart
            const smallProduct = smallPrimes.length > 0 ? 
                computeTruncatedProduct(smallPrimes, exponent) : null;
            createChannelChart(channelData, smallProduct, modulus, smallPrimes);
        }
        
        let channelChart = null;
        
        function createChannelChart(channelData, smallPrimesContrib, modulus, smallPrimes) {
            const ctx = document.getElementById('channelChart').getContext('2d');
            
            // Destroy existing chart if it exists
            if (channelChart) {
                channelChart.destroy();
            }
            
            // Prepare data for chart
            const labels = [];
            const contributions = [];
            const logContributions = [];
            const primeCounts = [];
            
            // Add small primes if they exist
            if (smallPrimesContrib !== null && smallPrimesContrib !== 1) {
                labels.push(`Divisors of ${modulus} (${smallPrimes.join(',')})`);
                contributions.push(smallPrimesContrib);
                logContributions.push(Math.log(smallPrimesContrib));
                primeCounts.push(smallPrimes.length);
            }
            
            // Add residue channels
            channelData.forEach(d => {
                labels.push(`≡ ${d.residue} (mod ${modulus})`);
                contributions.push(d.contribution);
                logContributions.push(d.logContribution);
                primeCounts.push(d.primeCount);
            });
            
            // Create gradient colors - more colors for larger moduli
            const numColors = labels.length;
            const colors = [];
            
            for (let i = 0; i < numColors; i++) {
                if (i === 0 && smallPrimesContrib !== null && smallPrimesContrib !== 1) {
                    colors.push('rgba(255, 215, 0, 0.8)'); // Gold for small primes
                } else {
                    const hue = (i * 360 / numColors) % 360;
                    colors.push(`hsla(${hue}, 70%, 60%, 0.8)`);
                }
            }
            
            channelChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Channel Contribution',
                        data: contributions,
                        backgroundColor: colors,
                        borderColor: colors.map(c => c.replace('0.8', '1')),
                        borderWidth: 2,
                        borderRadius: 8,
                        borderSkipped: false,
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: true,
                    aspectRatio: 2.5,
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            backgroundColor: 'rgba(0, 0, 0, 0.9)',
                            titleColor: '#fff',
                            bodyColor: '#fff',
                            borderColor: 'rgba(255, 255, 255, 0.3)',
                            borderWidth: 1,
                            callbacks: {
                                label: function(context) {
                                    const idx = context.dataIndex;
                                    const contrib = contributions[idx];
                                    const logContrib = logContributions[idx];
                                    const count = primeCounts[idx];
                                    
                                    return [
                                        `Contribution: ${contrib.toFixed(6)}`,
                                        `log(Contribution): ${logContrib.toFixed(4)}`,
                                        `Prime count: ${count}`
                                    ];
                                }
                            }
                        }
                    },
                    scales: {
                        x: {
                            ticks: {
                                color: '#fff',
                                maxRotation: 90,
                                minRotation: 45,
                                font: {
                                    size: labels.length > 30 ? 7 : (labels.length > 20 ? 8 : 10)
                                },
                                autoSkip: labels.length > 50,
                                maxTicksLimit: labels.length > 50 ? 50 : undefined
                            },
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            }
                        },
                        y: {
                            ticks: {
                                color: '#fff',
                                callback: function(value) {
                                    return value.toFixed(3);
                                }
                            },
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            },
                            title: {
                                display: true,
                                text: `ℤₐ(s;${modulus}) Contribution`,
                                color: '#fff',
                                font: {
                                    size: 14,
                                    weight: 'bold'
                                }
                            }
                        }
                    },
                    animation: {
                        duration: 1500,
                        easing: 'easeOutQuart'
                    }
                }
            });
            
            // Show chart section
            document.getElementById('chart-section').style.display = 'block';
            
            // Create legend
            createChartLegend(labels, colors, primeCounts);
        }
        
        function createChartLegend(labels, colors, primeCounts) {
            let legendHtml = '';
            
            for (let i = 0; i < labels.length; i++) {
                legendHtml += `
                    <div class="legend-item">
                        <div class="legend-color" style="background-color: ${colors[i]};"></div>
                        <span>${labels[i]} (${primeCounts[i]} primes)</span>
                    </div>
                `;
            }
            
            document.getElementById('chartLegend').innerHTML = legendHtml;
        }
        
        // Initialize with default computation
        window.onload = () => {
            compute();
        };
    </script>
</body>
</html>
