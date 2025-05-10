# No Country for Old Keys

## Solution
![image](img.png)
This challenge asks us to find the API key of a person named Anthony McConnolly, maybe leak API key?

![image](img1.png)
first I immediately looked for the github account related to this man name, because API keys usually leak in their github repos, i got `@antmcconn` user.


![image](img2.png)
I looked at the repo here ai-web-browser and there is a file with the code line `#define API_KEY “YOUR_API_KEY_HERE”`.

![image](img3.png)

I checked the blame of this file and found that the API key was deleted by Anthony. 
![image](img4.png)
![image](img5.png)

Flag : CIT{ap9gt04qtxcqfin9}
