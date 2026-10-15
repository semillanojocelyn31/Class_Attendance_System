<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>BSIT 3B Class Attendance System</title>
  
  <script src="https://cdn.tailwindcss.com"></script>
  
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

  <style>
    body {
      background: linear-gradient(135deg, #052e16 0%, #064e3b 50%, #022c22 100%);
      min-height: 100vh;
      color: #fef08a;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    .glass-card {
      background: rgba(6, 78, 59, 0.75);
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
      border: 1px solid rgba(234, 179, 8, 0.3);
      box-shadow: 0 0 25px rgba(22, 101, 52, 0.5);
    }
   
    .toggle-checkbox:checked {
      right: 0;
      border-color: #facc15;
    }
    .toggle-checkbox:checked + .toggle-label {
      background-color: #f59e0b;
      box-shadow: 0 0 12px #facc15;
    }
  </style>
</head>
<body class="p-4 md:p-8">

  <div class="max-w-5xl mx-auto space-y-6">

    
    <header class="glass-card rounded-2xl p-6 shadow-2xl flex flex-col md:flex-row justify-between items-center gap-4 border-b-4 border-yellow-400">
      <div class="flex items-center gap-4">
        <img src="logo.jpg" alt="CPSU Logo" class="w-16 h-16 object-contain drop-shadow-[0_0_10px_rgba(250,204,21,0.6)]" />
        <div>
          <h1 class="text-2xl font-extrabold tracking-wide text-yellow-300 drop-shadow">BSIT 3B Attendance System</h1>
          <p class="text-xs text-yellow-100 font-medium">Central Philippines State University Moises Padilla Campus</p>
        </div>
      </div>
      <div class="flex flex-wrap items-center gap-3">
        <button onclick="exportDocumentationPDF()" class="bg-yellow-500 hover:bg-yellow-400 text-emerald-950 px-4 py-2 rounded-xl text-sm font-bold transition flex items-center gap-2 shadow-lg shadow-yellow-500/30">
          <i class="fa-solid fa-file-pdf"></i> Export PDF Doc
        </button>
        <div class="bg-emerald-950/80 border border-yellow-500/40 px-4 py-2 rounded-xl text-right">
          <div id="currentDate" class="text-xs text-yellow-200">---</div>
          <div id="currentTime" class="text-sm font-bold text-yellow-400">--:--:--</div>
        </div>
      </div>
    </header>

    
    <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
      <div class="glass-card p-4 rounded-2xl border-l-4 border-l-yellow-400">
        <p class="text-xs uppercase font-bold text-yellow-200">Total Enrolled</p>
        <p id="totalCount" class="text-3xl font-black text-yellow-300 mt-1">18</p>
      </div>
      <div class="glass-card p-4 rounded-2xl border-l-4 border-l-emerald-400">
        <p class="text-xs uppercase font-bold text-emerald-200">Present Count</p>
        <p id="presentCount" class="text-3xl font-black text-emerald-300 mt-1">0</p>
      </div>
      <div class="glass-card p-4 rounded-2xl border-l-4 border-l-rose-400 col-span-2 md:col-span-1">
        <p class="text-xs uppercase font-bold text-rose-200">Absent Count</p>
        <p id="absentCount" class="text-3xl font-black text-rose-300 mt-1">18</p>
      </div>
    </div>

    
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">

      
      <div class="glass-card rounded-2xl p-5 space-y-4">
        <div class="flex justify-between items-center border-b border-yellow-500/30 pb-3">
          <h2 class="text-lg font-bold text-yellow-300 flex items-center gap-2">
            <i class="fa-solid fa-mars text-yellow-400"></i> Boys <span class="text-xs bg-yellow-500/20 text-yellow-300 border border-yellow-500/30 px-2 py-0.5 rounded-full font-normal">9 Students</span>
          </h2>
          <button onclick="markGroup('boys', true)" class="text-xs bg-yellow-500/20 hover:bg-yellow-500/30 text-yellow-300 border border-yellow-500/30 px-2.5 py-1 rounded-lg transition">All Present</button>
        </div>
        <div id="boysList" class="space-y-2.5 max-h-[450px] overflow-y-auto pr-1">
          
        </div>
      </div>

      
      <div class="glass-card rounded-2xl p-5 space-y-4">
        <div class="flex justify-between items-center border-b border-yellow-500/30 pb-3">
          <h2 class="text-lg font-bold text-yellow-300 flex items-center gap-2">
            <i class="fa-solid fa-venus text-yellow-400"></i> Girls <span class="text-xs bg-yellow-500/20 text-yellow-300 border border-yellow-500/30 px-2 py-0.5 rounded-full font-normal">9 Students</span>
          </h2>
          <button onclick="markGroup('girls', true)" class="text-xs bg-yellow-500/20 hover:bg-yellow-500/30 text-yellow-300 border border-yellow-500/30 px-2.5 py-1 rounded-lg transition">All Present</button>
        </div>
        <div id="girlsList" class="space-y-2.5 max-h-[450px] overflow-y-auto pr-1">
          
        </div>
      </div>

    </div>

    
    <div class="glass-card rounded-2xl p-6 space-y-4">
      <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-3 border-b border-yellow-500/30 pb-3">
        <div>
          <h3 class="text-lg font-bold text-yellow-300">Attendance Summary</h3>
          <p id="summarySessionDate" class="text-xs text-yellow-200">Class Session | ---</p>
        </div>
        <button onclick="copyAbsentList()" class="bg-emerald-600 hover:bg-emerald-500 text-white px-4 py-2 rounded-xl text-xs font-semibold transition flex items-center gap-2 shadow-md">
          <i class="fa-solid fa-clipboard"></i> Copy Absent List to Clipboard
        </button>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div class="bg-emerald-950/60 p-4 rounded-xl border border-emerald-500/30">
          <h4 id="presentSummaryHeader" class="font-bold text-emerald-300 text-sm mb-2">Present: 0</h4>
          <ul id="presentNamesList" class="text-xs text-emerald-200 space-y-1 list-disc pl-4">
            
          </ul>
        </div>
        <div class="bg-rose-950/60 p-4 rounded-xl border border-rose-500/30">
          <h4 id="absentSummaryHeader" class="font-bold text-rose-300 text-sm mb-2">Absent: 18</h4>
          <ul id="absentNamesList" class="text-xs text-rose-200 space-y-1 list-disc pl-4">
            
          </ul>
        </div>
      </div>
    </div>

  </div>

  
  <div id="pdfDocContainer" class="hidden bg-white text-slate-900 p-8 space-y-6">
    <div class="border-b pb-4">
      <h1 class="text-2xl font-bold text-slate-900">BSIT 3B Class Attendance System</h1>
      <p class="text-sm text-slate-600">Application Documentation & Activity Submission</p>
    </div>

    <div class="grid grid-cols-2 gap-4 text-sm bg-slate-50 p-4 rounded-lg border">
      <div>
        <p><strong>Developed Application:</strong> BSIT 3B Class Attendance System</p>
        <p><strong>Course:</strong> Application Development & Emerging Technologies</p>
      </div>
      <div>
        <p><strong>Student Name:</strong> Jocelyn Semillano</p>
        <p><strong>Year & Section:</strong> BSIT 3B</p>
        <p><strong>Instructor:</strong> Sir Mike</p>
      </div>
    </div>

    <div>
      <h2 class="text-lg font-bold text-slate-800 border-b pb-1 mb-2">Application Purpose & Functionality</h2>
      <p class="text-sm text-slate-700 leading-relaxed">
        The BSIT 3B Class Attendance System is an aesthetic web application styled in Central Philippines State University's official color palette. It features categorized boys and girls rosters with interactive neon toggle switches, real-time metrics, live summary lists, and clipboard export capabilities for efficient classroom management.
      </p>
    </div>

    <div class="space-y-4">
      <h2 class="text-lg font-bold text-slate-800 border-b pb-1">Interface Overview & Workflow Screenshots</h2>
      
      <div class="border p-3 rounded-lg bg-slate-50">
        <h3 class="font-bold text-sm text-slate-800 mb-1">1. Categorized Attendance Roster & Toggle Switches</h3>
        <p class="text-xs text-slate-600 mb-2">
          Organized sections for boys and girls equipped with glowing toggle switches to easily record attendance status in real time.
        </p>
      </div>

      <div class="border p-3 rounded-lg bg-slate-50">
        <h3 class="font-bold text-sm text-slate-800 mb-1">2. Attendance Summary & Clipboard Export</h3>
        <p class="text-xs text-slate-600 mb-2">
          Dynamic summary counters and bulleted lists automatically categorize present and absent students with one-click clipboard copying.
        </p>
      </div>
    </div>
  </div>

  <script>
    
    const boys = [
      "Jero Esteban",
      "John Rogen Bullag",
      "Kenneth Ortega Espinosa",
      "Luke Axel Barroccum",
      "Mario Nebres Jr.",
      "Thurd Catague",
      "Tisoy Maque",
      "Veecemark De Jesus",
      "Aj Marfil"
    ];

    const girls = [
      "Jaaz Eniah Castillo",
      "Khyla Albaracin",
      "Kristine Joy Basa",
      "Lene De La Llave",
      "Rechelle An Casilangan",
      "Shannon Gale Delacruz",
      "Tina Cabrillos",
      "Trisha Agravante",
      "Jocelyn Semillano"
    ];

    
    const attendanceState = {};
    [...boys, ...girls].forEach(name => {
      attendanceState[name] = false; // default absent
    });

    function renderLists() {
      
      const boysContainer = document.getElementById("boysList");
      boysContainer.innerHTML = "";
      boys.forEach(name => {
        const isPresent = attendanceState[name];
        const div = document.createElement("div");
        div.className = `flex justify-between items-center p-3 rounded-xl transition border ${isPresent ? 'bg-emerald-950/40 border-emerald-500/40 shadow-[0_0_10px_rgba(16,185,129,0.2)]' : 'bg-emerald-900/20 border-yellow-500/20'}`;
        div.innerHTML = `
          <span class="text-sm font-semibold text-yellow-100">${name}</span>
          <label class="relative inline-flex items-center cursor-pointer">
            <input type="checkbox" class="sr-only peer" ${isPresent ? 'checked' : ''} onchange="toggleAttendance('${name}')">
            <div class="w-11 h-6 bg-emerald-950 border border-yellow-500/40 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-yellow-500 peer-checked:border-yellow-400"></div>
          </label>
        `;
        boysContainer.appendChild(div);
      });

      
      const girlsContainer = document.getElementById("girlsList");
      girlsContainer.innerHTML = "";
      girls.forEach(name => {
        const isPresent = attendanceState[name];
        const div = document.createElement("div");
        div.className = `flex justify-between items-center p-3 rounded-xl transition border ${isPresent ? 'bg-emerald-950/40 border-emerald-500/40 shadow-[0_0_10px_rgba(16,185,129,0.2)]' : 'bg-emerald-900/20 border-yellow-500/20'}`;
        div.innerHTML = `
          <span class="text-sm font-semibold text-yellow-100">${name}</span>
          <label class="relative inline-flex items-center cursor-pointer">
            <input type="checkbox" class="sr-only peer" ${isPresent ? 'checked' : ''} onchange="toggleAttendance('${name}')">
            <div class="w-11 h-6 bg-emerald-950 border border-yellow-500/40 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-yellow-500 peer-checked:border-yellow-400"></div>
          </label>
        `;
        girlsContainer.appendChild(div);
      });

      updateSummary();
    }

    function toggleAttendance(name) {
      attendanceState[name] = !attendanceState[name];
      updateSummary();
    }

    function markGroup(groupType, status) {
      const list = groupType === 'boys' ? boys : girls;
      list.forEach(name => {
        attendanceState[name] = status;
      });
      renderLists();
    }

    function updateSummary() {
      const allStudents = [...boys, ...girls];
      const presentList = allStudents.filter(name => attendanceState[name]);
      const absentList = allStudents.filter(name => !attendanceState[name]);

      document.getElementById("totalCount").innerText = allStudents.length;
      document.getElementById("presentCount").innerText = presentList.length;
      document.getElementById("absentCount").innerText = absentList.length;

      document.getElementById("presentSummaryHeader").innerText = `Present: ${presentList.length}`;
      document.getElementById("absentSummaryHeader").innerText = `Absent: ${absentList.length}`;

      const presentUl = document.getElementById("presentNamesList");
      presentUl.innerHTML = presentList.length > 0 ? presentList.map(n => `<li>${n}</li>`).join('') : '<li class="list-none text-emerald-400/60 italic">No students present</li>';

      const absentUl = document.getElementById("absentNamesList");
      absentUl.innerHTML = absentList.length > 0 ? absentList.map(n => `<li>${n}</li>`).join('') : '<li class="list-none text-rose-400/60 italic">No students absent</li>';
    }

    function copyAbsentList() {
      const allStudents = [...boys, ...girls];
      const absentList = allStudents.filter(name => !attendanceState[name]);
      const textToCopy = "Absent Students:\n" + absentList.map(n => `• ${n}`).join('\n');
      navigator.clipboard.writeText(textToCopy).then(() => {
        alert("Absent list copied to clipboard!");
      });
    }

    function updateClock() {
      const now = new Date();
      const dateStr = now.toLocaleDateString('en-US', { year: 'numeric', month: '2-digit', day: '2-digit' }).replace(/\//g, '-');
      document.getElementById("currentDate").innerText = now.toLocaleDateString('en-US', { weekday: 'short', month: 'short', day: 'numeric', year: 'numeric' });
      document.getElementById("summarySessionDate").innerText = `Class Session | ${dateStr}`;
      document.getElementById("currentTime").innerText = now.toLocaleTimeString('en-US', { hour12: true });
    }
    setInterval(updateClock, 1000);
    updateClock();

    function exportDocumentationPDF() {
      const element = document.getElementById('pdfDocContainer');
      element.classList.remove('hidden');
      
      const opt = {
        margin:       0.5,
        filename:     'BSIT_3B_Class_Attendance_System_Documentation.pdf',
        image:        { type: 'jpeg', quality: 0.98 },
        html2canvas:  { scale: 2 },
        jsPDF:        { unit: 'in', format: 'letter', orientation: 'portrait' }
      };

      html2pdf().set(opt).from(element).save().then(() => {
        element.classList.add('hidden');
      });
    }

    
    renderLists();
  </script>
</body>
</html>
