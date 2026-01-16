# تعليمات رفع التحديثات إلى السيرفر

## المشكلة التي تم إصلاحها
تم إصلاح أخطاء في ملف `app.html` كانت تمنع الموقع من التحميل بشكل صحيح. المشكلة كانت في بناء جملة Jinja2 template في قسم CSS حيث كانت الأقواس المتداخلة مكسورة.

## الملفات المعدلة
- `datavalue_theme_15/www/app.html` - تم إصلاح أخطاء Jinja2 template syntax

## خطوات رفع التحديثات

### 1. الاتصال بالسيرفر عبر SSH
```bash
ssh username@sarh-nobel.nobelapp.online
```

### 2. الانتقال إلى مجلد البنش (Bench)
```bash
cd /path/to/frappe-bench
```

### 3. إيقاف الموقع مؤقتاً (اختياري)
```bash
bench --site sarh-nobel.nobelapp.online set-maintenance-mode on
```

### 4. رفع الملف المعدل
يمكنك استخدام إحدى الطرق التالية:

#### الطريقة الأولى: استخدام Git
إذا كان الثيم في مستودع Git:
```bash
cd apps/datavalue_theme_15
git pull origin main
cd ../..
bench build --app datavalue_theme_15
bench restart
```

#### الطريقة الثانية: نسخ الملف مباشرة
استخدم SCP أو SFTP لنسخ الملف:
```bash
# من جهازك المحلي
scp "c:\Users\ًWinDows\Desktop\DD\datavalue_theme-main\datavalue_theme_15\www\app.html" username@sarh-nobel.nobelapp.online:/path/to/frappe-bench/apps/datavalue_theme_15/datavalue_theme_15/www/app.html
```

ثم على السيرفر:
```bash
bench build --app datavalue_theme_15
bench restart
```

### 5. مسح الكاش
```bash
bench clear-cache
bench clear-website-cache
```

### 6. إعادة تشغيل الموقع
```bash
bench restart
```

### 7. إلغاء وضع الصيانة (إذا تم تفعيله)
```bash
bench --site sarh-nobel.nobelapp.online set-maintenance-mode off
```

### 8. التحقق من عمل الموقع
افتح المتصفح وتوجه إلى:
https://sarh-nobel.nobelapp.online/app

## ملاحظات مهمة
- تأكد من أخذ نسخة احتياطية من الملف الأصلي قبل الاستبدال
- إذا واجهت مشاكل، يمكنك استعادة الملف الأصلي
- تأكد من صلاحيات الملفات بعد النسخ:
  ```bash
  chown -R frappe:frappe /path/to/frappe-bench/apps/datavalue_theme_15
  ```

## التحقق من الأخطاء
إذا لم يعمل الموقع بعد التحديث، تحقق من سجلات الأخطاء:
```bash
# سجل أخطاء Frappe
tail -f /path/to/frappe-bench/logs/web.error.log

# سجل أخطاء Nginx
tail -f /var/log/nginx/error.log
```
