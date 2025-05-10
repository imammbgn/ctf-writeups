# Breaking Authentication - Web

## Solution

inject simple sqli payload `' OR 1=1 - --` in the username input on the login page.

![Screenshot_3](img1.png)

error response indicates that the username parameter is vulnerable to sqli.

![Screenshot_18](img2.png)

dump databases using sqlmap, then found flag.

![Screenshot_1](img3.png)

Flag : CIT{36b0efdc2ec7132}

