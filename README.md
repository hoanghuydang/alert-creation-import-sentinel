# alert-import-sentinel

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
  


