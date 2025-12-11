# 🔷 Portfolio Backend - Laravel (Gestor de Contenido)

<div align="center">

![PHP](https://img.shields.io/badge/PHP-8.2-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-12-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

**API REST para gestión de contenido estructurado del portfolio**

[📖 API Docs](#endpoints) • [🗄️ Modelos](#modelos) • [🚀 Deploy](#deploy)

</div>

---

## 📖 **Descripción**

Backend **Laravel** que actúa como "gestor de contenido estructurado". Maneja:
- 💼 **Proyectos** del portfolio con filtros avanzados
- 💻 **Experiencia laboral** (timeline)
- 🛠️ **Skills & Stack tecnológico**
- 🎓 **Certificaciones**
- 🏆 **Testimonios**

---

## 🏗️ **Arquitectura**

```
portfolio-backend-laravel/
├── app/
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── Api/
│   │   │   │   ├── ProjectController.php
│   │   │   │   ├── ExperienceController.php
│   │   │   │   ├── SkillController.php
│   │   │   │   └── TestimonialController.php
│   │   ├── Resources/
│   │   │   ├── ProjectResource.php
│   │   │   ├── ExperienceResource.php
│   │   │   └── SkillResource.php
│   │   └── Requests/
│   │       └── StoreProjectRequest.php
│   ├── Models/
│   │   ├── Project.php
│   │   ├── Experience.php
│   │   ├── Skill.php
│   │   └── Testimonial.php
│   └── Providers/
│       └── RouteServiceProvider.php
├── database/
│   ├── migrations/
│   │   ├── 2025_01_01_create_projects_table.php
│   │   ├── 2025_01_02_create_experiences_table.php
│   │   ├── 2025_01_03_create_skills_table.php
│   │   └── 2025_01_04_create_testimonials_table.php
│   ├── factories/
│   │   ├── ProjectFactory.php
│   │   └── ExperienceFactory.php
│   └── seeders/
│       ├── ProjectSeeder.php
│       ├── ExperienceSeeder.php
│       └── DatabaseSeeder.php
├── routes/
│   ├── api.php
│   └── web.php
├── tests/
│   ├── Feature/
│   │   ├── ProjectApiTest.php
│   │   └── ExperienceApiTest.php
│   └── Unit/
├── .env.example
├── Dockerfile
├── composer.json
└── README.md
```

---

## 🚀 **Quick Start**

### **Pre-requisitos**
```bash
PHP 8.2+
Composer
MySQL 8.0+ (o PostgreSQL)
Docker (opcional)
```

### **Instalación Local**

```bash
# Clonar el repo
git clone https://github.com/enlabedev/portfolio-backend-laravel.git
cd portfolio-backend-laravel

# Instalar dependencias
composer install

# Copiar .env
cp .env.example .env
php artisan key:generate

# Configurar base de datos en .env
# DB_CONNECTION=mysql
# DB_HOST=127.0.0.1
# DB_PORT=3306
# DB_DATABASE=portfolio_laravel
# DB_USERNAME=root
# DB_PASSWORD=

# Ejecutar migraciones
php artisan migrate

# Ejecutar seeders con datos de ejemplo
php artisan db:seed

# Levantar servidor
php artisan serve --port=8081
```

### **Con Docker**

```bash
# Build
docker build -t laravel-backend .

# Run
docker run -p 8081:8081 --env-file .env laravel-backend

# O con docker-compose (desde portfolio-infra)
docker-compose up laravel-backend
```

---


## 🔌 **Endpoints API**

### **Base URL**
```
http://localhost:8081/api
```

---

### **📊 Health Check**

```http
GET /api/health
```

**Response:**
```json
{
  "status": "healthy",
  "version": "1.0.0",
  "database": "connected"
}
```

---

### **💼 Projects Endpoints**

#### **GET /api/projects** - Listar proyectos

**Query Parameters:**
- `page` - Número de página (default: 1)
- `per_page` - Items por página (default: 15)
- `featured` - Filtrar por destacados (true/false)
- `stack` - Filtrar por tecnología (ej: `?stack=Python`)
- `sort` - Ordenar por campo (default: `-created_at`)

**Example Request:**
```http
GET /api/projects?featured=true&stack=Python&page=1
```

**Response:**
```json
{
  "data": [
    {
      "id": 1,
      "title": "Xofi.ai - Plataforma SaaS de IA",
      "slug": "xofi-ai",
      "description": "Plataforma SaaS para automatización con IA. Cofundador y CTO.",
      "stack": ["Python", "FastAPI", "Vue.js", "PostgreSQL", "AWS"],
      "role": "CTO & Co-founder",
      "impact": "Generación de $50K+ MRR en 6 meses. 200+ empresas activas.",
      "github_url": null,
      "demo_url": "https://xofi.ai",
      "image_url": "/images/projects/xofi.jpg",
      "is_featured": true,
      "created_at": "2025-01-15T10:00:00Z"
    }
  ],
  "links": {
    "first": "http://localhost:8081/api/projects?page=1",
    "last": "http://localhost:8081/api/projects?page=3",
    "prev": null,
    "next": "http://localhost:8081/api/projects?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 3,
    "per_page": 15,
    "to": 15,
    "total": 45
  }
}
```

---

#### **GET /api/projects/{slug}** - Detalle de proyecto

**Example Request:**
```http
GET /api/projects/xofi-ai
```

**Response:**
```json
{
  "data": {
    "id": 1,
    "title": "Xofi.ai - Plataforma SaaS de IA",
    "slug": "xofi-ai",
    "description": "Plataforma SaaS completa...",
    "stack": ["Python", "FastAPI", "Vue.js", "PostgreSQL", "AWS"],
    "role": "CTO & Co-founder",
    "impact": "Generación de $50K+ MRR en 6 meses...",
    "github_url": null,
    "demo_url": "https://xofi.ai",
    "image_url": "/images/projects/xofi.jpg",
    "is_featured": true,
    "created_at": "2025-01-15T10:00:00Z"
  }
}
```

**Error Response (404):**
```json
{
  "message": "Project not found"
}
```

---

## 🧪 **Testing**

### **Ejecutar Tests**

```bash
# Todos los tests
php artisan test

# Con coverage
php artisan test --coverage

# Tests específicos
php artisan test --filter ProjectApiTest

# Tests con output detallado
php artisan test --parallel
```
---

## 📚 **Recursos**

- [📖 Laravel Docs](https://laravel.com/docs)
- [🔌 API Resources](https://laravel.com/docs/eloquent-resources)
- [☁️ Cloud Run PHP](https://cloud.google.com/run/docs/quickstarts/build-and-deploy/deploy-php-service)

---

## 👨‍💻 **Autor**

**Enrique Lazo Bello** - Senior Software Engineer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/enlabe)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/enlabedev)

---

## 📄 **Licencia**

MIT License

---

<div align="center">

Made with 🔷 Laravel and ❤️

</div>
