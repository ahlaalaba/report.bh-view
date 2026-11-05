<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>عرض تقارير مضيف أهل العباء</title>
  <style>
    * { box-sizing: border-box; font-family: 'Traditional Arabic', 'Amiri', 'Segoe UI', Tahoma, sans-serif; }
    body { background-color: #f5f5f5; margin: 0; padding: 20px; color: #000; }

    .page-container {
      width: 210mm;
      min-height: 297mm;
      background: white;
      margin: 0 auto;
      padding: 30px;
      border: 2px solid #000;
      position: relative;
      box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    }

    /* الشعار العلوي */
    .header { text-align: center; margin-bottom: 20px; }
    .logo-container { display: inline-block; text-align: center; margin-bottom: 5px; }
    .flower-icon { width: 60px; height: 60px; margin: 0 auto 5px auto; position: relative; }
    .petal { width: 22px; height: 22px; border: 1.5px solid #8b0000; border-radius: 50%; position: absolute; background: #fff; }
    .p1 { top: 0; left: 19px; }
    .p2 { top: 12px; right: 2px; }
    .p3 { bottom: 8px; right: 7px; }
    .p4 { bottom: 8px; left: 7px; }
    .p5 { top: 12px; left: 2px; }
    .p-center { top: 18px; left: 19px; width: 22px; height: 22px; background: #8b0000; border-radius: 50%; }

    .logo-text-top { font-size: 26px; font-weight: bold; color: #444; margin: 0; line-height: 1; }
    .logo-text-main { font-size: 38px; font-weight: bold; color: #8b0000; margin: -5px 0 0 0; text-shadow: 1px 1px 0px #000; }
    .doc-title { font-size: 24px; font-weight: bold; margin: 15px 0 5px 0; border-bottom: 2px solid #8b0000; display: inline-block; padding-bottom: 5px; }

    /* شريط البحث والفلترة */
    .search-box {
      margin: 20px 0;
      display: flex;
      gap: 10px;
      justify-content: center;
      align-items: center;
    }
    .search-input {
      padding: 8px 12px;
      font-size: 16px;
      border: 2px solid #000;
      border-radius: 5px;
      width: 60%;
      outline: none;
    }

    /* جدول عرض التقارير */
    table { width: 100%; border-collapse: collapse; margin-top: 15px; margin-bottom: 30px; }
    th, td { border: 2px solid #000; text-align: center; vertical-align: middle; padding: 10px; font-size: 16px; }
    th { background-color: #f0f0f0; font-size: 18px; font-weight: bold; }
    
    .badge {
      display: inline-block;
      padding: 3px 8px;
      border-radius: 4px;
      background-color: #e9ecef;
      border: 1px solid #ccc;
      font-weight: bold;
    }

    .btn-action {
      padding: 5px 10px;
      margin: 2px;
      font-size: 14px;
      font-weight: bold;
      border: none;
      border-radius: 3px;
      cursor: pointer;
      color: white;
    }
    .btn-view { background-color: #17a2b8; }
    .btn-edit-admin { background-color: #dc3545; }

    /* الفوتر والتوقيع */
    .footer-text { text-align: right; font-size: 18px; font-weight: bold; margin-bottom: 15px; padding-right: 20px; }
    .footer-salutation { text-align: center; font-size: 20px; font-weight: bold; margin-bottom: 40px; }
    
    .bottom-section { display: flex; justify-content: space-between; align-items: flex-end; padding: 0 40px; }
    .signature-box { background: #000; color: #fff; width: 220px; padding: 12px; text-align: center; font-weight: bold; font-size: 18px; }
    
    .stamp-container { text-align: center; }
    .circle-stamp {
      width: 120px; height: 120px; border: 6px solid #000; border-radius: 50%;
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      margin-bottom: 8px; padding: 5px;
    }
    .circle-stamp .s-top { font-size: 14px; font-weight: bold; }
    .circle-stamp .s-main { font-size: 18px; font-weight: bold; color: #8b0000; }
    .square-stamp { background: #000; color: #fff; width: 90px; padding: 8px; text-align: center; font-weight: bold; font-size: 14px; margin: 0 auto; }

    /* أزرار التحكم */
    .controls { width: 210mm; margin: 20px auto 0 auto; display: flex; gap: 10px; justify-content: center; }
    .btn-main { padding: 1
  
