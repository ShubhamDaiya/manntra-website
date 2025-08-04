<script type="text/javascript">
        var gk_isXlsx = false;
        var gk_xlsxFileLookup = {};
        var gk_fileData = {};
        function filledCell(cell) {
          return cell !== '' && cell != null;
        }
        function loadFileData(filename) {
        if (gk_isXlsx && gk_xlsxFileLookup[filename]) {
            try {
                var workbook = XLSX.read(gk_fileData[filename], { type: 'base64' });
                var firstSheetName = workbook.SheetNames[0];
                var worksheet = workbook.Sheets[firstSheetName];

                // Convert sheet to JSON to filter blank rows
                var jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1, blankrows: false, defval: '' });
                // Filter out blank rows (rows where all cells are empty, null, or undefined)
                var filteredData = jsonData.filter(row => row.some(filledCell));

                // Heuristic to find the header row by ignoring rows with fewer filled cells than the next row
                var headerRowIndex = filteredData.findIndex((row, index) =>
                  row.filter(filledCell).length >= filteredData[index + 1]?.filter(filledCell).length
                );
                // Fallback
                if (headerRowIndex === -1 || headerRowIndex > 25) {
                  headerRowIndex = 0;
                }

                // Convert filtered JSON back to CSV
                var csv = XLSX.utils.aoa_to_sheet(filteredData.slice(headerRowIndex)); // Create a new sheet from filtered array of arrays
                csv = XLSX.utils.sheet_to_csv(csv, { header: 1 });
                return csv;
            } catch (e) {
                console.error(e);
                return "";
            }
        }
        return gk_fileData[filename] || "";
        }
        </script><!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Manntra - Home</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Inter', sans-serif;
      margin: 0;
      background: #FFF8F2;
      color: #222;
    }
    header {
      background-color: #00777B;
      color: white;
      padding: 2rem 1rem;
      text-align: center;
    }
    header img {
      width: 80px;
      margin-bottom: 1rem;
    }
    h1 {
      margin: 0;
    }
    .tagline {
      font-size: 1rem;
      margin-top: 0.5rem;
    }
    .test-grid, .test-content {
      max-width: 800px;
      margin: 2rem auto;
      padding: 0 1rem;
      display: none;
    }
    .test-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      gap: 1rem;
    }
    .test-card {
      background: #fff;
      border-radius: 12px;
      padding: 1rem;
      text-align: center;
      box-shadow: 0 4px 8px rgba(0,0,0,0.05);
      cursor: pointer;
    }
    .test-card img {
      width: 60px;
      height: 60px;
      margin-bottom: 0.5rem;
    }
    .test-card p {
      margin: 0;
      font-size: 0.9rem;
    }
    .test-content h2 {
      text-align: center;
      margin-bottom: 1.5rem;
    }
    .question {
      background: #fff;
      border-radius: 12px;
      padding: 1rem;
      margin-bottom: 1rem;
      box-shadow: 0 4px 8px rgba(0,0,0,0.05);
    }
    .question p {
      margin: 0 0 1rem 0;
    }
    .question label {
      display: block;
      margin: 0.5rem 0;
    }
    button {
      background-color: #00777B;
      color: white;
      padding: 0.75rem 1.5rem;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      display: block;
      margin: 1rem auto;
      font-size: 1rem;
    }
    button:hover {
      background-color: #005f61;
    }
    #result {
      display: none;
      background: #fff;
      border-radius: 12px;
      padding: 1rem;
      margin-top: 1rem;
      text-align: center;
      box-shadow: 0 4px 8px rgba(0,0,0,0.05);
    }
  </style>
</head>
<body>
  <header>
    <img src="logo.png" alt="Manntra Logo">
    <h1>MANNTRA</h1>
    <p class="tagline">Jab Mann ho down, Manntra ho around</p>
  </header>

  <section class="test-grid" id="home-grid">
    <div class="test-card" onclick="showTest('depression')">
      <img src="depression.jpg" alt="Depression Test">
      <p>Depression Test</p>
    </div>
    <div class="test-card" onclick="showTest('anxiety')">
      <img src="anxiety.jpg" alt="Anxiety Test">
      <p>Anxiety Test</p>
    </div>
    <div class="test-card" onclick="showTest('bipolar')">
      <img src="bipolar.jpg" alt="Bipolar Test">
      <p>Bipolar Test</p>
    </div>
    <div class="test-card" onclick="showTest('addiction')">
      <img src="addiction.jpg" alt="Addiction Test">
      <p>Addiction Test</p>
    </div>
    <div class="test-card" onclick="showTest('ptsd')">
      <img src="ptsd.jpg" alt="PTSD Test">
      <p>PTSD Test</p>
    </div>
    <div class="test-card" onclick="showTest('eating_disorder')">
      <img src="eating_disorder.jpg" alt="Eating Disorder Test">
      <p>Eating Disorder Test</p>
    </div>
    <div class="test-card" onclick="showTest('self_injury')">
      <img src="self_injury.jpg" alt="Self-Injury Test">
      <p>Self-Injury Test</p>
    </div>
  </section>

  <section class="test-content" id="test-depression">
    <h2>Depression Test</h2>
    <p style="text-align: center; margin-bottom: 1rem;">Please answer based on how you've felt over the past two weeks. This is not a substitute for professional diagnosis.</p>
    <div class="question">
      <p>1. How often have you felt little interest or pleasure in doing things?</p>
      <label><input type="radio" name="q1" value="0"> Not at all</label>
      <label><input type="radio" name="q1" value="1"> Several days</label>
      <label><input type="radio" name="q1" value="2"> More than half the days</label>
      <label><input type="radio" name="q1" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>2. How often have you felt down, depressed, or hopeless?</p>
      <label><input type="radio" name="q2" value="0"> Not at all</label>
      <label><input type="radio" name="q2" value="1"> Several days</label>
      <label><input type="radio" name="q2" value="2"> More than half the days</label>
      <label><input type="radio" name="q2" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>3. How often have you had trouble concentrating on things?</p>
      <label><input type="radio" name="q3" value="0"> Not at all</label>
      <label><input type="radio" name="q3" value="1"> Several days</label>
      <label><input type="radio" name="q3" value="2"> More than half the days</label>
      <label><input type="radio" name="q3" value="3"> Nearly every day</label>
    </div>
    <button onclick="calculateResult('depression')">Submit</button>
    <button onclick="showTest('home')">Back to Home</button>
    <div id="result-depression"></div>
  </section>

  <section class="test-content" id="test-anxiety">
    <h2>Anxiety Test</h2>
    <p style="text-align: center; margin-bottom: 1rem;">Please answer based on how you've felt over the past two weeks. This is not a substitute for professional diagnosis.</p>
    <div class="question">
      <p>1. How often have you felt nervous, anxious, or on edge?</p>
      <label><input type="radio" name="a1" value="0"> Not at all</label>
      <label><input type="radio" name="a1" value="1"> Several days</label>
      <label><input type="radio" name="a1" value="2"> More than half the days</label>
      <label><input type="radio" name="a1" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>2. How often have you been unable to stop or control worrying?</p>
      <label><input type="radio" name="a2" value="0"> Not at all</label>
      <label><input type="radio" name="a2" value="1"> Several days</label>
      <label><input type="radio" name="a2" value="2"> More than half the days</label>
      <label><input type="radio" name="a2" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>3. How often have you felt restless or unable to relax?</p>
      <label><input type="radio" name="a3" value="0"> Not at all</label>
      <label><input type="radio" name="a3" value="1"> Several days</label>
      <label><input type="radio" name="a3" value="2"> More than half the days</label>
      <label><input type="radio" name="a3" value="3"> Nearly every day</label>
    </div>
    <button onclick="calculateResult('anxiety')">Submit</button>
    <button onclick="showTest('home')">Back to Home</button>
    <div id="result-anxiety"></div>
  </section>

  <section class="test-content" id="test-bipolar">
    <h2>Bipolar Test</h2>
    <p style="text-align: center; margin-bottom: 1rem;">Please answer based on how you've felt over the past month. This is not a substitute for professional diagnosis.</p>
    <div class="question">
      <p>1. How often have you experienced unusually high energy or irritability?</p>
      <label><input type="radio" name="b1" value="0"> Not at all</label>
      <label><input type="radio" name="b1" value="1"> Several days</label>
      <label><input type="radio" name="b1" value="2"> More than half the days</label>
      <label><input type="radio" name="b1" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>2. How often have you had periods of feeling extremely sad or hopeless?</p>
      <label><input type="radio" name="b2" value="0"> Not at all</label>
      <label><input type="radio" name="b2" value="1"> Several days</label>
      <label><input type="radio" name="b2" value="2"> More than half the days</label>
      <label><input type="radio" name="b2" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>3. How often have you had trouble sleeping or needed less sleep than usual?</p>
      <label><input type="radio" name="b3" value="0"> Not at all</label>
      <label><input type="radio" name="b3" value="1"> Several days</label>
      <label><input type="radio" name="b3" value="2"> More than half the days</label>
      <label><input type="radio" name="b3" value="3"> Nearly every day</label>
    </div>
    <button onclick="calculateResult('bipolar')">Submit</button>
    <button onclick="showTest('home')">Back to Home</button>
    <div id="result-bipolar"></div>
  </section>

  <section class="test-content" id="test-addiction">
    <h2>Addiction Test</h2>
    <p style="text-align: center; margin-bottom: 1rem;">Please answer based on your habits over the past month. This is not a substitute for professional diagnosis.</p>
    <div class="question">
      <p>1. How often have you felt a strong urge to use a substance or behavior?</p>
      <label><input type="radio" name="d1" value="0"> Not at all</label>
      <label><input type="radio" name="d1" value="1"> Several days</label>
      <label><input type="radio" name="d1" value="2"> More than half the days</label>
      <label><input type="radio" name="d1" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>2. How often have you neglected responsibilities due to this habit?</p>
      <label><input type="radio" name="d2" value="0"> Not at all</label>
      <label><input type="radio" name="d2" value="1"> Several days</label>
      <label><input type="radio" name="d2" value="2"> More than half the days</label>
      <label><input type="radio" name="d2" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>3. How often have you tried to cut down but couldn’t?</p>
      <label><input type="radio" name="d3" value="0"> Not at all</label>
      <label><input type="radio" name="d3" value="1"> Several days</label>
      <label><input type="radio" name="d3" value="2"> More than half the days</label>
      <label><input type="radio" name="d3" value="3"> Nearly every day</label>
    </div>
    <button onclick="calculateResult('addiction')">Submit</button>
    <button onclick="showTest('home')">Back to Home</button>
    <div id="result-addiction"></div>
  </section>

  <section class="test-content" id="test-ptsd">
    <h2>PTSD Test</h2>
    <p style="text-align: center; margin-bottom: 1rem;">Please answer based on how you've felt recently. This is not a substitute for professional diagnosis.</p>
    <div class="question">
      <p>1. How often have you had unwanted memories of a stressful event?</p>
      <label><input type="radio" name="p1" value="0"> Not at all</label>
      <label><input type="radio" name="p1" value="1"> Several days</label>
      <label><input type="radio" name="p1" value="2"> More than half the days</label>
      <label><input type="radio" name="p1" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>2. How often have you felt avoidant of reminders of a stressful event?</p>
      <label><input type="radio" name="p2" value="0"> Not at all</label>
      <label><input type="radio" name="p2" value="1"> Several days</label>
      <label><input type="radio" name="p2" value="2"> More than half the days</label>
      <label><input type="radio" name="p2" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>3. How often have you felt hypervigilant or easily startled?</p>
      <label><input type="radio" name="p3" value="0"> Not at all</label>
      <label><input type="radio" name="p3" value="1"> Several days</label>
      <label><input type="radio" name="p3" value="2"> More than half the days</label>
      <label><input type="radio" name="p3" value="3"> Nearly every day</label>
    </div>
    <button onclick="calculateResult('ptsd')">Submit</button>
    <button onclick="showTest('home')">Back to Home</button>
    <div id="result-ptsd"></div>
  </section>

  <section class="test-content" id="test-eating_disorder">
    <h2>Eating Disorder Test</h2>
    <p style="text-align: center; margin-bottom: 1rem;">Please answer based on your habits over the past month. This is not a substitute for professional diagnosis.</p>
    <div class="question">
      <p>1. How often have you felt preoccupied with body weight or shape?</p>
      <label><input type="radio" name="e1" value="0"> Not at all</label>
      <label><input type="radio" name="e1" value="1"> Several days</label>
      <label><input type="radio" name="e1" value="2"> More than half the days</label>
      <label><input type="radio" name="e1" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>2. How often have you engaged in binge eating or purging?</p>
      <label><input type="radio" name="e2" value="0"> Not at all</label>
      <label><input type="radio" name="e2" value="1"> Several days</label>
      <label><input type="radio" name="e2" value="2"> More than half the days</label>
      <label><input type="radio" name="e2" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>3. How often have you avoided certain foods due to fear of gaining weight?</p>
      <label><input type="radio" name="e3" value="0"> Not at all</label>
      <label><input type="radio" name="e3" value="1"> Several days</label>
      <label><input type="radio" name="e3" value="2"> More than half the days</label>
      <label><input type="radio" name="e3" value="3"> Nearly every day</label>
    </div>
    <button onclick="calculateResult('eating_disorder')">Submit</button>
    <button onclick="showTest('home')">Back to Home</button>
    <div id="result-eating_disorder"></div>
  </section>

  <section class="test-content" id="test-self_injury">
    <h2>Self-Injury Test</h2>
    <p style="text-align: center; margin-bottom: 1rem;">Please answer based on your behaviors over the past month. This is not a substitute for professional diagnosis.</p>
    <div class="question">
      <p>1. How often have you intentionally hurt yourself (e.g., cutting, burning)?</p>
      <label><input type="radio" name="s1" value="0"> Not at all</label>
      <label><input type="radio" name="s1" value="1"> Several days</label>
      <label><input type="radio" name="s1" value="2"> More than half the days</label>
      <label><input type="radio" name="s1" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>2. How often have you felt an urge to self-injure to cope with emotions?</p>
      <label><input type="radio" name="s2" value="0"> Not at all</label>
      <label><input type="radio" name="s2" value="1"> Several days</label>
      <label><input type="radio" name="s2" value="2"> More than half the days</label>
      <label><input type="radio" name="s2" value="3"> Nearly every day</label>
    </div>
    <div class="question">
      <p>3. How often have you hidden self-injury marks from others?</p>
      <label><input type="radio" name="s3" value="0"> Not at all</label>
      <label><input type="radio" name="s3" value="1"> Several days</label>
      <label><input type="radio" name="s3" value="2"> More than half the days</label>
      <label><input type="radio" name="s3" value="3"> Nearly every day</label>
    </div>
    <button onclick="calculateResult('self_injury')">Submit</button>
    <button onclick="showTest('home')">Back to Home</button>
    <div id="result-self_injury"></div>
  </section>

  <script>
    document.getElementById('home-grid').style.display = 'grid';

    function showTest(testId) {
      document.getElementById('home-grid').style.display = testId === 'home' ? 'grid' : 'none';
      document.querySelectorAll('.test-content').forEach(section => {
        section.style.display = section.id === 'test-' + testId ? 'block' : 'none';
      });
      // Clear previous results when navigating
      document.querySelectorAll('[id^="result-"]').forEach(result => {
        result.style.display = 'none';
        result.innerHTML = '';
      });
    }

    function calculateResult(testId) {
      const form = document.querySelectorAll(`#test-${testId} input[type="radio"]:checked`);
      let score = 0;
      form.forEach(input => {
        score += parseInt(input.value);
      });
      const resultDiv = document.getElementById('result-' + testId);
      resultDiv.style.display = 'block';
      const testName = testId === 'depression' ? 'depression' : 
                      testId === 'anxiety' ? 'anxiety' : 
                      testId === 'bipolar' ? 'bipolar' : 
                      testId === 'addiction' ? 'addiction' : 
                      testId === 'ptsd' ? 'PTSD' : 
                      testId === 'eating_disorder' ? 'eating disorder' : 
                      'self-injury';
      const resultData = { test: testName, score: score, date: new Date().toLocaleString() };
      localStorage.setItem('result-' + testId, JSON.stringify(resultData));
      if (score <= 3) {
        resultDiv.innerHTML = '<p>Your score: ' + score + '</p><p>Low ' + testName + ' levels. Consider professional advice if concerned.</p>';
      } else if (score <= 6) {
        resultDiv.innerHTML = '<p>Your score: ' + score + '</p><p>Moderate ' + testName + ' levels. Professional support may help.</p>';
      } else {
        resultDiv.innerHTML = '<p>Your score: ' + score + '</p><p>High ' + testName + ' levels. Please consult a professional.</p>';
      }
    }

    function viewResults() {
      let resultsHtml = '<h2>Saved Results</h2>';
      for (let i = 0; i < localStorage.length; i++) {
        const key = localStorage.key(i);
        if (key.startsWith('result-')) {
          const result = JSON.parse(localStorage.getItem(key));
          resultsHtml += `<p>Test: ${result.test}, Score: ${result.score}, Date: ${result.date}</p>`;
        }
      }
      document.getElementById('result-depression').innerHTML = resultsHtml;
      document.getElementById('test-depression').style.display = 'block';
      document.getElementById('home-grid').style.display = 'none';
      document.querySelectorAll('.test-content').forEach(section => {
        section.style.display = section.id === 'test-depression' ? 'block' : 'none';
      });
    }

    function clearResults() {
      localStorage.clear();
      alert('All results have been cleared.');
      showTest('home');
    }

    const homeGrid = document.getElementById('home-grid');
    const viewButton = document.createElement('button');
    viewButton.textContent = 'View Results';
    viewButton.onclick = viewResults;
    homeGrid.appendChild(viewButton);

    const clearButton = document.createElement('button');
    clearButton.textContent = 'Clear Results';
    clearButton.onclick = clearResults;
    homeGrid.appendChild(clearButton);
  </script>
</body>
</html>
