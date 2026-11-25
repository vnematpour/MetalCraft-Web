# MetalCraft AI Development Framework

بسته چهار اسکیل نقش‌محور و پروتکل زنجیره‌ای توسعه MetalCraft.

## Skills
- `skills/metalcraft-product-owner/SKILL.md`
- `skills/metalcraft-system-architect/SKILL.md`
- `skills/metalcraft-developer/SKILL.md`
- `skills/metalcraft-qa/SKILL.md`

## Workflow and docs
- `docs/workflow/METALCRAFT_WORKFLOW.md`
- `docs/workflow/COMMON_RULES.md`
- `docs/workflow/TASK_INDEX.md`
- `docs/workflow/DECISION_LOG.md`
- `docs/workflow/templates/` — قالب اسناد پنج مرحله هر Task
- `docs/discovery/` — مستندات تحلیل MetalCraft و تصمیم‌های تأییدشده فعلی

## Install
محتویات را در ریشه Repository نسخه وب Merge کن و ساختار پوشه‌ها را حفظ کن. اگر فایل هم‌نام وجود دارد، قبل از جایگزینی diff بگیر.

## Run
برای درخواست جدید، ابتدا فقط Product Owner را اجرا کن. پس از تولید و تأیید `00-task-brief.md`، به‌ترتیب Architect، Developer، QA و در پایان PO Acceptance اجرا شوند. هیچ Task بدون QA PASS و PO ACCEPTED Done نیست.

## Compatibility note
ساختار Skillها از قالب عمومی Agent Skills با `SKILL.md` و frontmatter استفاده می‌کند. پشتیبانی دقیق به محصول هوش مصنوعی بستگی دارد. این فایل‌ها خودشان Agent مستقل ایجاد نمی‌کنند؛ اگر چند Agent در دسترس نیست، نقش‌ها باید در نوبت‌های جدا اجرا شوند و خروجی فایل‌محور بین آن‌ها تحویل شود.
