# <!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>প্রশ্ন জেনারেটর AI</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@300;400;500;600;700&family=Noto+Sans+Bengali:wght@400;600;700&display=swap');

  :root {
    --bg: #0a0e1a;
    --surface: #111827;
    --surface2: #1a2235;
    --accent: #6366f1;
    --accent2: #8b5cf6;
    --gold: #f59e0b;
    --green: #10b981;
    --red: #ef4444;
    --text: #e2e8f0;
    --muted: #64748b;
    --border: #1e293b;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Hind Siliguri', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Animated background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: 
      radial-gradient(ellipse 80% 50% at 20% 10%, rgba(99,102,241,0.12) 0%, transparent 60%),
      radial-gradient(ellipse 60% 40% at 80% 80%, rgba(139,92,246,0.10) 0%, transparent 60%);
    pointer-events: none;
    z-index: 0;
  }

  .container {
    max-width: 860px;
    margin: 0 auto;
    padding: 2rem 1.5rem;
    position: relative;
    z-index: 1;
  }

  /* Header */
  header {
    text-align: center;
    margin-bottom: 2.5rem;
    animation: fadeDown 0.7s ease;
  }

  .logo-row {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.75rem;
    margin-bottom: 0.5rem;
  }

  .logo-icon {
    width: 48px; height: 48px;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    border-radius: 14px;
    display: flex; align-items: center; justify-content: center;
    font-size: 1.5rem;
    box-shadow: 0 0 30px rgba(99,102,241,0.4);
  }

  h1 {
    font-size: 2rem;
    font-weight: 700;
    background: linear-gradient(135deg, #818cf8, #c084fc);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .subtitle {
    color: var(--muted);
    font-size: 0.95rem;
    margin-top: 0.4rem;
  }

  /* Upload Zone */
  .upload-zone {
    background: var(--surface);
    border: 2px dashed var(--border);
    border-radius: 20px;
    padding: 3rem 2rem;
    text-align: center;
    cursor: pointer;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
    margin-bottom: 1.5rem;
    animation: fadeUp 0.7s ease 0.1s both;
  }

  .upload-zone:hover, .upload-zone.dragover {
    border-color: var(--accent);
    background: rgba(99,102,241,0.05);
    transform: translateY(-2px);
    box-shadow: 0 8px 30px rgba(99,102,241,0.15);
  }

  .upload-icon { font-size: 3rem; margin-bottom: 1rem; display: block; }
  .upload-zone h3 { font-size: 1.1rem; margin-bottom: 0.4rem; color: var(--text); }
  .upload-zone p { color: var(--muted); font-size: 0.85rem; }

  #fileInput { display: none; }

  .preview-container {
    display: none;
    margin-bottom: 1.5rem;
    animation: fadeUp 0.4s ease;
  }

  .preview-container.show { display: block; }

  .preview-img {
    width: 100%;
    max-height: 350px;
    object-fit: contain;
    border-radius: 16px;
    border: 1px solid var(--border);
    background: var(--surface);
  }

  .preview-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 0.75rem;
  }

  .preview-header span { font-size: 0.85rem; color: var(--muted); }

  .remove-btn {
    background: rgba(239,68,68,0.1);
    border: 1px solid rgba(239,68,68,0.3);
    color: var(--red);
    padding: 0.3rem 0.75rem;
    border-radius: 8px;
    cursor: pointer;
    font-size: 0.8rem;
    font-family: inherit;
    transition: all 0.2s;
  }
  .remove-btn:hover { background: rgba(239,68,68,0.2); }

  /* Controls */
  .controls {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
    margin-bottom: 1.5rem;
    animation: fadeUp 0.7s ease 0.2s both;
  }

  .control-group label {
    display: block;
    font-size: 0.82rem;
    color: var(--muted);
    margin-bottom: 0.4rem;
    font-weight: 500;
  }

  select, input[type="number"] {
    width: 100%;
    background: var(--surface);
    border: 1px solid var(--border);
    color: var(--text);
    padding: 0.65rem 1rem;
    border-radius: 10px;
    font-family: inherit;
    font-size: 0.9rem;
    outline: none;
    transition: border-color 0.2s;
    appearance: none;
  }

  select:focus, input[type="number"]:focus {
    border-color: var(--accent);
  }

  /* Generate Button */
  .gen-btn {
    width: 100%;
    padding: 1rem;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    border: none;
    border-radius: 14px;
    color: white;
    font-size: 1.05rem;
    font-weight: 600;
    font-family: inherit;
    cursor: pointer;
    transition: all 0.3s;
    box-shadow: 0 4px 20px rgba(99,102,241,0.35);
    margin-bottom: 2rem;
    animation: fadeUp 0.7s ease 0.3s both;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
  }

  .gen-btn:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: 0 8px 30px rgba(99,102,241,0.5);
  }

  .gen-btn:disabled {
    opacity: 0.6;
    cursor: not-allowed;
    transform: none;
  }

  /* Loading */
  .loading {
    display: none;
    text-align: center;
    padding: 3rem;
    animation: fadeUp 0.4s ease;
  }

  .loading.show { display: block; }

  .spinner {
    width: 50px; height: 50px;
    border: 3px solid var(--border);
    border-top-color: var(--accent);
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
    margin: 0 auto 1rem;
  }

  .loading p { color: var(--muted); font-size: 0.95rem; }
  .loading-dots::after {
    content: '';
    animation: dots 1.5s infinite;
  }

  /* Results */
  .results { animation: fadeUp 0.5s ease; }

  .results-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 1.5rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid var(--border);
  }

  .results-header h2 {
    font-size: 1.2rem;
    font-weight: 600;
  }

  .stats {
    display: flex;
    gap: 0.75rem;
  }

  .stat-badge {
    padding: 0.3rem 0.75rem;
    border-radius: 20px;
    font-size: 0.78rem;
    font-weight: 600;
  }

  .stat-mcq { background: rgba(99,102,241,0.15); color: #818cf8; border: 1px solid rgba(99,102,241,0.3); }
  .stat-short { background: rgba(16,185,129,0.15); color: #34d399; border: 1px solid rgba(16,185,129,0.3); }

  /* Section */
  .section-title {
    display: flex;
    align-items: center;
    gap: 0.6rem;
    font-size: 1rem;
    font-weight: 600;
    margin-bottom: 1rem;
    margin-top: 1.5rem;
  }

  .section-icon {
    width: 30px; height: 30px;
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 0.9rem;
  }

  .mcq-icon { background: rgba(99,102,241,0.2); }
  .short-icon { background: rgba(16,185,129,0.2); }

  /* MCQ Card */
  .mcq-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 1.25rem;
    margin-bottom: 1rem;
    transition: border-color 0.2s;
  }

  .mcq-card:hover { border-color: rgba(99,102,241,0.4); }

  .question-num {
    font-size: 0.75rem;
    color: var(--accent);
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    margin-bottom: 0.5rem;
  }

  .question-text {
    font-size: 0.97rem;
    font-weight: 500;
    line-height: 1.6;
    margin-bottom: 1rem;
    color: var(--text);
  }

  .options {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.5rem;
  }

  .option {
    padding: 0.55rem 0.9rem;
    border-radius: 10px;
    font-size: 0.85rem;
    border: 1px solid var(--border);
    background: var(--surface2);
    cursor: pointer;
    transition: all 0.2s;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    line-height: 1.4;
  }

  .option-label {
    width: 22px; height: 22px;
    border-radius: 6px;
    background: rgba(99,102,241,0.15);
    color: #818cf8;
    font-size: 0.75rem;
    font-weight: 700;
    display: flex; align-items: center; justify-content: center;
    flex-shrink: 0;
  }

  .option:hover { border-color: var(--accent); background: rgba(99,102,241,0.08); }

  .option.correct {
    border-color: var(--green);
    background: rgba(16,185,129,0.1);
  }
  .option.correct .option-label { background: rgba(16,185,129,0.2); color: #34d399; }

  .answer-reveal {
    margin-top: 0.75rem;
    padding: 0.6rem 0.9rem;
    background: rgba(16,185,129,0.08);
    border: 1px solid rgba(16,185,129,0.2);
    border-radius: 10px;
    font-size: 0.83rem;
    color: #34d399;
    display: none;
  }

  .answer-reveal.show { display: block; }

  .show-ans-btn {
    background: none;
    border: 1px solid var(--border);
    color: var(--muted);
    padding: 0.35rem 0.75rem;
    border-radius: 8px;
    font-size: 0.78rem;
    cursor: pointer;
    font-family: inherit;
    transition: all 0.2s;
    margin-top: 0.75rem;
  }
  .show-ans-btn:hover { border-color: var(--accent); color: var(--accent); }

  /* Short Answer Card */
  .short-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 1.25rem;
    margin-bottom: 1rem;
    transition: border-color 0.2s;
  }

  .short-card:hover { border-color: rgba(16,185,129,0.4); }

  .short-answer {
    margin-top: 0.75rem;
    padding: 0.6rem 0.9rem;
    background: rgba(16,185,129,0.07);
    border-radius: 10px;
    font-size: 0.85rem;
    color: #34d399;
    border-left: 3px solid var(--green);
    display: none;
  }

  .short-answer.show { display: block; }

  /* Download */
  .action-bar {
    display: flex;
    gap: 0.75rem;
    margin-top: 2rem;
    padding-top: 1.5rem;
    border-top: 1px solid var(--border);
  }

  .action-btn {
    flex: 1;
    padding: 0.8rem;
    border-radius: 12px;
    font-family: inherit;
    font-size: 0.88rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
    border: none;
  }

  .copy-btn {
    background: var(--surface2);
    color: var(--text);
    border: 1px solid var(--border);
  }
  .copy-btn:hover { border-color: var(--accent); color: var(--accent); }

  .download-btn {
    background: linear-gradient(135deg, #10b981, #059669);
    color: white;
    box-shadow: 0 4px 15px rgba(16,185,129,0.25);
  }
  .download-btn:hover { transform: translateY(-1px); box-shadow: 0 6px 20px rgba(16,185,129,0.35); }

  .new-btn {
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    color: white;
  }
  .new-btn:hover { transform: translateY(-1px); }

  /* Error */
  .error-box {
    background: rgba(239,68,68,0.1);
    border: 1px solid rgba(239,68,68,0.3);
    border-radius: 14px;
    padding: 1.25rem;
    color: #fca5a5;
    font-size: 0.9rem;
    margin-bottom: 1rem;
    display: none;
    animation: fadeUp 0.3s ease;
  }
  .error-box.show { display: block; }

  /* Animations */
  @keyframes fadeDown { from { opacity: 0; transform: translateY(-20px); } to { opacity: 1; transform: translateY(0); } }
  @keyframes fadeUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
  @keyframes spin { to { transform: rotate(360deg); } }
  @keyframes dots {
    0% { content: ''; }
    33% { content: '.'; }
    66% { content: '..'; }
    100% { content: '...'; }
  }

  @media (max-width: 600px) {
    .options { grid-template-columns: 1fr; }
    .controls { grid-template-columns: 1fr; }
    .action-bar { flex-direction: column; }
    h1 { font-size: 1.5rem; }
  }
</style>
</head>
<body>
<div class="container">

  <!-- Header -->
  <header>
    <div class="logo-row">
      <div class="logo-icon">🧠</div>
      <h1>প্রশ্ন জেনারেটর AI</h1>
    </div>
    <p class="subtitle">যেকোনো পৃষ্ঠার ছবি দিন — MCQ ও সংক্ষিপ্ত প্রশ্ন তৈরি হবে স্বয়ংক্রিয়ভাবে</p>
  </header>

  <!-- Error Box -->
  <div class="error-box" id="errorBox">⚠️ <span id="errorMsg"></span></div>

  <!-- Upload Zone -->
  <div class="upload-zone" id="uploadZone" onclick="document.getElementById('fileInput').click()">
    <span class="upload-icon">📷</span>
    <h3>ছবি আপলোড করুন</h3>
    <p>বই, নোট বা যেকোনো পৃষ্ঠার ছবি দিন</p>
    <p style="margin-top:0.5rem; font-size:0.8rem; color:#475569;">JPG, PNG, WEBP সাপোর্টেড • ড্র্যাগ & ড্রপ করুন</p>
    <input type="file" id="fileInput" accept="image/*">
  </div>

  <!-- Preview -->
  <div class="preview-container" id="previewContainer">
    <div class="preview-header">
      <span id="fileName">📄 ছবি লোড হয়েছে</span>
      <button class="remove-btn" onclick="removeImage()">✕ সরান</button>
    </div>
    <img id="previewImg" class="preview-img" src="" alt="preview">
  </div>

  <!-- Controls -->
  <div class="controls">
    <div class="control-group">
      <label>📝 MCQ সংখ্যা</label>
      <input type="number" id="mcqCount" value="5" min="1" max="15">
    </div>
    <div class="control-group">
      <label>✏️ সংক্ষিপ্ত প্রশ্ন সংখ্যা</label>
      <input type="number" id="shortCount" value="5" min="1" max="10">
    </div>
    <div class="control-group">
      <label>🌐 ভাষা</label>
      <select id="langSelect">
        <option value="bn">বাংলা</option>
        <option value="en">English</option>
        <option value="both">বাংলা + English</option>
      </select>
    </div>
    <div class="control-group">
      <label>📊 কঠিনতার মাত্রা</label>
      <select id="diffSelect">
        <option value="easy">সহজ</option>
        <option value="medium" selected>মাঝারি</option>
        <option value="hard">কঠিন</option>
      </select>
    </div>
  </div>

  <!-- Generate Button -->
  <button class="gen-btn" id="genBtn" onclick="generateQuestions()">
    <span>🚀</span>
    <span>প্রশ্ন তৈরি করুন</span>
  </button>

  <!-- Loading -->
  <div class="loading" id="loading">
    <div class="spinner"></div>
    <p>ছবি বিশ্লেষণ করা হচ্ছে<span class="loading-dots"></span></p>
    <p style="font-size:0.8rem; margin-top:0.5rem; color:#475569;">AI প্রশ্ন তৈরি করছে, একটু অপেক্ষা করুন</p>
  </div>

  <!-- Results -->
  <div class="results" id="results" style="display:none;"></div>

</div>

<script>
let currentImageBase64 = null;
let currentImageType = null;
let generatedData = null;

// Drag & Drop
const uploadZone = document.getElementById('uploadZone');
uploadZone.addEventListener('dragover', e => { e.preventDefault(); uploadZone.classList.add('dragover'); });
uploadZone.addEventListener('dragleave', () => uploadZone.classList.remove('dragover'));
uploadZone.addEventListener('drop', e => {
  e.preventDefault();
  uploadZone.classList.remove('dragover');
  const file = e.dataTransfer.files[0];
  if (file) handleFile(file);
});

document.getElementById('fileInput').addEventListener('change', e => {
  if (e.target.files[0]) handleFile(e.target.files[0]);
});

function handleFile(file) {
  if (!file.type.startsWith('image/')) {
    showError('শুধুমাত্র ছবি ফাইল সাপোর্টেড!');
    return;
  }
  hideError();
  currentImageType = file.type;
  document.getElementById('fileName').textContent = '📄 ' + file.name;

  const reader = new FileReader();
  reader.onload = e => {
    const base64 = e.target.result.split(',')[1];
    currentImageBase64 = base64;
    document.getElementById('previewImg').src = e.target.result;
    document.getElementById('previewContainer').classList.add('show');
    document.getElementById('uploadZone').style.display = 'none';
  };
  reader.readAsDataURL(file);
}

function removeImage() {
  currentImageBase64 = null;
  document.getElementById('previewContainer').classList.remove('show');
  document.getElementById('uploadZone').style.display = 'block';
  document.getElementById('fileInput').value = '';
  document.getElementById('results').style.display = 'none';
}

async function generateQuestions() {
  if (!currentImageBase64) {
    showError('প্রথমে একটি ছবি আপলোড করুন!');
    return;
  }

  const mcqCount = document.getElementById('mcqCount').value;
  const shortCount = document.getElementById('shortCount').value;
  const lang = document.getElementById('langSelect').value;
  const diff = document.getElementById('diffSelect').value;

  const langText = lang === 'bn' ? 'বাংলায়' : lang === 'en' ? 'in English' : 'in both Bengali and English';
  const diffText = diff === 'easy' ? 'সহজ' : diff === 'medium' ? 'মাঝারি' : 'কঠিন';

  hideError();
  document.getElementById('loading').classList.add('show');
  document.getElementById('results').style.display = 'none';
  document.getElementById('genBtn').disabled = true;

  const prompt = `এই ছবিতে যা লেখা আছে তা পড়ো এবং সেখান থেকে প্রশ্ন তৈরি করো।

নির্দেশনা:
- ${mcqCount}টি MCQ প্রশ্ন তৈরি করো (৪টি অপশন সহ, সঠিক উত্তর চিহ্নিত করো)
- ${shortCount}টি এক কথায় উত্তর প্রশ্ন তৈরি করো
- প্রশ্নগুলো ${langText} লিখো
- কঠিনতার মাত্রা: ${diffText}

শুধুমাত্র JSON ফরম্যাটে উত্তর দাও, অন্য কিছু লিখবে না:
{
  "topic": "বিষয়ের নাম",
  "mcq": [
    {
      "question": "প্রশ্ন",
      "options": ["ক) ...", "খ) ...", "গ) ...", "ঘ) ..."],
      "correct": 0,
      "explanation": "সংক্ষিপ্ত ব্যাখ্যা"
    }
  ],
  "short": [
    {
      "question": "প্রশ্ন",
      "answer": "উত্তর"
    }
  ]
}`;

  try {
    const response = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        model: "claude-sonnet-4-20250514",
        max_tokens: 4000,
        messages: [{
          role: "user",
          content: [
            {
              type: "image",
              source: {
                type: "base64",
                media_type: currentImageType || "image/jpeg",
                data: currentImageBase64
              }
            },
            { type: "text", text: prompt }
          ]
        }]
      })
    });

    const data = await response.json();
    const text = data.content.map(i => i.text || '').join('');
    
    // Parse JSON
    const clean = text.replace(/```json|```/g, '').trim();
    const jsonMatch = clean.match(/\{[\s\S]*\}/);
    if (!jsonMatch) throw new Error('JSON পার্স করা যাচ্ছে না');
    
    generatedData = JSON.parse(jsonMatch[0]);
    renderResults(generatedData);

  } catch (err) {
    showError('প্রশ্ন তৈরি করতে সমস্যা হয়েছে: ' + err.message);
  } finally {
    document.getElementById('loading').classList.remove('show');
    document.getElementById('genBtn').disabled = false;
  }
}

function renderResults(data) {
  const mcqs = data.mcq || [];
  const shorts = data.short || [];
  const topic = data.topic || 'বিষয়';

  let html = `
    <div class="results-header">
      <h2>📚 ${topic}</h2>
      <div class="stats">
        <span class="stat-badge stat-mcq">MCQ: ${mcqs.length}</span>
        <span class="stat-badge stat-short">সংক্ষিপ্ত: ${shorts.length}</span>
      </div>
    </div>
  `;

  // MCQ Section
  if (mcqs.length > 0) {
    html += `<div class="section-title"><div class="section-icon mcq-icon">🎯</div> বহুনির্বাচনী প্রশ্ন (MCQ)</div>`;
    mcqs.forEach((q, i) => {
      const opts = q.options || [];
      html += `
        <div class="mcq-card">
          <div class="question-num">প্রশ্ন ${i + 1}</div>
          <div class="question-text">${q.question}</div>
          <div class="options">
            ${opts.map((opt, oi) => `
              <div class="option ${oi === q.correct ? 'correct' : ''}">
                <span class="option-label">${['ক','খ','গ','ঘ'][oi]}</span>
                <span>${opt.replace(/^[ক-ঘ][)।\s]+/, '')}</span>
              </div>
            `).join('')}
          </div>
          <div class="answer-reveal" id="ans-${i}">
            ✅ সঠিক উত্তর: ${opts[q.correct] || ''} ${q.explanation ? `— ${q.explanation}` : ''}
          </div>
          <button class="show-ans-btn" onclick="toggleAnswer('ans-${i}', this)">উত্তর দেখুন</button>
        </div>`;
    });
  }

  // Short Answer Section
  if (shorts.length > 0) {
    html += `<div class="section-title"><AI-question-maker
