<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>PrintFlow Prototype</title>
  <!-- Tailwind CSS via CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
</head>
<body class="bg-slate-50 text-slate-800 font-sans min-h-screen flex flex-col">

  <!-- TOP NAVIGATION BAR -->
  <header class="bg-white border-b border-slate-200 sticky top-0 z-30">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex items-center justify-between h-16">
      <div class="flex items-center gap-3">
        <div class="w-9 h-9 rounded-lg bg-indigo-600 flex items-center justify-center text-white shadow-sm shadow-indigo-200">
          <i data-lucide="layers" class="w-5 h-5"></i>
        </div>
        <div>
          <span class="font-bold text-lg text-slate-900 tracking-tight">Print<span class="text-indigo-600">Flow</span></span>
          <span class="ml-2 text-xs bg-indigo-50 text-indigo-700 font-medium px-2 py-0.5 rounded-full border border-indigo-100">Prototype v1.0</span>
        </div>
      </div>

      <!-- Mode Switcher -->
      <nav class="flex items-center gap-1 bg-slate-100 p-1 rounded-xl">
        <button id="btn-tab-intake" onclick="switchView('intake')" class="flex items-center gap-2 px-3 py-1.5 text-sm font-medium rounded-lg transition bg-white text-slate-900 shadow-sm">
          <i data-lucide="file-plus" class="w-4 h-4"></i>
          <span>Order Intake</span>
        </button>
        <button id="btn-tab-queue" onclick="switchView('queue')" class="flex items-center gap-2 px-3 py-1.5 text-sm font-medium rounded-lg transition text-slate-600 hover:text-slate-900">
          <i data-lucide="kanban" class="w-4 h-4"></i>
          <span>Production Queue</span>
          <span id="badge-queue-count" class="ml-1 text-xs bg-indigo-100 text-indigo-700 px-1.5 py-0.2 rounded-full font-semibold">3</span>
        </button>
      </nav>
    </div>
  </header>

  <!-- MAIN CONTAINER -->
  <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8">

    <!-- ================= VIEW 1: ORDER INTAKE ================= -->
    <section id="view-intake" class="block">
      <div class="max-w-4xl mx-auto">
        <div class="mb-6">
          <h1 class="text-2xl font-bold text-slate-900">Create New Print Job</h1>
          <p class="text-sm text-slate-500">Configure your print specifications, calculate estimated costs, and send directly to production.</p>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
          <!-- Form Section -->
          <div class="lg:col-span-2 space-y-6">
            
            <!-- File Upload Zone -->
            <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm">
              <label class="block text-sm font-semibold text-slate-800 mb-2">Upload File</label>
              <div id="dropzone" class="border-2 border-dashed border-slate-300 hover:border-indigo-500 rounded-xl p-6 text-center cursor-pointer transition bg-slate-50 hover:bg-indigo-50/20">
                <input type="file" id="file-input" class="hidden" accept=".pdf,.png,.jpg,.jpeg,.ai,.psd" onchange="handleFileSelect(event)" />
                <div class="flex flex-col items-center">
                  <i data-lucide="upload-cloud" class="w-10 h-10 text-slate-400 mb-2"></i>
                  <p class="text-sm font-medium text-slate-700" id="file-label">Click or drag & drop files here</p>
                  <p class="text-xs text-slate-400 mt-1">PDF, AI, PSD, PNG, or JPG (Up to 100MB)</p>
                </div>
              </div>
            </div>

            <!-- Job Details -->
            <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm space-y-4">
              <h2 class="text-sm font-semibold text-slate-800 border-b border-slate-100 pb-3">Print Specifications</h2>
              
              <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                <div>
                  <label class="block text-xs font-semibold uppercase tracking-wider text-slate-500 mb-1">Customer / Project Name</label>
                  <input type="text" id="project-name" placeholder="e.g., Marketing Flyers Batch 1" class="w-full px-3 py-2 text-sm border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500" value="Acme Corp Annual Report" />
                </div>
                <div>
                  <label class="block text-xs font-semibold uppercase tracking-wider text-slate-500 mb-1">Paper Size</label>
                  <select id="paper-size" onchange="calculateTotal()" class="w-full px-3 py-2 text-sm border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    <option value="A4">A4 (8.27" x 11.69")</option>
                    <option value="Letter">Letter (8.5" x 11")</option>
                    <option value="A3">A3 (11.69" x 16.54")</option>
                    <option value="Poster">18" x 24" Poster</option>
                  </select>
                </div>
              </div>

              <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                <div>
                  <label class="block text-xs font-semibold uppercase tracking-wider text-slate-500 mb-1">Color Mode</label>
                  <select id="color-mode" onchange="calculateTotal()" class="w-full px-3 py-2 text-sm border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    <option value="full-color">Full Color (CMYK)</option>
                    <option value="monochrome">Monochrome / Black & White</option>
                  </select>
                </div>
                <div>
                  <label class="block text-xs font-semibold uppercase tracking-wider text-slate-500 mb-1">Paper Stock</label>
                  <select id="paper-stock" onchange="calculateTotal()" class="w-full px-3 py-2 text-sm border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    <option value="80gsm">Standard 80gsm Bond</option>
                    <option value="150gsm">150gsm Gloss Text</option>
                    <option value="300gsm">300gsm Heavy Cardstock</option>
                  </select>
                </div>
              </div>

              <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 pt-2">
                <div>
                  <label class="block text-xs font-semibold uppercase tracking-wider text-slate-500 mb-1">Quantity</label>
                  <input type="number" id="quantity" min="1" max="10000" value="100" onchange="calculateTotal()" oninput="calculateTotal()" class="w-full px-3 py-2 text-sm border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500" />
                </div>
                <div>
                  <label class="block text-xs font-semibold uppercase tracking-wider text-slate-500 mb-1">Finishing / Binding</label>
                  <select id="finishing" onchange="calculateTotal()" class="w-full px-3 py-2 text-sm border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    <option value="none">None (Cut to Size)</option>
                    <option value="stapled">Corner Stapled</option>
                    <option value="spiral">Spiral / Wire-O Bound</option>
                    <option value="laminated">Matte Lamination</option>
                  </select>
                </div>
              </div>
            </div>
          </div>

          <!-- Order Summary Card -->
          <div class="space-y-6">
            <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm sticky top-24">
              <h3 class="text-base font-bold text-slate-900 mb-4">Summary & Estimate</h3>
              
              <div class="space-y-3 text-sm border-b border-slate-100 pb-4">
                <div class="flex justify-between text-slate-600">
                  <span>Base Print Cost:</span>
                  <span id="summary-base">$25.00</span>
                </div>
                <div class="flex justify-between text-slate-600">
                  <span>Stock & Finishing:</span>
                  <span id="summary-addons">$12.00</span>
                </div>
                <div class="flex justify-between text-slate-600">
                  <span>Est. Turnaround:</span>
                  <span class="text-slate-900 font-medium">1-2 Business Days</span>
                </div>
              </div>

              <div class="flex items-baseline justify-between py-4">
                <span class="text-base font-semibold text-slate-900">Estimated Total:</span>
                <span id="summary-total" class="text-2xl font-black text-indigo-600">$37.00</span>
              </div>

              <button onclick="submitJob()" class="w-full py-3 px-4 bg-indigo-600 hover:bg-indigo-700 text-white font-medium rounded-xl shadow-sm transition flex items-center justify-center gap-2">
                <i data-lucide="send" class="w-4 h-4"></i>
                <span>Send to Production</span>
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ================= VIEW 2: PRODUCTION QUEUE (KANBAN) ================= -->
    <section id="view-queue" class="hidden">
      <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 mb-6">
        <div>
          <h1 class="text-2xl font-bold text-slate-900">Production Workflow</h1>
          <p class="text-sm text-slate-500">Track and advance print jobs across each workstation.</p>
        </div>
        <div class="flex items-center gap-2">
          <input type="text" id="queue-search" oninput="filterJobs()" placeholder="Search jobs or customers..." class="px-3 py-1.5 text-sm border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 bg-white" />
        </div>
      </div>

      <!-- Kanban Board -->
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 items-start">
        
        <!-- Stage 1: Received / Pre-Press -->
        <div class="bg-slate-100/70 p-3 rounded-2xl border border-slate-200">
          <div class="flex items-center justify-between px-2 py-1.5 mb-2">
            <div class="flex items-center gap-2">
              <span class="w-2.5 h-2.5 rounded-full bg-amber-500"></span>
              <h3 class="text-xs font-bold uppercase tracking-wider text-slate-700">Pre-Press / Review</h3>
            </div>
            <span id="count-prepress" class="text-xs font-semibold bg-slate-200 text-slate-600 px-2 py-0.5 rounded-full">0</span>
          </div>
          <div id="col-prepress" class="space-y-3 min-h-[350px]"></div>
        </div>

        <!-- Stage 2: In Print -->
        <div class="bg-slate-100/70 p-3 rounded-2xl border border-slate-200">
          <div class="flex items-center justify-between px-2 py-1.5 mb-2">
            <div class="flex items-center gap-2">
              <span class="w-2.5 h-2.5 rounded-full bg-blue-500"></span>
              <h3 class="text-xs font-bold uppercase tracking-wider text-slate-700">Printing</h3>
            </div>
            <span id="count-printing" class="text-xs font-semibold bg-slate-200 text-slate-600 px-2 py-0.5 rounded-full">0</span>
          </div>
          <div id="col-printing" class="space-y-3 min-h-[350px]"></div>
        </div>

        <!-- Stage 3: Finishing & Binding -->
        <div class="bg-slate-100/70 p-3 rounded-2xl border border-slate-200">
          <div class="flex items-center justify-between px-2 py-1.5 mb-2">
            <div class="flex items-center gap-2">
              <span class="w-2.5 h-2.5 rounded-full bg-purple-500"></span>
              <h3 class="text-xs font-bold uppercase tracking-wider text-slate-700">Finishing</h3>
            </div>
            <span id="count-finishing" class="text-xs font-semibold bg-slate-200 text-slate-600 px-2 py-0.5 rounded-full">0</span>
          </div>
          <div id="col-finishing" class="space-y-3 min-h-[350px]"></div>
        </div>

        <!-- Stage 4: Ready for Pickup / Shipped -->
        <div class="bg-slate-100/70 p-3 rounded-2xl border border-slate-200">
          <div class="flex items-center justify-between px-2 py-1.5 mb-2">
            <div class="flex items-center gap-2">
              <span class="w-2.5 h-2.5 rounded-full bg-emerald-500"></span>
              <h3 class="text-xs font-bold uppercase tracking-wider text-slate-700">Ready / Done</h3>
            </div>
            <span id="count-done" class="text-xs font-semibold bg-slate-200 text-slate-600 px-2 py-0.5 rounded-full">0</span>
          </div>
          <div id="col-done" class="space-y-3 min-h-[350px]"></div>
        </div>

      </div>
    </section>

  </main>

  <!-- JAVASCRIPT LOGIC -->
  <script>
    // Initial Seed Data
    let jobs = [
      {
        id: 'PF-1001',
        title: 'Tech Summit Name Badges',
        size: 'A4',
        stock: '300gsm Heavy Cardstock',
        qty: 250,
        color: 'Full Color',
        stage: 'prepress'
      },
      {
        id: 'PF-1002',
        title: 'Local Café Menus',
        size: 'Letter',
        stock: '150gsm Gloss Text',
        qty: 50,
        color: 'Full Color',
        stage: 'printing'
      },
      {
        id: 'PF-1003',
        title: 'Architecture Blueprint Set',
        size: 'A3',
        stock: 'Standard 80gsm',
        qty: 12,
        color: 'Monochrome',
        stage: 'finishing'
      }
    ];

    // Initialize Lucide icons
    lucide.createIcons();

    // Tab Navigation
    function switchView(view) {
      const intakeView = document.getElementById('view-intake');
      const queueView = document.getElementById('view-queue');
      const btnIntake = document.getElementById('btn-tab-intake');
      const btnQueue = document.getElementById('btn-tab-queue');

      if (view === 'intake') {
        intakeView.classList.remove('hidden');
        queueView.classList.add('hidden');
        btnIntake.className = "flex items-center gap-2 px-3 py-1.5 text-sm font-medium rounded-lg transition bg-white text-slate-900 shadow-sm";
        btnQueue.className = "flex items-center gap-2 px-3 py-1.5 text-sm font-medium rounded-lg transition text-slate-600 hover:text-slate-900";
      } else {
        intakeView.classList.add('hidden');
        queueView.classList.remove('hidden');
        btnQueue.className = "flex items-center gap-2 px-3 py-1.5 text-sm font-medium rounded-lg transition bg-white text-slate-900 shadow-sm";
        btnIntake.className = "flex items-center gap-2 px-3 py-1.5 text-sm font-medium rounded-lg transition text-slate-600 hover:text-slate-900";
        renderQueue();
      }
    }

    // Drag-and-drop / File Selector trigger
    document.getElementById('dropzone').addEventListener('click', () => {
      document.getElementById('file-input').click();
    });

    function handleFileSelect(event) {
      const file = event.target.files[0];
      if (file) {
        document.getElementById('file-label').innerHTML = `<span class="text-indigo-600 font-semibold">${file.name}</span> (${(file.size / (1024*1024)).toFixed(2)} MB)`;
      }
    }

    // Live Price Estimator
    function calculateTotal() {
      const qty = parseInt(document.getElementById('quantity').value) || 1;
      const color = document.getElementById('color-mode').value;
      const stock = document.getElementById('paper-stock').value;
      const finishing = document.getElementById('finishing').value;

      let basePerUnit = color === 'full-color' ? 0.25 : 0.08;
      let stockPerUnit = stock === '300gsm' ? 0.15 : (stock === '150gsm' ? 0.08 : 0.02);
      let finishFlat = finishing === 'spiral' ? 2.50 : (finishing === 'laminated' ? 1.00 : 0.00);

      const baseTotal = (basePerUnit * qty);
      const addonsTotal = (stockPerUnit * qty) + finishFlat;
      const total = baseTotal + addonsTotal;

      document.getElementById('summary-base').innerText = `$${baseTotal.toFixed(2)}`;
      document.getElementById('summary-addons').innerText = `$${addonsTotal.toFixed(2)}`;
      document.getElementById('summary-total').innerText = `$${total.toFixed(2)}`;
    }

    // Order Submission into Kanban
    function submitJob() {
      const title = document.getElementById('project-name').value || 'Untitled Job';
      const size = document.getElementById('paper-size').value;
      const stock = document.getElementById('paper-stock').options[document.getElementById('paper-stock').selectedIndex].text;
      const qty = parseInt(document.getElementById('quantity').value) || 10;
      const color = document.getElementById('color-mode').value === 'full-color' ? 'Full Color' : 'B&W';

      const newJob = {
        id: `PF-${Math.floor(1000 + Math.random() * 9000)}`,
        title: title,
        size: size,
        stock: stock,
        qty: qty,
        color: color,
        stage: 'prepress'
      };

      jobs.unshift(newJob);
      updateBadge();
      switchView('queue');
    }

    // Advance Job Stages
    function advanceStage(jobId) {
      const job = jobs.find(j => j.id === jobId);
      if (!job) return;

      const order = ['prepress', 'printing', 'finishing', 'done'];
      const currentIndex = order.indexOf(job.stage);
      if (currentIndex < order.length - 1) {
        job.stage = order[currentIndex + 1];
        renderQueue();
      }
    }

    function removeJob(jobId) {
      jobs = jobs.filter(j => j.id !== jobId);
      updateBadge();
      renderQueue();
    }

    function updateBadge() {
      const activeCount = jobs.filter(j => j.stage !== 'done').length;
      document.getElementById('badge-queue-count').innerText = activeCount;
    }

    // Render Kanban Cards
    function renderQueue(filterTerm = '') {
      const columns = {
        prepress: document.getElementById('col-prepress'),
        printing: document.getElementById('col-printing'),
        finishing: document.getElementById('col-finishing'),
        done: document.getElementById('col-done')
      };

      // Reset columns
      Object.keys(columns).forEach(key => columns[key].innerHTML = '');

      const filtered = jobs.filter(job => 
        job.title.toLowerCase().includes(filterTerm.toLowerCase()) ||
        job.id.toLowerCase().includes(filterTerm.toLowerCase())
      );

      // Populate counts & cards
      const counts = { prepress: 0, printing: 0, finishing: 0, done: 0 };

      filtered.forEach(job => {
        counts[job.stage]++;
        const col = columns[job.stage];
        if (!col) return;

        const nextLabelMap = {
          prepress: 'Print Now',
          printing: 'Send to Finishing',
          finishing: 'Mark Ready'
        };

        const card = document.createElement('div');
        card.className = "bg-white p-4 rounded-xl border border-slate-200 shadow-sm hover:shadow transition space-y-3";
        card.innerHTML = `
          <div class="flex items-start justify-between">
            <span class="text-xs font-bold text-indigo-600 bg-indigo-50 px-2 py-0.5 rounded border border-indigo-100">${job.id}</span>
            <button onclick="removeJob('${job.id}')" class="text-slate-400 hover:text-red-500 text-xs">
              <i data-lucide="trash-2" class="w-3.5 h-3.5"></i>
            </button>
          </div>
          <div>
            <h4 class="font-semibold text-slate-800 text-sm leading-snug">${job.title}</h4>
            <div class="flex flex-wrap gap-1 mt-1 text-[11px] text-slate-500">
              <span class="bg-slate-100 px-1.5 py-0.5 rounded">${job.size}</span>
              <span class="bg-slate-100 px-1.5 py-0.5 rounded">${job.qty} units</span>
              <span class="bg-slate-100 px-1.5 py-0.5 rounded">${job.color}</span>
            </div>
          </div>
          ${job.stage !== 'd# Print
