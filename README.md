<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>شاشة رصد وملاحظات بلاغات العباءة</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Cairo Font -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Cairo', sans-serif;
        }
        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(12px);
        }
        .custom-scrollbar::-webkit-scrollbar {
            width: 6px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #f1f5f9;
            border-radius: 8px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 8px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
        @media print {
            .no-print {
                display: none !important;
            }
            .print-area {
                box-shadow: none !important;
                border: none !important;
                padding: 0 !important;
            }
        }
    </style>
</head>
<body class="bg-slate-100 text-slate-800 min-h-screen flex flex-col justify-between selection:bg-indigo-500 selection:text-white">

    <!-- Screen 1: Lock / Auth Screen (PIN Protected 36386184) -->
    <div id="authScreen" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-900/95 backdrop-blur-md">
        <div class="glass-card max-w-md w-full p-8 rounded-3xl shadow-2xl border border-slate-700/40 text-center relative overflow-hidden">
            <div class="absolute -top-12 -right-12 w-32 h-32 bg-indigo-500/20 rounded-full blur-2xl"></div>
            <div class="absolute -bottom-12 -left-12 w-32 h-32 bg-amber-500/20 rounded-full blur-2xl"></div>
            
            <div class="w-20 h-20 bg-indigo-500/10 text-indigo-500 rounded-2xl flex items-center justify-center mx-auto mb-6 shadow-inner text-3xl">
                <i class="fa-solid fa-user-shield"></i>
            </div>
            
            <h2 class="text-2xl font-bold text-slate-800 mb-2">لوحة رصد وملاحظات العباءة</h2>
            <p class="text-slate-500 text-xs mb-6">يرجى إدخال الرقم السري المعتمد لاستعراض وكتابة الملاحظات</p>
            
            <form id="loginForm" onsubmit="handleLogin(event)" class="space-y-4">
                <div>
                    <input type="password" id="passcode" maxlength="12" placeholder="••••••••" required autofocus
                           class="w-full px-4 py-3 text-center text-2xl font-bold tracking-widest rounded-xl border border-slate-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all bg-slate-50">
                </div>
                <div id="authError" class="hidden text-rose-500 text-xs font-semibold animate-pulse">
                    <i class="fa-solid fa-circle-exclamation ml-1"></i> الرقم السري غير صحيح!
                </div>
                <button type="submit" 
                        class="w-full py-3.5 bg-gradient-to-r from-indigo-600 to-indigo-700 hover:from-indigo-700 hover:to-indigo-800 text-white font-bold rounded-xl shadow-lg shadow-indigo-500/30 hover:shadow-indigo-500/50 transition-all flex items-center justify-center gap-2">
                    <i class="fa-solid fa-lock-open"></i>
                    <span>دخول للسيستم</span>
                </button>
            </form>
            <div class="mt-6 pt-4 border-t border-slate-200/60 text-[11px] text-slate-400">
                الرقم السري المعتمد للرصد: <span class="font-mono bg-slate-200 px-2 py-0.5 rounded text-slate-700 font-bold">36386184</span>
            </div>
        </div>
    </div>

    <!-- Main Application Screen (Rasad & Notes System) -->
    <div id="mainContent" class="hidden min-h-screen flex flex-col">
        
        <!-- Navbar Header -->
        <header class="bg-slate-900 text-white sticky top-0 z-40 shadow-md no-print">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
                <div class="flex items-center space-x-3 space-x-reverse">
                    <div class="w-12 h-12 bg-indigo-600 text-white rounded-xl flex items-center justify-center font-bold text-xl shadow-md">
                        <i class="fa-solid fa-clipboard-list"></i>
                    </div>
                    <div>
                        <h1 class="text-lg sm:text-xl font-bold leading-tight">سيستم رصد وملاحظات بلاغات العباءة</h1>
                        <p class="text-xs text-indigo-300">نظام المتابعة الإدارية وإضافة التعليقات</p>
                    </div>
                </div>

                <div class="flex items-center gap-2 sm:gap-3">
                    <button onclick="window.print()" 
                            class="px-3 py-2 bg-slate-800 hover:bg-slate-700 text-slate-200 rounded-xl border border-slate-700 text-xs font-semibold transition-all flex items-center gap-1.5">
                        <i class="fa-solid fa-print"></i>
                        <span class="hidden sm:inline">طباعة الرصد</span>
                    </button>
                    <button onclick="logout()" 
                            class="px-3 py-2 bg-rose-500/10 hover:bg-rose-500/20 text-rose-400 rounded-xl border border-rose-500/30 text-xs font-semibold transition-all flex items-center gap-1.5">
                        <i class="fa-solid fa-right-from-bracket"></i>
                        <span>قفل الشاشة</span>
                    </button>
                </div>
            </div>
        </header>

        <!-- Dynamic Summary Statistics Bar -->
        <div class="bg-white border-b border-slate-200 py-4 shadow-sm no-print">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid grid-cols-2 sm:grid-cols-4 gap-4">
                <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200 flex items-center justify-between">
                    <div>
                        <p class="text-[11px] font-bold text-slate-500">إجمالي البلاغات</p>
                        <h3 id="statTotal" class="text-xl font-extrabold text-slate-800">0</h3>
                    </div>
                    <div class="w-10 h-10 bg-indigo-50 text-indigo-600 rounded-lg flex items-center justify-center text-lg">
                        <i class="fa-solid fa-box-archive"></i>
                    </div>
                </div>

                <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200 flex items-center justify-between">
                    <div>
                        <p class="text-[11px] font-bold text-slate-500">بلاغات بانتظار المتابعة</p>
                        <h3 id="statPending" class="text-xl font-extrabold text-amber-600">0</h3>
                    </div>
                    <div class="w-10 h-10 bg-amber-50 text-amber-600 rounded-lg flex items-center justify-center text-lg">
                        <i class="fa-solid fa-hourglass-half"></i>
                    </div>
                </div>

                <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200 flex items-center justify-between">
                    <div>
                        <p class="text-[11px] font-bold text-slate-500">ملاحظات مسجلة بالسيستم</p>
                        <h3 id="statNotes" class="text-xl font-extrabold text-blue-600">0</h3>
                    </div>
                    <div class="w-10 h-10 bg-blue-50 text-blue-600 rounded-lg flex items-center justify-center text-lg">
                        <i class="fa-solid fa-comments"></i>
                    </div>
                </div>

                <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200 flex items-center justify-between">
                    <div>
                        <p class="text-[11px] font-bold text-slate-500">بلاغات عاجلة جداً</p>
                        <h3 id="statUrgent" class="text-xl font-extrabold text-rose-600">0</h3>
                    </div>
                    <div class="w-10 h-10 bg-rose-50 text-rose-600 rounded-lg flex items-center justify-center text-lg">
                        <i class="fa-solid fa-triangle-exclamation"></i>
                    </div>
                </div>
            </div>
        </div>

        <!-- Main Workspace Area -->
        <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 flex-grow w-full">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 print-area">
                
                <!-- Filters & Search Toolbar -->
                <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-4 mb-6 pb-6 border-b border-slate-100 no-print">
                    <div>
                        <h2 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                            <i class="fa-solid fa-note-sticky text-indigo-600"></i>
                            رصد البلاغات وكتابة الملاحظات
                        </h2>
                        <p class="text-xs text-slate-400">يمكنك إضافة ملاحظات خاصة لكل بلاغ وحفظها مباشرة على السيستم</p>
                    </div>

                    <div class="flex flex-wrap items-center gap-2.5">
                        <!-- Search Box -->
                        <div class="relative min-w-[220px]">
                            <i class="fa-solid fa-magnifying-glass absolute right-3 top-3 text-slate-400 text-xs"></i>
                            <input type="text" id="searchInput" oninput="renderMonitorFeed()" placeholder="بحث باسم المضيف، رقم العباءة، أو الملاحظة..."
                                   class="w-full pr-8 pl-3 py-2 text-xs bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-500">
                        </div>

                        <!-- Status Filter -->
                        <select id="filterStatus" onchange="renderMonitorFeed()" class="text-xs bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                            <option value="الكل">جميع الحالات</option>
                            <option value="قيد المتابعة">قيد المتابعة</option>
                            <option value="تم الحل">تم الحل</option>
                            <option value="جديد">جديد</option>
                        </select>

                        <!-- Priority Filter -->
                        <select id="filterPriority" onchange="renderMonitorFeed()" class="text-xs bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                            <option value="الكل">كل درجات الأهمية</option>
                            <option value="عاجل جداً">عاجل جداً</option>
                            <option value="هام">هام</option>
                            <option value="عادي">عادي</option>
                        </select>

                        <button onclick="renderMonitorFeed()" title="تحديث القائمة" class="p-2 bg-slate-100 hover:bg-slate-200 text-slate-600 rounded-xl transition-colors">
                            <i class="fa-solid fa-arrows-rotate text-xs"></i>
                        </button>
                    </div>
                </div>

                <!-- Print Header -->
                <div class="hidden print:block mb-6 border-b-2 border-slate-800 pb-4">
                    <h1 class="text-2xl font-bold">تقرير رصد بلاغات وملاحظات العباءة الشامل</h1>
                    <p class="text-xs text-slate-600">تاريخ الاستخراج: <span id="printDate"></span></p>
                </div>

                <!-- Reports Feed Grid -->
                <div id="monitorList" class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <!-- Cards are dynamically rendered via JS -->
                </div>

                <!-- Empty State -->
                <div id="emptyState" class="hidden py-20 text-center">
                    <div class="w-16 h-16 bg-slate-100 text-slate-400 rounded-full flex items-center justify-center mx-auto mb-3 text-2xl">
                        <i class="fa-solid fa-clipboard-question"></i>
                    </div>
                    <h3 class="text-slate-600 font-bold text-sm">لا توجد بلاغات مسجلة مطابق للشروط</h3>
                    <p class="text-slate-400 text-xs mt-1">قم بتغيير خيارات التصفية أو إرسال بلاغ جديد من شاشة المضيف</p>
                </div>

            </div>
        </main>

        <!-- Footer -->
        <footer class="bg-white border-t border-slate-200 py-4 text-center text-xs text-slate-500 no-print">
            <p>سيستم إدارة وملاحظات بلاغات العباءة &copy; 2026 - جميع الحقوق محفوظة</p>
        </footer>
    </div>

    <!-- JavaScript Logic -->
    <script>
        const VALID_PASSCODE = "36386184";
        const STORAGE_KEY = "host_abaya_reports_data";
        const AUTH_KEY = "abaya_monitor_notes_session";

        window.onload = function() {
            const now = new Date();
            const dateStr = now.toLocaleDateString('ar-SA') + ' - ' + now.toLocaleTimeString('ar-SA');
            if (document.getElementById('printDate')) {
                document.getElementById('printDate').textContent = dateStr;
            }

            checkAuth();
            renderMonitorFeed();
        };

        // Authentication Handler
        function handleLogin(e) {
            e.preventDefault();
            const inputPass = document.getElementById('passcode').value.trim();
            const errorEl = document.getElementById('authError');

            if (inputPass === VALID_PASSCODE) {
                sessionStorage.setItem(AUTH_KEY, "true");
                errorEl.classList.add('hidden');
                document.getElementById('passcode').value = "";
                checkAuth();
                renderMonitorFeed();
            } else {
                errorEl.classList.remove('hidden');
                document.getElementById('passcode').focus();
            }
        }

        function checkAuth() {
            const isAuthenticated = sessionStorage.getItem(AUTH_KEY) === "true";
            const authScreen = document.getElementById('authScreen');
            const mainContent = document.getElementById('mainContent');

            if (isAuthenticated) {
                authScreen.classList.add('hidden');
                mainContent.classList.remove('hidden');
            } else {
                authScreen.classList.remove('hidden');
                mainContent.classList.add('hidden');
            }
        }

        function logout() {
            sessionStorage.removeItem(AUTH_KEY);
            checkAuth();
        }

        // Fetch reports with fallback initialization for notes/status arrays
        function getStoredReports() {
            const data = localStorage.getItem(STORAGE_KEY);
            if (!data) return [];
            
            try {
                const parsed = JSON.parse(data);
                return parsed.map(item => ({
                    ...item,
                    notesList: item.notesList || [], // Ensure notes array exists
                    status: item.status || "جديد"   // Default status
                }));
            } catch(e) {
                return [];
            }
        }

        function saveReports(reports) {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(reports));
        }

        // Add a new note to a specific report
        function addNoteToReport(reportId) {
            const noteInput = document.getElementById(`noteInput-${reportId}`);
            if (!noteInput) return;

            const noteText = noteInput.value.trim();
            if (!noteText) return;

            const reports = getStoredReports();
            const reportIndex = reports.findIndex(r => r.id === reportId);

            if (reportIndex !== -1) {
                const newNote = {
                    id: Date.now(),
                    text: noteText,
                    date: new Date().toLocaleString('ar-SA', { hour: '2-digit', minute: '2-digit', day: '2-digit', month: '2-digit' })
                };

                if (!reports[reportIndex].notesList) {
                    reports[reportIndex].notesList = [];
                }

                reports[reportIndex].notesList.push(newNote);
                saveReports(reports);
                renderMonitorFeed();
            }
        }

        // Delete a note from a report
        function deleteNote(reportId, noteId) {
            const reports = getStoredReports();
            const reportIndex = reports.findIndex(r => r.id === reportId);

            if (reportIndex !== -1 && reports[reportIndex].notesList) {
                reports[reportIndex].notesList = reports[reportIndex].notesList.filter(n => n.id !== noteId);
                saveReports(reports);
                renderMonitorFeed();
            }
        }

        // Change status of a report (جديد / قيد المتابعة / تم الحل)
        function updateReportStatus(reportId, newStatus) {
            const reports = getStoredReports();
            const reportIndex = reports.findIndex(r => r.id === reportId);

            if (reportIndex !== -1) {
                reports[reportIndex].status = newStatus;
                saveReports(reports);
                renderMonitorFeed();
            }
        }

        // Delete entire report
        function deleteReport(id) {
            if (confirm("هل أنت تأكد من رغبتك في مسح هذا البلاغ بشكل نهائي من السيستم؟")) {
                let reports = getStoredReports();
                reports = reports.filter(r => r.id !== id);
                saveReports(reports);
                renderMonitorFeed();
            }
        }

        // Render Cards & Dashboard View
        function renderMonitorFeed() {
            const reports = getStoredReports();
            const searchValue = document.getElementById('searchInput').value.trim().toLowerCase();
            const statusFilter = document.getElementById('filterStatus').value;
            const priorityFilter = document.getElementById('filterPriority').value;

            const monitorList = document.getElementById('monitorList');
            const emptyState = document.getElementById('emptyState');

            // Calculate Statistics
            let totalNotesCount = 0;
            let pendingCount = 0;
            let urgentCount = 0;

            reports.forEach(r => {
                if (r.notesList) totalNotesCount += r.notesList.length;
                if (r.status !== "تم الحل") pendingCount++;
                if (r.priority === "عاجل جداً") urgentCount++;
            });

            document.getElementById('statTotal').textContent = reports.length;
            document.getElementById('statPending').textContent = pendingCount;
            document.getElementById('statNotes').textContent = totalNotesCount;
            document.getElementById('statUrgent').textContent = urgentCount;

            // Apply Search & Filters
            let filtered = reports.filter(item => {
                const matchesSearch = 
                    (item.hostName && item.hostName.toLowerCase().includes(searchValue)) ||
                    (item.abayaTag && item.abayaTag.toLowerCase().includes(searchValue)) ||
                    (item.details && item.details.toLowerCase().includes(searchValue)) ||
                    (item.category && item.category.toLowerCase().includes(searchValue)) ||
                    (item.notesList && item.notesList.some(n => n.text.toLowerCase().includes(searchValue)));

                const matchesStatus = (statusFilter === "الكل") || (item.status === statusFilter);
                const matchesPriority = (priorityFilter === "الكل") || (item.priority === priorityFilter);

                return matchesSearch && matchesStatus && matchesPriority;
            });

            if (filtered.length === 0) {
                monitorList.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');

            monitorList.innerHTML = filtered.map(item => {
                let priorityBadge = "bg-slate-100 text-slate-700 border-slate-200";
                if (item.priority === "عاجل جداً") priorityBadge = "bg-rose-50 text-rose-700 border-rose-300 animate-pulse";
                if (item.priority === "هام") priorityBadge = "bg-amber-50 text-amber-700 border-amber-300";

                const notes = item.notesList || [];

                return `
                    <div class="p-5 rounded-2xl border border-slate-200 bg-slate-50/70 hover:bg-white transition-all shadow-sm hover:shadow-md flex flex-col justify-between gap-4">
                        
                        <!-- Header & Meta -->
                        <div>
                            <div class="flex items-start justify-between gap-2 border-b border-slate-100 pb-3">
                                <div>
                                    <span class="text-xs font-bold px-2.5 py-1 bg-indigo-50 text-indigo-700 rounded-lg border border-indigo-100 inline-block mb-1">
                                        <i class="fa-solid fa-tag text-indigo-500 ml-1"></i> ${item.category || 'بلاغ عام'}
                                    </span>
                                    <h4 class="font-bold text-slate-800 text-xs sm:text-sm">المضيف: <span class="text-indigo-600">${item.hostName || 'غير محدد'}</span></h4>
                                </div>

                                <div class="flex items-center gap-2">
                                    <span class="px-2.5 py-1 text-xs font-bold rounded-lg border ${priorityBadge}">
                                        ${item.priority}
                                    </span>
                                    <button onclick="deleteReport(${item.id})" title="حذف البلاغ" class="no-print text-slate-400 hover:text-rose-600 p-1">
                                        <i class="fa-solid fa-trash-can text-sm"></i>
                                    </button>
                                </div>
                            </div>

                            <!-- Details Content -->
                            <div class="mt-3 text-xs sm:text-sm text-slate-800 leading-relaxed font-semibold bg-white p-3.5 rounded-xl border border-slate-200/80">
                                ${item.details}
                            </div>

                            <!-- Extra Tag & Date -->
                            <div class="flex items-center justify-between text-[11px] text-slate-500 mt-2 px-1">
                                <span><i class="fa-solid fa-hashtag text-slate-400 ml-1"></i> رقم/مكان: <strong>${item.abayaTag || item.locationNum || 'غير مدون'}</strong></span>
                                <span><i class="fa-solid fa-clock text-slate-400 ml-1"></i> ${item.reportDate || item.date || ''} ${item.timestamp ? `- ${item.timestamp}` : ''}</span>
                            </div>
                        </div>

                        <!-- System Status Selector -->
                        <div class="bg-indigo-50/40 p-3 rounded-xl border border-indigo-100/60 no-print">
                            <div class="flex items-center justify-between gap-2">
                                <label class="text-xs font-bold text-indigo-900 flex items-center gap-1">
                                    <i class="fa-solid fa-arrows-spin text-indigo-600"></i> حالة البلاغ بالسيستم:
                                </label>
                                <select onchange="updateReportStatus(${item.id}, this.value)" 
                                        class="text-xs font-bold rounded-lg px-2.5 py-1 border border-indigo-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 ${
                                            item.status === 'تم الحل' ? 'bg-emerald-100 text-emerald-800' :
                                            item.status === 'قيد المتابعة' ? 'bg-amber-100 text-amber-800' : 'bg-white text-slate-700'
                                        }">
                                    <option value="جديد" ${item.status === 'جديد' ? 'selected' : ''}>جديد</option>
                                    <option value="قيد المتابعة" ${item.status === 'قيد المتابعة' ? 'selected' : ''}>قيد المتابعة</option>
                                    <option value="تم الحل" ${item.status === 'تم الحل' ? 'selected' : ''}>تم الحل</option>
                                </select>
                            </div>
                        </div>

                        <!-- Notes/Comments Section (ملاحظات السيستم) -->
                        <div class="border-t border-slate-200/80 pt-3">
                            <div class="flex items-center justify-between mb-2">
                                <h5 class="text-xs font-bold text-slate-700 flex items-center gap-1.5">
                                    <i class="fa-solid fa-comment-dots text-indigo-500"></i>
                                    ملاحظات التعقيب بالسيستم (${notes.length})
                                </h5>
                            </div>

                            <!-- List of existing notes -->
                            <div class="space-y-2 mb-3 max-h-36 overflow-y-auto custom-scrollbar pl-1">
                                ${notes.length === 0 ? `
                                    <p class="text-[11px] text-slate-400 italic bg-white p-2 rounded-lg border border-dashed border-slate-200">لا توجد ملاحظات مدونة على هذا البلاغ حتى الآن.</p>
                                ` : notes.map(n => `
                                    <div class="bg-white p-2.5 rounded-xl border border-slate-200 text-xs relative group flex items-start justify-between gap-2 shadow-2xs">
                                        <div class="space-y-0.5">
                                            <p class="text-slate-800 font-medium leading-snug">${n.text}</p>
                                            <span class="text-[10px] text-slate-400 font-mono block">${n.date}</span>
                                        </div>
                                        <button onclick="deleteNote(${item.id}, ${n.id})" title="مسح الملاحظة" class="no-print text-slate-300 hover:text-rose-500 transition-colors">
                                            <i class="fa-solid fa-xmark text-xs"></i>
                                        </button>
                                    </div>
                                `).join('')}
                            </div>

                            <!-- Add Note Form -->
                            <div class="flex items-center gap-2 no-print">
                                <input type="text" id="noteInput-${item.id}" 
                                       placeholder="اكتب ملاحظة جديدة..." 
                                       onkeydown="if(event.key === 'Enter') addNoteToReport(${item.id})"
                                       class="w-full text-xs px-3 py-2 rounded-xl border border-slate-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 bg-white">
                                <button onclick="addNoteToReport(${item.id})" 
                                        class="px-3 py-2 bg-indigo-600 hover:bg-indigo-700 text-white rounded-xl text-xs font-bold transition-all shadow-sm flex items-center gap-1 shrink-0">
                                    <i class="fa-solid fa-plus"></i>
                                    <span>إضافة</span>
                                </button>
                            </div>
                        </div>

                    </div>
                `;
            }).join('');
        }
    </script>
</body>
</html>
