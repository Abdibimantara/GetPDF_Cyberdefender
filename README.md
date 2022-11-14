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















