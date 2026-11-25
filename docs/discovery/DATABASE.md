# تحلیل پایگاه داده

## 1. تکنولوژی و اتصال

- دیتابیس فعلی SQLite است.
- Provider: `Microsoft.Data.Sqlite` نسخه `10.0.14`.
- نام فایل: `MetalCraft.db`.
- مسیر دیتابیس با متغیر محیطی `METALCRAFT_DB` قابل تعیین است؛ در غیر این صورت ابتدا پوشه Data کنار برنامه و در صورت عدم امکان نوشتن، LocalApplicationData بررسی می‌شود.
- اتصال با `ForeignKeys=true` باز می‌شود.

## 2. جداول و Viewهای مشاهده‌شده در دیتابیس Debug

| شیء | نقش |
|---|---|
| Customers | اطلاعات مشتریان |
| Projects | اطلاعات پروژه |
| ProjectCustomers | رابطه چندبه‌چند پروژه و مشتری |
| Orders | سفارش و مبالغ و وضعیت |
| OrderItems | اقلام سفارش |
| Stations | ایستگاه‌های تولید |
| OrderStations | مراحل مسیر تولید سفارش |
| Payments | پرداخت‌های سفارش |
| Users | حساب‌های کاربری |
| Settings | تنظیمات کلید/مقدار |
| vw_OrderSummary | View خلاصه سفارش، مالک، مبلغ و مانده |

## 3. ایندکس‌های مشاهده‌شده در Debug

- `IX_Customers_Name`
- `IX_Customers_Phone`
- `IX_OrderItems_Order`
- `IX_OrderStations_Order`
- `IX_OrderStations_Station`
- `IX_Orders_Customer`
- `IX_Orders_ProjectId`
- `IX_Orders_Station`
- `IX_Orders_Status`
- `IX_Payments_Order`
- `IX_ProjectCustomers_CustomerId`
- `UX_Projects_Code` (Unique و فیلترشده برای Code غیرخالی)

## 4. Constraintهای مهم

- کد مشتری Unique است.
- نام کاربری Unique و بدون حساسیت به بزرگی/کوچکی حروف است.
- OrderNumber یکتا است.
- OrderType فقط `Production` یا `Service` است.
- Status فقط چهار مقدار Enum را می‌پذیرد.
- سفارش باید دقیقاً یک مالک از نوع Customer یا Project داشته باشد.
- OrderItem.Quantity > 0 و UnitPrice >= 0.
- Payment.Amount > 0.
- حذف Order اقلام، مراحل و پرداخت‌ها را Cascade می‌کند.
- حذف Customer/Project مورد ارجاع سفارش با RESTRICT محدود شده است.
- حذف Station، ارجاع‌های مربوط را با SET NULL تغییر می‌دهد.

## 5. اختلاف دیتابیس Debug و Release

این مهم‌ترین نکته دیتابیسی آرشیو است:

| ویژگی | Debug | Release |
|---|---|---|
| Projects | دارد | ندارد |
| ProjectCustomers | دارد | ندارد |
| Orders.ProjectId | دارد | ندارد |
| Customer.Phone2 | دارد | ندارد |
| OrderItems.Color و Brand | دارد | ندارد |
| Orders.ProformaNumber و InvoiceNumber | دارد | ندارد |
| شاخص ProjectId و ارتباط ProjectCustomers | دارد | ندارد |
| View با LEFT JOIN برای مشتری یا پروژه | دارد | View قدیمی و Customer-only |

بنابراین دیتابیس Release داخل آرشیو با سورس فعلی هم‌نسخه نیست. برای انتقال داده باید ابتدا نسخه مرجع Schema مشخص شود.

## 6. مسئله فایل Schema.sql

فایل `Data/Schema.sql` در آرشیو شامل Queryهای عملیاتی است؛ مانند SELECT/INSERT/UPDATE برای مشتری، سفارش، پرداخت و داشبورد. در آن اسکریپت DDL کامل ایجاد جدول‌ها و Viewها مشاهده نشد.

با این وجود `Db.Initialize()`، محتوای همین Resource را با `Execute` اجرا می‌کند. از روی سورس فعلی، ایجاد دیتابیس کاملاً خالی قابل اتکا نیست و باید به‌صورت عملی بررسی شود. ممکن است فایل DB آماده، بخشی از فرآیند نصب فعلی باشد یا فایل SQL به اشتباه جایگزین شده باشد؛ این موضوع هنوز قطعی نیست.

## 7. نسخه‌بندی Schema

`Db.SchemaVersion` مقدار 1 دارد و مقدار آن در Settings ذخیره می‌شود. در سورس بررسی‌شده Migrationهای صریح از نسخه N به N+1 دیده نشد.

برای نسخه وب:
- Migrationها باید در Repository کنترل نسخه ثبت شوند.
- نصب روی دیتابیس خالی باید تست شود.
- ارتقا از نسخه قبلی باید تست شود.
- مهاجرت از SQLite باید جدا از Migrationهای دیتابیس مقصد طراحی شود.
- قبل از هر Migration مخرب، Backup و امکان Rollback/Recovery مشخص شود.

## 8. پیشنهاد فنی

انتخاب دیتابیس مقصد را تا تعیین محیط استقرار و تعداد کاربران هم‌زمان قطعی نکنیم. اگر نسخه وب قرار است چندکاربره و روی سرور LAN اجرا شود، دیتابیس Server-based معمولاً گزینه مناسب‌تری از اشتراک مستقیم فایل SQLite است؛ اما تصمیم نهایی باید با نیاز عملیاتی و محیط نصب گرفته شود.
