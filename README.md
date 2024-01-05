# alert-creation-import-sentinel

***LAB Precursor:***

It is important to highlight that during the initial deployment, all resources were exposed directly to the internet. 
Specifically, the Virtual Machines had their Network Security Groups and built-in firewalls configured in an open manner, allowing broad access. 


***High-Level Overview:***

In this lab, we will be exploring these two sections:
+ ***Part 1: Manual Alert Creation***
  + Manually create the “TEST: Brute Force ATTEMPT - Windows” Rule
  + SecurityEvent  
    | where EventID == 4625  
    | where TimeGenerated > ago(60m)  
    | summarize FailureCount = count() by AttackerIP = IpAddress, EventID, Activity, DestinationHostName = Computer  
    | where FailureCount >= 10
  + Try to trigger it  
  + Delete the Rule when finished  
+ ***Part 2: Automatic Alert Import***
  + Select the “CUSTOM: Brute Force SUCCESS - Windows” Alerts and “Edit” it
  + Copy the query and fully inspect/dissect it in Log Analytics Workspace
  + Do for the remaining “CUSTOM” Analytics Rules
 


***INTO THE LAB***

***Part 1: Manual Alert Creation***

***Step 1: Query the script in Log Analytics Workspaces***

![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/c3a349f7-5d58-4e37-a8ec-13a548fc903e)

Removed last two lines of query:

![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/625ad4a4-ad22-4098-b92f-a37e70ef5031)

The initial script did not retrieve any results, so work backward from the query and realize why it is not returning results.

Expand the results to investigate:

![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/ea9f6872-2960-4990-8175-2853d515c989)

***Step 2: Create a rule in Microsoft Sentinel***

![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/5ec1e7c7-6e9e-4554-af35-216a16b7eacb)
![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/26eb7c9e-fb5c-4b99-84d2-dffeede83dba)
![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/1f999077-39a1-4dda-89ae-5c1ee8e18e8d)
![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/3252759d-3ecd-4c53-8b11-4327f558a02b)

Add a new Entity:

![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/edef2dc9-fd6c-4b60-a1d6-95a9cc189446)

Choose how often to run the query:

![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/f20aaa98-c402-4708-a804-8923f85e1014)
![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/ec422641-42dc-4163-a2ca-ca978392f6a5)
![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/271d89c4-ac27-4893-a533-f19ebc8660e7)

Now we have our analytics rule.

![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/f3bcb34f-52c4-4227-8e30-46ed5c21c587)


***Step 3: Attempt to login into 'windows-vm' at least 10 times***

![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/e9b4ab4d-3442-45d9-bd9f-2046d757c0b1)

Fail to login at least 10 times to trigger the alert.

***Step 4: Query the script in Log Analytics Again - to see if alert was triggered***

![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/2d6f5844-9136-4ecf-8f2c-880cec5a3b31)

SUCCESS it was triggered.

***Step 4: Check Microsoft Sentinel to see if an incident appeared***

![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/a8e24019-a4fd-439a-95bf-5811f6229916)

We can investigate the Brute Force attempt:

![image](https://github.com/hoanghuydang/alert-creation-import-sentinel/assets/127445164/6ea3777e-7f15-4005-af12-5f1d342de16b)

Entity Mapping comes into play (image above) - You can see the incident that got triggered, the computer (destination host), and the attacker that triggered the incident from the host

***Step 5: Clean up the and Delete the Rule***



