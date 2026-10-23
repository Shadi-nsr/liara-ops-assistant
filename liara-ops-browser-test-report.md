# گزارش تست Liara Ops

تاریخ بررسی: `۱۴۰۵/۰۸/۰۱`

## مسیرهای بررسی‌شده

- `healthy`: متریک سالم، بدون پیشنهاد اقدام اصلاحی و بدون approval gate.
- `high_error_rate`: evidence شامل نرخ خطای `۷٫۸٪` و `p95=۱۱۲۰ms`؛ پیشنهاد `rollback` تا قبل از تأیید متوقف می‌ماند.
- `crash_loop`: پیشنهاد `restart` فقط برای نمونه‌ی ناسالم.
- `oom`: پیشنهاد `scale` از `۳` به `۴` replica.
- `deploy`: ساخت plan، توقف قبل از اجرا و ثبت `dry-run` بعد از تأیید.
- ورودی نامعتبر مثل `../unsafe` و بیش از `۱۰۰` replica با خطای قابل فهم رد می‌شود.
- خروجی مرورگر و کنسول بدون خطای `warning` یا `error` بررسی شد.

## نتیجه

تمام عملیات همچنان آفلاین و `dry-run` هستند. هیچ حساب لیارا، کد پروژه یا زیرساخت واقعی تغییر نکرده است. برای اتصال production، facade فعلی محل اتصال adapter رسمی Liara و ایجنت کدنویسی برای patch و redeploy تأییدشده است.

## اجرای مجدد

```bash
cd outputs/liara-ops
python3 -m unittest discover -s tests -v
python3 -m liara_ops web --port 8123
```
