# Annie THM
*** 
Difficulty - Medium

## Description
    

<p align="center">
  <img src="https://github.com/Uz169/F.NS357-Machines-writeup/blob/main/Annie%20THM%20(medium)%20/files/1.png">
</p>

<p align="center">
  <img src="https://github.com/Uz169/F.NS357-Machines-writeup/blob/main/Annie%20THM%20(medium)%20/files/2.png">
</p>

## Solve

1. gobuster , nmap tool ашиглан мэдээлэл цуглуулна.

2. nmap дээр зөвхөн 80,22р портууд ажиллаж байсан бол gobuster дээр /panel , /uploads гэд>
хэсэг гарж ирсэн нь сонирхол татсан

3. panel дотор file upload хэсэг байсан тул Insecure File upload exploit хийх боломжтой
гэж үзсэн.

4.PentestMonkey.php reverse shell файлыг upload хийх гэхэд "PHP não é permitido!"
гэсэн error зааж байсан тул .php файл оруулахыг хориглосон байсан үүнийг нь тойрч гарахын
тулд
``mv pentestmonkey.php pentestmonkey.phtml гэж оруулахад файл хүлээн авч байсан`` 

5.хортой код маань upload хийгдэж байсан тул эхлээд listener асаагаад дараа нь /uploads
дотроос ажлуулахад reverse shell авсан.

6.Нэвтэрч орсоны дараа suid file хайхад 
``find / -perm /4000 2>/dev/`` 

/usr/bin/python олдсон ба GTFOBins сайтаас хайхад файл унших command олсон
``python -c 'print(open("/root/root.txt").read())'``

``root.txt - THM{pr1v1l3g3_3sc4l4t10n}``
