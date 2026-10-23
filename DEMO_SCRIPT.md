# سناریوی ویدئوی دمو (حدود ۹۰ ثانیه)

## ۰:۰۰ تا ۰:۱۰ — مسئله

«وقتی سرویس روی لیارا دچار خطا می‌شود، داشتن metric به‌تنهایی کافی نیست. باید بفهمیم چه رخ داده و قبل از تغییر زیرساخت آن را قابل بازبینی کنیم.»

## ۰:۱۰ تا ۰:۲۵ — محصول

ترمینال را باز کنید و بگویید:

```bash
cd outputs/liara-ops
python3 -m liara_ops demo
```

«دستیار سه کار می‌کند: observe، explain، plan. خروجی JSON شامل متریک، evidence، confidence و operation است.»

## ۰:۲۵ تا ۰:۴۵ — plan استقرار

```bash
python3 -m liara_ops deploy checkout-api --strategy blue-green
```

«در حالت پیش‌فرض هیچ عملیاتی اجرا نمی‌شود. فقط plan می‌بینیم و می‌توانیم image، branch، region و replicas را بررسی کنیم.»

## ۰:۴۵ تا ۱:۰۵ — تشخیص incident

```bash
python3 -m liara_ops diagnose checkout-api --scenario high_error_rate
```

«عامل شواهد را جمع می‌کند: نرخ خطا ۷٫۸ درصد و p95 برابر ۱۱۲۰ میلی‌ثانیه است. نتیجه rollback به آخرین نسخه پایدار است، اما هنوز قفل تأیید انسانی فعال است.»

## ۱:۰۵ تا ۱:۲۰ — تأیید و audit

```bash
python3 -m liara_ops diagnose checkout-api --scenario high_error_rate --approve
```

«این تأیید فقط dry-run را اجرا می‌کند. هیچ درخواست شبکه‌ای ارسال نمی‌شود؛ payload و زمان در audit log ثبت شده‌اند. در نسخه production، همین facade به adapter رسمی Liara متصل می‌شود.»

## ۱:۲۰ تا ۱:۳۰ — جمع‌بندی

«Liara Ops کمک می‌کند اقدام درست سریع‌تر پیدا شود، بدون اینکه عامل مستقلانه زیرساخت را تغییر دهد: evidence، plan، approval و audit در یک مسیر روشن.»

## اجرای همان سناریو در مرورگر

برای تست انسانی رابط، بدون اتصال به حساب واقعی:

```bash
python3 -m liara_ops web --port 8123
```

سپس `http://127.0.0.1:8123` را باز کنید. از پنل سمت راست سناریو را انتخاب کنید، ابتدا evidence و plan را ببینید و فقط در صورت تأیید، دکمه‌ی ثبت `dry-run` را بزنید.
