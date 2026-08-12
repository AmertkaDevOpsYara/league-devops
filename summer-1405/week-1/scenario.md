<div dir="rtl">

<div align="center">

# به نام خدا

</div>
---
هدف از سناریوی عملی ارزیابی توانایی شرکت‌کنندگان در طراحی، پیاده‌سازی، استقرار، پایش، عیب‌یابی و نگهداری یک سامانه Native Cloud مطابق استانداردهای DevOps است.

## فهرست مطالب

1. قوانین کلی سناریوی عملی مسابقه
2. سناریو هفته نخست

   * 2.1 آماده‌سازی زیرساخت

     * مدیریت کد و همکاری تیمی
     * مدیریت Secrets
     * توسعه Ansible Collection
     * Provisioning مانیتورینگ
     * Alerting Rule
     * Webhook Notification Service
   * 2.2 استقرار نخستین نسخه نرم‌افزار

     * 2.2.1 کنترل کیفیت کد Quality Left-Shift
     * 2.2.2 کنترل‌های امنیتی Pipeline Shift-Left Security
   * 2.3 برآیند سناریوی هفته نخست
3. راهنمای آماده سازی کانال بله جهت هشداردهی

---

# 1. قوانین کلی سناریوی عملی مسابقه

* همه شرکت کنندگان موظف اند در سیستم احراز هویت مرکزی به آدرس `com.amertka.sso` حساب کاربری با فرمت `lastname.firstname` ایجاد کنند.
* تمامی تغییرات باید از طریق GitLab و CI/CD انجام شوند.
* همه Configuration ها و Source Code ها باید داخل GitLab نگهداری شود.
* تمامی Repository ها باید دارای README باشند.
* شرکت‌کنندگان مجاز هستند برای یافتن بهترین راهکارها از مستندات رسمی و منابع معتبر اینترنت استفاده کنند.
* همه شرکت کنندگان موظف اند Image ها و Package ها و Library های مورد نیاز خود را از Repository Registry برگزار کننده مسابقات تهیه کنند.

### Python Package Repository

* `pbr.amertka.com/pyhub`

### Operating System Repository

* `pbr.amertka.com/debian`

* `pbr.amertka.com/ubuntu`

* تمامی نام کاربری و پسورد و دیگر داده های مورد نیاز شرکت کنندگان در سامانه کلید در سازمان DevOps-League به آدرس `com.amertka.kelid` توسط برگزار کننده قرار داده خواهد شد.

* به منظور اجرای pipeline های CI/CD می‌توانید از Tag Runner با نام `runner_ady` استفاده کنید.

---

# 2. سناریو هفته نخست

شرکت Amertka در حال مهاجرت از زیرساخت سنتی به معماری Native-Cloud است. نخستین سرویس این شرکت آماده ورود به چرخه انتشار بوده و تیم DevOps مسئول طراحی، پیاده‌سازی و آماده‌سازی پلتفرمی استاندارد برای توسعه، استقرار، مانیتورینگ و بهره‌برداری از این سرویس است.

هدف این مرحله، ایجاد بستری استاندارد، امن، خودکار و قابل تکرار برای مدیریت چرخه عمر نرم‌افزار است؛ به گونه‌ای که تمامی فرآیندهای زیرساختی و انتشار نرم‌افزار بر پایه Infrastructure as Code و Automation CI/CD مدیریت شوند.

---

## 2.1 آماده‌سازی زیرساخت

در این مرحله، تیم DevOps موظف است زیرساخت‌های پایه مورد نیاز برای توسعه و استقرار سرویس‌ها را آماده‌سازی و استانداردسازی کند.

### مدیریت کد و همکاری تیمی

سامانه Source Control Management یا SCM با آدرس `com.amertka.git` آماده سازی شده و باید به همراه ساختار استاندارد پروژه‌ها، مدیریت کاربران، سطوح دسترسی، Branch Strategy و فرآیندهای همکاری تیمی راه‌اندازی و پیکربندی شود تا تمامی تغییرات زیرساخت و نرم‌افزار از طریق مخازن Git مدیریت شوند.

#### انتظارات

* ایجاد Group و Project ها (گروه اصلی باید با نام تیم یکی باشد)
* استفاده از Merge Request برای دیپلوی تمامی تغییرات
* Branch Strategy براساس Gitflow
* تعریف و مدیریت دسترسی اعضای تیم
* قرار دادن Webhook Gitlab Amertka برای هر پروژه

> **نکته:** اضافه کردن داوران به گروه هر تیم‌ها الزامی می‌باشد.

---

### مدیریت Secrets

به منظور نگهداری اطلاعات محرمانه سامانه کلید (محل نگهداری اطلاعات حساس) راه‌اندازی شده تا اطلاعاتی نظیر Credential ها، Password ها، API Token ها، Private Key ها و سایر داده‌های حساس در محل‌های نامربوط و ناامن نگهداری نشود.

#### انتظارات

* هر تیم دارای یک Organization و Collections های مورد نیاز بین هم تیمی ها با دسترسی مناسب

> **نکته:** اضافه کردن داوران به عنوان مالک سازمان

---

### توسعه Ansible Collection

در راستای پیاده‌سازی رویکرد Infrastructure as Code، یک Ansible Collection مطابق استانداردهای Ansible Galaxy توسعه داده شود که قابلیت آماده‌سازی، پیکربندی و مدیریت اجزای زیرساخت را فراهم کند.

این Collection حداقل باید شامل Role های زیر باشد:

#### Linux Management

* ایجاد و حذف کاربران سیستم‌عامل
* پیکربندی Repository های Debian
* نصب Package های پایه شامل:

  * vim
  * curl
  * ca-certificates

#### Kubernetes Management

* استقرار یک سرویس وب Nginx روی Kubernetes با استفاده از Ansible

#### انتظارات

* انتشار Collection در Galaxy آمرتکا

---

### Provisioning مانیتورینگ

تمامی تنظیمات و منابع Grafana باید به صورت Declarative و با استفاده از OpenTofu مدیریت شوند تا ایجاد Dashboard ها، Data Source ها و سایر تنظیمات به صورت نسخه‌بندی شده و قابل تکرار انجام گیرد.

همچنین پس از لاگین به سرویس Grafana با استفاده از SSO به تیم پشتیبانی اعلام نمایید تا دسترسی به سازمان مربوطه را اعمال کند.

#### Address Dashboard

##### Application

`https://pbr.amertka.com/rawhub/Dashboards/FlaskMonitoring.json`

##### Kubernetes

`https://pbr.amertka.com/rawhub/Dashboards/KubeNamespaceMonitoring.json`

#### انتظارات

* پیکربندی Organization هر تیم در Grafana توسط OpenTofu باید انجام شود.
* Dashboard ها با پوشه بندی Infrastructure و Application به طور جداگانه باید باشند.

  * Infrastructure
  * Application

---

### Alerting Rule

زیرساخت مانیتورینگ شامل Grafana، Prometheus و Alertmanager از پیش آماده شده است.

هر تیم موظف است یک Repository مستقل برای مدیریت Alerting Rules ایجاد کرده و تمامی قوانین هشداردهی مورد نیاز را به صورت فایل‌های Version Controlled در آن نگهداری کند.

پس از تکمیل و اعتبارسنجی Rule ها، تغییرات باید از طریق Merge Request به شاخه Main ارسال شوند. فرآیند بررسی، بازبینی فنی (Review) و اعمال نهایی تغییرات توسط تیم پشتیبانی انجام خواهد شد و پس از تأیید، قوانین جدید در محیط مانیتورینگ Deploy و فعال می‌شوند.

به منظور مشاهده وضعیت Alert ها، بررسی رخدادهای فعال و مدیریت Notification ها، می‌توانید از طریق آدرس زیر به سرویس Alertmanager دسترسی داشته باشید.

جهت دسترسی Pipeline های تیم زیرساخت به Alert Rule های هر تیم، Access Token پروژه با دسترسی مناسب را در اختیار تیم پشتیبانی قرار دهید.

#### انتظارات

##### طراحی و پیاده‌سازی Prometheus Alert Rules برای Flask

* Flask Service Down
* High HTTP 5xx Error Rate
* Performance
* Too Many Requests
* Exception Rate
* Database Connection Failure

##### سایر موارد

* دسته‌بندی هشدارها بر اساس Severity
* پوشش رویدادهای مرتبط با زیرساخت، Kubernetes و سرویس کاربردی

---

### Webhook Notification Service

هر تیم باید یک سرویس Webhook توسعه دهد که پس از دریافت رویداد از Alertmanager، به صورت خودکار پیام هشدار را در پیام‌رسان بله ارسال کند.

این سرویس باید قابلیت دریافت Alert های مختلف، پردازش داده‌های ورودی و تولید پیام مناسب برای تیم عملیاتی را داشته باشد.

#### انتظارات

* توسعه Webhook اختصاصی توسط هر تیم
* ارسال هشدارها به پیام‌رسان بله
* استقرار سرویس بر روی Kubernetes در Namespace اختصاصی تیم
* دسترسی از طریق دامنه

---

## 2.2 استقرار نخستین نسخه نرم‌افزار

پس از آماده‌سازی زیرساخت، نخستین نسخه نرم‌افزار باید با استفاده از Pipeline های CI/CD تولید، اعتبارسنجی و منتشر شود.

فرآیند استقرار باید بر پایه GitOps و با استفاده از Argo CD انجام گیرد تا وضعیت محیط عملیاتی همواره با وضعیت مخازن Git همگام باشد.

در پایان از راه:

`com.amertka.league.<Team Name>`

در دسترس باشد.

برای مشاهده و دریافت کد به این آدرس مراجعه کنید:

`https://git.amertka.com/devops-league/scenario-league`

### انتظارات

* آماده‌سازی Kubernetes Manifest ها
* اجرای تست‌های نرم‌افزاری
* اجرای کنترل‌های امنیتی
* Build و انتشار Container Image
* استقرار بر روی Kubernetes
* مدیریت Release ها از طریق GitOps و Argo CD

---

### 2.2.1 کنترل کیفیت کد (Quality Left-Shift)

به منظور اجرای رویکرد Left-Shift، پیش از Push هرگونه تغییر به مخزن Git، تمامی کدهای Python باید به صورت خودکار بررسی شده و تمامی خطاها و مغایرت‌ها پیش از ارسال کد برطرف شوند.

با استفاده از ابزار Ruff حداقل کنترل‌های مورد نیاز عبارت‌اند از:

* سازگاری کامل با Python 3.11
* رعایت استانداردهای PEP 8
* حداکثر طول ۱۲۰ کاراکتر برای هر خط
* شناسایی متغیرهای تعریف نشده (Undefined Variables)
* حذف متغیرهای استفاده نشده (Unused Variables)
* حذف Import های استفاده نشده (Unused Imports)
* مرتب‌سازی و گروه‌بندی Import ها
* به‌روزرسانی Syntax های قدیمی و استفاده از قابلیت‌های Python 3.11
* ساده‌سازی و بهینه‌سازی ساختار کد (Code Simplification)
* شناسایی پارامترهای استفاده نشده در توابع
* استفاده یکنواخت از Double Quotes
* افزایش خوانایی، نگهداشت‌پذیری و یکپارچگی کد
* شناسایی آسیب‌پذیری‌های امنیتی و خطاهای منطقی رایج

---

### 2.2.2 کنترل‌های امنیتی Pipeline Shift-Left Security

پس از Push کد به سامانه SCM، کد باید تحلیل شده و حداقل کنترل‌های زیر اعمال شود (با استفاده از ابزار Semgrep):

* شناسایی فعال بودن Debug Mode در محیط Production
* شناسایی آسیب‌پذیری Path Traversal
* شناسایی Cross-Site Scripting (XSS)
* شناسایی Dangerous Deserialization
* شناسایی استفاده ناامن از subprocess
* شناسایی OS Command Execution
* شناسایی Command Injection
* شناسایی SQL Injection
* شناسایی Hardcoded Secrets شامل:

  * Password
  * API Key
  * Access Token
  * Private Key
  * Connection String
  * سایر اطلاعات محرمانه

در صورت شناسایی هر یک از موارد فوق، Pipeline باید با وضعیت Failed خاتمه یافته و از ادامه مراحل Merge، Build و Release تا زمان رفع کامل مشکلات جلوگیری کند.

---

## 2.3 برآیند سناریوی هفته نخست

* Git Repository Structure
* Ansible Collection
* Terraform Project
* Kubernetes Manifests
* CI/CD Pipelines
* Grafana Dashboards
* Prometheus Alert Rules
* Webhook Notification Service
* GitOps Deployment Configuration
* Documentation

---

## ساختار پیشنهادی Repository

```text
Team-name (GitLab Group)
│
├── infrastructure (Sub Group)
│ │
│ ├── OpenTofu (Project)
│ │ ├── dashboards/
│ │ ├── .gitlab-ci.yml
│ │ └── README.md
│ │
│ ├── ansible (Project)
│ │ ├── ansible/
│ │ │ └── <name of team>
│ │ │ └── <collection name>
│ │ │ ├── docs/
│ │ │ ├── README.md
│ │ │ └── roles/
│ │ │ ├── kubernetes/
│ │ │ └── linux/
│ │ ├── .gitlab-ci.yml
│ │ └── README.md
│ │
│ ├── prometheus-alert-rules (Project)
│ │ ├── rules/
│ │ │ └── application-alert.yml
│ │ ├── .gitlab-ci.yml
│ │ └── README.md
│ │
│ └── webhook-send-alert (Project)
│ ├── main.py
│ ├── .gitlab-ci.yml
│ ├── requirements.txt
│ ├── Dockerfile
│ └── README.md
│
└── application (Project)
 ├── source/
 ├── pipelines/
 ├── Dockerfile
 ├── requirements.txt
 ├── .gitlab-ci.yml
 └── README.md
```

---

# 4. راهنمای آماده سازی کانال بله جهت هشداردهی

## 4.1 ساخت کانال

ساخت کانال در پیام رسان بله با فرمت:

```text
Alert_Team Name
```

## 4.2 افزودن اعضا

عضو کردن داوران و هم تیمی ها و فرستادن نخستین پیام:

```text
پاینده ایران
```

## 4.3 ساخت بازو بله

> نکته: شما باید با دادن برخی اطلاعات سطح کاربری را افزایش دهید.

### a

در اپلیکیشن بله، حساب `@botfather` را جستجو کرده و گفت‌وگو را آغاز کنید و دستور زیر را وارد نمایید:

```text
/start
```

### b

نام بازو را با فرمت زیر وارد کنید:

```text
alert_Team Name_devops_leauge
```

### c

نام کاربری را وارد کنید:

```text
Team Name_devops_league_bot
```

### d

پس از اینکه گام ها را درست پیش رفته باشید، یک سری داده ها به شما داده می‌شود که باید در جایی مطمئن نگهداری کنید.

### e

سپس روی دکمه عضویت بازو در گروه را بزنید و سپس دکمه فعال سازی را بزنید.

## 4.4 افزودن بازو به کانال

سپس به کانال ساخته شده (که در گام ۱ گفته شد) رفته و بازوی ساخته شده را عضو کنید و دسترسی ادمین بدهید.

نام بازو در گام 3.b گفته شده که در بخش افزودن عضو جدید با جستجو می‌توانید نام بازو را پیدا کنید.

## 4.5 آماده سازی کد پایتون

> چت آیدی توسط تیم پشتیبانی لیگ در اختیار شما قرار خواهد گرفت.

نمونه کد:

```python
import requests

TOKEN = ""

url = f"https://tapi.bale.ai/bot{TOKEN}/sendMessage"

payload = {
    "chat_id": "CHAT_ID",  # or channel ID bale .
    "text": "درود این نخستین پیام تیم با بازو بله است"
}

response = requests.post(url, json=payload)

print(response.status_code)
print(response.text)
```

---

## Template Alertmanager برای بله

> نکته: Template یا الگو Alertmanager برای فرستادن هشدارها به کانال بله بر این اساس می‌باشد.

```gotemplate
{{ define "bale.message" }}
Alert Notification

Alert: {{ .CommonLabels.alertname }}
Team: {{ .CommonLabels.team }}
Severity: {{ .CommonLabels.severity }}
Status: {{ .Status }}

{{ range .Alerts }}
Description:
{{ .Annotations.description }}

Instance:
{{ .Labels.instance }}

Started:
{{ .StartsAt }}
{{ end }}

{{ end }}
```
</div>