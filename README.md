# 🛡️ alx-backend-security

This Django-based project implements backend-level security features to protect web applications by monitoring and restricting suspicious IP activity. It includes IP logging, blacklisting, geolocation tracking, rate limiting, and anomaly detection using middleware, scheduled tasks, and Django management commands.

---

## 📁 Project Structure

```bash
alx-backend-security/
├── ip_tracking/
│   ├── middleware.py
│   ├── models.py
│   ├── views.py
│   ├── tasks.py
│   └── management/
│       └── commands/
│           └── block_ip.py
├── settings.py
└── ...
```

---

## ✅ Tasks Overview

### 0. Basic IP Logging Middleware

**Objective:**
Log every incoming request's IP address, timestamp, and accessed path.

**Implementation:**

* Created `RequestLog` model in `ip_tracking/models.py`.
* Implemented middleware in `ip_tracking/middleware.py` to log:

  * IP address
  * Timestamp
  * Requested path
* Registered the middleware in `settings.py`.

---

### 1. IP Blacklisting

**Objective:**
Block traffic from specific blacklisted IPs.

**Implementation:**

* Added `BlockedIP` model in `ip_tracking/models.py`.
* Updated middleware to return HTTP 403 for blocked IPs.
* Created management command `block_ip`:

  ```bash
  python manage.py block_ip <ip_address>
  ```

---

### 2. IP Geolocation Analytics

**Objective:**
Log geolocation info (country, city) with each request.

**Implementation:**

* Installed `django-ipgeolocation`.
* Extended `RequestLog` model with `country` and `city` fields.
* Enhanced middleware to:

  * Fetch geolocation info using the API.
  * Cache IP location results for 24 hours.

---

### 3. Rate Limiting by IP

**Objective:**
Prevent abuse by limiting requests per IP.

**Implementation:**

* Installed `django-ratelimit`.
* Applied rate limits in `ip_tracking/views.py`:

  * 10 requests/minute for authenticated users.
  * 5 requests/minute for anonymous users.
* Configured rate limiting in `settings.py`.

---

### 4. Anomaly Detection

**Objective:**
Automatically detect and flag suspicious IP behavior.

**Implementation:**

* Created a periodic Celery task in `ip_tracking/tasks.py`:

  * Runs hourly.
  * Flags IPs exceeding 100 requests/hour or accessing paths like `/admin`, `/login`.
* Created `SuspiciousIP` model in `ip_tracking/models.py`.

---

## 🔧 Setup Instructions

1. **Clone the repository:**

   ```bash
   git clone https://github.com/YOUR_USERNAME/alx-backend-security.git
   cd alx-backend-security
   ```

2. **Install dependencies:**

   ```bash
   pip install -r requirements.txt
   ```

3. **Apply migrations:**

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

4. **Run the server:**

   ```bash
   python manage.py runserver
   ```

5. **(Optional) Start Celery for anomaly detection:**

   ```bash
   celery -A your_project_name worker -B
   ```

---

## 🧪 Testing Features

* Use tools like Postman or `curl` to send requests.
* Add IPs to the blacklist using:

  ```bash
  python manage.py block_ip 123.123.123.123
  ```
* Trigger rate limiting and check middleware logs.
* Check database for `RequestLog`, `BlockedIP`, and `SuspiciousIP` entries.

---

## 📈 Dependencies

* Django
* django-ipgeolocation
* django-ratelimit
* Celery
* Redis (recommended for Celery broker)

---

## 📄 License

This project is for **educational purposes** as part of the ALX Software Engineering curriculum.
