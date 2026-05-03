# Practices 1 Sistem Transportasi
class Kendaraan:
  def berjajian (self):
    print("Kendaraan sedang berjalan")

class Mobil(Kendaraan):
  def berjalan(self):
    print("Mobil berjalan di jalan raya")

class Motor(Kendaraan):
  def berjalan(self):
    print("Motor melaju di jalan raya")

class Pesawat(Kendaraan):
  def berjalan(self):
    print("Pesawat terbang di udara")

class Kapal(Kendaraan):
  def berjalan(self):
    print("Kapal berlayar di air")

daftar_kendaraan = [
    Mobil(),
    Motor(),
    Pesawat(),
    Kapal()
]

for kendaraan in daftar_kendaraan:
  kendaraan.berjalan()

# Practices 2 Sistem Penggajian

class Karyawan:
  def __init__(self, nama):
    self.nama = nama

  def hitung_gaji(self):
    pass

class KaryawanTetap(Karyawan):
  def __init__(self, nama, gaji_bulanan):
    super().__init__(nama)
    self.gaji_bulanan = gaji_bulanan

  def hitung_gaji(self):
    return self.gaji_bulanan

class KaryawanFreelance(Karyawan):
  def __init__(self, nama, jam_kerja, tarif_per_jam):
    super().__init__(nama)
    self.jam_kerja = jam_kerja
    self.tarif_per_jam = tarif_per_jam

  def hitung_gaji(self):
    return self.jam_kerja * self.tarif_per_jam

daftar_karyawan = [
    KaryawanTetap("Andi", 5000000),
    KaryawanFreelance("Budi", 100, 20000),
    KaryawanTetap("Citra", 7000000),
    KaryawanFreelance("Dewi", 80, 25000)
]

for karyawan in daftar_karyawan:
    print(f"Nama: {karyawan.nama}")
    print(f"Gaji: {karyawan.hitung_gaji()}")
    print("-" * 30)

# Practices 3 Update Model Data slide 10

class Barang:
  def __init__(self, id_barang, nama, harga):
    self.id_barang = id_barang
    self.nama = nama
    self.harga = harga

  def info(self):
    return f"[{self.id_barang}] {self.nama:15} | Rp{self.harga:10,}"

class Barang:
  def __init__(self, id_barang, nama, harga):
    self.id_barang = id_barang
    self.nama = nama
    self.harga = harga

  def info(self):
    return f"[{self.id_barang}] {self.nama:18} | Rp{self.harga:10,}"

  def to_dict(self):
     return {"tipe": "umum", "id": self.id_barang, "nama": self.nama, "harga": self.harga}


# Kelas Barang Elektronik

class BarangElektronik(Barang):
  def __init__(self, id_barang, nama, harga, garansi):
    super().__init__(id_barang, nama, harga)
    self.garansi = garansi

  def info(self):
    return f"{super().info()} | Garansi: {self.garansi} bln"

class BarangKonsumsi(Barang):
  def __init__(self, id_barang, nama, harga, tgl_exp):
    super().__init__(id_barang, nama, harga)
    self.tgl_exp = tgl_exp

  def info(self):
    return f"{super().info()} | Exp: {self.tgl_exp}"

laptop = BarangElektronik("E01", "Laptop Gaming", 15000000, 24)
susu = BarangKonsumsi("K05", "Susu UHT", 18000, "22026-12-01")

print(laptop.info())
print(susu.info())

# Practice 3 Update Model Data slide 11

class BarangElektronik(Barang):
  def __init__(self, id_barang, nama, harga, garansai):
    self.garansi = garansai

  def info(self):
    return f"{super().info()} | Garansi: {self.garansi} bln"

  def to_dict(self):
    d = super().to_dict()
    d.update({"tipe": "elektronik", "garansi": self.garansi"})
    return d

class BarangKonsumsi(Barang):
    def __init__(self, id_barang, nama, harga, tgl_exp):
      super().__init__(id_barang, nama, harga)
      self.tgl_exp = tgl_exp

    def info(self):
      d = super().to_dict()
      d.update({"tipe": "konsumsi", "tgl_exp": self.tgl_exp})
      return d

# Practices 3 Update Class Manager(Logika Muat Data Cerdas)

import json
import os

class GudangPolimorfik:
  def __init__(self, file_db='gudang_v3.json'):
    self.file_db = file_db
    self.koleksi = []
    self.muat_data()

  def tambah_barang(self, objek_barang):
    self.koleksi.append(objek_barang)
    self.simpan_data()

  def simpan_data(self):
    with open(self.file_db, 'w') as f:
      data = [b.to_dict() for b in self.koleksi]
      json.dump(data, f, indent=4)

  def muat_data(self):
    if os.path.exists(self.file_db):
      with open(self.file_db, 'r') as f:
        data_list = json.load(f)
        self.koleksi = []
        for d in data_list:

          if d['tipe'] == "elektronik":
            obj = BarangElektronik(d['id'], d['nama'], d['harga'], d['garansi'])
          elif d['tipe'] == "konsumsi":
            obj = BarangKonsumsi(d['id'], d['nama'], d['harga'], d['tgl_exp'])
          else:
            obj = Barang(d['id'], d['nama'], d['harga'])
          self.koleksi.append(obj)

  def laporan_stok(self):
    print("\n" + "="*70)
    print(f"{'id':6} | {'NAMA BARANG':18} | {'HARGA:13'} | {'KETERANGAN TAMBAHAN'}")
    print("_" *70)
    for b in self.koleksi:

        print(b.info())
    print("="*70)

def main():
      app = GudangPolimorfik()

      if not app.koleksi:
        print("💾 Database kosong, membuat data awal...")
        app.tambah_barang(BarangElektronik("E01", "Loptop Pro", 12000000, 12))
        app.tambah_barang(BarangKonsumsi("K02", "Roti Coklat", 15000, "2026-05-10"))
        app.tambah_barang(Barang("U03", "Kabel Data", 25000))

        app.laporan_stok()

if __name__ == "__main__":
    main()

import json
import os

# Practice 3 Update Model Data slide 11


class BarangElektronik(Barang):
  def __init__(self, id_barang, nama, harga, garansai):
    self.garansi = garansai

  def info(self):
    return f"{super().info()} | Garansi: {self.garansi} bln"

  def to_dict(self):
    d = super().to_dict()
    d.update({"tipe": "elektronik", "garansi": self.garansi})
    return d

class BarangKonsumsi(Barang):
  def __init__(self, id_barang, nama, harga, tgl_exp):
      super().__init__(id_barang, nama, harga)
      self.tgl_exp = tgl_exp

  def info(self):
      d = super().to_dict()
      d.update({"tipe": "konsumsi", "tgl_exp": self.tgl_exp})
      return d

# Practices 3 Update Class Manager(Logika Muat Data Cerdas)

class GudangPolimorfik:
  def __init__(self, file_db='gudang_v3.json'):
    self.file_db = file_db
    self.koleksi = []
    self.muat_data()

  def tambah_barang(self, objek_barang):
    self.koleksi.append(objek_barang)
    self.simpan_data()

  def simpan_data(self):
    with open(self.file_db, 'w') as f:
      data = [b.to_dict() for b in self.koleksi]
      json.dump(data, f, indent=4)

  def muat_data(self):
    if os.path.exists(self.file_db):
      with open(self.file_db, 'r') as f:
        data_list = json.load(f)
        self.koleksi = []
        for d in data_list:

          if d['tipe'] == "elektronik":
            obj = BarangElektronik(d['id'], d['nama'], d['harga'], d['garansi'])
          elif d['tipe'] == "konsumsi":
            obj = BarangKonsumsi(d['id'], d['nama'], d['harga'], d['tgl_exp'])
          else:
            obj = Barang(d['id'], d['nama'], d['harga'])
          self.koleksi.append(obj)

  def laporan_stok(self):
    print("\n" + "="*70)
    print(f"{'id':6} | {'NAMA BARANG':18} | {'HARGA:13'} | {'KETERANGAN TAMBAHAN'}")
    print("_" *70)
    for b in self.koleksi:

        print(b.info())
    print("="*70)

def main():
      app = GudangPolimorfik()

      if not app.koleksi:
        print("💾 Database kosong, membuat data awal...")
        app.tambah_barang(BarangElektronik("E01", "Loptop Pro", 12000000, 12))
        app.tambah_barang(BarangKonsumsi("K02", "Roti Coklat", 15000, "2026-05-10"))
        app.tambah_barang(Barang("U03", "Kabel Data", 25000))

        app.laporan_stok()

if __name__ == "__main__":
    main()
# TUGAS PRAKTIKUM 1
import json
import os
from datetime import datetime

# DEFINISI CLASS BARANG (MODEL)

class Barang:
    def __init__(self, id_barang, nama, harga):
        self.id = id_barang
        self.nama = nama
        self.harga = harga
        self.tipe = "umum"

    def to_dict(self):
        return {
            "id": self.id,
            "nama": self.nama,
            "harga": self.harga,
            "tipe": self.tipe
        }

    def info(self):
        return f"{self.id:6} | {self.nama:18} | {self.harga:<13} | -"

class BarangElektronik(Barang):
    def __init__(self, id_barang, nama, harga, garansi):
        super().__init__(id_barang, nama, harga)
        self.garansi = garansi
        self.tipe = "elektronik"

    def to_dict(self):
        d = super().to_dict()
        d.update({"garansi": self.garansi})
        return d

    def info(self):
        return f"{self.id:6} | {self.nama:18} | {self.harga:<13} | Garansi: {self.garansi} bulan"

class BarangKonsumsi(Barang):
    def __init__(self, id_barang, nama, harga, tgl_exp):
        super().__init__(id_barang, nama, harga)
        self.tgl_exp = tgl_exp
        self.tipe = "konsumsi"

    def to_dict(self):
        d = super().to_dict()
        d.update({"tgl_exp": self.tgl_exp})
        return d

    def apakah_kadaluarsa(self):
        # Mengubah string "YYYY-MM-DD" menjadi objek tanggal untuk dibandingkan
        tgl_target = datetime.strptime(self.tgl_exp, "%Y-%m-%d").date()
        return tgl_target < datetime.now().date()

    def info(self):
        status = "[KADALUARSA]" if self.apakah_kadaluarsa() else ""
        return f"{self.id:6} | {self.nama:18} | {self.harga:<13} | Exp: {self.tgl_exp} {status}"

# CLASS GUDANG (LOGIKA BISNIS)

class GudangPolimorfik:
    def __init__(self, file_db='gudang_v3.json'):
        self.file_db = file_db
        self.koleksi = []
        self.muat_data()

    def tambah_barang(self, objek_barang):
        self.koleksi.append(objek_barang)
        self.simpan_data()

    def simpan_data(self):
        with open(self.file_db, 'w') as f:
            data = [b.to_dict() for b in self.koleksi]
            json.dump(data, f, indent=4)

    def muat_data(self):
        if os.path.exists(self.file_db):
            with open(self.file_db, 'r') as f:
                data_list = json.load(f)
                self.koleksi = []
                for d in data_list:
                    if d['tipe'] == "elektronik":
                        obj = BarangElektronik(d['id'], d['nama'], d['harga'], d['garansi'])
                    elif d['tipe'] == "konsumsi":
                        obj = BarangKonsumsi(d['id'], d['nama'], d['harga'], d['tgl_exp'])
                    else:
                        obj = Barang(d['id'], d['nama'], d['harga'])
                    self.koleksi.append(obj)

    def laporan_stok(self):
        print("\n" + "="*75)
        print(f"{'ID':6} | {'NAMA BARANG':18} | {'HARGA':13} | {'KETERANGAN TAMBAHAN'}")
        print("-" *75)
        for b in self.koleksi:
            print(b.info())
        print("="*75)

    def cek_barang_kadaluarsa(self):
        """Fitur baru untuk memfilter barang yang sudah expired"""
        print("\n⚠️ PERINGATAN BARANG KADALUARSA ⚠️")
        print("-" * 40)
        found = False
        for b in self.koleksi:
            if isinstance(b, BarangKonsumsi) and b.apakah_kadaluarsa():
                print(f"ID: {b.id} | Nama: {b.nama} | Exp: {b.tgl_exp}")
                found = True

        if not found:
            print("Tidak ada barang kadaluarsa. Semua stok aman.")
        print("-" * 40)

# MAIN PROGRAM

def main():
    app = GudangPolimorfik()

    # Jika database baru/kosong, isi dengan data contoh
    if not app.koleksi:
        print("💾 Database kosong, membuat data awal...")
        app.tambah_barang(BarangElektronik("E01", "Laptop Pro", 12000000, 12))
        app.tambah_barang(BarangKonsumsi("K02", "Roti Coklat", 15000, "2026-05-10")) # Belum Expired
        app.tambah_barang(BarangKonsumsi("K03", "Susu Segar", 10000, "2023-01-01"))  # Sudah Expired
        app.tambah_barang(Barang("U03", "Kabel Data", 25000))

    # Tampilkan semua barang
    app.laporan_stok()

    # Jalankan fitur cek kadaluarsa
    app.cek_barang_kadaluarsa()

if __name__ == "__main__":
    main()

#TUGAS Praktikum 2
class Karyawan:
    def __init__(self, nama):
        self.nama = nama

    def hitung_gaji(self):
        pass

    def gaji_bersih(self):
        # Mengambil gaji kotor (base salary + bonus jika ada)
        gaji_kotor = self.hitung_gaji()

        # Logika pajak: Jika gaji diatas 5 juta dipotong pajak 5%
        if gaji_kotor > 5000000:
            pajak = gaji_kotor * 0.05
        else:
            pajak = 0

        return gaji_kotor - pajak

class KaryawanTetap(Karyawan):
    def __init__(self, nama, gaji_bulanan):
        super().__init__(nama)
        self.gaji_bulanan = gaji_bulanan

    def hitung_gaji(self):
        # Karyawan tetap dapat bonus 10%
        bonus = self.gaji_bulanan * 0.10
        return self.gaji_bulanan + bonus

class KaryawanFreelance(Karyawan):
    def __init__(self, nama, jam_kerja, tarif_per_jam):
        super().__init__(nama)
        self.jam_kerja = jam_kerja
        self.tarif_per_jam = tarif_per_jam

    def hitung_gaji(self):
        return self.jam_kerja * self.tarif_per_jam

# Data Karyawan
daftar_karyawan = [
    KaryawanTetap("Andi", 5000000),      # Gaji kotor: 5jt + 500rb (bonus) = 5.5jt (Kena Pajak)
    KaryawanFreelance("Budi", 100, 20000), # Gaji kotor: 2jt (Tidak Pajak)
    KaryawanTetap("Citra", 7000000),     # Gaji kotor: 7jt + 700rb (bonus) = 7.7jt (Kena Pajak)
    KaryawanFreelance("Dewi", 80, 25000)   # Gaji kotor: 2jt (Tidak Pajak)
]

# Output
print(f"{'NAMA':<10} | {'GAJI KOTOR':<12} | {'GAJI BERSIH':<12}")
print("-" * 40)
for karyawan in daftar_karyawan:
    print(f"{karyawan.nama:<10} | {karyawan.hitung_gaji():<12,.0f} | {karyawan.gaji_bersih():<12,.0f}")

    #TUGAS PRAKTIKUM 3

import json
import os
from datetime import datetime  # Step 1: Impor datetime

#  CLASS BARANG (MODEL)
# Step 1
class Barang:
    def __init__(self, id_barang, nama, harga):
        self.id_barang = id_barang
        self.nama = nama
        self.harga = harga

    def info(self):
        return f"[{self.id_barang:4}] {self.nama:18} | Rp{self.harga:10,}"

    def to_dict(self):
        return {"tipe": "umum", "id": self.id_barang, "nama": self.nama, "harga": self.harga}

class BarangElektronik(Barang):
    def __init__(self, id_barang, nama, harga, garansi):
        super().__init__(id_barang, nama, harga) # Pastikan memanggil super()
        self.garansi = garansi

    def info(self):
        return f"{super().info()} | Garansi: {self.garansi} bln"

    def to_dict(self):
        d = super().to_dict()
        d.update({"tipe": "elektronik", "garansi": self.garansi})
        return d

class BarangKonsumsi(Barang):
    def __init__(self, id_barang, nama, harga, tgl_exp):
        super().__init__(id_barang, nama, harga)
        self.tgl_exp = tgl_exp

# Step 2: Modifikasi info() dengan logika pengecekan kadaluarsa
    def info(self):
        # Ambil tanggal hari ini
        tgl_sekarang = datetime.now().date()
        # Ubah string tgl_exp (format YYYY-MM-DD) menjadi objek tanggal
        tgl_kadaluarsa = datetime.strptime(self.tgl_exp, "%Y-%m-%d").date()

        status = ""
        if tgl_kadaluarsa < tgl_sekarang:
            status = " (!!! KADALUARSA !!!)"

        return f"{super().info()} | Exp: {self.tgl_exp}{status}"

    def to_dict(self):
        d = super().to_dict()
        d.update({"tipe": "konsumsi", "tgl_exp": self.tgl_exp})
        return d


# CLASS GUDANG (MANAGER)

class GudangPolimorfik:
    def __init__(self, file_db='gudang_v3.json'):
        self.file_db = file_db
        self.koleksi = []
        self.muat_data()

    def tambah_barang(self, objek_barang):
        self.koleksi.append(objek_barang)
        self.simpan_data()

    def simpan_data(self):
        with open(self.file_db, 'w') as f:
            data = [b.to_dict() for b in self.koleksi]
            json.dump(data, f, indent=4)

    def muat_data(self):
        if os.path.exists(self.file_db):
            with open(self.file_db, 'r') as f:
                data_list = json.load(f)
                self.koleksi = []
                for d in data_list:
                    if d['tipe'] == "elektronik":
                        obj = BarangElektronik(d['id'], d['nama'], d['harga'], d['garansi'])
                    elif d['tipe'] == "konsumsi":
                        obj = BarangKonsumsi(d['id'], d['nama'], d['harga'], d['tgl_exp'])
                    else:
                        obj = Barang(d['id'], d['nama'], d['harga'])
                    self.koleksi.append(obj)

    def laporan_stok(self):
        print("\n" + "="*85)
        print(f"{'ID':6} | {'NAMA BARANG':18} | {'HARGA':13} | {'KETERANGAN TAMBAHAN'}")
        print("-" * 85)
        for b in self.koleksi:
            print(b.info())
        print("="*85)

# MAIN PROGRAM

def main():
    app = GudangPolimorfik()

    if not app.koleksi:
        print("💾 Database kosong, membuat data awal...")
        app.tambah_barang(BarangElektronik("E01", "Laptop Pro", 12000000, 12))
        # Contoh barang kadaluarsa (set tanggal ke masa lalu untuk testing)
        app.tambah_barang(BarangKonsumsi("K02", "Susu Basi", 15000, "2023-01-01"))
        # Contoh barang belum kadaluarsa
        app.tambah_barang(BarangKonsumsi("K03", "Roti Coklat", 15000, "2026-05-10"))
        app.tambah_barang(Barang("U03", "Kabel Data", 25000))

    app.laporan_stok()

if __name__ == "__main__":
    main()
