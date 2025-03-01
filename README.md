# Data Exfiltration from PIP'd Employee

## **Scenario**
- An employee named **John Doe**, working in a sensitive department, was recently placed on a **Performance Improvement Plan (PIP)**. After reacting negatively, management raised concerns that John might attempt to **steal proprietary information** before quitting the company.  
- Your task is to **investigate John's activities** on his corporate device (**EDR-Test-Vm**) using **Microsoft Defender for Endpoint (MDE)** and ensure no suspicious activity is taking place.  
- John is an **administrator** on his device and has no restrictions on the applications he uses. He may attempt to **archive/compress sensitive data** and **send it to a private drive or another location**.

---

## **Data Collection**
I performed a search within **MDE DeviceFileEvents** for any activity involving **ZIP files** and found a lot of regular **archiving activity** and **file movements to a "backup" folder**.

![image](https://github.com/user-attachments/assets/20ce2fa1-1566-43d2-b13f-0818365a92ef)

I selected an instance of a **ZIP file creation**, noted the timestamp, and searched **DeviceProcessEvents** for related activities **two minutes before and after** the archive was created.  
- I discovered that a **PowerShell script silently installed 7-Zip** and then **used 7-Zip to archive employee data**.  

![image](https://github.com/user-attachments/assets/a6cfe590-77ec-49be-8be6-4cc9cb1d1cdd)

I then searched for any signs of **data exfiltration via the network**, but **no logs indicated any outbound transfers**.

<img width="443" alt="image" src="https://github.com/user-attachments/assets/a7a8de92-edb1-481d-9a9d-eefabba457f5" />

---

## **Response**
- Immediately isolated the system upon discovering the archiving activities
- I relayed all findings to **John's manager**, including details about the **PowerShell-automated archiving process** occurring at **regular intervals**.  
- There was **no evidence of data exfiltration**, but I am standing by for further instructions from management.  

---

## **MITRE ATT&CK Framework TTPs**

### **T1560.001 - Archive via Utility** (Data Collection)  
- The use of **7-Zip** to compress files, particularly sensitive employee data, suggests an attempt to **stage data for exfiltration**.

### **T1059.001 - PowerShell** (Execution)  
- PowerShell was used to **silently install 7-Zip** and execute the compression process. This is a common method for **automating malicious activities**.

### **T1074.001 - Local Data Staging** (Collection)  
- The data was being **archived in a "backup" folder**, likely to **prepare for potential exfiltration**.

### **T1070.004 - File Deletion** (Defense Evasion)  
- While not explicitly mentioned, if the PowerShell script **cleaned up evidence** (e.g., **deleting installation artifacts**), this could indicate an **attempt to evade detection**.

### **T1027 - Obfuscated Files or Information** (Defense Evasion)  
- Archiving is sometimes used to **obfuscate** sensitive data before exfiltration, making it harder for security tools to detect unauthorized file transfers.

### **T1105 - Ingress Tool Transfer** (Command and Control)  
- The **silent installation of 7-Zip via PowerShell** may be considered an instance of **downloading and using external tools** for unauthorized actions.

