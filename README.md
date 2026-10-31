<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>مضيف أهل العباء - البلاد القديم | عرض ومتابعة الرصد (رقم 2)</title>
  
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800;900&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: { cairo: ['Cairo', 'sans-serif'] },
          colors: {
            maroon: { 50: '#fdf2f2', 100: '#fde8e8', 700: '#800000', 800: '#660000', 900: '#4a0000' },
            gold: { 400: '#fbbf24', 500: '#d97706', 600: '#b45309' }
          }
        }
      }
    }
  </script>

  <style>
    body { font-family: 'Cairo', sans-serif; background-color: #f3f4f6; }
    .watermark-bg { position: relative; }
    .watermark-bg::before {
      content: ""; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
      width: 75%; height: 75%;
      background-image: url('image_be668db9-removebg-preview.png');
      background-repeat: no-repeat; background-position: center; background-size: contain;
      opacity: 0.50; pointer-events: none; z-index: 0;
    }
    .print-content { position: relative; z-index: 10; }
    @media print {
      .no-print { display: none !important; }
      body { background-color: white !important; padding: 0 !important; margin: 0 !important; }
      .print-container { box-shadow: none !important; border: none !important; width: 100% !important; padding: 0 !important; }
      .watermark-bg::before { opacity: 0.30 !important; -webkit-print-color-adjust: exact; print-color-adjust: exact; }
    }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-3 md:p-6 text-gray-800">

  <div class="max-w-4xl mx-auto mb-20">
    <div class="print-container watermark-bg bg-white rounded-xl shadow-lg border border-gray-200 p-6 md:p-10 relative overflow-hidden">
      <div class="print-content">

        <header class="border-b-2 border-maroon-700 pb-6 mb-6">
          <div class="flex flex-col md:flex-row justify-between items-center gap-4 text-center md:text-right">
            <div class="flex items-center gap-4">
              <img src="image_be668db9-removebg-preview.png" alt="مضيف أهل العباء" class="h-20 w-auto object-contain">
              <div>
                <h1 class="text-2xl md:text-3xl font-black text-maroon-800 tracking-wide">مضيف أهل العباء</h1>
                <p class="text-xs md:text-sm text-gold-600 font-bold mt-1">
                  <i class="fa-solid fa-location-dot ml-1"></i> البلاد القديم — مملكة البحرين
                </p>
                <p class="text-[11px] text-gray-400 font-semibold mt-0.5">لوحة عرض ومتابعة الرصد والتقارير (رقم 2)</p>
              </div>
            </div>

            <div class="bg-maroon-50/90 backdrop-blur-sm border border-maroon-100 rounded-lg p-3 text-center md:text-left min-w-[210px]">
              <div id="liveDay" class="text-maroon-800 font-black text-sm">--</div>
              <div id="liveDate" class="text-gray-600 text-xs font-semibold my-0.5">--/--/----</div>
              <div id="liveTime" class="text-gold-600 font-black text-sm dir-ltr">--:--:-- --</div>
            </div>
          </div>
        </header>

        <!-- معلومات التقرير المستلم -->
        <div class="bg-gray-50/95 border border-gray-200 rounded-xl p-4 mb-6 grid grid-cols-1 md:grid-cols-3 gap-4">
          <div>
            <span class="block text-xs font-bold text-gray-500 mb-1">اسم الكريم (مُعد التقرير):</span>
            <div id="viewAuthor" class="text-sm font-black text-gray-800 bg-white p-2.5 rounded-lg border border-gray-200">جاري استقبال التقرير...</div>
          </div>

          <div>
            <span class="block text-xs font-bold text-gray-500 mb-1">رقم الهاتف:</span>
            <div id="viewPhone" class="text-sm font-black text-gray-800 bg-white p-2.5 rounded-lg border border-gray-200 dir-ltr text-right">--</div>
          </div>

          <!-- حالة التقرير (جديد ، قيد متابعة ، تم الحل) -->
          <div>
            <label class="block text-xs font-bold text-gray-500 mb-1">حالة وإشعار التقرير:</label>
            <select id="reportStatus" onchange="updateStatusStyle()" class="w-full border-2 rounded-lg p-2 text-sm font-black outline-none transition cursor-pointer">
              <option value="جديد" class="bg-blue-100 text-blue-800">🔵 جديد</option>
              <option value="قيد متابعة" class="bg-amber-100 text-amber-800">🟡 قيد متابعة</option>
              <option value="تم الحل" class="bg-green-100 text-green-800">🟢 تم الحل</option>
            </select>
          </div>
        </div>

        <!-- نص التقرير المرسل -->
        <div class="mb-8">
          <label class="block text-sm font-bold text-gray-700 mb-2">
            <i class="fa-solid fa-file-lines text-maroon-700 ml-1"></i> تفاصيل ومضمون التقرير المُرسل من (رقم 1):
          </label>
          <div id="viewContent" class="w-full min-h-[140px] border border-gray-200 rounded-xl p-4 bg-white/95 text-gray-800 text-sm font-medium leading-relaxed shadow-inner">
            في انتظار إرسال تقرير جديد من صفحة الإرسال...
          </div>
        </div>

        <!-- قسم الملاحظات والتعليقات -->
        <div class="border-t-2 border-dashed border-gray-300 pt-6 mb-6">
          <h3 class="text-base font-black text-maroon-800 mb-3 flex items-center gap-2">
            <i class="fa-solid fa-comments text-gold-500"></i> ملاحظات وتعليقات المتابعة:
          </h3>

          <div id="notesContainer" class="space-y-3 mb-4"></div>

          <div class="no-print bg-maroon-50/70 p-4 rounded-xl border border-maroon-100">
            <label class="block text-xs font-bold text-gray-700 mb-2">كتابة ملاحظة جديدة على التقرير:</label>
            <div class="flex flex-col md:flex-row gap-2">
              <textarea id="newNoteInput" rows="2" placeholder="اكتب ملاحظاتك أو تعليقك هنا..." class="w-full border border-gray-300 rounded-lg p-3 text-sm focus:ring-2 focus:ring-maroon-700 outline-none bg-white"></textarea>
              <button type="button" onclick="addNote()" class="px-5 py-2.5 bg-maroon-700 hover:bg-maroon-800 text-white rounded-lg font-bold text-sm shadow transition shrink-0 flex items-center gap-1.5">
                <i class="fa-solid fa-paper-plane text-gold-400"></i> إضافة ملاحظة
              </button>
            </div>
          </div>
        </div>

        <footer class="mt-8 pt-4 border-t border-gray-200 text-center text-xs text-gray-500 hidden print:block font-bold">
          مضيف أهل العباء — البلاد القديم (مملكة البحرين)
        </footer>

      </div>
    </div>
  </div>

  <div class="no-print fixed bottom-0 left-0 right-0 bg-white/95 backdrop-blur-md border-t border-gray-200 py-3 px-4 shadow-2xl z-50">
    <div class="max-w-4xl mx-auto flex items-center justify-between gap-3">
      <div class="text-xs text-gray-600 font-bold hidden md:flex items-center gap-2">
        <img src="image_be668db9-removebg-preview.png" alt="شعار" class="h-6 w-auto inline">
        مضيف أهل العباء - لوحة الرصد (رقم 2)
      </div>
      <div class="flex items-center gap-3 w-full md:w-auto justify-end">
        <button type="button" onclick="saveTrackingState()" class="px-5 py-2.5 bg-maroon-700 hover:bg-maroon-800 text-white rounded-lg font-bold text-sm transition shadow flex items-center gap-2">
          <i class="fa-solid fa-floppy-disk text-gold-400"></i> حفظ الحالة والملاحظات
        </button>
        <button type="button" onclick="window.print()" class="px-5 py-2.5 bg-gold-500 hover:bg-gold-600 text-white rounded-lg font-bold text-sm transition shadow flex items-center gap-2">
          <i class="fa-solid fa-print"></i> طباعة / PDF
        </button>
      </div>
    </div>
  </div>

  <script>
    let notesList = [];

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

    function updateStatusStyle() {
      const statusSelect = document.getElementById('reportStatus');
      const val = statusSelect.value;
      statusSelect.className = "w-full border-2 rounded-lg p-2 text-sm font-black outline-none transition cursor-pointer ";
      if (val === "جديد") statusSelect.classList.add("border-blue-500", "bg-blue-50", "text-blue-800");
      else if (val === "قيد متابعة") statusSelect.classList.add("border-amber-500", "bg-amber-50", "text-amber-800");
      else if (val === "تم الحل") statusSelect.classList.add("border-green-500", "bg-green-50", "text-green-800");
    }

    function addNote() {
      const input = document.getElementById('newNoteInput');
      const text = input.value.trim();
      if (!text) { alert('الرجاء كتابة الملاحظة أولاً.'); return; }
      notesList.push({ text: text, time: document.getElementById('liveTime').textContent, date: document.getElementById('liveDate').textContent });
      input.value = '';
      renderNotes();
      saveTrackingState();
    }

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

    function loadData() {
      const savedReport = localStorage.getItem('ahl_alabaa_bh_report');
      if (savedReport) {
        try {
          const report = JSON.parse(savedReport);
          document.getElementById('viewAuthor').textContent = report.name || 'غير معروف';
          document.getElementById('viewPhone').textContent = `${report.countryCode || ''} ${report.phone || ''}`;
          document.getElementById('viewContent').textContent = report.details || 'لا توجد تفاصيل';
        } catch(e) {}
      }

      const savedTracking = localStorage.getItem('ahl_alabaa_tracking_db');
      if (savedTracking) {
        try {
          const tracking = JSON.parse(savedTracking);
          if (tracking.status) document.getElementById('reportStatus').value = tracking.status;
          if (tracking.notes) notesList = tracking.notes;
        } catch(e) {}
      }
      updateStatusStyle();
      renderNotes();
    }

    function saveTrackingState() {
      const data = { status: document.getElementById('reportStatus').value, notes: notesList };
      localStorage.setItem('ahl_alabaa_tracking_db', JSON.stringify(data));
      alert('تم حفظ الحالة والملاحظات بنجاح!');
    }

    window.addEventListener('DOMContentLoaded', loadData);
  </script>
</body>
</html>
