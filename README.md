# Farm-management-system-FMS
# Farm Management System

A role-based web application for managing farms, crops, livestock, inventory, employees, expenses, sales, and weather records. Built with PHP 8 MVC (no framework), MySQL, and Bootstrap 5.

---

## Requirements

| Requirement | Version |
|-------------|---------|
| PHP         | 8.0+    |
| MySQL       | 8.0+    |
| Apache      | 2.4+ with `mod_rewrite` enabled |
| Composer    | 2.x     |

---

## Installation

### 1. Copy files to your server
Clone or copy the project into your Apache document root (e.g. `C:/wamp64/www/`).

### 2. Configure Apache DocumentRoot
Point your virtual host `DocumentRoot` to the `public/` subdirectory:

```apache
<VirtualHost *:80>
    ServerName farm.local
    DocumentRoot "C:/wamp64/www/farm-management/public"
    <Directory "C:/wamp64/www/farm-management/public">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

Ensure `mod_rewrite` is enabled. The included `public/.htaccess` routes all requests through the front controller.

### 3. Create the MySQL database

```sql
CREATE DATABASE farm_management CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 4. Import schema and seed data

```bash
mysql -u root -p farm_management < database/schema.sql
mysql -u root -p farm_management < database/seeds.sql
```

### 5. Configure database credentials
Edit `config/database.php` and update the connection details:

```php
return [
    'host'     => '127.0.0.1',
    'dbname'   => 'farm_management',
    'username' => 'your_db_user',
    'password' => 'your_db_password',
    'charset'  => 'utf8mb4',
];
```

### 6. Install PHP dependencies

```bash
composer install
```

This installs `dompdf/dompdf` (used for PDF report generation).

### 7. Visit the application
Open your browser and navigate to:

```
http://localhost/
```

### 8. Default admin credentials

| Field    | Value            |
|----------|------------------|
| Email    | admin@farm.com   |
| Password | Admin@1234       |

> **Important:** Change the default admin password immediately after first login.

---

## Modules

| Module             | Role    | Description                                          |
|--------------------|---------|------------------------------------------------------|
| Dashboard          | Both    | Summary stats, recent activity, quick links          |
| Users              | Admin   | Create, edit, activate/deactivate user accounts      |
| Farms              | Farmer  | Register and manage farms (ownership-enforced)       |
| Crop Categories    | Admin   | Manage crop category reference data                  |
| Crops              | Farmer  | Track planted crops, growth status, expected harvest |
| Harvests           | Farmer  | Log harvest records per crop                         |
| Livestock Types    | Admin   | Manage livestock type reference data                 |
| Livestock          | Farmer  | Track livestock by type, quantity, and health        |
| Inventory          | Farmer  | Track farm supplies with expiry alerts               |
| Inventory Categories | Admin | Manage inventory category reference data             |
| Suppliers          | Admin   | Manage supplier directory                            |
| Employees          | Farmer  | Manage farm workers and payroll records              |
| Expenses           | Farmer  | Log and categorise farm expenses                     |
| Customers          | Farmer  | Manage buyer contacts                                |
| Sales              | Farmer  | Record sales transactions with line items            |
| Weather Records    | Farmer  | Log daily weather data per farm                      |
| Notifications      | Both    | In-app notifications; admin can broadcast messages   |
| Reports            | Both    | Generate and export PDF / HTML farm reports          |
| Settings           | Admin   | System settings and database backup                  |
| Profile            | Farmer  | Update profile and change password                   |

---

## Security

- **Authentication** — session-based with 30-minute inactivity timeout and password-change invalidation.
- **Passwords** — bcrypt (cost 12) via `password_hash()` / `password_verify()`.
- **CSRF protection** — token-per-session checked on every state-changing POST request.
- **Role-Based Access Control (RBAC)** — `RoleMiddleware` enforces Admin / Farmer separation on every protected route.
- **Ownership checks** — `OwnershipMiddleware` prevents Farmers from modifying other users' farm records.
- **Input validation** — `Validator` class performs server-side validation before any database write.
- **Prepared statements** — all database queries use PDO prepared statements; no raw user input is interpolated into SQL.
- **No directory listing** — `Options -Indexes` set in `.htaccess`.

---

## Database Schema

The schema (`database/schema.sql`) defines 19 InnoDB tables:

`users`, `farms`, `crop_categories`, `crops`, `livestock_types`, `livestock`,
`inventory_categories`, `inventory`, `suppliers`, `employees`, `expenses`,
`customers`, `sales`, `sale_items`, `harvest`, `weather_records`,
`notifications`, `settings`, `backup_log`

All foreign keys use `ON DELETE CASCADE` where appropriate. Charset is `utf8mb4` throughout.

---

## Project Structure

```
├── app/
│   ├── Controllers/        # Admin/ and Farmer/ subdirectories + shared
│   ├── Core/               # Router, Database, Session, Validator, etc.
│   ├── Middleware/         # Auth, Role, CSRF, Ownership
│   ├── Models/             # One model per database table
│   └── Views/              # PHP view templates + layouts
├── config/
│   ├── app.php             # Base URL, timezone, etc.
│   ├── database.php        # DB credentials
│   └── routes.php          # All 105 route definitions
├── database/
│   ├── schema.sql          # DDL for all 19 tables
│   └── seeds.sql           # Default admin user + reference data
├── public/                 # Web root (DocumentRoot)
│   ├── assets/css/
│   ├── assets/js/
│   ├── .htaccess
│   └── index.php           # Front controller
├── vendor/                 # Composer dependencies
└── composer.json
```

Import 
schema.sql
 then 
seeds.sql
 into MySQL
Set Apache DocumentRoot to the public/ folder
Run composer install (if not already done)
Visit http://localhost/ and log in with admin@farm.com / Admin@1234
