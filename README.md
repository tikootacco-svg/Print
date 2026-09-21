<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
  <title>PrintFlow Prototype</title>
  <!-- Tailwind CSS via CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* Smooth transitions & touch improvements */
    button, select, input { -webkit-tap-highlight-color: transparent; }
    .toast-animate {
      animation: slideDown 0.3s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }
    @keyframes slideDown {
      from { transform: translateY(-100%); opacity: 0; }
      to { transform: translateY(0); opacity: 1; }
    }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans min-h-screen flex flex-col selection:bg-indigo-500 selection:text-white pb-12">

  <!-- TOAST NOTIFICATION CONTAINER -->
  <div id="toast" class="fixed top-4 left-1/2 -translate-x-1/2 z-50 hidden">
    <div id="toast-body" class="bg-slate-900 text-white text-xs font-semibold px-4 py-2.5 rounded-full shadow-lg flex items-center gap-2 border border-slate-700">
      <svg class="w-4 h-4 text-emerald-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
      <span id="toast-text">Notification message</span>
    </div>
  </div>

  <!-- HEADER & NAVIGATION -->
  <header class="bg-white border-b border-slate-200 sticky top-0 z-30 shadow-xs">
    <div class="max-w-6xl mx-auto px-4 flex flex-col sm:flex-row items-center justify-between py-3 sm:h-16 gap-3">
      <div class="flex items-center justify-between w-full sm:w-auto">
        <div class="flex items-center gap-2.5">
          <div class="w-9 h-9 rounded-xl bg-indigo-600 flex items-center justify-center text-white shadow-md shadow-indigo-200">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10"></path></svg>
          </div>
          <span class="font-bold text-lg text-slate-900 tracking-tight">Print<span class="text-indigo-600">Flow</span></span>
        </div>
        <span class="text-[11px] bg-indigo-50 text-indigo-700 font-semibold px-2 py-0.5 rounded-full border border-indigo-100 sm:hidden">Prototype</span>
      </div>

      <!-- Tab Switcher Buttons -->
      <div class="flex items-center bg-slate-100 p-1 rounded-xl w-full sm:w-auto">
        <button type="button" id="btn-tab-intake" onclick="switchView('intake')" class="flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2 text-xs sm:text-sm font-semibold rounded-lg transition bg-white text-slate-900 shadow-sm">
          <svg class="w-4 h-4 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 13h6m-3-3v6m5 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg>
          <span>Order Intake</span>
        </button>
        <button type="button" id="btn-tab-queue" onclick="switchView('queue')" class="flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2 text-xs sm:text-sm font-semibold rounded-lg transition text-slate-600 hover:text-slate-900">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path></svg>
          <span>Queue</span>
          <span id="badge-queue-count" class="ml-1 text-[11px] bg-indigo-600 text-white px-1.5 py-0.2 rounded-full font-bold">3</span>
        </button>
      </div>
    </div>
  </header>

  <!-- CONTENT WRAPPER -->
  <main class="flex-1 max-w-6xl w-full mx-auto px-4 py-6">

    <!-- ================= VIEW 1: ORDER INTAKE ================= -->
    <section id="view-intake" class="block">
      <div class="max-w-3xl mx-auto space-y-6">
        <div>
          <h1 class="text-xl sm:text-2xl font-black text-slate-900">Create Print Job</h1>
          <p class="text-xs sm:text-sm text-slate-500 mt-1">Configure print specs, preview live pricing, and dispatch to production.</p>
        </div>

        <!-- File Upload Card -->
        <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
          <label class="block text-xs font-bold uppercase tracking-wider text-slate-500 mb-2">Upload Artwork</label>
          <div id="dropzone" onclick="document.getElementById('file-input').click()" class="border-2 border-dashed border-slate-200 hover:border-indigo-500 rounded-xl p-6 text-center cursor-pointer transition bg-slate-50 hover:bg-indigo-50/20 active:scale-[0.99]">
            <input type="file" id="file-input" class="hidden" accept=".pdf,.png,.jpg,.jpeg,.ai,.psd" onchange="handleFileSelect(event)" />
            <div class="flex flex-col items-center">
              <div class="w-10 h-10 rounded-full bg-indigo-50 text-indigo-600 flex items-center justify-center mb-2">
                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12"></path></svg>
              </div>
              <p class="text-sm font-semibold text-slate-700" id="file-label">Tap to upload file</p>
              <p class="text-xs text-slate-400 mt-1">PDF, AI, PNG, or JPG (Up to 100MB)</p>
            </div>
          </div>
        </div>

        <!-- Specifications Form -->
        <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm space-y-4">
          <h2 class="text-xs font-bold uppercase tracking-wider text-slate-500 border-b border-slate-100 pb-2">Job Options</h2>
          
          <div>
            <label class="block text-xs font-semibold text-slate-700 mb-1">Job Title / Client Name</label>
            <input type="text" id="project-name" value="Event Promo Flyers" class="w-full px-3.5 py-2.5 text-sm border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-500 font-medium" />
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-semibold text-slate-700 mb-1">Paper Format</label>
              <select id="paper-size" onchange="calculateTotal()" class="w-full px-3.5 py-2.5 text-sm border border-slate-200 rounded-xl bg-white focus:outline-none focus:ring-2 focus:ring-indigo-500">
                <option value="A4">A4 (8.27" x 11.69")</option>
                <option value="Letter">Letter (8.5" x 11")</option>
                <option value="A3">A3 (11.69" x 16.54")</option>
                <option value="Poster 18x24">Poster (18" x 24")</option>
              </select>
            </div>
            <div>
              <label class="block text-xs font-semibold text-slate-700 mb-1">Color Options</label>
              <select id="color-mode" onchange="calculateTotal()" class="w-full px-3.5 py-2.5 text-sm border border-slate-200 rounded-xl bg-white focus:outline-none focus:ring-2 focus:ring-indigo-500">
                <option value="full-color">Full Color (CMYK)</option>
                <option value="monochrome">Monochrome / Grayscale</option>
              </select>
            </div>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-semibold text-slate-700 mb-1">Paper Material</label>
              <select id="paper-stock" onchange="calculateTotal()" class="w-full px-3.5 py-2.5 text-sm border border-slate-200 rounded-xl bg-white focus:outline-none focus:ring-2 focus:ring-indigo-500">
                <option value="80gsm">Standard Bond 80gsm</option>
                <option value="150gsm">Gloss Text 150gsm</option>
                <option value="300gsm">Premium Cardstock 300gsm</option>
              </select>
            </div>
            <div>
              <label class="block text-xs font-semibold text-slate-700 mb-1">Finishing</label>
              <select id="finishing" onchange="calculateTotal()" class="w-full px-3.5 py-2.5 text-sm border border-slate-200 rounded-xl bg-white focus:outline-none focus:ring-2 focus:ring-indigo-500">
                <option value="none">Standard Cut (No Binding)</option>
                <option value="stapled">Corner Stapled</option>
                <option value="spiral">Wire-O / Spiral Binding</option>
                <option value="laminated">Matte Protective Lamination</option>
              </select>
            </div>
          </div>

          <div>
            <label class="block text-xs font-semibold text-slate-700 mb-1">Quantity (Units)</label>
            <input type="number" id="quantity" min="1" max="10000" value="100" oninput="calculateTotal()" onchange="calculateTotal()" class="w-full px-3.5 py-2.5 text-sm border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-500 font-semibold" />
          </div>
        </div>

        <!-- Live Price & Submit Button -->
        <div class="bg-indigo-900 text-white p-5 sm:p-6 rounded-2xl shadow-xl flex flex-col sm:flex-row items-center justify-between gap-4">
          <div>
            <span class="text-xs text-indigo-300 uppercase tracking-wider font-medium">Estimated Order Price</span>
            <div class="text-3xl font-black text-white" id="summary-total">$37.00</div>
            <p class="text-[11px] text-indigo-200 mt-0.5">Includes base printing, stock, and finishing</p>
          </div>
          <button type="button" onclick="submitJob()" class="w-full sm:w-auto px-6 py-3.5 bg-indigo-500 hover:bg-indigo-400 active:scale-95 text-white font-bold text-sm rounded-xl shadow-md transition flex items-center justify-center gap-2">
            <span>Send to Production</span>
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3"></path></svg>
          </button>
        </div>
      </div>
    </section>

    <!-- ================= VIEW 2: PRODUCTION QUEUE (KANBAN) ================= -->
    <section id="view-queue" class="hidden">
      <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 mb-6">
        <div>
          <h1 class="text-xl sm:text-2xl font-black text-slate-900">Workstation Pipeline</h1>
          <p class="text-xs sm:text-sm text-slate-500">Tap the arrow button on any card to move it to the next step.</p>
        </div>
        <div>
          <input type="text" id="queue-search" oninput="filterJobs()" placeholder="Search jobs by ID or title..." class="w-full sm:w-64 px-3.5 py-2 text-xs sm:text-sm border border-slate-200 rounded-xl bg-white focus:outline-none focus:ring-2 focus:ring-indigo-500" />
        </div>
      </div>

      <!-- Kanban Columns -->
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
        
        <!-- Pre-Press -->
        <div class="bg-slate-100/80 p-3.5 rounded-2xl border border-slate-200">
          <div class="flex items-center justify-between mb-3 px-1">
            <div class="flex items-center gap-2">
              <span class="w-2.5 h-2.5 rounded-full bg-amber-500"></span>
              <span class="text-xs font-bold text-slate-700 uppercase tracking-wider">1. Pre-Press</span>
            </div>
            <span id="count-prepress" class="text-xs font-bold bg-white text-slate-700 px-2 py-0.5 rounded-full border border-slate-200">0</span>
          </div>
          <div id="col-prepress" class="space-y-2.5 min-h-[150px]"></div>
        </div>

        <!-- Printing -->
        <div class="bg-slate-100/80 p-3.5 rounded-2xl border border-slate-200">
          <div class="flex items-center justify-between mb-3 px-1">
            <div class="flex items-center gap-2">
              <span class="w-2.5 h-2.5 rounded-full bg-blue-500"></span>
              <span class="text-xs font-bold text-slate-700 uppercase tracking-wider">2. Printing</span>
            </div>
            <span id="count-printing" class="text-xs font-bold bg-white text-slate-700 px-2 py-0.5 rounded-full border border-slate-200">0</span>
          </div>
          <div id="col-printing" class="space-y-2.5 min-h-[150px]"></div>
        </div>

        <!-- Finishing -->
        <div class="bg-slate-100/80 p-3.5 rounded-2xl border border-slate-200">
          <div class="flex items-center justify-between mb-3 px-1">
            <div class="flex items-center gap-2">
              <span class="w-2.5 h-2.5 rounded-full bg-purple-500"></span>
              <span class="text-xs font-bold text-slate-700 uppercase tracking-wider">3. Finishing</span>
            </div>
            <span id="count-finishing" class="text-xs font-bold bg-white text-slate-700 px-2 py-0.5 rounded-full border border-slate-200">0</span>
          </div>
          <div id="col-finishing" class="space-y-2.5 min-h-[150px]"></div>
        </div>

        <!-- Ready -->
        <div class="bg-slate-100/80 p-3.5 rounded-2xl border border-slate-200">
          <div class="flex items-center justify-between mb-3 px-1">
            <div class="flex items-center gap-2">
              <span class="w-2.5 h-2.5 rounded-full bg-emerald-500"></span>
              <span class="text-xs font-bold text-slate-700 uppercase tracking-wider">4. Ready</span>
            </div>
            <span id="count-done" class="text-xs font-bold bg-white text-slate-700 px-2 py-0.5 rounded-full border border-slate-200">0</span>
          </div>
          <div id="col-done" class="space-y-2.5 min-h-[150px]"></div>
        </div>

      </div>
    </section>

  </main>

  <!-- JAVASCRIPT ENGINE -->
  <script>
    // Live dataset
    window.jobs = [
      { id: 'PF-1001', title: 'Tech Summit Badges', size: 'A4', stock: 'Cardstock', qty: 250, color: 'Full Color', stage: 'prepress' },
      { id: 'PF-1002', title: 'Café Lunch Menus', size: 'Letter', stock: 'Gloss 150gsm', qty: 50, color: 'Full Color', stage: 'printing' },
      { id: 'PF-1003', title: 'Architect Plan Blueprints', size: 'A3', stock: 'Bond 80gsm', qty: 15, color: 'Grayscale', stage: 'finishing' }
    ];

    // Show interactive toast
    window.showToast = function(msg) {
      const toast = document.getElementById('toast');
      const text = document.getElementById('toast-text');
      text.innerText = msg;
      toast.classList.remove('hidden');
      toast.classList.add('toast-animate');
      setTimeout(() => {
        toast.classList.add('hidden');
        toast.classList.remove('toast-animate');
      }, 2400);
    };

    // Tab Navigation
    window.switchView = function(view) {
      const intakeView = document.getElementById('view-intake');
      const queueView = document.getElementById('view-queue');
      const btnIntake = document.getElementById('btn-tab-intake');
      const btnQueue = document.getElementById('btn-tab-queue');

      if (view === 'intake') {
        intakeView.classList.remove('hidden');
        queueView.classList.add('hidden');
        btnIntake.className = "flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2 text-xs sm:text-sm font-semibold rounded-lg transition bg-white text-slate-900 shadow-sm";
        btnQueue.className = "flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2 text-xs sm:text-sm font-semibold rounded-lg transition text-slate-600 hover:text-slate-900";
      } else {
        intakeView.classList.add('hidden');
        queueView.classList.remove('hidden');
        btnQueue.className = "flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2 text-xs sm:text-sm font-semibold rounded-lg transition bg-white text-slate-900 shadow-sm";
        btnIntake.className = "flex-1 sm:flex-initial flex items-center justify-center gap-2 px-4 py-2 text-xs sm:text-sm font-semibold rounded-lg transition text-slate-600 hover:text-slate-900";
        window.renderQueue();
      }
    };

    // File selection UI trigger
    window.handleFileSelect = function(event) {
      const file = event.target.files[0];
      if (file) {
        document.getElementById('file-label').innerHTML = `<span class="text-indigo-600 font-bold">${file.name}</span> (${(file.size / 1024).toFixed(1)} KB)`;
        window.showToast("File attached: " + file.name);
      }
    };

    // Dynamic cost calculator
    window.calculateTotal = function() {
      const qtyInput = document.getElementById('quantity');
      const qty = parseInt(qtyInput.value) || 1;
      const color = document.getElementById('color-mode').value;
      const stock = document.getElementById('paper-stock').value;
      const finishing = document.getElementById('finishing').value;

      const basePerUnit = color === 'full-color' ? 0.25 : 0.08;
      const stockPerUnit = stock === '300gsm' ? 0.15 : (stock === '150gsm' ? 0.08 : 0.02);
      const finishFlat = finishing === 'spiral' ? 2.50 : (finishing === 'laminated' ? 1.00 : 0.00);

      const total = (basePerUnit * qty) + (stockPerUnit * qty) + finishFlat;
      document.getElementById('summary-total').innerText = `$${total.toFixed(2)}`;
    };

    // Submit a new order
    window.submitJob = function() {
      const title = document.getElementById('project-name').value.trim() || 'Untitled Job';
      const size = document.getElementById('paper-size').value;
      const stockSelect = document.getElementById('paper-stock');
      const stock = stockSelect.options[stockSelect.selectedIndex].text.split(' ')[0];
      const qty = parseInt(document.getElementById('quantity').value) || 10;
      const color = document.getElementById('color-mode').value === 'full-color' ? 'Full Color' : 'Grayscale';

      const newId = `PF-${Math.floor(1000 + Math.random() * 9000)}`;
      const newJob = {
        id: newId,
        title: title,
        size: size,
        stock: stock,
        qty: qty,
        color: color,
        stage: 'prepress'
      };

      window.jobs.unshift(newJob);
      window.updateBadge();
      window.showToast(`Job ${newId} dispatched to Queue!`);
      window.switchView('queue');
    };

    // Move job forward in the pipeline
    window.advanceStage = function(jobId) {
      const job = window.jobs.find(j => j.id === jobId);
      if (!job) return;

      const stages = ['prepress', 'printing', 'finishing', 'done'];
      const currentIndex = stages.indexOf(job.stage);
      if (currentIndex < stages.length - 1) {
        job.stage = stages[currentIndex + 1];
        window.showToast(`${job.id} moved to ${job.stage.toUpperCase()}`);
        window.renderQueue();
      }
    };

    // Delete job
    window.removeJob = function(jobId) {
      window.jobs = window.jobs.filter(j => j.id !== jobId);
      window.updateBadge();
      window.showToast(`Job ${jobId} removed`);
      window.renderQueue();
    };

    // Update the counter on the tab
    window.updateBadge = function() {
      const count = window.jobs.filter(j => j.stage !== 'done').length;
      document.getElementById('badge-queue-count').innerText = count;
    };

    // Render cards into their respective stage column
    window.renderQueue = function(filterTerm = '') {
      const columns = {
        prepress: document.getElementById('col-prepress'),
        printing: document.getElementById('col-printing'),
        finishing: document.getElementById('col-finishing'),
        done: document.getElementById('col-done')
      };

      Object.keys(columns).forEach(key => {
        if (columns[key]) columns[key].innerHTML = '';
      });

      const filtered = window.jobs.filter(job => 
        job.title.toLowerCase().includes(filterTerm.toLowerCase()) ||
   
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
