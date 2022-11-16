# GetPDF_Cyberdefender

Repositori ini sengaja saya tulis untuk mendokumentasikan bagaimana saya dalam menganalisa suatu insiden mengenai malware. Insiden tersebut berupa challenge yang dibuat oleh "The Honeynet Project" dalam platform cyberdefenders. Challenge tersebut berjudul "GetPDF" yang dapat diakses pada link berikut : https://cyberdefenders.org/blueteam-ctf-challenges/47 . 

Berikut adalah spesifikasi dari challenge tersebut : 
- SHA1SUM : 81b99e0094edde5de6cec7d9f5cd391d9eca3eb2
- Published : 12 Februari 2022
- Author : The Honeynet Project
- Tags	: PDF, MACRO, CVE, EXPLOIT
- Size : 30 KB 
- Number of questions : 11 Questions

# Skenario Challenge
PDF format is the de-facto standard in exchanging documents online. Such popularity, however, has also attracted cyber criminals in spreading malware to unsuspecting users. The ability to generate malicious pdf files to distribute malware is a functionality that has been built into many exploit kits. As users are less cautious about opening PDF files, the malicious PDF file has become quite a successful attack vector. The network traffic is captured in lala.pcap contains network traffic related to a typical malicious PDF file attack, in which an unsuspecting user opens a compromised web page, which redirects the user’s web browser to a URL of a malicious PDF file. As the PDF plug-in of the browser opens the PDF, the unpatched version of Adobe Acrobat Reader is exploited and, as a result, downloads and silently installs malware on the user’s machine.

# Tools yang digunakan 
- Wireshark
- LinuX OS (Kali Linux)
- Pdfparser
- peepdf
- 

# Tampilan data wireshark yang telah didownload 
Berdasarkan Aturan yang dijelaskan pada website cyberdefender, kita diharuskan untuk mendownload data berkektensi file .pcap yang mana data tersebut akan menjadi bahan analisa kita dalam menyelesaikan challenge tersebut. 
![image](https://user-images.githubusercontent.com/43168046/201688005-76af4b00-6871-49ac-8793-219a6c32c4c7.png)


# Questions 
<p>1. How many URL path(s) are involved in this incident? </p>
<p> answer : 6 </p>
<p> Analisis : Kami memulai dengan membukan data pcap yang telah didownload sebelumnya. Disni kami langsung melakukan filter data menggunakan menu Eksport Object. Lalu kami fokus pada content type yang berisikan text/html. kami menemukan sebanyak 6 url path yang menjadi destination dari korban. URL path tersebut terdiri dari :

<ul>blog.honeynet.org.my/forensic_challenge</ul>
<ul>blog.honeynet.org.my/forensic_challenge/</ul>
<ul>blog.honeynet.org.my/getpdf.php</ul>
<ul>blog.honeynet.org.my/favicon.ico</ul>
<ul>blog.honeynet.org.my/the_real_malware.exe</ul>
<ul>blog.honeynet.org.my/favicon.ico</ul>
</p>

![image](https://user-images.githubusercontent.com/43168046/201691756-15ddff2a-ae4b-454f-96bd-b00b4396d5e4.png)

<p>2. What is the URL which contains the JS code? </p>
<p> answer : http://blog.honeynet.org.my/forensic_challenge/ </p>
<p> Analisis :  Melalui tool wireshark, kami kembali melakukan beberapa analisis terkait req url yang dibuat oleh korban. Dari 6 url path tersebut kami mendapati bahwa korban melakukan req ke url path blog.honeynet.org.my/forensic_challenge/ dan mendapatkan response code 200 (ok). sehingga destinatisi tujuan pun menampilkan hasinya, dimana hasilnya berisi javasripct code yang ditandai dengan penggunaan tag <script>. </p>

![image](https://user-images.githubusercontent.com/43168046/201835339-0fb5495c-32ed-408b-90c2-98ba3e080f1d.png)
  
<p>3. What is the URL hidden in the JS code? </p>
<p> answer : http://blog.honeynet.org.my/forensic_challenge/getpdf.php </p>
<p> Analisis :  Melalui tool wireshark, kami kembali melakukan beberapa analisis terkait req url yang dibuat oleh korban. Kami kembali menanalisi url sebelumnya yaitu "blog.honeynet.org.my/forensic_challenge/". Namun saat kami melakukan analyisi follow http stream, kami sedikit kebingan, lalu kami mengubah menjadi follow tcp stream, dan benar saja kami menemukan adanya indikasi url ynag terhubung dalam javascript code tersebut yang mengarah ke http://blog.honeynet.org.my/forensic_challenge/getpdf.php . </p>  

![image](https://user-images.githubusercontent.com/43168046/201840354-560c764e-1291-49de-a032-e32a72c84c97.png)

<p>4. What is the MD5 hash of the PDF file contained in the packet? </p>
<p> answer : 659cf4c6baa87b082227540047538c2a </p>
<p> Analisis : Menggunakan menu filter yang tersedia pada wireshark, kami mencoba mengskpor object yang telah didownload sebelumnya oleh korban yang mengarah ke url  /forensic_challenge/fcexploit.pdf. Diketahui bahwa korban mendownload file berukuran 25 KB. Namun saat kami melakukan pengecekkan menggunakan file menggunakan linux, ouput ynag dikeluarkan bukanlah pdf, namun data, tentu saja ini mencurigakan. Menggunakan commandline yang tersedia di linux, kami berhasil mendapati hash md5 dari file tersebut. Kami memvalidasi menggunakan virus totals, dan benar file tersebut adalah malware.  </p> 

![image](https://user-images.githubusercontent.com/43168046/201845989-03020149-748a-42b3-8858-457c491551e2.png)

<p>5. How many object(s) are contained inside the PDF file? </p>
<p> answer : 19 </p>
<p> Analisis : Untuk mengetahui jumlah object yang terdapat dari file pdf tersebut, kami menggunakan bantuaan dari tools pdfid. melalui pdfid, kami dapat mengetahui struturk penyusund dari pdf tersebut. Jumlah object yang terdapat pada file tersebut adalah 19 </p> 

![image](https://user-images.githubusercontent.com/43168046/201888181-4daffa18-3538-4b86-9854-4f31456306cc.png)

<p>6. How many filtering schemes are used for the object streams? </p>
<p> answer : 4 </p>
<p> Analisis : Untuk mengetahui beberapa filter yang digunakan pada object dalam file tersebut, kami menggunakan tools pdf-parser. Melalui tools pdf-parser kami kembali mengcek satu persatu dari semua object yang berjumlah 19. kami mengcek satu persatu, terdapat 4 pobject yang menggunakan filter, yaitu object 5, 7, 9, 10   </p>

![image](https://user-images.githubusercontent.com/43168046/201928338-1217b51f-b9b4-4631-a693-3a93a1cefdb1.png)

<p>7. What is the number of the 'object stream' that might contain malicious JS code? </p>
<p> answer : 5 </p>
<p> Analisis : Disini kami kembali menggunakan tools peepdf. Melalui tools tersebut kami mendapati bahwa object nomer 5 teridindikasi sebagai malicious dengan ditandai masuk dalam kategori "object with js code". Kami pun mengekstrak object tersebut menggunakan peepdf, namun kami hanya menemukan sebuah script JS Code yang sulit dibaca. Menggunakan tools pdfstreamdumspter kami mencoba melakukan ekstraksi object tersebut, dan benar sajar kami mendapati bahwa object tersebut sebagai Exploit CVE-2009-1492 Date:5.12.09 v9.1 </p>

![image](https://user-images.githubusercontent.com/43168046/201973441-ea2ed9da-6f6b-4f0e-a81f-e5e2e70c5c77.png)

<p>8. Analyzing the PDF file. What 'object-streams' contain the JS code responsible for executing the shellcodes? The JS code is divided into two streams. Format: two numbers separated with ','. Put the numbers in ascending order </p>
<p> answer : 7,9 </p>
<p> Analisis : Setelah mengerahui bahwa terdapat 5 object yang masuk dalam kategori "stream". Kami mencoba mencoba menganalisa setiap object tersebut, dan tepatnya pada object 5 kami mendapatkan hasil yang cukup memuaskan. Kami mendapat pentujuk unutk mempermudah kami dalam menganalisa setiap onjectnya. setelah kami mengikuti pentujuk yang didaptakan, kami mengidentifikasi bahwa object 7 dan 9 memuat suatu code java scirpt yang kemungkinan bertugas menjalankan shellcode. </p>

![image](https://user-images.githubusercontent.com/43168046/202206141-14979e83-729b-4f22-be81-9f1d71b70b45.png)

<p>9. Analyzing the PDF file. What 'object-streams' contain the JS code responsible for executing the shellcodes? The JS code is divided into two streams. Format: two numbers separated with ','. Put the numbers in ascending order </p>
<p> answer : c:\WINDOWS\system32\a.exe </p>
<p> Analisis : setelah berhasil mendapat malicious jscode yang terdapat pada object 7 dan 9, kami melanjutkan menganalisa mengenai adanya kemungkinan shell code yang terdapat pada malicious object tersebut. Kami sepertinya menemukan shell code yang kami cari pada malicious jscode tersebut. kami pun membukanya dengan bantuan peepdf dan melakukan unescape yang berfungsi untuk menyaring kode atau varibel ynag tersembunyi tersebut. Setelah itu kami berhasil mendapatkan raw shellcodenya, lalu kami menjalankan shllecode tersebut menggunakan function sctest. dan kami mendapatkan informasi yang kami inginkan.  </p>

![image](https://user-images.githubusercontent.com/43168046/202204608-f7be6022-677a-43d6-bf39-7e20c7c92f3b.png)

<p>10. The PDF file contains another exploit related to CVE-2010-0188. What is the URL of the malicious executable that the shellcode associated with this exploit drop? </p>
<p> answer : http://blog.honeynet.org.my/forensic_challenge/the_real_malware.exe </p>
<p> Analisis : Untuk mengeahui another exploit yang terdapat selain dalam pdf tersebut, kami kembali menganalisi traffic pcap. Disini kami menemukan req url yang buat oleh korban dan megarah ke url path /forensic_challenge/the_real_malware.exe </p>

![image](https://user-images.githubusercontent.com/43168046/202209013-9e57eafa-dad2-404c-9d8b-ce74380fb7e3.png)




 



 





















