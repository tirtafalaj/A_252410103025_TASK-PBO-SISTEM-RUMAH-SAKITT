using System;
using System.Collections.Generic;

namespace SistemRumahSakit
{
    public class Orang
    {
        public string Nama { get; set; }
        public int Umur { get; set; }

        public Orang(string nama, int umur)
        {
            Nama = nama;
            Umur = umur;
        }

        public virtual void Aktivitas()
        {
            Console.WriteLine($"{Nama} sedang melakukan aktivitas sehari-hari.");
        }

        public virtual void InfoOrang()
        {
            Console.WriteLine($"Nama: {Nama} | Umur: {Umur} tahun");
        }
    }

    public class Pasien : Orang
    {
        public string Keluhan { get; set; }

        public Pasien(string nama, int umur, string keluhan) : base(nama, umur)
        {
            Keluhan = keluhan;
        }

        public void CekKeluhan()
        {
            Console.WriteLine($"Keluhan {Nama}: {Keluhan}");
        }

        public override void Aktivitas()
        {
            Console.WriteLine($"{Nama} (Pasien) sedang menunggu pemeriksaan.");
        }

        public override void InfoOrang()
        {
            base.InfoOrang();
            Console.WriteLine($"   Keluhan: {Keluhan}");
        }
    }

    public class TenagaMedis : Orang
    {
        public string Spesialisasi { get; set; }

        public TenagaMedis(string nama, int umur, string spesialisasi) : base(nama, umur)
        {
            Spesialisasi = spesialisasi;
        }

        public void CekSpesialis()
        {
            Console.WriteLine($"Spesialisasi {Nama}: {Spesialisasi}");
        }

        public override void Aktivitas()
        {
            Console.WriteLine($"{Nama} (Tenaga Medis) sedang bertugas di rumah sakit.");
        }

        public override void InfoOrang()
        {
            base.InfoOrang();
            Console.WriteLine($"   Spesialisasi: {Spesialisasi}");
        }
    }

    public class Dokter : TenagaMedis
    {
        public Dokter(string nama, int umur, string spesialisasi)
            : base(nama, umur, spesialisasi) { }

        public void Diagnosa()
        {
            Console.WriteLine($"{Nama} sedang mendiagnosa penyakit pasien sesuai bidang {Spesialisasi}.");
        }

        public override void Aktivitas()
        {
            Console.WriteLine($"{Nama} (Dokter) sedang memeriksa dan menangani pasien.");
        }
    }

    public class Perawat : TenagaMedis
    {
        public Perawat(string nama, int umur, string spesialisasi)
            : base(nama, umur, spesialisasi) { }

        public void CekPasien()
        {
            Console.WriteLine($"{Nama} sedang mengecek suhu tubuh, tekanan darah, dan kondisi pasien.");
        }

        public override void Aktivitas()
        {
            Console.WriteLine($"{Nama} (Perawat) sedang membantu dokter dan merawat pasien.");
        }
    }

    public class PasienAnak : Pasien
    {
        public PasienAnak(string nama, int umur, string keluhan)
            : base(nama, umur, keluhan) { }

        public void Menangis()
        {
            Console.WriteLine($"{Nama} (Anak) menangis karena tidak nyaman dengan keluhannya: {Keluhan}.");
        }

        public override void Aktivitas()
        {
            Console.WriteLine($"{Nama} (Pasien Anak) sedang bermain ditemani orang tuanya.");
        }
    }

    public class PasienDewasa : Pasien
    {
        public PasienDewasa(string nama, int umur, string keluhan)
            : base(nama, umur, keluhan) { }

        public void Konsultasi()
        {
            Console.WriteLine($"{Nama} sedang berkonsultasi dengan dokter mengenai keluhan: {Keluhan}.");
        }

        public override void Aktivitas()
        {
            Console.WriteLine($"{Nama} (Pasien Dewasa) sedang mengantre untuk konsultasi.");
        }
    }

    public class RumahSakit
    {
        public string Nama { get; }
        private List<Orang> daftarOrang = new List<Orang>();

        public IReadOnlyList<Orang> KoleksiOrang => daftarOrang;

        public RumahSakit(string nama)
        {
            Nama = nama;
        }

        public void TambahOrang(Orang orang)
        {
            if (orang == null)
            {
                Console.WriteLine("Data orang tidak valid (null).");
                return;
            }
            daftarOrang.Add(orang);
            Console.WriteLine($"[+] {orang.Nama} berhasil ditambahkan ke {Nama}.");
        }

        public void DaftarOrang()
        {
            Console.WriteLine($"--- Daftar Orang di {Nama} (Total: {daftarOrang.Count}) ---");
            int no = 1;
            foreach (Orang o in daftarOrang)
            {
                Console.Write($"{no++}. [{o.GetType().Name}] ");
                o.InfoOrang();
            }
        }
    }

    class Program
    {
        static void Judul(string teks)
        {
            Console.WriteLine();
            Console.WriteLine($"=== {teks} ===");
        }

        static void Main(string[] args)
        {

            RumahSakit rs = new RumahSakit("RS Sehat Sentosa");

            Orang pengunjung = new Orang("Pak Joko", 50);
            Dokter dokter = new Dokter("dr. Andi", 40, "Penyakit Dalam");
            Perawat perawat = new Perawat("Siti", 28, "Keperawatan Umum");
            Pasien pasienUmum = new Pasien("Dewi", 30, "Batuk berkepanjangan");
            PasienAnak pasienAnak = new PasienAnak("Budi", 6, "Demam tinggi");
            PasienDewasa pasienDewasa = new PasienDewasa("Rina", 35, "Sakit kepala");

            Judul("6c. Menambahkan ke rumah sakit");
            rs.TambahOrang(pengunjung);
            rs.TambahOrang(dokter);
            rs.TambahOrang(perawat);
            rs.TambahOrang(pasienUmum);
            rs.TambahOrang(pasienAnak);
            rs.TambahOrang(pasienDewasa);

            Judul("6d. Tampilkan semua data");
            rs.DaftarOrang();

            Judul("6e. Polymorphism: Aktivitas() dipanggil lewat tipe Orang");
            foreach (Orang o in rs.KoleksiOrang)
            {
                o.Aktivitas();
            }

            Judul("6f. Memanggil method khusus tiap kelas");
            foreach (Orang o in rs.KoleksiOrang)
            {
                switch (o)
                {
                    case Dokter d:
                        d.CekSpesialis();
                        d.Diagnosa();
                        break;
                    case Perawat p:
                        p.CekSpesialis();
                        p.CekPasien();
                        break;
                    case PasienAnak pa:
                        pa.CekKeluhan();
                        pa.Menangis();
                        break;
                    case PasienDewasa pd:
                        pd.CekKeluhan();
                        pd.Konsultasi();
                        break;
                    case Pasien ps:
                        ps.CekKeluhan();
                        break;
                    default:
                        Console.WriteLine($"{o.Nama} tidak memiliki method khusus.");
                        break;
                }
            }

            Judul("Soal 1: Aktivitas() pada Dokter dan Pasien");
            dokter.Aktivitas();
            pasienUmum.Aktivitas();

            Judul("Soal 2: Diagnosa() pada Dokter");
            dokter.Diagnosa();

            Judul("Soal 3: Informasi lengkap Dokter");
            dokter.InfoOrang(); 
            Console.WriteLine($"Cara 2 -> Nama: {dokter.Nama}, Umur: {dokter.Umur}, Spesialisasi: {dokter.Spesialisasi}");

            Judul("Soal 4: Menangis() pada PasienAnak");
            pasienAnak.Menangis();

            Judul("Soal 5: Variabel bertipe Orang berisi objek Perawat");
            Orang orang = new Perawat("Ani", 25, "Keperawatan Anak");
            orang.Aktivitas();

            if (orang is Perawat perawatCast)
            {
                perawatCast.CekPasien();
            }

            Console.WriteLine();
            Console.WriteLine("Tekan sembarang tombol untuk keluar...");
            Console.ReadKey();
        }
    }
}
