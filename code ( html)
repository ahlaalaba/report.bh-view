<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>شاشة رصد وعرض بلاغات العباءة</title>
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

    <!-- Screen 1: Lock / Auth Screen (Password Protected) -->
    <div id="authScreen" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-900/95 backdrop-blur-md">
        <div class="glass-card max-w-md w-full p-8 rounded-3xl shadow-2xl border border-slate-700/40 text-center relative overflow-hidden">
            <div class="absolute -top-12 -right-12 w-32 h-32 bg-indigo-500/20 rounded-full blur-2xl"></div>
            <div class="absolute -bottom-12 -left-12 w-32 h-32 bg-amber-500/20 rounded-full blur-2xl"></div>
            
            <div class="w-20 h-20 bg-indigo-500/10 text-indigo-500 rounded-2xl flex items-center justify-center mx-auto mb-6 shadow-inner text-3xl">
                <i class="fa-solid fa-shield-halved"></i>
            </div>
            
            <h2 class="text-2xl font-bold text-slate-800 mb-2">شاشة الرصد والمتابعة</h2>
            <p class="text-slate-500 text-xs mb-6">يرجى إدخال الرقم السري المعتمد لاستعراض البلاغات المسجلة</p>
            
            <form id="loginForm" onsubmit="handleLogin(event)" class="space-y-4">
                <div>
                    <input type="password" id="passcode" maxlength="12" placeholder="••••••••" required
                           class="w-full px-4 py-3 text-center text-2xl font-bold tracking-widest rounded-xl border border-slate-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all bg-slate-50">
                </div>
                <div id="authError" class="hidden text-rose-500 text-xs font-semibold animate-pulse">
                    <i class="fa-solid fa-circle-exclamation ml-1"></i> الرقم السري غير صحيح! يرجى المحاولة مرة أخرى.
                </div>
                <button type="submit" 
                        class="w-full py-3.5 bg-gradient-to-r from-indigo-600 to-indigo-700 hover:from-indigo-700 hover:to-indigo-800 text-white font-bold rounded-xl shadow-lg shadow-indigo-500/30 hover:shadow-indigo-500/50 transition-all flex items-center justify-center gap-2">
                    <i class="fa-solid fa-lock-open"></i>
                    <span>فتح لوحة الرصد</span>
                </button>
            </form>
            <div class="mt-6 pt-4 border-t border-slate-200/60 text-[11px] text-slate-400">
                الرقم السري التجريبي المعتمد: <span class="font-mono bg-slate-200 px-2 py-0.5 rounded text-slate-700 font-bold">36386184</span>
            </div>
        </div>
    </div>

    <!-- Main Application UI (Rasad & View Only) -->
    <div id="mainContent" class="hidden min-h-screen flex flex-col">
        <!-- Header -->
        <header class="bg-slate-900 text-white sticky top-0 z-40 shadow-md no-print">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
                <div class="flex items-center space-x-3 space-x-reverse">
                    <div class="w-12 h-12 bg-indigo-600 text-white rounded-xl flex items-center justify-center font-bold text-xl shadow-md">
                        <i class="fa-solid fa-chart-line"></i>
                    </div>
                    <div>
                        <h1 class="text-lg sm:text-xl font-bold leading-tight">لوحة رصد بلاغات العباءة</h1>
                        <p class="text-xs text-indigo-300">نظام الاستعراض والتدقيق المباشر</p>
                    </div>
                </div>

                <div class="flex items-center gap-2 sm:gap-3">
                    <button onclick="window.print()" 
                            class="px-3 py-2 bg-slate-800 hover:bg-slate-700 text-slate-200 rounded-xl border border-slate-700 text-xs font-semibold transition-all flex items-center gap-1.5">
                        <i class="fa-solid fa-print"></i>
                        <span class="hidden sm:inline">طباعة التقرير</span>
                    </button>
                    <button onclick="logout()" 
                            class="px-3 py-2 bg-rose-500/10 hover:bg-rose-500/20 text-rose-400 rounded-xl border border-rose-500/30 text-xs font-semibold transition-all flex items-center gap-1.5">
                        <i class="fa-solid fa-right-from-bracket"></i>
                        <span>قفل الشاشة</span>
                    </button>
                </div>
            </div>
        </header>

        <!-- Stats Bar -->
        <div class="bg-white border-b border-slate-200 py-4 shadow-sm no-print">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid grid-cols-2 sm:grid-cols-4 gap-4">
                <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200 flex items-center justify-between">
                    <div>
                        <p class="text-[11px] font-bold text-slate-500">إجمالي البلاغات</p>
                        <h3 id="statTotal" class="text-xl font-extrabold text-slate-800">0</h3>
                    </div>
                    <div class="w-10 h-10 bg-indigo-50 text-indigo-600 rounded-lg flex items-center justify-center text-lg">
                        <i class="fa-solid fa-folder"></i>
                    </div>
                </div>
                <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200 flex items-center justify-between">
                    <div>
                        <p class="text-[11px] font-bold text-slate-500">حالات عاجلة جداً</p>
                        <h3 id="statUrgent" class="text-xl font-extrabold text-rose-600">0</h3>
                    </div>
                    <div class="w-10 h-10 bg-rose-50 text-rose-600 rounded-lg flex items-center justify-center text-lg">
                        <i class="fa-solid fa-bell"></i>
                    </div>
                </div>
                <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200 flex items-center justify-between">
                    <div>
                        <p class="text-[11px] font-bold text-slate-500">عباءات مفقودة</p>
                        <h3 id="statLost" class="text-xl font-extrabold text-amber-600">0</h3>
                    </div>
                    <div class="w-10 h-10 bg-amber-50 text-amber-600 rounded-lg flex items-center justify-center text-lg">
                        <i class="fa-solid fa-magnifying-glass"></i>
                    </div>
                </div>
                <div class="bg-slate-50 p-3.5 rounded-xl border border-slate-200 flex items-center justify-between">
                    <div>
                        <p class="text-[11px] font-bold text-slate-500">حالة الربط بالسيستم</p>
                        <h3 class="text-xs font-bold text-emerald-600 flex items-center gap-1 mt-1">
                            <span class="w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></span> مفعّل (مباشر)
                        </h3>
                    </div>
                    <div class="w-10 h-10 bg-emerald-50 text-emerald-600 rounded-lg flex items-center justify-center text-lg">
                        <i class="fa-solid fa-database"></i>
                    </div>
                </div>
            </div>
        </div>

        <!-- Main Body Container -->
        <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 flex-grow w-full">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 print-area">
                
                <!-- Filter and Search Header -->
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6 pb-6 border-b border-slate-100 no-print">
                    <div>
                        <h2 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                            <i class="fa-solid fa-eye text-indigo-600"></i>
                            رصد سجل البلاغات الحية
                        </h2>
                        <p class="text-xs text-slate-400">بيانات مستخرجة تلقائياً من نظام المضيفين</p>
                    </div>

                    <!-- Filter Controls -->
                    <div class="flex flex-wrap items-center gap-3">
                        <!-- Search Bar -->
                        <div class="relative min-w-[200px]">
                            <i class="fa-solid fa-magnifying-glass absolute right-3 top-3 text-slate-400 text-xs"></i>
                            <input type="text" id="searchInput" oninput="renderMonitorFeed()" placeholder="بحث باسم المضيف/المكان/الرقم..."
                                   class="w-full pr-8 pl-3 py-2 text-xs bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-500">
                        </div>

                        <!-- Category Filter -->
                        <select id="filterCategory" onchange="renderMonitorFeed()" class="text-xs bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                            <option value="الكل">كل التصنيفات</option>
                            <option value="عباءة مفقودة / عُثر عليها">عباءة مفقودة / عُثر عليها</option>
                            <option value="تبديل عباءة بالخطأ">تبديل بالخطأ</option>
                            <option value="عباءة بدون بطاقة / الرقم السري">بدون بطاقة</option>
                            <option value="ملاحظة أو طلب خاص للعباءة">ملاحظة خاصة</option>
                            <option value="أخرى">أخرى</option>
                        </select>

                        <!-- Priority Filter -->
                        <select id="filterPriority" onchange="renderMonitorFeed()" class="text-xs bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                            <option value="الكل">كل درجات الأهمية</option>
                            <option value="عاجل جداً">عاجل جداً</option>
                            <option value="هام">هام</option>
                            <option value="عادي">عادي</option>
                        </select>

                        <!-- Refresh Data Button -->
                        <button onclick="renderMonitorFeed()" title="تحديث البيانات" class="p-2 bg-slate-100 hover:bg-slate-200 text-slate-600 rounded-xl transition-colors">
                            <i class="fa-solid fa-arrows-rotate text-xs"></i>
                        </button>
                    </div>
                </div>

                <!-- Print Header (Visible only when printing) -->
                <div class="hidden print:block mb-6 border-b-2 border-slate-800 pb-4">
                    <h1 class="text-2xl font-bold">تقرير رصد بلاغات العباءة الشامل</h1>
                    <p class="text-xs text-slate-600">تاريخ الطباعة: <span id="printDate"></span></p>
                </div>

                <!-- Reports Grid View -->
                <div id="monitorList" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <!-- Dynamic Items render here -->
                </div>

                <!-- Empty State -->
                <div id="emptyState" class="hidden py-20 text-center">
                    <div class="w-16 h-16 bg-slate-100 text-slate-400 rounded-full flex items-center justify-center mx-auto mb-3 text-2xl">
                        <i class="fa-solid fa-inbox"></i>
                    </div>
                    <h3 class="text-slate-600 font-bold text-sm">لا توجد بلاغات مرصودة حالياً</h3>
                    <p class="text-slate-400 text-xs mt-1">تأكد من اختيار التصفية الصحيحة أو انتظار إرسال بلاغات جديدة من المضيف</p>
                </div>

            </div>
        </main>

        <!-- Footer -->
        <footer class="bg-white border-t border-slate-200 py-4 text-center text-xs text-slate-500 no-print">
            <p>شاشة الرصد والمتابعة الفنية &copy; 2026 - نظام بلاغات العباءة</p>
        </footer>
    </div>

    <!-- Application Script -->
    <script>
        const VALID_PASSCODE = "36386184";
        const STORAGE_KEY = "host_abaya_reports_data";
        const AUTH_KEY = "abaya_monitor_auth_session";

        // Initial check on page load
        window.onload = function() {
            // Setup print date
            const now = new Date();
            const dateStr = now.toLocaleDateString('ar-SA') + ' - ' + now.toLocaleTimeString('ar-SA');
            if (document.getElementById('printDate')) {
                document.getElementById('printDate').textContent = dateStr;
            }

            // Always enforce Auth check on load/refresh
            checkAuth();
            renderMonitorFeed();
        };

        // Authenticate User
        function handleLogin(e) {
            e.preventDefault();
            const inputPass = document.getElementById('passcode').value.trim();
            const errorEl = document.getElementById('authError');

            if (inputPass === VALID_PASSCODE) {
                sessionStorage.setItem(AUTH_KEY, "true");
                errorEl.classList.add('hidden');
                document.getElementById('passcode').value = "";
                checkAuth();
            } else {
                errorEl.classList.remove('hidden');
                document.getElementById('passcode').focus();
            }
        }

        // Verify Session Security
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

        // Lock / Logout
        function logout() {
            sessionStorage.removeItem(AUTH_KEY);
            checkAuth();
        }

        // Read LocalStorage Data
        function getStoredReports() {
            const data = localStorage.getItem(STORAGE_KEY);
            return data ? JSON.parse(data) : [];
        }

        // Delete Report directly from Monitor
        function deleteReport(id) {
            if (confirm("هل أنت تأكد من رغبتك في مسح هذا البلاغ من النظام؟")) {
                let reports = getStoredReports();
                reports = reports.filter(r => r.id !== id);
                localStorage.setItem(STORAGE_KEY, JSON.stringify(reports));
                renderMonitorFeed();
            }
        }

        // Main Render Logic
        function renderMonitorFeed() {
            const reports = getStoredReports();
            const searchValue = document.getElementById('searchInput').value.trim().toLowerCase();
            const categoryFilter = document.getElementById('filterCategory').value;
            const priorityFilter = document.getElementById('filterPriority').value;

            const monitorList = document.getElementById('monitorList');
            const emptyState = document.getElementById('emptyState');

            // Update Header Stats
            document.getElementById('statTotal').textContent = reports.length;
            document.getElementById('statUrgent').textContent = reports.filter(r => r.priority === "عاجل جداً").length;
            document.getElementById('statLost').textContent = reports.filter(r => r.category.includes("مفقودة")).length;

            // Filtering
            let filtered = reports.filter(item => {
                const matchesSearch = 
                    item.hostName.toLowerCase().includes(searchValue) ||
                    item.locationNum.toLowerCase().includes(searchValue) ||
                    (item.abayaTag && item.abayaTag.toLowerCase().includes(searchValue)) ||
                    item.details.toLowerCase().includes(searchValue);

                const matchesCategory = (categoryFilter === "الكل") || (item.category === categoryFilter);
                const matchesPriority = (priorityFilter === "الكل") || (item.priority === priorityFilter);

                return matchesSearch && matchesCategory && matchesPriority;
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

                return `
                    <div class="p-5 rounded-2xl border border-slate-200 bg-slate-50/70 hover:bg-white transition-all shadow-sm hover:shadow-md flex flex-col justify-between gap-4">
                        <div class="flex items-start justify-between gap-2 border-b border-slate-100 pb-3">
                            <div>
                                <span class="text-xs font-bold px-2.5 py-1 bg-indigo-50 text-indigo-700 rounded-lg border border-indigo-100 inline-block mb-1">
                                    <i class="fa-solid fa-tag text-indigo-500 ml-1"></i> ${item.category}
                                </span>
                                <h4 class="font-bold text-slate-800 text-sm">طاولة/جناح: <span class="text-indigo-600">${item.locationNum}</span></h4>
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

                        <!-- Report Body -->
                        <div class="text-xs text-slate-700 leading-relaxed font-medium bg-white p-3.5 rounded-xl border border-slate-200/80">
                            ${item.details}
                        </div>

                        <!-- Card Footer Info -->
                        <div class="grid grid-cols-2 gap-2 text-xs text-slate-500 pt-2 border-t border-slate-100">
                            <div>
                                <i class="fa-solid fa-user text-slate-400 ml-1"></i>
                                <span>المضيف: <strong>${item.hostName}</strong></span>
                            </div>
                            <div class="text-left">
                                <i class="fa-solid fa-hashtag text-slate-400 ml-1"></i>
                                <span>العباءة: <strong>${item.abayaTag}</strong></span>
                            </div>
                            <div class="col-span-2 text-slate-400 text-[10px] mt-1 flex justify-between items-center">
                                <span><i class="fa-solid fa-clock ml-1"></i>${item.timestamp || 'غير محدد'}</span>
                                <span><i class="fa-solid fa-calendar ml-1"></i>${item.date || ''}</span>
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }
    </script>
</body>
</html>
