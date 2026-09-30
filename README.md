# 📚 Tugas Laravel: Sistem Manajemen Akademik (SIAKAD)

## Deskripsi Proyek

Membangun **Sistem Informasi Akademik (SIAKAD)** menggunakan Laravel dengan fitur lengkap meliputi:
- CRUD data menggunakan **Eloquent ORM**
- **Autentikasi** (Login & Logout)
- **Middleware** untuk proteksi route
- **Gate & Policy** berdasarkan role (**Admin** dan **User**)
- Relasi antar tabel: **One-to-One**, **One-to-Many**, dan **Many-to-Many**

---

## 📐 Perancangan Database (ERD)

### Daftar 18 Tabel (16 Tabel Utama + 2 Tabel Pivot)

| No | Nama Tabel               | Keterangan                              |
|----|---------------------------|-----------------------------------------|
| 1  | `users`                   | Data pengguna & autentikasi             |
| 2  | `profiles`                | Profil detail pengguna                  |
| 3  | `departments`             | Data jurusan/departemen                 |
| 4  | `teachers`                | Data dosen                              |
| 5  | `students`                | Data mahasiswa                          |
| 6  | `categories`              | Kategori mata kuliah                    |
| 7  | `courses`                 | Data mata kuliah                        |
| 8  | `classrooms`              | Data ruang kelas                        |
| 9  | `schedules`               | Jadwal perkuliahan                      |
| 10 | `enrollments`             | Pendaftaran mata kuliah (pivot)         |
| 11 | `assignments`             | Data tugas/penugasan                    |
| 12 | `submissions`             | Pengumpulan tugas oleh mahasiswa        |
| 13 | `grades`                  | Data nilai mahasiswa                    |
| 14 | `announcements`           | Pengumuman                              |
| 15 | `tags`                    | Tag untuk mata kuliah                   |
| 16 | `course_tag`              | Pivot tabel relasi courses & tags       |
| 17 | `extracurriculars`        | Data kegiatan ekstrakurikuler           |
| 18 | `extracurricular_student` | Pivot tabel relasi students & extracurriculars |

---

### Diagram Relasi (ERD)

```
┌──────────────┐       1:1       ┌──────────────┐
│    users     │───────────────▶│   profiles    │
│              │                 │              │
│ id           │       1:1       │ id           │
│ name         │──────┐          │ user_id (FK) │
│ email        │      │          │ phone        │
│ password     │      │          │ address      │
│ role         │      │          │ avatar       │
│ timestamps   │      │          │ birth_date   │
└──────┬───────┘      │          └──────────────┘
       │              │
       │ 1:M          │ 1:1
       ▼              ▼
┌──────────────┐  ┌──────────────┐     ┌──────────────┐
│announcements │  │   teachers   │───▶│ departments  │
│              │  │              │ M:1 │              │
│ id           │  │ id           │     │ id           │
│ user_id (FK) │  │ user_id (FK) │     │ name         │
│ title        │  │ department_id│     │ code         │
│ content      │  │ nip          │     │ description  │
│ is_published │  │ specialization│    │ timestamps   │
│ timestamps   │  │ timestamps   │     └──────┬───────┘
└──────────────┘  └──────┬───────┘            │
                         │                     │ 1:M
                         │ 1:M                 │
                         ▼                     ▼
                  ┌──────────────┐     ┌──────────────┐
                  │  schedules   │     │   students   │
                  │              │     │              │
                  │ id           │     │ id           │
                  │ course_id(FK)│     │ user_id (FK) │ ◀── 1:1 dari users
                  │ teacher_id(FK)│    │ department_id│
                  │ classroom_id │     │ nim          │
                  │ day          │     │ semester     │
                  │ start_time   │     │ timestamps   │
                  │ end_time     │     └──────┬───────┘
                  │ timestamps   │            │
                  └──────────────┘            │
                         ▲                    │ M:M (via enrollments)
                         │ M:1                │
                  ┌──────────────┐            │
                  │  classrooms  │            │
                  │              │            │
                  │ id           │            │
                  │ name         │            ▼
                  │ building     │     ┌──────────────┐
                  │ capacity     │     │ enrollments  │ (Pivot Table)
                  │ timestamps   │     │              │
                  └──────────────┘     │ id           │
                                       │ student_id   │
┌──────────────┐       1:M             │ course_id    │
│  categories  │──────────┐            │ academic_year│
│              │          │            │ semester     │
│ id           │          ▼            │ status       │
│ name         │   ┌──────────────┐    │ timestamps   │
│ description  │   │   courses    │    └──────────────┘
│ timestamps   │   │              │
└──────────────┘   │ id           │         M:M (via course_tag)
                   │ category_id  │──────────────────┐
                   │ department_id│                  │
┌──────────────┐   │ code         │         ┌────────▼───────┐
│    tags      │   │ name         │         │   course_tag   │ (Pivot)
│              │   │ credits      │         │                │
│ id           │   │ description  │         │ course_id (FK) │
│ name         │◀──│ timestamps   │         │ tag_id (FK)    │
│ slug         │   └──────┬───────┘         │ timestamps     │
│ timestamps   │          │                 └────────────────┘
└──────────────┘          │ 1:M
                          ▼
                   ┌──────────────┐       1:M       ┌──────────────┐
                   │ assignments  │───────────────▶│ submissions  │
                   │              │                 │              │
                   │ id           │                 │ id           │
                   │ course_id(FK)│                 │ assignment_id│
                   │ title        │                 │ student_id   │
                   │ description  │                 │ file_path    │
                   │ due_date     │                 │ notes        │
                   │ timestamps   │                 │ submitted_at │
                   └──────────────┘                 │ score        │
                                                    │ timestamps   │
                   ┌──────────────┐                 └──────────────┘
                   │    grades    │
                   │              │
                   │ id           │
                   │ student_id   │ (FK)
                   │ course_id    │ (FK)
                   │ academic_year│
                   │ midterm_score│
                   │ final_score  │
                   │ grade_letter │
                   │ timestamps   │
                   └──────────────┘

┌────────────────────┐         M:M          ┌────────────────────────┐
│ extracurriculars   │◀───────────────────▶│ extracurricular_student│ (Pivot)
│                    │                      │                        │
│ id                 │                      │ extracurricular_id(FK) │
│ name               │                      │ student_id (FK)        │
│ description        │                      │ joined_at              │
│ max_members        │                      │ role                   │
│ timestamps         │                      │ timestamps             │
└────────────────────┘                      └────────────────────────┘
```

---

### Ringkasan Relasi

#### 🔹 One-to-One (1:1)
| Tabel A     | Tabel B    | Keterangan                           |
|-------------|------------|--------------------------------------|
| `users`     | `profiles` | Setiap user memiliki satu profil     |
| `users`     | `teachers` | Setiap dosen terhubung satu user     |
| `users`     | `students` | Setiap mahasiswa terhubung satu user |

#### 🔸 One-to-Many (1:M)
| Tabel Parent       | Tabel Child      | Keterangan                                    |
|--------------------|------------------|-----------------------------------------------|
| `departments`      | `teachers`       | Satu jurusan memiliki banyak dosen            |
| `departments`      | `students`       | Satu jurusan memiliki banyak mahasiswa        |
| `departments`      | `courses`        | Satu jurusan memiliki banyak mata kuliah      |
| `categories`       | `courses`        | Satu kategori memiliki banyak mata kuliah     |
| `courses`          | `schedules`      | Satu mata kuliah memiliki banyak jadwal       |
| `courses`          | `assignments`    | Satu mata kuliah memiliki banyak tugas        |
| `teachers`         | `schedules`      | Satu dosen memiliki banyak jadwal mengajar    |
| `classrooms`       | `schedules`      | Satu ruangan memiliki banyak jadwal           |
| `assignments`      | `submissions`    | Satu tugas memiliki banyak pengumpulan        |
| `users`            | `announcements`  | Satu user membuat banyak pengumuman           |

#### 🔻 Many-to-Many (M:M)
| Tabel A            | Tabel B              | Tabel Pivot                  | Keterangan                        |
|--------------------|----------------------|------------------------------|-----------------------------------|
| `students`         | `courses`            | `enrollments`                | Mahasiswa mendaftar mata kuliah   |
| `courses`          | `tags`               | `course_tag`                 | Mata kuliah memiliki banyak tag   |
| `students`         | `extracurriculars`   | `extracurricular_student`    | Mahasiswa mengikuti ekstrakurikuler|

---

## 🔐 Sistem Autentikasi & Autorisasi

### Role Pengguna

| Role    | Deskripsi                                                                |
|---------|--------------------------------------------------------------------------|
| `admin` | Dapat mengakses semua fitur CRUD pada semua modul                        |
| `user`  | Hanya dapat melihat (Read) data, tidak bisa Create, Update, atau Delete  |

### Fitur Autentikasi
- **Login**: Form login dengan email & password
- **Logout**: Tombol logout di navbar
- **Middleware `auth`**: Memproteksi semua halaman agar hanya bisa diakses jika sudah login
- **Gate `admin`**: Membatasi akses Create, Update, Delete hanya untuk role `admin`

---

## 🛠️ Perancangan Fitur CRUD per Modul

### Daftar Modul CRUD

| No | Modul                 | Controller                   | Route Prefix         |
|----|-----------------------|------------------------------|----------------------|
| 1  | Users                 | `UserController`             | `/users`             |
| 2  | Profiles              | `ProfileController`          | `/profiles`          |
| 3  | Departments           | `DepartmentController`       | `/departments`       |
| 4  | Teachers              | `TeacherController`          | `/teachers`          |
| 5  | Students              | `StudentController`          | `/students`          |
| 6  | Categories            | `CategoryController`         | `/categories`        |
| 7  | Courses               | `CourseController`           | `/courses`           |
| 8  | Classrooms            | `ClassroomController`        | `/classrooms`        |
| 9  | Schedules             | `ScheduleController`         | `/schedules`         |
| 10 | Enrollments           | `EnrollmentController`       | `/enrollments`       |
| 11 | Assignments           | `AssignmentController`       | `/assignments`       |
| 12 | Submissions           | `SubmissionController`       | `/submissions`       |
| 13 | Grades                | `GradeController`            | `/grades`            |
| 14 | Announcements         | `AnnouncementController`     | `/announcements`     |
| 15 | Tags                  | `TagController`              | `/tags`              |
| 16 | Extracurriculars      | `ExtracurricularController`  | `/extracurriculars`  |

---

# 📋 LANGKAH-LANGKAH PENGERJAAN

---

## TAHAP 1: Install Laravel Project

### 1.1 Prasyarat
Pastikan sudah terinstall:
- **PHP** >= 8.2
- **Composer**
- **Node.js** & **NPM**
- **MySQL** / **MariaDB**
- **Git**

### 1.2 Buat Project Laravel Baru

```bash
laravel new siakad-laravel
cd siakad-laravel
```

### 1.3 Jalankan Server Development

```bash
composer run dev
```

Buka browser dan akses `http://localhost:8000` untuk memastikan instalasi berhasil.

---

## TAHAP 2: Buat Database & Sambungkan ke Project

### 2.1 Buat Database Baru

Masuk ke MySQL dan buat database:

```sql
CREATE DATABASE siakad_laravel;
```

### 2.2 Konfigurasi File `.env`

Buka file `.env` di root project dan ubah konfigurasi database:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=siakad_laravel
DB_USERNAME=root
DB_PASSWORD=
```

### 2.3 Test Koneksi

```bash
php artisan migrate
```

Jika berhasil tanpa error, koneksi database sudah terhubung.

---

## TAHAP 3: Buat Migration

Buat file migration untuk setiap tabel sesuai urutan dependensi (tabel yang tidak bergantung pada tabel lain dibuat terlebih dahulu).

### 3.1 Modifikasi Migration `users` (sudah ada)

File: `database/migrations/0001_01_01_000000_create_users_table.php`

Modifikasi method `up()`:

```php
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('email')->unique();
    $table->timestamp('email_verified_at')->nullable();
    $table->string('password');
    $table->enum('role', ['admin', 'user'])->default('user');
    $table->rememberToken();
    $table->timestamps();
});
```

> **Catatan:** Tambahkan kolom `role` dengan tipe `enum` yang berisi nilai `admin` dan `user`, dengan default `user`.

### 3.2 Migration `profiles`

```bash
php artisan make:migration create_profiles_table
```

```php
Schema::create('profiles', function (Blueprint $table) {
    $table->id();
    $table->foreignId('user_id')->constrained()->onDelete('cascade');
    $table->string('phone', 20)->nullable();
    $table->text('address')->nullable();
    $table->string('avatar')->nullable();
    $table->date('birth_date')->nullable();
    $table->timestamps();
});
```

### 3.3 Migration `departments`

```bash
php artisan make:migration create_departments_table
```

```php
Schema::create('departments', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('code', 10)->unique();
    $table->text('description')->nullable();
    $table->timestamps();
});
```

### 3.4 Migration `teachers`

```bash
php artisan make:migration create_teachers_table
```

```php
Schema::create('teachers', function (Blueprint $table) {
    $table->id();
    $table->foreignId('user_id')->constrained()->onDelete('cascade');
    $table->foreignId('department_id')->constrained()->onDelete('cascade');
    $table->string('nip', 30)->unique();
    $table->string('specialization')->nullable();
    $table->timestamps();
});
```

### 3.5 Migration `students`

```bash
php artisan make:migration create_students_table
```

```php
Schema::create('students', function (Blueprint $table) {
    $table->id();
    $table->foreignId('user_id')->constrained()->onDelete('cascade');
    $table->foreignId('department_id')->constrained()->onDelete('cascade');
    $table->string('nim', 20)->unique();
    $table->integer('semester')->default(1);
    $table->timestamps();
});
```

### 3.6 Migration `categories`

```bash
php artisan make:migration create_categories_table
```

```php
Schema::create('categories', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->text('description')->nullable();
    $table->timestamps();
});
```

### 3.7 Migration `courses`

```bash
php artisan make:migration create_courses_table
```

```php
Schema::create('courses', function (Blueprint $table) {
    $table->id();
    $table->foreignId('category_id')->constrained()->onDelete('cascade');
    $table->foreignId('department_id')->constrained()->onDelete('cascade');
    $table->string('code', 10)->unique();
    $table->string('name');
    $table->integer('credits')->default(2);
    $table->text('description')->nullable();
    $table->timestamps();
});
```

### 3.8 Migration `classrooms`

```bash
php artisan make:migration create_classrooms_table
```

```php
Schema::create('classrooms', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('building');
    $table->integer('capacity')->default(30);
    $table->timestamps();
});
```

### 3.9 Migration `schedules`

```bash
php artisan make:migration create_schedules_table
```

```php
Schema::create('schedules', function (Blueprint $table) {
    $table->id();
    $table->foreignId('course_id')->constrained()->onDelete('cascade');
    $table->foreignId('teacher_id')->constrained()->onDelete('cascade');
    $table->foreignId('classroom_id')->constrained()->onDelete('cascade');
    $table->enum('day', ['Senin', 'Selasa', 'Rabu', 'Kamis', 'Jumat', 'Sabtu']);
    $table->time('start_time');
    $table->time('end_time');
    $table->timestamps();
});
```

### 3.10 Migration `enrollments` (Tabel Pivot: Students ↔ Courses)

```bash
php artisan make:migration create_enrollments_table
```

```php
Schema::create('enrollments', function (Blueprint $table) {
    $table->id();
    $table->foreignId('student_id')->constrained()->onDelete('cascade');
    $table->foreignId('course_id')->constrained()->onDelete('cascade');
    $table->string('academic_year', 9); // contoh: "2025/2026"
    $table->enum('semester', ['Ganjil', 'Genap']);
    $table->enum('status', ['active', 'dropped', 'completed'])->default('active');
    $table->timestamps();
});
```

### 3.11 Migration `assignments`

```bash
php artisan make:migration create_assignments_table
```

```php
Schema::create('assignments', function (Blueprint $table) {
    $table->id();
    $table->foreignId('course_id')->constrained()->onDelete('cascade');
    $table->string('title');
    $table->text('description')->nullable();
    $table->dateTime('due_date');
    $table->timestamps();
});
```

### 3.12 Migration `submissions`

```bash
php artisan make:migration create_submissions_table
```

```php
Schema::create('submissions', function (Blueprint $table) {
    $table->id();
    $table->foreignId('assignment_id')->constrained()->onDelete('cascade');
    $table->foreignId('student_id')->constrained()->onDelete('cascade');
    $table->string('file_path')->nullable();
    $table->text('notes')->nullable();
    $table->dateTime('submitted_at')->nullable();
    $table->decimal('score', 5, 2)->nullable();
    $table->timestamps();
});
```

### 3.13 Migration `grades`

```bash
php artisan make:migration create_grades_table
```

```php
Schema::create('grades', function (Blueprint $table) {
    $table->id();
    $table->foreignId('student_id')->constrained()->onDelete('cascade');
    $table->foreignId('course_id')->constrained()->onDelete('cascade');
    $table->string('academic_year', 9);
    $table->decimal('midterm_score', 5, 2)->nullable();
    $table->decimal('final_score', 5, 2)->nullable();
    $table->string('grade_letter', 2)->nullable(); // A, AB, B, BC, C, D, E
    $table->timestamps();
});
```

### 3.14 Migration `announcements`

```bash
php artisan make:migration create_announcements_table
```

```php
Schema::create('announcements', function (Blueprint $table) {
    $table->id();
    $table->foreignId('user_id')->constrained()->onDelete('cascade');
    $table->string('title');
    $table->text('content');
    $table->boolean('is_published')->default(false);
    $table->timestamps();
});
```

### 3.15 Migration `tags`

```bash
php artisan make:migration create_tags_table
```

```php
Schema::create('tags', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('slug')->unique();
    $table->timestamps();
});
```

### 3.16 Migration `course_tag` (Tabel Pivot: Courses ↔ Tags)

```bash
php artisan make:migration create_course_tag_table
```

```php
Schema::create('course_tag', function (Blueprint $table) {
    $table->foreignId('course_id')->constrained()->onDelete('cascade');
    $table->foreignId('tag_id')->constrained()->onDelete('cascade');
    $table->primary(['course_id', 'tag_id']);
    $table->timestamps();
});
```

### 3.17 Migration `extracurriculars`

```bash
php artisan make:migration create_extracurriculars_table
```

```php
Schema::create('extracurriculars', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->text('description')->nullable();
    $table->integer('max_members')->default(50);
    $table->timestamps();
});
```

### 3.18 Migration `extracurricular_student` (Tabel Pivot: Students ↔ Extracurriculars)

```bash
php artisan make:migration create_extracurricular_student_table
```

```php
Schema::create('extracurricular_student', function (Blueprint $table) {
    $table->foreignId('extracurricular_id')->constrained()->onDelete('cascade');
    $table->foreignId('student_id')->constrained()->onDelete('cascade');
    $table->primary(['extracurricular_id', 'student_id']);
    $table->date('joined_at')->nullable();
    $table->string('role')->default('member'); // member, leader, secretary
    $table->timestamps();
});
```

### 3.19 Jalankan Semua Migration

```bash
php artisan migrate
```

---

## TAHAP 4: Buat Model

Buat model untuk setiap tabel beserta relasi Eloquent-nya.

### 4.1 Model `User` (modifikasi model bawaan)

File: `app/Models/User.php`

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;

class User extends Authenticatable
{
    use HasFactory, Notifiable;

    protected $fillable = [
        'name',
        'email',
        'password',
        'role',
    ];

    protected $hidden = [
        'password',
        'remember_token',
    ];

    protected function casts(): array
    {
        return [
            'email_verified_at' => 'datetime',
            'password' => 'hashed',
        ];
    }

    // ===== RELASI =====

    // One-to-One: User memiliki satu Profile
    public function profile()
    {
        return $this->hasOne(Profile::class);
    }

    // One-to-One: User memiliki satu data Teacher
    public function teacher()
    {
        return $this->hasOne(Teacher::class);
    }

    // One-to-One: User memiliki satu data Student
    public function student()
    {
        return $this->hasOne(Student::class);
    }

    // One-to-Many: User membuat banyak Announcement
    public function announcements()
    {
        return $this->hasMany(Announcement::class);
    }

    // Helper method untuk cek role
    public function isAdmin(): bool
    {
        return $this->role === 'admin';
    }
}
```

### 4.2 Model `Profile`

```bash
php artisan make:model Profile
```

File: `app/Models/Profile.php`

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Profile extends Model
{
    use HasFactory;

    protected $fillable = [
        'user_id',
        'phone',
        'address',
        'avatar',
        'birth_date',
    ];

    protected function casts(): array
    {
        return [
            'birth_date' => 'date',
        ];
    }

    // One-to-One (inverse): Profile milik satu User
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```

### 4.3 Model `Department`

```bash
php artisan make:model Department
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Department extends Model
{
    use HasFactory;

    protected $fillable = [
        'name',
        'code',
        'description',
    ];

    // One-to-Many: Department memiliki banyak Teacher
    public function teachers()
    {
        return $this->hasMany(Teacher::class);
    }

    // One-to-Many: Department memiliki banyak Student
    public function students()
    {
        return $this->hasMany(Student::class);
    }

    // One-to-Many: Department memiliki banyak Course
    public function courses()
    {
        return $this->hasMany(Course::class);
    }
}
```

### 4.4 Model `Teacher`

```bash
php artisan make:model Teacher
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Teacher extends Model
{
    use HasFactory;

    protected $fillable = [
        'user_id',
        'department_id',
        'nip',
        'specialization',
    ];

    // One-to-One (inverse): Teacher milik satu User
    public function user()
    {
        return $this->belongsTo(User::class);
    }

    // Many-to-One: Teacher milik satu Department
    public function department()
    {
        return $this->belongsTo(Department::class);
    }

    // One-to-Many: Teacher memiliki banyak Schedule
    public function schedules()
    {
        return $this->hasMany(Schedule::class);
    }
}
```

### 4.5 Model `Student`

```bash
php artisan make:model Student
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Student extends Model
{
    use HasFactory;

    protected $fillable = [
        'user_id',
        'department_id',
        'nim',
        'semester',
    ];

    // One-to-One (inverse): Student milik satu User
    public function user()
    {
        return $this->belongsTo(User::class);
    }

    // Many-to-One: Student milik satu Department
    public function department()
    {
        return $this->belongsTo(Department::class);
    }

    // Many-to-Many: Student terdaftar di banyak Course (via enrollments)
    public function courses()
    {
        return $this->belongsToMany(Course::class, 'enrollments')
                    ->withPivot('academic_year', 'semester', 'status')
                    ->withTimestamps();
    }

    // One-to-Many: Student memiliki banyak Submission
    public function submissions()
    {
        return $this->hasMany(Submission::class);
    }

    // One-to-Many: Student memiliki banyak Grade
    public function grades()
    {
        return $this->hasMany(Grade::class);
    }

    // Many-to-Many: Student mengikuti banyak Extracurricular
    public function extracurriculars()
    {
        return $this->belongsToMany(Extracurricular::class, 'extracurricular_student')
                    ->withPivot('joined_at', 'role')
                    ->withTimestamps();
    }
}
```

### 4.6 Model `Category`

```bash
php artisan make:model Category
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Category extends Model
{
    use HasFactory;

    protected $fillable = [
        'name',
        'description',
    ];

    // One-to-Many: Category memiliki banyak Course
    public function courses()
    {
        return $this->hasMany(Course::class);
    }
}
```

### 4.7 Model `Course`

```bash
php artisan make:model Course
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Course extends Model
{
    use HasFactory;

    protected $fillable = [
        'category_id',
        'department_id',
        'code',
        'name',
        'credits',
        'description',
    ];

    // Many-to-One: Course milik satu Category
    public function category()
    {
        return $this->belongsTo(Category::class);
    }

    // Many-to-One: Course milik satu Department
    public function department()
    {
        return $this->belongsTo(Department::class);
    }

    // One-to-Many: Course memiliki banyak Schedule
    public function schedules()
    {
        return $this->hasMany(Schedule::class);
    }

    // One-to-Many: Course memiliki banyak Assignment
    public function assignments()
    {
        return $this->hasMany(Assignment::class);
    }

    // One-to-Many: Course memiliki banyak Grade
    public function grades()
    {
        return $this->hasMany(Grade::class);
    }

    // Many-to-Many: Course memiliki banyak Student (via enrollments)
    public function students()
    {
        return $this->belongsToMany(Student::class, 'enrollments')
                    ->withPivot('academic_year', 'semester', 'status')
                    ->withTimestamps();
    }

    // Many-to-Many: Course memiliki banyak Tag
    public function tags()
    {
        return $this->belongsToMany(Tag::class, 'course_tag')
                    ->withTimestamps();
    }
}
```

### 4.8 Model `Classroom`

```bash
php artisan make:model Classroom
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Classroom extends Model
{
    use HasFactory;

    protected $fillable = [
        'name',
        'building',
        'capacity',
    ];

    // One-to-Many: Classroom memiliki banyak Schedule
    public function schedules()
    {
        return $this->hasMany(Schedule::class);
    }
}
```

### 4.9 Model `Schedule`

```bash
php artisan make:model Schedule
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Schedule extends Model
{
    use HasFactory;

    protected $fillable = [
        'course_id',
        'teacher_id',
        'classroom_id',
        'day',
        'start_time',
        'end_time',
    ];

    // Many-to-One: Schedule milik satu Course
    public function course()
    {
        return $this->belongsTo(Course::class);
    }

    // Many-to-One: Schedule milik satu Teacher
    public function teacher()
    {
        return $this->belongsTo(Teacher::class);
    }

    // Many-to-One: Schedule milik satu Classroom
    public function classroom()
    {
        return $this->belongsTo(Classroom::class);
    }
}
```

### 4.10 Model `Enrollment`

```bash
php artisan make:model Enrollment
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Enrollment extends Model
{
    use HasFactory;

    protected $fillable = [
        'student_id',
        'course_id',
        'academic_year',
        'semester',
        'status',
    ];

    // Many-to-One: Enrollment milik satu Student
    public function student()
    {
        return $this->belongsTo(Student::class);
    }

    // Many-to-One: Enrollment milik satu Course
    public function course()
    {
        return $this->belongsTo(Course::class);
    }
}
```

### 4.11 Model `Assignment`

```bash
php artisan make:model Assignment
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Assignment extends Model
{
    use HasFactory;

    protected $fillable = [
        'course_id',
        'title',
        'description',
        'due_date',
    ];

    protected function casts(): array
    {
        return [
            'due_date' => 'datetime',
        ];
    }

    // Many-to-One: Assignment milik satu Course
    public function course()
    {
        return $this->belongsTo(Course::class);
    }

    // One-to-Many: Assignment memiliki banyak Submission
    public function submissions()
    {
        return $this->hasMany(Submission::class);
    }
}
```

### 4.12 Model `Submission`

```bash
php artisan make:model Submission
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Submission extends Model
{
    use HasFactory;

    protected $fillable = [
        'assignment_id',
        'student_id',
        'file_path',
        'notes',
        'submitted_at',
        'score',
    ];

    protected function casts(): array
    {
        return [
            'submitted_at' => 'datetime',
            'score' => 'decimal:2',
        ];
    }

    // Many-to-One: Submission milik satu Assignment
    public function assignment()
    {
        return $this->belongsTo(Assignment::class);
    }

    // Many-to-One: Submission milik satu Student
    public function student()
    {
        return $this->belongsTo(Student::class);
    }
}
```

### 4.13 Model `Grade`

```bash
php artisan make:model Grade
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Grade extends Model
{
    use HasFactory;

    protected $fillable = [
        'student_id',
        'course_id',
        'academic_year',
        'midterm_score',
        'final_score',
        'grade_letter',
    ];

    protected function casts(): array
    {
        return [
            'midterm_score' => 'decimal:2',
            'final_score' => 'decimal:2',
        ];
    }

    // Many-to-One: Grade milik satu Student
    public function student()
    {
        return $this->belongsTo(Student::class);
    }

    // Many-to-One: Grade milik satu Course
    public function course()
    {
        return $this->belongsTo(Course::class);
    }
}
```

### 4.14 Model `Announcement`

```bash
php artisan make:model Announcement
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Announcement extends Model
{
    use HasFactory;

    protected $fillable = [
        'user_id',
        'title',
        'content',
        'is_published',
    ];

    protected function casts(): array
    {
        return [
            'is_published' => 'boolean',
        ];
    }

    // Many-to-One: Announcement milik satu User
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```

### 4.15 Model `Tag`

```bash
php artisan make:model Tag
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Tag extends Model
{
    use HasFactory;

    protected $fillable = [
        'name',
        'slug',
    ];

    // Many-to-Many: Tag memiliki banyak Course
    public function courses()
    {
        return $this->belongsToMany(Course::class, 'course_tag')
                    ->withTimestamps();
    }
}
```

### 4.16 Model `Extracurricular`

```bash
php artisan make:model Extracurricular
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Extracurricular extends Model
{
    use HasFactory;

    protected $fillable = [
        'name',
        'description',
        'max_members',
    ];

    // Many-to-Many: Extracurricular memiliki banyak Student
    public function students()
    {
        return $this->belongsToMany(Student::class, 'extracurricular_student')
                    ->withPivot('joined_at', 'role')
                    ->withTimestamps();
    }
}
```

---

## TAHAP 5: Isikan Data Dummy (Seeder & Factory)

### 5.1 Buat Seeder Utama

```bash
php artisan make:seeder DatabaseSeeder
```

File: `database/seeders/DatabaseSeeder.php`

```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\Hash;
use App\Models\User;
use App\Models\Profile;
use App\Models\Department;
use App\Models\Teacher;
use App\Models\Student;
use App\Models\Category;
use App\Models\Course;
use App\Models\Classroom;
use App\Models\Schedule;
use App\Models\Assignment;
use App\Models\Submission;
use App\Models\Grade;
use App\Models\Announcement;
use App\Models\Tag;
use App\Models\Extracurricular;

class DatabaseSeeder extends Seeder
{
    public function run(): void
    {
        // ============================
        // 1. USERS (Admin & User)
        // ============================
        $admin = User::create([
            'name' => 'Administrator',
            'email' => 'admin@siakad.com',
            'password' => Hash::make('password'),
            'role' => 'admin',
        ]);

        Profile::create([
            'user_id' => $admin->id,
            'phone' => '081234567890',
            'address' => 'Jl. Admin No. 1, Jakarta',
            'birth_date' => '1985-05-15',
        ]);

        // Buat 5 user dosen
        $dosenUsers = [];
        $dosenNames = ['Dr. Budi Santoso', 'Dr. Siti Aminah', 'Prof. Ahmad Dahlan', 'Dr. Rina Kartika', 'Dr. Hendra Wijaya'];
        foreach ($dosenNames as $i => $name) {
            $user = User::create([
                'name' => $name,
                'email' => 'dosen' . ($i + 1) . '@siakad.com',
                'password' => Hash::make('password'),
                'role' => 'user',
            ]);
            Profile::create([
                'user_id' => $user->id,
                'phone' => '0812345678' . ($i + 10),
                'address' => 'Jl. Dosen No. ' . ($i + 1),
                'birth_date' => '198' . $i . '-0' . ($i + 1) . '-10',
            ]);
            $dosenUsers[] = $user;
        }

        // Buat 10 user mahasiswa
        $mhsUsers = [];
        $mhsNames = ['Andi Pratama', 'Bela Safitri', 'Candra Wijaya', 'Dina Puspita', 'Eko Saputra',
                      'Fira Handayani', 'Galih Permana', 'Hani Rahmawati', 'Irfan Maulana', 'Jasmine Putri'];
        foreach ($mhsNames as $i => $name) {
            $user = User::create([
                'name' => $name,
                'email' => 'mahasiswa' . ($i + 1) . '@siakad.com',
                'password' => Hash::make('password'),
                'role' => 'user',
            ]);
            Profile::create([
                'user_id' => $user->id,
                'phone' => '0856789012' . ($i + 10),
                'address' => 'Jl. Mahasiswa No. ' . ($i + 1),
                'birth_date' => '200' . ($i % 5) . '-0' . (($i % 9) + 1) . '-' . (($i + 1) * 2),
            ]);
            $mhsUsers[] = $user;
        }

        // ============================
        // 2. DEPARTMENTS
        // ============================
        $departments = [];
        $deptData = [
            ['name' => 'Teknik Informatika', 'code' => 'TI', 'description' => 'Jurusan Teknik Informatika'],
            ['name' => 'Sistem Informasi', 'code' => 'SI', 'description' => 'Jurusan Sistem Informasi'],
            ['name' => 'Teknik Elektro', 'code' => 'TE', 'description' => 'Jurusan Teknik Elektro'],
            ['name' => 'Manajemen', 'code' => 'MN', 'description' => 'Jurusan Manajemen'],
            ['name' => 'Akuntansi', 'code' => 'AK', 'description' => 'Jurusan Akuntansi'],
        ];
        foreach ($deptData as $d) {
            $departments[] = Department::create($d);
        }

        // ============================
        // 3. TEACHERS
        // ============================
        $teachers = [];
        $specializations = ['Artificial Intelligence', 'Database Systems', 'Network Security', 'Software Engineering', 'Data Science'];
        foreach ($dosenUsers as $i => $user) {
            $teachers[] = Teacher::create([
                'user_id' => $user->id,
                'department_id' => $departments[$i]->id,
                'nip' => '19800' . ($i + 1) . '0101' . str_pad($i + 1, 4, '0', STR_PAD_LEFT),
                'specialization' => $specializations[$i],
            ]);
        }

        // ============================
        // 4. STUDENTS
        // ============================
        $students = [];
        foreach ($mhsUsers as $i => $user) {
            $students[] = Student::create([
                'user_id' => $user->id,
                'department_id' => $departments[$i % 5]->id,
                'nim' => '2024' . str_pad($i + 1, 6, '0', STR_PAD_LEFT),
                'semester' => rand(1, 8),
            ]);
        }

        // ============================
        // 5. CATEGORIES
        // ============================
        $categories = [];
        $catData = [
            ['name' => 'Pemrograman', 'description' => 'Mata kuliah terkait pemrograman'],
            ['name' => 'Basis Data', 'description' => 'Mata kuliah terkait basis data'],
            ['name' => 'Jaringan', 'description' => 'Mata kuliah terkait jaringan komputer'],
            ['name' => 'Matematika', 'description' => 'Mata kuliah terkait matematika'],
            ['name' => 'Umum', 'description' => 'Mata kuliah umum'],
        ];
        foreach ($catData as $c) {
            $categories[] = Category::create($c);
        }

        // ============================
        // 6. COURSES
        // ============================
        $courses = [];
        $courseData = [
            ['category_id' => 1, 'department_id' => 1, 'code' => 'TI101', 'name' => 'Pemrograman Web', 'credits' => 3, 'description' => 'Dasar pemrograman web dengan HTML, CSS, JS'],
            ['category_id' => 1, 'department_id' => 1, 'code' => 'TI102', 'name' => 'Pemrograman Mobile', 'credits' => 3, 'description' => 'Pengembangan aplikasi mobile'],
            ['category_id' => 2, 'department_id' => 1, 'code' => 'TI201', 'name' => 'Basis Data Lanjut', 'credits' => 3, 'description' => 'Konsep lanjutan basis data'],
            ['category_id' => 3, 'department_id' => 3, 'code' => 'TE101', 'name' => 'Jaringan Komputer', 'credits' => 3, 'description' => 'Dasar jaringan komputer'],
            ['category_id' => 4, 'department_id' => 2, 'code' => 'SI101', 'name' => 'Statistika', 'credits' => 2, 'description' => 'Dasar statistika'],
            ['category_id' => 1, 'department_id' => 1, 'code' => 'TI301', 'name' => 'Framework Laravel', 'credits' => 3, 'description' => 'Pengembangan web dengan Laravel'],
            ['category_id' => 5, 'department_id' => 4, 'code' => 'MN101', 'name' => 'Pengantar Manajemen', 'credits' => 2, 'description' => 'Dasar-dasar manajemen'],
            ['category_id' => 5, 'department_id' => 5, 'code' => 'AK101', 'name' => 'Akuntansi Dasar', 'credits' => 3, 'description' => 'Dasar-dasar akuntansi'],
            ['category_id' => 2, 'department_id' => 2, 'code' => 'SI201', 'name' => 'Data Warehouse', 'credits' => 3, 'description' => 'Konsep data warehouse'],
            ['category_id' => 1, 'department_id' => 1, 'code' => 'TI401', 'name' => 'Machine Learning', 'credits' => 3, 'description' => 'Dasar machine learning'],
        ];
        foreach ($courseData as $c) {
            $courses[] = Course::create($c);
        }

        // ============================
        // 7. CLASSROOMS
        // ============================
        $classrooms = [];
        $roomData = [
            ['name' => 'Lab Komputer 1', 'building' => 'Gedung A', 'capacity' => 40],
            ['name' => 'Lab Komputer 2', 'building' => 'Gedung A', 'capacity' => 35],
            ['name' => 'Ruang 301', 'building' => 'Gedung B', 'capacity' => 50],
            ['name' => 'Ruang 302', 'building' => 'Gedung B', 'capacity' => 45],
            ['name' => 'Aula Utama', 'building' => 'Gedung C', 'capacity' => 200],
            ['name' => 'Lab Jaringan', 'building' => 'Gedung A', 'capacity' => 30],
        ];
        foreach ($roomData as $r) {
            $classrooms[] = Classroom::create($r);
        }

        // ============================
        // 8. SCHEDULES
        // ============================
        $days = ['Senin', 'Selasa', 'Rabu', 'Kamis', 'Jumat'];
        foreach ($courses as $i => $course) {
            Schedule::create([
                'course_id' => $course->id,
                'teacher_id' => $teachers[$i % 5]->id,
                'classroom_id' => $classrooms[$i % 6]->id,
                'day' => $days[$i % 5],
                'start_time' => sprintf('%02d:00', 7 + ($i % 5) * 2),
                'end_time' => sprintf('%02d:40', 8 + ($i % 5) * 2),
            ]);
        }

        // ============================
        // 9. ENROLLMENTS (Many-to-Many: Students <-> Courses)
        // ============================
        foreach ($students as $i => $student) {
            // Setiap mahasiswa mengambil 3-4 mata kuliah
            $courseIds = collect($courses)->random(rand(3, 4))->pluck('id');
            foreach ($courseIds as $courseId) {
                $student->courses()->attach($courseId, [
                    'academic_year' => '2025/2026',
                    'semester' => $i % 2 == 0 ? 'Ganjil' : 'Genap',
                    'status' => 'active',
                ]);
            }
        }

        // ============================
        // 10. ASSIGNMENTS
        // ============================
        $assignments = [];
        foreach ($courses as $course) {
            for ($j = 1; $j <= 2; $j++) {
                $assignments[] = Assignment::create([
                    'course_id' => $course->id,
                    'title' => 'Tugas ' . $j . ' - ' . $course->name,
                    'description' => 'Deskripsi tugas ' . $j . ' untuk mata kuliah ' . $course->name,
                    'due_date' => now()->addDays(rand(7, 30)),
                ]);
            }
        }

        // ============================
        // 11. SUBMISSIONS
        // ============================
        foreach ($assignments as $assignment) {
            $randomStudents = collect($students)->random(rand(3, 5));
            foreach ($randomStudents as $student) {
                Submission::create([
                    'assignment_id' => $assignment->id,
                    'student_id' => $student->id,
                    'file_path' => 'submissions/tugas_' . $student->nim . '_' . $assignment->id . '.pdf',
                    'notes' => 'Pengumpulan tugas oleh ' . $student->user->name,
                    'submitted_at' => now()->subDays(rand(1, 5)),
                    'score' => rand(60, 100),
                ]);
            }
        }

        // ============================
        // 12. GRADES
        // ============================
        $gradeLetters = ['A', 'AB', 'B', 'BC', 'C', 'D', 'E'];
        foreach ($students as $student) {
            $enrolledCourses = $student->courses;
            foreach ($enrolledCourses as $course) {
                $midterm = rand(50, 100);
                $final = rand(50, 100);
                $avg = ($midterm + $final) / 2;

                if ($avg >= 85) $letter = 'A';
                elseif ($avg >= 80) $letter = 'AB';
                elseif ($avg >= 70) $letter = 'B';
                elseif ($avg >= 65) $letter = 'BC';
                elseif ($avg >= 55) $letter = 'C';
                elseif ($avg >= 45) $letter = 'D';
                else $letter = 'E';

                Grade::create([
                    'student_id' => $student->id,
                    'course_id' => $course->id,
                    'academic_year' => '2025/2026',
                    'midterm_score' => $midterm,
                    'final_score' => $final,
                    'grade_letter' => $letter,
                ]);
            }
        }

        // ============================
        // 13. ANNOUNCEMENTS
        // ============================
        $announcementData = [
            ['title' => 'Jadwal UTS Semester Ganjil 2025/2026', 'content' => 'Ujian Tengah Semester akan dilaksanakan pada tanggal 15-22 Oktober 2025.', 'is_published' => true],
            ['title' => 'Pendaftaran Semester Baru', 'content' => 'Pendaftaran mata kuliah semester baru dibuka mulai 1 September 2025.', 'is_published' => true],
            ['title' => 'Libur Nasional', 'content' => 'Perkuliahan diliburkan pada tanggal 17 Agustus 2025.', 'is_published' => true],
            ['title' => 'Workshop Laravel', 'content' => 'Workshop Laravel akan diadakan pada tanggal 25 September 2025 di Lab Komputer 1.', 'is_published' => false],
            ['title' => 'Wisuda Periode Oktober', 'content' => 'Wisuda periode Oktober 2025 akan dilaksanakan pada tanggal 30 Oktober 2025.', 'is_published' => true],
        ];
        foreach ($announcementData as $a) {
            Announcement::create(array_merge($a, ['user_id' => $admin->id]));
        }

        // ============================
        // 14. TAGS
        // ============================
        $tags = [];
        $tagData = [
            ['name' => 'Backend', 'slug' => 'backend'],
            ['name' => 'Frontend', 'slug' => 'frontend'],
            ['name' => 'Database', 'slug' => 'database'],
            ['name' => 'AI/ML', 'slug' => 'ai-ml'],
            ['name' => 'Wajib', 'slug' => 'wajib'],
            ['name' => 'Pilihan', 'slug' => 'pilihan'],
            ['name' => 'Praktikum', 'slug' => 'praktikum'],
        ];
        foreach ($tagData as $t) {
            $tags[] = Tag::create($t);
        }

        // ============================
        // 15. COURSE_TAG (Many-to-Many: Courses <-> Tags)
        // ============================
        foreach ($courses as $course) {
            $tagIds = collect($tags)->random(rand(2, 4))->pluck('id');
            $course->tags()->attach($tagIds);
        }

        // ============================
        // 16. EXTRACURRICULARS
        // ============================
        $extras = [];
        $extraData = [
            ['name' => 'Klub Pemrograman', 'description' => 'Klub untuk belajar dan berlatih pemrograman', 'max_members' => 50],
            ['name' => 'Basket', 'description' => 'Tim basket universitas', 'max_members' => 20],
            ['name' => 'Paduan Suara', 'description' => 'Kelompok paduan suara universitas', 'max_members' => 40],
            ['name' => 'Robotika', 'description' => 'Klub robotika dan IoT', 'max_members' => 30],
            ['name' => 'English Club', 'description' => 'Klub bahasa Inggris', 'max_members' => 60],
        ];
        foreach ($extraData as $e) {
            $extras[] = Extracurricular::create($e);
        }

        // ============================
        // 17. EXTRACURRICULAR_STUDENT (Many-to-Many: Students <-> Extracurriculars)
        // ============================
        $roles = ['member', 'leader', 'secretary'];
        foreach ($students as $i => $student) {
            $extraIds = collect($extras)->random(rand(1, 3))->pluck('id');
            foreach ($extraIds as $j => $extraId) {
                $student->extracurriculars()->attach($extraId, [
                    'joined_at' => now()->subMonths(rand(1, 12))->format('Y-m-d'),
                    'role' => $j === 0 && $i < 5 ? $roles[$i % 3] : 'member',
                ]);
            }
        }
    }
}
```

### 5.2 Jalankan Seeder

```bash
php artisan migrate:fresh --seed
```

> **Akun Login:**
> - **Admin**: `admin@siakad.com` / `password`
> - **User (Dosen)**: `dosen1@siakad.com` s/d `dosen5@siakad.com` / `password`
> - **User (Mahasiswa)**: `mahasiswa1@siakad.com` s/d `mahasiswa10@siakad.com` / `password`

---

## TAHAP 6: Buat Route

### 6.1 Setup Autentikasi Route

File: `routes/web.php`

```php
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\AuthController;
use App\Http\Controllers\DashboardController;
use App\Http\Controllers\UserController;
use App\Http\Controllers\ProfileController;
use App\Http\Controllers\DepartmentController;
use App\Http\Controllers\TeacherController;
use App\Http\Controllers\StudentController;
use App\Http\Controllers\CategoryController;
use App\Http\Controllers\CourseController;
use App\Http\Controllers\ClassroomController;
use App\Http\Controllers\ScheduleController;
use App\Http\Controllers\EnrollmentController;
use App\Http\Controllers\AssignmentController;
use App\Http\Controllers\SubmissionController;
use App\Http\Controllers\GradeController;
use App\Http\Controllers\AnnouncementController;
use App\Http\Controllers\TagController;
use App\Http\Controllers\ExtracurricularController;

// ============================
// Route Autentikasi (Guest Only)
// ============================
Route::middleware('guest')->group(function () {
    Route::get('/login', [AuthController::class, 'showLoginForm'])->name('login');
    Route::post('/login', [AuthController::class, 'login'])->name('login.process');
});

// Route Logout
Route::post('/logout', [AuthController::class, 'logout'])->name('logout')->middleware('auth');

// ============================
// Route yang Membutuhkan Login (Middleware 'auth')
// ============================
Route::middleware('auth')->group(function () {

    // Dashboard
    Route::get('/', [DashboardController::class, 'index'])->name('dashboard');

    // ============================
    // Resource Routes untuk semua modul CRUD
    // ============================
    Route::resource('users', UserController::class);
    Route::resource('profiles', ProfileController::class);
    Route::resource('departments', DepartmentController::class);
    Route::resource('teachers', TeacherController::class);
    Route::resource('students', StudentController::class);
    Route::resource('categories', CategoryController::class);
    Route::resource('courses', CourseController::class);
    Route::resource('classrooms', ClassroomController::class);
    Route::resource('schedules', ScheduleController::class);
    Route::resource('enrollments', EnrollmentController::class);
    Route::resource('assignments', AssignmentController::class);
    Route::resource('submissions', SubmissionController::class);
    Route::resource('grades', GradeController::class);
    Route::resource('announcements', AnnouncementController::class);
    Route::resource('tags', TagController::class);
    Route::resource('extracurriculars', ExtracurricularController::class);
});
```

### 6.2 Daftar Route yang Dihasilkan oleh `Route::resource()`

Setiap `Route::resource()` menghasilkan 7 route:

| Method    | URI                          | Action    | Route Name               | Keterangan         |
|-----------|------------------------------|-----------|--------------------------|---------------------|
| GET       | `/departments`               | `index`   | `departments.index`      | Tampilkan semua data|
| GET       | `/departments/create`        | `create`  | `departments.create`     | Form tambah data    |
| POST      | `/departments`               | `store`   | `departments.store`      | Simpan data baru    |
| GET       | `/departments/{department}`  | `show`    | `departments.show`       | Detail satu data    |
| GET       | `/departments/{department}/edit` | `edit`| `departments.edit`       | Form edit data      |
| PUT/PATCH | `/departments/{department}`  | `update`  | `departments.update`     | Update data         |
| DELETE    | `/departments/{department}`  | `destroy` | `departments.destroy`    | Hapus data          |

> Pola yang sama berlaku untuk semua 16 modul resource.

---

## TAHAP 7: Buat Controller

### 7.1 AuthController (Autentikasi)

```bash
php artisan make:controller AuthController
```

File: `app/Http/Controllers/AuthController.php`

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;

class AuthController extends Controller
{
    // Tampilkan form login
    public function showLoginForm()
    {
        return view('auth.login');
    }

    // Proses login
    public function login(Request $request)
    {
        $credentials = $request->validate([
            'email' => 'required|email',
            'password' => 'required',
        ]);

        if (Auth::attempt($credentials, $request->boolean('remember'))) {
            $request->session()->regenerate();
            return redirect()->intended('/');
        }

        return back()->withErrors([
            'email' => 'Email atau password salah.',
        ])->onlyInput('email');
    }

    // Proses logout
    public function logout(Request $request)
    {
        Auth::logout();
        $request->session()->invalidate();
        $request->session()->regenerateToken();
        return redirect('/login');
    }
}
```

### 7.2 Setup Gate di `AppServiceProvider`

File: `app/Providers/AppServiceProvider.php`

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Gate;

class AppServiceProvider extends ServiceProvider
{
    public function register(): void
    {
        //
    }

    public function boot(): void
    {
        // Definisikan Gate 'admin'
        // Hanya user dengan role 'admin' yang bisa create, update, delete
        Gate::define('admin', function ($user) {
            return $user->role === 'admin';
        });
    }
}
```

### 7.3 DashboardController

```bash
php artisan make:controller DashboardController
```

```php
<?php

namespace App\Http\Controllers;

use App\Models\User;
use App\Models\Student;
use App\Models\Teacher;
use App\Models\Course;
use App\Models\Department;

class DashboardController extends Controller
{
    public function index()
    {
        $stats = [
            'users' => User::count(),
            'students' => Student::count(),
            'teachers' => Teacher::count(),
            'courses' => Course::count(),
            'departments' => Department::count(),
        ];

        return view('dashboard', compact('stats'));
    }
}
```

### 7.4 Contoh Controller CRUD Lengkap — `DepartmentController`

> **Pola ini berlaku untuk semua 16 modul CRUD.** Setiap controller mengikuti struktur yang sama. Di bawah ini diberikan contoh lengkap untuk modul **Department**, lalu panduan singkat perbedaan di setiap modul lainnya.

```bash
php artisan make:controller DepartmentController --resource
```

File: `app/Http/Controllers/DepartmentController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Department;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;

class DepartmentController extends Controller
{
    /**
     * Tampilkan semua data department.
     * Dapat diakses oleh semua user yang sudah login.
     */
    public function index()
    {
        $departments = Department::withCount(['teachers', 'students', 'courses'])->paginate(10);
        return view('departments.index', compact('departments'));
    }

    /**
     * Tampilkan form untuk membuat department baru.
     * Hanya admin yang bisa mengakses.
     */
    public function create()
    {
        Gate::authorize('admin');
        return view('departments.create');
    }

    /**
     * Simpan department baru ke database.
     * Hanya admin yang bisa mengakses.
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'code' => 'required|string|max:10|unique:departments,code',
            'description' => 'nullable|string',
        ]);

        Department::create($validated);

        return redirect()->route('departments.index')
                         ->with('success', 'Department berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail satu department.
     * Dapat diakses oleh semua user yang sudah login.
     */
    public function show(Department $department)
    {
        $department->load(['teachers.user', 'students.user', 'courses']);
        return view('departments.show', compact('department'));
    }

    /**
     * Tampilkan form untuk mengedit department.
     * Hanya admin yang bisa mengakses.
     */
    public function edit(Department $department)
    {
        Gate::authorize('admin');
        return view('departments.edit', compact('department'));
    }

    /**
     * Update data department di database.
     * Hanya admin yang bisa mengakses.
     */
    public function update(Request $request, Department $department)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'code' => 'required|string|max:10|unique:departments,code,' . $department->id,
            'description' => 'nullable|string',
        ]);

        $department->update($validated);

        return redirect()->route('departments.index')
                         ->with('success', 'Department berhasil diupdate.');
    }

    /**
     * Hapus department dari database.
     * Hanya admin yang bisa mengakses.
     */
    public function destroy(Department $department)
    {
        Gate::authorize('admin');

        $department->delete();

        return redirect()->route('departments.index')
                         ->with('success', 'Department berhasil dihapus.');
    }
}
```

---

### 7.5 Panduan Controller untuk Setiap Modul

Setiap controller mengikuti pola yang sama seperti `DepartmentController`. Berikut **perbedaan spesifik** untuk setiap modul:

#### 📌 Modul 1: `UserController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `User`                                                                   |
| Validation    | `name` (required), `email` (required, email, unique), `password` (required saat create, nullable saat edit), `role` (required, in:admin,user) |
| Relasi Load   | `index`: `with('profile')` · `show`: `with(['profile', 'teacher', 'student', 'announcements'])` |
| Catatan       | Password di-hash saat store/update. Saat update, password hanya diubah jika diisi. |

#### 📌 Modul 2: `ProfileController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Profile`                                                                |
| Validation    | `user_id` (required, exists:users,id), `phone` (nullable, max:20), `address` (nullable), `avatar` (nullable, image), `birth_date` (nullable, date) |
| Relasi Load   | `index`: `with('user')` · `show`: `with('user')` |
| Catatan       | Pada form create, tampilkan dropdown daftar user yang belum memiliki profil. |

#### 📌 Modul 3: `DepartmentController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Department`                                                             |
| Validation    | `name` (required), `code` (required, max:10, unique), `description` (nullable) |
| Relasi Load   | `index`: `withCount(['teachers', 'students', 'courses'])` · `show`: `with(['teachers.user', 'students.user', 'courses'])` |

#### 📌 Modul 4: `TeacherController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Teacher`                                                                |
| Validation    | `user_id` (required, exists:users,id), `department_id` (required, exists:departments,id), `nip` (required, unique), `specialization` (nullable) |
| Relasi Load   | `index`: `with(['user', 'department'])` · `show`: `with(['user.profile', 'department', 'schedules.course'])` |
| Form Data     | Kirim daftar `$users` dan `$departments` ke view create/edit. |

#### 📌 Modul 5: `StudentController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Student`                                                                |
| Validation    | `user_id` (required, exists:users,id), `department_id` (required, exists:departments,id), `nim` (required, unique), `semester` (required, integer, min:1, max:14) |
| Relasi Load   | `index`: `with(['user', 'department'])` · `show`: `with(['user.profile', 'department', 'courses', 'extracurriculars'])` |
| Form Data     | Kirim daftar `$users` dan `$departments` ke view create/edit. |

#### 📌 Modul 6: `CategoryController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Category`                                                               |
| Validation    | `name` (required), `description` (nullable) |
| Relasi Load   | `index`: `withCount('courses')` · `show`: `with('courses')` |

#### 📌 Modul 7: `CourseController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Course`                                                                 |
| Validation    | `category_id` (required, exists), `department_id` (required, exists), `code` (required, unique), `name` (required), `credits` (required, integer, min:1, max:6), `description` (nullable), `tags` (nullable, array) |
| Relasi Load   | `index`: `with(['category', 'department'])` · `show`: `with(['category', 'department', 'tags', 'schedules.teacher', 'assignments'])` |
| Form Data     | Kirim `$categories`, `$departments`, `$tags` ke view create/edit. |
| Catatan       | Gunakan `$course->tags()->sync($request->tags)` untuk menyimpan relasi many-to-many dengan tags. |

#### 📌 Modul 8: `ClassroomController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Classroom`                                                              |
| Validation    | `name` (required), `building` (required), `capacity` (required, integer, min:1) |
| Relasi Load   | `index`: query biasa · `show`: `with('schedules.course')` |

#### 📌 Modul 9: `ScheduleController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Schedule`                                                               |
| Validation    | `course_id` (required, exists), `teacher_id` (required, exists), `classroom_id` (required, exists), `day` (required, in:Senin,...,Sabtu), `start_time` (required), `end_time` (required, after:start_time) |
| Relasi Load   | `index`: `with(['course', 'teacher.user', 'classroom'])` · `show`: sama |
| Form Data     | Kirim `$courses`, `$teachers`, `$classrooms` ke view create/edit. |

#### 📌 Modul 10: `EnrollmentController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Enrollment`                                                             |
| Validation    | `student_id` (required, exists), `course_id` (required, exists), `academic_year` (required, max:9), `semester` (required, in:Ganjil,Genap), `status` (required, in:active,dropped,completed) |
| Relasi Load   | `index`: `with(['student.user', 'course'])` · `show`: sama |
| Form Data     | Kirim `$students` (with user name) dan `$courses` ke view create/edit. |

#### 📌 Modul 11: `AssignmentController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Assignment`                                                             |
| Validation    | `course_id` (required, exists), `title` (required), `description` (nullable), `due_date` (required, date, after:today) |
| Relasi Load   | `index`: `with('course')` · `show`: `with(['course', 'submissions.student.user'])` |
| Form Data     | Kirim `$courses` ke view create/edit. |

#### 📌 Modul 12: `SubmissionController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Submission`                                                             |
| Validation    | `assignment_id` (required, exists), `student_id` (required, exists), `file_path` (nullable), `notes` (nullable), `submitted_at` (nullable, date), `score` (nullable, numeric, min:0, max:100) |
| Relasi Load   | `index`: `with(['assignment.course', 'student.user'])` · `show`: sama |
| Form Data     | Kirim `$assignments` dan `$students` ke view create/edit. |

#### 📌 Modul 13: `GradeController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Grade`                                                                  |
| Validation    | `student_id` (required, exists), `course_id` (required, exists), `academic_year` (required), `midterm_score` (nullable, numeric, min:0, max:100), `final_score` (nullable, numeric, min:0, max:100), `grade_letter` (nullable, in:A,AB,B,BC,C,D,E) |
| Relasi Load   | `index`: `with(['student.user', 'course'])` · `show`: sama |
| Form Data     | Kirim `$students` dan `$courses` ke view create/edit. |

#### 📌 Modul 14: `AnnouncementController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Announcement`                                                           |
| Validation    | `title` (required), `content` (required), `is_published` (boolean) |
| Relasi Load   | `index`: `with('user')` · `show`: `with('user')` |
| Catatan       | `user_id` diisi otomatis dari `auth()->id()` saat store. |

#### 📌 Modul 15: `TagController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Tag`                                                                    |
| Validation    | `name` (required), `slug` (required, unique) |
| Relasi Load   | `index`: `withCount('courses')` · `show`: `with('courses')` |
| Catatan       | Slug bisa di-generate otomatis dari name menggunakan `Str::slug()`. |

#### 📌 Modul 16: `ExtracurricularController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Extracurricular`                                                        |
| Validation    | `name` (required), `description` (nullable), `max_members` (required, integer, min:1) |
| Relasi Load   | `index`: `withCount('students')` · `show`: `with('students.user')` |

---

## TAHAP 8: Buat View — Read (Index / Tampilkan Semua Data)

### 8.1 Setup Layout Utama dengan Blade Component

Buat layout utama yang akan digunakan oleh semua view.

File: `resources/views/layouts/app.blade.php`

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SIAKAD - @yield('title', 'Dashboard')</title>
    <!-- Bootstrap 5 CDN -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Bootstrap Icons -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.0/font/bootstrap-icons.css" rel="stylesheet">
</head>
<body>
    <!-- Navbar -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container">
            <a class="navbar-brand" href="{{ route('dashboard') }}">
                <i class="bi bi-mortarboard-fill"></i> SIAKAD
            </a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav me-auto">
                    <li class="nav-item"><a class="nav-link" href="{{ route('departments.index') }}">Jurusan</a></li>
                    <li class="nav-item"><a class="nav-link" href="{{ route('teachers.index') }}">Dosen</a></li>
                    <li class="nav-item"><a class="nav-link" href="{{ route('students.index') }}">Mahasiswa</a></li>
                    <li class="nav-item"><a class="nav-link" href="{{ route('courses.index') }}">Mata Kuliah</a></li>
                    <li class="nav-item"><a class="nav-link" href="{{ route('schedules.index') }}">Jadwal</a></li>
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">
                            Lainnya
                        </a>
                        <ul class="dropdown-menu">
                            <li><a class="dropdown-item" href="{{ route('users.index') }}">Users</a></li>
                            <li><a class="dropdown-item" href="{{ route('profiles.index') }}">Profiles</a></li>
                            <li><a class="dropdown-item" href="{{ route('categories.index') }}">Kategori</a></li>
                            <li><a class="dropdown-item" href="{{ route('classrooms.index') }}">Ruangan</a></li>
                            <li><a class="dropdown-item" href="{{ route('enrollments.index') }}">Enrollment</a></li>
                            <li><a class="dropdown-item" href="{{ route('assignments.index') }}">Tugas</a></li>
                            <li><a class="dropdown-item" href="{{ route('submissions.index') }}">Submission</a></li>
                            <li><a class="dropdown-item" href="{{ route('grades.index') }}">Nilai</a></li>
                            <li><a class="dropdown-item" href="{{ route('announcements.index') }}">Pengumuman</a></li>
                            <li><a class="dropdown-item" href="{{ route('tags.index') }}">Tags</a></li>
                            <li><a class="dropdown-item" href="{{ route('extracurriculars.index') }}">Ekstrakurikuler</a></li>
                        </ul>
                    </li>
                </ul>
                <ul class="navbar-nav">
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">
                            <i class="bi bi-person-circle"></i>
                            {{ Auth::user()->name }}
                            <span class="badge bg-{{ Auth::user()->role === 'admin' ? 'danger' : 'primary' }}">
                                {{ ucfirst(Auth::user()->role) }}
                            </span>
                        </a>
                        <ul class="dropdown-menu dropdown-menu-end">
                            <li>
                                <form action="{{ route('logout') }}" method="POST">
                                    @csrf
                                    <button type="submit" class="dropdown-item text-danger">
                                        <i class="bi bi-box-arrow-right"></i> Logout
                                    </button>
                                </form>
                            </li>
                        </ul>
                    </li>
                </ul>
            </div>
        </div>
    </nav>

    <!-- Content -->
    <div class="container mt-4">
        {{-- Flash Messages --}}
        @if(session('success'))
            <div class="alert alert-success alert-dismissible fade show" role="alert">
                {{ session('success') }}
                <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
            </div>
        @endif

        @if($errors->any())
            <div class="alert alert-danger alert-dismissible fade show" role="alert">
                <ul class="mb-0">
                    @foreach($errors->all() as $error)
                        <li>{{ $error }}</li>
                    @endforeach
                </ul>
                <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
            </div>
        @endif

        @yield('content')
    </div>

    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

### 8.2 View Login

File: `resources/views/auth/login.blade.php`

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SIAKAD - Login</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="bg-light">
    <div class="container">
        <div class="row justify-content-center mt-5">
            <div class="col-md-5">
                <div class="card shadow">
                    <div class="card-body p-4">
                        <h3 class="text-center mb-4">🎓 SIAKAD Login</h3>

                        @if($errors->any())
                            <div class="alert alert-danger">
                                @foreach($errors->all() as $error)
                                    <p class="mb-0">{{ $error }}</p>
                                @endforeach
                            </div>
                        @endif

                        <form method="POST" action="{{ route('login.process') }}">
                            @csrf
                            <div class="mb-3">
                                <label for="email" class="form-label">Email</label>
                                <input type="email" class="form-control" id="email" name="email"
                                       value="{{ old('email') }}" required autofocus>
                            </div>
                            <div class="mb-3">
                                <label for="password" class="form-label">Password</label>
                                <input type="password" class="form-control" id="password" name="password" required>
                            </div>
                            <div class="mb-3 form-check">
                                <input type="checkbox" class="form-check-input" id="remember" name="remember">
                                <label class="form-check-label" for="remember">Ingat Saya</label>
                            </div>
                            <button type="submit" class="btn btn-primary w-100">Login</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```

### 8.3 View Dashboard

File: `resources/views/dashboard.blade.php`

```html
@extends('layouts.app')

@section('title', 'Dashboard')

@section('content')
<h2>Dashboard</h2>
<p>Selamat datang, <strong>{{ Auth::user()->name }}</strong>! Anda login sebagai <strong>{{ ucfirst(Auth::user()->role) }}</strong>.</p>

<div class="row mt-4">
    <div class="col-md-3">
        <div class="card text-white bg-primary mb-3">
            <div class="card-body">
                <h5 class="card-title">Total Users</h5>
                <h2>{{ $stats['users'] }}</h2>
            </div>
        </div>
    </div>
    <div class="col-md-3">
        <div class="card text-white bg-success mb-3">
            <div class="card-body">
                <h5 class="card-title">Total Mahasiswa</h5>
                <h2>{{ $stats['students'] }}</h2>
            </div>
        </div>
    </div>
    <div class="col-md-3">
        <div class="card text-white bg-warning mb-3">
            <div class="card-body">
                <h5 class="card-title">Total Dosen</h5>
                <h2>{{ $stats['teachers'] }}</h2>
            </div>
        </div>
    </div>
    <div class="col-md-3">
        <div class="card text-white bg-info mb-3">
            <div class="card-body">
                <h5 class="card-title">Total Mata Kuliah</h5>
                <h2>{{ $stats['courses'] }}</h2>
            </div>
        </div>
    </div>
</div>
@endsection
```

### 8.4 Contoh View Index — `departments/index.blade.php`

> **Pola ini berlaku untuk semua modul.** Sesuaikan nama kolom tabel dan data yang ditampilkan.

File: `resources/views/departments/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Jurusan')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Jurusan</h2>
    @can('admin')
        <a href="{{ route('departments.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Jurusan
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Kode</th>
            <th>Nama Jurusan</th>
            <th>Jumlah Dosen</th>
            <th>Jumlah Mahasiswa</th>
            <th>Jumlah Mata Kuliah</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($departments as $i => $dept)
        <tr>
            <td>{{ $departments->firstItem() + $i }}</td>
            <td><span class="badge bg-secondary">{{ $dept->code }}</span></td>
            <td>{{ $dept->name }}</td>
            <td>{{ $dept->teachers_count }}</td>
            <td>{{ $dept->students_count }}</td>
            <td>{{ $dept->courses_count }}</td>
            <td>
                <a href="{{ route('departments.show', $dept) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('departments.edit', $dept) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('departments.destroy', $dept) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus?')">
                        @csrf
                        @method('DELETE')
                        <button type="submit" class="btn btn-sm btn-danger">
                            <i class="bi bi-trash"></i> Hapus
                        </button>
                    </form>
                @endcan
            </td>
        </tr>
        @empty
        <tr>
            <td colspan="7" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $departments->links() }}
@endsection
```

> **Catatan penting:**
> - Directive `@can('admin')` digunakan untuk menyembunyikan tombol **Tambah**, **Edit**, dan **Hapus** dari user non-admin.
> - User biasa (role `user`) hanya bisa melihat tabel data dan tombol **Detail**.

---

## TAHAP 9: Buat View — Create (Tambah Data Baru)

### 9.1 Contoh View Create — `departments/create.blade.php`

File: `resources/views/departments/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Jurusan')

@section('content')
<h2>Tambah Jurusan Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('departments.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="name" class="form-label">Nama Jurusan <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name') }}" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="code" class="form-label">Kode Jurusan <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('code') is-invalid @enderror"
                       id="code" name="code" value="{{ old('code') }}" maxlength="10" required>
                @error('code')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="description" class="form-label">Deskripsi</label>
                <textarea class="form-control @error('description') is-invalid @enderror"
                          id="description" name="description" rows="3">{{ old('description') }}</textarea>
                @error('description')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('departments.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

### 9.2 Contoh View Create dengan Dropdown Relasi — `courses/create.blade.php`

> Untuk modul yang memiliki relasi foreign key, tampilkan **dropdown select** untuk memilih data dari tabel yang berelasi.

File: `resources/views/courses/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Mata Kuliah')

@section('content')
<h2>Tambah Mata Kuliah Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('courses.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="category_id" class="form-label">Kategori <span class="text-danger">*</span></label>
                <select class="form-select @error('category_id') is-invalid @enderror"
                        id="category_id" name="category_id" required>
                    <option value="">-- Pilih Kategori --</option>
                    @foreach($categories as $category)
                        <option value="{{ $category->id }}" {{ old('category_id') == $category->id ? 'selected' : '' }}>
                            {{ $category->name }}
                        </option>
                    @endforeach
                </select>
                @error('category_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="department_id" class="form-label">Jurusan <span class="text-danger">*</span></label>
                <select class="form-select @error('department_id') is-invalid @enderror"
                        id="department_id" name="department_id" required>
                    <option value="">-- Pilih Jurusan --</option>
                    @foreach($departments as $department)
                        <option value="{{ $department->id }}" {{ old('department_id') == $department->id ? 'selected' : '' }}>
                            {{ $department->name }}
                        </option>
                    @endforeach
                </select>
                @error('department_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="code" class="form-label">Kode Mata Kuliah <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('code') is-invalid @enderror"
                       id="code" name="code" value="{{ old('code') }}" maxlength="10" required>
                @error('code')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="name" class="form-label">Nama Mata Kuliah <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name') }}" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="credits" class="form-label">SKS <span class="text-danger">*</span></label>
                <input type="number" class="form-control @error('credits') is-invalid @enderror"
                       id="credits" name="credits" value="{{ old('credits', 2) }}" min="1" max="6" required>
                @error('credits')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="description" class="form-label">Deskripsi</label>
                <textarea class="form-control" id="description" name="description" rows="3">{{ old('description') }}</textarea>
            </div>

            {{-- Many-to-Many: Tags (Checkbox) --}}
            <div class="mb-3">
                <label class="form-label">Tags</label>
                <div class="row">
                    @foreach($tags as $tag)
                        <div class="col-md-3">
                            <div class="form-check">
                                <input class="form-check-input" type="checkbox" name="tags[]"
                                       value="{{ $tag->id }}" id="tag{{ $tag->id }}"
                                       {{ in_array($tag->id, old('tags', [])) ? 'checked' : '' }}>
                                <label class="form-check-label" for="tag{{ $tag->id }}">
                                    {{ $tag->name }}
                                </label>
                            </div>
                        </div>
                    @endforeach
                </div>
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('courses.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

### 9.3 Panduan Form Create untuk Setiap Modul

| Modul              | Tipe Input Field                                                                                                |
|--------------------|-----------------------------------------------------------------------------------------------------------------|
| Users              | `name` (text), `email` (email), `password` (password), `role` (select: admin/user)                              |
| Profiles           | `user_id` (select dropdown users), `phone` (text), `address` (textarea), `avatar` (file), `birth_date` (date)  |
| Departments        | `name` (text), `code` (text), `description` (textarea)                                                         |
| Teachers           | `user_id` (select), `department_id` (select), `nip` (text), `specialization` (text)                            |
| Students           | `user_id` (select), `department_id` (select), `nim` (text), `semester` (number)                                 |
| Categories         | `name` (text), `description` (textarea)                                                                        |
| Courses            | `category_id` (select), `department_id` (select), `code` (text), `name` (text), `credits` (number), `description` (textarea), `tags[]` (checkbox) |
| Classrooms         | `name` (text), `building` (text), `capacity` (number)                                                          |
| Schedules          | `course_id` (select), `teacher_id` (select), `classroom_id` (select), `day` (select), `start_time` (time), `end_time` (time) |
| Enrollments        | `student_id` (select), `course_id` (select), `academic_year` (text), `semester` (select), `status` (select)    |
| Assignments        | `course_id` (select), `title` (text), `description` (textarea), `due_date` (datetime-local)                    |
| Submissions        | `assignment_id` (select), `student_id` (select), `file_path` (text/file), `notes` (textarea), `submitted_at` (datetime-local), `score` (number) |
| Grades             | `student_id` (select), `course_id` (select), `academic_year` (text), `midterm_score` (number), `final_score` (number), `grade_letter` (select) |
| Announcements      | `title` (text), `content` (textarea), `is_published` (checkbox)                                                |
| Tags               | `name` (text), `slug` (text, auto-generate dari name)                                                          |
| Extracurriculars   | `name` (text), `description` (textarea), `max_members` (number)                                                |

---

## TAHAP 10: Buat View — Show (Detail Data)

### 10.1 Contoh View Show — `departments/show.blade.php`

File: `resources/views/departments/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Jurusan')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Jurusan: {{ $department->name }}</h2>
    <a href="{{ route('departments.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card mb-4">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">Kode</th>
                <td>: {{ $department->code }}</td>
            </tr>
            <tr>
                <th>Nama Jurusan</th>
                <td>: {{ $department->name }}</td>
            </tr>
            <tr>
                <th>Deskripsi</th>
                <td>: {{ $department->description ?? '-' }}</td>
            </tr>
            <tr>
                <th>Dibuat pada</th>
                <td>: {{ $department->created_at->format('d M Y H:i') }}</td>
            </tr>
        </table>
    </div>
</div>

{{-- Daftar Dosen di Jurusan ini (Relasi One-to-Many) --}}
<div class="card mb-4">
    <div class="card-header">
        <h5 class="mb-0">Dosen di Jurusan Ini ({{ $department->teachers->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>NIP</th>
                    <th>Nama</th>
                    <th>Spesialisasi</th>
                </tr>
            </thead>
            <tbody>
                @forelse($department->teachers as $teacher)
                <tr>
                    <td>{{ $teacher->nip }}</td>
                    <td>{{ $teacher->user->name }}</td>
                    <td>{{ $teacher->specialization ?? '-' }}</td>
                </tr>
                @empty
                <tr><td colspan="3" class="text-center">Belum ada data.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>

{{-- Daftar Mahasiswa di Jurusan ini (Relasi One-to-Many) --}}
<div class="card mb-4">
    <div class="card-header">
        <h5 class="mb-0">Mahasiswa di Jurusan Ini ({{ $department->students->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>NIM</th>
                    <th>Nama</th>
                    <th>Semester</th>
                </tr>
            </thead>
            <tbody>
                @forelse($department->students as $student)
                <tr>
                    <td>{{ $student->nim }}</td>
                    <td>{{ $student->user->name }}</td>
                    <td>{{ $student->semester }}</td>
                </tr>
                @empty
                <tr><td colspan="3" class="text-center">Belum ada data.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>

{{-- Daftar Mata Kuliah di Jurusan ini (Relasi One-to-Many) --}}
<div class="card">
    <div class="card-header">
        <h5 class="mb-0">Mata Kuliah di Jurusan Ini ({{ $department->courses->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>Kode</th>
                    <th>Nama</th>
                    <th>SKS</th>
                </tr>
            </thead>
            <tbody>
                @forelse($department->courses as $course)
                <tr>
                    <td>{{ $course->code }}</td>
                    <td>{{ $course->name }}</td>
                    <td>{{ $course->credits }}</td>
                </tr>
                @empty
                <tr><td colspan="3" class="text-center">Belum ada data.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>
@endsection
```

### 10.2 Panduan Data yang Ditampilkan di View Show

| Modul              | Data Utama                                                          | Data Relasi yang Ditampilkan                                                     |
|--------------------|----------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Users              | name, email, role, created_at                                        | Profile (phone, address), Teacher/Student data, Announcements list               |
| Profiles           | phone, address, avatar, birth_date                                   | User (name, email)                                                                |
| Departments        | name, code, description                                              | List Teachers, List Students, List Courses                                        |
| Teachers           | nip, specialization                                                  | User (name, email), Department name, List Schedules                               |
| Students           | nim, semester                                                        | User (name, email), Department name, List Courses (enrollments), Extracurriculars |
| Categories         | name, description                                                    | List Courses                                                                      |
| Courses            | code, name, credits, description                                     | Category, Department, Tags (badges), List Schedules, List Assignments             |
| Classrooms         | name, building, capacity                                             | List Schedules                                                                    |
| Schedules          | day, start_time, end_time                                            | Course name, Teacher name, Classroom name                                         |
| Enrollments        | academic_year, semester, status                                      | Student (nim, name), Course (code, name)                                          |
| Assignments        | title, description, due_date                                         | Course name, List Submissions                                                     |
| Submissions        | file_path, notes, submitted_at, score                                | Assignment title, Student (nim, name)                                             |
| Grades             | academic_year, midterm_score, final_score, grade_letter              | Student (nim, name), Course (code, name)                                          |
| Announcements      | title, content, is_published, created_at                             | User (author name)                                                                |
| Tags               | name, slug                                                           | List Courses yang memiliki tag ini                                                |
| Extracurriculars   | name, description, max_members                                       | List Students (members, with role & joined_at)                                    |

---

## TAHAP 11: Buat View — Edit (Update Data)

### 11.1 Contoh View Edit — `departments/edit.blade.php`

File: `resources/views/departments/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Jurusan')

@section('content')
<h2>Edit Jurusan: {{ $department->name }}</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('departments.update', $department) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="name" class="form-label">Nama Jurusan <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name', $department->name) }}" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="code" class="form-label">Kode Jurusan <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('code') is-invalid @enderror"
                       id="code" name="code" value="{{ old('code', $department->code) }}" maxlength="10" required>
                @error('code')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="description" class="form-label">Deskripsi</label>
                <textarea class="form-control @error('description') is-invalid @enderror"
                          id="description" name="description" rows="3">{{ old('description', $department->description) }}</textarea>
                @error('description')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('departments.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

### 11.2 Catatan Penting pada Form Edit

| Aspek                  | Detail                                                                                     |
|------------------------|--------------------------------------------------------------------------------------------|
| Method                 | Gunakan `@method('PUT')` di dalam form karena HTML form hanya support GET dan POST          |
| Old Value              | Gunakan `old('field', $model->field)` untuk menampilkan nilai lama jika validasi gagal      |
| Unique Validation      | Pada validasi unique, exclude ID data yang sedang diedit: `'unique:table,column,' . $model->id` |
| Many-to-Many (Edit)    | Untuk relasi many-to-many (misal tags pada courses), pre-select checkbox yang sudah terpilih menggunakan `$course->tags->pluck('id')->toArray()` |

### 11.3 Contoh Edit Form Many-to-Many — Tags pada `courses/edit.blade.php`

Pada bagian checkbox tags, pre-select tag yang sudah terpilih:

```html
{{-- Many-to-Many: Tags (Checkbox) - dengan pre-selected --}}
<div class="mb-3">
    <label class="form-label">Tags</label>
    <div class="row">
        @foreach($tags as $tag)
            <div class="col-md-3">
                <div class="form-check">
                    <input class="form-check-input" type="checkbox" name="tags[]"
                           value="{{ $tag->id }}" id="tag{{ $tag->id }}"
                           {{ in_array($tag->id, old('tags', $course->tags->pluck('id')->toArray())) ? 'checked' : '' }}>
                    <label class="form-check-label" for="tag{{ $tag->id }}">
                        {{ $tag->name }}
                    </label>
                </div>
            </div>
        @endforeach
    </div>
</div>
```

---

## TAHAP 12: Buat Delete Data

### 12.1 Implementasi Delete

Delete sudah diimplementasikan pada:

1. **View (tombol delete)** — di file `index.blade.php` setiap modul:

```html
@can('admin')
    <form action="{{ route('departments.destroy', $dept) }}" method="POST" class="d-inline"
          onsubmit="return confirm('Yakin ingin menghapus data ini?')">
        @csrf
        @method('DELETE')
        <button type="submit" class="btn btn-sm btn-danger">
            <i class="bi bi-trash"></i> Hapus
        </button>
    </form>
@endcan
```

2. **Controller (method destroy)** — di setiap controller:

```php
public function destroy(Department $department)
{
    Gate::authorize('admin');

    $department->delete();

    return redirect()->route('departments.index')
                     ->with('success', 'Data berhasil dihapus.');
}
```

### 12.2 Catatan Penting Delete

| Aspek                  | Detail                                                                                     |
|------------------------|--------------------------------------------------------------------------------------------|
| Konfirmasi             | Selalu tampilkan dialog konfirmasi `confirm()` sebelum delete                               |
| Cascade Delete         | Karena migration menggunakan `onDelete('cascade')`, data anak akan otomatis terhapus        |
| Gate Authorization     | Method `destroy` harus dicek dengan `Gate::authorize('admin')` agar hanya admin yang bisa menghapus |
| Method Spoofing        | Gunakan `@method('DELETE')` karena HTML form tidak support method DELETE secara native       |
| Many-to-Many Cleanup   | Untuk data yang memiliki relasi many-to-many, Laravel secara otomatis menghapus data pivot jika menggunakan `onDelete('cascade')` |

---

## 📁 Struktur Direktori View Lengkap

```
resources/views/
├── layouts/
│   └── app.blade.php              # Layout utama
├── auth/
│   └── login.blade.php            # Halaman login
├── dashboard.blade.php            # Dashboard
├── users/
│   ├── index.blade.php            # List semua users
│   ├── create.blade.php           # Form tambah user
│   ├── show.blade.php             # Detail user
│   └── edit.blade.php             # Form edit user
├── profiles/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── departments/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── teachers/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── students/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── categories/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── courses/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── classrooms/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── schedules/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── enrollments/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── assignments/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── submissions/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── grades/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── announcements/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
├── tags/
│   ├── index.blade.php
│   ├── create.blade.php
│   ├── show.blade.php
│   └── edit.blade.php
└── extracurriculars/
    ├── index.blade.php
    ├── create.blade.php
    ├── show.blade.php
    └── edit.blade.php
```

---

## ✅ Checklist Penyelesaian Tugas

### Tahap Persiapan
- [ ] Install Laravel project baru
- [ ] Buat database MySQL dan konfigurasi `.env`
- [ ] Test koneksi database berhasil

### Tahap Database (Migration & Model)
- [ ] Buat/modifikasi 18 file migration
- [ ] Jalankan `php artisan migrate` berhasil
- [ ] Buat 16 model dengan relasi Eloquent lengkap
- [ ] Buat DatabaseSeeder dengan data dummy
- [ ] Jalankan `php artisan migrate:fresh --seed` berhasil

### Tahap Autentikasi & Autorisasi
- [ ] Buat `AuthController` (login, logout)
- [ ] Buat view login
- [ ] Setup middleware `auth` di routes
- [ ] Setup Gate `admin` di `AppServiceProvider`
- [ ] Tombol Create/Edit/Delete hanya muncul untuk admin (`@can('admin')`)
- [ ] Method create/store/edit/update/destroy dicek dengan `Gate::authorize('admin')`

### Tahap CRUD per Modul (ulangi untuk 16 modul)
- [ ] Buat Controller dengan 7 method resource
- [ ] Buat route `Route::resource()`
- [ ] Buat view `index.blade.php` — Read (tampil semua data + pagination)
- [ ] Buat view `create.blade.php` — Create (form tambah dengan validasi)
- [ ] Buat view `show.blade.php` — Show (detail data + relasi)
- [ ] Buat view `edit.blade.php` — Edit (form edit dengan old values)
- [ ] Implementasi Delete (tombol + konfirmasi + method destroy)

### Testing
- [ ] Login sebagai **admin** → bisa CRUD semua modul
- [ ] Login sebagai **user** → hanya bisa Read (index & show), tombol C/U/D tersembunyi
- [ ] Akses halaman tanpa login → redirect ke `/login`
- [ ] Validasi form berfungsi (tampil pesan error)
- [ ] Data relasi tampil dengan benar di halaman detail
- [ ] Pagination berfungsi
- [ ] Flash message success muncul setelah create/update/delete

---

## 🔑 Ringkasan Teknologi yang Digunakan

| Komponen            | Teknologi                                |
|---------------------|------------------------------------------|
| Framework           | Laravel 11+                              |
| Bahasa              | PHP 8.2+                                 |
| Database            | MySQL / MariaDB                          |
| ORM                 | Eloquent ORM                             |
| Template Engine     | Blade                                    |
| CSS Framework       | Bootstrap 5 (CDN)                        |
| Icon                | Bootstrap Icons (CDN)                    |
| Authentication      | Laravel Auth (manual, tanpa package)     |
| Authorization       | Gate & `@can` directive                  |
| Routing             | Resource Route (`Route::resource()`)     |

---

## 📝 Catatan Akhir

1. **Urutan pengerjaan disarankan**: mulai dari tabel yang **tidak memiliki foreign key** (departments, categories, classrooms, tags, extracurriculars) → lalu tabel yang **bergantung** pada tabel lain (teachers, students, courses, dll).

2. **Pola semua modul sama**: Setiap modul mengikuti pola **MVC** yang identik. Jika sudah menyelesaikan satu modul (misal Department), modul lain hanya perlu menyesuaikan nama model, field, validasi, dan relasi yang di-load.

3. **Relasi yang wajib ditampilkan**:
   - **One-to-One**: Tampilkan data profil di halaman detail user (dan sebaliknya).
   - **One-to-Many**: Tampilkan daftar child di halaman detail parent (misal daftar dosen di detail jurusan).
   - **Many-to-Many**: Tampilkan data relasi menggunakan checkbox di form create/edit (misal tags di courses, extracurriculars di students).

4. **Semua password dummy**: `password`
