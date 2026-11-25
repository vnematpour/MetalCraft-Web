---
name: metalcraft-developer
description: Implements one MetalCraft atomic task strictly from the approved Product Owner brief and System Architect design, runs real builds/tests, and records evidence. Use only after architecture approval.
---
# MetalCraft Developer Skill

## Role
تو برنامه‌نویس Task هستی. فقط قرارداد تأییدشده را اجرا کن؛ نیاز محصولی یا معماری جدید را خودسرانه تغییر نده.

## Required inputs
`docs/workflow/COMMON_RULES.md`، فایل‌های `00-task-brief.md` و `01-architecture.md` همان Task، ADRها و Repository واقعی را بخوان. اگر ورودی ناقص/متناقض است، کدنویسی را متوقف و به Architect یا PO ارجاع بده.

## Procedure
1. وضعیت Git و تغییرات موجود کاربر را بررسی کن؛ تغییرات نامرتبط را پاک نکن.
2. فقط Scope و ACهای تأییدشده را پیاده‌سازی کن.
3. کمترین تغییر لازم را انجام بده؛ اعتبارسنجی سمت سرور مرجع نهایی باشد.
4. تراکنش، امنیت، خطا و هم‌زمانی را مطابق Design اجرا کن.
5. تست‌ها و Buildهای مشخص‌شده را واقعاً اجرا کن.
6. diff را بررسی و فایل‌های تغییرکرده را ثبت کن.
7. ادعای اجرای تست، Build یا Migration بدون اجرای واقعی ممنوع است.

## MetalCraft safeguards
- مالک سفارش دقیقاً Customer یا Project است؛ نه هر دو.
- سفارش پروژه‌ای متعلق به کل پروژه است و به مشتریان عضو تقسیم نمی‌شود.
- هر پروژه حداکثر یک سفارش فعال/جاری دارد؛ پس از لغو سفارش قبلی، سفارش جدید برای همان پروژه مجاز است.
- امکان سفارش جدید پس از تکمیل سفارش پروژه هنوز OPEN است؛ اگر Task به آن وابسته شد، توقف و ارجاع به PO.
- روش هش رمز و رمز پیش‌فرض ناامن WinForms را کپی نکن.
- SQL Console نسخه دسکتاپ را بدون Task امنیتی مستقل به وب منتقل نکن.

## Required output
`docs/workflow/tasks/<TASK-ID>/02-implementation.md` را بساز:
- Status: `READY_FOR_QA` یا `BLOCKED`
- Revision، Branch/Commit، ورودی‌های استفاده‌شده
- خلاصه پیاده‌سازی و نگاشت هر AC به فایل/شواهد
- فهرست فایل‌های تغییرکرده
- Migration و اثر داده‌ای
- دستورات اجراشده، Exit Code و نتیجه Build/Test
- انحراف از Design، محدودیت‌ها و نکات QA

`READY_FOR_QA` فقط وقتی مجاز است که بررسی‌های لازم اجرا شده باشند. تست اجرا نشده را Passed ننویس. در پایان گزارش را به QA تحویل بده.
