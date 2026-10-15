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
  </style>
</head>
<body class="p-4 md:p-8">

  <div class="max-w-5xl mx-auto space-y-6">

    <!-- Header Section -->
    <header class="glass-card rounded-2xl p-6 shadow-2xl flex flex-col md:flex-row justify-between items-center gap-4 border-b-4 border-yellow-400">
      <div class="flex items-center gap-4">
        <img src="logo.jpg" alt="CPSU Logo" class="w-16 h-16 object-cover rounded-full border-2 border-yellow-400 drop-shadow-[0_0_10px_rgba(250,204,21,0.6)]" />
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

    <!-- Metrics Grid -->
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
