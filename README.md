# basdat

## Entity Relationship Diagram

```mermaid
erDiagram
    %% Superclass
    PENGGUNA {
        string username UK
        string email UK
        string password_hash
        string nama_lengkap
        string nomor_hp
        string alamat_domisili
    }
    
    %% Subclasses (Overlap)
    DONATUR {
        float total_donasi_akumulasi
        string preferensi_kategori_Multi
    }
    INISIATOR {
        string nomor_identitas UK
        int skor_reputasi
    }
    
    %% Superclass
    KAMPANYE {
        string id_kampanye PK
        string judul_kampanye UK
        text deskripsi
        float target_nominal
        date deadline
        date tanggal_pembuatan
        string status_kampanye
        string akun_virtual
    }
    
    %% Subclasses (Disjoint)
    KAMPANYE_BENCANA_ALAM {
        float latitude
        float longitude
    }
    KAMPANYE_SOSIAL_MEDIS {
        string penerima_manfaat
        string dokumen_pendukung_Multi
    }

    TRANSAKSI_DONASI {
        string kode_transaksi PK
        float nominal_donasi
        datetime waktu_transaksi
        string saluran_pembayaran
        string status_pembayaran
    }

    PENCAIRAN_DANA {
        string kode_pencairan PK
        float jumlah_dana
        string status_persetujuan
        string nama_bank_tujuan
        string nomor_rekening_tujuan
        string nama_pemilik_rekening
        string bukti_transfer
    }

    %% Weak Entity
    LAPORAN_UPDATE {
        int nomor_sekuensial Partial_Key
        string judul_update
        text deskripsi_penggunaan
        string lampiran_bukti_Multi
        date tanggal_pelaporan
    }

    %% Relasi Spesialisasi
    PENGGUNA ||--o| DONATUR : "is-a (Overlap)"
    PENGGUNA ||--o| INISIATOR : "is-a (Overlap)"
    KAMPANYE ||--o| KAMPANYE_BENCANA_ALAM : "is-a (Disjoint)"
    KAMPANYE ||--o| KAMPANYE_SOSIAL_MEDIS : "is-a (Disjoint)"

    %% Relasi Operasional
    INISIATOR ||--o{ KAMPANYE : "mengelola"
    DONATUR ||--o{ TRANSAKSI_DONASI : "melakukan"
    KAMPANYE ||--o{ TRANSAKSI_DONASI : "menerima_inflow"
    
    INISIATOR ||--o{ PENCAIRAN_DANA : "mengajukan"
    KAMPANYE ||--o{ PENCAIRAN_DANA : "sumber_outflow"

    %% Identifying Relationship
    KAMPANYE ||--|{ LAPORAN_UPDATE : "memiliki (Identifying)"
```