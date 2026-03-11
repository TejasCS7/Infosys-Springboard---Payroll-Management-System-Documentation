# 💼 Payroll Service Management System

> A robust, Django-powered backend system for streamlining payroll processing, leave management, and employee record-keeping — built during the **Infosys Internship 4.0** program.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [System Architecture](#-system-architecture)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Configuration](#environment-configuration)
  - [Running the App](#running-the-app)
- [API Documentation](#-api-documentation)
- [Project Structure](#-project-structure)
- [Testing](#-testing)
- [Deployment](#-deployment)
- [Known Issues & Solutions](#-known-issues--solutions)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🌟 Overview

The **Payroll Service Management System** is a web-based backend application designed to simplify and automate payroll operations for small to mid-sized organizations. It handles everything from employee onboarding to salary disbursement — all through a clean RESTful API.

### 🎯 Key Objectives

| Goal | Description |
|------|-------------|
| ⚡ Automation | Eliminate manual payroll processing to reduce time and human error |
| 🎯 Accuracy | Precise salary calculations with configurable deductions and allowances |
| 🔒 Security | Role-based access control with encrypted sensitive data |
| 📋 Compliance | Simplified tax and regulatory compliance reporting |
| 📬 Accessibility | Employees receive payslips and leave updates via email |

---

## ✨ Features

### 👥 User Management
- Secure registration and authentication for employees and HR admins
- Role-based access control (`Employee` / `HR`)
- Password reset functionality
- Comprehensive user profile management

### 🏢 Employee Management *(HR only)*
- Add, update, and terminate employee records
- Verify and approve new hire registrations
- Maintain detailed personal, professional, and contractual profiles

### 🏖️ Leave Management
- Employee self-service leave request submission
- HR approval / rejection workflow with centralized dashboard
- Automatic leave balance accrual and deduction
- Real-time email notifications on leave status changes

### 💰 Payroll Management
- Configurable compensation components (salary, allowances, deductions)
- Automated payroll calculations based on employee data and leave records
- Individual payslip generation and distribution via email
- Scheduled payroll processing using APScheduler (monthly/bi-weekly cycles)
- Full payroll history and reporting

---

## 🛠️ Tech Stack

### Backend
| Technology | Purpose |
|-----------|---------|
| ![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) | Core programming language |
| ![Django](https://img.shields.io/badge/Django-092E20?style=flat&logo=django&logoColor=white) | Web framework (MVT pattern) |
| **Django REST Framework** | RESTful API development |
| **drf-yasg** | Auto-generated Swagger/OpenAPI documentation |
| **APScheduler** | Background task scheduling (payroll cycles) |
| **django-cors-headers** | Cross-Origin Resource Sharing (CORS) handling |
| **Django Rest Auth** | Authentication & registration endpoints |

### Database
| Technology | Purpose |
|-----------|---------|
| ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat&logo=postgresql&logoColor=white) | Primary relational database |
| **pgAdmin** | Database administration and management |

### DevOps & Tools
| Technology | Purpose |
|-----------|---------|
| ![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white) | Version control |
| ![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat&logo=github-actions&logoColor=white) | CI/CD pipeline |
| ![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white) | Deployment (Elastic Beanstalk + S3 + CloudFront) |
| **Postman** | API testing and exploration |
| **Swagger UI** | Interactive API documentation |

---

## 🏗️ System Architecture

The project follows a **Monolithic Architecture** built on Django, with clear separation of concerns across four core modules:

```
┌─────────────────────────────────────────────────────────────┐
│                    Django Web Application                    │
│                                                             │
│  ┌─────────────┐  ┌──────────────┐  ┌───────────────────┐  │
│  │    User     │  │   Employee   │  │      Leave        │  │
│  │ Management  │  │  Management  │  │    Management     │  │
│  └─────────────┘  └──────────────┘  └───────────────────┘  │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              Payroll Management Module               │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
           │                              │
           ▼                              ▼
  ┌─────────────────┐           ┌──────────────────┐
  │   PostgreSQL DB  │           │  SMTP Email       │
  │  (Employee Data, │           │  Service (Gmail)  │
  │  Leave Records,  │           │  Notifications    │
  │  Payroll Info)   │           └──────────────────┘
  └─────────────────┘
```

### Design Patterns
- **MVT (Model-View-Template)** — Django's native architectural pattern
- **RESTful API Design** — Stateless, resource-based endpoints via DRF
- **ORM** — Django ORM abstracts PostgreSQL operations
- **Background Scheduling** — APScheduler for async payroll processing

---

## 🚀 Getting Started

### Prerequisites

Make sure you have the following installed:

- Python `3.8+`
- PostgreSQL `13+`
- pip
- Git

```bash
# Verify installations
python --version
pip --version
psql --version
```

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/your-username/payroll-service-management.git
cd payroll-service-management
```

**2. Create and activate a virtual environment**
```bash
# Create virtual environment
python -m venv env

# Activate — Windows
env\Scripts\activate.bat

# Activate — Mac/Linux
source env/bin/activate
```

**3. Install dependencies**
```bash
pip install --upgrade pip
pip install -r requirements.txt
```

**4. Set up PostgreSQL database**
```sql
-- In psql shell
CREATE DATABASE payroll_db;
CREATE USER payroll_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE payroll_db TO payroll_user;
```

### Environment Configuration

Create a `.env` file in the project root directory:

```env
# Django Settings
DJANGO_SETTINGS_MODULE=payroll.settings.development
DJANGO_APPLICATION_ENVIRONMENT=Development
SECRET_KEY=your-secret-key-here

# Database
DATABASE_URL=postgresql://payroll_user:your_password@localhost:5432/payroll_db

# Email (SMTP)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_HOST_USER=your-email@gmail.com
EMAIL_HOST_PASSWORD=your-app-password
EMAIL_USE_TLS=True

# CORS
CORS_ALLOWED_ORIGINS=http://localhost:3000
```

> ⚠️ **Never commit your `.env` file to version control.** It's already in `.gitignore`.

### Running the App

```bash
# Apply database migrations
python manage.py migrate

# Create a superuser (for admin access)
python manage.py createsuperuser

# Start the development server
python manage.py runserver
```

The app will be running at **http://127.0.0.1:8000/** 🎉

---

## 📖 API Documentation

Once the server is running, interactive API documentation is available at:

| Interface | URL |
|-----------|-----|
| **Swagger UI** | `http://127.0.0.1:8000/swagger/` |
| **ReDoc** | `http://127.0.0.1:8000/redoc/` |
| **Django Admin** | `http://127.0.0.1:8000/admin/` |

### Key API Endpoints

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| `POST` | `/api/auth/signup/` | Register a new user | Public |
| `POST` | `/api/auth/login/` | User login | Public |
| `GET` | `/api/employees/` | List all employees | HR only |
| `POST` | `/api/employees/` | Add a new employee | HR only |
| `GET` | `/api/leaves/` | View all leave requests | HR only |
| `POST` | `/api/leaves/apply/` | Submit a leave request | Employee |
| `PATCH` | `/api/leaves/{id}/status/` | Approve or reject leave | HR only |
| `GET` | `/api/payroll/` | View payroll records | HR only |
| `GET` | `/api/payroll/payslip/` | Get own payslip | Employee |
| `POST` | `/api/payroll/process/` | Trigger payroll run | HR only |

---

## 📁 Project Structure

```
payroll-service-management/
├── payroll/                    # Main Django project
│   ├── settings/
│   │   ├── base.py
│   │   ├── development.py
│   │   └── production.py
│   ├── urls.py
│   └── wsgi.py
│
├── users/                      # User Management Module
│   ├── models.py
│   ├── views.py
│   ├── serializers.py
│   └── urls.py
│
├── employees/                  # Employee Management Module
│   ├── models.py
│   ├── views.py
│   ├── serializers.py
│   └── urls.py
│
├── leaves/                     # Leave Management Module
│   ├── models.py
│   ├── views.py
│   ├── serializers.py
│   └── urls.py
│
├── payroll_mgmt/               # Payroll Management Module
│   ├── models.py
│   ├── views.py
│   ├── serializers.py
│   ├── scheduler.py            # APScheduler config
│   └── urls.py
│
├── notifications/              # Email Notification Service
│   └── email_service.py
│
├── tests/                      # Test suites
│   ├── test_models.py
│   ├── test_views.py
│   └── test_serializers.py
│
├── requirements.txt
├── manage.py
├── .env.example
└── README.md
```

---

## 🧪 Testing

The project maintains **80%+ code coverage** across unit, integration, and system tests.

```bash
# Run all tests
python manage.py test

# Run tests for a specific app
python manage.py test employees
python manage.py test leaves
python manage.py test payroll_mgmt

# Run with coverage report
pip install coverage
coverage run manage.py test
coverage report
coverage html  # Generates htmlcov/index.html
```

### Testing Strategy

| Test Type | Tools Used | Coverage |
|-----------|-----------|---------|
| **Unit Tests** | `unittest`, `pytest` | Models, Views, Serializers, Utilities |
| **Integration Tests** | `Postman`, Django `TestCase` | API endpoints, module interactions |
| **Performance Tests** | `Locust`, `Apache JMeter` | Load testing, bottleneck detection |
| **Security Tests** | Manual + automated | SQLi, XSS, CSRF vulnerability checks |

---

## 🚢 Deployment

The application is deployed on **AWS** using the following infrastructure:

```
GitHub (push) → GitHub Actions CI/CD
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
  Backend (Django)         Frontend (Static)
  AWS Elastic Beanstalk    AWS S3 + CloudFront
```

### Deploy to Production

```bash
# 1. Set production environment variables
export DJANGO_APPLICATION_ENVIRONMENT=Production

# 2. Collect static files
python manage.py collectstatic --no-input

# 3. Apply migrations
python manage.py migrate

# 4. Start production server with Gunicorn
gunicorn payroll.wsgi:application --bind 0.0.0.0:8000
```

---

## 🐛 Known Issues & Solutions

| Issue | Solution |
|-------|----------|
| **CORS errors** between frontend/backend on different ports | Configured `django-cors-headers` with allowed origins |
| **SSL certification error** on HTTPS | Properly configured SSL/TLS certificates, verified correct HTTPS ports |
| **APScheduler conflicts** with Django dev server (dual process) | Set `scheduler.start()` inside `AppConfig.ready()` with `--noreload` flag |
| **Git push failures** due to large files | Increased Git buffer size; used Git LFS for binary files |
| **TypeError in serializers** | Added explicit type checks and data validation in serializer `validate()` methods |

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/your-feature-name`
3. **Commit** your changes: `git commit -m "feat: add your feature"`
4. **Push** to the branch: `git push origin feature/your-feature-name`
5. **Open** a Pull Request

### Commit Convention
Follow [Conventional Commits](https://www.conventionalcommits.org/):
- `feat:` — New feature
- `fix:` — Bug fix
- `docs:` — Documentation change
- `test:` — Adding or updating tests
- `chore:` — Maintenance tasks

---

## 👤 Author

**Tejas Gaikawad**
- 🏢 Infosys Internship 4.0
- 📧 [your-email@example.com]
- 🔗 [LinkedIn](https://linkedin.com/in/your-profile)
- 🐙 [GitHub](https://github.com/your-username)

---

## 📜 License

This project was developed as part of the **Infosys Internship 4.0** program. All rights reserved.

---

<div align="center">

Made with ❤️ during Infosys Internship 4.0

⭐ Star this repo if you found it helpful!

</div>
