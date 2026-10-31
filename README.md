<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>مضيف أهل العباء - البلاد القديم | عرض ومتابعة الرصد</title>
  
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

  <div class="max-w-4xl mx-auto mb-20">
    
    <!-- ورقة رصد ومتابعة التقارير -->
    <div class="print-container watermark-bg bg-white rounded-xl shadow-lg border border-gray-200 p-6 md:p-10 relative overflow-hidden">
      
      <div class="print-content">

        <!-- الترويسة الهيدر الرسمية -->
        <header class="border-b-2 border-maroon-700 pb-6 mb-6">
          <div class="flex flex-col md:flex-row justify-between items-center gap-4 text-center md:text-right">
            
            <div class="flex items-center gap-4">
              <img src="image_be668db9-removebg-preview.png" alt="مضيف أهل العباء" class="h-20 w-auto object-contain">
              <div>
                <h1 class="text-2xl md:text-3xl font-black text-maroon-800 tracking-wide">مضيف أهل العباء</h1>
                <p class="text-xs md:text-sm text-gold-600 font-bold mt-1">
                  <i class="fa-solid fa-location-dot ml-1"></i> البلاد القديم — مملكة البحرين
                </p>
                <p class="text-[11px] text-gray-400 font-semibold mt-0.5">لوحة عرض ومتابعة الرصد والتقارير</p>
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

        <!-- بيانات الرصد المسجلة (اسم الكريم، الهاتف، الحالة) -->
        <div class="bg-gray-50/95 border border-gray-200 rounded-xl p-4 mb-6 grid grid-cols-1 md:grid-cols-3 gap-4">
          
          <div>
            <span class="block text-xs font-bold text-gray-500 mb-1">اسم الكريم (مُعد التقرير):</span>
            <div id="viewAuthor" class="text-sm font-black text-gray-800 bg-white p-2.5 rounded-lg border border-gray-200">جاري التحميل...</div>
          </div>

          <div>
            <span class="block text-xs font-bold text-gray-500 mb-1">رقم الهاتف:</span>
            <div id="viewPhone" class="text-sm font-black text-gray-800 bg-white p-2.5 rounded-lg border border-gray-200 dir-ltr text-right">--</div>
          </div>

          <div>
            <label class="block text-xs font-bold text-gray-500 mb-1">حالة الرصد:</label>
            <select 
              id="reportStatus" 
              onchange="updateStatusStyle()" 
              class="w-full border-2 rounded-lg p-2 text-sm font-black outline-none transition cursor-pointer"
            >
              <option value="جديد" class="bg-blue-100 text-blue-800">🔵 جديد</option>
              <option value="قيد متابعة" class="bg-amber-100 text-amber-800">🟡 قيد متابعة</option>
              <option value="تم الحل" class="bg-green-100 text-green-800">🟢 تم الحل</option>
            </select>
          </div>

        </div>

        <!-- تفاصيل التقرير أو الرصد الأصلي -->
        <div class="mb-8">
          <label class="block text-sm font-bold text-gray-700 mb-2">
            <i class="fa-solid fa-file-lines text-maroon-700 ml-1"></i> تفاصيل الرصد أو الطلب المرسل:
          
      
