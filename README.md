# 📚 Tugas Laravel: Sistem Manajemen Akademik (SIAKAD)

## Deskripsi Proyek

Membangun **Sistem Informasi Akademik (SIAKAD)** menggunakan Laravel dengan fitur lengkap meliputi:
- CRUD data menggunakan **Eloquent ORM**
- **Autentikasi** (Login & Logout)
- **Middleware** untuk proteksi route
- **Gate & Policy** berdasarkan role (**Admin** dan **User**)
- Relasi antar tabel: **One-to-One**, **One-to-Many**, dan **Many-to-Many**

---

## 🧭 Cara Menggunakan Panduan Ini

Panduan ini disusun **berurutan dari atas ke bawah**. Kerjakan setiap tahap sampai selesai, cek hasilnya, lalu lanjut ke tahap berikutnya.

```
BAGIAN A  Perancangan Sistem   → baca & pahami ERD, role, dan daftar modul
BAGIAN B  TAHAP 1 – 6          → project, database, migration, model, seeder, route
BAGIAN C  TAHAP 7 – 8          → login/logout, Gate, layout, dashboard   (✔ sudah bisa login)
BAGIAN D  TAHAP 9 – 10         → pahami pola CRUD, lalu kerjakan Modul 01 s/d Modul 16
BAGIAN E  TAHAP 11             → uji coba akhir & checklist penyelesaian
LAMPIRAN                       → ringkasan spesifikasi per modul, struktur folder, teknologi
```

| Tahap | Yang Dikerjakan | Tanda Berhasil |
|-------|-----------------|----------------|
| 1 | Install Laravel | Halaman awal Laravel tampil di `http://localhost:8000` |
| 2 | Buat database & atur `.env` | `php artisan migrate` berjalan tanpa error |
| 3 | Buat 18 migration | Semua tabel terbentuk di database |
| 4 | Buat 16 model + relasi | 16 file model ada di `app/Models` |
| 5 | Isi data dummy (seeder) | `php artisan migrate:fresh --seed` berhasil |
| 6 | Daftarkan route | `routes/web.php` berisi route login + 16 resource |
| 7 | Autentikasi & Gate | `AuthController` dan Gate `admin` siap |
| 8 | Dashboard, layout & login | Bisa login sebagai admin, dashboard tampil, bisa logout |
| 9 | Pahami pola CRUD | (cukup dibaca) Paham isi 1 controller + 4 view |
| 10 | Kerjakan 16 modul | Setiap modul lolos checklist **Uji Coba** masing-masing |
| 11 | Uji coba akhir | Semua checklist penyelesaian tercentang |

> 💡 **Tip:** Kode di TAHAP 7–10 sudah lengkap dan siap disalin. Bagian **Penjelasan** di bawah setiap kode menerangkan *mengapa* kode ditulis seperti itu. Bacalah, karena pola yang sama akan terus diulang.

---

## 📑 Daftar Isi

- [🧭 Cara Menggunakan Panduan Ini](#-cara-menggunakan-panduan-ini)
- **[BAGIAN A: Perancangan Sistem](#bagian-a-perancangan-sistem)**
  - [📐 Perancangan Database (ERD)](#-perancangan-database-erd)
  - [🔐 Sistem Autentikasi & Autorisasi](#-sistem-autentikasi--autorisasi)
  - [🛠️ Perancangan Fitur CRUD per Modul](#-perancangan-fitur-crud-per-modul)
- **[BAGIAN B: Persiapan Project & Database](#bagian-b-persiapan-project--database)**
  - [TAHAP 1: Install Laravel Project](#tahap-1-install-laravel-project)
  - [TAHAP 2: Buat Database & Sambungkan ke Project](#tahap-2-buat-database--sambungkan-ke-project)
  - [TAHAP 3: Buat Migration](#tahap-3-buat-migration)
  - [TAHAP 4: Buat Model](#tahap-4-buat-model)
  - [TAHAP 5: Isikan Data Dummy (Seeder & Factory)](#tahap-5-isikan-data-dummy-seeder--factory)
  - [TAHAP 6: Daftarkan Route](#tahap-6-daftarkan-route)
- **[BAGIAN C: Fondasi Aplikasi](#bagian-c-fondasi-aplikasi)**
  - [TAHAP 7: Buat Autentikasi & Gate](#tahap-7-buat-autentikasi--gate)
  - [TAHAP 8: Buat Dashboard, Layout & Halaman Login](#tahap-8-buat-dashboard-layout--halaman-login)
- **[BAGIAN D: Membangun 16 Modul CRUD](#bagian-d-membangun-16-modul-crud)**
  - [TAHAP 9: Pahami Pola CRUD Sebelum Mulai](#tahap-9-pahami-pola-crud-sebelum-mulai)
  - [TAHAP 10: Kerjakan 16 Modul CRUD Satu per Satu](#tahap-10-kerjakan-16-modul-crud-satu-per-satu)
    - [Modul 01: Jurusan (`departments`)](#modul-01-jurusan-departments)
    - [Modul 02: Kategori Mata Kuliah (`categories`)](#modul-02-kategori-mata-kuliah-categories)
    - [Modul 03: Ruang Kelas (`classrooms`)](#modul-03-ruang-kelas-classrooms)
    - [Modul 04: Tag Mata Kuliah (`tags`)](#modul-04-tag-mata-kuliah-tags)
    - [Modul 05: Ekstrakurikuler (`extracurriculars`)](#modul-05-ekstrakurikuler-extracurriculars)
    - [Modul 06: Users (`users`)](#modul-06-users-users)
    - [Modul 07: Profil Pengguna (`profiles`)](#modul-07-profil-pengguna-profiles)
    - [Modul 08: Dosen (`teachers`)](#modul-08-dosen-teachers)
    - [Modul 09: Mahasiswa (`students`)](#modul-09-mahasiswa-students)
    - [Modul 10: Mata Kuliah (`courses`)](#modul-10-mata-kuliah-courses)
    - [Modul 11: Jadwal Perkuliahan (`schedules`)](#modul-11-jadwal-perkuliahan-schedules)
    - [Modul 12: Enrollment (KRS) (`enrollments`)](#modul-12-enrollment-krs-enrollments)
    - [Modul 13: Tugas (`assignments`)](#modul-13-tugas-assignments)
    - [Modul 14: Pengumpulan Tugas (`submissions`)](#modul-14-pengumpulan-tugas-submissions)
    - [Modul 15: Nilai (`grades`)](#modul-15-nilai-grades)
    - [Modul 16: Pengumuman (dengan Policy) (`announcements`)](#modul-16-pengumuman-dengan-policy-announcements)
- **[BAGIAN E: Penyelesaian](#bagian-e-penyelesaian)**
  - [TAHAP 11: Uji Coba Akhir & Checklist Penyelesaian](#tahap-11-uji-coba-akhir--checklist-penyelesaian)
- **[Lampiran](#lampiran)**
  - [Lampiran A: Ringkasan Spesifikasi per Modul](#lampiran-a-ringkasan-spesifikasi-per-modul)
  - [Lampiran B: Struktur Direktori View Lengkap](#lampiran-b-struktur-direktori-view-lengkap)
  - [Lampiran C: Ringkasan Teknologi yang Digunakan](#lampiran-c-ringkasan-teknologi-yang-digunakan)
  - [📝 Catatan Akhir](#-catatan-akhir)

---

# BAGIAN A: Perancangan Sistem

Bagian ini berisi rancangan yang akan dibangun. Belum ada kode yang dikerjakan; pahami dulu tabel, relasi, dan role pengguna.

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
│    users      │───────────────▶│   profiles    │
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
│announcements │  │   teachers   │────▶│ departments  │
│              │  │              │ M:1  │              │
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
                         ▲                     │ M:M (via enrollments)
                         │ M:1                 │
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
┌──────────────┐       1:M            │ course_id    │
│  categories  │──────────┐           │ academic_year│
│              │          │           │ semester     │
│ id           │          ▼           │ status       │
│ name         │   ┌──────────────┐   │ timestamps   │
│ description  │   │   courses    │   └──────────────┘
│ timestamps   │   │              │
└──────────────┘   │ id           │         M:M (via course_tag)
                   │ category_id  │──────────────────┐
                   │ department_id│                   │
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
                   │ assignments  │────────────────▶│ submissions  │
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
│ extracurriculars   │◀────────────────────▶│ extracurricular_student │ (Pivot)
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
| Tabel A     | Tabel B    | Keterangan                        |
|-------------|------------|-----------------------------------|
| `users`     | `profiles` | Setiap user memiliki satu profil  |
| `users`     | `teachers` | Setiap dosen terhubung satu user  |
| `users`     | `students` | Setiap mahasiswa terhubung satu user |

#### 🔸 One-to-Many (1:M)
| Tabel Parent      | Tabel Child      | Keterangan                                    |
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

| Role    | Deskripsi                                                                 |
|---------|---------------------------------------------------------------------------|
| `admin` | Dapat mengakses semua fitur CRUD pada semua modul                        |
| `user`  | Hanya dapat melihat (Read) data, tidak bisa Create, Update, atau Delete  |

### Fitur Autentikasi
- **Login**: Form login dengan email & password
- **Logout**: Tombol logout di navbar
- **Middleware `auth`**: Memproteksi semua halaman agar hanya bisa diakses jika sudah login
- **Gate `admin`**: Membatasi akses Create, Update, Delete hanya untuk role `admin`
- **Policy `AnnouncementPolicy`**: Contoh otorisasi berbasis model pada modul Pengumuman (draft hanya terlihat oleh admin), lihat [Modul 16](#modul-16-pengumuman-dengan-policy-announcements)

---

## 🛠️ Perancangan Fitur CRUD per Modul

### Daftar Modul CRUD

Nomor modul di bawah mengikuti **urutan pengerjaan** di TAHAP 10: dimulai dari tabel tanpa foreign key, lalu tabel yang bergantung pada tabel lain.

| No | Modul | Tabel | Controller | Route Prefix |
|----|-------|-------|------------|--------------|
| 01 | [Jurusan](#modul-01-jurusan-departments) | `departments` | `DepartmentController` | `/departments` |
| 02 | [Kategori Mata Kuliah](#modul-02-kategori-mata-kuliah-categories) | `categories` | `CategoryController` | `/categories` |
| 03 | [Ruang Kelas](#modul-03-ruang-kelas-classrooms) | `classrooms` | `ClassroomController` | `/classrooms` |
| 04 | [Tag Mata Kuliah](#modul-04-tag-mata-kuliah-tags) | `tags` | `TagController` | `/tags` |
| 05 | [Ekstrakurikuler](#modul-05-ekstrakurikuler-extracurriculars) | `extracurriculars` | `ExtracurricularController` | `/extracurriculars` |
| 06 | [Users](#modul-06-users-users) | `users` | `UserController` | `/users` |
| 07 | [Profil Pengguna](#modul-07-profil-pengguna-profiles) | `profiles` | `ProfileController` | `/profiles` |
| 08 | [Dosen](#modul-08-dosen-teachers) | `teachers` | `TeacherController` | `/teachers` |
| 09 | [Mahasiswa](#modul-09-mahasiswa-students) | `students` | `StudentController` | `/students` |
| 10 | [Mata Kuliah](#modul-10-mata-kuliah-courses) | `courses` | `CourseController` | `/courses` |
| 11 | [Jadwal Perkuliahan](#modul-11-jadwal-perkuliahan-schedules) | `schedules` | `ScheduleController` | `/schedules` |
| 12 | [Enrollment (KRS)](#modul-12-enrollment-krs-enrollments) | `enrollments` | `EnrollmentController` | `/enrollments` |
| 13 | [Tugas](#modul-13-tugas-assignments) | `assignments` | `AssignmentController` | `/assignments` |
| 14 | [Pengumpulan Tugas](#modul-14-pengumpulan-tugas-submissions) | `submissions` | `SubmissionController` | `/submissions` |
| 15 | [Nilai](#modul-15-nilai-grades) | `grades` | `GradeController` | `/grades` |
| 16 | [Pengumuman (dengan Policy)](#modul-16-pengumuman-dengan-policy-announcements) | `announcements` | `AnnouncementController` | `/announcements` |
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

# BAGIAN B: Persiapan Project & Database

Menyiapkan project Laravel, database, struktur tabel, model, data dummy, dan route.

## TAHAP 1: Install Laravel Project

### 1.1 Prasyarat
Pastikan sudah terinstall:
- **PHP** >= 8.3 (wajib untuk Laravel 13, versi yang terpasang saat menjalankan `composer create-project`)
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

> Biarkan server tetap berjalan di terminal ini selama mengerjakan panduan, dan gunakan terminal lain untuk perintah `php artisan`. Jika tidak ingin memakai Vite, server juga bisa dijalankan dengan `php artisan serve`.

> ✅ **Cek hasil TAHAP 1:** halaman awal Laravel tampil di browser.

---

## TAHAP 2: Buat Database & Sambungkan ke Project

### 2.1 Buat Database Baru

Masuk ke MySQL dan buat database:

```sql
CREATE DATABASE siakad_laravel;
```

### 2.2 Konfigurasi File `.env`

Buka file `.env` di root project dan ubah konfigurasi database:

> **Catatan Laravel 13:** Secara bawaan `.env` berisi `DB_CONNECTION=sqlite` dan baris `DB_HOST` s/d `DB_PASSWORD` diberi tanda komentar (`#`). Ganti `sqlite` menjadi `mysql` dan **hapus tanda `#`** di depan baris-baris tersebut agar konfigurasinya terbaca.

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

> ✅ **Cek hasil TAHAP 2:** `php artisan migrate` selesai tanpa error dan tabel bawaan Laravel (`users`, `sessions`, `cache`, `jobs`, dll) muncul di database `siakad_laravel`.

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
php artisan migrate:fresh
```

> Gunakan `migrate:fresh` (bukan `migrate`), karena tabel `users` sudah dibuat saat TAHAP 2.3 sebelum kolom `role` ditambahkan. `migrate:fresh` menghapus semua tabel lalu membuat ulang dari awal, sehingga kolom `role` ikut terbentuk.

> ✅ **Cek hasil TAHAP 3:** ke-18 tabel pada daftar di [Perancangan Database](#-perancangan-database-erd) sudah ada di database (ditambah tabel bawaan Laravel), dan tabel `users` memiliki kolom `role`.

---

## TAHAP 4: Buat Model

Buat model untuk setiap tabel beserta relasi Eloquent-nya.

### 4.1 Model `User` (modifikasi model bawaan)

File: `app/Models/User.php`

> **Catatan Laravel 13:** Model bawaan memakai atribut PHP `#[Fillable([...])]` dan `#[Hidden([...])]` di atas nama class. **Ganti seluruh isi file** dengan kode di bawah (yang memakai properti `$fillable` dan `$hidden`) agar kolom `role` ikut bisa diisi.

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

> ✅ **Cek hasil TAHAP 4:** folder `app/Models` berisi 16 model: `User` (dimodifikasi) dan 15 model baru.

---

## TAHAP 5: Isikan Data Dummy (Seeder & Factory)

### 5.1 Ubah Seeder Utama

File `DatabaseSeeder.php` **sudah ada** sejak project dibuat, jadi tidak perlu menjalankan `php artisan make:seeder` (perintah itu akan gagal dengan pesan *Seeder already exists*). Cukup ganti seluruh isinya dengan kode berikut.

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

> ✅ **Cek hasil TAHAP 5:** tabel `users` berisi 16 akun (1 admin, 5 dosen, 10 mahasiswa) dan tabel lain seperti `departments`, `courses`, `grades` sudah terisi data dummy.

---

## TAHAP 6: Daftarkan Route

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

> **Catatan:** route untuk ke-16 modul didaftarkan **sekarang sekaligus**, sehingga tidak perlu menambah route lagi di tahap berikutnya. Controller-nya baru dibuat bertahap di TAHAP 7–10. Selama controller belum ada, membuka menu modul tersebut akan menampilkan error *Target class [...Controller] does not exist*. Ini normal. Penjelasan 7 route yang dihasilkan `Route::resource()` ada di [TAHAP 9.1](#91-tujuh-route-dari-routeresource).

> ✅ **Cek hasil TAHAP 6:** file `routes/web.php` tersimpan. Aplikasi belum bisa dicoba di browser karena `AuthController` dan halaman login baru dibuat di TAHAP 7–8.

---

# BAGIAN C: Fondasi Aplikasi

Membuat autentikasi, Gate, layout, halaman login, dan dashboard. Di akhir bagian ini aplikasi sudah bisa dipakai untuk login dan logout.

## TAHAP 7: Buat Autentikasi & Gate

Tahap ini menyiapkan proses **login/logout** dan aturan otorisasi **Gate `admin`** yang dipakai oleh hampir semua modul.

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

### 7.2 Setup Gate & Pagination di `AppServiceProvider`

File: `app/Providers/AppServiceProvider.php`

```php
<?php

namespace App\Providers;

use Illuminate\Pagination\Paginator;
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

        // Gunakan tampilan pagination Bootstrap 5
        // (bawaan Laravel memakai Tailwind, sehingga tombol halaman tampil berantakan)
        Paginator::useBootstrapFive();
    }
}
```

> ✅ **Cek hasil TAHAP 7:** file `AuthController.php` dan `AppServiceProvider.php` tersimpan. Login baru bisa diuji setelah halaman login dibuat di TAHAP 8.

---

## TAHAP 8: Buat Dashboard, Layout & Halaman Login

Tahap ini membuat halaman pertama yang bisa dibuka di browser: halaman login, layout utama (navbar + flash message), dan dashboard.

Buat file view-nya terlebih dahulu:

```bash
php artisan make:view layouts.app
php artisan make:view auth.login
php artisan make:view dashboard
```

Hapus isi bawaan ketiga file tersebut, lalu isi dengan kode pada langkah-langkah berikut.

### 8.1 Buat `DashboardController`

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

### 8.2 Buat Layout Utama

Layout utama berisi navbar, flash message, dan area konten. Semua halaman lain cukup menulis `@extends('layouts.app')` lalu mengisi `@section('content')`.

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

### 8.3 Buat View Login

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

### 8.4 Buat View Dashboard

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

### 8.5 Uji Coba Fondasi Aplikasi

Pastikan server berjalan, buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Membuka `http://localhost:8000` tanpa login → otomatis diarahkan ke halaman `/login`.
- [ ] Login dengan password salah → muncul pesan *Email atau password salah.*
- [ ] Login sebagai `admin@siakad.com` / `password` → dashboard tampil dengan jumlah user, mahasiswa, dosen, dan mata kuliah. Di navbar tampil nama dan badge **Admin**.
- [ ] Klik nama di navbar → **Logout** → kembali ke halaman login.
- [ ] Login sebagai `mahasiswa1@siakad.com` → badge di navbar berwarna biru (**User**).

> Menu di navbar sudah tampil, tetapi menu modul belum bisa dibuka sampai modulnya dikerjakan di TAHAP 10.

> ✅ **Cek hasil TAHAP 8:** semua poin uji coba di atas berhasil. Fondasi aplikasi selesai, saatnya membangun modul CRUD.

---

# BAGIAN D: Membangun 16 Modul CRUD

Memahami pola CRUD, lalu mengerjakan ke-16 modul satu per satu sampai selesai.

## TAHAP 9: Pahami Pola CRUD Sebelum Mulai

Ke-16 modul di TAHAP 10 dibangun dengan pola yang **sama persis**: **1 controller resource + 4 view** (`index`, `create`, `show`, `edit`). Tahap ini menjelaskan pola tersebut sekali saja dengan contoh modul Jurusan.

> Tidak ada file yang dibuat di tahap ini. Cukup baca dan pahami, karena kode lengkapnya dikerjakan mulai [Modul 01](#modul-01-jurusan-departments).

### 9.1 Tujuh Route dari `Route::resource()`

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

Route `index` dan `show` boleh dibuka **semua user yang login**. Lima route lainnya (`create`, `store`, `edit`, `update`, `destroy`) **khusus admin**.

### 9.2 Pola Controller

Setiap controller dibuat dengan `php artisan make:controller NamaController --resource` dan berisi 7 method berikut:

| Method | Tugas | Hasil | Akses |
|--------|-------|-------|-------|
| `index` | Ambil daftar data dengan `paginate(10)` | Tampilkan view `index` | Semua user login |
| `create` | Siapkan data dropdown (jika ada) | Tampilkan view `create` | Admin |
| `store` | Validasi → simpan data baru | Redirect ke `index` + pesan sukses | Admin |
| `show` | Ambil satu data beserta relasinya | Tampilkan view `show` | Semua user login |
| `edit` | Siapkan data lama + dropdown | Tampilkan view `edit` | Admin |
| `update` | Validasi → perbarui data | Redirect ke `index` + pesan sukses | Admin |
| `destroy` | Hapus data | Redirect ke `index` + pesan sukses | Admin |

Konsep yang dipakai di semua controller:

- **`Gate::authorize('admin')`** di baris pertama `create`, `store`, `edit`, `update`, dan `destroy`. Jika bukan admin, Laravel menampilkan **403 Forbidden**. (Modul 16 memakai **Policy** sebagai pengganti Gate.)
- **Route Model Binding**: parameter seperti `Department $department` otomatis diisi data sesuai ID di URL, atau **404** jika tidak ditemukan.
- **Validasi** dengan `$request->validate([...])`. Saat update, aturan unique mengecualikan data sendiri: `'unique:departments,code,' . $department->id`.
- **Eager loading** `with()` / `load()` dan `withCount()` untuk memuat relasi sekaligus (mencegah masalah *N+1 query*).
- **Flash message**: `redirect()->route('...index')->with('success', '...')` ditampilkan otomatis oleh layout.

> Contoh lengkap beserta penjelasan per baris: [Modul 01 → Langkah 1](#modul-01-jurusan-departments).

### 9.3 Pola View

| View | Isi Halaman | Bagian Wajib |
|------|-------------|--------------|
| `index.blade.php` | Tabel daftar data + tombol aksi + pagination | `@can('admin')` untuk tombol Tambah/Edit/Hapus, nomor `$data->firstItem() + $i`, `@forelse ... @empty`, `{{ $data->links() }}` |
| `create.blade.php` | Form tambah data | `@csrf`, `old('field')`, `@error('field')` + class `is-invalid` |
| `show.blade.php` | Detail data + tabel data relasi | Akses relasi (`$department->teachers`), `@forelse ... @empty` |
| `edit.blade.php` | Form ubah data | `@csrf`, `@method('PUT')`, `old('field', $model->field)` |

Catatan penting:

- `@can('admin')` **hanya menyembunyikan tombol**. Perlindungan sesungguhnya tetap `Gate::authorize()` di controller, karena user bisa mengetik URL secara manual.
- **Dropdown relasi** (foreign key): `<option value="{{ $item->id }}" {{ old('x_id') == $item->id ? 'selected' : '' }}>`. Contoh: [Modul 08: Dosen](#modul-08-dosen-teachers).
- **Checkbox Many-to-Many**: `name="tags[]"` dan `in_array($tag->id, old('tags', [...]))`. Contoh: [Modul 09: Mahasiswa](#modul-09-mahasiswa-students) dan [Modul 10: Mata Kuliah](#modul-10-mata-kuliah-courses).
- **Upload file**: form wajib memakai `enctype="multipart/form-data"`. Contoh: [Modul 07: Profil Pengguna](#modul-07-profil-pengguna-profiles).

#### Catatan Form Edit

| Aspek                  | Detail                                                                                     |
|------------------------|--------------------------------------------------------------------------------------------|
| Method                 | Gunakan `@method('PUT')` di dalam form karena HTML form hanya support GET dan POST          |
| Old Value              | Gunakan `old('field', $model->field)` untuk menampilkan nilai lama jika validasi gagal      |
| Unique Validation      | Pada validasi unique, exclude ID data yang sedang diedit: `'unique:table,column,' . $model->id` |
| Many-to-Many (Edit)    | Untuk relasi many-to-many (misal tags pada courses), pre-select checkbox yang sudah terpilih menggunakan `$course->tags->pluck('id')->toArray()` |

### 9.4 Pola Delete

Hapus data tidak memiliki view sendiri. Fitur ini terdiri dari dua bagian yang sudah termasuk di kode setiap modul:

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

#### Catatan Penting Delete

| Aspek                  | Detail                                                                                     |
|------------------------|--------------------------------------------------------------------------------------------|
| Konfirmasi             | Selalu tampilkan dialog konfirmasi `confirm()` sebelum delete                               |
| Cascade Delete         | Karena migration menggunakan `onDelete('cascade')`, data anak akan otomatis terhapus        |
| Gate Authorization     | Method `destroy` harus dicek dengan `Gate::authorize('admin')` agar hanya admin yang bisa menghapus |
| Method Spoofing        | Gunakan `@method('DELETE')` karena HTML form tidak support method DELETE secara native       |
| Many-to-Many Cleanup   | Untuk data yang memiliki relasi many-to-many, Laravel secara otomatis menghapus data pivot jika menggunakan `onDelete('cascade')` |

### 9.5 Urutan Pengerjaan Modul

Modul diurutkan dari yang paling sederhana (tanpa foreign key) ke yang paling kompleks. Klik nama modul untuk langsung menuju kodenya.

| No | Modul | Materi Utama |
|----|-------|--------------|
| 01 | [Jurusan (`departments`)](#modul-01-jurusan-departments) | Pola dasar CRUD, Gate, `withCount`, One-to-Many |
| 02 | [Kategori Mata Kuliah (`categories`)](#modul-02-kategori-mata-kuliah-categories) | Latihan mengulang pola CRUD, `Str::limit` |
| 03 | [Ruang Kelas (`classrooms`)](#modul-03-ruang-kelas-classrooms) | Validasi angka, format kolom `TIME` |
| 04 | [Tag Mata Kuliah (`tags`)](#modul-04-tag-mata-kuliah-tags) | Slug otomatis, `$request->merge()` |
| 05 | [Ekstrakurikuler (`extracurriculars`)](#modul-05-ekstrakurikuler-extracurriculars) | Kolom tabel pivot (`->pivot`), validasi dinamis |
| 06 | [Users (`users`)](#modul-06-users-users) | Hash password, `confirmed`, relasi One-to-One |
| 07 | [Profil Pengguna (`profiles`)](#modul-07-profil-pengguna-profiles) | One-to-One, **upload file**, `storage:link` |
| 08 | [Dosen (`teachers`)](#modul-08-dosen-teachers) | Dropdown relasi, query `doesntHave` + `when` |
| 09 | [Mahasiswa (`students`)](#modul-09-mahasiswa-students) | **Checkbox Many-to-Many**, `attach`, `sync` |
| 10 | [Mata Kuliah (`courses`)](#modul-10-mata-kuliah-courses) | Many-to-Many tags, detail dengan banyak relasi |
| 11 | [Jadwal Perkuliahan (`schedules`)](#modul-11-jadwal-perkuliahan-schedules) | Tiga foreign key, validasi jam |
| 12 | [Enrollment (KRS) (`enrollments`)](#modul-12-enrollment-krs-enrollments) | CRUD tabel pivot, `Rule::unique` gabungan |
| 13 | [Tugas (`assignments`)](#modul-13-tugas-assignments) | Input `datetime-local`, cast `datetime` |
| 14 | [Pengumpulan Tugas (`submissions`)](#modul-14-pengumpulan-tugas-submissions) | Upload dokumen, nilai default |
| 15 | [Nilai (`grades`)](#modul-15-nilai-grades) | Hitung nilai huruf otomatis, `match` |
| 16 | [Pengumuman (dengan Policy) (`announcements`)](#modul-16-pengumuman-dengan-policy-announcements) | **Policy**, `$request->boolean()`, mencegah XSS |

### 9.6 Alur Kerja Setiap Modul

```bash
# 1. Buat controller resource
php artisan make:controller NamaController --resource

# 2. Buat 4 file view
php artisan make:view namamodul.index
php artisan make:view namamodul.create
php artisan make:view namamodul.show
php artisan make:view namamodul.edit

# 3. Salin kode dari bagian modul di bawah, lalu jalankan server dan uji coba
php artisan serve
```

> **Catatan:**
> - Route semua modul sudah didaftarkan sekaligus di TAHAP 6, jadi tidak perlu menambah route lagi.
> - Beberapa halaman berisi link ke modul lain (misalnya nama mata kuliah di detail kategori). Link tersebut baru bisa diklik setelah modul tujuannya dikerjakan. Jika diklik lebih awal, akan muncul error *Target class [...Controller] does not exist*. Ini normal.
> - Jika ingin mengulang data dari awal, jalankan `php artisan migrate:fresh --seed`.
> - Seluruh kode di TAHAP 10 sudah diuji pada **Laravel 13 + MySQL 8**: semua halaman dan aksi CRUD untuk admin, pembatasan akses untuk user biasa, validasi, upload file, dan relasi Many-to-Many.

---

## TAHAP 10: Kerjakan 16 Modul CRUD Satu per Satu

Kerjakan modul **berurutan dari Modul 01 sampai Modul 16** (lihat [urutan pengerjaan](#95-urutan-pengerjaan-modul)). Setiap modul berisi bagian yang sama:

1. Tujuan belajar dan relasi tabel yang dipakai
2. Perintah `php artisan` yang perlu dijalankan
3. Kode lengkap **controller** beserta penjelasannya
4. Kode lengkap 4 **view**: `index`, `create`, `show`, `edit`
5. Checklist **uji coba** (sebagai admin dan sebagai user biasa)
6. Tantangan opsional untuk latihan

Sebuah modul dianggap **selesai** jika semua poin di bagian **Uji Coba**-nya berhasil. Setelah itu lanjut ke modul berikutnya lewat link navigasi ➡ di akhir modul.

---

### Modul 01: Jurusan (`departments`)

[⬅ TAHAP 9: Pola CRUD](#tahap-9-pahami-pola-crud-sebelum-mulai) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 02: Kategori Mata Kuliah ➡](#modul-02-kategori-mata-kuliah-categories)

Modul pertama dan paling sederhana. Tabel `departments` tidak memiliki foreign key, tetapi menjadi **induk** (parent) bagi `teachers`, `students`, dan `courses`.

> Modul ini adalah **modul acuan**: pola yang dijelaskan di [TAHAP 9](#tahap-9-pahami-pola-crud-sebelum-mulai) diambil dari kode modul ini. Kerjakan dengan teliti dan baca setiap bagian **Penjelasan**.

Pola di modul ini akan diulang di **semua** modul berikutnya, jadi pahami baik-baik setiap bagiannya.

#### 🎯 Yang Dipelajari

- Pola dasar controller *resource* (7 method: `index`, `create`, `store`, `show`, `edit`, `update`, `destroy`)
- Membatasi aksi tulis hanya untuk admin dengan `Gate::authorize('admin')` dan `@can('admin')`
- Validasi `unique` saat tambah dan saat edit (mengecualikan ID data sendiri)
- `withCount()` untuk menghitung jumlah data relasi
- Menampilkan relasi One-to-Many di halaman detail

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| One-to-Many | `departments` → `teachers` | Satu jurusan memiliki banyak dosen |
| One-to-Many | `departments` → `students` | Satu jurusan memiliki banyak mahasiswa |
| One-to-Many | `departments` → `courses` | Satu jurusan memiliki banyak mata kuliah |

#### 📋 Sebelum Mulai

- TAHAP 1–8 sudah selesai (migration, model, seeder, route, `AuthController`, Gate, layout, login, dashboard) dan uji coba di TAHAP 8.5 berhasil.
- Sudah membaca pola CRUD di TAHAP 9.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/DepartmentController.php` | Controller (logika CRUD) |
| `resources/views/departments/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/departments/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/departments/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/departments/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller DepartmentController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

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

**Penjelasan:**

- `Gate::authorize('admin')` dipanggil di awal `create`, `store`, `edit`, `update`, dan `destroy`. Jika yang login bukan admin, Laravel otomatis menampilkan halaman **403 Forbidden**. Method `index` dan `show` tidak memakainya, sehingga semua user yang login bisa membaca data.
- Parameter `Department $department` adalah *Route Model Binding*: Laravel otomatis mencari data berdasarkan ID di URL (misal `/departments/3`) dan menampilkan **404** jika tidak ditemukan.
- `withCount(['teachers', 'students', 'courses'])` menambahkan atribut `teachers_count`, `students_count`, dan `courses_count` pada setiap jurusan tanpa perlu memuat seluruh data relasinya.
- `paginate(10)` membagi data menjadi 10 baris per halaman.
- `$department->load([...])` (*eager loading*) memuat relasi sekaligus agar tidak terjadi masalah *N+1 query*. `teachers.user` artinya: muat dosen, lalu muat data user milik setiap dosen (karena nama dosen ada di tabel `users`).
- Pada `update`, aturan `'unique:departments,code,' . $department->id` mengecualikan data yang sedang diedit, sehingga menyimpan ulang dengan kode yang sama tidak dianggap duplikat.

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view departments.index
php artisan make:view departments.create
php artisan make:view departments.show
php artisan make:view departments.edit
```

Perintah di atas membuat folder `resources/views/departments/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

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

**Penjelasan:**

- `@can('admin') ... @endcan` menyembunyikan tombol Tambah, Edit, dan Hapus dari user biasa. Ini hanya soal tampilan; perlindungan sesungguhnya tetap `Gate::authorize()` di controller.
- Nomor urut ditulis `$departments->firstItem() + $i` agar tetap berlanjut di halaman 2, 3, dan seterusnya.
- Form hapus memakai `@method('DELETE')` (HTML hanya mengenal GET dan POST) dan `onsubmit="return confirm(...)"` untuk konfirmasi.
- `{{ $departments->links() }}` menampilkan tombol halaman (pagination).

#### Langkah 4: View Create (Create: form tambah data)

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

**Penjelasan:**

- `@csrf` **wajib** ada di setiap form POST. Tanpa itu Laravel menolak request dengan error **419 Page Expired**.
- `old('name')` mengisi kembali input jika validasi gagal, sehingga user tidak perlu mengetik ulang.
- `@error('name') ... @enderror` bersama class `is-invalid` menampilkan pesan error tepat di bawah input yang salah.

#### Langkah 5: View Show (Read: detail data + relasi)

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

**Penjelasan:**

- Tiga tabel di bawah informasi utama adalah contoh relasi One-to-Many: `$department->teachers`, `$department->students`, dan `$department->courses`.
- `@forelse ... @empty ... @endforelse` menampilkan baris "Belum ada data." jika relasinya kosong.

#### Langkah 6: View Edit (Update: form ubah data)

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

**Penjelasan:**

- `@method('PUT')` diperlukan karena form HTML hanya mendukung GET dan POST.
- `old('name', $department->name)` memakai input terakhir jika validasi gagal; jika tidak, memakai nilai dari database.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Login sebagai admin (`admin@siakad.com` / `password`), buka menu **Jurusan** → tampil 5 jurusan beserta jumlah dosen, mahasiswa, dan mata kuliah.
- [ ] Klik **Tambah Jurusan**, kosongkan semua input lalu simpan → muncul pesan error di bawah input.
- [ ] Isi Nama `Teknik Sipil` dan Kode `TS` → kembali ke daftar dengan pesan hijau *Department berhasil ditambahkan.*
- [ ] Tambah lagi dengan kode `TI` → muncul error *The code has already been taken.*
- [ ] Klik **Detail** pada Teknik Informatika → tampil daftar dosen, mahasiswa, dan mata kuliah jurusan tersebut.
- [ ] Klik **Edit** pada Teknik Sipil, ubah namanya tanpa mengubah kode → tersimpan tanpa error duplikat.
- [ ] Klik **Hapus** pada Teknik Sipil → muncul konfirmasi, lalu data terhapus.
- [ ] Logout, login sebagai `mahasiswa1@siakad.com` → tombol Tambah, Edit, dan Hapus tidak tampil. Ketik manual URL `http://localhost:8000/departments/create` → muncul **403 Forbidden**.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Tambahkan kotak pencarian nama jurusan di halaman index (petunjuk: `Department::where('name', 'like', '%' . $request->search . '%')` dan `->withQueryString()` pada paginator).
- Pesan validasi bawaan Laravel berbahasa Inggris. Jalankan `php artisan lang:publish`, salin folder `lang/en` menjadi `lang/id`, terjemahkan isi `validation.php`, lalu ubah `APP_LOCALE=id` di `.env`.

[⬅ TAHAP 9: Pola CRUD](#tahap-9-pahami-pola-crud-sebelum-mulai) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 02: Kategori Mata Kuliah ➡](#modul-02-kategori-mata-kuliah-categories)

---

### Modul 02: Kategori Mata Kuliah (`categories`)

[⬅ Modul 01: Jurusan](#modul-01-jurusan-departments) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 03: Ruang Kelas ➡](#modul-03-ruang-kelas-classrooms)

Kategori mata kuliah (Pemrograman, Basis Data, Jaringan, dll). Strukturnya mirip Jurusan, jadi modul ini adalah latihan untuk **mengulang pola CRUD dasar** secara mandiri.

#### 🎯 Yang Dipelajari

- Mengulang pola CRUD dari Modul 01
- `withCount('courses')` dan `orderBy()`
- `Str::limit()` untuk memotong teks panjang di tabel
- Relasi bertingkat di halaman detail (`courses.department`)

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| One-to-Many | `categories` → `courses` | Satu kategori memiliki banyak mata kuliah |

#### 📋 Sebelum Mulai

- Modul 01 (Jurusan) sudah selesai dan berjalan.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/CategoryController.php` | Controller (logika CRUD) |
| `resources/views/categories/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/categories/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/categories/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/categories/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller CategoryController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/CategoryController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Category;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;

class CategoryController extends Controller
{
    /**
     * Tampilkan semua kategori beserta jumlah mata kuliahnya.
     */
    public function index()
    {
        $categories = Category::withCount('courses')->orderBy('name')->paginate(10);
        return view('categories.index', compact('categories'));
    }

    /**
     * Tampilkan form tambah kategori (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');
        return view('categories.create');
    }

    /**
     * Simpan kategori baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'description' => 'nullable|string',
        ]);

        Category::create($validated);

        return redirect()->route('categories.index')
                         ->with('success', 'Kategori berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail kategori beserta daftar mata kuliahnya (One-to-Many).
     */
    public function show(Category $category)
    {
        $category->load('courses.department');
        return view('categories.show', compact('category'));
    }

    /**
     * Tampilkan form edit kategori (khusus admin).
     */
    public function edit(Category $category)
    {
        Gate::authorize('admin');
        return view('categories.edit', compact('category'));
    }

    /**
     * Update data kategori (khusus admin).
     */
    public function update(Request $request, Category $category)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'description' => 'nullable|string',
        ]);

        $category->update($validated);

        return redirect()->route('categories.index')
                         ->with('success', 'Kategori berhasil diupdate.');
    }

    /**
     * Hapus kategori (khusus admin).
     * Mata kuliah di kategori ini ikut terhapus karena onDelete('cascade').
     */
    public function destroy(Category $category)
    {
        Gate::authorize('admin');

        $category->delete();

        return redirect()->route('categories.index')
                         ->with('success', 'Kategori berhasil dihapus.');
    }
}
```

**Penjelasan:**

- `withCount('courses')` menambahkan atribut `courses_count`, sedangkan `orderBy('name')` mengurutkan kategori berdasarkan abjad sebelum dipaginasi.
- `load('courses.department')` memuat mata kuliah sekaligus jurusan setiap mata kuliah, karena nama jurusan ditampilkan di halaman detail.
- Foreign key `courses.category_id` memakai `onDelete('cascade')`, sehingga **menghapus kategori ikut menghapus semua mata kuliah di dalamnya**. Karena itu pesan konfirmasi hapus di view diberi peringatan.

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view categories.index
php artisan make:view categories.create
php artisan make:view categories.show
php artisan make:view categories.edit
```

Perintah di atas membuat folder `resources/views/categories/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/categories/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Kategori')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Kategori Mata Kuliah</h2>
    @can('admin')
        <a href="{{ route('categories.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Kategori
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Nama Kategori</th>
            <th>Deskripsi</th>
            <th>Jumlah Mata Kuliah</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($categories as $i => $category)
        <tr>
            <td>{{ $categories->firstItem() + $i }}</td>
            <td>{{ $category->name }}</td>
            <td>{{ Str::limit($category->description ?? '-', 50) }}</td>
            <td>{{ $category->courses_count }}</td>
            <td>
                <a href="{{ route('categories.show', $category) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('categories.edit', $category) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('categories.destroy', $category) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus? Semua mata kuliah di kategori ini ikut terhapus.')">
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
            <td colspan="5" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $categories->links() }}
@endsection
```

**Penjelasan:**

- `Str::limit($category->description ?? '-', 50)` memotong deskripsi maksimal 50 karakter. Class `Str` bisa langsung dipakai di Blade tanpa `use`.

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/categories/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Kategori')

@section('content')
<h2>Tambah Kategori Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('categories.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="name" class="form-label">Nama Kategori <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name') }}" required>
                @error('name')
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
                <a href="{{ route('categories.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Sama seperti form Jurusan: `@csrf`, `old()`, dan `@error`.

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/categories/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Kategori')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Kategori: {{ $category->name }}</h2>
    <a href="{{ route('categories.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card mb-4">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">Nama Kategori</th>
                <td>: {{ $category->name }}</td>
            </tr>
            <tr>
                <th>Deskripsi</th>
                <td>: {{ $category->description ?? '-' }}</td>
            </tr>
            <tr>
                <th>Dibuat pada</th>
                <td>: {{ $category->created_at->format('d M Y H:i') }}</td>
            </tr>
        </table>
    </div>
</div>

{{-- Daftar Mata Kuliah di Kategori ini (Relasi One-to-Many) --}}
<div class="card">
    <div class="card-header">
        <h5 class="mb-0">Mata Kuliah di Kategori Ini ({{ $category->courses->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>Kode</th>
                    <th>Nama Mata Kuliah</th>
                    <th>SKS</th>
                    <th>Jurusan</th>
                </tr>
            </thead>
            <tbody>
                @forelse($category->courses as $course)
                <tr>
                    <td>{{ $course->code }}</td>
                    <td><a href="{{ route('courses.show', $course) }}">{{ $course->name }}</a></td>
                    <td>{{ $course->credits }}</td>
                    <td>{{ $course->department->name }}</td>
                </tr>
                @empty
                <tr><td colspan="4" class="text-center">Belum ada data.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Nama mata kuliah dibuat sebagai link ke `route('courses.show', $course)`. Link ini baru bisa diklik setelah **Modul 10 (Mata Kuliah)** selesai.

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/categories/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Kategori')

@section('content')
<h2>Edit Kategori: {{ $category->name }}</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('categories.update', $category) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="name" class="form-label">Nama Kategori <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name', $category->name) }}" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="description" class="form-label">Deskripsi</label>
                <textarea class="form-control @error('description') is-invalid @enderror"
                          id="description" name="description" rows="3">{{ old('description', $category->description) }}</textarea>
                @error('description')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('categories.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Sama seperti form create, ditambah `@method('PUT')` dan nilai lama `old('name', $category->name)`.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Lainnya → Kategori** → tampil 5 kategori dengan jumlah mata kuliahnya.
- [ ] Simpan form tambah dengan nama kosong → muncul error.
- [ ] Tambah kategori `Desain` → muncul di daftar dengan jumlah mata kuliah 0.
- [ ] Klik **Detail** pada **Pemrograman** → tampil daftar mata kuliah (Pemrograman Web, Framework Laravel, dll).
- [ ] Edit kategori `Desain` menjadi `Desain Grafis`, lalu hapus.
- [ ] Login sebagai user biasa → hanya bisa melihat daftar dan detail.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Tampilkan total SKS seluruh mata kuliah di halaman detail kategori (petunjuk: `$category->courses->sum('credits')`).

[⬅ Modul 01: Jurusan](#modul-01-jurusan-departments) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 03: Ruang Kelas ➡](#modul-03-ruang-kelas-classrooms)

---

### Modul 03: Ruang Kelas (`classrooms`)

[⬅ Modul 02: Kategori Mata Kuliah](#modul-02-kategori-mata-kuliah-categories) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 04: Tag Mata Kuliah ➡](#modul-04-tag-mata-kuliah-tags)

Data ruang kelas. Ruangan dipakai oleh tabel `schedules`, sehingga halaman detail ruangan menampilkan jadwal kuliah yang memakai ruangan tersebut.

#### 🎯 Yang Dipelajari

- Validasi angka: `integer|min:1`
- Atribut `placeholder` dan nilai default input (`old('capacity', 30)`)
- Relasi bertingkat: jadwal → mata kuliah & dosen → user
- Memformat kolom `TIME` dari `08:00:00` menjadi `08:00`

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| One-to-Many | `classrooms` → `schedules` | Satu ruangan dipakai oleh banyak jadwal |

#### 📋 Sebelum Mulai

- Modul 01 sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/ClassroomController.php` | Controller (logika CRUD) |
| `resources/views/classrooms/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/classrooms/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/classrooms/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/classrooms/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller ClassroomController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/ClassroomController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Classroom;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;

class ClassroomController extends Controller
{
    /**
     * Tampilkan semua ruang kelas beserta jumlah jadwal yang memakainya.
     */
    public function index()
    {
        $classrooms = Classroom::withCount('schedules')->orderBy('building')->orderBy('name')->paginate(10);
        return view('classrooms.index', compact('classrooms'));
    }

    /**
     * Tampilkan form tambah ruang kelas (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');
        return view('classrooms.create');
    }

    /**
     * Simpan ruang kelas baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'building' => 'required|string|max:255',
            'capacity' => 'required|integer|min:1',
        ]);

        Classroom::create($validated);

        return redirect()->route('classrooms.index')
                         ->with('success', 'Ruang kelas berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail ruang kelas beserta jadwal yang memakainya (One-to-Many).
     */
    public function show(Classroom $classroom)
    {
        $classroom->load(['schedules.course', 'schedules.teacher.user']);
        return view('classrooms.show', compact('classroom'));
    }

    /**
     * Tampilkan form edit ruang kelas (khusus admin).
     */
    public function edit(Classroom $classroom)
    {
        Gate::authorize('admin');
        return view('classrooms.edit', compact('classroom'));
    }

    /**
     * Update data ruang kelas (khusus admin).
     */
    public function update(Request $request, Classroom $classroom)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'building' => 'required|string|max:255',
            'capacity' => 'required|integer|min:1',
        ]);

        $classroom->update($validated);

        return redirect()->route('classrooms.index')
                         ->with('success', 'Ruang kelas berhasil diupdate.');
    }

    /**
     * Hapus ruang kelas (khusus admin).
     * Jadwal yang memakai ruangan ini ikut terhapus karena onDelete('cascade').
     */
    public function destroy(Classroom $classroom)
    {
        Gate::authorize('admin');

        $classroom->delete();

        return redirect()->route('classrooms.index')
                         ->with('success', 'Ruang kelas berhasil dihapus.');
    }
}
```

**Penjelasan:**

- `withCount('schedules')` menghasilkan `schedules_count`. `orderBy('building')->orderBy('name')` mengurutkan per gedung, lalu per nama ruangan.
- `load(['schedules.course', 'schedules.teacher.user'])`: nama dosen tersimpan di tabel `users`, sehingga harus dimuat lewat `teacher.user`.
- Menghapus ruangan ikut menghapus jadwal yang memakainya (cascade).

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view classrooms.index
php artisan make:view classrooms.create
php artisan make:view classrooms.show
php artisan make:view classrooms.edit
```

Perintah di atas membuat folder `resources/views/classrooms/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/classrooms/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Ruang Kelas')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Ruang Kelas</h2>
    @can('admin')
        <a href="{{ route('classrooms.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Ruangan
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Nama Ruangan</th>
            <th>Gedung</th>
            <th>Kapasitas</th>
            <th>Jumlah Jadwal</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($classrooms as $i => $classroom)
        <tr>
            <td>{{ $classrooms->firstItem() + $i }}</td>
            <td>{{ $classroom->name }}</td>
            <td>{{ $classroom->building }}</td>
            <td>{{ $classroom->capacity }} orang</td>
            <td>{{ $classroom->schedules_count }}</td>
            <td>
                <a href="{{ route('classrooms.show', $classroom) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('classrooms.edit', $classroom) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('classrooms.destroy', $classroom) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus? Jadwal di ruangan ini ikut terhapus.')">
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
            <td colspan="6" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $classrooms->links() }}
@endsection
```

**Penjelasan:**

- Kolom kapasitas ditambah teks `orang` agar lebih jelas.

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/classrooms/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Ruang Kelas')

@section('content')
<h2>Tambah Ruang Kelas Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('classrooms.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="name" class="form-label">Nama Ruangan <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name') }}" placeholder="contoh: Lab Komputer 3" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="building" class="form-label">Gedung <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('building') is-invalid @enderror"
                       id="building" name="building" value="{{ old('building') }}" placeholder="contoh: Gedung A" required>
                @error('building')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="capacity" class="form-label">Kapasitas <span class="text-danger">*</span></label>
                <input type="number" class="form-control @error('capacity') is-invalid @enderror"
                       id="capacity" name="capacity" value="{{ old('capacity', 30) }}" min="1" required>
                @error('capacity')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('classrooms.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- `placeholder` memberi contoh isian. `old('capacity', 30)` mengisi nilai awal 30 saat form pertama kali dibuka.
- `min="1"` pada input number hanya membantu di browser; validasi sesungguhnya tetap `min:1` di controller.

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/classrooms/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Ruang Kelas')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Ruangan: {{ $classroom->name }}</h2>
    <a href="{{ route('classrooms.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card mb-4">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">Nama Ruangan</th>
                <td>: {{ $classroom->name }}</td>
            </tr>
            <tr>
                <th>Gedung</th>
                <td>: {{ $classroom->building }}</td>
            </tr>
            <tr>
                <th>Kapasitas</th>
                <td>: {{ $classroom->capacity }} orang</td>
            </tr>
        </table>
    </div>
</div>

{{-- Daftar Jadwal di Ruangan ini (Relasi One-to-Many) --}}
<div class="card">
    <div class="card-header">
        <h5 class="mb-0">Jadwal di Ruangan Ini ({{ $classroom->schedules->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>Hari</th>
                    <th>Jam</th>
                    <th>Mata Kuliah</th>
                    <th>Dosen</th>
                </tr>
            </thead>
            <tbody>
                @forelse($classroom->schedules as $schedule)
                <tr>
                    <td>{{ $schedule->day }}</td>
                    <td>{{ substr($schedule->start_time, 0, 5) }} - {{ substr($schedule->end_time, 0, 5) }}</td>
                    <td>{{ $schedule->course->name }}</td>
                    <td>{{ $schedule->teacher->user->name }}</td>
                </tr>
                @empty
                <tr><td colspan="4" class="text-center">Belum ada jadwal.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Kolom `TIME` dari MySQL berbentuk `08:00:00`. `substr($schedule->start_time, 0, 5)` mengambil 5 karakter pertama sehingga tampil `08:00`.

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/classrooms/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Ruang Kelas')

@section('content')
<h2>Edit Ruangan: {{ $classroom->name }}</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('classrooms.update', $classroom) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="name" class="form-label">Nama Ruangan <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name', $classroom->name) }}" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="building" class="form-label">Gedung <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('building') is-invalid @enderror"
                       id="building" name="building" value="{{ old('building', $classroom->building) }}" required>
                @error('building')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="capacity" class="form-label">Kapasitas <span class="text-danger">*</span></label>
                <input type="number" class="form-control @error('capacity') is-invalid @enderror"
                       id="capacity" name="capacity" value="{{ old('capacity', $classroom->capacity) }}" min="1" required>
                @error('capacity')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('classrooms.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Sama seperti form create dengan nilai lama dari database.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Lainnya → Ruangan** → tampil 6 ruangan.
- [ ] Tambah ruangan dengan kapasitas `0` → muncul error *must be at least 1*.
- [ ] Tambah `Lab 9`, `Gedung D`, kapasitas 25 → berhasil.
- [ ] Klik **Detail** pada **Lab Komputer 1** → tampil jadwal yang memakai ruangan tersebut.
- [ ] Edit kapasitas `Lab 9`, lalu hapus.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Beri badge merah di kolom kapasitas jika kapasitas ruangan kurang dari 30.

[⬅ Modul 02: Kategori Mata Kuliah](#modul-02-kategori-mata-kuliah-categories) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 04: Tag Mata Kuliah ➡](#modul-04-tag-mata-kuliah-tags)

---

### Modul 04: Tag Mata Kuliah (`tags`)

[⬅ Modul 03: Ruang Kelas](#modul-03-ruang-kelas-classrooms) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 05: Ekstrakurikuler ➡](#modul-05-ekstrakurikuler-extracurriculars)

Tag untuk mata kuliah (Backend, Frontend, Wajib, Pilihan, dll). Tag berelasi **Many-to-Many** dengan mata kuliah melalui tabel pivot `course_tag`. Modul ini juga mengajarkan cara membuat **slug** otomatis.

#### 🎯 Yang Dipelajari

- Membuat slug otomatis dengan `Str::slug()`
- Mengubah input sebelum divalidasi dengan `$request->merge()`
- Sisi kebalikan relasi Many-to-Many (`$tag->courses`)

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| Many-to-Many | `tags` ↔ `courses` (pivot `course_tag`) | Satu tag dipakai banyak mata kuliah, satu mata kuliah punya banyak tag |

#### 📋 Sebelum Mulai

- Modul 01 sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/TagController.php` | Controller (logika CRUD) |
| `resources/views/tags/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/tags/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/tags/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/tags/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller TagController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/TagController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Tag;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;
use Illuminate\Support\Str;

class TagController extends Controller
{
    /**
     * Tampilkan semua tag beserta jumlah mata kuliah yang memakainya.
     */
    public function index()
    {
        $tags = Tag::withCount('courses')->orderBy('name')->paginate(10);
        return view('tags.index', compact('tags'));
    }

    /**
     * Tampilkan form tambah tag (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');
        return view('tags.create');
    }

    /**
     * Simpan tag baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        // Jika slug dikosongkan, buat otomatis dari nama.
        // Contoh: "AI/ML" -> "aiml", "Basis Data" -> "basis-data"
        $request->merge([
            'slug' => Str::slug($request->input('slug') ?: $request->input('name')),
        ]);

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'slug' => 'required|string|max:255|unique:tags,slug',
        ]);

        Tag::create($validated);

        return redirect()->route('tags.index')
                         ->with('success', 'Tag berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail tag beserta mata kuliah yang memakainya (Many-to-Many).
     */
    public function show(Tag $tag)
    {
        $tag->load('courses.department');
        return view('tags.show', compact('tag'));
    }

    /**
     * Tampilkan form edit tag (khusus admin).
     */
    public function edit(Tag $tag)
    {
        Gate::authorize('admin');
        return view('tags.edit', compact('tag'));
    }

    /**
     * Update data tag (khusus admin).
     */
    public function update(Request $request, Tag $tag)
    {
        Gate::authorize('admin');

        $request->merge([
            'slug' => Str::slug($request->input('slug') ?: $request->input('name')),
        ]);

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'slug' => 'required|string|max:255|unique:tags,slug,' . $tag->id,
        ]);

        $tag->update($validated);

        return redirect()->route('tags.index')
                         ->with('success', 'Tag berhasil diupdate.');
    }

    /**
     * Hapus tag (khusus admin).
     * Baris di tabel pivot course_tag ikut terhapus karena onDelete('cascade'),
     * sedangkan data mata kuliahnya tetap ada.
     */
    public function destroy(Tag $tag)
    {
        Gate::authorize('admin');

        $tag->delete();

        return redirect()->route('tags.index')
                         ->with('success', 'Tag berhasil dihapus.');
    }
}
```

**Penjelasan:**

- **Slug** adalah versi nama yang aman dipakai di URL: huruf kecil, spasi diganti tanda `-`, simbol dibuang. Contoh: `Basis Data` → `basis-data`, `AI/ML` → `aiml`.
- `$request->merge([...])` mengganti nilai input `slug` **sebelum** validasi. Jika slug dikosongkan, slug dibuat dari nama; jika diisi, isinya tetap dirapikan dengan `Str::slug()`. Dengan begitu aturan `unique:tags,slug` memeriksa slug yang benar-benar akan disimpan.
- Menghapus tag hanya menghapus baris di tabel pivot `course_tag` (cascade); data mata kuliahnya tetap ada.

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view tags.index
php artisan make:view tags.create
php artisan make:view tags.show
php artisan make:view tags.edit
```

Perintah di atas membuat folder `resources/views/tags/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/tags/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Tag')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Tag</h2>
    @can('admin')
        <a href="{{ route('tags.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Tag
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Nama Tag</th>
            <th>Slug</th>
            <th>Jumlah Mata Kuliah</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($tags as $i => $tag)
        <tr>
            <td>{{ $tags->firstItem() + $i }}</td>
            <td><span class="badge bg-info text-dark">{{ $tag->name }}</span></td>
            <td><code>{{ $tag->slug }}</code></td>
            <td>{{ $tag->courses_count }}</td>
            <td>
                <a href="{{ route('tags.show', $tag) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('tags.edit', $tag) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('tags.destroy', $tag) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus tag ini?')">
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
            <td colspan="5" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $tags->links() }}
@endsection
```

**Penjelasan:**

- Slug ditampilkan dengan tag `<code>` agar terlihat seperti teks teknis.

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/tags/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Tag')

@section('content')
<h2>Tambah Tag Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('tags.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="name" class="form-label">Nama Tag <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name') }}" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="slug" class="form-label">Slug</label>
                <input type="text" class="form-control @error('slug') is-invalid @enderror"
                       id="slug" name="slug" value="{{ old('slug') }}">
                <div class="form-text">Kosongkan untuk dibuat otomatis dari nama tag (contoh: "Basis Data" &rarr; <code>basis-data</code>).</div>
                @error('slug')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('tags.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Input slug tidak diberi `required` karena boleh dikosongkan. Teks kecil `form-text` memberi petunjuk kepada user.

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/tags/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Tag')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Tag: {{ $tag->name }}</h2>
    <a href="{{ route('tags.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card mb-4">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">Nama Tag</th>
                <td>: {{ $tag->name }}</td>
            </tr>
            <tr>
                <th>Slug</th>
                <td>: <code>{{ $tag->slug }}</code></td>
            </tr>
        </table>
    </div>
</div>

{{-- Daftar Mata Kuliah yang memakai tag ini (Relasi Many-to-Many) --}}
<div class="card">
    <div class="card-header">
        <h5 class="mb-0">Mata Kuliah dengan Tag Ini ({{ $tag->courses->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>Kode</th>
                    <th>Nama Mata Kuliah</th>
                    <th>SKS</th>
                    <th>Jurusan</th>
                </tr>
            </thead>
            <tbody>
                @forelse($tag->courses as $course)
                <tr>
                    <td>{{ $course->code }}</td>
                    <td><a href="{{ route('courses.show', $course) }}">{{ $course->name }}</a></td>
                    <td>{{ $course->credits }}</td>
                    <td>{{ $course->department->name }}</td>
                </tr>
                @empty
                <tr><td colspan="4" class="text-center">Belum ada mata kuliah dengan tag ini.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- `$tag->courses` adalah relasi Many-to-Many dari sisi tag (`belongsToMany(Course::class, 'course_tag')` di model `Tag`).

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/tags/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Tag')

@section('content')
<h2>Edit Tag: {{ $tag->name }}</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('tags.update', $tag) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="name" class="form-label">Nama Tag <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name', $tag->name) }}" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="slug" class="form-label">Slug</label>
                <input type="text" class="form-control @error('slug') is-invalid @enderror"
                       id="slug" name="slug" value="{{ old('slug', $tag->slug) }}">
                <div class="form-text">Kosongkan untuk dibuat ulang otomatis dari nama tag.</div>
                @error('slug')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('tags.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Jika slug dikosongkan saat edit, slug dibuat ulang dari nama tag.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Lainnya → Tags** → tampil 7 tag.
- [ ] Tambah tag bernama `Basis Data Lanjut` dengan slug kosong → slug otomatis `basis-data-lanjut`.
- [ ] Tambah tag `Backend` lagi → muncul error karena slug `backend` sudah dipakai.
- [ ] Edit tag baru, isi slug `Slug Saya` → tersimpan sebagai `slug-saya`.
- [ ] Klik **Detail** pada salah satu tag dengan jumlah mata kuliah lebih dari 0 → tampil daftar mata kuliah yang memiliki tag tersebut.
- [ ] Hapus tag baru → mata kuliah yang memakai tag itu tetap ada.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Tampilkan pratinjau slug secara langsung saat user mengetik nama (JavaScript event `input`).

[⬅ Modul 03: Ruang Kelas](#modul-03-ruang-kelas-classrooms) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 05: Ekstrakurikuler ➡](#modul-05-ekstrakurikuler-extracurriculars)

---

### Modul 05: Ekstrakurikuler (`extracurriculars`)

[⬅ Modul 04: Tag Mata Kuliah](#modul-04-tag-mata-kuliah-tags) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 06: Users ➡](#modul-06-users-users)

Data kegiatan ekstrakurikuler. Ekstrakurikuler berelasi **Many-to-Many** dengan mahasiswa melalui tabel pivot `extracurricular_student` yang memiliki **kolom tambahan** `joined_at` dan `role`.

Pendaftaran anggota dilakukan dari form Mahasiswa (Modul 09). Di modul ini kita menampilkan daftar anggotanya.

#### 🎯 Yang Dipelajari

- Membaca kolom tambahan tabel pivot dengan `->pivot`
- Aturan validasi dinamis (`min:` berdasarkan jumlah anggota)
- `Carbon::parse()` untuk memformat tanggal yang belum di-cast

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| Many-to-Many | `extracurriculars` ↔ `students` (pivot `extracurricular_student`) | Kolom pivot tambahan: `joined_at`, `role` |

#### 📋 Sebelum Mulai

- Modul 01 sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/ExtracurricularController.php` | Controller (logika CRUD) |
| `resources/views/extracurriculars/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/extracurriculars/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/extracurriculars/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/extracurriculars/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller ExtracurricularController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/ExtracurricularController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Extracurricular;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;

class ExtracurricularController extends Controller
{
    /**
     * Tampilkan semua ekstrakurikuler beserta jumlah anggotanya.
     */
    public function index()
    {
        $extracurriculars = Extracurricular::withCount('students')->orderBy('name')->paginate(10);
        return view('extracurriculars.index', compact('extracurriculars'));
    }

    /**
     * Tampilkan form tambah ekstrakurikuler (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');
        return view('extracurriculars.create');
    }

    /**
     * Simpan ekstrakurikuler baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'description' => 'nullable|string',
            'max_members' => 'required|integer|min:1',
        ]);

        Extracurricular::create($validated);

        return redirect()->route('extracurriculars.index')
                         ->with('success', 'Ekstrakurikuler berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail ekstrakurikuler beserta anggotanya (Many-to-Many).
     * Data peran & tanggal bergabung diambil dari tabel pivot.
     */
    public function show(Extracurricular $extracurricular)
    {
        $extracurricular->load(['students.user', 'students.department']);
        return view('extracurriculars.show', compact('extracurricular'));
    }

    /**
     * Tampilkan form edit ekstrakurikuler (khusus admin).
     */
    public function edit(Extracurricular $extracurricular)
    {
        Gate::authorize('admin');
        return view('extracurriculars.edit', compact('extracurricular'));
    }

    /**
     * Update data ekstrakurikuler (khusus admin).
     */
    public function update(Request $request, Extracurricular $extracurricular)
    {
        Gate::authorize('admin');

        // Kuota tidak boleh lebih kecil dari jumlah anggota yang sudah terdaftar
        $memberCount = $extracurricular->students()->count();

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'description' => 'nullable|string',
            'max_members' => 'required|integer|min:' . max(1, $memberCount),
        ]);

        $extracurricular->update($validated);

        return redirect()->route('extracurriculars.index')
                         ->with('success', 'Ekstrakurikuler berhasil diupdate.');
    }

    /**
     * Hapus ekstrakurikuler (khusus admin).
     * Data keanggotaan di tabel pivot ikut terhapus, data mahasiswa tetap ada.
     */
    public function destroy(Extracurricular $extracurricular)
    {
        Gate::authorize('admin');

        $extracurricular->delete();

        return redirect()->route('extracurriculars.index')
                         ->with('success', 'Ekstrakurikuler berhasil dihapus.');
    }
}
```

**Penjelasan:**

- `withCount('students')` menghasilkan `students_count`, dipakai untuk menampilkan `anggota / kuota`.
- Pada `update`, kuota minimal dihitung dari jumlah anggota saat ini: `'min:' . max(1, $memberCount)`. Jadi kuota tidak bisa diturunkan di bawah jumlah anggota yang sudah terdaftar. Ini contoh aturan validasi yang dibuat secara dinamis.
- Menghapus ekstrakurikuler hanya menghapus data keanggotaan di tabel pivot; data mahasiswa tetap ada.

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view extracurriculars.index
php artisan make:view extracurriculars.create
php artisan make:view extracurriculars.show
php artisan make:view extracurriculars.edit
```

Perintah di atas membuat folder `resources/views/extracurriculars/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/extracurriculars/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Ekstrakurikuler')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Ekstrakurikuler</h2>
    @can('admin')
        <a href="{{ route('extracurriculars.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Ekstrakurikuler
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Nama</th>
            <th>Deskripsi</th>
            <th>Anggota</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($extracurriculars as $i => $extracurricular)
        <tr>
            <td>{{ $extracurriculars->firstItem() + $i }}</td>
            <td>{{ $extracurricular->name }}</td>
            <td>{{ Str::limit($extracurricular->description ?? '-', 50) }}</td>
            <td>{{ $extracurricular->students_count }} / {{ $extracurricular->max_members }}</td>
            <td>
                <a href="{{ route('extracurriculars.show', $extracurricular) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('extracurriculars.edit', $extracurricular) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('extracurriculars.destroy', $extracurricular) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus ekstrakurikuler ini?')">
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
            <td colspan="5" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $extracurriculars->links() }}
@endsection
```

**Penjelasan:**

- Kolom Anggota menampilkan `students_count / max_members`.

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/extracurriculars/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Ekstrakurikuler')

@section('content')
<h2>Tambah Ekstrakurikuler Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('extracurriculars.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="name" class="form-label">Nama Ekstrakurikuler <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name') }}" required>
                @error('name')
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

            <div class="mb-3">
                <label for="max_members" class="form-label">Kuota Anggota <span class="text-danger">*</span></label>
                <input type="number" class="form-control @error('max_members') is-invalid @enderror"
                       id="max_members" name="max_members" value="{{ old('max_members', 50) }}" min="1" required>
                @error('max_members')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('extracurriculars.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Nilai awal kuota 50, sama dengan nilai default kolom di migration.

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/extracurriculars/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Ekstrakurikuler')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Ekstrakurikuler: {{ $extracurricular->name }}</h2>
    <a href="{{ route('extracurriculars.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card mb-4">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">Nama</th>
                <td>: {{ $extracurricular->name }}</td>
            </tr>
            <tr>
                <th>Deskripsi</th>
                <td>: {{ $extracurricular->description ?? '-' }}</td>
            </tr>
            <tr>
                <th>Anggota / Kuota</th>
                <td>: {{ $extracurricular->students->count() }} / {{ $extracurricular->max_members }} orang</td>
            </tr>
        </table>
    </div>
</div>

{{-- Daftar Anggota (Relasi Many-to-Many, kolom tambahan dari tabel pivot) --}}
<div class="card">
    <div class="card-header">
        <h5 class="mb-0">Daftar Anggota ({{ $extracurricular->students->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>NIM</th>
                    <th>Nama</th>
                    <th>Jurusan</th>
                    <th>Peran</th>
                    <th>Tanggal Bergabung</th>
                </tr>
            </thead>
            <tbody>
                @forelse($extracurricular->students as $student)
                <tr>
                    <td>{{ $student->nim }}</td>
                    <td><a href="{{ route('students.show', $student) }}">{{ $student->user->name }}</a></td>
                    <td>{{ $student->department->name }}</td>
                    <td>{{ ucfirst($student->pivot->role) }}</td>
                    <td>
                        {{ $student->pivot->joined_at
                            ? \Carbon\Carbon::parse($student->pivot->joined_at)->format('d M Y')
                            : '-' }}
                    </td>
                </tr>
                @empty
                <tr><td colspan="5" class="text-center">Belum ada anggota.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- `$student->pivot->role` dan `$student->pivot->joined_at` membaca kolom tambahan di tabel pivot. Kolom ini bisa diakses karena relasi di model `Extracurricular` ditulis dengan `->withPivot('joined_at', 'role')`.
- `joined_at` dari pivot masih berupa teks (misal `2025-03-10`), sehingga diformat dengan `\Carbon\Carbon::parse(...)->format('d M Y')`.
- Nama mahasiswa adalah link ke `route('students.show', $student)` yang bisa diklik setelah **Modul 09** selesai.

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/extracurriculars/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Ekstrakurikuler')

@section('content')
<h2>Edit Ekstrakurikuler: {{ $extracurricular->name }}</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('extracurriculars.update', $extracurricular) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="name" class="form-label">Nama Ekstrakurikuler <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name', $extracurricular->name) }}" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="description" class="form-label">Deskripsi</label>
                <textarea class="form-control @error('description') is-invalid @enderror"
                          id="description" name="description" rows="3">{{ old('description', $extracurricular->description) }}</textarea>
                @error('description')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="max_members" class="form-label">Kuota Anggota <span class="text-danger">*</span></label>
                <input type="number" class="form-control @error('max_members') is-invalid @enderror"
                       id="max_members" name="max_members" value="{{ old('max_members', $extracurricular->max_members) }}" min="1" required>
                <div class="form-text">Tidak boleh lebih kecil dari jumlah anggota saat ini.</div>
                @error('max_members')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('extracurriculars.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Teks bantuan mengingatkan bahwa kuota tidak boleh lebih kecil dari jumlah anggota saat ini.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Lainnya → Ekstrakurikuler** → tampil 5 data dengan kolom `anggota / kuota`.
- [ ] Klik **Detail** pada ekstrakurikuler yang memiliki anggota → tampil daftar anggota beserta peran (Member/Leader/Secretary) dan tanggal bergabung.
- [ ] Edit ekstrakurikuler yang memiliki anggota, isi kuota lebih kecil dari jumlah anggotanya → muncul error *must be at least ...*.
- [ ] Tambah `Futsal` dengan kuota 22, edit, lalu hapus.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Di halaman index, tampilkan badge **Penuh** jika `students_count >= max_members`.

[⬅ Modul 04: Tag Mata Kuliah](#modul-04-tag-mata-kuliah-tags) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 06: Users ➡](#modul-06-users-users)

---

### Modul 06: Users (`users`)

[⬅ Modul 05: Ekstrakurikuler](#modul-05-ekstrakurikuler-extracurriculars) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 07: Profil Pengguna ➡](#modul-07-profil-pengguna-profiles)

Mengelola akun pengguna (admin dan user). Tabel `users` adalah pusat relasi **One-to-One**: setiap user bisa memiliki satu profil, dan terhubung ke satu data dosen **atau** satu data mahasiswa.

#### 🎯 Yang Dipelajari

- Meng-hash password dengan `Hash::make()`
- Konfirmasi password dengan aturan `confirmed`
- Password opsional saat edit
- Mencegah admin menghapus akunnya sendiri
- Menampilkan relasi One-to-One yang bisa bernilai `null` dengan operator `?->`

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| One-to-One | `users` → `profiles` | Satu user memiliki satu profil |
| One-to-One | `users` → `teachers` | Satu user bisa menjadi satu dosen |
| One-to-One | `users` → `students` | Satu user bisa menjadi satu mahasiswa |
| One-to-Many | `users` → `announcements` | Satu user menulis banyak pengumuman |

#### 📋 Sebelum Mulai

- Modul 01 sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/UserController.php` | Controller (logika CRUD) |
| `resources/views/users/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/users/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/users/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/users/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller UserController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/UserController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;
use Illuminate\Support\Facades\Hash;

class UserController extends Controller
{
    /**
     * Tampilkan semua user beserta profilnya (One-to-One).
     */
    public function index()
    {
        $users = User::with('profile')->orderBy('name')->paginate(10);
        return view('users.index', compact('users'));
    }

    /**
     * Tampilkan form tambah user (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');
        return view('users.create');
    }

    /**
     * Simpan user baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|email|max:255|unique:users,email',
            'password' => 'required|string|min:8|confirmed',
            'role' => 'required|in:admin,user',
        ]);

        // Password wajib di-hash sebelum disimpan
        $validated['password'] = Hash::make($validated['password']);

        User::create($validated);

        return redirect()->route('users.index')
                         ->with('success', 'User berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail user beserta seluruh relasinya:
     * profile & teacher/student (One-to-One), announcements (One-to-Many).
     */
    public function show(User $user)
    {
        $user->load(['profile', 'teacher.department', 'student.department', 'announcements']);
        return view('users.show', compact('user'));
    }

    /**
     * Tampilkan form edit user (khusus admin).
     */
    public function edit(User $user)
    {
        Gate::authorize('admin');
        return view('users.edit', compact('user'));
    }

    /**
     * Update data user (khusus admin).
     */
    public function update(Request $request, User $user)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|email|max:255|unique:users,email,' . $user->id,
            'password' => 'nullable|string|min:8|confirmed',
            'role' => 'required|in:admin,user',
        ]);

        // Password hanya diganti jika kolomnya diisi
        if ($request->filled('password')) {
            $validated['password'] = Hash::make($validated['password']);
        } else {
            unset($validated['password']);
        }

        $user->update($validated);

        return redirect()->route('users.index')
                         ->with('success', 'User berhasil diupdate.');
    }

    /**
     * Hapus user (khusus admin).
     * Profile, data dosen/mahasiswa, dan pengumuman milik user ikut terhapus (cascade).
     */
    public function destroy(User $user)
    {
        Gate::authorize('admin');

        // Cegah admin menghapus akunnya sendiri yang sedang dipakai login
        if ($user->id === auth()->id()) {
            return back()->withErrors(['user' => 'Anda tidak dapat menghapus akun Anda sendiri.']);
        }

        $user->delete();

        return redirect()->route('users.index')
                         ->with('success', 'User berhasil dihapus.');
    }
}
```

**Penjelasan:**

- Password **tidak boleh** disimpan sebagai teks biasa. `Hash::make()` mengubahnya menjadi hash bcrypt yang tidak bisa dikembalikan ke teks aslinya.
- Aturan `confirmed` mewajibkan adanya input bernama `password_confirmation` yang isinya sama dengan `password`.
- Pada `update`, password bersifat `nullable`. Jika kolom password dikosongkan, `unset($validated['password'])` membuang key tersebut sehingga password lama tidak berubah.
- `destroy` menolak menghapus akun yang sedang login (`$user->id === auth()->id()`). Pesannya dikirim lewat `withErrors()`, yang otomatis ditampilkan oleh alert merah di layout.
- Menghapus user ikut menghapus profil, data dosen/mahasiswa, dan pengumuman miliknya, karena semua foreign key `user_id` memakai `onDelete('cascade')`.

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view users.index
php artisan make:view users.create
php artisan make:view users.show
php artisan make:view users.edit
```

Perintah di atas membuat folder `resources/views/users/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/users/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data User')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data User</h2>
    @can('admin')
        <a href="{{ route('users.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah User
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Nama</th>
            <th>Email</th>
            <th>Role</th>
            <th>No. HP</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($users as $i => $user)
        <tr>
            <td>{{ $users->firstItem() + $i }}</td>
            <td>{{ $user->name }}</td>
            <td>{{ $user->email }}</td>
            <td>
                <span class="badge bg-{{ $user->role === 'admin' ? 'danger' : 'primary' }}">
                    {{ ucfirst($user->role) }}
                </span>
            </td>
            {{-- Relasi One-to-One: $user->profile bisa null jika user belum punya profil --}}
            <td>{{ $user->profile?->phone ?? '-' }}</td>
            <td>
                <a href="{{ route('users.show', $user) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('users.edit', $user) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    {{-- Tombol hapus tidak ditampilkan untuk akun yang sedang login --}}
                    @if($user->id !== auth()->id())
                        <form action="{{ route('users.destroy', $user) }}" method="POST" class="d-inline"
                              onsubmit="return confirm('Yakin ingin menghapus? Profil, data dosen/mahasiswa, dan pengumuman user ini ikut terhapus.')">
                            @csrf
                            @method('DELETE')
                            <button type="submit" class="btn btn-sm btn-danger">
                                <i class="bi bi-trash"></i> Hapus
                            </button>
                        </form>
                    @endif
                @endcan
            </td>
        </tr>
        @empty
        <tr>
            <td colspan="6" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $users->links() }}
@endsection
```

**Penjelasan:**

- `$user->profile?->phone ?? '-'`: operator *nullsafe* `?->` mencegah error jika user belum memiliki profil (`$user->profile` bernilai `null`).
- Tombol Hapus disembunyikan pada baris akun yang sedang login (`$user->id !== auth()->id()`).

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/users/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah User')

@section('content')
<h2>Tambah User Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('users.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="name" class="form-label">Nama Lengkap <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name') }}" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="email" class="form-label">Email <span class="text-danger">*</span></label>
                <input type="email" class="form-control @error('email') is-invalid @enderror"
                       id="email" name="email" value="{{ old('email') }}" required>
                @error('email')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="row">
                <div class="col-md-6 mb-3">
                    <label for="password" class="form-label">Password <span class="text-danger">*</span></label>
                    <input type="password" class="form-control @error('password') is-invalid @enderror"
                           id="password" name="password" required>
                    <div class="form-text">Minimal 8 karakter.</div>
                    @error('password')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-6 mb-3">
                    {{-- Nama field harus "password_confirmation" agar aturan 'confirmed' bekerja --}}
                    <label for="password_confirmation" class="form-label">Konfirmasi Password <span class="text-danger">*</span></label>
                    <input type="password" class="form-control"
                           id="password_confirmation" name="password_confirmation" required>
                </div>
            </div>

            <div class="mb-3">
                <label for="role" class="form-label">Role <span class="text-danger">*</span></label>
                <select class="form-select @error('role') is-invalid @enderror" id="role" name="role" required>
                    <option value="user" {{ old('role') === 'user' ? 'selected' : '' }}>User</option>
                    <option value="admin" {{ old('role') === 'admin' ? 'selected' : '' }}>Admin</option>
                </select>
                @error('role')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('users.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Field konfirmasi **harus** bernama `password_confirmation` agar aturan `confirmed` bekerja.
- Role dipilih dengan `<select>` berisi `user` dan `admin`.

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/users/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail User')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail User: {{ $user->name }}</h2>
    <a href="{{ route('users.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card mb-4">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">Nama</th>
                <td>: {{ $user->name }}</td>
            </tr>
            <tr>
                <th>Email</th>
                <td>: {{ $user->email }}</td>
            </tr>
            <tr>
                <th>Role</th>
                <td>:
                    <span class="badge bg-{{ $user->role === 'admin' ? 'danger' : 'primary' }}">
                        {{ ucfirst($user->role) }}
                    </span>
                </td>
            </tr>
            <tr>
                <th>Terdaftar sejak</th>
                <td>: {{ $user->created_at->format('d M Y H:i') }}</td>
            </tr>
        </table>
    </div>
</div>

{{-- Profil (Relasi One-to-One) --}}
<div class="card mb-4">
    <div class="card-header">
        <h5 class="mb-0">Profil</h5>
    </div>
    <div class="card-body">
        @if($user->profile)
            <table class="table table-borderless mb-0">
                <tr>
                    <th width="200">No. HP</th>
                    <td>: {{ $user->profile->phone ?? '-' }}</td>
                </tr>
                <tr>
                    <th>Alamat</th>
                    <td>: {{ $user->profile->address ?? '-' }}</td>
                </tr>
                <tr>
                    <th>Tanggal Lahir</th>
                    <td>: {{ $user->profile->birth_date?->format('d M Y') ?? '-' }}</td>
                </tr>
            </table>
            <a href="{{ route('profiles.show', $user->profile) }}" class="btn btn-sm btn-outline-primary mt-2">Lihat Profil</a>
        @else
            <p class="text-muted mb-2">User ini belum memiliki profil.</p>
            @can('admin')
                <a href="{{ route('profiles.create') }}" class="btn btn-sm btn-primary">
                    <i class="bi bi-plus-circle"></i> Buat Profil
                </a>
            @endcan
        @endif
    </div>
</div>

{{-- Data Akademik (Relasi One-to-One ke teachers atau students) --}}
<div class="card mb-4">
    <div class="card-header">
        <h5 class="mb-0">Data Akademik</h5>
    </div>
    <div class="card-body">
        @if($user->teacher)
            <p class="mb-1"><span class="badge bg-warning text-dark">Dosen</span></p>
            <p class="mb-1">NIP: {{ $user->teacher->nip }}</p>
            <p class="mb-1">Jurusan: {{ $user->teacher->department->name }}</p>
            <p class="mb-2">Spesialisasi: {{ $user->teacher->specialization ?? '-' }}</p>
            <a href="{{ route('teachers.show', $user->teacher) }}" class="btn btn-sm btn-outline-primary">Lihat Data Dosen</a>
        @elseif($user->student)
            <p class="mb-1"><span class="badge bg-success">Mahasiswa</span></p>
            <p class="mb-1">NIM: {{ $user->student->nim }}</p>
            <p class="mb-1">Jurusan: {{ $user->student->department->name }}</p>
            <p class="mb-2">Semester: {{ $user->student->semester }}</p>
            <a href="{{ route('students.show', $user->student) }}" class="btn btn-sm btn-outline-primary">Lihat Data Mahasiswa</a>
        @else
            <p class="text-muted mb-0">User ini tidak terdaftar sebagai dosen maupun mahasiswa.</p>
        @endif
    </div>
</div>

{{-- Pengumuman yang dibuat user ini (Relasi One-to-Many) --}}
<div class="card">
    <div class="card-header">
        <h5 class="mb-0">Pengumuman yang Dibuat ({{ $user->announcements->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>Judul</th>
                    <th>Status</th>
                    <th>Tanggal</th>
                </tr>
            </thead>
            <tbody>
                @forelse($user->announcements as $announcement)
                <tr>
                    <td>{{ $announcement->title }}</td>
                    <td>
                        <span class="badge bg-{{ $announcement->is_published ? 'success' : 'secondary' }}">
                            {{ $announcement->is_published ? 'Published' : 'Draft' }}
                        </span>
                    </td>
                    <td>{{ $announcement->created_at->format('d M Y') }}</td>
                </tr>
                @empty
                <tr><td colspan="3" class="text-center">Belum ada pengumuman.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Card **Profil** memeriksa `@if($user->profile)` karena relasi One-to-One bisa kosong.
- Card **Data Akademik** memeriksa apakah user adalah dosen (`$user->teacher`) atau mahasiswa (`$user->student`).
- Link ke profil, dosen, dan mahasiswa bisa diklik setelah modul terkait (07, 08, 09) selesai.

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/users/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit User')

@section('content')
<h2>Edit User: {{ $user->name }}</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('users.update', $user) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="name" class="form-label">Nama Lengkap <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name', $user->name) }}" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="email" class="form-label">Email <span class="text-danger">*</span></label>
                <input type="email" class="form-control @error('email') is-invalid @enderror"
                       id="email" name="email" value="{{ old('email', $user->email) }}" required>
                @error('email')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            {{-- Password tidak wajib diisi saat edit --}}
            <div class="row">
                <div class="col-md-6 mb-3">
                    <label for="password" class="form-label">Password Baru</label>
                    <input type="password" class="form-control @error('password') is-invalid @enderror"
                           id="password" name="password">
                    <div class="form-text">Kosongkan jika tidak ingin mengganti password.</div>
                    @error('password')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-6 mb-3">
                    <label for="password_confirmation" class="form-label">Konfirmasi Password Baru</label>
                    <input type="password" class="form-control"
                           id="password_confirmation" name="password_confirmation">
                </div>
            </div>

            <div class="mb-3">
                <label for="role" class="form-label">Role <span class="text-danger">*</span></label>
                <select class="form-select @error('role') is-invalid @enderror" id="role" name="role" required>
                    <option value="user" {{ old('role', $user->role) === 'user' ? 'selected' : '' }}>User</option>
                    <option value="admin" {{ old('role', $user->role) === 'admin' ? 'selected' : '' }}>Admin</option>
                </select>
                @error('role')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('users.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Input password tidak diberi `required` dan tidak diisi nilai lama, karena password tersimpan dalam bentuk hash dan tidak bisa ditampilkan.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Lainnya → Users** → tampil 16 user (1 admin, 5 dosen, 10 mahasiswa) dalam 2 halaman.
- [ ] Tambah user dengan konfirmasi password berbeda → muncul error *The password field confirmation does not match.*
- [ ] Tambah user `Budi`, `budi@siakad.com`, password `password123`, role `user` → berhasil. Logout dan login dengan akun Budi → berhasil.
- [ ] Login kembali sebagai admin, edit Budi tanpa mengisi password → password lama (`password123`) tetap berlaku.
- [ ] Detail `dosen1@siakad.com` → tampil NIP; detail `mahasiswa1@siakad.com` → tampil NIM; detail Administrator → tampil daftar pengumuman yang dibuat.
- [ ] Pada baris Administrator (akun yang sedang login) tidak ada tombol Hapus.
- [ ] Hapus user Budi → data hilang dari daftar.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Cegah admin mengubah role dirinya sendiri menjadi `user` agar tidak kehilangan akses admin.

[⬅ Modul 05: Ekstrakurikuler](#modul-05-ekstrakurikuler-extracurriculars) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 07: Profil Pengguna ➡](#modul-07-profil-pengguna-profiles)

---

### Modul 07: Profil Pengguna (`profiles`)

[⬅ Modul 06: Users](#modul-06-users-users) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 08: Dosen ➡](#modul-08-dosen-teachers)

Profil detail pengguna: nomor HP, alamat, tanggal lahir, dan foto. Relasi `users` ↔ `profiles` adalah **One-to-One**: satu user hanya boleh memiliki satu profil. Modul ini juga mengajarkan **upload file** (foto profil).

#### 🎯 Yang Dipelajari

- Relasi One-to-One dari sisi `belongsTo` (`$profile->user`)
- Menjaga aturan One-to-One: dropdown hanya berisi user tanpa profil (`doesntHave`) + validasi `unique:profiles,user_id`
- Upload, ganti, dan hapus file dengan `Storage`
- Atribut form `enctype="multipart/form-data"`
- Input `type="date"` dan cast `date` di model

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| One-to-One (inverse) | `profiles` → `users` | Setiap profil milik tepat satu user |

#### 📋 Sebelum Mulai

- Modul 06 (Users) sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/ProfileController.php` | Controller (logika CRUD) |
| `resources/views/profiles/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/profiles/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/profiles/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/profiles/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Hubungkan Folder Storage

Jalankan perintah ini **satu kali** saja untuk seluruh project:

```bash
php artisan storage:link
```

Perintah ini membuat *shortcut* `public/storage` yang mengarah ke `storage/app/public`. File yang di-upload ke disk `public` akan tersimpan di `storage/app/public/...` dan bisa dibuka dari browser lewat URL `http://localhost:8000/storage/...`.

#### Langkah 2: Buat Controller

```bash
php artisan make:controller ProfileController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/ProfileController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Profile;
use App\Models\User;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;
use Illuminate\Support\Facades\Storage;

class ProfileController extends Controller
{
    /**
     * Tampilkan semua profil beserta data user-nya (One-to-One).
     */
    public function index()
    {
        $profiles = Profile::with('user')->latest()->paginate(10);
        return view('profiles.index', compact('profiles'));
    }

    /**
     * Tampilkan form tambah profil (khusus admin).
     * Dropdown hanya berisi user yang BELUM memiliki profil,
     * karena relasi user-profile adalah One-to-One.
     */
    public function create()
    {
        Gate::authorize('admin');

        $users = User::doesntHave('profile')->orderBy('name')->get();

        return view('profiles.create', compact('users'));
    }

    /**
     * Simpan profil baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'user_id' => 'required|exists:users,id|unique:profiles,user_id',
            'phone' => 'nullable|string|max:20',
            'address' => 'nullable|string',
            'avatar' => 'nullable|image|max:2048', // maksimal 2 MB
            'birth_date' => 'nullable|date|before:today',
        ]);

        // Simpan file avatar ke storage/app/public/avatars
        if ($request->hasFile('avatar')) {
            $validated['avatar'] = $request->file('avatar')->store('avatars', 'public');
        }

        Profile::create($validated);

        return redirect()->route('profiles.index')
                         ->with('success', 'Profil berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail profil.
     */
    public function show(Profile $profile)
    {
        $profile->load('user');
        return view('profiles.show', compact('profile'));
    }

    /**
     * Tampilkan form edit profil (khusus admin).
     * Dropdown berisi user yang belum punya profil + pemilik profil ini.
     */
    public function edit(Profile $profile)
    {
        Gate::authorize('admin');

        $users = User::doesntHave('profile')
                     ->orWhere('id', $profile->user_id)
                     ->orderBy('name')
                     ->get();

        return view('profiles.edit', compact('profile', 'users'));
    }

    /**
     * Update data profil (khusus admin).
     */
    public function update(Request $request, Profile $profile)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'user_id' => 'required|exists:users,id|unique:profiles,user_id,' . $profile->id,
            'phone' => 'nullable|string|max:20',
            'address' => 'nullable|string',
            'avatar' => 'nullable|image|max:2048',
            'birth_date' => 'nullable|date|before:today',
        ]);

        if ($request->hasFile('avatar')) {
            // Hapus avatar lama agar tidak menumpuk di storage
            if ($profile->avatar) {
                Storage::disk('public')->delete($profile->avatar);
            }
            $validated['avatar'] = $request->file('avatar')->store('avatars', 'public');
        } else {
            // Tidak upload file baru -> avatar lama dipertahankan
            unset($validated['avatar']);
        }

        $profile->update($validated);

        return redirect()->route('profiles.index')
                         ->with('success', 'Profil berhasil diupdate.');
    }

    /**
     * Hapus profil beserta file avatarnya (khusus admin).
     */
    public function destroy(Profile $profile)
    {
        Gate::authorize('admin');

        if ($profile->avatar) {
            Storage::disk('public')->delete($profile->avatar);
        }

        $profile->delete();

        return redirect()->route('profiles.index')
                         ->with('success', 'Profil berhasil dihapus.');
    }
}
```

**Penjelasan:**

- `User::doesntHave('profile')` mengambil user yang **belum** memiliki profil, dipakai untuk dropdown form tambah. Pada form edit, `orWhere('id', $profile->user_id)` menambahkan pemilik profil saat ini agar tetap muncul di dropdown.
- `unique:profiles,user_id` memastikan satu user tidak bisa memiliki dua profil, walaupun request dikirim tanpa lewat form.
- `$request->file('avatar')->store('avatars', 'public')` menyimpan file ke `storage/app/public/avatars/` dengan nama acak, lalu mengembalikan path-nya (misal `avatars/Xy12...jpg`). Path inilah yang disimpan di kolom `avatar`.
- Saat update dengan foto baru, foto lama dihapus dengan `Storage::disk('public')->delete(...)`. Jika tidak ada foto baru, `unset($validated['avatar'])` mencegah kolom `avatar` tertimpa `null`.
- Aturan `image|max:2048` hanya menerima file gambar maksimal 2 MB (satuan `max` untuk file adalah kilobyte). `before:today` mencegah tanggal lahir di masa depan.
- Saat profil dihapus, file fotonya ikut dihapus agar tidak menjadi sampah di storage.

#### Langkah 3: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view profiles.index
php artisan make:view profiles.create
php artisan make:view profiles.show
php artisan make:view profiles.edit
```

Perintah di atas membuat folder `resources/views/profiles/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 4: View Index (Read: daftar data)

File: `resources/views/profiles/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Profil')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Profil Pengguna</h2>
    @can('admin')
        <a href="{{ route('profiles.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Profil
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped align-middle">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Foto</th>
            <th>Nama User</th>
            <th>Email</th>
            <th>No. HP</th>
            <th>Tanggal Lahir</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($profiles as $i => $profile)
        <tr>
            <td>{{ $profiles->firstItem() + $i }}</td>
            <td>
                @if($profile->avatar)
                    <img src="{{ asset('storage/' . $profile->avatar) }}" alt="Avatar"
                         class="rounded-circle" width="40" height="40" style="object-fit: cover;">
                @else
                    <i class="bi bi-person-circle fs-3 text-secondary"></i>
                @endif
            </td>
            <td>{{ $profile->user->name }}</td>
            <td>{{ $profile->user->email }}</td>
            <td>{{ $profile->phone ?? '-' }}</td>
            <td>{{ $profile->birth_date?->format('d M Y') ?? '-' }}</td>
            <td>
                <a href="{{ route('profiles.show', $profile) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('profiles.edit', $profile) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('profiles.destroy', $profile) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus profil ini?')">
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

{{ $profiles->links() }}
@endsection
```

**Penjelasan:**

- `asset('storage/' . $profile->avatar)` menghasilkan URL gambar, misal `http://localhost:8000/storage/avatars/Xy12...jpg`.
- `$profile->birth_date?->format('d M Y')` bisa langsung dipakai karena `birth_date` di-cast `date` di model `Profile`, sehingga sudah berupa objek Carbon.

#### Langkah 5: View Create (Create: form tambah data)

File: `resources/views/profiles/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Profil')

@section('content')
<h2>Tambah Profil Baru</h2>

<div class="card">
    <div class="card-body">
        {{-- enctype="multipart/form-data" WAJIB ada agar file avatar ikut terkirim --}}
        <form action="{{ route('profiles.store') }}" method="POST" enctype="multipart/form-data">
            @csrf

            <div class="mb-3">
                <label for="user_id" class="form-label">User <span class="text-danger">*</span></label>
                <select class="form-select @error('user_id') is-invalid @enderror" id="user_id" name="user_id" required>
                    <option value="">-- Pilih User --</option>
                    @foreach($users as $user)
                        <option value="{{ $user->id }}" {{ old('user_id') == $user->id ? 'selected' : '' }}>
                            {{ $user->name }} ({{ $user->email }})
                        </option>
                    @endforeach
                </select>
                <div class="form-text">Hanya user yang belum memiliki profil yang ditampilkan.</div>
                @error('user_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="phone" class="form-label">No. HP</label>
                <input type="text" class="form-control @error('phone') is-invalid @enderror"
                       id="phone" name="phone" value="{{ old('phone') }}" maxlength="20">
                @error('phone')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="address" class="form-label">Alamat</label>
                <textarea class="form-control @error('address') is-invalid @enderror"
                          id="address" name="address" rows="3">{{ old('address') }}</textarea>
                @error('address')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="birth_date" class="form-label">Tanggal Lahir</label>
                <input type="date" class="form-control @error('birth_date') is-invalid @enderror"
                       id="birth_date" name="birth_date" value="{{ old('birth_date') }}">
                @error('birth_date')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="avatar" class="form-label">Foto Profil</label>
                <input type="file" class="form-control @error('avatar') is-invalid @enderror"
                       id="avatar" name="avatar" accept="image/*">
                <div class="form-text">Format gambar (jpg, png, dll), maksimal 2 MB.</div>
                @error('avatar')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('profiles.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- **Wajib** menambahkan `enctype="multipart/form-data"` pada tag `<form>`. Tanpa atribut ini file tidak ikut terkirim dan `$request->hasFile('avatar')` selalu `false`.
- `accept="image/*"` hanya membantu memfilter file di dialog pilih file. Validasi sesungguhnya tetap aturan `image` di controller.

#### Langkah 6: View Show (Read: detail data + relasi)

File: `resources/views/profiles/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Profil')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Profil: {{ $profile->user->name }}</h2>
    <a href="{{ route('profiles.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card">
    <div class="card-body">
        <div class="row">
            <div class="col-md-3 text-center mb-3">
                @if($profile->avatar)
                    <img src="{{ asset('storage/' . $profile->avatar) }}" alt="Avatar"
                         class="img-thumbnail rounded-circle" width="160" height="160" style="object-fit: cover;">
                @else
                    <i class="bi bi-person-circle text-secondary" style="font-size: 8rem;"></i>
                @endif
            </div>
            <div class="col-md-9">
                <table class="table table-borderless">
                    {{-- Data dari tabel users (Relasi One-to-One, sisi belongsTo) --}}
                    <tr>
                        <th width="200">Nama</th>
                        <td>: <a href="{{ route('users.show', $profile->user) }}">{{ $profile->user->name }}</a></td>
                    </tr>
                    <tr>
                        <th>Email</th>
                        <td>: {{ $profile->user->email }}</td>
                    </tr>
                    <tr>
                        <th>Role</th>
                        <td>: {{ ucfirst($profile->user->role) }}</td>
                    </tr>
                    {{-- Data dari tabel profiles --}}
                    <tr>
                        <th>No. HP</th>
                        <td>: {{ $profile->phone ?? '-' }}</td>
                    </tr>
                    <tr>
                        <th>Alamat</th>
                        <td>: {{ $profile->address ?? '-' }}</td>
                    </tr>
                    <tr>
                        <th>Tanggal Lahir</th>
                        <td>: {{ $profile->birth_date?->format('d M Y') ?? '-' }}</td>
                    </tr>
                </table>
            </div>
        </div>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Jika belum ada foto, ditampilkan ikon `bi-person-circle` sebagai pengganti.

#### Langkah 7: View Edit (Update: form ubah data)

File: `resources/views/profiles/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Profil')

@section('content')
<h2>Edit Profil: {{ $profile->user->name }}</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('profiles.update', $profile) }}" method="POST" enctype="multipart/form-data">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="user_id" class="form-label">User <span class="text-danger">*</span></label>
                <select class="form-select @error('user_id') is-invalid @enderror" id="user_id" name="user_id" required>
                    @foreach($users as $user)
                        <option value="{{ $user->id }}" {{ old('user_id', $profile->user_id) == $user->id ? 'selected' : '' }}>
                            {{ $user->name }} ({{ $user->email }})
                        </option>
                    @endforeach
                </select>
                @error('user_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="phone" class="form-label">No. HP</label>
                <input type="text" class="form-control @error('phone') is-invalid @enderror"
                       id="phone" name="phone" value="{{ old('phone', $profile->phone) }}" maxlength="20">
                @error('phone')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="address" class="form-label">Alamat</label>
                <textarea class="form-control @error('address') is-invalid @enderror"
                          id="address" name="address" rows="3">{{ old('address', $profile->address) }}</textarea>
                @error('address')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="birth_date" class="form-label">Tanggal Lahir</label>
                {{-- Input type="date" membutuhkan format Y-m-d --}}
                <input type="date" class="form-control @error('birth_date') is-invalid @enderror"
                       id="birth_date" name="birth_date"
                       value="{{ old('birth_date', $profile->birth_date?->format('Y-m-d')) }}">
                @error('birth_date')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="avatar" class="form-label">Foto Profil</label>
                @if($profile->avatar)
                    <div class="mb-2">
                        <img src="{{ asset('storage/' . $profile->avatar) }}" alt="Avatar saat ini"
                             class="img-thumbnail" width="100">
                    </div>
                @endif
                <input type="file" class="form-control @error('avatar') is-invalid @enderror"
                       id="avatar" name="avatar" accept="image/*">
                <div class="form-text">Kosongkan jika tidak ingin mengganti foto.</div>
                @error('avatar')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('profiles.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Input `type="date"` membutuhkan format `Y-m-d`, sehingga nilai lama ditulis `$profile->birth_date?->format('Y-m-d')`.
- Foto saat ini ditampilkan sebagai pratinjau kecil di atas input file.

#### Langkah 8: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Pastikan `php artisan storage:link` sudah dijalankan.
- [ ] Buka menu **Lainnya → Profiles** → tampil 16 profil dari seeder (tanpa foto).
- [ ] Klik **Tambah Profil** → dropdown kosong karena semua user sudah punya profil. Buat user baru di menu Users, lalu kembali → user baru muncul di dropdown.
- [ ] Pilih file PDF sebagai foto → muncul error *must be an image*.
- [ ] Isi data lengkap dan pilih foto JPG/PNG → foto tampil bulat di daftar profil.
- [ ] Edit tanpa memilih foto baru → foto lama tetap. Edit dengan foto baru → foto berganti dan file lama hilang dari folder `storage/app/public/avatars`.
- [ ] Hapus profil → file fotonya ikut terhapus.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Tampilkan umur pengguna di halaman detail dengan `$profile->birth_date->age`.

[⬅ Modul 06: Users](#modul-06-users-users) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 08: Dosen ➡](#modul-08-dosen-teachers)

---

### Modul 08: Dosen (`teachers`)

[⬅ Modul 07: Profil Pengguna](#modul-07-profil-pengguna-profiles) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 09: Mahasiswa ➡](#modul-09-mahasiswa-students)

Data dosen. Setiap dosen terhubung ke satu akun user (**One-to-One**) dan satu jurusan (**Many-to-One**), serta memiliki banyak jadwal mengajar (**One-to-Many**).

#### 🎯 Yang Dipelajari

- Dropdown dari dua tabel relasi (`users` dan `departments`)
- Method `private` untuk query yang dipakai berulang (`availableUsers()`)
- Query kondisional dengan `when()`
- Validasi `unique` lintas tabel (user tidak boleh sekaligus dosen dan mahasiswa)

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| One-to-One (inverse) | `teachers` → `users` | Setiap dosen memakai satu akun user |
| Many-to-One | `teachers` → `departments` | Banyak dosen berada di satu jurusan |
| One-to-Many | `teachers` → `schedules` | Satu dosen mengajar banyak jadwal |

#### 📋 Sebelum Mulai

- Modul 01 (Jurusan) dan 06 (Users) sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/TeacherController.php` | Controller (logika CRUD) |
| `resources/views/teachers/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/teachers/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/teachers/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/teachers/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller TeacherController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/TeacherController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Department;
use App\Models\Teacher;
use App\Models\User;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;

class TeacherController extends Controller
{
    /**
     * Tampilkan semua dosen beserta user & jurusannya.
     */
    public function index()
    {
        $teachers = Teacher::with(['user', 'department'])->orderBy('nip')->paginate(10);
        return view('teachers.index', compact('teachers'));
    }

    /**
     * Tampilkan form tambah dosen (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');

        $users = $this->availableUsers();
        $departments = Department::orderBy('name')->get();

        return view('teachers.create', compact('users', 'departments'));
    }

    /**
     * Simpan dosen baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            // Satu user hanya boleh menjadi satu dosen, dan tidak boleh sekaligus mahasiswa
            'user_id' => 'required|exists:users,id|unique:teachers,user_id|unique:students,user_id',
            'department_id' => 'required|exists:departments,id',
            'nip' => 'required|string|max:30|unique:teachers,nip',
            'specialization' => 'nullable|string|max:255',
        ]);

        Teacher::create($validated);

        return redirect()->route('teachers.index')
                         ->with('success', 'Dosen berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail dosen beserta jadwal mengajarnya (One-to-Many).
     */
    public function show(Teacher $teacher)
    {
        $teacher->load(['user.profile', 'department', 'schedules.course', 'schedules.classroom']);
        return view('teachers.show', compact('teacher'));
    }

    /**
     * Tampilkan form edit dosen (khusus admin).
     */
    public function edit(Teacher $teacher)
    {
        Gate::authorize('admin');

        $users = $this->availableUsers($teacher->user_id);
        $departments = Department::orderBy('name')->get();

        return view('teachers.edit', compact('teacher', 'users', 'departments'));
    }

    /**
     * Update data dosen (khusus admin).
     */
    public function update(Request $request, Teacher $teacher)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'user_id' => 'required|exists:users,id|unique:students,user_id|unique:teachers,user_id,' . $teacher->id,
            'department_id' => 'required|exists:departments,id',
            'nip' => 'required|string|max:30|unique:teachers,nip,' . $teacher->id,
            'specialization' => 'nullable|string|max:255',
        ]);

        $teacher->update($validated);

        return redirect()->route('teachers.index')
                         ->with('success', 'Data dosen berhasil diupdate.');
    }

    /**
     * Hapus data dosen (khusus admin).
     * Akun user-nya TIDAK ikut terhapus, tetapi jadwal mengajarnya ikut terhapus.
     */
    public function destroy(Teacher $teacher)
    {
        Gate::authorize('admin');

        $teacher->delete();

        return redirect()->route('teachers.index')
                         ->with('success', 'Data dosen berhasil dihapus.');
    }

    /**
     * Daftar user ber-role "user" yang belum terdaftar sebagai dosen maupun mahasiswa.
     * Saat edit, user milik dosen yang sedang diedit ikut dimasukkan.
     */
    private function availableUsers(?int $currentUserId = null)
    {
        return User::where('role', 'user')
                   ->doesntHave('teacher')
                   ->doesntHave('student')
                   ->when($currentUserId, fn ($query) => $query->orWhere('id', $currentUserId))
                   ->orderBy('name')
                   ->get();
    }
}
```

**Penjelasan:**

- `availableUsers()` mengambil user ber-role `user` yang **belum** menjadi dosen maupun mahasiswa. Saat edit, parameter `$currentUserId` menambahkan user milik dosen yang sedang diedit lewat `when(...)` + `orWhere(...)`.
- Method yang diawali `private` hanya bisa dipanggil dari dalam class ini, sehingga tidak dianggap sebagai action oleh route.
- Aturan `unique:teachers,user_id|unique:students,user_id` menolak user yang sudah tercatat sebagai dosen lain atau sebagai mahasiswa.
- Menghapus dosen **tidak** menghapus akun user-nya (relasinya `belongsTo`), tetapi jadwal mengajarnya ikut terhapus (cascade).

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view teachers.index
php artisan make:view teachers.create
php artisan make:view teachers.show
php artisan make:view teachers.edit
```

Perintah di atas membuat folder `resources/views/teachers/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/teachers/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Dosen')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Dosen</h2>
    @can('admin')
        <a href="{{ route('teachers.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Dosen
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>NIP</th>
            <th>Nama Dosen</th>
            <th>Jurusan</th>
            <th>Spesialisasi</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($teachers as $i => $teacher)
        <tr>
            <td>{{ $teachers->firstItem() + $i }}</td>
            <td>{{ $teacher->nip }}</td>
            <td>{{ $teacher->user->name }}</td>
            <td>{{ $teacher->department->name }}</td>
            <td>{{ $teacher->specialization ?? '-' }}</td>
            <td>
                <a href="{{ route('teachers.show', $teacher) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('teachers.edit', $teacher) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('teachers.destroy', $teacher) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus? Jadwal mengajar dosen ini ikut terhapus.')">
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
            <td colspan="6" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $teachers->links() }}
@endsection
```

**Penjelasan:**

- Nama dosen diambil dari tabel `users` lewat relasi: `$teacher->user->name`.

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/teachers/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Dosen')

@section('content')
<h2>Tambah Dosen Baru</h2>

<div class="card">
    <div class="card-body">
        @if($users->isEmpty())
            <div class="alert alert-warning">
                Tidak ada user yang tersedia. Buat akun user baru terlebih dahulu di menu
                <a href="{{ route('users.create') }}">Users</a>.
            </div>
        @endif

        <form action="{{ route('teachers.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="user_id" class="form-label">Akun User <span class="text-danger">*</span></label>
                <select class="form-select @error('user_id') is-invalid @enderror" id="user_id" name="user_id" required>
                    <option value="">-- Pilih User --</option>
                    @foreach($users as $user)
                        <option value="{{ $user->id }}" {{ old('user_id') == $user->id ? 'selected' : '' }}>
                            {{ $user->name }} ({{ $user->email }})
                        </option>
                    @endforeach
                </select>
                <div class="form-text">Hanya user yang belum terdaftar sebagai dosen/mahasiswa yang ditampilkan.</div>
                @error('user_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="department_id" class="form-label">Jurusan <span class="text-danger">*</span></label>
                <select class="form-select @error('department_id') is-invalid @enderror" id="department_id" name="department_id" required>
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
                <label for="nip" class="form-label">NIP <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('nip') is-invalid @enderror"
                       id="nip" name="nip" value="{{ old('nip') }}" maxlength="30" required>
                @error('nip')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="specialization" class="form-label">Spesialisasi</label>
                <input type="text" class="form-control @error('specialization') is-invalid @enderror"
                       id="specialization" name="specialization" value="{{ old('specialization') }}">
                @error('specialization')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('teachers.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Semua user di data dummy sudah menjadi dosen atau mahasiswa, sehingga dropdown akan kosong dan muncul peringatan kuning. Buat user baru terlebih dahulu di menu Users.
- Dropdown jurusan diisi dari `$departments` yang dikirim controller.

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/teachers/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Dosen')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Dosen: {{ $teacher->user->name }}</h2>
    <a href="{{ route('teachers.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card mb-4">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">NIP</th>
                <td>: {{ $teacher->nip }}</td>
            </tr>
            <tr>
                <th>Nama</th>
                <td>: <a href="{{ route('users.show', $teacher->user) }}">{{ $teacher->user->name }}</a></td>
            </tr>
            <tr>
                <th>Email</th>
                <td>: {{ $teacher->user->email }}</td>
            </tr>
            <tr>
                <th>No. HP</th>
                {{-- Relasi berantai: teacher -> user -> profile --}}
                <td>: {{ $teacher->user->profile?->phone ?? '-' }}</td>
            </tr>
            <tr>
                <th>Jurusan</th>
                <td>: <a href="{{ route('departments.show', $teacher->department) }}">{{ $teacher->department->name }}</a></td>
            </tr>
            <tr>
                <th>Spesialisasi</th>
                <td>: {{ $teacher->specialization ?? '-' }}</td>
            </tr>
        </table>
    </div>
</div>

{{-- Jadwal Mengajar (Relasi One-to-Many) --}}
<div class="card">
    <div class="card-header">
        <h5 class="mb-0">Jadwal Mengajar ({{ $teacher->schedules->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>Hari</th>
                    <th>Jam</th>
                    <th>Mata Kuliah</th>
                    <th>Ruangan</th>
                </tr>
            </thead>
            <tbody>
                @forelse($teacher->schedules as $schedule)
                <tr>
                    <td>{{ $schedule->day }}</td>
                    <td>{{ substr($schedule->start_time, 0, 5) }} - {{ substr($schedule->end_time, 0, 5) }}</td>
                    <td>{{ $schedule->course->name }}</td>
                    <td>{{ $schedule->classroom->name }}</td>
                </tr>
                @empty
                <tr><td colspan="4" class="text-center">Belum ada jadwal mengajar.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- `$teacher->user->profile?->phone` adalah relasi berantai: dosen → user → profil.

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/teachers/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Dosen')

@section('content')
<h2>Edit Dosen: {{ $teacher->user->name }}</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('teachers.update', $teacher) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="user_id" class="form-label">Akun User <span class="text-danger">*</span></label>
                <select class="form-select @error('user_id') is-invalid @enderror" id="user_id" name="user_id" required>
                    @foreach($users as $user)
                        <option value="{{ $user->id }}" {{ old('user_id', $teacher->user_id) == $user->id ? 'selected' : '' }}>
                            {{ $user->name }} ({{ $user->email }})
                        </option>
                    @endforeach
                </select>
                @error('user_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="department_id" class="form-label">Jurusan <span class="text-danger">*</span></label>
                <select class="form-select @error('department_id') is-invalid @enderror" id="department_id" name="department_id" required>
                    @foreach($departments as $department)
                        <option value="{{ $department->id }}" {{ old('department_id', $teacher->department_id) == $department->id ? 'selected' : '' }}>
                            {{ $department->name }}
                        </option>
                    @endforeach
                </select>
                @error('department_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="nip" class="form-label">NIP <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('nip') is-invalid @enderror"
                       id="nip" name="nip" value="{{ old('nip', $teacher->nip) }}" maxlength="30" required>
                @error('nip')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="specialization" class="form-label">Spesialisasi</label>
                <input type="text" class="form-control @error('specialization') is-invalid @enderror"
                       id="specialization" name="specialization" value="{{ old('specialization', $teacher->specialization) }}">
                @error('specialization')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('teachers.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Opsi dropdown terpilih ditentukan dengan `old('user_id', $teacher->user_id) == $user->id`.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Dosen** → tampil 5 dosen dengan jurusan masing-masing.
- [ ] Klik **Tambah Dosen** → muncul peringatan *Tidak ada user yang tersedia*. Buat user baru `Dosen Baru` (role user) di menu Users, lalu kembali → user tersebut muncul di dropdown.
- [ ] Simpan dengan NIP yang sudah dipakai dosen lain → muncul error.
- [ ] Simpan data valid → dosen baru tampil di daftar.
- [ ] Klik **Detail** pada **Dr. Budi Santoso** → tampil jadwal mengajar.
- [ ] Hapus dosen baru → dosen hilang, tetapi akun `Dosen Baru` masih ada di menu Users.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Di halaman detail dosen, tampilkan total SKS yang diajar (petunjuk: `$teacher->schedules->sum(fn ($s) => $s->course->credits)`).

[⬅ Modul 07: Profil Pengguna](#modul-07-profil-pengguna-profiles) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 09: Mahasiswa ➡](#modul-09-mahasiswa-students)

---

### Modul 09: Mahasiswa (`students`)

[⬅ Modul 08: Dosen](#modul-08-dosen-teachers) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 10: Mata Kuliah ➡](#modul-10-mata-kuliah-courses)

Data mahasiswa, modul dengan relasi terbanyak: One-to-One ke user, Many-to-One ke jurusan, Many-to-Many ke mata kuliah (lewat `enrollments`) dan ke ekstrakurikuler (lewat `extracurricular_student`), serta One-to-Many ke nilai.

Di modul ini kita membuat **checkbox Many-to-Many** untuk memilih ekstrakurikuler, termasuk mengisi kolom tambahan di tabel pivot.

#### 🎯 Yang Dipelajari

- Checkbox Many-to-Many (`name="extracurriculars[]"`)
- `attach()` dengan data kolom pivot tambahan
- `sync()` dan hasil kembaliannya (`attached`, `detached`)
- `updateExistingPivot()`
- `Arr::except()` untuk membuang input yang bukan kolom tabel
- Membaca kolom pivot di view (`$course->pivot->status`)

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| One-to-One (inverse) | `students` → `users` | Setiap mahasiswa memakai satu akun user |
| Many-to-One | `students` → `departments` | Banyak mahasiswa berada di satu jurusan |
| Many-to-Many | `students` ↔ `courses` (pivot `enrollments`) | Mahasiswa mengambil banyak mata kuliah |
| Many-to-Many | `students` ↔ `extracurriculars` (pivot `extracurricular_student`) | Mahasiswa mengikuti banyak ekstrakurikuler |
| One-to-Many | `students` → `grades` | Satu mahasiswa memiliki banyak nilai |

#### 📋 Sebelum Mulai

- Modul 01 (Jurusan), 05 (Ekstrakurikuler), dan 06 (Users) sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/StudentController.php` | Controller (logika CRUD) |
| `resources/views/students/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/students/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/students/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/students/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller StudentController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/StudentController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Department;
use App\Models\Extracurricular;
use App\Models\Student;
use App\Models\User;
use Illuminate\Http\Request;
use Illuminate\Support\Arr;
use Illuminate\Support\Facades\Gate;

class StudentController extends Controller
{
    /**
     * Tampilkan semua mahasiswa beserta user & jurusannya.
     */
    public function index()
    {
        $students = Student::with(['user', 'department'])->orderBy('nim')->paginate(10);
        return view('students.index', compact('students'));
    }

    /**
     * Tampilkan form tambah mahasiswa (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');

        $users = $this->availableUsers();
        $departments = Department::orderBy('name')->get();
        $extracurriculars = Extracurricular::orderBy('name')->get();

        return view('students.create', compact('users', 'departments', 'extracurriculars'));
    }

    /**
     * Simpan mahasiswa baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'user_id' => 'required|exists:users,id|unique:students,user_id|unique:teachers,user_id',
            'department_id' => 'required|exists:departments,id',
            'nim' => 'required|string|max:20|unique:students,nim',
            'semester' => 'required|integer|min:1|max:14',
            'extracurriculars' => 'nullable|array',
            'extracurriculars.*' => 'exists:extracurriculars,id',
        ]);

        // 'extracurriculars' bukan kolom tabel students, jadi dipisahkan dulu
        $student = Student::create(Arr::except($validated, 'extracurriculars'));

        // Many-to-Many: daftarkan ke ekstrakurikuler + isi kolom tambahan di tabel pivot
        $student->extracurriculars()->attach($request->input('extracurriculars', []), [
            'joined_at' => now()->toDateString(),
            'role' => 'member',
        ]);

        return redirect()->route('students.index')
                         ->with('success', 'Mahasiswa berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail mahasiswa beserta mata kuliah, nilai, dan ekstrakurikulernya.
     */
    public function show(Student $student)
    {
        $student->load(['user.profile', 'department', 'courses', 'grades.course', 'extracurriculars']);
        return view('students.show', compact('student'));
    }

    /**
     * Tampilkan form edit mahasiswa (khusus admin).
     */
    public function edit(Student $student)
    {
        Gate::authorize('admin');

        $users = $this->availableUsers($student->user_id);
        $departments = Department::orderBy('name')->get();
        $extracurriculars = Extracurricular::orderBy('name')->get();
        $student->load('extracurriculars');

        return view('students.edit', compact('student', 'users', 'departments', 'extracurriculars'));
    }

    /**
     * Update data mahasiswa (khusus admin).
     */
    public function update(Request $request, Student $student)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'user_id' => 'required|exists:users,id|unique:teachers,user_id|unique:students,user_id,' . $student->id,
            'department_id' => 'required|exists:departments,id',
            'nim' => 'required|string|max:20|unique:students,nim,' . $student->id,
            'semester' => 'required|integer|min:1|max:14',
            'extracurriculars' => 'nullable|array',
            'extracurriculars.*' => 'exists:extracurriculars,id',
        ]);

        $student->update(Arr::except($validated, 'extracurriculars'));

        // sync(): yang dicentang ditambahkan, yang tidak dicentang dilepas.
        // Ekstrakurikuler yang sudah diikuti sebelumnya tidak diubah (peran & tanggal tetap).
        $changes = $student->extracurriculars()->sync($request->input('extracurriculars', []));

        // Isi tanggal bergabung untuk ekstrakurikuler yang baru saja ditambahkan
        foreach ($changes['attached'] as $extracurricularId) {
            $student->extracurriculars()->updateExistingPivot($extracurricularId, [
                'joined_at' => now()->toDateString(),
            ]);
        }

        return redirect()->route('students.index')
                         ->with('success', 'Data mahasiswa berhasil diupdate.');
    }

    /**
     * Hapus data mahasiswa (khusus admin).
     * Data enrollment, submission, nilai, dan keanggotaan ekstrakurikuler ikut terhapus (cascade).
     */
    public function destroy(Student $student)
    {
        Gate::authorize('admin');

        $student->delete();

        return redirect()->route('students.index')
                         ->with('success', 'Data mahasiswa berhasil dihapus.');
    }

    /**
     * Daftar user ber-role "user" yang belum terdaftar sebagai dosen maupun mahasiswa.
     * Saat edit, user milik mahasiswa yang sedang diedit ikut dimasukkan.
     */
    private function availableUsers(?int $currentUserId = null)
    {
        return User::where('role', 'user')
                   ->doesntHave('teacher')
                   ->doesntHave('student')
                   ->when($currentUserId, fn ($query) => $query->orWhere('id', $currentUserId))
                   ->orderBy('name')
                   ->get();
    }
}
```

**Penjelasan:**

- Validasi `'extracurriculars' => 'nullable|array'` dan `'extracurriculars.*' => 'exists:extracurriculars,id'` memeriksa bahwa input berupa array dan **setiap** ID yang dicentang benar-benar ada.
- `Arr::except($validated, 'extracurriculars')` membuang key `extracurriculars` sebelum `create()`/`update()`, karena itu bukan kolom tabel `students`.
- **Store:** `attach($ids, ['joined_at' => ..., 'role' => 'member'])` menambahkan baris ke tabel pivot sekaligus mengisi kolom tambahannya.
- **Update:** `sync($ids)` menyamakan isi pivot dengan checkbox. Yang baru dicentang ditambahkan, yang tidak dicentang dilepas, dan yang tetap dicentang **tidak diubah** (peran dan tanggal bergabung lama tetap). `sync()` mengembalikan array berisi ID yang `attached`, `detached`, dan `updated`. ID yang baru ditambahkan lalu diisi `joined_at`-nya dengan `updateExistingPivot()`.
- Jika semua checkbox dikosongkan, browser **tidak mengirim** `extracurriculars` sama sekali. `$request->input('extracurriculars', [])` memberi nilai default array kosong, sehingga `sync([])` melepas semua ekstrakurikuler.
- Menghapus mahasiswa ikut menghapus enrollment, pengumpulan tugas, nilai, dan keanggotaan ekstrakurikulernya (cascade).

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view students.index
php artisan make:view students.create
php artisan make:view students.show
php artisan make:view students.edit
```

Perintah di atas membuat folder `resources/views/students/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/students/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Mahasiswa')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Mahasiswa</h2>
    @can('admin')
        <a href="{{ route('students.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Mahasiswa
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>NIM</th>
            <th>Nama Mahasiswa</th>
            <th>Jurusan</th>
            <th>Semester</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($students as $i => $student)
        <tr>
            <td>{{ $students->firstItem() + $i }}</td>
            <td>{{ $student->nim }}</td>
            <td>{{ $student->user->name }}</td>
            <td>{{ $student->department->name }}</td>
            <td>{{ $student->semester }}</td>
            <td>
                <a href="{{ route('students.show', $student) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('students.edit', $student) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('students.destroy', $student) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus? Data KRS, tugas, dan nilai mahasiswa ini ikut terhapus.')">
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
            <td colspan="6" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $students->links() }}
@endsection
```

**Penjelasan:**

- Struktur sama dengan modul Dosen.

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/students/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Mahasiswa')

@section('content')
<h2>Tambah Mahasiswa Baru</h2>

<div class="card">
    <div class="card-body">
        @if($users->isEmpty())
            <div class="alert alert-warning">
                Tidak ada user yang tersedia. Buat akun user baru terlebih dahulu di menu
                <a href="{{ route('users.create') }}">Users</a>.
            </div>
        @endif

        <form action="{{ route('students.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="user_id" class="form-label">Akun User <span class="text-danger">*</span></label>
                <select class="form-select @error('user_id') is-invalid @enderror" id="user_id" name="user_id" required>
                    <option value="">-- Pilih User --</option>
                    @foreach($users as $user)
                        <option value="{{ $user->id }}" {{ old('user_id') == $user->id ? 'selected' : '' }}>
                            {{ $user->name }} ({{ $user->email }})
                        </option>
                    @endforeach
                </select>
                <div class="form-text">Hanya user yang belum terdaftar sebagai dosen/mahasiswa yang ditampilkan.</div>
                @error('user_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="department_id" class="form-label">Jurusan <span class="text-danger">*</span></label>
                <select class="form-select @error('department_id') is-invalid @enderror" id="department_id" name="department_id" required>
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

            <div class="row">
                <div class="col-md-8 mb-3">
                    <label for="nim" class="form-label">NIM <span class="text-danger">*</span></label>
                    <input type="text" class="form-control @error('nim') is-invalid @enderror"
                           id="nim" name="nim" value="{{ old('nim') }}" maxlength="20" required>
                    @error('nim')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="semester" class="form-label">Semester <span class="text-danger">*</span></label>
                    <input type="number" class="form-control @error('semester') is-invalid @enderror"
                           id="semester" name="semester" value="{{ old('semester', 1) }}" min="1" max="14" required>
                    @error('semester')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
            </div>

            {{-- Many-to-Many: Ekstrakurikuler (Checkbox) --}}
            <div class="mb-3">
                <label class="form-label">Ekstrakurikuler yang Diikuti</label>
                <div class="row">
                    @foreach($extracurriculars as $extracurricular)
                        <div class="col-md-4">
                            <div class="form-check">
                                <input class="form-check-input" type="checkbox" name="extracurriculars[]"
                                       value="{{ $extracurricular->id }}" id="extracurricular{{ $extracurricular->id }}"
                                       {{ in_array($extracurricular->id, old('extracurriculars', [])) ? 'checked' : '' }}>
                                <label class="form-check-label" for="extracurricular{{ $extracurricular->id }}">
                                    {{ $extracurricular->name }}
                                </label>
                            </div>
                        </div>
                    @endforeach
                </div>
                @error('extracurriculars.*')
                    <div class="text-danger small">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('students.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- `name="extracurriculars[]"` (dengan `[]`) membuat PHP menerima input sebagai array ID.
- `in_array($extracurricular->id, old('extracurriculars', []))` menjaga checkbox tetap tercentang ketika validasi gagal.
- `@error('extracurriculars.*')` menampilkan error jika ada ID ekstrakurikuler yang tidak valid.

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/students/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Mahasiswa')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Mahasiswa: {{ $student->user->name }}</h2>
    <a href="{{ route('students.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card mb-4">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">NIM</th>
                <td>: {{ $student->nim }}</td>
            </tr>
            <tr>
                <th>Nama</th>
                <td>: <a href="{{ route('users.show', $student->user) }}">{{ $student->user->name }}</a></td>
            </tr>
            <tr>
                <th>Email</th>
                <td>: {{ $student->user->email }}</td>
            </tr>
            <tr>
                <th>No. HP</th>
                <td>: {{ $student->user->profile?->phone ?? '-' }}</td>
            </tr>
            <tr>
                <th>Jurusan</th>
                <td>: <a href="{{ route('departments.show', $student->department) }}">{{ $student->department->name }}</a></td>
            </tr>
            <tr>
                <th>Semester</th>
                <td>: {{ $student->semester }}</td>
            </tr>
        </table>
    </div>
</div>

{{-- Mata Kuliah yang Diambil (Relasi Many-to-Many lewat tabel enrollments) --}}
<div class="card mb-4">
    <div class="card-header">
        <h5 class="mb-0">Mata Kuliah yang Diambil ({{ $student->courses->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>Kode</th>
                    <th>Mata Kuliah</th>
                    <th>SKS</th>
                    <th>Tahun Ajaran</th>
                    <th>Semester</th>
                    <th>Status</th>
                </tr>
            </thead>
            <tbody>
                @forelse($student->courses as $course)
                <tr>
                    <td>{{ $course->code }}</td>
                    <td>{{ $course->name }}</td>
                    <td>{{ $course->credits }}</td>
                    {{-- Kolom tambahan dari tabel pivot diakses lewat ->pivot --}}
                    <td>{{ $course->pivot->academic_year }}</td>
                    <td>{{ $course->pivot->semester }}</td>
                    <td>{{ ucfirst($course->pivot->status) }}</td>
                </tr>
                @empty
                <tr><td colspan="6" class="text-center">Belum mengambil mata kuliah.</td></tr>
                @endforelse
            </tbody>
            @if($student->courses->isNotEmpty())
                <tfoot>
                    <tr>
                        <th colspan="2" class="text-end">Total SKS</th>
                        <th colspan="4">{{ $student->courses->sum('credits') }}</th>
                    </tr>
                </tfoot>
            @endif
        </table>
    </div>
</div>

{{-- Nilai (Relasi One-to-Many) --}}
<div class="card mb-4">
    <div class="card-header">
        <h5 class="mb-0">Nilai ({{ $student->grades->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>Mata Kuliah</th>
                    <th>Tahun Ajaran</th>
                    <th>UTS</th>
                    <th>UAS</th>
                    <th>Nilai Huruf</th>
                </tr>
            </thead>
            <tbody>
                @forelse($student->grades as $grade)
                <tr>
                    <td>{{ $grade->course->name }}</td>
                    <td>{{ $grade->academic_year }}</td>
                    <td>{{ $grade->midterm_score ?? '-' }}</td>
                    <td>{{ $grade->final_score ?? '-' }}</td>
                    <td><strong>{{ $grade->grade_letter ?? '-' }}</strong></td>
                </tr>
                @empty
                <tr><td colspan="5" class="text-center">Belum ada nilai.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>

{{-- Ekstrakurikuler (Relasi Many-to-Many) --}}
<div class="card">
    <div class="card-header">
        <h5 class="mb-0">Ekstrakurikuler ({{ $student->extracurriculars->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>Nama</th>
                    <th>Peran</th>
                    <th>Tanggal Bergabung</th>
                </tr>
            </thead>
            <tbody>
                @forelse($student->extracurriculars as $extracurricular)
                <tr>
                    <td>{{ $extracurricular->name }}</td>
                    <td>{{ ucfirst($extracurricular->pivot->role) }}</td>
                    <td>
                        {{ $extracurricular->pivot->joined_at
                            ? \Carbon\Carbon::parse($extracurricular->pivot->joined_at)->format('d M Y')
                            : '-' }}
                    </td>
                </tr>
                @empty
                <tr><td colspan="3" class="text-center">Tidak mengikuti ekstrakurikuler.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- `$course->pivot->academic_year`, `semester`, dan `status` berasal dari tabel `enrollments`, bisa diakses karena relasi di model `Student` memakai `withPivot('academic_year', 'semester', 'status')`.
- `$student->courses->sum('credits')` menjumlahkan SKS seluruh mata kuliah yang diambil.

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/students/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Mahasiswa')

@section('content')
<h2>Edit Mahasiswa: {{ $student->user->name }}</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('students.update', $student) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="user_id" class="form-label">Akun User <span class="text-danger">*</span></label>
                <select class="form-select @error('user_id') is-invalid @enderror" id="user_id" name="user_id" required>
                    @foreach($users as $user)
                        <option value="{{ $user->id }}" {{ old('user_id', $student->user_id) == $user->id ? 'selected' : '' }}>
                            {{ $user->name }} ({{ $user->email }})
                        </option>
                    @endforeach
                </select>
                @error('user_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="department_id" class="form-label">Jurusan <span class="text-danger">*</span></label>
                <select class="form-select @error('department_id') is-invalid @enderror" id="department_id" name="department_id" required>
                    @foreach($departments as $department)
                        <option value="{{ $department->id }}" {{ old('department_id', $student->department_id) == $department->id ? 'selected' : '' }}>
                            {{ $department->name }}
                        </option>
                    @endforeach
                </select>
                @error('department_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="row">
                <div class="col-md-8 mb-3">
                    <label for="nim" class="form-label">NIM <span class="text-danger">*</span></label>
                    <input type="text" class="form-control @error('nim') is-invalid @enderror"
                           id="nim" name="nim" value="{{ old('nim', $student->nim) }}" maxlength="20" required>
                    @error('nim')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="semester" class="form-label">Semester <span class="text-danger">*</span></label>
                    <input type="number" class="form-control @error('semester') is-invalid @enderror"
                           id="semester" name="semester" value="{{ old('semester', $student->semester) }}" min="1" max="14" required>
                    @error('semester')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
            </div>

            {{-- Many-to-Many: Ekstrakurikuler (Checkbox) - dengan pre-selected --}}
            <div class="mb-3">
                <label class="form-label">Ekstrakurikuler yang Diikuti</label>
                <div class="row">
                    @foreach($extracurriculars as $extracurricular)
                        <div class="col-md-4">
                            <div class="form-check">
                                <input class="form-check-input" type="checkbox" name="extracurriculars[]"
                                       value="{{ $extracurricular->id }}" id="extracurricular{{ $extracurricular->id }}"
                                       {{ in_array($extracurricular->id, old('extracurriculars', $student->extracurriculars->pluck('id')->toArray())) ? 'checked' : '' }}>
                                <label class="form-check-label" for="extracurricular{{ $extracurricular->id }}">
                                    {{ $extracurricular->name }}
                                </label>
                            </div>
                        </div>
                    @endforeach
                </div>
                @error('extracurriculars.*')
                    <div class="text-danger small">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('students.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Checkbox yang sudah dipilih diambil dari `$student->extracurriculars->pluck('id')->toArray()`.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Mahasiswa** → tampil 10 mahasiswa.
- [ ] Klik **Detail** pada **Andi Pratama** → tampil mata kuliah yang diambil beserta total SKS, nilai, dan ekstrakurikuler.
- [ ] Buat user baru di menu Users, lalu **Tambah Mahasiswa**: pilih user tersebut, jurusan, NIM baru, dan centang 2 ekstrakurikuler → simpan. Buka detailnya → 2 ekstrakurikuler tampil dengan peran *Member* dan tanggal hari ini.
- [ ] Edit mahasiswa tersebut: hilangkan 1 centang dan tambahkan 1 centang lain → yang dihilangkan terlepas, yang baru muncul, dan yang tetap dicentang tidak berubah tanggal bergabungnya.
- [ ] Buka **Detail Ekstrakurikuler** yang dipilih → mahasiswa baru muncul sebagai anggota.
- [ ] Simpan mahasiswa dengan NIM yang sudah dipakai → muncul error.
- [ ] Hapus mahasiswa baru → data enrollment, nilai, dan keanggotaan ekstrakurikulernya ikut terhapus.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Tolak pendaftaran ekstrakurikuler yang kuotanya sudah penuh (bandingkan `students()->count()` dengan `max_members`).

[⬅ Modul 08: Dosen](#modul-08-dosen-teachers) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 10: Mata Kuliah ➡](#modul-10-mata-kuliah-courses)

---

### Modul 10: Mata Kuliah (`courses`)

[⬅ Modul 09: Mahasiswa](#modul-09-mahasiswa-students) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 11: Jadwal Perkuliahan ➡](#modul-11-jadwal-perkuliahan-schedules)

Data mata kuliah. Mata kuliah memiliki kategori dan jurusan (**Many-to-One**), serta tag (**Many-to-Many** lewat `course_tag`). Halaman detailnya menampilkan hampir semua jenis relasi: jadwal, tugas, dan mahasiswa yang mengambil.

#### 🎯 Yang Dipelajari

- Dua dropdown relasi dan checkbox Many-to-Many dalam satu form
- `sync()` untuk relasi Many-to-Many tanpa kolom tambahan
- Menampilkan badge tag di daftar data
- Halaman detail dengan banyak relasi sekaligus

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| Many-to-One | `courses` → `categories` | Banyak mata kuliah dalam satu kategori |
| Many-to-One | `courses` → `departments` | Banyak mata kuliah milik satu jurusan |
| Many-to-Many | `courses` ↔ `tags` (pivot `course_tag`) | Mata kuliah memiliki banyak tag |
| One-to-Many | `courses` → `schedules`, `assignments`, `grades` | Mata kuliah memiliki banyak jadwal, tugas, dan nilai |
| Many-to-Many | `courses` ↔ `students` (pivot `enrollments`) | Mata kuliah diambil banyak mahasiswa |

#### 📋 Sebelum Mulai

- Modul 01 (Jurusan), 02 (Kategori), dan 04 (Tags) sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/CourseController.php` | Controller (logika CRUD) |
| `resources/views/courses/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/courses/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/courses/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/courses/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller CourseController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/CourseController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Category;
use App\Models\Course;
use App\Models\Department;
use App\Models\Tag;
use Illuminate\Http\Request;
use Illuminate\Support\Arr;
use Illuminate\Support\Facades\Gate;

class CourseController extends Controller
{
    /**
     * Tampilkan semua mata kuliah beserta kategori, jurusan, dan tag-nya.
     */
    public function index()
    {
        $courses = Course::with(['category', 'department', 'tags'])->orderBy('code')->paginate(10);
        return view('courses.index', compact('courses'));
    }

    /**
     * Tampilkan form tambah mata kuliah (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');

        $categories = Category::orderBy('name')->get();
        $departments = Department::orderBy('name')->get();
        $tags = Tag::orderBy('name')->get();

        return view('courses.create', compact('categories', 'departments', 'tags'));
    }

    /**
     * Simpan mata kuliah baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'category_id' => 'required|exists:categories,id',
            'department_id' => 'required|exists:departments,id',
            'code' => 'required|string|max:10|unique:courses,code',
            'name' => 'required|string|max:255',
            'credits' => 'required|integer|min:1|max:6',
            'description' => 'nullable|string',
            'tags' => 'nullable|array',
            'tags.*' => 'exists:tags,id',
        ]);

        $course = Course::create(Arr::except($validated, 'tags'));

        // Many-to-Many: simpan relasi ke tabel pivot course_tag
        $course->tags()->sync($request->input('tags', []));

        return redirect()->route('courses.index')
                         ->with('success', 'Mata kuliah berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail mata kuliah beserta seluruh relasinya.
     */
    public function show(Course $course)
    {
        $course->load([
            'category',
            'department',
            'tags',
            'schedules.teacher.user',
            'schedules.classroom',
            'assignments',
            'students.user',
        ]);

        return view('courses.show', compact('course'));
    }

    /**
     * Tampilkan form edit mata kuliah (khusus admin).
     */
    public function edit(Course $course)
    {
        Gate::authorize('admin');

        $categories = Category::orderBy('name')->get();
        $departments = Department::orderBy('name')->get();
        $tags = Tag::orderBy('name')->get();
        $course->load('tags');

        return view('courses.edit', compact('course', 'categories', 'departments', 'tags'));
    }

    /**
     * Update data mata kuliah (khusus admin).
     */
    public function update(Request $request, Course $course)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'category_id' => 'required|exists:categories,id',
            'department_id' => 'required|exists:departments,id',
            'code' => 'required|string|max:10|unique:courses,code,' . $course->id,
            'name' => 'required|string|max:255',
            'credits' => 'required|integer|min:1|max:6',
            'description' => 'nullable|string',
            'tags' => 'nullable|array',
            'tags.*' => 'exists:tags,id',
        ]);

        $course->update(Arr::except($validated, 'tags'));

        // sync(): tag yang tidak dicentang akan dilepas, yang baru dicentang ditambahkan
        $course->tags()->sync($request->input('tags', []));

        return redirect()->route('courses.index')
                         ->with('success', 'Mata kuliah berhasil diupdate.');
    }

    /**
     * Hapus mata kuliah (khusus admin).
     * Jadwal, tugas, enrollment, nilai, dan relasi tag ikut terhapus (cascade).
     */
    public function destroy(Course $course)
    {
        Gate::authorize('admin');

        $course->delete();

        return redirect()->route('courses.index')
                         ->with('success', 'Mata kuliah berhasil dihapus.');
    }
}
```

**Penjelasan:**

- `Course::with(['category', 'department', 'tags'])` memuat relasi untuk seluruh baris sekaligus (*eager loading*), sehingga daftar 10 mata kuliah cukup dengan beberapa query, bukan puluhan.
- `$course->tags()->sync($request->input('tags', []))` dipakai di `store` **dan** `update`. Pada mata kuliah baru, `sync()` sama dengan `attach()`. Pada update, tag yang tidak dicentang otomatis dilepas.
- `Arr::except($validated, 'tags')` membuang key `tags` karena bukan kolom tabel `courses`.
- Menghapus mata kuliah ikut menghapus jadwal, tugas, enrollment, nilai, dan relasi tag-nya (cascade).

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view courses.index
php artisan make:view courses.create
php artisan make:view courses.show
php artisan make:view courses.edit
```

Perintah di atas membuat folder `resources/views/courses/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/courses/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Mata Kuliah')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Mata Kuliah</h2>
    @can('admin')
        <a href="{{ route('courses.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Mata Kuliah
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Kode</th>
            <th>Nama Mata Kuliah</th>
            <th>SKS</th>
            <th>Kategori</th>
            <th>Jurusan</th>
            <th>Tags</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($courses as $i => $course)
        <tr>
            <td>{{ $courses->firstItem() + $i }}</td>
            <td><span class="badge bg-secondary">{{ $course->code }}</span></td>
            <td>{{ $course->name }}</td>
            <td>{{ $course->credits }}</td>
            <td>{{ $course->category->name }}</td>
            <td>{{ $course->department->name }}</td>
            <td>
                @foreach($course->tags as $tag)
                    <span class="badge bg-info text-dark">{{ $tag->name }}</span>
                @endforeach
            </td>
            <td>
                <a href="{{ route('courses.show', $course) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('courses.edit', $course) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('courses.destroy', $course) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus? Jadwal, tugas, KRS, dan nilai mata kuliah ini ikut terhapus.')">
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
            <td colspan="8" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $courses->links() }}
@endsection
```

**Penjelasan:**

- Tag ditampilkan sebagai badge dengan `@foreach($course->tags as $tag)`.

#### Langkah 4: View Create (Create: form tambah data)

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

**Penjelasan:**

- Form ini menggabungkan **dua dropdown relasi** (Kategori dan Jurusan) dengan **checkbox Many-to-Many** (Tags).

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/courses/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Mata Kuliah')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Mata Kuliah: {{ $course->name }}</h2>
    <a href="{{ route('courses.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card mb-4">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">Kode</th>
                <td>: {{ $course->code }}</td>
            </tr>
            <tr>
                <th>Nama Mata Kuliah</th>
                <td>: {{ $course->name }}</td>
            </tr>
            <tr>
                <th>SKS</th>
                <td>: {{ $course->credits }}</td>
            </tr>
            <tr>
                <th>Kategori</th>
                <td>: <a href="{{ route('categories.show', $course->category) }}">{{ $course->category->name }}</a></td>
            </tr>
            <tr>
                <th>Jurusan</th>
                <td>: <a href="{{ route('departments.show', $course->department) }}">{{ $course->department->name }}</a></td>
            </tr>
            <tr>
                <th>Deskripsi</th>
                <td>: {{ $course->description ?? '-' }}</td>
            </tr>
            <tr>
                <th>Tags</th>
                <td>:
                    {{-- Relasi Many-to-Many --}}
                    @forelse($course->tags as $tag)
                        <a href="{{ route('tags.show', $tag) }}" class="badge bg-info text-dark text-decoration-none">{{ $tag->name }}</a>
                    @empty
                        -
                    @endforelse
                </td>
            </tr>
        </table>
    </div>
</div>

{{-- Jadwal (Relasi One-to-Many) --}}
<div class="card mb-4">
    <div class="card-header">
        <h5 class="mb-0">Jadwal Perkuliahan ({{ $course->schedules->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>Hari</th>
                    <th>Jam</th>
                    <th>Dosen</th>
                    <th>Ruangan</th>
                </tr>
            </thead>
            <tbody>
                @forelse($course->schedules as $schedule)
                <tr>
                    <td>{{ $schedule->day }}</td>
                    <td>{{ substr($schedule->start_time, 0, 5) }} - {{ substr($schedule->end_time, 0, 5) }}</td>
                    <td>{{ $schedule->teacher->user->name }}</td>
                    <td>{{ $schedule->classroom->name }}</td>
                </tr>
                @empty
                <tr><td colspan="4" class="text-center">Belum ada jadwal.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>

{{-- Tugas (Relasi One-to-Many) --}}
<div class="card mb-4">
    <div class="card-header">
        <h5 class="mb-0">Tugas ({{ $course->assignments->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>Judul</th>
                    <th>Deadline</th>
                </tr>
            </thead>
            <tbody>
                @forelse($course->assignments as $assignment)
                <tr>
                    <td><a href="{{ route('assignments.show', $assignment) }}">{{ $assignment->title }}</a></td>
                    <td>{{ $assignment->due_date->format('d M Y H:i') }}</td>
                </tr>
                @empty
                <tr><td colspan="2" class="text-center">Belum ada tugas.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>

{{-- Mahasiswa yang Mengambil (Relasi Many-to-Many lewat tabel enrollments) --}}
<div class="card">
    <div class="card-header">
        <h5 class="mb-0">Mahasiswa Terdaftar ({{ $course->students->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>NIM</th>
                    <th>Nama</th>
                    <th>Tahun Ajaran</th>
                    <th>Status</th>
                </tr>
            </thead>
            <tbody>
                @forelse($course->students as $student)
                <tr>
                    <td>{{ $student->nim }}</td>
                    <td>{{ $student->user->name }}</td>
                    <td>{{ $student->pivot->academic_year }} ({{ $student->pivot->semester }})</td>
                    <td>{{ ucfirst($student->pivot->status) }}</td>
                </tr>
                @empty
                <tr><td colspan="4" class="text-center">Belum ada mahasiswa terdaftar.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Tag ditampilkan sebagai link ke detail tag.
- Tabel **Mahasiswa Terdaftar** membaca relasi Many-to-Many `$course->students` beserta kolom pivot `academic_year`, `semester`, dan `status`.

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/courses/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Mata Kuliah')

@section('content')
<h2>Edit Mata Kuliah: {{ $course->name }}</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('courses.update', $course) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="category_id" class="form-label">Kategori <span class="text-danger">*</span></label>
                <select class="form-select @error('category_id') is-invalid @enderror"
                        id="category_id" name="category_id" required>
                    @foreach($categories as $category)
                        <option value="{{ $category->id }}" {{ old('category_id', $course->category_id) == $category->id ? 'selected' : '' }}>
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
                    @foreach($departments as $department)
                        <option value="{{ $department->id }}" {{ old('department_id', $course->department_id) == $department->id ? 'selected' : '' }}>
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
                       id="code" name="code" value="{{ old('code', $course->code) }}" maxlength="10" required>
                @error('code')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="name" class="form-label">Nama Mata Kuliah <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('name') is-invalid @enderror"
                       id="name" name="name" value="{{ old('name', $course->name) }}" required>
                @error('name')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="credits" class="form-label">SKS <span class="text-danger">*</span></label>
                <input type="number" class="form-control @error('credits') is-invalid @enderror"
                       id="credits" name="credits" value="{{ old('credits', $course->credits) }}" min="1" max="6" required>
                @error('credits')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="description" class="form-label">Deskripsi</label>
                <textarea class="form-control" id="description" name="description" rows="3">{{ old('description', $course->description) }}</textarea>
            </div>

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

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
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

**Penjelasan:**

- Checkbox tag yang sudah dipilih diambil dari `$course->tags->pluck('id')->toArray()`.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Mata Kuliah** → tampil 10 mata kuliah lengkap dengan badge tag.
- [ ] Tambah mata kuliah dengan SKS 9 → muncul error (maksimal 6).
- [ ] Tambah `TI999` - `Cloud Computing`, centang tag Backend dan Wajib → badge tampil di daftar.
- [ ] Edit dan ubah centang tag → badge berubah. Kosongkan semua centang → tidak ada tag.
- [ ] Klik **Detail** pada **Pemrograman Web** → tampil tag, jadwal, tugas, dan mahasiswa terdaftar.
- [ ] Hapus mata kuliah baru.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Tambahkan filter dropdown kategori di halaman index (petunjuk: `->when($request->category_id, fn ($q, $id) => $q->where('category_id', $id))`).

[⬅ Modul 09: Mahasiswa](#modul-09-mahasiswa-students) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 11: Jadwal Perkuliahan ➡](#modul-11-jadwal-perkuliahan-schedules)

---

### Modul 11: Jadwal Perkuliahan (`schedules`)

[⬅ Modul 10: Mata Kuliah](#modul-10-mata-kuliah-courses) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 12: Enrollment (KRS) ➡](#modul-12-enrollment-krs-enrollments)

Jadwal perkuliahan menghubungkan tiga tabel sekaligus: mata kuliah, dosen, dan ruangan (semuanya **Many-to-One**).

#### 🎯 Yang Dipelajari

- Konstanta class untuk daftar pilihan (`DAYS`)
- Method `private` `rules()` dan `formData()` agar kode tidak ditulis dua kali
- Validasi jam: `date_format:H:i` dan `after:start_time`
- Input `type="time"`
- Pengurutan kolom ENUM di MySQL

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| Many-to-One | `schedules` → `courses` | Jadwal untuk satu mata kuliah |
| Many-to-One | `schedules` → `teachers` | Jadwal diajar satu dosen |
| Many-to-One | `schedules` → `classrooms` | Jadwal memakai satu ruangan |

#### 📋 Sebelum Mulai

- Modul 03 (Ruangan), 08 (Dosen), dan 10 (Mata Kuliah) sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/ScheduleController.php` | Controller (logika CRUD) |
| `resources/views/schedules/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/schedules/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/schedules/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/schedules/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller ScheduleController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/ScheduleController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Classroom;
use App\Models\Course;
use App\Models\Schedule;
use App\Models\Teacher;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;

class ScheduleController extends Controller
{
    /**
     * Daftar hari kuliah, sama dengan isi kolom enum 'day' di migration.
     */
    private const DAYS = ['Senin', 'Selasa', 'Rabu', 'Kamis', 'Jumat', 'Sabtu'];

    /**
     * Tampilkan semua jadwal, diurutkan per hari lalu jam mulai.
     * Di MySQL, kolom ENUM diurutkan sesuai urutan definisinya (Senin, Selasa, ...),
     * bukan urutan abjad.
     */
    public function index()
    {
        $schedules = Schedule::with(['course', 'teacher.user', 'classroom'])
                             ->orderBy('day')
                             ->orderBy('start_time')
                             ->paginate(10);

        return view('schedules.index', compact('schedules'));
    }

    /**
     * Tampilkan form tambah jadwal (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');

        return view('schedules.create', $this->formData());
    }

    /**
     * Simpan jadwal baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate($this->rules());

        Schedule::create($validated);

        return redirect()->route('schedules.index')
                         ->with('success', 'Jadwal berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail jadwal.
     */
    public function show(Schedule $schedule)
    {
        $schedule->load(['course.department', 'teacher.user', 'classroom']);
        return view('schedules.show', compact('schedule'));
    }

    /**
     * Tampilkan form edit jadwal (khusus admin).
     */
    public function edit(Schedule $schedule)
    {
        Gate::authorize('admin');

        return view('schedules.edit', array_merge($this->formData(), compact('schedule')));
    }

    /**
     * Update data jadwal (khusus admin).
     */
    public function update(Request $request, Schedule $schedule)
    {
        Gate::authorize('admin');

        $validated = $request->validate($this->rules());

        $schedule->update($validated);

        return redirect()->route('schedules.index')
                         ->with('success', 'Jadwal berhasil diupdate.');
    }

    /**
     * Hapus jadwal (khusus admin).
     */
    public function destroy(Schedule $schedule)
    {
        Gate::authorize('admin');

        $schedule->delete();

        return redirect()->route('schedules.index')
                         ->with('success', 'Jadwal berhasil dihapus.');
    }

    /**
     * Aturan validasi yang sama untuk store & update.
     */
    private function rules(): array
    {
        return [
            'course_id' => 'required|exists:courses,id',
            'teacher_id' => 'required|exists:teachers,id',
            'classroom_id' => 'required|exists:classrooms,id',
            'day' => 'required|in:' . implode(',', self::DAYS),
            'start_time' => 'required|date_format:H:i',
            'end_time' => 'required|date_format:H:i|after:start_time',
        ];
    }

    /**
     * Data dropdown yang dibutuhkan form create & edit.
     */
    private function formData(): array
    {
        return [
            'courses' => Course::orderBy('name')->get(),
            'teachers' => Teacher::with('user')->get()->sortBy('user.name'),
            'classrooms' => Classroom::orderBy('name')->get(),
            'days' => self::DAYS,
        ];
    }
}
```

**Penjelasan:**

- `private const DAYS` menyimpan daftar hari di satu tempat. Daftar ini dipakai untuk aturan validasi `in:` dan dikirim ke view untuk dropdown.
- `rules()` dipakai oleh `store` dan `update`, sedangkan `formData()` dipakai oleh `create` dan `edit`. `array_merge($this->formData(), compact('schedule'))` menggabungkan data dropdown dengan data jadwal yang diedit.
- `date_format:H:i` memastikan format jam seperti `08:00`. `after:start_time` memastikan jam selesai lebih besar dari jam mulai.
- `orderBy('day')`: di MySQL, kolom ENUM diurutkan berdasarkan urutan saat didefinisikan di migration (Senin, Selasa, ...), bukan berdasarkan abjad.
- `Teacher::with('user')->get()->sortBy('user.name')` mengurutkan dosen berdasarkan nama. Nama ada di tabel `users`, sehingga diurutkan setelah data diambil.

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view schedules.index
php artisan make:view schedules.create
php artisan make:view schedules.show
php artisan make:view schedules.edit
```

Perintah di atas membuat folder `resources/views/schedules/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/schedules/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Jadwal Perkuliahan')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Jadwal Perkuliahan</h2>
    @can('admin')
        <a href="{{ route('schedules.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Jadwal
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Hari</th>
            <th>Jam</th>
            <th>Mata Kuliah</th>
            <th>Dosen</th>
            <th>Ruangan</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($schedules as $i => $schedule)
        <tr>
            <td>{{ $schedules->firstItem() + $i }}</td>
            <td>{{ $schedule->day }}</td>
            {{-- Kolom TIME tersimpan sebagai "08:00:00", ambil 5 karakter pertama -> "08:00" --}}
            <td>{{ substr($schedule->start_time, 0, 5) }} - {{ substr($schedule->end_time, 0, 5) }}</td>
            <td>{{ $schedule->course->name }}</td>
            <td>{{ $schedule->teacher->user->name }}</td>
            <td>{{ $schedule->classroom->name }} ({{ $schedule->classroom->building }})</td>
            <td>
                <a href="{{ route('schedules.show', $schedule) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('schedules.edit', $schedule) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('schedules.destroy', $schedule) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus jadwal ini?')">
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

{{ $schedules->links() }}
@endsection
```

**Penjelasan:**

- Kolom TIME tersimpan sebagai `08:00:00`, ditampilkan `08:00` dengan `substr(..., 0, 5)`.

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/schedules/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Jadwal')

@section('content')
<h2>Tambah Jadwal Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('schedules.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="course_id" class="form-label">Mata Kuliah <span class="text-danger">*</span></label>
                <select class="form-select @error('course_id') is-invalid @enderror" id="course_id" name="course_id" required>
                    <option value="">-- Pilih Mata Kuliah --</option>
                    @foreach($courses as $course)
                        <option value="{{ $course->id }}" {{ old('course_id') == $course->id ? 'selected' : '' }}>
                            {{ $course->code }} - {{ $course->name }}
                        </option>
                    @endforeach
                </select>
                @error('course_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="teacher_id" class="form-label">Dosen <span class="text-danger">*</span></label>
                <select class="form-select @error('teacher_id') is-invalid @enderror" id="teacher_id" name="teacher_id" required>
                    <option value="">-- Pilih Dosen --</option>
                    @foreach($teachers as $teacher)
                        <option value="{{ $teacher->id }}" {{ old('teacher_id') == $teacher->id ? 'selected' : '' }}>
                            {{ $teacher->user->name }}
                        </option>
                    @endforeach
                </select>
                @error('teacher_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="classroom_id" class="form-label">Ruangan <span class="text-danger">*</span></label>
                <select class="form-select @error('classroom_id') is-invalid @enderror" id="classroom_id" name="classroom_id" required>
                    <option value="">-- Pilih Ruangan --</option>
                    @foreach($classrooms as $classroom)
                        <option value="{{ $classroom->id }}" {{ old('classroom_id') == $classroom->id ? 'selected' : '' }}>
                            {{ $classroom->name }} - {{ $classroom->building }} (kapasitas {{ $classroom->capacity }})
                        </option>
                    @endforeach
                </select>
                @error('classroom_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="row">
                <div class="col-md-4 mb-3">
                    <label for="day" class="form-label">Hari <span class="text-danger">*</span></label>
                    <select class="form-select @error('day') is-invalid @enderror" id="day" name="day" required>
                        <option value="">-- Pilih Hari --</option>
                        @foreach($days as $day)
                            <option value="{{ $day }}" {{ old('day') === $day ? 'selected' : '' }}>{{ $day }}</option>
                        @endforeach
                    </select>
                    @error('day')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="start_time" class="form-label">Jam Mulai <span class="text-danger">*</span></label>
                    <input type="time" class="form-control @error('start_time') is-invalid @enderror"
                           id="start_time" name="start_time" value="{{ old('start_time') }}" required>
                    @error('start_time')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="end_time" class="form-label">Jam Selesai <span class="text-danger">*</span></label>
                    <input type="time" class="form-control @error('end_time') is-invalid @enderror"
                           id="end_time" name="end_time" value="{{ old('end_time') }}" required>
                    @error('end_time')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('schedules.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Hari dipilih dari `$days` (konstanta `DAYS` di controller), jam memakai `<input type="time">`.

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/schedules/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Jadwal')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Jadwal</h2>
    <a href="{{ route('schedules.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">Hari</th>
                <td>: {{ $schedule->day }}</td>
            </tr>
            <tr>
                <th>Jam</th>
                <td>: {{ substr($schedule->start_time, 0, 5) }} - {{ substr($schedule->end_time, 0, 5) }}</td>
            </tr>
            {{-- Relasi Many-to-One: schedule milik satu course, teacher, dan classroom --}}
            <tr>
                <th>Mata Kuliah</th>
                <td>: <a href="{{ route('courses.show', $schedule->course) }}">{{ $schedule->course->code }} - {{ $schedule->course->name }}</a>
                    ({{ $schedule->course->credits }} SKS)</td>
            </tr>
            <tr>
                <th>Jurusan</th>
                <td>: {{ $schedule->course->department->name }}</td>
            </tr>
            <tr>
                <th>Dosen</th>
                <td>: <a href="{{ route('teachers.show', $schedule->teacher) }}">{{ $schedule->teacher->user->name }}</a></td>
            </tr>
            <tr>
                <th>Ruangan</th>
                <td>: <a href="{{ route('classrooms.show', $schedule->classroom) }}">{{ $schedule->classroom->name }}</a>
                    - {{ $schedule->classroom->building }}</td>
            </tr>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Mata kuliah, dosen, dan ruangan dibuat sebagai link ke halaman detail masing-masing.

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/schedules/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Jadwal')

@section('content')
<h2>Edit Jadwal</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('schedules.update', $schedule) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="course_id" class="form-label">Mata Kuliah <span class="text-danger">*</span></label>
                <select class="form-select @error('course_id') is-invalid @enderror" id="course_id" name="course_id" required>
                    @foreach($courses as $course)
                        <option value="{{ $course->id }}" {{ old('course_id', $schedule->course_id) == $course->id ? 'selected' : '' }}>
                            {{ $course->code }} - {{ $course->name }}
                        </option>
                    @endforeach
                </select>
                @error('course_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="teacher_id" class="form-label">Dosen <span class="text-danger">*</span></label>
                <select class="form-select @error('teacher_id') is-invalid @enderror" id="teacher_id" name="teacher_id" required>
                    @foreach($teachers as $teacher)
                        <option value="{{ $teacher->id }}" {{ old('teacher_id', $schedule->teacher_id) == $teacher->id ? 'selected' : '' }}>
                            {{ $teacher->user->name }}
                        </option>
                    @endforeach
                </select>
                @error('teacher_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="classroom_id" class="form-label">Ruangan <span class="text-danger">*</span></label>
                <select class="form-select @error('classroom_id') is-invalid @enderror" id="classroom_id" name="classroom_id" required>
                    @foreach($classrooms as $classroom)
                        <option value="{{ $classroom->id }}" {{ old('classroom_id', $schedule->classroom_id) == $classroom->id ? 'selected' : '' }}>
                            {{ $classroom->name }} - {{ $classroom->building }} (kapasitas {{ $classroom->capacity }})
                        </option>
                    @endforeach
                </select>
                @error('classroom_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="row">
                <div class="col-md-4 mb-3">
                    <label for="day" class="form-label">Hari <span class="text-danger">*</span></label>
                    <select class="form-select @error('day') is-invalid @enderror" id="day" name="day" required>
                        @foreach($days as $day)
                            <option value="{{ $day }}" {{ old('day', $schedule->day) === $day ? 'selected' : '' }}>{{ $day }}</option>
                        @endforeach
                    </select>
                    @error('day')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="start_time" class="form-label">Jam Mulai <span class="text-danger">*</span></label>
                    {{-- Nilai dari database "08:00:00" dipotong menjadi "08:00" agar lolos validasi date_format:H:i --}}
                    <input type="time" class="form-control @error('start_time') is-invalid @enderror"
                           id="start_time" name="start_time"
                           value="{{ old('start_time', substr($schedule->start_time, 0, 5)) }}" required>
                    @error('start_time')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="end_time" class="form-label">Jam Selesai <span class="text-danger">*</span></label>
                    <input type="time" class="form-control @error('end_time') is-invalid @enderror"
                           id="end_time" name="end_time"
                           value="{{ old('end_time', substr($schedule->end_time, 0, 5)) }}" required>
                    @error('end_time')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('schedules.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Nilai jam dari database (`08:00:00`) dipotong menjadi `08:00` dengan `substr(..., 0, 5)` agar sesuai format input `type="time"` dan lolos validasi `date_format:H:i`.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Jadwal** → jadwal tampil urut dari Senin sampai Jumat.
- [ ] Tambah jadwal dengan jam selesai lebih kecil dari jam mulai → muncul error.
- [ ] Tambah jadwal hari Sabtu jam 13:00–14:40 → tampil paling akhir di daftar.
- [ ] Edit jadwal tersebut tanpa mengubah apa pun lalu klik **Update** → berhasil tanpa error format jam.
- [ ] Klik **Detail** → tampil mata kuliah, dosen, dan ruangan yang bisa diklik.
- [ ] Hapus jadwal baru.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Tolak jadwal yang bentrok: ruangan sama, hari sama, dan jam beririsan (petunjuk: `where('start_time', '<', $end)->where('end_time', '>', $start)`).

[⬅ Modul 10: Mata Kuliah](#modul-10-mata-kuliah-courses) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 12: Enrollment (KRS) ➡](#modul-12-enrollment-krs-enrollments)

---

### Modul 12: Enrollment (KRS) (`enrollments`)

[⬅ Modul 11: Jadwal Perkuliahan](#modul-11-jadwal-perkuliahan-schedules) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 13: Tugas ➡](#modul-13-tugas-assignments)

Enrollment (Kartu Rencana Studi) adalah **tabel pivot** Many-to-Many antara mahasiswa dan mata kuliah. Karena memiliki model sendiri (`Enrollment`) dan kolom tambahan (tahun ajaran, semester, status), tabel pivot ini bisa di-CRUD seperti tabel biasa.

#### 🎯 Yang Dipelajari

- CRUD langsung pada tabel pivot
- Validasi *unique* gabungan beberapa kolom dengan `Rule::unique()->where()`
- Validasi format dengan `regex`
- Pesan error kustom
- Warna badge berdasarkan status

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| Many-to-One | `enrollments` → `students` | Setiap baris KRS milik satu mahasiswa |
| Many-to-One | `enrollments` → `courses` | Setiap baris KRS untuk satu mata kuliah |
| Many-to-Many | `students` ↔ `courses` | Secara keseluruhan, tabel ini menghubungkan mahasiswa dan mata kuliah |

#### 📋 Sebelum Mulai

- Modul 09 (Mahasiswa) dan 10 (Mata Kuliah) sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/EnrollmentController.php` | Controller (logika CRUD) |
| `resources/views/enrollments/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/enrollments/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/enrollments/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/enrollments/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller EnrollmentController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/EnrollmentController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Course;
use App\Models\Enrollment;
use App\Models\Student;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;
use Illuminate\Validation\Rule;

class EnrollmentController extends Controller
{
    /**
     * Tampilkan semua data enrollment (KRS).
     * Tabel enrollments adalah tabel pivot Many-to-Many antara students dan courses
     * yang juga punya model sendiri, sehingga bisa di-CRUD seperti tabel biasa.
     */
    public function index()
    {
        $enrollments = Enrollment::with(['student.user', 'course'])->latest()->paginate(10);
        return view('enrollments.index', compact('enrollments'));
    }

    /**
     * Tampilkan form tambah enrollment (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');

        return view('enrollments.create', $this->formData());
    }

    /**
     * Simpan enrollment baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate($this->rules($request), $this->messages());

        Enrollment::create($validated);

        return redirect()->route('enrollments.index')
                         ->with('success', 'Enrollment berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail enrollment.
     */
    public function show(Enrollment $enrollment)
    {
        $enrollment->load(['student.user', 'student.department', 'course.department']);
        return view('enrollments.show', compact('enrollment'));
    }

    /**
     * Tampilkan form edit enrollment (khusus admin).
     */
    public function edit(Enrollment $enrollment)
    {
        Gate::authorize('admin');

        return view('enrollments.edit', array_merge($this->formData(), compact('enrollment')));
    }

    /**
     * Update data enrollment (khusus admin).
     */
    public function update(Request $request, Enrollment $enrollment)
    {
        Gate::authorize('admin');

        $validated = $request->validate($this->rules($request, $enrollment), $this->messages());

        $enrollment->update($validated);

        return redirect()->route('enrollments.index')
                         ->with('success', 'Enrollment berhasil diupdate.');
    }

    /**
     * Hapus enrollment (khusus admin).
     */
    public function destroy(Enrollment $enrollment)
    {
        Gate::authorize('admin');

        $enrollment->delete();

        return redirect()->route('enrollments.index')
                         ->with('success', 'Enrollment berhasil dihapus.');
    }

    /**
     * Aturan validasi untuk store & update.
     * Mahasiswa tidak boleh mengambil mata kuliah yang sama dua kali
     * pada tahun ajaran & semester yang sama.
     */
    private function rules(Request $request, ?Enrollment $enrollment = null): array
    {
        $uniqueEnrollment = Rule::unique('enrollments')
            ->where('course_id', $request->input('course_id'))
            ->where('academic_year', $request->input('academic_year'))
            ->where('semester', $request->input('semester'));

        // Saat update, abaikan data yang sedang diedit
        if ($enrollment) {
            $uniqueEnrollment->ignore($enrollment->id);
        }

        return [
            'student_id' => ['required', 'exists:students,id', $uniqueEnrollment],
            'course_id' => ['required', 'exists:courses,id'],
            'academic_year' => ['required', 'string', 'max:9', 'regex:/^\d{4}\/\d{4}$/'],
            'semester' => ['required', 'in:Ganjil,Genap'],
            'status' => ['required', 'in:active,dropped,completed'],
        ];
    }

    /**
     * Pesan error kustom.
     */
    private function messages(): array
    {
        return [
            'student_id.unique' => 'Mahasiswa ini sudah terdaftar di mata kuliah tersebut pada tahun ajaran & semester yang sama.',
            'academic_year.regex' => 'Format tahun ajaran harus seperti 2025/2026.',
        ];
    }

    /**
     * Data dropdown untuk form create & edit.
     */
    private function formData(): array
    {
        return [
            'students' => Student::with('user')->orderBy('nim')->get(),
            'courses' => Course::orderBy('code')->get(),
        ];
    }
}
```

**Penjelasan:**

- `Rule::unique('enrollments')->where(...)` memeriksa apakah kombinasi mahasiswa + mata kuliah + tahun ajaran + semester sudah ada. Aturan ini ditempel pada field `student_id`, sehingga pesan errornya muncul di dropdown mahasiswa.
- Saat update, `->ignore($enrollment->id)` mengecualikan data yang sedang diedit.
- Aturan yang memakai `regex` **harus** ditulis dalam bentuk array (`['required', 'regex:/.../']`), bukan string dengan pemisah `|`, karena pola regex bisa mengandung karakter `|`.
- Parameter kedua `validate($rules, $messages)` berisi pesan error kustom dalam bahasa Indonesia.

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view enrollments.index
php artisan make:view enrollments.create
php artisan make:view enrollments.show
php artisan make:view enrollments.edit
```

Perintah di atas membuat folder `resources/views/enrollments/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/enrollments/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Enrollment')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Enrollment (KRS)</h2>
    @can('admin')
        <a href="{{ route('enrollments.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Enrollment
        </a>
    @endcan
</div>

@php
    // Warna badge untuk setiap status
    $statusColors = ['active' => 'success', 'dropped' => 'danger', 'completed' => 'secondary'];
@endphp

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>NIM</th>
            <th>Nama Mahasiswa</th>
            <th>Mata Kuliah</th>
            <th>Tahun Ajaran</th>
            <th>Semester</th>
            <th>Status</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($enrollments as $i => $enrollment)
        <tr>
            <td>{{ $enrollments->firstItem() + $i }}</td>
            <td>{{ $enrollment->student->nim }}</td>
            <td>{{ $enrollment->student->user->name }}</td>
            <td>{{ $enrollment->course->code }} - {{ $enrollment->course->name }}</td>
            <td>{{ $enrollment->academic_year }}</td>
            <td>{{ $enrollment->semester }}</td>
            <td>
                <span class="badge bg-{{ $statusColors[$enrollment->status] }}">
                    {{ ucfirst($enrollment->status) }}
                </span>
            </td>
            <td>
                <a href="{{ route('enrollments.show', $enrollment) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('enrollments.edit', $enrollment) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('enrollments.destroy', $enrollment) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus enrollment ini?')">
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
            <td colspan="8" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $enrollments->links() }}
@endsection
```

**Penjelasan:**

- Blok `@php ... @endphp` mendefinisikan array warna badge, lalu `$statusColors[$enrollment->status]` memilih warna sesuai status.

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/enrollments/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Enrollment')

@section('content')
<h2>Tambah Enrollment Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('enrollments.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="student_id" class="form-label">Mahasiswa <span class="text-danger">*</span></label>
                <select class="form-select @error('student_id') is-invalid @enderror" id="student_id" name="student_id" required>
                    <option value="">-- Pilih Mahasiswa --</option>
                    @foreach($students as $student)
                        <option value="{{ $student->id }}" {{ old('student_id') == $student->id ? 'selected' : '' }}>
                            {{ $student->nim }} - {{ $student->user->name }}
                        </option>
                    @endforeach
                </select>
                @error('student_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="course_id" class="form-label">Mata Kuliah <span class="text-danger">*</span></label>
                <select class="form-select @error('course_id') is-invalid @enderror" id="course_id" name="course_id" required>
                    <option value="">-- Pilih Mata Kuliah --</option>
                    @foreach($courses as $course)
                        <option value="{{ $course->id }}" {{ old('course_id') == $course->id ? 'selected' : '' }}>
                            {{ $course->code }} - {{ $course->name }} ({{ $course->credits }} SKS)
                        </option>
                    @endforeach
                </select>
                @error('course_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="row">
                <div class="col-md-4 mb-3">
                    <label for="academic_year" class="form-label">Tahun Ajaran <span class="text-danger">*</span></label>
                    <input type="text" class="form-control @error('academic_year') is-invalid @enderror"
                           id="academic_year" name="academic_year" value="{{ old('academic_year', '2025/2026') }}"
                           placeholder="2025/2026" maxlength="9" required>
                    @error('academic_year')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="semester" class="form-label">Semester <span class="text-danger">*</span></label>
                    <select class="form-select @error('semester') is-invalid @enderror" id="semester" name="semester" required>
                        <option value="Ganjil" {{ old('semester') === 'Ganjil' ? 'selected' : '' }}>Ganjil</option>
                        <option value="Genap" {{ old('semester') === 'Genap' ? 'selected' : '' }}>Genap</option>
                    </select>
                    @error('semester')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="status" class="form-label">Status <span class="text-danger">*</span></label>
                    <select class="form-select @error('status') is-invalid @enderror" id="status" name="status" required>
                        <option value="active" {{ old('status', 'active') === 'active' ? 'selected' : '' }}>Active</option>
                        <option value="dropped" {{ old('status') === 'dropped' ? 'selected' : '' }}>Dropped</option>
                        <option value="completed" {{ old('status') === 'completed' ? 'selected' : '' }}>Completed</option>
                    </select>
                    @error('status')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('enrollments.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Tahun ajaran diisi default `2025/2026` dengan `old('academic_year', '2025/2026')`.

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/enrollments/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Enrollment')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Enrollment</h2>
    <a href="{{ route('enrollments.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="row">
    <div class="col-md-6 mb-4">
        <div class="card h-100">
            <div class="card-header"><h5 class="mb-0">Mahasiswa</h5></div>
            <div class="card-body">
                <table class="table table-borderless mb-0">
                    <tr>
                        <th width="120">NIM</th>
                        <td>: {{ $enrollment->student->nim }}</td>
                    </tr>
                    <tr>
                        <th>Nama</th>
                        <td>: <a href="{{ route('students.show', $enrollment->student) }}">{{ $enrollment->student->user->name }}</a></td>
                    </tr>
                    <tr>
                        <th>Jurusan</th>
                        <td>: {{ $enrollment->student->department->name }}</td>
                    </tr>
                </table>
            </div>
        </div>
    </div>
    <div class="col-md-6 mb-4">
        <div class="card h-100">
            <div class="card-header"><h5 class="mb-0">Mata Kuliah</h5></div>
            <div class="card-body">
                <table class="table table-borderless mb-0">
                    <tr>
                        <th width="120">Kode</th>
                        <td>: {{ $enrollment->course->code }}</td>
                    </tr>
                    <tr>
                        <th>Nama</th>
                        <td>: <a href="{{ route('courses.show', $enrollment->course) }}">{{ $enrollment->course->name }}</a></td>
                    </tr>
                    <tr>
                        <th>SKS</th>
                        <td>: {{ $enrollment->course->credits }}</td>
                    </tr>
                </table>
            </div>
        </div>
    </div>
</div>

<div class="card">
    <div class="card-header"><h5 class="mb-0">Data Enrollment</h5></div>
    <div class="card-body">
        <table class="table table-borderless mb-0">
            <tr>
                <th width="200">Tahun Ajaran</th>
                <td>: {{ $enrollment->academic_year }}</td>
            </tr>
            <tr>
                <th>Semester</th>
                <td>: {{ $enrollment->semester }}</td>
            </tr>
            <tr>
                <th>Status</th>
                <td>: {{ ucfirst($enrollment->status) }}</td>
            </tr>
            <tr>
                <th>Didaftarkan pada</th>
                <td>: {{ $enrollment->created_at->format('d M Y H:i') }}</td>
            </tr>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Data mahasiswa dan mata kuliah ditampilkan berdampingan memakai grid Bootstrap (`row` dan `col-md-6`).

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/enrollments/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Enrollment')

@section('content')
<h2>Edit Enrollment</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('enrollments.update', $enrollment) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="student_id" class="form-label">Mahasiswa <span class="text-danger">*</span></label>
                <select class="form-select @error('student_id') is-invalid @enderror" id="student_id" name="student_id" required>
                    @foreach($students as $student)
                        <option value="{{ $student->id }}" {{ old('student_id', $enrollment->student_id) == $student->id ? 'selected' : '' }}>
                            {{ $student->nim }} - {{ $student->user->name }}
                        </option>
                    @endforeach
                </select>
                @error('student_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="course_id" class="form-label">Mata Kuliah <span class="text-danger">*</span></label>
                <select class="form-select @error('course_id') is-invalid @enderror" id="course_id" name="course_id" required>
                    @foreach($courses as $course)
                        <option value="{{ $course->id }}" {{ old('course_id', $enrollment->course_id) == $course->id ? 'selected' : '' }}>
                            {{ $course->code }} - {{ $course->name }} ({{ $course->credits }} SKS)
                        </option>
                    @endforeach
                </select>
                @error('course_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="row">
                <div class="col-md-4 mb-3">
                    <label for="academic_year" class="form-label">Tahun Ajaran <span class="text-danger">*</span></label>
                    <input type="text" class="form-control @error('academic_year') is-invalid @enderror"
                           id="academic_year" name="academic_year"
                           value="{{ old('academic_year', $enrollment->academic_year) }}" maxlength="9" required>
                    @error('academic_year')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="semester" class="form-label">Semester <span class="text-danger">*</span></label>
                    <select class="form-select @error('semester') is-invalid @enderror" id="semester" name="semester" required>
                        @foreach(['Ganjil', 'Genap'] as $semester)
                            <option value="{{ $semester }}" {{ old('semester', $enrollment->semester) === $semester ? 'selected' : '' }}>
                                {{ $semester }}
                            </option>
                        @endforeach
                    </select>
                    @error('semester')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="status" class="form-label">Status <span class="text-danger">*</span></label>
                    <select class="form-select @error('status') is-invalid @enderror" id="status" name="status" required>
                        @foreach(['active', 'dropped', 'completed'] as $status)
                            <option value="{{ $status }}" {{ old('status', $enrollment->status) === $status ? 'selected' : '' }}>
                                {{ ucfirst($status) }}
                            </option>
                        @endforeach
                    </select>
                    @error('status')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('enrollments.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Pilihan semester dan status dibuat dengan `@foreach` agar tidak menulis `<option>` satu per satu.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Lainnya → Enrollment** → tampil data KRS dari seeder.
- [ ] Tambah enrollment dengan tahun ajaran `2026-2027` → muncul error *Format tahun ajaran harus seperti 2025/2026.*
- [ ] Tambah enrollment untuk mahasiswa dan mata kuliah yang sudah terdaftar (tahun ajaran dan semester sama) → muncul error *sudah terdaftar*.
- [ ] Tambah enrollment yang valid → muncul di daftar. Buka **Detail Mahasiswa** tersebut → mata kuliah baru ikut tampil (relasi Many-to-Many).
- [ ] Edit status menjadi `Completed` → warna badge berubah menjadi abu-abu.
- [ ] Hapus enrollment baru.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Batasi total SKS per mahasiswa per semester maksimal 24.

[⬅ Modul 11: Jadwal Perkuliahan](#modul-11-jadwal-perkuliahan-schedules) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 13: Tugas ➡](#modul-13-tugas-assignments)

---

### Modul 13: Tugas (`assignments`)

[⬅ Modul 12: Enrollment (KRS)](#modul-12-enrollment-krs-enrollments) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 14: Pengumpulan Tugas ➡](#modul-14-pengumpulan-tugas-submissions)

Data tugas per mata kuliah. Setiap tugas memiliki banyak pengumpulan (*submissions*).

#### 🎯 Yang Dipelajari

- Input `datetime-local` dan formatnya (`Y-m-d\TH:i`)
- Cast `datetime` di model dan method Carbon (`format()`, `isPast()`)
- Aturan validasi yang berbeda untuk tambah dan edit

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| Many-to-One | `assignments` → `courses` | Tugas milik satu mata kuliah |
| One-to-Many | `assignments` → `submissions` | Satu tugas dikumpulkan banyak mahasiswa |

#### 📋 Sebelum Mulai

- Modul 10 (Mata Kuliah) sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/AssignmentController.php` | Controller (logika CRUD) |
| `resources/views/assignments/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/assignments/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/assignments/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/assignments/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller AssignmentController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/AssignmentController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Assignment;
use App\Models\Course;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;

class AssignmentController extends Controller
{
    /**
     * Tampilkan semua tugas, diurutkan dari deadline terdekat.
     */
    public function index()
    {
        $assignments = Assignment::with('course')
                                 ->withCount('submissions')
                                 ->orderBy('due_date')
                                 ->paginate(10);

        return view('assignments.index', compact('assignments'));
    }

    /**
     * Tampilkan form tambah tugas (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');

        $courses = Course::orderBy('code')->get();

        return view('assignments.create', compact('courses'));
    }

    /**
     * Simpan tugas baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'course_id' => 'required|exists:courses,id',
            'title' => 'required|string|max:255',
            'description' => 'nullable|string',
            'due_date' => 'required|date|after:today', // tugas baru tidak boleh punya deadline di masa lalu
        ]);

        Assignment::create($validated);

        return redirect()->route('assignments.index')
                         ->with('success', 'Tugas berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail tugas beserta daftar pengumpulannya (One-to-Many).
     */
    public function show(Assignment $assignment)
    {
        $assignment->load(['course', 'submissions.student.user']);
        return view('assignments.show', compact('assignment'));
    }

    /**
     * Tampilkan form edit tugas (khusus admin).
     */
    public function edit(Assignment $assignment)
    {
        Gate::authorize('admin');

        $courses = Course::orderBy('code')->get();

        return view('assignments.edit', compact('assignment', 'courses'));
    }

    /**
     * Update data tugas (khusus admin).
     */
    public function update(Request $request, Assignment $assignment)
    {
        Gate::authorize('admin');

        $validated = $request->validate([
            'course_id' => 'required|exists:courses,id',
            'title' => 'required|string|max:255',
            'description' => 'nullable|string',
            // Saat edit tidak memakai after:today, agar tugas yang deadline-nya
            // sudah lewat tetap bisa diubah judul/deskripsinya
            'due_date' => 'required|date',
        ]);

        $assignment->update($validated);

        return redirect()->route('assignments.index')
                         ->with('success', 'Tugas berhasil diupdate.');
    }

    /**
     * Hapus tugas (khusus admin).
     * Semua pengumpulan (submission) untuk tugas ini ikut terhapus (cascade).
     */
    public function destroy(Assignment $assignment)
    {
        Gate::authorize('admin');

        $assignment->delete();

        return redirect()->route('assignments.index')
                         ->with('success', 'Tugas berhasil dihapus.');
    }
}
```

**Penjelasan:**

- `withCount('submissions')` menghasilkan `submissions_count`, dan `orderBy('due_date')` mengurutkan dari deadline terdekat.
- Store memakai `after:today` (deadline tugas baru tidak boleh di masa lalu), sedangkan update cukup `date`, agar tugas lama yang deadline-nya sudah lewat tetap bisa diedit.
- Menghapus tugas ikut menghapus seluruh pengumpulannya (cascade).

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view assignments.index
php artisan make:view assignments.create
php artisan make:view assignments.show
php artisan make:view assignments.edit
```

Perintah di atas membuat folder `resources/views/assignments/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/assignments/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Tugas')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Tugas</h2>
    @can('admin')
        <a href="{{ route('assignments.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Tugas
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Judul Tugas</th>
            <th>Mata Kuliah</th>
            <th>Deadline</th>
            <th>Pengumpulan</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($assignments as $i => $assignment)
        <tr>
            <td>{{ $assignments->firstItem() + $i }}</td>
            <td>{{ $assignment->title }}</td>
            <td>{{ $assignment->course->name }}</td>
            <td>
                {{-- due_date sudah di-cast ke datetime di model, jadi bisa langsung di-format --}}
                {{ $assignment->due_date->format('d M Y H:i') }}
                @if($assignment->due_date->isPast())
                    <span class="badge bg-danger">Lewat</span>
                @endif
            </td>
            <td>{{ $assignment->submissions_count }} mahasiswa</td>
            <td>
                <a href="{{ route('assignments.show', $assignment) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('assignments.edit', $assignment) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('assignments.destroy', $assignment) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus? Semua pengumpulan tugas ini ikut terhapus.')">
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
            <td colspan="6" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $assignments->links() }}
@endsection
```

**Penjelasan:**

- `due_date` di-cast `datetime` di model `Assignment`, sehingga bisa langsung memakai `->format()` dan `->isPast()` untuk menampilkan badge **Lewat**.

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/assignments/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Tugas')

@section('content')
<h2>Tambah Tugas Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('assignments.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="course_id" class="form-label">Mata Kuliah <span class="text-danger">*</span></label>
                <select class="form-select @error('course_id') is-invalid @enderror" id="course_id" name="course_id" required>
                    <option value="">-- Pilih Mata Kuliah --</option>
                    @foreach($courses as $course)
                        <option value="{{ $course->id }}" {{ old('course_id') == $course->id ? 'selected' : '' }}>
                            {{ $course->code }} - {{ $course->name }}
                        </option>
                    @endforeach
                </select>
                @error('course_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="title" class="form-label">Judul Tugas <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('title') is-invalid @enderror"
                       id="title" name="title" value="{{ old('title') }}" required>
                @error('title')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="description" class="form-label">Deskripsi</label>
                <textarea class="form-control @error('description') is-invalid @enderror"
                          id="description" name="description" rows="4">{{ old('description') }}</textarea>
                @error('description')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="due_date" class="form-label">Deadline <span class="text-danger">*</span></label>
                <input type="datetime-local" class="form-control @error('due_date') is-invalid @enderror"
                       id="due_date" name="due_date" value="{{ old('due_date') }}" required>
                @error('due_date')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('assignments.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Deadline memakai `<input type="datetime-local">` yang mengirim nilai seperti `2025-10-15T23:59`.

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/assignments/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Tugas')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Tugas: {{ $assignment->title }}</h2>
    <a href="{{ route('assignments.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card mb-4">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">Judul</th>
                <td>: {{ $assignment->title }}</td>
            </tr>
            <tr>
                <th>Mata Kuliah</th>
                <td>: <a href="{{ route('courses.show', $assignment->course) }}">{{ $assignment->course->code }} - {{ $assignment->course->name }}</a></td>
            </tr>
            <tr>
                <th>Deadline</th>
                <td>: {{ $assignment->due_date->format('d M Y H:i') }}
                    @if($assignment->due_date->isPast())
                        <span class="badge bg-danger">Sudah lewat</span>
                    @else
                        <span class="badge bg-success">Masih dibuka</span>
                    @endif
                </td>
            </tr>
            <tr>
                <th>Deskripsi</th>
                <td>: {{ $assignment->description ?? '-' }}</td>
            </tr>
        </table>
    </div>
</div>

{{-- Daftar Pengumpulan (Relasi One-to-Many) --}}
<div class="card">
    <div class="card-header">
        <h5 class="mb-0">Pengumpulan Tugas ({{ $assignment->submissions->count() }})</h5>
    </div>
    <div class="card-body">
        <table class="table table-sm table-striped">
            <thead>
                <tr>
                    <th>NIM</th>
                    <th>Nama Mahasiswa</th>
                    <th>Waktu Pengumpulan</th>
                    <th>Nilai</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                @forelse($assignment->submissions as $submission)
                <tr>
                    <td>{{ $submission->student->nim }}</td>
                    <td>{{ $submission->student->user->name }}</td>
                    <td>{{ $submission->submitted_at?->format('d M Y H:i') ?? '-' }}</td>
                    <td>{{ $submission->score ?? 'Belum dinilai' }}</td>
                    <td><a href="{{ route('submissions.show', $submission) }}" class="btn btn-sm btn-outline-info">Detail</a></td>
                </tr>
                @empty
                <tr><td colspan="5" class="text-center">Belum ada yang mengumpulkan.</td></tr>
                @endforelse
            </tbody>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Daftar pengumpulan memakai `$submission->submitted_at?->format(...)` karena kolom ini boleh kosong.

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/assignments/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Tugas')

@section('content')
<h2>Edit Tugas: {{ $assignment->title }}</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('assignments.update', $assignment) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="course_id" class="form-label">Mata Kuliah <span class="text-danger">*</span></label>
                <select class="form-select @error('course_id') is-invalid @enderror" id="course_id" name="course_id" required>
                    @foreach($courses as $course)
                        <option value="{{ $course->id }}" {{ old('course_id', $assignment->course_id) == $course->id ? 'selected' : '' }}>
                            {{ $course->code }} - {{ $course->name }}
                        </option>
                    @endforeach
                </select>
                @error('course_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="title" class="form-label">Judul Tugas <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('title') is-invalid @enderror"
                       id="title" name="title" value="{{ old('title', $assignment->title) }}" required>
                @error('title')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="description" class="form-label">Deskripsi</label>
                <textarea class="form-control @error('description') is-invalid @enderror"
                          id="description" name="description" rows="4">{{ old('description', $assignment->description) }}</textarea>
                @error('description')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="due_date" class="form-label">Deadline <span class="text-danger">*</span></label>
                {{-- Input datetime-local membutuhkan format "Y-m-d\TH:i", contoh: 2025-10-15T23:59 --}}
                <input type="datetime-local" class="form-control @error('due_date') is-invalid @enderror"
                       id="due_date" name="due_date"
                       value="{{ old('due_date', $assignment->due_date->format('Y-m-d\TH:i')) }}" required>
                @error('due_date')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('assignments.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Input `datetime-local` membutuhkan format `2025-10-15T23:59`, sehingga nilai lama ditulis `format('Y-m-d\TH:i')`. Huruf `T` di-escape menjadi `\T` agar tidak dianggap kode format tanggal.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Lainnya → Tugas** → tampil 20 tugas, 10 per halaman. Klik tombol halaman 2.
- [ ] Tambah tugas dengan deadline kemarin → muncul error.
- [ ] Tambah tugas dengan deadline 3 hari lagi → tampil di daftar.
- [ ] Edit tugas tersebut dan ubah deadline ke tanggal yang sudah lewat → tersimpan, badge **Lewat** muncul.
- [ ] Klik **Detail** pada salah satu tugas dari seeder → tampil daftar mahasiswa yang mengumpulkan.
- [ ] Hapus tugas baru.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Tampilkan sisa waktu menuju deadline memakai `$assignment->due_date->diffForHumans()`.

[⬅ Modul 12: Enrollment (KRS)](#modul-12-enrollment-krs-enrollments) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 14: Pengumpulan Tugas ➡](#modul-14-pengumpulan-tugas-submissions)

---

### Modul 14: Pengumpulan Tugas (`submissions`)

[⬅ Modul 13: Tugas](#modul-13-tugas-assignments) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 15: Nilai ➡](#modul-15-nilai-grades)

Pengumpulan tugas oleh mahasiswa, lengkap dengan **upload file** tugas dan pemberian nilai.

#### 🎯 Yang Dipelajari

- Upload dokumen (PDF/DOC/DOCX/ZIP) dengan validasi `mimes`
- Nilai default (`submitted_at` = waktu sekarang)
- *Unique* gabungan: satu mahasiswa hanya satu pengumpulan per tugas
- Mengecek keberadaan file dengan `Storage::exists()`

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| Many-to-One | `submissions` → `assignments` | Pengumpulan untuk satu tugas |
| Many-to-One | `submissions` → `students` | Pengumpulan oleh satu mahasiswa |

#### 📋 Sebelum Mulai

- Modul 09 (Mahasiswa) dan 13 (Tugas) sudah selesai.
- `php artisan storage:link` sudah dijalankan (lihat Modul 07).

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/SubmissionController.php` | Controller (logika CRUD) |
| `resources/views/submissions/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/submissions/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/submissions/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/submissions/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller SubmissionController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/SubmissionController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Assignment;
use App\Models\Student;
use App\Models\Submission;
use Illuminate\Http\Request;
use Illuminate\Support\Arr;
use Illuminate\Support\Facades\Gate;
use Illuminate\Support\Facades\Storage;
use Illuminate\Validation\Rule;

class SubmissionController extends Controller
{
    /**
     * Tampilkan semua pengumpulan tugas.
     */
    public function index()
    {
        $submissions = Submission::with(['assignment.course', 'student.user'])->latest()->paginate(10);
        return view('submissions.index', compact('submissions'));
    }

    /**
     * Tampilkan form tambah pengumpulan (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');

        return view('submissions.create', $this->formData());
    }

    /**
     * Simpan pengumpulan baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate($this->rules($request), $this->messages());

        // Simpan file ke storage/app/public/submissions, path-nya disimpan di kolom file_path
        if ($request->hasFile('file')) {
            $validated['file_path'] = $request->file('file')->store('submissions', 'public');
        }

        // Jika waktu pengumpulan tidak diisi, gunakan waktu sekarang
        $validated['submitted_at'] = $validated['submitted_at'] ?? now();

        // 'file' bukan nama kolom, jadi dibuang sebelum disimpan
        Submission::create(Arr::except($validated, 'file'));

        return redirect()->route('submissions.index')
                         ->with('success', 'Pengumpulan tugas berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail pengumpulan.
     */
    public function show(Submission $submission)
    {
        $submission->load(['assignment.course', 'student.user']);
        return view('submissions.show', compact('submission'));
    }

    /**
     * Tampilkan form edit pengumpulan (khusus admin).
     */
    public function edit(Submission $submission)
    {
        Gate::authorize('admin');

        return view('submissions.edit', array_merge($this->formData(), compact('submission')));
    }

    /**
     * Update data pengumpulan (khusus admin), misalnya untuk memberi nilai.
     */
    public function update(Request $request, Submission $submission)
    {
        Gate::authorize('admin');

        $validated = $request->validate($this->rules($request, $submission), $this->messages());

        if ($request->hasFile('file')) {
            // Hapus file lama sebelum menyimpan file baru
            if ($submission->file_path) {
                Storage::disk('public')->delete($submission->file_path);
            }
            $validated['file_path'] = $request->file('file')->store('submissions', 'public');
        }

        $submission->update(Arr::except($validated, 'file'));

        return redirect()->route('submissions.index')
                         ->with('success', 'Pengumpulan tugas berhasil diupdate.');
    }

    /**
     * Hapus pengumpulan beserta file-nya (khusus admin).
     */
    public function destroy(Submission $submission)
    {
        Gate::authorize('admin');

        if ($submission->file_path) {
            Storage::disk('public')->delete($submission->file_path);
        }

        $submission->delete();

        return redirect()->route('submissions.index')
                         ->with('success', 'Pengumpulan tugas berhasil dihapus.');
    }

    /**
     * Aturan validasi untuk store & update.
     * Satu mahasiswa hanya boleh punya satu pengumpulan per tugas.
     */
    private function rules(Request $request, ?Submission $submission = null): array
    {
        $uniqueSubmission = Rule::unique('submissions')
            ->where('assignment_id', $request->input('assignment_id'));

        if ($submission) {
            $uniqueSubmission->ignore($submission->id);
        }

        return [
            'assignment_id' => ['required', 'exists:assignments,id'],
            'student_id' => ['required', 'exists:students,id', $uniqueSubmission],
            'file' => ['nullable', 'file', 'mimes:pdf,doc,docx,zip', 'max:2048'], // maksimal 2 MB
            'notes' => ['nullable', 'string'],
            'submitted_at' => ['nullable', 'date'],
            'score' => ['nullable', 'numeric', 'min:0', 'max:100'],
        ];
    }

    /**
     * Pesan error kustom.
     */
    private function messages(): array
    {
        return [
            'student_id.unique' => 'Mahasiswa ini sudah mengumpulkan tugas tersebut.',
        ];
    }

    /**
     * Data dropdown untuk form create & edit.
     */
    private function formData(): array
    {
        return [
            'assignments' => Assignment::with('course')->orderBy('due_date')->get(),
            'students' => Student::with('user')->orderBy('nim')->get(),
        ];
    }
}
```

**Penjelasan:**

- Nama input file adalah `file`, sedangkan nama kolom tabelnya `file_path`. Path hasil `store()` dimasukkan ke `$validated['file_path']`, lalu key `file` dibuang dengan `Arr::except()`.
- `mimes:pdf,doc,docx,zip` membatasi jenis file dan `max:2048` membatasi ukuran maksimal 2 MB.
- `$validated['submitted_at'] ?? now()` mengisi waktu pengumpulan otomatis jika dikosongkan.
- `Rule::unique('submissions')->where('assignment_id', ...)` mencegah satu mahasiswa mengumpulkan tugas yang sama dua kali.
- Pada update, file lama hanya dihapus jika ada file baru yang di-upload. Saat data dihapus, file-nya ikut dihapus.

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view submissions.index
php artisan make:view submissions.create
php artisan make:view submissions.show
php artisan make:view submissions.edit
```

Perintah di atas membuat folder `resources/views/submissions/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/submissions/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Pengumpulan Tugas')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Pengumpulan Tugas</h2>
    @can('admin')
        <a href="{{ route('submissions.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Pengumpulan
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Tugas</th>
            <th>Mata Kuliah</th>
            <th>Mahasiswa</th>
            <th>Waktu Pengumpulan</th>
            <th>Nilai</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($submissions as $i => $submission)
        <tr>
            <td>{{ $submissions->firstItem() + $i }}</td>
            <td>{{ $submission->assignment->title }}</td>
            <td>{{ $submission->assignment->course->name }}</td>
            <td>{{ $submission->student->nim }} - {{ $submission->student->user->name }}</td>
            <td>{{ $submission->submitted_at?->format('d M Y H:i') ?? '-' }}</td>
            <td>
                @if(is_null($submission->score))
                    <span class="badge bg-secondary">Belum dinilai</span>
                @else
                    {{ $submission->score }}
                @endif
            </td>
            <td>
                <a href="{{ route('submissions.show', $submission) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('submissions.edit', $submission) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('submissions.destroy', $submission) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus pengumpulan ini?')">
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

{{ $submissions->links() }}
@endsection
```

**Penjelasan:**

- Jika `score` masih `null`, ditampilkan badge **Belum dinilai**.

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/submissions/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Pengumpulan')

@section('content')
<h2>Tambah Pengumpulan Tugas</h2>

<div class="card">
    <div class="card-body">
        {{-- enctype="multipart/form-data" WAJIB ada untuk upload file --}}
        <form action="{{ route('submissions.store') }}" method="POST" enctype="multipart/form-data">
            @csrf

            <div class="mb-3">
                <label for="assignment_id" class="form-label">Tugas <span class="text-danger">*</span></label>
                <select class="form-select @error('assignment_id') is-invalid @enderror" id="assignment_id" name="assignment_id" required>
                    <option value="">-- Pilih Tugas --</option>
                    @foreach($assignments as $assignment)
                        <option value="{{ $assignment->id }}" {{ old('assignment_id') == $assignment->id ? 'selected' : '' }}>
                            [{{ $assignment->course->code }}] {{ $assignment->title }}
                        </option>
                    @endforeach
                </select>
                @error('assignment_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="student_id" class="form-label">Mahasiswa <span class="text-danger">*</span></label>
                <select class="form-select @error('student_id') is-invalid @enderror" id="student_id" name="student_id" required>
                    <option value="">-- Pilih Mahasiswa --</option>
                    @foreach($students as $student)
                        <option value="{{ $student->id }}" {{ old('student_id') == $student->id ? 'selected' : '' }}>
                            {{ $student->nim }} - {{ $student->user->name }}
                        </option>
                    @endforeach
                </select>
                @error('student_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="file" class="form-label">File Tugas</label>
                <input type="file" class="form-control @error('file') is-invalid @enderror"
                       id="file" name="file" accept=".pdf,.doc,.docx,.zip">
                <div class="form-text">Format PDF, DOC, DOCX, atau ZIP. Maksimal 2 MB.</div>
                @error('file')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="notes" class="form-label">Catatan</label>
                <textarea class="form-control @error('notes') is-invalid @enderror"
                          id="notes" name="notes" rows="3">{{ old('notes') }}</textarea>
                @error('notes')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="row">
                <div class="col-md-6 mb-3">
                    <label for="submitted_at" class="form-label">Waktu Pengumpulan</label>
                    <input type="datetime-local" class="form-control @error('submitted_at') is-invalid @enderror"
                           id="submitted_at" name="submitted_at" value="{{ old('submitted_at') }}">
                    <div class="form-text">Kosongkan untuk memakai waktu sekarang.</div>
                    @error('submitted_at')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-6 mb-3">
                    <label for="score" class="form-label">Nilai (0-100)</label>
                    <input type="number" class="form-control @error('score') is-invalid @enderror"
                           id="score" name="score" value="{{ old('score') }}" min="0" max="100" step="0.01">
                    <div class="form-text">Kosongkan jika belum dinilai.</div>
                    @error('score')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('submissions.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Form **wajib** memakai `enctype="multipart/form-data"`.
- Input nilai memakai `step="0.01"` agar bisa diisi angka desimal (misal 88.5).

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/submissions/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Pengumpulan')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Pengumpulan Tugas</h2>
    <a href="{{ route('submissions.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">Tugas</th>
                <td>: <a href="{{ route('assignments.show', $submission->assignment) }}">{{ $submission->assignment->title }}</a></td>
            </tr>
            <tr>
                <th>Mata Kuliah</th>
                <td>: {{ $submission->assignment->course->code }} - {{ $submission->assignment->course->name }}</td>
            </tr>
            <tr>
                <th>Deadline</th>
                <td>: {{ $submission->assignment->due_date->format('d M Y H:i') }}</td>
            </tr>
            <tr>
                <th>Mahasiswa</th>
                <td>: <a href="{{ route('students.show', $submission->student) }}">{{ $submission->student->nim }} - {{ $submission->student->user->name }}</a></td>
            </tr>
            <tr>
                <th>Waktu Pengumpulan</th>
                <td>: {{ $submission->submitted_at?->format('d M Y H:i') ?? '-' }}
                    @if($submission->submitted_at && $submission->submitted_at->gt($submission->assignment->due_date))
                        <span class="badge bg-danger">Terlambat</span>
                    @endif
                </td>
            </tr>
            <tr>
                <th>File</th>
                <td>:
                    @if($submission->file_path && Storage::disk('public')->exists($submission->file_path))
                        <a href="{{ asset('storage/' . $submission->file_path) }}" target="_blank" class="btn btn-sm btn-outline-primary">
                            <i class="bi bi-download"></i> Unduh File
                        </a>
                    @elseif($submission->file_path)
                        {{ $submission->file_path }} <span class="text-muted">(file tidak ditemukan di storage)</span>
                    @else
                        -
                    @endif
                </td>
            </tr>
            <tr>
                <th>Catatan</th>
                <td>: {{ $submission->notes ?? '-' }}</td>
            </tr>
            <tr>
                <th>Nilai</th>
                <td>: <strong>{{ $submission->score ?? 'Belum dinilai' }}</strong></td>
            </tr>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Data dummy dari seeder hanya menyimpan *nama* file (file aslinya tidak ada). Karena itu `Storage::disk('public')->exists()` dipakai untuk menampilkan keterangan "file tidak ditemukan" alih-alih link yang rusak.
- Badge **Terlambat** muncul jika `submitted_at` lebih besar dari deadline tugas (`gt()` = *greater than*).

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/submissions/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Pengumpulan')

@section('content')
<h2>Edit Pengumpulan Tugas</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('submissions.update', $submission) }}" method="POST" enctype="multipart/form-data">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="assignment_id" class="form-label">Tugas <span class="text-danger">*</span></label>
                <select class="form-select @error('assignment_id') is-invalid @enderror" id="assignment_id" name="assignment_id" required>
                    @foreach($assignments as $assignment)
                        <option value="{{ $assignment->id }}" {{ old('assignment_id', $submission->assignment_id) == $assignment->id ? 'selected' : '' }}>
                            [{{ $assignment->course->code }}] {{ $assignment->title }}
                        </option>
                    @endforeach
                </select>
                @error('assignment_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="student_id" class="form-label">Mahasiswa <span class="text-danger">*</span></label>
                <select class="form-select @error('student_id') is-invalid @enderror" id="student_id" name="student_id" required>
                    @foreach($students as $student)
                        <option value="{{ $student->id }}" {{ old('student_id', $submission->student_id) == $student->id ? 'selected' : '' }}>
                            {{ $student->nim }} - {{ $student->user->name }}
                        </option>
                    @endforeach
                </select>
                @error('student_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="file" class="form-label">File Tugas</label>
                @if($submission->file_path)
                    <div class="form-text mb-1">File saat ini: <code>{{ $submission->file_path }}</code></div>
                @endif
                <input type="file" class="form-control @error('file') is-invalid @enderror"
                       id="file" name="file" accept=".pdf,.doc,.docx,.zip">
                <div class="form-text">Kosongkan jika tidak ingin mengganti file.</div>
                @error('file')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="notes" class="form-label">Catatan</label>
                <textarea class="form-control @error('notes') is-invalid @enderror"
                          id="notes" name="notes" rows="3">{{ old('notes', $submission->notes) }}</textarea>
                @error('notes')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="row">
                <div class="col-md-6 mb-3">
                    <label for="submitted_at" class="form-label">Waktu Pengumpulan</label>
                    <input type="datetime-local" class="form-control @error('submitted_at') is-invalid @enderror"
                           id="submitted_at" name="submitted_at"
                           value="{{ old('submitted_at', $submission->submitted_at?->format('Y-m-d\TH:i')) }}">
                    @error('submitted_at')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-6 mb-3">
                    <label for="score" class="form-label">Nilai (0-100)</label>
                    <input type="number" class="form-control @error('score') is-invalid @enderror"
                           id="score" name="score" value="{{ old('score', $submission->score) }}" min="0" max="100" step="0.01">
                    @error('score')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('submissions.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Path file saat ini ditampilkan di atas input file. Kosongkan input jika tidak ingin mengganti file.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Lainnya → Submission** → tampil data pengumpulan dari seeder.
- [ ] Tambah pengumpulan dengan file `.exe` → muncul error tipe file.
- [ ] Tambah pengumpulan dengan file PDF, waktu dan nilai dikosongkan → tersimpan, waktu terisi otomatis, kolom nilai *Belum dinilai*.
- [ ] Buka **Detail** → klik **Unduh File** → PDF terbuka.
- [ ] Tambah lagi untuk mahasiswa dan tugas yang sama → muncul error *sudah mengumpulkan*.
- [ ] Edit dan isi nilai `88.5` tanpa memilih file → nilai tersimpan, file tetap.
- [ ] Buka **Detail** salah satu data dari seeder → tampil keterangan *file tidak ditemukan di storage*.
- [ ] Hapus pengumpulan baru → file-nya ikut hilang dari `storage/app/public/submissions`.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Tolak pengumpulan jika mahasiswa belum mengambil (enroll) mata kuliah dari tugas tersebut.

[⬅ Modul 13: Tugas](#modul-13-tugas-assignments) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 15: Nilai ➡](#modul-15-nilai-grades)

---

### Modul 15: Nilai (`grades`)

[⬅ Modul 14: Pengumpulan Tugas](#modul-14-pengumpulan-tugas-submissions) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 16: Pengumuman (dengan Policy) ➡](#modul-16-pengumuman-dengan-policy-announcements)

Nilai mahasiswa per mata kuliah per tahun ajaran (UTS, UAS, dan nilai huruf). Nilai huruf bisa dipilih manual atau **dihitung otomatis** dari rata-rata UTS dan UAS.

#### 🎯 Yang Dipelajari

- Menaruh logika perhitungan di controller (method `fillGradeLetter()`)
- Ekspresi `match (true)` di PHP 8
- `isset()` untuk memeriksa beberapa key sekaligus
- *Unique* gabungan mahasiswa + mata kuliah + tahun ajaran

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| Many-to-One | `grades` → `students` | Nilai milik satu mahasiswa |
| Many-to-One | `grades` → `courses` | Nilai untuk satu mata kuliah |

#### 📋 Sebelum Mulai

- Modul 09 (Mahasiswa) dan 10 (Mata Kuliah) sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/GradeController.php` | Controller (logika CRUD) |
| `resources/views/grades/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/grades/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/grades/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/grades/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Controller

```bash
php artisan make:controller GradeController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/GradeController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Course;
use App\Models\Grade;
use App\Models\Student;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;
use Illuminate\Validation\Rule;

class GradeController extends Controller
{
    /**
     * Tampilkan semua data nilai.
     */
    public function index()
    {
        $grades = Grade::with(['student.user', 'course'])->latest()->paginate(10);
        return view('grades.index', compact('grades'));
    }

    /**
     * Tampilkan form tambah nilai (khusus admin).
     */
    public function create()
    {
        Gate::authorize('admin');

        return view('grades.create', $this->formData());
    }

    /**
     * Simpan nilai baru (khusus admin).
     */
    public function store(Request $request)
    {
        Gate::authorize('admin');

        $validated = $request->validate($this->rules($request), $this->messages());
        $validated = $this->fillGradeLetter($validated);

        Grade::create($validated);

        return redirect()->route('grades.index')
                         ->with('success', 'Nilai berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail nilai.
     */
    public function show(Grade $grade)
    {
        $grade->load(['student.user', 'student.department', 'course']);
        return view('grades.show', compact('grade'));
    }

    /**
     * Tampilkan form edit nilai (khusus admin).
     */
    public function edit(Grade $grade)
    {
        Gate::authorize('admin');

        return view('grades.edit', array_merge($this->formData(), compact('grade')));
    }

    /**
     * Update data nilai (khusus admin).
     */
    public function update(Request $request, Grade $grade)
    {
        Gate::authorize('admin');

        $validated = $request->validate($this->rules($request, $grade), $this->messages());
        $validated = $this->fillGradeLetter($validated);

        $grade->update($validated);

        return redirect()->route('grades.index')
                         ->with('success', 'Nilai berhasil diupdate.');
    }

    /**
     * Hapus data nilai (khusus admin).
     */
    public function destroy(Grade $grade)
    {
        Gate::authorize('admin');

        $grade->delete();

        return redirect()->route('grades.index')
                         ->with('success', 'Nilai berhasil dihapus.');
    }

    /**
     * Aturan validasi untuk store & update.
     * Satu mahasiswa hanya punya satu nilai per mata kuliah per tahun ajaran.
     */
    private function rules(Request $request, ?Grade $grade = null): array
    {
        $uniqueGrade = Rule::unique('grades')
            ->where('course_id', $request->input('course_id'))
            ->where('academic_year', $request->input('academic_year'));

        if ($grade) {
            $uniqueGrade->ignore($grade->id);
        }

        return [
            'student_id' => ['required', 'exists:students,id', $uniqueGrade],
            'course_id' => ['required', 'exists:courses,id'],
            'academic_year' => ['required', 'string', 'max:9', 'regex:/^\d{4}\/\d{4}$/'],
            'midterm_score' => ['nullable', 'numeric', 'min:0', 'max:100'],
            'final_score' => ['nullable', 'numeric', 'min:0', 'max:100'],
            'grade_letter' => ['nullable', 'in:A,AB,B,BC,C,D,E'],
        ];
    }

    /**
     * Pesan error kustom.
     */
    private function messages(): array
    {
        return [
            'student_id.unique' => 'Nilai mahasiswa ini untuk mata kuliah & tahun ajaran tersebut sudah ada.',
            'academic_year.regex' => 'Format tahun ajaran harus seperti 2025/2026.',
        ];
    }

    /**
     * Jika nilai huruf dikosongkan dan nilai UTS & UAS sudah diisi,
     * hitung nilai huruf otomatis dari rata-ratanya (aturan sama dengan seeder).
     */
    private function fillGradeLetter(array $validated): array
    {
        if (empty($validated['grade_letter'])
            && isset($validated['midterm_score'], $validated['final_score'])) {
            $average = ($validated['midterm_score'] + $validated['final_score']) / 2;

            $validated['grade_letter'] = match (true) {
                $average >= 85 => 'A',
                $average >= 80 => 'AB',
                $average >= 70 => 'B',
                $average >= 65 => 'BC',
                $average >= 55 => 'C',
                $average >= 45 => 'D',
                default => 'E',
            };
        }

        return $validated;
    }

    /**
     * Data dropdown untuk form create & edit.
     */
    private function formData(): array
    {
        return [
            'students' => Student::with('user')->orderBy('nim')->get(),
            'courses' => Course::orderBy('code')->get(),
            'gradeLetters' => ['A', 'AB', 'B', 'BC', 'C', 'D', 'E'],
        ];
    }
}
```

**Penjelasan:**

- `fillGradeLetter()` dijalankan setelah validasi. Jika `grade_letter` kosong **dan** nilai UTS serta UAS terisi, nilai huruf dihitung dari rata-ratanya dengan aturan yang sama seperti di seeder: ≥85 A, ≥80 AB, ≥70 B, ≥65 BC, ≥55 C, ≥45 D, selebihnya E.
- `match (true) { kondisi => hasil, ... }` memeriksa kondisi dari atas ke bawah dan mengembalikan hasil dari kondisi pertama yang bernilai `true`.
- `isset($validated['midterm_score'], $validated['final_score'])` bernilai `true` hanya jika kedua key ada dan tidak `null`.
- Daftar huruf `gradeLetters` dikirim ke view lewat `formData()` agar tidak ditulis ulang di dua view.

#### Langkah 2: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view grades.index
php artisan make:view grades.create
php artisan make:view grades.show
php artisan make:view grades.edit
```

Perintah di atas membuat folder `resources/views/grades/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 3: View Index (Read: daftar data)

File: `resources/views/grades/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Data Nilai')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Data Nilai Mahasiswa</h2>
    @can('admin')
        <a href="{{ route('grades.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Nilai
        </a>
    @endcan
</div>

@php
    // Warna badge untuk setiap nilai huruf
    $letterColors = [
        'A' => 'success', 'AB' => 'success',
        'B' => 'primary', 'BC' => 'primary',
        'C' => 'warning', 'D' => 'danger', 'E' => 'danger',
    ];
@endphp

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>NIM</th>
            <th>Nama Mahasiswa</th>
            <th>Mata Kuliah</th>
            <th>Tahun Ajaran</th>
            <th>UTS</th>
            <th>UAS</th>
            <th>Nilai Huruf</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($grades as $i => $grade)
        <tr>
            <td>{{ $grades->firstItem() + $i }}</td>
            <td>{{ $grade->student->nim }}</td>
            <td>{{ $grade->student->user->name }}</td>
            <td>{{ $grade->course->name }}</td>
            <td>{{ $grade->academic_year }}</td>
            <td>{{ $grade->midterm_score ?? '-' }}</td>
            <td>{{ $grade->final_score ?? '-' }}</td>
            <td>
                @if($grade->grade_letter)
                    <span class="badge bg-{{ $letterColors[$grade->grade_letter] }}">{{ $grade->grade_letter }}</span>
                @else
                    -
                @endif
            </td>
            <td>
                <a href="{{ route('grades.show', $grade) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                @can('admin')
                    <a href="{{ route('grades.edit', $grade) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                    <form action="{{ route('grades.destroy', $grade) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus nilai ini?')">
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
            <td colspan="9" class="text-center">Belum ada data.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $grades->links() }}
@endsection
```

**Penjelasan:**

- Array `$letterColors` di blok `@php` memberi warna badge: hijau untuk A/AB, biru untuk B/BC, kuning untuk C, merah untuk D/E.

#### Langkah 4: View Create (Create: form tambah data)

File: `resources/views/grades/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Nilai')

@section('content')
<h2>Tambah Nilai Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('grades.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="student_id" class="form-label">Mahasiswa <span class="text-danger">*</span></label>
                <select class="form-select @error('student_id') is-invalid @enderror" id="student_id" name="student_id" required>
                    <option value="">-- Pilih Mahasiswa --</option>
                    @foreach($students as $student)
                        <option value="{{ $student->id }}" {{ old('student_id') == $student->id ? 'selected' : '' }}>
                            {{ $student->nim }} - {{ $student->user->name }}
                        </option>
                    @endforeach
                </select>
                @error('student_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="course_id" class="form-label">Mata Kuliah <span class="text-danger">*</span></label>
                <select class="form-select @error('course_id') is-invalid @enderror" id="course_id" name="course_id" required>
                    <option value="">-- Pilih Mata Kuliah --</option>
                    @foreach($courses as $course)
                        <option value="{{ $course->id }}" {{ old('course_id') == $course->id ? 'selected' : '' }}>
                            {{ $course->code }} - {{ $course->name }}
                        </option>
                    @endforeach
                </select>
                @error('course_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="academic_year" class="form-label">Tahun Ajaran <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('academic_year') is-invalid @enderror"
                       id="academic_year" name="academic_year" value="{{ old('academic_year', '2025/2026') }}"
                       placeholder="2025/2026" maxlength="9" required>
                @error('academic_year')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="row">
                <div class="col-md-4 mb-3">
                    <label for="midterm_score" class="form-label">Nilai UTS</label>
                    <input type="number" class="form-control @error('midterm_score') is-invalid @enderror"
                           id="midterm_score" name="midterm_score" value="{{ old('midterm_score') }}" min="0" max="100" step="0.01">
                    @error('midterm_score')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="final_score" class="form-label">Nilai UAS</label>
                    <input type="number" class="form-control @error('final_score') is-invalid @enderror"
                           id="final_score" name="final_score" value="{{ old('final_score') }}" min="0" max="100" step="0.01">
                    @error('final_score')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="grade_letter" class="form-label">Nilai Huruf</label>
                    <select class="form-select @error('grade_letter') is-invalid @enderror" id="grade_letter" name="grade_letter">
                        <option value="">-- Hitung Otomatis --</option>
                        @foreach($gradeLetters as $letter)
                            <option value="{{ $letter }}" {{ old('grade_letter') === $letter ? 'selected' : '' }}>{{ $letter }}</option>
                        @endforeach
                    </select>
                    <div class="form-text">Pilih "Hitung Otomatis" agar dihitung dari rata-rata UTS & UAS.</div>
                    @error('grade_letter')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('grades.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Opsi pertama dropdown nilai huruf bernilai kosong ("Hitung Otomatis").

#### Langkah 5: View Show (Read: detail data + relasi)

File: `resources/views/grades/show.blade.php`

```html
@extends('layouts.app')

@section('title', 'Detail Nilai')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Detail Nilai</h2>
    <a href="{{ route('grades.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card">
    <div class="card-body">
        <table class="table table-borderless">
            <tr>
                <th width="200">NIM</th>
                <td>: {{ $grade->student->nim }}</td>
            </tr>
            <tr>
                <th>Nama Mahasiswa</th>
                <td>: <a href="{{ route('students.show', $grade->student) }}">{{ $grade->student->user->name }}</a></td>
            </tr>
            <tr>
                <th>Jurusan</th>
                <td>: {{ $grade->student->department->name }}</td>
            </tr>
            <tr>
                <th>Mata Kuliah</th>
                <td>: <a href="{{ route('courses.show', $grade->course) }}">{{ $grade->course->code }} - {{ $grade->course->name }}</a></td>
            </tr>
            <tr>
                <th>Tahun Ajaran</th>
                <td>: {{ $grade->academic_year }}</td>
            </tr>
            <tr>
                <th>Nilai UTS</th>
                <td>: {{ $grade->midterm_score ?? '-' }}</td>
            </tr>
            <tr>
                <th>Nilai UAS</th>
                <td>: {{ $grade->final_score ?? '-' }}</td>
            </tr>
            <tr>
                <th>Rata-rata</th>
                <td>:
                    @if(! is_null($grade->midterm_score) && ! is_null($grade->final_score))
                        {{ number_format(($grade->midterm_score + $grade->final_score) / 2, 2) }}
                    @else
                        -
                    @endif
                </td>
            </tr>
            <tr>
                <th>Nilai Huruf</th>
                <td>: <strong class="fs-5">{{ $grade->grade_letter ?? '-' }}</strong></td>
            </tr>
        </table>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Rata-rata dihitung langsung di view dan dibulatkan 2 angka di belakang koma dengan `number_format(..., 2)`.

#### Langkah 6: View Edit (Update: form ubah data)

File: `resources/views/grades/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Nilai')

@section('content')
<h2>Edit Nilai</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('grades.update', $grade) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="student_id" class="form-label">Mahasiswa <span class="text-danger">*</span></label>
                <select class="form-select @error('student_id') is-invalid @enderror" id="student_id" name="student_id" required>
                    @foreach($students as $student)
                        <option value="{{ $student->id }}" {{ old('student_id', $grade->student_id) == $student->id ? 'selected' : '' }}>
                            {{ $student->nim }} - {{ $student->user->name }}
                        </option>
                    @endforeach
                </select>
                @error('student_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="course_id" class="form-label">Mata Kuliah <span class="text-danger">*</span></label>
                <select class="form-select @error('course_id') is-invalid @enderror" id="course_id" name="course_id" required>
                    @foreach($courses as $course)
                        <option value="{{ $course->id }}" {{ old('course_id', $grade->course_id) == $course->id ? 'selected' : '' }}>
                            {{ $course->code }} - {{ $course->name }}
                        </option>
                    @endforeach
                </select>
                @error('course_id')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="academic_year" class="form-label">Tahun Ajaran <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('academic_year') is-invalid @enderror"
                       id="academic_year" name="academic_year"
                       value="{{ old('academic_year', $grade->academic_year) }}" maxlength="9" required>
                @error('academic_year')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="row">
                <div class="col-md-4 mb-3">
                    <label for="midterm_score" class="form-label">Nilai UTS</label>
                    <input type="number" class="form-control @error('midterm_score') is-invalid @enderror"
                           id="midterm_score" name="midterm_score"
                           value="{{ old('midterm_score', $grade->midterm_score) }}" min="0" max="100" step="0.01">
                    @error('midterm_score')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="final_score" class="form-label">Nilai UAS</label>
                    <input type="number" class="form-control @error('final_score') is-invalid @enderror"
                           id="final_score" name="final_score"
                           value="{{ old('final_score', $grade->final_score) }}" min="0" max="100" step="0.01">
                    @error('final_score')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
                <div class="col-md-4 mb-3">
                    <label for="grade_letter" class="form-label">Nilai Huruf</label>
                    <select class="form-select @error('grade_letter') is-invalid @enderror" id="grade_letter" name="grade_letter">
                        <option value="">-- Hitung Otomatis --</option>
                        @foreach($gradeLetters as $letter)
                            <option value="{{ $letter }}" {{ old('grade_letter', $grade->grade_letter) === $letter ? 'selected' : '' }}>{{ $letter }}</option>
                        @endforeach
                    </select>
                    @error('grade_letter')
                        <div class="invalid-feedback">{{ $message }}</div>
                    @enderror
                </div>
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('grades.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Sama seperti form tambah, dengan nilai lama dari database.

#### Langkah 7: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Buka menu **Lainnya → Nilai** → tampil data nilai dari seeder.
- [ ] Tambah nilai dengan UTS 101 → muncul error (maksimal 100).
- [ ] Tambah nilai dengan UTS 80, UAS 90, nilai huruf *Hitung Otomatis* → tersimpan dengan nilai huruf **A** (rata-rata 85).
- [ ] Tambah lagi untuk mahasiswa, mata kuliah, dan tahun ajaran yang sama → muncul error duplikat.
- [ ] Edit dan pilih nilai huruf manual `B` → tersimpan `B`. Edit lagi dengan UTS 60, UAS 60, *Hitung Otomatis* → menjadi **C**.
- [ ] Buka **Detail Mahasiswa** tersebut → nilai baru tampil di tabel Nilai.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Tampilkan IPK di halaman detail mahasiswa (A=4, AB=3.5, B=3, BC=2.5, C=2, D=1, E=0, dibobot dengan SKS).

[⬅ Modul 14: Pengumpulan Tugas](#modul-14-pengumpulan-tugas-submissions) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [Modul 16: Pengumuman (dengan Policy) ➡](#modul-16-pengumuman-dengan-policy-announcements)

---

### Modul 16: Pengumuman (dengan Policy) (`announcements`)

[⬅ Modul 15: Nilai](#modul-15-nilai-grades) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [TAHAP 11: Uji Coba Akhir ➡](#tahap-11-uji-coba-akhir--checklist-penyelesaian)

Pengumuman kampus. Berbeda dari 15 modul lain yang memakai Gate `admin`, modul ini memakai **Policy**, yaitu cara Laravel mengelompokkan aturan otorisasi untuk satu model.

Aturannya:
- Semua user boleh membaca pengumuman yang sudah dipublikasikan.
- Pengumuman **draft** hanya bisa dilihat admin.
- Hanya admin yang boleh membuat, mengubah, dan menghapus pengumuman.

#### 🎯 Yang Dipelajari

- Membuat Policy dengan `php artisan make:policy`
- `Gate::authorize('aksi', $model)` dan `@can('aksi', $model)` yang memanggil Policy
- Query kondisional `when()` (user biasa hanya melihat yang sudah dipublikasikan)
- Menyimpan data lewat relasi (`$request->user()->announcements()->create()`)
- Checkbox boolean dengan `$request->boolean()`
- Menampilkan teks multi-baris dengan aman (`nl2br(e(...))`)

#### 🔗 Relasi Tabel

| Jenis | Relasi | Keterangan |
|-------|--------|------------|
| Many-to-One | `announcements` → `users` | Setiap pengumuman ditulis oleh satu user |

#### 📋 Sebelum Mulai

- Modul 06 (Users) sudah selesai.

#### 📁 File yang Dibuat

| File | Keterangan |
|------|------------|
| `app/Http/Controllers/AnnouncementController.php` | Controller (logika CRUD) |
| `app/Policies/AnnouncementPolicy.php` | Policy (aturan otorisasi pengumuman) |
| `resources/views/announcements/index.blade.php` | View Index (Read: daftar data) |
| `resources/views/announcements/create.blade.php` | View Create (Create: form tambah data) |
| `resources/views/announcements/show.blade.php` | View Show (Read: detail data + relasi) |
| `resources/views/announcements/edit.blade.php` | View Edit (Update: form ubah data) |

#### Langkah 1: Buat Policy

```bash
php artisan make:policy AnnouncementPolicy --model=Announcement
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Policies/AnnouncementPolicy.php`

```php
<?php

namespace App\Policies;

use App\Models\Announcement;
use App\Models\User;

class AnnouncementPolicy
{
    /**
     * Semua user yang login boleh membuka halaman daftar pengumuman.
     */
    public function viewAny(User $user): bool
    {
        return true;
    }

    /**
     * Pengumuman yang sudah dipublikasikan boleh dilihat semua user.
     * Pengumuman draft hanya boleh dilihat oleh admin.
     */
    public function view(User $user, Announcement $announcement): bool
    {
        return $announcement->is_published || $user->isAdmin();
    }

    /**
     * Hanya admin yang boleh membuat pengumuman.
     */
    public function create(User $user): bool
    {
        return $user->isAdmin();
    }

    /**
     * Hanya admin yang boleh mengubah pengumuman.
     */
    public function update(User $user, Announcement $announcement): bool
    {
        return $user->isAdmin();
    }

    /**
     * Hanya admin yang boleh menghapus pengumuman.
     */
    public function delete(User $user, Announcement $announcement): bool
    {
        return $user->isAdmin();
    }

    /**
     * Fitur restore (soft delete) tidak dipakai.
     */
    public function restore(User $user, Announcement $announcement): bool
    {
        return false;
    }

    /**
     * Fitur force delete (soft delete) tidak dipakai.
     */
    public function forceDelete(User $user, Announcement $announcement): bool
    {
        return false;
    }
}
```

**Penjelasan:**

- Laravel **otomatis** menghubungkan `AnnouncementPolicy` dengan model `Announcement` karena penamaannya mengikuti konvensi (`App\Models\Announcement` → `App\Policies\AnnouncementPolicy`). Tidak perlu didaftarkan di mana pun.
- Setiap method mengembalikan `true` (boleh) atau `false` (dilarang → **403**).
- Method `viewAny` dan `create` tidak menerima objek pengumuman karena belum ada data tertentu yang diakses. Method `view`, `update`, dan `delete` menerima pengumuman yang sedang diakses.
- `restore` dan `forceDelete` dari stub bawaan dibiarkan `false` karena fitur *soft delete* tidak dipakai.
- `$user->isAdmin()` adalah helper yang sudah dibuat di model `User` (TAHAP 4.1).

#### Langkah 2: Buat Controller

```bash
php artisan make:controller AnnouncementController --resource
```

Ganti seluruh isi file yang dihasilkan dengan kode berikut.

File: `app/Http/Controllers/AnnouncementController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Announcement;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Gate;

/**
 * Modul ini memakai POLICY (App\Policies\AnnouncementPolicy),
 * bukan Gate 'admin' seperti modul lainnya.
 * Gate::authorize('aksi', $model) otomatis memanggil method policy yang sesuai.
 */
class AnnouncementController extends Controller
{
    /**
     * Tampilkan daftar pengumuman.
     * Admin melihat semua pengumuman, user biasa hanya melihat yang sudah dipublikasikan.
     */
    public function index(Request $request)
    {
        Gate::authorize('viewAny', Announcement::class);

        $announcements = Announcement::with('user')
            ->when(! $request->user()->isAdmin(), fn ($query) => $query->where('is_published', true))
            ->latest()
            ->paginate(10);

        return view('announcements.index', compact('announcements'));
    }

    /**
     * Tampilkan form tambah pengumuman -> AnnouncementPolicy@create
     */
    public function create()
    {
        Gate::authorize('create', Announcement::class);

        return view('announcements.create');
    }

    /**
     * Simpan pengumuman baru -> AnnouncementPolicy@create
     */
    public function store(Request $request)
    {
        Gate::authorize('create', Announcement::class);

        $validated = $request->validate([
            'title' => 'required|string|max:255',
            'content' => 'required|string',
            'is_published' => 'nullable|boolean',
        ]);

        // Checkbox yang tidak dicentang tidak ikut terkirim, jadi nilainya diambil dengan boolean()
        $validated['is_published'] = $request->boolean('is_published');

        // Simpan lewat relasi One-to-Many User -> Announcement,
        // sehingga user_id otomatis terisi dengan id user yang sedang login
        $request->user()->announcements()->create($validated);

        return redirect()->route('announcements.index')
                         ->with('success', 'Pengumuman berhasil ditambahkan.');
    }

    /**
     * Tampilkan detail pengumuman -> AnnouncementPolicy@view
     * User biasa yang membuka pengumuman draft akan mendapat error 403.
     */
    public function show(Announcement $announcement)
    {
        Gate::authorize('view', $announcement);

        $announcement->load('user');

        return view('announcements.show', compact('announcement'));
    }

    /**
     * Tampilkan form edit pengumuman -> AnnouncementPolicy@update
     */
    public function edit(Announcement $announcement)
    {
        Gate::authorize('update', $announcement);

        return view('announcements.edit', compact('announcement'));
    }

    /**
     * Update pengumuman -> AnnouncementPolicy@update
     */
    public function update(Request $request, Announcement $announcement)
    {
        Gate::authorize('update', $announcement);

        $validated = $request->validate([
            'title' => 'required|string|max:255',
            'content' => 'required|string',
            'is_published' => 'nullable|boolean',
        ]);

        $validated['is_published'] = $request->boolean('is_published');

        $announcement->update($validated);

        return redirect()->route('announcements.index')
                         ->with('success', 'Pengumuman berhasil diupdate.');
    }

    /**
     * Hapus pengumuman -> AnnouncementPolicy@delete
     */
    public function destroy(Announcement $announcement)
    {
        Gate::authorize('delete', $announcement);

        $announcement->delete();

        return redirect()->route('announcements.index')
                         ->with('success', 'Pengumuman berhasil dihapus.');
    }
}
```

**Penjelasan:**

- `Gate::authorize('create', Announcement::class)` memanggil `AnnouncementPolicy::create()`. Untuk aksi pada data tertentu, kirim objeknya: `Gate::authorize('update', $announcement)`.
- `when(! $request->user()->isAdmin(), fn ($query) => $query->where('is_published', true))` hanya menambahkan filter jika yang login **bukan** admin.
- Checkbox yang tidak dicentang **tidak dikirim** oleh browser. `$request->boolean('is_published')` menghasilkan `true` jika dicentang dan `false` jika tidak ada.
- `$request->user()->announcements()->create($validated)` menyimpan pengumuman lewat relasi One-to-Many, sehingga `user_id` otomatis terisi ID user yang sedang login.

#### Langkah 3: Buat File View

Buat 4 file view sekaligus dengan perintah berikut:

```bash
php artisan make:view announcements.index
php artisan make:view announcements.create
php artisan make:view announcements.show
php artisan make:view announcements.edit
```

Perintah di atas membuat folder `resources/views/announcements/` berisi 4 file. **Hapus isi bawaannya** (`<div>` berisi kutipan) dan ganti dengan kode pada langkah-langkah berikut.

#### Langkah 4: View Index (Read: daftar data)

File: `resources/views/announcements/index.blade.php`

```html
@extends('layouts.app')

@section('title', 'Pengumuman')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>Pengumuman</h2>
    {{-- @can dengan nama class -> memanggil AnnouncementPolicy@create --}}
    @can('create', App\Models\Announcement::class)
        <a href="{{ route('announcements.create') }}" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Tambah Pengumuman
        </a>
    @endcan
</div>

<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>No</th>
            <th>Judul</th>
            <th>Penulis</th>
            <th>Status</th>
            <th>Tanggal</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>
        @forelse($announcements as $i => $announcement)
        <tr>
            <td>{{ $announcements->firstItem() + $i }}</td>
            <td>{{ $announcement->title }}</td>
            <td>{{ $announcement->user->name }}</td>
            <td>
                <span class="badge bg-{{ $announcement->is_published ? 'success' : 'secondary' }}">
                    {{ $announcement->is_published ? 'Published' : 'Draft' }}
                </span>
            </td>
            <td>{{ $announcement->created_at->format('d M Y') }}</td>
            <td>
                <a href="{{ route('announcements.show', $announcement) }}" class="btn btn-sm btn-info">
                    <i class="bi bi-eye"></i> Detail
                </a>
                {{-- @can dengan objek model -> memanggil AnnouncementPolicy@update / @delete --}}
                @can('update', $announcement)
                    <a href="{{ route('announcements.edit', $announcement) }}" class="btn btn-sm btn-warning">
                        <i class="bi bi-pencil"></i> Edit
                    </a>
                @endcan
                @can('delete', $announcement)
                    <form action="{{ route('announcements.destroy', $announcement) }}" method="POST" class="d-inline"
                          onsubmit="return confirm('Yakin ingin menghapus pengumuman ini?')">
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
            <td colspan="6" class="text-center">Belum ada pengumuman.</td>
        </tr>
        @endforelse
    </tbody>
</table>

{{ $announcements->links() }}
@endsection
```

**Penjelasan:**

- `@can('create', App\Models\Announcement::class)`: untuk aksi tanpa data tertentu, kirim nama class.
- `@can('update', $announcement)` dan `@can('delete', $announcement)`: untuk aksi pada data tertentu, kirim objeknya.

#### Langkah 5: View Create (Create: form tambah data)

File: `resources/views/announcements/create.blade.php`

```html
@extends('layouts.app')

@section('title', 'Tambah Pengumuman')

@section('content')
<h2>Tambah Pengumuman Baru</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('announcements.store') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="title" class="form-label">Judul <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('title') is-invalid @enderror"
                       id="title" name="title" value="{{ old('title') }}" required>
                @error('title')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="content" class="form-label">Isi Pengumuman <span class="text-danger">*</span></label>
                <textarea class="form-control @error('content') is-invalid @enderror"
                          id="content" name="content" rows="6" required>{{ old('content') }}</textarea>
                @error('content')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3 form-check">
                <input type="checkbox" class="form-check-input" id="is_published" name="is_published" value="1"
                       {{ old('is_published') ? 'checked' : '' }}>
                <label class="form-check-label" for="is_published">Publikasikan sekarang</label>
                <div class="form-text">Jika tidak dicentang, pengumuman disimpan sebagai draft dan hanya terlihat oleh admin.</div>
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary">
                    <i class="bi bi-save"></i> Simpan
                </button>
                <a href="{{ route('announcements.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- Checkbox diberi `value="1"`. Jika tidak dicentang, pengumuman tersimpan sebagai draft.

#### Langkah 6: View Show (Read: detail data + relasi)

File: `resources/views/announcements/show.blade.php`

```html
@extends('layouts.app')

@section('title', $announcement->title)

@section('content')
<div class="d-flex justify-content-between align-items-center mb-3">
    <h2>{{ $announcement->title }}</h2>
    <a href="{{ route('announcements.index') }}" class="btn btn-secondary">
        <i class="bi bi-arrow-left"></i> Kembali
    </a>
</div>

<div class="card">
    <div class="card-header d-flex justify-content-between">
        <span>
            <i class="bi bi-person"></i> {{ $announcement->user->name }}
            &middot; <i class="bi bi-calendar"></i> {{ $announcement->created_at->format('d M Y H:i') }}
        </span>
        <span class="badge bg-{{ $announcement->is_published ? 'success' : 'secondary' }}">
            {{ $announcement->is_published ? 'Published' : 'Draft' }}
        </span>
    </div>
    <div class="card-body">
        {{-- e() meng-escape HTML agar aman dari XSS, nl2br() mengubah baris baru menjadi <br> --}}
        <p class="card-text">{!! nl2br(e($announcement->content)) !!}</p>
    </div>
    @can('update', $announcement)
        <div class="card-footer">
            <a href="{{ route('announcements.edit', $announcement) }}" class="btn btn-sm btn-warning">
                <i class="bi bi-pencil"></i> Edit
            </a>
        </div>
    @endcan
</div>
@endsection
```

**Penjelasan:**

- `{!! nl2br(e($announcement->content)) !!}`: `e()` meng-*escape* HTML (mencegah serangan XSS), lalu `nl2br()` mengubah baris baru menjadi `<br>`. Jangan pernah menulis `{!! $announcement->content !!}` tanpa `e()`.
- Judul halaman memakai `@section('title', $announcement->title)`.

#### Langkah 7: View Edit (Update: form ubah data)

File: `resources/views/announcements/edit.blade.php`

```html
@extends('layouts.app')

@section('title', 'Edit Pengumuman')

@section('content')
<h2>Edit Pengumuman</h2>

<div class="card">
    <div class="card-body">
        <form action="{{ route('announcements.update', $announcement) }}" method="POST">
            @csrf
            @method('PUT')

            <div class="mb-3">
                <label for="title" class="form-label">Judul <span class="text-danger">*</span></label>
                <input type="text" class="form-control @error('title') is-invalid @enderror"
                       id="title" name="title" value="{{ old('title', $announcement->title) }}" required>
                @error('title')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="content" class="form-label">Isi Pengumuman <span class="text-danger">*</span></label>
                <textarea class="form-control @error('content') is-invalid @enderror"
                          id="content" name="content" rows="6" required>{{ old('content', $announcement->content) }}</textarea>
                @error('content')
                    <div class="invalid-feedback">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3 form-check">
                {{-- Jika validasi gagal pakai old(), jika tidak pakai nilai dari database --}}
                <input type="checkbox" class="form-check-input" id="is_published" name="is_published" value="1"
                       {{ (old() ? old('is_published') : $announcement->is_published) ? 'checked' : '' }}>
                <label class="form-check-label" for="is_published">Publikasikan</label>
            </div>

            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-warning">
                    <i class="bi bi-save"></i> Update
                </button>
                <a href="{{ route('announcements.index') }}" class="btn btn-secondary">
                    <i class="bi bi-arrow-left"></i> Kembali
                </a>
            </div>
        </form>
    </div>
</div>
@endsection
```

**Penjelasan:**

- `old() ? old('is_published') : $announcement->is_published`: jika validasi gagal, status checkbox mengikuti input terakhir; jika tidak, mengikuti data di database.

#### Langkah 8: Uji Coba

Jalankan server (`php artisan serve`), buka `http://localhost:8000`, lalu cek satu per satu:

- [ ] Login sebagai admin, buka menu **Lainnya → Pengumuman** → tampil 5 pengumuman, termasuk *Workshop Laravel* berstatus **Draft**.
- [ ] Tambah pengumuman tanpa mencentang *Publikasikan sekarang* → tersimpan sebagai Draft dengan penulis Administrator.
- [ ] Isi pengumuman dengan beberapa baris dan teks `<b>tes</b>` → di halaman detail baris baru tampil, sedangkan `<b>tes</b>` tampil sebagai teks biasa (tidak dijalankan sebagai HTML).
- [ ] Edit dan centang *Publikasikan* → status menjadi Published.
- [ ] Logout, login sebagai `mahasiswa1@siakad.com` → *Workshop Laravel* (draft) tidak muncul di daftar. Buka manual `http://localhost:8000/announcements/4` → **403**.
- [ ] Sebagai user biasa, tombol Tambah, Edit, dan Hapus tidak tampil. Buka `/announcements/create` → **403**.

> Fitur **Delete** sudah lengkap pada langkah di atas: tombol Hapus ada di View Index (form dengan `@method('DELETE')` + konfirmasi) dan prosesnya di method `destroy()` controller.

#### 💡 Tantangan (Opsional)

- Ubah policy agar admin hanya bisa mengedit pengumuman yang ia tulis sendiri (`$announcement->user_id === $user->id`).
- Tampilkan 3 pengumuman terbaru yang sudah dipublikasikan di halaman Dashboard.

[⬅ Modul 15: Nilai](#modul-15-nilai-grades) · [⬆ Daftar Modul](#95-urutan-pengerjaan-modul) · [TAHAP 11: Uji Coba Akhir ➡](#tahap-11-uji-coba-akhir--checklist-penyelesaian)

> 🎉 **Selamat, ke-16 modul selesai!** Lanjutkan ke [TAHAP 11: Uji Coba Akhir](#tahap-11-uji-coba-akhir--checklist-penyelesaian) untuk memastikan seluruh aplikasi berjalan dengan benar.

---

# BAGIAN E: Penyelesaian

## TAHAP 11: Uji Coba Akhir & Checklist Penyelesaian

Setelah ke-16 modul selesai, lakukan pengecekan menyeluruh. Centang setiap poin; jika ada yang gagal, kembali ke tahap yang disebutkan di judul bagiannya.

### Persiapan (TAHAP 1–2)
- [ ] Install Laravel project baru
- [ ] Buat database MySQL dan konfigurasi `.env`
- [ ] Test koneksi database berhasil

### Database: Migration, Model & Seeder (TAHAP 3–5)
- [ ] Buat/modifikasi 18 file migration
- [ ] Jalankan `php artisan migrate` berhasil
- [ ] Buat 16 model dengan relasi Eloquent lengkap
- [ ] Buat DatabaseSeeder dengan data dummy
- [ ] Jalankan `php artisan migrate:fresh --seed` berhasil

### Route, Autentikasi & Autorisasi (TAHAP 6–8)
- [ ] Buat `AuthController` (login, logout)
- [ ] Buat view login
- [ ] Setup middleware `auth` di routes
- [ ] Setup Gate `admin` di `AppServiceProvider`
- [ ] Tombol Create/Edit/Delete hanya muncul untuk admin (`@can('admin')`)
- [ ] Method create/store/edit/update/destroy dicek dengan `Gate::authorize('admin')`
- [ ] Buat `AnnouncementPolicy` dan gunakan untuk modul Pengumuman
- [ ] Pagination memakai Bootstrap (`Paginator::useBootstrapFive()`)
- [ ] Jalankan `php artisan storage:link` untuk upload file

### CRUD per Modul (TAHAP 10, ulangi untuk ke-16 modul)
- [ ] Buat Controller dengan 7 method resource
- [ ] Route `Route::resource()` sudah terdaftar (TAHAP 6)
- [ ] Buat view `index.blade.php` — Read (tampil semua data + pagination)
- [ ] Buat view `create.blade.php` — Create (form tambah dengan validasi)
- [ ] Buat view `show.blade.php` — Show (detail data + relasi)
- [ ] Buat view `edit.blade.php` — Edit (form edit dengan old values)
- [ ] Implementasi Delete (tombol + konfirmasi + method destroy)

### Pengujian Menyeluruh
- [ ] Login sebagai **admin** → bisa CRUD semua modul
- [ ] Login sebagai **user** → hanya bisa Read (index & show), tombol C/U/D tersembunyi
- [ ] Akses halaman tanpa login → redirect ke `/login`
- [ ] Validasi form berfungsi (tampil pesan error)
- [ ] Data relasi tampil dengan benar di halaman detail
- [ ] Pagination berfungsi
- [ ] Flash message success muncul setelah create/update/delete

> 🏁 Jika semua poin tercentang, **tugas SIAKAD selesai**.

---

# Lampiran

## Lampiran A: Ringkasan Spesifikasi per Modul

Ringkasan ini berguna sebagai **contekan cepat** saat mengerjakan atau memeriksa modul. Kode lengkapnya tetap ada di TAHAP 10. Urutan mengikuti nomor modul.

### A.1 Controller: Validasi & Relasi yang Dimuat

#### 📌 Modul 01: `DepartmentController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Department`                                                             |
| Validation    | `name` (required), `code` (required, max:10, unique), `description` (nullable) |
| Relasi Load   | `index`: `withCount(['teachers', 'students', 'courses'])` · `show`: `with(['teachers.user', 'students.user', 'courses'])` |

➡ Kode lengkap: [Modul 01: Jurusan](#modul-01-jurusan-departments)

#### 📌 Modul 02: `CategoryController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Category`                                                               |
| Validation    | `name` (required), `description` (nullable) |
| Relasi Load   | `index`: `withCount('courses')` · `show`: `with('courses')` |

➡ Kode lengkap: [Modul 02: Kategori Mata Kuliah](#modul-02-kategori-mata-kuliah-categories)

#### 📌 Modul 03: `ClassroomController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Classroom`                                                              |
| Validation    | `name` (required), `building` (required), `capacity` (required, integer, min:1) |
| Relasi Load   | `index`: query biasa · `show`: `with('schedules.course')` |

➡ Kode lengkap: [Modul 03: Ruang Kelas](#modul-03-ruang-kelas-classrooms)

#### 📌 Modul 04: `TagController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Tag`                                                                    |
| Validation    | `name` (required), `slug` (required, unique) |
| Relasi Load   | `index`: `withCount('courses')` · `show`: `with('courses')` |
| Catatan       | Slug bisa di-generate otomatis dari name menggunakan `Str::slug()`. |

➡ Kode lengkap: [Modul 04: Tag Mata Kuliah](#modul-04-tag-mata-kuliah-tags)

#### 📌 Modul 05: `ExtracurricularController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Extracurricular`                                                        |
| Validation    | `name` (required), `description` (nullable), `max_members` (required, integer, min:1) |
| Relasi Load   | `index`: `withCount('students')` · `show`: `with('students.user')` |

➡ Kode lengkap: [Modul 05: Ekstrakurikuler](#modul-05-ekstrakurikuler-extracurriculars)

#### 📌 Modul 06: `UserController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `User`                                                                   |
| Validation    | `name` (required), `email` (required, email, unique), `password` (required saat create, nullable saat edit), `role` (required, in:admin,user) |
| Relasi Load   | `index`: `with('profile')` · `show`: `with(['profile', 'teacher', 'student', 'announcements'])` |
| Catatan       | Password di-hash saat store/update. Saat update, password hanya diubah jika diisi. |

➡ Kode lengkap: [Modul 06: Users](#modul-06-users-users)

#### 📌 Modul 07: `ProfileController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Profile`                                                                |
| Validation    | `user_id` (required, exists:users,id), `phone` (nullable, max:20), `address` (nullable), `avatar` (nullable, image), `birth_date` (nullable, date) |
| Relasi Load   | `index`: `with('user')` · `show`: `with('user')` |
| Catatan       | Pada form create, tampilkan dropdown daftar user yang belum memiliki profil. Avatar di-upload ke `storage/app/public/avatars` (jalankan `php artisan storage:link`) dan form wajib memakai `enctype="multipart/form-data"`. |

➡ Kode lengkap: [Modul 07: Profil Pengguna](#modul-07-profil-pengguna-profiles)

#### 📌 Modul 08: `TeacherController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Teacher`                                                                |
| Validation    | `user_id` (required, exists:users,id), `department_id` (required, exists:departments,id), `nip` (required, unique), `specialization` (nullable) |
| Relasi Load   | `index`: `with(['user', 'department'])` · `show`: `with(['user.profile', 'department', 'schedules.course'])` |
| Form Data     | Kirim daftar `$users` dan `$departments` ke view create/edit. |

➡ Kode lengkap: [Modul 08: Dosen](#modul-08-dosen-teachers)

#### 📌 Modul 09: `StudentController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Student`                                                                |
| Validation    | `user_id` (required, exists:users,id), `department_id` (required, exists:departments,id), `nim` (required, unique), `semester` (required, integer, min:1, max:14), `extracurriculars` (nullable, array) |
| Relasi Load   | `index`: `with(['user', 'department'])` · `show`: `with(['user.profile', 'department', 'courses', 'grades.course', 'extracurriculars'])` |
| Form Data     | Kirim daftar `$users`, `$departments`, dan `$extracurriculars` ke view create/edit. |
| Catatan       | Ekstrakurikuler dipilih dengan checkbox (Many-to-Many). Gunakan `attach()` saat store dan `sync()` saat update. |

➡ Kode lengkap: [Modul 09: Mahasiswa](#modul-09-mahasiswa-students)

#### 📌 Modul 10: `CourseController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Course`                                                                 |
| Validation    | `category_id` (required, exists), `department_id` (required, exists), `code` (required, unique), `name` (required), `credits` (required, integer, min:1, max:6), `description` (nullable), `tags` (nullable, array) |
| Relasi Load   | `index`: `with(['category', 'department'])` · `show`: `with(['category', 'department', 'tags', 'schedules.teacher', 'assignments'])` |
| Form Data     | Kirim `$categories`, `$departments`, `$tags` ke view create/edit. |
| Catatan       | Gunakan `$course->tags()->sync($request->tags)` untuk menyimpan relasi many-to-many dengan tags. |

➡ Kode lengkap: [Modul 10: Mata Kuliah](#modul-10-mata-kuliah-courses)

#### 📌 Modul 11: `ScheduleController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Schedule`                                                               |
| Validation    | `course_id` (required, exists), `teacher_id` (required, exists), `classroom_id` (required, exists), `day` (required, in:Senin,...,Sabtu), `start_time` (required), `end_time` (required, after:start_time) |
| Relasi Load   | `index`: `with(['course', 'teacher.user', 'classroom'])` · `show`: sama |
| Form Data     | Kirim `$courses`, `$teachers`, `$classrooms` ke view create/edit. |

➡ Kode lengkap: [Modul 11: Jadwal Perkuliahan](#modul-11-jadwal-perkuliahan-schedules)

#### 📌 Modul 12: `EnrollmentController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Enrollment`                                                             |
| Validation    | `student_id` (required, exists), `course_id` (required, exists), `academic_year` (required, max:9), `semester` (required, in:Ganjil,Genap), `status` (required, in:active,dropped,completed) |
| Relasi Load   | `index`: `with(['student.user', 'course'])` · `show`: sama |
| Form Data     | Kirim `$students` (with user name) dan `$courses` ke view create/edit. |

➡ Kode lengkap: [Modul 12: Enrollment (KRS)](#modul-12-enrollment-krs-enrollments)

#### 📌 Modul 13: `AssignmentController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Assignment`                                                             |
| Validation    | `course_id` (required, exists), `title` (required), `description` (nullable), `due_date` (required, date, after:today) |
| Relasi Load   | `index`: `with('course')` · `show`: `with(['course', 'submissions.student.user'])` |
| Form Data     | Kirim `$courses` ke view create/edit. |

➡ Kode lengkap: [Modul 13: Tugas](#modul-13-tugas-assignments)

#### 📌 Modul 14: `SubmissionController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Submission`                                                             |
| Validation    | `assignment_id` (required, exists), `student_id` (required, exists), `file` (nullable, file, mimes:pdf,doc,docx,zip, max:2048), `notes` (nullable), `submitted_at` (nullable, date), `score` (nullable, numeric, min:0, max:100) |
| Relasi Load   | `index`: `with(['assignment.course', 'student.user'])` · `show`: sama |
| Form Data     | Kirim `$assignments` dan `$students` ke view create/edit. |
| Catatan       | File di-upload ke `storage/app/public/submissions`, path-nya disimpan di kolom `file_path`. |

➡ Kode lengkap: [Modul 14: Pengumpulan Tugas](#modul-14-pengumpulan-tugas-submissions)

#### 📌 Modul 15: `GradeController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Grade`                                                                  |
| Validation    | `student_id` (required, exists), `course_id` (required, exists), `academic_year` (required), `midterm_score` (nullable, numeric, min:0, max:100), `final_score` (nullable, numeric, min:0, max:100), `grade_letter` (nullable, in:A,AB,B,BC,C,D,E) |
| Relasi Load   | `index`: `with(['student.user', 'course'])` · `show`: sama |
| Form Data     | Kirim `$students` dan `$courses` ke view create/edit. |
| Catatan       | Jika `grade_letter` dikosongkan, nilai huruf dihitung otomatis dari rata-rata UTS & UAS. |

➡ Kode lengkap: [Modul 15: Nilai](#modul-15-nilai-grades)

#### 📌 Modul 16: `AnnouncementController`

| Aspek         | Detail                                                                   |
|---------------|--------------------------------------------------------------------------|
| Model         | `Announcement`                                                           |
| Validation    | `title` (required), `content` (required), `is_published` (boolean) |
| Relasi Load   | `index`: `with('user')` · `show`: `with('user')` |
| Catatan       | `user_id` diisi otomatis dari user yang login saat store. Otorisasi memakai **Policy** `AnnouncementPolicy` (bukan Gate `admin`), dan user biasa hanya melihat pengumuman yang sudah dipublikasikan. |

➡ Kode lengkap: [Modul 16: Pengumuman (dengan Policy)](#modul-16-pengumuman-dengan-policy-announcements)

### A.2 Field pada Form Create & Edit

| Modul              | Tipe Input Field                                                                                                |
|--------------------|-----------------------------------------------------------------------------------------------------------------|
| Departments        | `name` (text), `code` (text), `description` (textarea)                                                         |
| Categories         | `name` (text), `description` (textarea)                                                                        |
| Classrooms         | `name` (text), `building` (text), `capacity` (number)                                                          |
| Tags               | `name` (text), `slug` (text, auto-generate dari name)                                                          |
| Extracurriculars   | `name` (text), `description` (textarea), `max_members` (number)                                                |
| Users              | `name` (text), `email` (email), `password` (password), `role` (select: admin/user)                              |
| Profiles           | `user_id` (select dropdown users), `phone` (text), `address` (textarea), `avatar` (file), `birth_date` (date)  |
| Teachers           | `user_id` (select), `department_id` (select), `nip` (text), `specialization` (text)                            |
| Students           | `user_id` (select), `department_id` (select), `nim` (text), `semester` (number), `extracurriculars[]` (checkbox) |
| Courses            | `category_id` (select), `department_id` (select), `code` (text), `name` (text), `credits` (number), `description` (textarea), `tags[]` (checkbox) |
| Schedules          | `course_id` (select), `teacher_id` (select), `classroom_id` (select), `day` (select), `start_time` (time), `end_time` (time) |
| Enrollments        | `student_id` (select), `course_id` (select), `academic_year` (text), `semester` (select), `status` (select)    |
| Assignments        | `course_id` (select), `title` (text), `description` (textarea), `due_date` (datetime-local)                    |
| Submissions        | `assignment_id` (select), `student_id` (select), `file` (file), `notes` (textarea), `submitted_at` (datetime-local), `score` (number) |
| Grades             | `student_id` (select), `course_id` (select), `academic_year` (text), `midterm_score` (number), `final_score` (number), `grade_letter` (select, kosong = hitung otomatis) |
| Announcements      | `title` (text), `content` (textarea), `is_published` (checkbox)                                                |

### A.3 Data yang Ditampilkan di View Show

| Modul              | Data Utama                                                          | Data Relasi yang Ditampilkan                                                     |
|--------------------|----------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Departments        | name, code, description                                              | List Teachers, List Students, List Courses                                        |
| Categories         | name, description                                                    | List Courses                                                                      |
| Classrooms         | name, building, capacity                                             | List Schedules                                                                    |
| Tags               | name, slug                                                           | List Courses yang memiliki tag ini                                                |
| Extracurriculars   | name, description, max_members                                       | List Students (members, with role & joined_at)                                    |
| Users              | name, email, role, created_at                                        | Profile (phone, address), Teacher/Student data, Announcements list               |
| Profiles           | phone, address, avatar, birth_date                                   | User (name, email)                                                                |
| Teachers           | nip, specialization                                                  | User (name, email), Department name, List Schedules                               |
| Students           | nim, semester                                                        | User (name, email), Department name, List Courses (enrollments), Extracurriculars |
| Courses            | code, name, credits, description                                     | Category, Department, Tags (badges), List Schedules, List Assignments             |
| Schedules          | day, start_time, end_time                                            | Course name, Teacher name, Classroom name                                         |
| Enrollments        | academic_year, semester, status                                      | Student (nim, name), Course (code, name)                                          |
| Assignments        | title, description, due_date                                         | Course name, List Submissions                                                     |
| Submissions        | file_path, notes, submitted_at, score                                | Assignment title, Student (nim, name)                                             |
| Grades             | academic_year, midterm_score, final_score, grade_letter              | Student (nim, name), Course (code, name)                                          |
| Announcements      | title, content, is_published, created_at                             | User (author name)                                                                |

---

## Lampiran B: Struktur Direktori View Lengkap

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

## Lampiran C: Ringkasan Teknologi yang Digunakan

| Komponen            | Teknologi                                |
|---------------------|------------------------------------------|
| Framework           | Laravel 11+ (diuji pada Laravel 13)      |
| Bahasa              | PHP 8.3+                                 |
| Database            | MySQL / MariaDB                          |
| ORM                 | Eloquent ORM                             |
| Template Engine     | Blade                                    |
| CSS Framework       | Bootstrap 5 (CDN)                        |
| Icon                | Bootstrap Icons (CDN)                    |
| Authentication      | Laravel Auth (manual, tanpa package)     |
| Authorization       | Gate, Policy & `@can` directive          |
| File Upload         | Laravel Storage (disk `public`)          |
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
