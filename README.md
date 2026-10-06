# Student Management System

Aplikasi **Student Management System** merupakan aplikasi desktop berbasis **WPF (.NET 8)** yang digunakan untuk mengelola data mahasiswa. Aplikasi ini menerapkan arsitektur **MVVM (Model-View-ViewModel)**, **Data Binding**, **Command**, serta akses database menggunakan **ADO.NET dan SQL Server LocalDB**.

## Fitur

Aplikasi memiliki beberapa fitur utama:

* Menambahkan data mahasiswa
* Menampilkan data mahasiswa
* Mengubah data mahasiswa
* Menghapus data mahasiswa
* Mencari mahasiswa berdasarkan:

  * NIM
  * Nama
  * Jurusan
* Menampilkan statistik mahasiswa:

  * Total mahasiswa
  * Total mahasiswa Informatika
  * Total mahasiswa Sistem Informasi
  * Total mahasiswa laki-laki
  * Total mahasiswa perempuan
* Reset form input
* Menampilkan data menggunakan DataGrid

## Teknologi

| Teknologi                | Keterangan                        |
| ------------------------ | --------------------------------- |
| C#                       | Bahasa pemrograman                |
| .NET 8                   | Framework                         |
| WPF                      | Framework untuk antarmuka desktop |
| XAML                     | Perancangan UI                    |
| MVVM                     | Arsitektur aplikasi               |
| Data Binding             | Penghubung View dengan ViewModel  |
| ADO.NET                  | Akses database                    |
| SQL Server LocalDB       | Database                          |
| Microsoft.Data.SqlClient | SQL Server provider               |
| Visual Studio            | IDE                               |

## Arsitektur

Aplikasi menggunakan pola **MVVM** dengan alur:

```text
User
  ↓
WPF / XAML
  ↓ Data Binding
ViewModel
  ↓
Model
  ↓
Repository
  ↓ ADO.NET
SQL Server
```

Pembagian tanggung jawab:

* **View** menangani tampilan menggunakan XAML.
* **ViewModel** menangani state, command, pencarian, statistik, dan logika UI.
* **Model** merepresentasikan data mahasiswa.
* **Repository** menangani komunikasi dengan database.
* **SQL Server** menyimpan data mahasiswa.

## Struktur Project

```text
StudentManager
│
├── App.xaml
├── App.xaml.cs
├── MainWindow.xaml
├── MainWindow.xaml.cs
│
├── Models
│   └── Student.cs
│
├── ViewModels
│   ├── StudentViewModel.cs
│   └── RelayCommand.cs
│
└── Data
    └── StudentRepository.cs
```

## Struktur Database

Database yang digunakan bernama:

```text
StudentDB
```

Tabel yang digunakan:

```text
Students
```

Struktur tabel:

| Field   | Tipe Data    | Keterangan            |
| ------- | ------------ | --------------------- |
| Id      | INT          | Primary Key, Identity |
| NIM     | VARCHAR(20)  | Nomor Induk Mahasiswa |
| Nama    | VARCHAR(100) | Nama mahasiswa        |
| Jurusan | VARCHAR(100) | Jurusan mahasiswa     |
| Gender  | VARCHAR(20)  | Jenis kelamin         |
| Email   | VARCHAR(100) | Email mahasiswa       |

## Membuat Database

Jalankan query berikut pada SQL Server:

```sql
CREATE DATABASE StudentDB;
GO

USE StudentDB;
GO

CREATE TABLE Students
(
    Id INT IDENTITY(1,1) PRIMARY KEY,
    NIM VARCHAR(20) NOT NULL,
    Nama VARCHAR(100) NOT NULL,
    Jurusan VARCHAR(100) NOT NULL,
    Gender VARCHAR(20) NOT NULL,
    Email VARCHAR(100)
);
GO

INSERT INTO Students
(NIM, Nama, Jurusan, Gender, Email)
VALUES
('23001', 'Budi Santoso', 'Informatika', 'Laki-laki', 'budi@gmail.com'),
('23002', 'Siti Aminah', 'Sistem Informasi', 'Perempuan', 'siti@gmail.com'),
('23003', 'Andi Wijaya', 'Informatika', 'Laki-laki', 'andi@gmail.com'),
('23004', 'Rina Sari', 'Sistem Informasi', 'Perempuan', 'rina@gmail.com');
GO
```

## Connection String

Aplikasi menggunakan SQL Server LocalDB dengan konfigurasi:

```text
Server=(localdb)\MSSQLLocalDB
Database=StudentDB
Trusted_Connection=True
TrustServerCertificate=True
```

Connection string terdapat pada:

```text
Data/StudentRepository.cs
```

## Instalasi

### 1. Membuat Project

Buka Visual Studio kemudian pilih:

```text
Create a new project
        ↓
WPF Application
        ↓
StudentManager
        ↓
.NET 8
```

### 2. Install Package

Buka:

```text
Tools
→ NuGet Package Manager
→ Package Manager Console
```

Kemudian jalankan:

```powershell
Install-Package Microsoft.Data.SqlClient
```

### 3. Menyiapkan Database

Buka:

```text
View
→ SQL Server Object Explorer
```

Pastikan terdapat:

```text
(localdb)\MSSQLLocalDB
```

Kemudian jalankan query database yang terdapat pada bagian **Struktur Database**.

## Menjalankan Aplikasi

Setelah semua kode dan database selesai dibuat:

### Build

Tekan:

```text
Ctrl + Shift + B
```

Pastikan proses build berhasil.

### Run

Tekan:

```text
F5
```

atau klik tombol:

```text
Start
```

Aplikasi kemudian akan menampilkan halaman **Student Management System**.

## Alur CRUD

### Create

```text
Form Input
    ↓
SaveCommand
    ↓
StudentViewModel
    ↓
StudentRepository
    ↓
INSERT
    ↓
SQL Server
    ↓
DataGrid
```

### Read

```text
SQL Server
    ↓
SELECT
    ↓
StudentRepository
    ↓
ObservableCollection
    ↓
DataGrid
```

### Update

```text
DataGrid
    ↓
SelectedStudent
    ↓
Form Input
    ↓
SaveCommand
    ↓
Repository.Update()
    ↓
SQL Server
```

### Delete

```text
DataGrid
    ↓
SelectedStudent
    ↓
DeleteCommand
    ↓
Repository.Delete()
    ↓
SQL Server
    ↓
DataGrid
```

## Alur Search

Pencarian dilakukan berdasarkan NIM, Nama, atau Jurusan.

```text
SearchText
    ↓
SearchCommand
    ↓
StudentViewModel
    ↓
Repository.Search()
    ↓
SQL Server
    ↓
Students Collection
    ↓
DataGrid
```

## Data Binding

Data Binding digunakan untuk menghubungkan komponen UI dengan ViewModel.

Contoh:

```text
TextBox
    ↓
SelectedStudent.Nama
```

DataGrid menggunakan:

```text
ObservableCollection<Student>
        ↓
ItemsSource
        ↓
DataGrid
```

Ketika mahasiswa dipilih dari DataGrid:

```text
DataGrid.SelectedItem
        ↓
SelectedStudent
        ↓
Form Input
```

## Statistik

Aplikasi menampilkan statistik secara otomatis berdasarkan data yang tersedia.

```text
Total Mahasiswa
Informatika
Sistem Informasi
Laki-laki
Perempuan
```

Statistik diperbarui setelah proses CRUD maupun pencarian.

## Pengujian

| Pengujian  | Hasil yang Diharapkan                                 |
| ---------- | ----------------------------------------------------- |
| READ       | Data mahasiswa muncul pada DataGrid                   |
| CREATE     | Data baru tersimpan dan muncul pada tabel             |
| UPDATE     | Perubahan data tersimpan                              |
| DELETE     | Data terhapus dari tabel                              |
| SEARCH     | Data dapat dicari berdasarkan NIM, Nama, atau Jurusan |
| STATISTICS | Statistik berubah sesuai data                         |
| RESET      | Form kembali ke kondisi awal                          |

## Kesimpulan

Student Management System merupakan implementasi aplikasi desktop menggunakan **WPF dan MVVM**. Aplikasi menggabungkan **XAML, Data Binding, Command, ObservableCollection, INotifyPropertyChanged, ADO.NET, dan SQL Server** untuk menghasilkan sistem pengelolaan data mahasiswa yang terstruktur.

Dengan pemisahan antara View, ViewModel, Model, dan Repository, logika aplikasi dan akses database menjadi lebih terorganisir serta lebih mudah dikembangkan.
