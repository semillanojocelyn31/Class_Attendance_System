<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

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
  </style>
</head>

<body class="p-4 md:p-8">
  <div class="max-w-5xl mx-auto space-y-6">

    <!-- Header -->
    <header class="glass-card rounded-2xl p-6 shadow-2xl flex flex-col md:flex-row justify-between items-center gap-4 border-b-4 border-yellow-400">
      <div class="flex items-center gap-4">
        <img src="logo.jpg" alt="CPSU Logo" class="w-16 h-16 object-cover rounded-full border-2 border-yellow-400">

        <div>
          <h1 class="text-2xl font-extrabold tracking-wide text-yellow-300">
            BSIT 3B Attendance System
          </h1>
          <p class="text-xs text-yellow-100 font-medium">
            Central Philippines State University Moises Padilla Campus
          </p>
        </div>
      </div>

      <div class="flex flex-wrap items-center gap-3">
        <button onclick="exportDocumentationPDF()"
          class="bg-yellow-500 hover:bg-yellow-400 text-emerald-950 px-4 py-2 rounded-xl text-sm font-bold transition flex items-center gap-2">
          <i class="fa-solid fa-file-pdf"></i> Export PDF Doc
        </button>

        <div class="bg-emerald-950/80 border border-yellow-500/40 px-4 py-2 rounded-xl text-right">
          <div id="currentDate" class="text-xs text-yellow-200">---</div>
          <div id="currentTime" class="text-sm font-bold text-yellow-400">--:--:--</div>
        </div>
      </div>
    </header>

    <!-- Attendance Counters -->
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

    <!-- Student Lists -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">

      <!-- Boys -->
      <div class="glass-card rounded-2xl p-5 space-y-4">
        <div class="flex justify-between items-center border-b border-yellow-500/30 pb-3">
          <h2 class="text-lg font-bold text-yellow-300 flex items-center gap-2">
            <i class="fa-solid fa-mars text-yellow-400"></i>
            Boys
            <span class="text-xs bg-yellow-500/20 text-yellow-300 border border-yellow-500/30 px-2 py-0.5 rounded-full font-normal">
              9 Students
            </span>
          </h2>

          <button onclick="markGroup('boys', true)"
            class="text-xs bg-yellow-500/20 hover:bg-yellow-500/30 text-yellow-300 border border-yellow-500/30 px-2.5 py-1 rounded-lg">
            All Present
          </button>
        </div>

        <div id="boysList" class="space-y-2.5 max-h-[450px] overflow-y-auto pr-1"></div>
      </div>

      <!-- Girls -->
      <div class="glass-card rounded-2xl p-5 space-y-4">
        <div class="flex justify-between items-center border-b border-yellow-500/30 pb-3">
          <h2 class="text-lg font-bold text-yellow-300 flex items-center gap-2">
            <i class="fa-solid fa-venus text-yellow-400"></i>
            Girls
            <span class="text-xs bg-yellow-500/20 text-yellow-300 border border-yellow-500/30 px-2 py-0.5 rounded-full font-normal">
              9 Students
            </span>
          </h2>

          <button onclick="markGroup('girls', true)"
            class="text-xs bg-yellow-500/20 hover:bg-yellow-500/30 text-yellow-300 border border-yellow-500/30 px-2.5 py-1 rounded-lg">
            All Present
          </button>
        </div>

        <div id="girlsList" class="space-y-2.5 max-h-[450px] overflow-y-auto pr-1"></div>
      </div>
    </div>

    <!-- Attendance Summary -->
    <div class="glass-card rounded-2xl p-6 space-y-4">
      <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-3 border-b border-yellow-500/30 pb-3">
        <div>
          <h3 class="text-lg font-bold text-yellow-300">Attendance Summary</h3>
          <p id="summarySessionDate" class="text-xs text-yellow-200">Class Session | ---</p>
        </div>

        <button onclick="copyAbsentList()"
          class="bg-emerald-600 hover:bg-emerald-500 text-white px-4 py-2 rounded-xl text-xs font-semibold flex items-center gap-2">
          <i class="fa-solid fa-clipboard"></i>
          Copy Absent List to Clipboard
        </button>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div class="bg-emerald-950/60 p-4 rounded-xl border border-emerald-500/30">
          <h4 id="presentSummaryHeader" class="font-bold text-emerald-300 text-sm mb-2">Present: 0</h4>
          <ul id="presentNamesList" class="text-xs text-emerald-200 space-y-1 list-disc pl-4"></ul>
        </div>

        <div class="bg-rose-950/60 p-4 rounded-xl border border-rose-500/30">
          <h4 id="absentSummaryHeader" class="font-bold text-rose-300 text-sm mb-2">Absent: 18</h4>
          <ul id="absentNamesList" class="text-xs text-rose-200 space-y-1 list-disc pl-4"></ul>
        </div>
      </div>
    </div>
  </div>

  <!-- PDF Documentation Template -->
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
      attendanceState[name] = false;
    });

    function renderGroup(names, containerId) {
      const container = document.getElementById(containerId);
      container.innerHTML = "";

      names.forEach(name => {
        const isPresent = attendanceState[name];
        const div = document.createElement("div");

        div.className =
          `flex justify-between items-center p-3 rounded-xl transition border ${
            isPresent
              ? "bg-emerald-950/40 border-emerald-500/40"
              : "bg-emerald-900/20 border-yellow-500/20"
          }`;

        const nameLabel = document.createElement("span");
        nameLabel.className = "text-sm font-semibold text-yellow-100";
        nameLabel.textContent = name;

        const label = document.createElement("label");
        label.className = "inline-flex items-center gap-2 cursor-pointer";

        const checkbox = document.createElement("input");
        checkbox.type = "checkbox";
        checkbox.checked = isPresent;
        checkbox.className = "w-5 h-5 accent-yellow-400";
        checkbox.setAttribute("aria-label", "Mark " + name + " present");

        checkbox.addEventListener("change", () => {
          attendanceState[name] = checkbox.checked;
          updateSummary();
          renderGroup(names, containerId);
        });

        const status = document.createElement("span");
        status.className = "text-xs text-yellow-200";
        status.textContent = isPresent ? "Present" : "Absent";

        label.appendChild(checkbox);
        label.appendChild(status);

        div.appendChild(nameLabel);
        div.appendChild(label);
        container.appendChild(div);
      });
    }

    function renderLists() {
      renderGroup(boys, "boysList");
      renderGroup(girls, "girlsList");
      updateSummary();
    }

    function markGroup(groupType, status) {
      const list = groupType === "boys" ? boys : girls;

      list.forEach(name => {
        attendanceState[name] = status;
      });

      renderLists();
    }

    function updateSummary() {
      const allStudents = [...boys, ...girls];

      const presentList = allStudents.filter(
        name => attendanceState[name]
      );

      const absentList = allStudents.filter(
        name => !attendanceState[name]
      );

      document.getElementById("totalCount").textContent =
        allStudents.length;

      document.getElementById("presentCount").textContent =
        presentList.length;

      document.getElementById("absentCount").textContent =
        absentList.length;

      document.getElementById("presentSummaryHeader").textContent =
        `Present: ${presentList.length}`;

      document.getElementById("absentSummaryHeader").textContent =
        `Absent: ${absentList.length}`;

      const presentUl = document.getElementById("presentNamesList");
      const absentUl = document.getElementById("absentNamesList");

      presentUl.innerHTML = "";
      absentUl.innerHTML = "";

      if (presentList.length === 0) {
        const li = document.createElement("li");
        li.className = "list-none italic";
        li.textContent = "No students present";
        presentUl.appendChild(li);
      } else {
        presentList.forEach(name => {
          const li = document.createElement("li");
          li.textContent = name;
          presentUl.appendChild(li);
        });
      }

      if (absentList.length === 0) {
        const li = document.createElement("li");
        li.className = "list-none italic";
        li.textContent = "No students absent";
        absentUl.appendChild(li);
      } else {
        absentList.forEach(name => {
          const li = document.createElement("li");
          li.textContent = name;
          absentUl.appendChild(li);
        });
      }
    }

    async function copyAbsentList() {
      const allStudents = [...boys, ...girls];

      const absentList = allStudents.filter(
        name => !attendanceState[name]
      );

      const textToCopy =
        "Absent Students:\n" +
        absentList.map(name => "• " + name).join("\n");

      try {
        await navigator.clipboard.writeText(textToCopy);
        alert("Absent list copied to clipboard!");
      } catch (error) {
        window.prompt("Copy the absent list below:", textToCopy);
      }
    }

    function updateClock() {
      const now = new Date();

      const dateStr = now.toLocaleDateString("en-US", {
        year: "numeric",
        month: "2-digit",
        day: "2-digit"
      }).replace(/\//g, "-");

      document.getElementById("currentDate").textContent =
        now.toLocaleDateString("en-US", {
          weekday: "short",
          month: "short",
          day: "numeric",
          year: "numeric"
        });

      document.getElementById("summarySessionDate").textContent =
        `Class Session | ${dateStr}`;

      document.getElementById("currentTime").textContent =
        now.toLocaleTimeString("en-US", {
          hour12: true
        });
    }

    function exportDocumentationPDF() {
      const element = document.getElementById("pdfDocContainer");

      if (typeof html2pdf === "undefined") {
        alert("PDF library could not load. Please check your internet connection.");
        return;
      }

      element.classList.remove("hidden");

      const options = {
        margin: 0.5,
        filename: "BSIT_3B_Class_Attendance_System_Documentation.pdf",
        image: {
          type: "jpeg",
          quality: 0.98
        },
        html2canvas: {
          scale: 2
        },
        jsPDF: {
          unit: "in",
          format: "letter",
          orientation: "portrait"
        }
      };

      html2pdf()
        .set(options)
        .from(element)
        .save()
        .then(() => {
          element.classList.add("hidden");
        })
        .catch(error => {
          element.classList.add("hidden");
          console.error("PDF export failed:", error);
          alert("Unable to export the PDF. Please try again.");
        });
    }

    renderLists();
    updateClock();
    setInterval(updateClock, 1000);
  </script>
</body>
</html>
