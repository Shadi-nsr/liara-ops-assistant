# Liara Ops Assistant

دستیار امن برای استقرار، پایش و رفع‌اشکال سرویس‌های لیارا. این نمونه برای چالش لیارا ساخته شده و از ابتدا با اصل **توضیح‌پذیری قبل از اجرا** طراحی شده است: عامل ابتدا evidence و plan تولید می‌کند، عملیات حساس را متوقف می‌کند تا انسان تأیید کند و سپس درخواست را در یک API client قابل تعویض ثبت می‌کند.

این نسخه کاملاً آفلاین است. `DryRunTransport` هیچ سوکت شبکه‌ای باز نمی‌کند و هیچ حساب یا زیرساختی را تغییر نمی‌دهد. اتصال واقعی Liara یک seam مشخص در `liara_ops/client.py` دارد و عمداً در این تحویل فعال نشده است.

## اجرا

```bash
cd outputs/liara-ops
python3 -m liara_ops demo
```

خروجی JSON شامل plan استقرار، متریک، تشخیص incident، عملیات پیشنهادی و audit log است. برای استفاده‌ی مرحله‌ای:

```bash
# فقط plan؛ هیچ عملیاتی اجرا نمی‌شود
python3 -m liara_ops deploy checkout-api --branch main --strategy blue-green

# همان plan با تأیید انسانی؛ همچنان فقط dry-run است
python3 -m liara_ops deploy checkout-api --branch main --strategy blue-green --approve

# پایش یک سناریوی شبیه‌سازی‌شده
python3 -m liara_ops monitor checkout-api --scenario high_error_rate

# evidence و پیشنهاد rollback؛ بدون --approve اجرا نمی‌شود
python3 -m liara_ops diagnose checkout-api --scenario high_error_rate

# اجرای dry-run اقدام پیشنهادی پس از تأیید انسانی
python3 -m liara_ops diagnose checkout-api --scenario high_error_rate --approve
```

## چرخه‌ی عامل

1. **Observe:** snapshot متریک‌ها و وضعیت replicaها را می‌خواند.
2. **Explain:** finding را با severity، شواهد، confidence و next steps چاپ می‌کند.
3. **Plan:** عملیات typed مثل `rollback`، `restart` یا `scale` می‌سازد.
4. **Approve:** عملیات medium/high بدون `--approve` متوقف می‌ماند.
5. **Act:** درخواست در `DryRunTransport` ثبت می‌شود و audit log قابل بررسی تولید می‌کند.

## تست

```bash
cd outputs/liara-ops
python3 -m unittest discover -s tests -v
```

## نکات طراحی برای داوری

- **Safety by default:** حالت پیش‌فرض فقط plan است؛ حتی با `--approve` هم اجرای این بسته dry-run باقی می‌ماند.
- **قابل اتصال به API:** facade متدهای `plan_deploy`، `deploy`، `metrics` و `remediate` را ثابت نگه می‌دارد تا adapter رسمی بدون تغییر در planner جایگزین شود.
- **قابل توضیح:** هر finding دلیل، داده‌ی پشتیبان و اعتماد مدل را دارد.
- **قابل بازبینی:** هر call با method، path، payload، زمان و برچسب dry-run در audit ثبت می‌شود.
- **بازگشت‌پذیر:** actionهای پرریسک به‌صورت operation مستقل با شناسه تولید می‌شوند و قبل از ارسال قابل بازبینی‌اند.

