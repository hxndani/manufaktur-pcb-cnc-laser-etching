# Perkenalan 
ini adalah repository untuk dokumentasi proses pembuatan pcb menggunakan cnc untuk drilling, laser untuk membuat jalur, dan etching kimia untuk ekspos jalur yang terpakai dan tidak terpakai.

## persiapan
1. install semua software yang diperlukan
   1. flatcam untuk membuat cam titik pengeboran lubang mesin cnc desktop
   2. universal gcode sender yang dapat diinstall disini [UGS_Download](https://github.com/winder/Universal-G-Code-Sender/releases/tag/v2.1.17)
   3. ezcad2 beserta driver mesin laser untuk mengirim dan mengontrol perintah ke mesin laser 
2. persiapkan file pcb yang ingin dibuat, misalkan saya memiliki file seperti ini (file pcb output dari kicad)
![gambar file pcb gerber](contoh_file_gerber.png)
3. setelah file dipersiapkan, buka aplikasi flatcam dan klik tab File -> klik Open -> open Gerber -> pilih file gerber seperti board outline/garis bentuk pcb kita, jalur pcb, dan soldermask (optional).
   <img width="636" height="484" alt="image" src="https://github.com/user-attachments/assets/c0b2f14e-968d-4861-a99e-ba3f7c21f76a" />
   <img width="1576" height="649" alt="image" src="https://github.com/user-attachments/assets/a7fe8ee9-2b56-473f-939f-d2b431b9fec2" />

4. Setelah itu, klik tab File -> Open -> Open Excellon -> pilih semua file .DRL yang ingin kita lubangi
   <img width="838" height="489" alt="image" src="https://github.com/user-attachments/assets/da3da586-17a8-43d4-8c89-b5325d94ddc8" />
   <img width="1193" height="588" alt="image" src="https://github.com/user-attachments/assets/aa30bfae-96d1-44a2-9b6f-9531f541501d" />
   
5. Pilih semua file Gerber dan Excellon yang ada, lalu klik 'Move To Origin' untuk memindahkan semua file ke center/titik 0,0
   <img width="923" height="492" alt="image" src="https://github.com/user-attachments/assets/76cc0a96-1e50-4927-bad0-f74ecffcb300" /> 
   <img width="706" height="526" alt="image" src="https://github.com/user-attachments/assets/8fc9ae94-90ae-46da-a5a6-0f9accfd73cf" />

6. Jika file excellon kita ada lebih dari 1, kita gabungkan terlebih dahulu file-file excellon tersebut menjadi 1 file. Caranya klik semua file excellon yang ada -> lalu klik tab Edit -> Klik Join Objects -> Join Excellon(s). Maka akan muncul file baru bernama Combo_Excellon. Pilih semua file Excellon selain Combo_Excellon -> lalu klik kanan -> Disable Plot
   <img width="721" height="532" alt="image" src="https://github.com/user-attachments/assets/86533559-feb7-4c3c-9627-f9d711eb130c" />
   <img width="517" height="372" alt="image" src="https://github.com/user-attachments/assets/337a38ac-364d-478b-8356-663e9b6e0924" />
   
7. Lanjut, Klik kiri 2 kali file Combo_Excellon -> Excellon Editor. Di Excellon editor, kita bisa ubah semua diameter yang ada menjadi 1 tool, jika mata bor yang digunakan 0.9 mm, ganti semua diameter nya menjadi 0.9mm. Kalau mau lebih efisien, lubang yang untuk terminal plug atau lubang baut, tidak perlu diubah menjadi 0.9mm, langsung saja kita hapus terlebih dahulu. Caranya tinggal klik diameternya, lalu tekan delete pada keyboard.
<img width="1380" height="827" alt="image" src="https://github.com/user-attachments/assets/9441ca33-9c7a-440d-a2a4-07f36bf283b1" />

8. Selanjutnya, kita tambahkan diameter lubang tambahan di setiap ujung pcb sebagai penanda untuk memudahkan saat melaser jalur. Caranya, Klik '+ Add'. Jika saat ingin meletakkan tanda plus tidak pas dengan garis outline pcb, ubah ukuran gridnya (letaknya ada di bawah kanan) menjadi lebih kecil sesuai kebutuhan. Misal menjadi 0,001
   <img width="482" height="589" alt="image" src="https://github.com/user-attachments/assets/674473e5-d7c6-4e82-8fbe-6c37bf9d9105" />
   <img width="713" height="598" alt="image" src="https://github.com/user-attachments/assets/673cfd65-3257-438f-9dc9-2e50c6e12c1b" />
   <img width="126" height="68" alt="image" src="https://github.com/user-attachments/assets/a99840b4-14d6-41a6-862c-c249ecd4b870" />

9. Jika sudah menambahkan lubang di setiap ujung outline pcb, biasanya yang muncul defaultnya adalah diameter 1mm, kita ubah kembali menjadi 0.9 mm
   <img width="502" height="576" alt="image" src="https://github.com/user-attachments/assets/1fd1f0b8-85ce-4583-8ff1-c675bfb4e512" />

10. Jika terdapat Slots (lihat Kolom S) pada drill kita, klik diameter lubangnya sampai berwarna biru pada baris itu -> Convert Slots -> Setelah itu Klik Exit Editor -> Yes untuk save
    <img width="368" height="407" alt="image" src="https://github.com/user-attachments/assets/c95922aa-1805-463f-a46a-aa96b3951fd2" />

11. Klik file hasil Edit (Combo_Excellon_Edit) -> Klik Drilling Tool -> Ubah parameter nya seperti pada gambar di bawah -> Lalu, klik Generate CNC Objects -> Setelah itu Save CNC code -> beri nama file yang deskriptif, misal, 'drill_0.9mm_board_xyz.nc'
   <img width="335" height="855" alt="image" src="https://github.com/user-attachments/assets/913f965b-1e5c-499d-b1ff-0a8db9bed6fd" />
   <img width="1130" height="552" alt="image" src="https://github.com/user-attachments/assets/8d9fce8c-5289-41db-bf6a-5cabe013ae73" />

12. Lakukan langkah yang sama jika ingin membuat file gcode ukhusus melubangi terminal nya saja, atau lubang baut nya saja, kita tinggal edit file excellon nya.
13. Sebelum lanjut untuk melubangi pcb, kita edit dulu file gcode tadi di VSCode, dan ubah semua bagian naik (Axis-Z) setelah melubangi yang defaultnya 2 mm menjadi 5mm.
    blok bagian Z2.0000 -> tekan Ctrl + H -> ubah menjadi Z5.0000 -> klik Replace all -> lalu Save (Ctrl+S)
    <img width="1272" height="903" alt="image" src="https://github.com/user-attachments/assets/9fafb5da-c7c4-43cb-878d-7b3b886d99f4" />

14. Selanjutnya, buka UGS, dan letakkan pcb (bagian atas/top yang dilubangi), setting titik offset X,Y,dan Z. Open file gcode yang sudah dibuat, dan jalankan.
15. Setelah proses drilling pcb selesai. Pilok terlebih dahulu pcb dan keringkan.
16. Buka aplikasi FlatCAM kembali, dan pilih semua gerber -> klik tab Edit -> Join Objects -> Join Gerber(s)
   <img width="400" height="471" alt="image" src="https://github.com/user-attachments/assets/031bcba7-7646-4043-9ef7-e82a2e75c187" />
   <img width="676" height="410" alt="image" src="https://github.com/user-attachments/assets/6a8030e9-c1ef-4b07-8968-04b8de847899" />

17. Disable semua file gerber selain Combo_Gerber
    <img width="402" height="242" alt="image" src="https://github.com/user-attachments/assets/82b6b6fe-ece8-42c4-a5ef-c03b5aace1ad" />

18. Setelah itu klik file gerber nya, lalu klik tab Tool -> Invert Gerber tool -> Invert Gerbert
   <img width="687" height="732" alt="image" src="https://github.com/user-attachments/assets/01e4f47a-3e2e-4a69-82a5-816a86cbbbed" />
   
19. Disable file combo gerber sebelumnya
   <img width="516" height="475" alt="image" src="https://github.com/user-attachments/assets/d9ad355f-65fb-4123-8783-4812f2153183" />

20. Untuk sementara ini, karena software EZCAD2 nya agak anomali, dan belum diriset kembali, Jika jalur yang ingin kita laser adalah top layer, maka perlu diflip terlebih dahulu. Caranya klik file 'Combo_Gerber_Inverted' tadi -> klik Options -> Flip on X Axis. Jika yang ingin dilaser adalah bottom layer, maka tidak perlu diflip.
<img width="420" height="268" alt="image" src="https://github.com/user-attachments/assets/7bf0650d-454c-4cf0-aef1-b0447c64570d" />

21. Kemudian, Klik file 'Combo_Gerber_Inverted' kembali -> klik tab File -> Export -> Export SVG
    <img width="532" height="438" alt="image" src="https://github.com/user-attachments/assets/9508a661-e4fb-4ec9-82e3-2d19e5111195" />
 
22. Buka, aplikasi EZCAD2 (pastikan plug terlebih dahulu usb dari laser ke pc/laptop). Klik File -> Import File -> Import vector file -> pilih file svg yg ingin dilaser.
23. setelah muncul, 
