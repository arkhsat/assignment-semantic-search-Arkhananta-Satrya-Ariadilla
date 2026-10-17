# 🛒 E-Commerce Customer Service FAQ Assistant (RAG Prototype)

Prototype asisten AI (*Customer Support Assistant*) berbasis **Retrieval-Augmented Generation (RAG)** untuk menjawab pertanyaan pelanggan e-commerce secara otomatis, akurat, dan ramah berdasarkan *Knowledge Base* FAQ resmi.

---

## 📌 Daftar Isi
1. [Deskripsi Problem Statement & Knowledge Base](#1-deskripsi-problem-statement--knowledge-base)
2. [Penjelasan Alur Sistem](#2-penjelasan-alur-sistem)
3. [Hasil Pengujian Query & Analisis Relevansi](#3-hasil-pengujian-query--analisis-relevansi)
4. [Keterbatasan Sistem yang Ditemukan](#4-keterbatasan-sistem-yang-ditemukan)
5. [Cara Menjalankan Notebook dan Script](#5-cara-menjalankan-notebook-dan-script)

---

## 1. Deskripsi Problem Statement & Knowledge Base

### 1.1. Problem Statement
Sebuah tim *customer service* e-commerce menerima ratusan pertanyaan repetitif dari pelanggan setiap harinya seputar pemesanan, pengiriman, pembayaran, hingga pengembalian barang. Tim ingin membangun prototype asisten AI yang dapat menjawab pertanyaan pelanggan secara otomatis berdasarkan *knowledge base* FAQ yang sudah ada, sebelum sistem ini dikembangkan lebih lanjut menjadi RAG system yang lebih kompleks.

Sebagai AI Engineer, dibangun prototype awal: sistem yang dapat mencari entri FAQ paling relevan terhadap pertanyaan pelanggan (meskipun pelanggan tidak menggunakan kata-kata yang persis sama dengan FAQ), lalu menyusun jawaban yang natural menggunakan LLM berdasarkan FAQ yang ditemukan.

### 1.2. Knowledge Base
*Knowledge Base* yang digunakan adalah dataset FAQ e-commerce terstruktur yang mencakup topik-topik utama:
- **Produk:** Pengecekan keaslian produk dan garansi.
- **Pengembalian:** Prosedur retur barang dan eskalasi komplain penjual.
- **Promo:** Penggunaan kode voucher saat checkout.
- **Akun:** Atur ulang kata sandi, perubahan nomor telepon, dan penghapusan akun.
- **Pengiriman & Keamanan Data:** Layanan logistik dan privasi akun.

Setiap entri memuat data terstruktur: `(ID, Kategori, Pertanyaan, Jawaban)` yang bertindak sebagai *single source of truth*.

---

## 2. Penjelasan Alur Sistem

Sistem bekerja melalui lima tahapan RAG pipeline:



1. **Ingest (Data Ingestion):** Membaca dataset FAQ dari file sumber (CSV/JSON), membersihkan teks, dan menyusun teks gabungan antara kategori, pertanyaan, dan jawaban.
2. **Embed (Vector Embedding):** Mengubah teks FAQ dan query pelanggan menjadi vektor numerik menggunakan embedding model semantik agar dapat memahami makna kalimat meski kata-katanya berbeda.
3. **Store (Vector Storage):** Menyimpan vektor embedding beserta metadata teks dokumen ke dalam vector database/index (seperti ChromaDB atau FAISS).
4. **Retrieve (Semantic Retrieval):** Menerima query pelanggan, mengubahnya menjadi vektor, lalu mencari entri FAQ paling mirip menggunakan perhitungan *Cosine Similarity*.
5. **Generate (Response Generation):** Menyusun prompt berisi pertanyaan pengguna dan konteks FAQ terpilih, lalu menginstruksikan LLM untuk menghasilkan jawaban yang natural dan setia pada dokumen acuan (*grounded*).

---

## 3. Hasil Pengujian Query & Analisis Relevansi

Berikut merupakan hasil pengujian sistem berdasarkan query uji yang diberikan:

### 3.1. Query 1

#### 1.1. Bagaimana cara mengetahui apakah suatu produk asli atau palsu?
* **Asal pertanyaan:** Sesuai dengan entri *knowledge base* `(42, Produk, "Bagaimana cara mengetahui apakah suatu produk asli atau palsu?")`.
* **Jawaban:** Inti jawaban yang dihasilkan sesuai dengan informasi yang terdapat dalam *knowledge base*.
* **Evaluasi:** **Retriever bekerja dengan baik.**

#### 1.2. Bagaimana jika penjual menolak pengajuan pengembalian saya?
* **Asal pertanyaan:** Sesuai dengan entri *knowledge base* `(29, Pengembalian, "Bagaimana jika penjual menolak pengajuan pengembalian saya?")`.
* **Jawaban:** Inti jawaban yang dihasilkan sesuai dengan informasi yang terdapat dalam *knowledge base*.
* **Evaluasi:** **Retriever bekerja dengan baik.**

#### 1.3. Saya memiliki voucher, bagaimana cara menggunakannya?
* **Asal pertanyaan:** Merujuk pada entri `(37, Promo, "Bagaimana cara menggunakan kode voucher saat checkout?")`. Meskipun susunan kalimatnya berbeda, inti pertanyaan tetap sama.
* **Jawaban:** Inti jawaban yang dihasilkan sesuai dengan informasi yang terdapat dalam *knowledge base*.
* **Evaluasi:** **Retriever bekerja dengan baik.**

#### 1.4. Cara mengganti password akun saya yang lupa?
* **Asal pertanyaan:** Merujuk pada entri `(32, Akun, "Saya lupa password akun, bagaimana cara mengaturnya ulang?")`. Pertanyaan disampaikan dengan kalimat berbeda namun maksud sama.
* **Jawaban:** Inti jawaban yang dihasilkan sesuai dengan informasi yang terdapat dalam *knowledge base*.
* **Evaluasi:** **Retriever bekerja dengan baik.**

#### 1.5. Apa itu *reverse engineering* dan bagaimana cara kerjanya?
* **Asal pertanyaan:** Pertanyaan ini tidak ditemukan dalam *knowledge base*.
* **Jawaban:** Model memberikan respons yang sesuai dengan kondisi tersebut, yaitu menyatakan bahwa informasi yang dibutuhkan tidak diketahui atau tidak tersedia, alih-alih memberikan jawaban yang tidak didukung oleh sumber.
* **Evaluasi:** **Retriever bekerja dengan baik**, dengan catatan bahwa kemampuan model menolak menjawab tanpa sumber yang relevan juga perlu dievaluasi secara terpisah.

---

### 3.2. Query 2

#### 2.1. Apakah bisa menukar barang dengan barang yang berbeda?
* **Asal pertanyaan:** Pertanyaan ini tidak ditemukan dalam *knowledge base*.
* **Jawaban:** Model memberikan respons yang sesuai dengan kondisi tersebut, yaitu menyatakan bahwa informasi yang dibutuhkan tidak tersedia, alih-alih memberikan jawaban tanpa dasar yang jelas.
* **Evaluasi:** **Retriever bekerja dengan baik**, dengan catatan bahwa respons model juga perlu dievaluasi untuk memastikan tidak terjadi halusinasi.

#### 2.2. Apakah bisa barang sampai besok?
* **Asal pertanyaan:** Pertanyaan ini tidak ditemukan dalam *knowledge base*.
* **Jawaban:** Model memberikan respons yang sesuai dengan kondisi tersebut, yaitu menyatakan bahwa informasi yang dibutuhkan tidak tersedia, alih-alih memberikan jawaban tanpa dasar yang jelas.
* **Evaluasi:** **Retriever bekerja dengan baik**, dengan catatan bahwa respons model juga perlu dievaluasi untuk memastikan tidak terjadi halusinasi.

#### 2.3. Saya sudah tidak mau menggunakan aplikasi ini lagi, bagaimana cara menghapus akun?
* **Asal pertanyaan:** Merujuk pada entri `(35, Akun, "Bagaimana cara menghapus akun secara permanen?")`. Meskipun susunan kalimat berbeda, inti pertanyaan sama.
* **Jawaban:** Inti jawaban yang dihasilkan sesuai dengan informasi yang terdapat dalam *knowledge base*.
* **Evaluasi:** **Retriever bekerja dengan baik.**

#### 2.4. Cara mengganti password akun saya yang lupa? Berapa 5 + 4? Dan bagaimana cara mengubah nomor telepon?
* **Asal pertanyaan:** Menggabungkan tiga pertanyaan sekaligus (dua pertanyaan sesuai KB, satu di luar KB):
  1. `(32, Akun, "Saya lupa password akun, bagaimana cara mengaturnya ulang?")`
  2. `(33, Akun, "Bagaimana cara mengubah nomor telepon yang terdaftar di akun?")`
  3. Pertanyaan *"Berapa 5 + 4?"* (tidak ada dalam KB).
* **Jawaban:** Model berhasil menjawab kedua pertanyaan yang informasinya tersedia dalam *knowledge base* (pengaturan sandi dan perubahan nomor telepon). Model juga berhasil menjawab pertanyaan aritmetika sederhana $5 + 4 = 9$, meskipun pertanyaan tersebut tidak berasal dari *knowledge base*.
* **Evaluasi:** **Retriever bekerja dengan baik** dalam menemukan informasi relevan untuk dua pertanyaan dari KB. Selain itu, model menunjukkan kemampuan untuk menjawab pertanyaan tambahan di luar cakupan *knowledge base*.

#### 2.5. Apakah data saya aman? Metode atau cara apa yang digunakan untuk menjaga keamanan data saya?
* **Asal pertanyaan:** Pertanyaan ini tidak ditemukan dalam *knowledge base*.
* **Jawaban:** Model memberikan respons yang sesuai dengan kondisi tersebut, yaitu menyatakan bahwa informasi yang dibutuhkan tidak tersedia, alih-alih memberikan penjelasan yang tidak didukung oleh sumber.
* **Evaluasi:** **Retriever bekerja dengan baik**, dengan catatan bahwa respons model perlu dievaluasi secara terpisah untuk memastikan model tidak menghasilkan informasi yang tidak didukung oleh *knowledge base*.

---

### 3.3. Kesimpulan Pengujian
Berdasarkan hasil pengujian, sistem menunjukkan kemampuan yang baik dalam menangani pertanyaan yang informasinya tersedia dalam *knowledge base*, termasuk pertanyaan yang disampaikan dengan susunan kalimat berbeda. Sistem juga menunjukkan kemampuan untuk menangani pertanyaan yang tidak tercakup dalam *knowledge base* dengan tidak memberikan jawaban yang tidak didukung oleh sumber.

Selain itu, pada pengujian yang menggabungkan beberapa pertanyaan dalam satu query, model mampu menjawab pertanyaan yang relevan dengan *knowledge base* serta pertanyaan tambahan di luar cakupan sumber.

Meskipun demikian, evaluasi perlu membedakan kinerja *retriever* dalam menemukan dokumen yang relevan dengan kemampuan model dalam menyusun jawaban dan menolak menjawab ketika informasi tidak tersedia. Oleh karena itu, hasil pengujian ini menunjukkan indikasi kinerja yang baik, tetapi belum cukup untuk menyimpulkan bahwa seluruh komponen sistem telah bekerja secara optimal tanpa pengujian tambahan.

---

## 4. Keterbatasan Sistem yang Ditemukan

1. **Sistem Terikat Hanya Menjawab Sesuai Knowledge Base (Strict RAG Constraint):**
   Keterbatasannya adalah sistem hanya menjawab apa yang ada pada *Knowledge Base* saja atau hanya dari RAG. Pada test query sudah dilakukan pengujian dengan menanyakan hasil dari `5 + 4`, dan pada sistem yang dikunci ketat (*strict guardrail*), sistem tidak dapat menjawab pertanyaan tersebut padahal itu merupakan pertanyaan mudah untuk LLM. Hal ini terjadi karena batasan guardrail melarang model menggunakan pengetahuan umum bawaannya jika tidak didukung dokumen konteks.
2. **Ketiadaan Data Operasional Real-Time:**
   Knowledge base bersifat statis, sehingga pertanyaan yang memerlukan status real-time (seperti *"Apakah bisa barang sampai besok?"* atau pelacakan nomor resi) tidak dapat dijawab.
3. **Penanganan Pertanyaan Majemuk (Multi-Intent):**
   Retriever mencari kecocokan secara tunggal. Jika query mengandung beberapa pertanyaan dari kategori yang sangat berbeda, ada kemungkinan dokumen pendukung lainnya tidak terambil secara lengkap tanpa adanya teknik *query decomposition*.

---

## 5. Cara Menjalankan Notebook dan Script

### 5.1. Clone 
- open terminal and type git clone https://github.com/arkhsat/assignment-semantic-search-Arkhananta-Satrya-Ariadilla.git

### 5.2. Prasyarat
- pip install requirement.txt

### 5.3. Cara menjalankan
- run starter_notebook_semantic_search.ipynb each cell one by one or use run all