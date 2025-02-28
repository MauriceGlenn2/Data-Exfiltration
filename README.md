# (WIP) Data Exfiltration from PIP'd Employee

## Scenario: 
- An employee named John Doe, working in a sensitive department, recently got put on a performance improvement plan (PIP). After John threw a fit, management has raised concerns that John may be planning to steal proprietary information and then quit the company. Your task is to investigate John's activities on his corporate device (EDR-Test-Vm) using Microsoft Defender for Endpoint (MDE) and ensure nothing suspicious is taking place.
- John is an administrator on his device and is not limited on which applications he uses. He may try to archive/compress sensitive information and send it to a private drive or something.

# Data Colection
 I did a search within MDE DeviceFileEvents for any activites for any activies with zip files, and found a lot of regular activity oof achving stuff and moving to a "backup" folder
 ![image](https://github.com/user-attachments/assets/20ce2fa1-1566-43d2-b13f-0818365a92ef)

 I took one of the instances of a zip file being created, took the timestamp and searched under DeviceProcessEvents for anything happening 2 minutes before the archive was created and 2 minutes after. I discovered around the same time, a powershell script silently installed 7zip and then used 7zip to zip up employee data into an archive. 
![image](https://github.com/user-attachments/assets/a6cfe590-77ec-49be-8be6-4cc9cb1d1cdd)

