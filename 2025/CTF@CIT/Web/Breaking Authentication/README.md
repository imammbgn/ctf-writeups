# Breaking Authentication - Web

## Solution

inject simple sqli payload `' OR 1=1 - --` in the username input on the login page.
![Screenshot_3](https://hackmd.io/_uploads/r1g5nhd2gxx.png)

error response indicates that the username parameter is vulnerable to sqli.
![Screenshot_18](https://hackmd.io/_uploads/Hy923O2xeg.png)

dump databases using sqlmap, then found flag.
![Screenshot_1](https://hackmd.io/_uploads/Sy9hhu3ege.png)

Flag : CIT{36b0efdc2ec7132}

