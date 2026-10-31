<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>عرض ومتابعة تقارير مضيف أهل العباء - البلاد القديم</title>
  
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Google Fonts: Cairo -->
  <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800;900&display=swap" rel="stylesheet">
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            cairo: ['Cairo', 'sans-serif'],
          },
          colors: {
            maroon: {
              50: '#fdf2f2',
              100: '#fde8e8',
              700: '#800000',
              800: '#660000',
              900: '#4a0000',
            },
            gold: {
              400: '#fbbf24',
              500: '#d97706',
              600: '#b45309',
            }
          }
        }
      }
    }
  </script>

  <style>
    body {
      font-family: 'Cairo', sans-serif;
      background-color: #f3f4f6;
    }
    
    /* الشعار في خلفية الورقة بنسبة وضوح 50% */
    .watermark-bg {
      position: relative;
    }
    .watermark-bg::before {
      content: "";
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 75%;
      height: 75%;
      background-image: url('image_be668db9-removebg-preview.png');
      background-repeat: no-repeat;
      background-position: center;
      background-size: contain;
      opacity: 0.50;
      pointer-events: none;
      z-index: 0;
    }

    .print-content {
      position: relative;
      z-index: 10;
    }

    @media print {
      .no-print {
        display: none !important;
      }
      body {
        background-color: white !important;
        padding: 0 !important;
        margin: 0 !important;
      }
      .print-container {
        box-shadow: none !important;
        border: none !important;
        width: 100% !important;
        max-width: 100% !important;
        padding: 0 !important;
      }
      .watermark-bg::before {
        opacity: 0.30 !important;
        -webkit-print-color-adjust: exact;
        print-color-adjust: exact;
      }
    }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-3 md:p-6 text-gray-800">

  <!-- ================= 1. شاشة تسجيل الدخول بالرمز السري ================= -->
  <div id="loginScreen" class="fixed inset-0 bg-gray-900/80 backdrop-blur-md flex items-center justify-center p-4 z-50">
    <div class="bg-white rounded-2xl shadow-2xl border border-gray-200 max-w-md w-full p-6 md:p-8 text-center relative overflow-hidden">
      
      <!-- شعار المضيف -->
      <div class="mb-4">
        <img src="image_be668db9-removebg-preview.png" alt="مضيف أهل العباء" class="h-20 mx-auto object-contain">
      </div>

      <h2 class="text-2xl font-black text-maroon-800 mb-1">مضيف أهل العباء</h2>
      <p class="text-xs text-gold-600 font-bold mb-6">البلاد القديم — مملكة البحرين</p>

      <!-- تنبيه الخطأ عند إدخال رمز خاطئ -->
      <div id="loginError" class="hidden mb-4 bg-red-100 border-r-4 border-red-600 text-red-700 p-3 rounded-lg text-xs font-bold text-right flex items-center gap-2">
        <i class="fa-solid fa-circle-exclamation text-base"></i>
        <span>الرقم السري غير صحيح، الرجاء المحاولة مرة أخرى!</span>
      </div>

      <form onsubmit="handleLogin(event)" class="space-y-4 text-right">
        
        <!-- حقل الرقم السري (مخفي) -->
        <div>
          <label class="block text-xs font-bold text-gray-700 mb-1">أدخل الرقم السري للوصول للتقارير:</label>
          <input 
            type="password" 
            id="loginPin" 
            placeholder="••••••••" 
            required 
            class="w-full border border-gray-300 rounded-lg px-4 py-2.5 text-center text-xl font-bold tracking-widest focus:ring-2 focus:ring-maroon-700 outline-none"
          />
        </div>

        <button 
          type="submit" 
          class="w-full py-3 bg-maroon-700 hover:bg-maroon-800 text-white font-bold rounded-lg shadow-md transition flex items-center justify-center gap-2 text-sm"
        >
          <i class="fa-solid fa-lock-open"></i> تسجيل الدخول
        </button>

      </form>
    </div>
  </div>

  <!-- ================= 2. واجهة عرض ومتابعة التقارير والملاحظات ================= -->
  <div id="mainDashboard" class="hidden max-w-5xl mx-auto mb-20">

    <!-- شريط معلومات أعلى الشاشة -->
    <div class="no-print bg-white rounded-xl shadow-md p-4 mb-6 border border-gray-200 flex justify-between items-center">
      <div class="flex items-center gap-3">
        <div class="w-10 h-10 rounded-full bg-maroon-700 text-gold-400 flex items-center justify-center font-bold text-lg">
          <i class="fa-solid fa-clipboard-check"></i>
        </div>
        <div>
          <div class="text-xs text-gray-500 font-bold">النظام:</div>
          <div class="text-sm font-black text-maroon-800">عرض ومتابعة حالات التقارير</div>
        </div>
      </div>

      <button onclick="logout()" class="px-4 py-2 bg-red-100 hover:bg-red-200 text-red-700 rounded-lg font-bold text-xs transition">
        <i class="fa-solid fa-right-from-bracket"></i> خروج
      </button>
    </div>

    <!-- ورقة التقرير الموثق (للعرض فقط) -->
    <div class="print-container watermark-bg bg-white rounded-xl shadow-lg border border-gray-200 p-6 md:p-10 relative overflow-hidden">
      
      <div class="print-content">

        <!-- الترويسة الهيدر -->
        <header class="border-b-2 border-maroon-700 pb-6 mb-6">
          <div class="flex flex-col md:flex-row justify-between items-center gap-4 text-center md:text-right">
            
            <div class="flex items-center gap-4">
              <img src="image_be668db9-removebg-preview.png" alt="مضيف أهل العباء" class="h-20 w-auto object-contain">
              <div>
                <h1 class="text-2xl md:text-3xl font-black text-maroon-800 tracking-wide">مضيف أهل العباء</h1>
                <p class="text-xs md:text-sm text-gold-600 font-bold mt-1">
                  <i class="fa-solid fa-location-dot ml-1"></i> البلاد القديم — مملكة البحرين
                </p>
                <p class="text-[11px] text-gray-400 font-semibold mt-0.5">شاشة متابعة وتحديث التقارير</p>
              </div>
            </div>

            <!-- اليوم والتاريخ والساعة الحية -->
            <div class="bg-maroon-50/90 backdrop-blur-sm border border-maroon-100 rounded-lg p-3 text-center md:text-left min-w-[210px]">
              <div id="liveDay" class="text-maroon-800 font-black text-sm">--</div>
              <div id="liveDate" class="text-gray-600 text-xs font-semibold my-0.5">--/--/----</div>
              <div id="liveTime" class="text-gold-600 font-black text-sm dir-ltr">--:--:-- --</div>
            </div>

          </div>
        </header>

        <!-- تفاصيل التقرير المعروض وحالته -->
        <div class="bg-gray-50/90 border border-gray-200 rounded-xl p-4 mb-6 grid grid-cols-1 md:grid-cols-3 gap-4">
          <div>
            <span class="block text-xs font-bold text-gray-500 mb-1">اسم الكريم (مُعد التقرير):</span>
            <div id="viewAuthor" class="text-sm font-black text-gray-800 bg-white p-2.5 rounded-lg border border-gray-200">عبدالله المحمود</div>
          </div>

          <div>
            <span class="block text-xs font-bold text-gray-500 mb-1">رقم الهاتف:</span>
            <div id="viewPhone" class="text-sm font-black text-gray-800 bg-white p-2.5 rounded-lg border border-gray-200 dir-ltr text-right">🇧🇭 +973 39445566</div>
          </div>

          <!-- تحديد إشعار وحالة التقرير (جديد / قيد متابعة / تم الحل) -->
          <div>
            <span class="block text-xs font-bold text-gray-500 mb-1">إشعار وحالة التقرير:</span>
            <select 
              id="reportStatus" 
              onchange="updateStatusStyle()" 
              class="w-full border-2 rounded-lg p-2 text-sm font-black outline-none transition cursor-pointer"
            >
              <option value="جديد" class="bg-blue-100 text-blue-800">🔵 جديد</option>
              <option value="قيد المتابعة" class="bg-amber-100 text-amber-800">🟡 قيد المتابعة</option>
              <option value="تم الحل" class="bg-green-100 text-green-800">🟢 تم الحل</option>
            </select>
          </div>
        </div>

        <!-- نص التقرير (عرض فقط - غير قابل للتعديل) -->
        <div class="mb-8">
          <label class="block text-sm font-bold text-gray-700 mb-2">
            <i class="fa-solid fa-file-lines text-maroon-700 ml-1"></i> تفاصيل ومضمون التقرير المعروض:
          </label>
          <div id="viewContent" class="w-full min-h-[160px] border border-gray-200 rounded-xl p-4 bg-white/90 text-gray-800 text-sm font-medium leading-relaxed shadow-inner">
            تمت المتابعة وتجهيز كافة المستلزمات اليومية في مضيف أهل العباء بالبلاد القديم وتوفير المشروبات والمأكولات للزوار بنجاح وانسيابية كاملة.
          </div>
        </div>

        <!-- قسم الملاحظات والتعليقات -->
        <div class="border-t-2 border-dashed border-gray-300 pt-6 mb-6">
          <h3 class="text-base font-black text-maroon-800 mb-3 flex items-center gap-2">
            <i class="fa-solid fa-comments text-gold-500"></i> ملاحظات وتعليقات المتابعة:
          </h3>

          <!-- قائمة الملاحظات المضافة -->
          <div id="notesContainer" class="space-y-3 mb-4">
            <!-- تظهر الملاحظات هنا -->
          </div>

          <!-- إضافة ملاحظة جديدة -->
          <div class="no-print bg-maroon-50/60 p-4 rounded-xl border border-maroon-100">
            <label class="block text-xs font-bold text-gray-700 mb-2">كتابة ملاحظة جديدة على التقرير:</label>
            <div class="flex flex-col md:flex-row gap-2">
              <textarea 
                id="newNoteInput" 
                rows="2" 
                placeholder="اكتب ملاحظاتك أو توجيهاتك هنا..." 
                class="w-full border border-gray-300 rounded-lg p-3 text-sm focus:ring-2 focus:ring-maroon-700 outline-none bg-white"
              ></textarea>
              <button 
                onclick="addNote()" 
                class="px-5 py-2.5 bg-maroon-700 hover:bg-maroon-800 text-white rounded-lg font-bold text-sm shadow transition shrink-0 flex items-center justify-center gap-1.5"
              >
                <i class="fa-solid fa-paper-plane text-gold-400"></i> إضافة الملاحظة
              </button>
            </div>
          </div>
        </div>

        <!-- ترويسة سفلية للطباعة -->
        <footer class="mt-8 pt-4 border-t border-gray-200 text-center text-xs text-gray-500 hidden print:block font-bold">
          مضيف أهل العباء — البلاد القديم (مملكة البحرين)
        </footer>

      </div>

    </div>

  </div>

  <!-- الشريط السفلي الثابت للطباعة والحفظ -->
  <div class="no-print fixed bottom-0 left-0 right-0 bg-white/95 backdrop-blur-md border-t border-gray-200 py-3 px-4 shadow-2xl z-40">
    <div class="max-w-5xl mx-auto flex items-center justify-between gap-3">
      
      <div class="text-xs text-gray-600 font-bold hidden md:flex items-center gap-2">
        <img src="image_be668db9-removebg-preview.png" alt="شعار" class="h-6 w-auto inline">
        مضيف أهل العباء — البلاد القديم
      </div>

      <div class="flex items-center gap-3 w-full md:w-auto justify-end">
        <!-- زر حفظ الحالة والملاحظات -->
        <button 
          onclick="saveReportState()" 
          class="flex-1 md:flex-none px-5 py-2.5 bg-maroon-700 hover:bg-maroon-800 text-white rounded-lg font-bold text-sm transition shadow flex items-center justify-center gap-2"
        >
          <i class="fa-solid fa-floppy-disk text-gold-400"></i> حفظ التغيرات والملاحظات
        </button>

        <!-- زر الطباعة -->
        <button 
          onclick="window.print()" 
          class="flex-1 md:flex-none px-5 py-2.5 bg-gold-500 hover:bg-gold-600 text-white rounded-lg font-bold text-sm transition shadow flex items-center justify-center gap-2"
        >
          <i class="fa-solid fa-print"></i> طباعة / PDF
        </button>
      </div>

    </div>
  </div>

  <script>
    // الرقم السري المعتمد للدخول
    const SECRET_PIN = "36386184";
    let notesList = [];

    // 1. تسجيل الدخول بالرقم السري
    function handleLogin(e) {
      e.preventDefault();
      const pin = document.getElementById('loginPin').value.trim();
      const errorBox = document.getElementById('loginError');

      if (pin === SECRET_PIN) {
        errorBox.classList.add('hidden');
        document.getElementById('loginScreen').classList.add('hidden');
        document.getElementById('mainDashboard').classList.remove('hidden');
        loadSavedData();
      } else {
        errorBox.classList.remove('hidden');
      }
    }

    function logout() {
      document.getElementById('loginPin').value = '';
      document.getElementById('loginScreen').classList.remove('hidden');
      document.getElementById('mainDashboard').classList.add('hidden');
    }

    // 2. تغيير لون وتنسيق إشعار وحالة التقرير (جديد / قيد متابعة / تم الحل)
    function updateStatusStyle() {
      const statusSelect = document.getElementById('reportStatus');
      const val = statusSelect.value;
      statusSelect.className = "w-full border-2 rounded-lg p-2 text-sm font-black outline-none transition cursor-pointer ";

      if (val === "جديد") {
        statusSelect.classList.add("border-blue-500", "bg-blue-50", "text-blue-800");
      } else if (val === "قيد المتابعة") {
        statusSelect.classList.add("border-amber-500", "bg-amber-50", "text-amber-800");
      } else if (val === "تم الحل") {
        statusSelect.classList.add("border-green-500", "bg-green-50", "text-green-800");
      }
    }

    // 3. التوقيت والتاريخ التلقائي الحي
    function updateLiveDateTime() {
      const now = new Date();
      const days = ['الأحد', 'الإثنين', 'الثلاثاء', 'الأربعاء', 'الخميس', 'الجمعة', 'السبت'];
      
      const dayName = days[now.getDay()];
      const day = String(now.getDate()).padStart(2, '0');
      const month = String(now.getMonth() + 1).padStart(2, '0');
      const year = now.getFullYear();

      let hours = now.getHours();
      const minutes = String(now.getMinutes()).padStart(2, '0');
      const seconds = String(now.getSeconds()).padStart(2, '0');
      const ampm = hours >= 12 ? 'م' : 'ص';
      hours = hours % 12 || 12;

      document.getElementById('liveDay').textContent = dayName;
      document.getElementById('liveDate').textContent = `${year}/${month}/${day}`;
      document.getElementById('liveTime').textContent = `${hours}:${minutes}:${seconds} ${ampm}`;
    }

    setInterval(updateLiveDateTime, 1000);
    updateLiveDateTime();

    // 4. إضافة ملاحظة جديدة
    function addNote() {
      const input = document.getElementById('newNoteInput');
      const text = input.value.trim();

      if (!text) {
        alert('الرجاء كتابة الملاحظة أولاً.');
        return;
      }

      const newNote = {
        text: text,
        time: document.getElementById('liveTime').textContent,
        date: document.getElementById('liveDate').textContent
      };

      notesList.push(newNote);
      input.value = '';
      renderNotes();
      saveReportState();
    }

    // 5. عرض قائمة الملاحظات
    function renderNotes() {
      const container = document.getElementById('notesContainer');
      container.innerHTML = '';

      if (notesList.length === 0) {
        container.innerHTML = '<div class="text-xs text-gray-400 italic">لا توجد ملاحظات مضافة حتى الآن.</div>';
        return;
      }

      notesList.forEach((note) => {
        const item = document.createElement('div');
        item.className = 'bg-white p-3 rounded-lg border border-gray-200 shadow-sm text-xs';
        item.innerHTML = `
          <div class="flex justify-between items-center mb-1 text-gray-500 font-bold border-b pb-1">
            <span class="text-maroon-700 font-black"><i class="fa-solid fa-comment-dots text-gold-500 ml-1"></i> ملاحظة متابعة</span>
            <span class="text-[10px] dir-ltr">${note.date} | ${note.time}</span>
          </div>
          <p class="text-gray-800 leading-relaxed font-medium mt-1">${note.text}</p>
        `;
        container.appendChild(item);
      });
    }

    // 6. حفظ حالة التقرير والملاحظات
    function saveReportState() {
      const data = {
        status: document.getElementById('reportStatus').value,
        notes: notesList
      };

      localStorage.setItem('alabaa_report_view2_db', JSON.stringify(data));
      alert('تم حفظ حالة التقرير والملاحظات بنجاح!');
    }

    // 7. استرجاع البيانات المحفوظة
    function loadSavedData() {
      const saved = localStorage.getItem('alabaa_report_view2_db');
      if (saved) {
        try {
          const data = JSON.parse(saved);
          if (data.status) document.getElementById('reportStatus').value = data.status;
          if (data.notes) notesList = data.notes;
        } catch(e) {}
      }
      updateStatusStyle();
      renderNotes();
    }
  </script>
</body>
</html>
