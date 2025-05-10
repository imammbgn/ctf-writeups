# No Country for Old Keys

## Solution
![image](https://hackmd.io/_uploads/Bk3utY2lxg.png)
This challenge asks us to find the API key of a person named Anthony McConnolly, maybe leak API key?

![image](https://hackmd.io/_uploads/S10yct2lgg.png)
first I immediately looked for the github account related to this man name, because API keys usually leak in their github repos, i got `@antmcconn` user.


![image](https://hackmd.io/_uploads/SyesqFhggg.png)
I looked at the repo here ai-web-browser and there is a file with the code line `#define API_KEY “YOUR_API_KEY_HERE”`.

![image](https://hackmd.io/_uploads/rkE3cY2egx.png)

I checked the blame of this file and found that the API key was deleted by Anthony. 
![image](https://hackmd.io/_uploads/H18CcKnege.png)
![image](https://hackmd.io/_uploads/SkC0qFhggg.png)

Flag : CIT{ap9gt04qtxcqfin9}