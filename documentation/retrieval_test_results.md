# Hasil Pengujian Sistem RAG (Answer)

## 1. Pendahuluan

Pengujian ini dilakukan untuk mengevaluasi kemampuan sistem *Retrieval-Augmented Generation* (RAG) dalam menemukan informasi yang relevan dari *knowledge base* serta kemampuan model dalam menghasilkan jawaban berdasarkan informasi yang tersedia.

Pengujian mencakup pertanyaan yang sesuai dengan data dalam *knowledge base*, pertanyaan dengan susunan kalimat berbeda, pertanyaan yang tidak tercakup dalam sumber, serta pertanyaan yang menggabungkan beberapa topik sekaligus.

## 2. Hasil Pengujian Query 1

### 2.1. Bagaimana cara mengetahui apakah suatu produk asli atau palsu?

- **Asal pertanyaan:** Sesuai dengan entri *knowledge base* ID `42`, kategori `Produk`, dengan pertanyaan "Bagaimana cara mengetahui apakah suatu produk asli atau palsu?"
- **Jawaban:** Inti jawaban yang dihasilkan sesuai dengan informasi dalam *knowledge base*.
- **Evaluasi:** Retriever berhasil menemukan informasi yang relevan.

### 2.2. Bagaimana jika penjual menolak pengajuan pengembalian saya?

- **Asal pertanyaan:** Sesuai dengan entri *knowledge base* ID `29`, kategori `Pengembalian`, dengan pertanyaan "Bagaimana jika penjual menolak pengajuan pengembalian saya?"
- **Jawaban:** Inti jawaban yang dihasilkan sesuai dengan informasi dalam *knowledge base*.
- **Evaluasi:** Retriever berhasil menemukan informasi yang relevan.

### 2.3. Saya memiliki voucher, bagaimana cara menggunakannya?

- **Asal pertanyaan:** Merujuk pada entri *knowledge base* ID `37`, kategori `Promo`, dengan pertanyaan "Bagaimana cara menggunakan kode voucher saat checkout?"
- **Jawaban:** Meskipun susunan kalimat berbeda, sistem menghasilkan jawaban yang sesuai dengan informasi dalam *knowledge base*.
- **Evaluasi:** Retriever mampu menangani variasi formulasi pertanyaan dan menemukan informasi yang relevan.

### 2.4. Cara mengganti password akun saya yang lupa?

- **Asal pertanyaan:** Merujuk pada entri *knowledge base* ID `32`, kategori `Akun`, dengan pertanyaan "Saya lupa password akun, bagaimana cara mengaturnya ulang?"
- **Jawaban:** Inti jawaban yang dihasilkan sesuai dengan informasi dalam *knowledge base*.
- **Evaluasi:** Retriever mampu menemukan informasi yang relevan meskipun pertanyaan disampaikan dengan susunan kalimat berbeda.

### 2.5. Apa itu *reverse engineering* dan bagaimana cara kerjanya?

- **Asal pertanyaan:** Tidak ditemukan dalam *knowledge base*.
- **Jawaban:** Model menyatakan bahwa informasi yang dibutuhkan tidak tersedia, alih-alih memberikan jawaban yang tidak didukung oleh sumber.
- **Evaluasi:** Sistem menunjukkan perilaku yang sesuai ketika informasi tidak tersedia. Namun, kemampuan retriever dalam mendeteksi ketiadaan informasi dan kemampuan model untuk menolak menjawab perlu dievaluasi secara terpisah.

## 3. Hasil Pengujian Query 2

### 3.1. Apakah bisa menukar barang dengan barang yang berbeda?

- **Asal pertanyaan:** Tidak ditemukan dalam *knowledge base*.
- **Jawaban:** Model menyatakan bahwa informasi yang dibutuhkan tidak tersedia, alih-alih memberikan jawaban tanpa dasar yang jelas.
- **Evaluasi:** Sistem menunjukkan perilaku yang sesuai untuk pertanyaan yang tidak tercakup dalam sumber. Validitas penolakan jawaban tetap perlu diuji lebih lanjut.

### 3.2. Apakah bisa barang sampai besok?

- **Asal pertanyaan:** Tidak ditemukan dalam *knowledge base*.
- **Jawaban:** Model menyatakan bahwa informasi yang dibutuhkan tidak tersedia.
- **Evaluasi:** Sistem menunjukkan perilaku yang sesuai ketika tidak terdapat informasi yang mendukung jawaban. Pengujian tambahan diperlukan untuk memastikan model tidak menghasilkan informasi yang tidak berdasar.

### 3.3. Saya sudah tidak mau menggunakan aplikasi ini lagi, bagaimana cara menghapus akun?

- **Asal pertanyaan:** Merujuk pada entri *knowledge base* ID `35`, kategori `Akun`, dengan pertanyaan "Bagaimana cara menghapus akun secara permanen?"
- **Jawaban:** Inti jawaban yang dihasilkan sesuai dengan informasi dalam *knowledge base*.
- **Evaluasi:** Retriever mampu menemukan informasi yang relevan meskipun pertanyaan disampaikan dengan formulasi berbeda.

### 3.4. Cara mengganti password akun saya yang lupa? Berapa 5 + 4? Dan bagaimana cara mengubah nomor telepon?

- **Asal pertanyaan:** Query menggabungkan tiga pertanyaan, yaitu:
  1. Pengaturan ulang kata sandi, sesuai dengan entri ID `32`: "Saya lupa password akun, bagaimana cara mengaturnya ulang?"
  2. Perhitungan aritmetika sederhana, yaitu "Berapa 5 + 4?", yang tidak berasal dari *knowledge base*.
  3. Perubahan nomor telepon, sesuai dengan entri ID `33`: "Bagaimana cara mengubah nomor telepon yang terdaftar di akun?"
- **Jawaban:** Model berhasil menjawab pertanyaan mengenai pengaturan ulang kata sandi dan perubahan nomor telepon berdasarkan informasi yang tersedia. Model juga menjawab pertanyaan aritmetika dengan benar, yaitu `5 + 4 = 9`.
- **Evaluasi:** Retriever menunjukkan kemampuan menemukan informasi yang relevan untuk pertanyaan yang tercakup dalam *knowledge base*. Sementara itu, kemampuan menjawab pertanyaan aritmetika menunjukkan kemampuan tambahan model di luar cakupan sumber. Kedua kemampuan tersebut perlu dinilai secara terpisah.

### 3.5. Apakah data saya aman? Metode atau cara apa yang digunakan untuk menjaga keamanan data saya?

- **Asal pertanyaan:** Tidak ditemukan dalam *knowledge base*.
- **Jawaban:** Model menyatakan bahwa informasi yang dibutuhkan tidak tersedia, alih-alih memberikan penjelasan yang tidak didukung oleh sumber.
- **Evaluasi:** Sistem menunjukkan perilaku yang sesuai ketika informasi keamanan data tidak tersedia dalam *knowledge base*. Pengujian tambahan diperlukan untuk memastikan model tidak membuat klaim keamanan yang tidak memiliki dasar.

## 4. Ringkasan Hasil Pengujian

Berdasarkan pengujian terhadap 10 pertanyaan atau skenario, diperoleh ringkasan berikut.

| Aspek pengujian | Hasil pengamatan |
|---|---|
| Pertanyaan yang sesuai dengan *knowledge base* | Sistem menghasilkan jawaban yang sesuai dengan informasi sumber. |
| Variasi formulasi pertanyaan | Sistem mampu menangani pertanyaan dengan susunan kalimat berbeda. |
| Pertanyaan di luar cakupan sumber | Model menyatakan bahwa informasi tidak tersedia. |
| Query dengan beberapa pertanyaan | Model mampu menjawab pertanyaan yang tercakup dalam sumber dan pertanyaan aritmetika sederhana. |
| Potensi halusinasi | Perlu pengujian terukur untuk memastikan jawaban tetap didukung sumber dan penolakan dilakukan secara tepat. |

*Catatan:* Ringkasan ini berdasarkan pengamatan terhadap jawaban yang dihasilkan. Pengujian ini belum mencakup pengukuran kuantitatif seperti Precision@K, Recall@K, MRR, atau evaluasi otomatis terhadap relevansi dokumen.

## 5. Kesimpulan

Berdasarkan hasil pengujian, sistem menunjukkan indikasi kinerja yang baik dalam menangani pertanyaan yang informasinya tersedia dalam *knowledge base*, termasuk pertanyaan dengan variasi formulasi dan query yang menggabungkan beberapa topik.

Sistem juga menunjukkan perilaku yang sesuai ketika menerima pertanyaan yang tidak tercakup dalam sumber, yaitu menyatakan bahwa informasi yang dibutuhkan tidak tersedia. Pada query yang menggabungkan pertanyaan tentang pengaturan ulang kata sandi, perhitungan aritmetika, dan perubahan nomor telepon, model mampu menjawab pertanyaan yang relevan dengan sumber sekaligus menjawab pertanyaan tambahan di luar cakupan *knowledge base*.

Meskipun demikian, evaluasi terhadap sistem perlu membedakan tiga komponen utama:

1. **Retrieval:** Kemampuan menemukan dokumen yang relevan dengan pertanyaan.
2. **Answer generation:** Kemampuan model menyusun jawaban yang benar dan sesuai dengan konteks yang ditemukan.
3. **Abstention:** Kemampuan model menolak memberikan jawaban ketika informasi yang diperlukan tidak tersedia atau tidak cukup didukung oleh sumber.

Dengan demikian, hasil pengujian ini memberikan indikasi awal bahwa sistem berfungsi dengan baik pada skenario yang diuji. Namun, diperlukan pengujian tambahan dengan data yang lebih beragam dan metrik evaluasi yang terukur sebelum menyimpulkan bahwa seluruh komponen sistem telah bekerja secara optimal.
