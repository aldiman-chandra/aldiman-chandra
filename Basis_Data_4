CREATE DATABASE KeseimbanganHidup;
USE KeseimbanganHidup;

CREATE TABLE Pengguna (
    IDPengguna INT PRIMARY KEY AUTO_INCREMENT,
    NamaPengguna VARCHAR(50) NOT NULL UNIQUE,
    KataSandi VARCHAR(255) NOT NULL,
    Email VARCHAR(100) NOT NULL UNIQUE,
    NamaLengkap VARCHAR(100) NOT NULL,
    TanggalLahir DATE,
    Dibuat TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE Aktivitas (
    IDAktivitas INT PRIMARY KEY AUTO_INCREMENT,
    IDPengguna INT,
    JenisAktivitas ENUM('Kerja', 'Keluarga', 'Kesehatan') NOT NULL,
    Judul VARCHAR(100) NOT NULL,
    Deskripsi TEXT,
    WaktuMulai DATETIME,
    WaktuSelesai DATETIME,
    DiBuat TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (IDPengguna) REFERENCES Pengguna(IDPengguna)
);

CREATE TABLE MetrikKesehatan (
    IDMetrik INT PRIMARY KEY AUTO_INCREMENT,
    IDPengguna INT,
    JenisMetrik ENUM('Langkah', 'Tidur', 'Air', 'Olahraga') NOT NULL,
    Nilai FLOAT NOT NULL,
    Tanggal DATE NOT NULL,
    DiBuat TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (IDPengguna) REFERENCES Pengguna(IDPengguna)
);

CREATE TABLE AnggotaKeluarga (
    IDAnggota INT PRIMARY KEY AUTO_INCREMENT,
    IDPengguna INT,
    Nama VARCHAR(100) NOT NULL,
    Hubungan VARCHAR(50) NOT NULL,
    TanggalLahir DATE,
    DiBuat TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (IDPengguna) REFERENCES Pengguna(IDPengguna)
);

CREATE TABLE Tujuan (
    IDTujuan INT PRIMARY KEY AUTO_INCREMENT,
    IDPengguna INT,
    JenisTujuan ENUM('Kerja', 'Keluarga', 'Kesehatan') NOT NULL,
    Judul VARCHAR(100) NOT NULL,
    Deskripsi TEXT,
    TanggalTarget DATE,
    Status ENUM('Belum Dimulai', 'Sedang Berlangsung', 'Selesai') DEFAULT 'Belum Dimulai',
    DiBuat TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (IDPengguna) REFERENCES Pengguna(IDPengguna)
);

CREATE TABLE Pengingat (
    IDPengingat INT PRIMARY KEY AUTO_INCREMENT,
    IDPengguna INT,
    Judul VARCHAR(100) NOT NULL,
    Deskripsi TEXT,
    WaktuPengingat DATETIME NOT NULL,
    ApakahBerulang BOOLEAN DEFAULT FALSE,
    PolaPengulangan VARCHAR(50),
    DiBuat TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (IDPengguna) REFERENCES Pengguna(IDPengguna)
);

CREATE TABLE ProyekKerja (
    IDProyek INT PRIMARY KEY AUTO_INCREMENT,
    IDPengguna INT,
    NamaProyek VARCHAR(100) NOT NULL,
    Deskripsi TEXT,
    TanggalMulai DATE,
    TanggalSelesai DATE,
    Status ENUM('Belum Dimulai', 'Sedang Berlangsung', 'Selesai') DEFAULT 'Belum Dimulai',
    DiBuat TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (IDPengguna) REFERENCES Pengguna(IDPengguna)
);

CREATE VIEW RingkasanAktivitasHarian AS
SELECT 
    IDPengguna,
    DATE(WaktuMulai) AS Tanggal,
    JenisAktivitas,
    COUNT(*) AS JumlahAktivitas,
    SUM(TIMESTAMPDIFF(MINUTE, WaktuMulai, WaktuSelesai)) AS TotalMenit
FROM Aktivitas
GROUP BY IDPengguna, DATE(WaktuMulai), JenisAktivitas;

CREATE VIEW KemajuanTujuan AS
SELECT 
    t.IDPengguna,
    t.JenisTujuan,
    t.Judul,
    t.TanggalTarget,
    t.Status,
    COUNT(a.IDAktivitas) AS JumlahAktivitasTerkait
FROM Tujuan t
LEFT JOIN Aktivitas a ON t.IDPengguna = a.IDPengguna AND t.JenisTujuan = a.JenisAktivitas
GROUP BY t.IDTujuan;

CREATE VIEW RingkasanKesehatanMingguan AS
SELECT 
    IDPengguna,
    YEAR(Tanggal) AS Tahun,
    WEEK(Tanggal) AS Minggu,
    AVG(CASE WHEN JenisMetrik = 'Langkah' THEN Nilai ELSE NULL END) AS RataLangkah,
    AVG(CASE WHEN JenisMetrik = 'Tidur' THEN Nilai ELSE NULL END) AS RataJamTidur,
    AVG(CASE WHEN JenisMetrik = 'Air' THEN Nilai ELSE NULL END) AS RataAsupanAir,
    SUM(CASE WHEN JenisMetrik = 'Olahraga' THEN Nilai ELSE 0 END) AS TotalMenitOlahraga
FROM MetrikKesehatan
GROUP BY IDPengguna, YEAR(Tanggal), WEEK(Tanggal);


INSERT INTO Pengguna (NamaPengguna, KataSandi, Email, NamaLengkap, TanggalLahir)
VALUES ('maya_seimbang', 'kataSandiTerenkripsi123', 'maya@contoh.com', 'Maya Suria', '1987-05-15');

INSERT INTO Aktivitas (IDPengguna, JenisAktivitas, Judul, Deskripsi, WaktuMulai, WaktuSelesai)
VALUES (1, 'Kerja', 'Rapat dengan Tim Pemasaran', 'Diskusi strategi Q3', '2024-09-26 10:00:00', '2024-09-26 11:30:00');

INSERT INTO MetrikKesehatan (IDPengguna, JenisMetrik, Nilai, Tanggal)
VALUES (1, 'Langkah', 8500, '2024-09-26');

INSERT INTO AnggotaKeluarga (IDPengguna, Nama, Hubungan, TanggalLahir)
VALUES (1, 'Lily', 'Anak', '2018-03-10');

INSERT INTO Tujuan (IDPengguna, JenisTujuan, Judul, Deskripsi, TanggalTarget)
VALUES (1, 'Kesehatan', 'Lari Maraton', 'Selesaikan maraton penuh dalam waktu kurang dari 4 jam', '2025-03-01');

SELECT * FROM Aktivitas 
WHERE IDPengguna = 1 AND DATE(WaktuMulai) = CURDATE();

SELECT * FROM RingkasanAktivitasHarian 
WHERE IDPengguna = 1 AND Tanggal >= DATE_SUB(CURDATE(), INTERVAL 7 DAY);

SELECT * FROM KemajuanTujuan WHERE IDPengguna = 1;

SELECT * FROM RingkasanKesehatanMingguan 
WHERE IDPengguna = 1 AND Tahun = YEAR(CURDATE()) AND Minggu = WEEK(CURDATE());

UPDATE Tujuan 
SET Status = 'Sedang Berlangsung' 
WHERE IDTujuan = 1;

UPDATE MetrikKesehatan 
SET Nilai = 10000 
WHERE IDMetrik = 1;

UPDATE Aktivitas 
SET WaktuMulai = '2024-09-26 11:00:00', WaktuSelesai = '2024-09-26 12:30:00' 
WHERE IDAktivitas = 1;

DELETE FROM Pengingat WHERE IDPengingat = 1;

DELETE FROM ProyekKerja 
WHERE Status = 'Selesai' AND TanggalSelesai < DATE_SUB(CURDATE(), INTERVAL 1 TAHUN);
