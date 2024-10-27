# Logging-and-Monitoring

![Architecture Diagram](https://imgur.com/Qa1MyLI.png)
### Geo IP Data Ingestion + Log Analytics and Microsoft Sentinel (SIEM) Setup
> [!NOTE]
> In this lab, we will set up a Log Analytics Workspace to serve as our central log repository. Next, we’ll configure Azure Sentinel as our SIEM and link it to the Log Analytics Workspace. We’ll also create a Sentinel watchlist, which will include geo data to plot attacker IP addresses on a map in a future lab.




<details>
<summary>Part 1: Put Large Geo-Data Files in Azure Storage.</summary>

1. Create a Log Analytics Workspace (our log aggregator) named: LAW-Cyber-Lab-0x.<br>
    a. Search “Log Analytics workspaces” → select create log analytics workspace.
    ![Azure image](https://imgur.com/ZG2ErXM.png)
    b. Put it in the EG-Cyber-Lab Resource Group & name it LAW-Cyber-Lab-0x & keep the region the same (East US 2) → select “review + create”.
    ![Azure image](https://imgur.com/kF38dVg.png)
2. Setup Sentinel and connect it to our Log Analytics Workspace.<br>
    a. Search for “sentinel” → select create Microsoft Sentinel.
    ![Azure image](https://imgur.com/TaAjWMt.png)
    b. Select LAW-Cyber-Lab-01 → select add.
    ![Azure image](https://imgur.com/6grwBGN.png)
</details>

<details>
<summary>Part 2: Create the geoip watchlist.</summary>

1. Download this [geoip-summarized.csv file](https://github.com/joshmadakor1/Cyber-Course-v2/blob/main/Sentinel-Maps(JSON)/geoip-summarized.csv) onto your PC.<br>
    a. Go to watchlist in the workspace of ‘LAW-Cyber-Lab-0x’ & click ‘+ new’ to create a new Watchlist.<br>
    ![Azure image](https://imgur.com/JLZ02ar.png)
    b. Name/Alias should be: geoip → select next.<br>
    ![Azure image](https://imgur.com/fTLAKVZ.png)
    c. Source type: Local File, then click ‘browse for files’ & select what we downloaded earlier.<br>
    ![Azure image](https://imgur.com/q5ecjpK.png)
    ![Azure image](https://imgur.com/j5nywDg.png)
    d. The File Preview should show up → SearchKey should be Network → select ‘review + create’.<br>
    ![Azure image](https://imgur.com/RMt8skB.png)
    e. Go back to watchlist to see the one we just created.<br>
    ![Azure image](https://imgur.com/EzAvOh5.png)
    f. Allow time for these files to “upload”/load from your storage account into Sentinel/Log Analytics Workspace. There are about 27k /55k rows/records.<br>
</details>

<details>
<summary>Part 3: Query and observe logs in the Log Analytics Workspace.</summary>

1. Go to Log Analytics Workspace & click on ‘LAW-Cyber-Lab-0x’.<br>
![Azure image](https://imgur.com/9dkslq8.png)
2. Click on logs and paste the query ‘_GetWatchlist("geoip")’ then click Run.<br>
![Azure image](https://imgur.com/8IDPIDY.png)
3. Typing ‘| count’ will return the total number of records there are.<br>
![Azure image](https://imgur.com/BRDoI8l.png)
</details>



![Architecture Diagram](https://imgur.com/Qa1MyLI.png)
### Enabling Microsoft Defender for Cloud
> [!NOTE]
> In this lab, we will enable Microsoft Defender for Cloud for our Log Analytics Workspace, subscription, Cloud Continuous Export, and resources. Microsoft Defender for Cloud provides insights into our security posture and ongoing activities. Its primary function here is to collect logs from the virtual machine and forward them to the Log Analytics Workspace.



<details>
<summary>Part 1: Enable Microsoft Defender for Cloud for Log Analytics Workspace.</summary>

</details>

<details>
<summary>Part 2: Enable Defender Plans for VMs and SQL Instances on VMs.</summary>

</details>

<details>
<summary>Part 3: Enable Data Collection (All Events).</summary>

</details>

<details>
<summary>Part 4: Enable Microsoft Defender for Cloud for Subscription.</summary>

</details>

<details>
<summary>Part 5: Enable Microsoft Defender for Cloud Continuous Export in Environment Settings.</summary>

</details>

<details>
<summary>Part 6: Ensure MDC didn’t create another Log Analytics Workspace, delete it if so.</summary>

</details>


![Architecture Diagram](https://imgur.com/Qa1MyLI.png)
### Enable Log Collection for VMs and Network Security Groups
> [!NOTE]
> In this lab, we will complete the configuration to enable logs from the virtual machine and Network Security Groups (NSGs) to be sent to our Log Analytics Workspace. By the end, we will query the Log Analytics Workspace for logs from the Linux VM, Windows VM, and NSGs, ensuring all logs are properly routed to our central repository.



<details>
<summary>Part 1: Create an Azure Storage Account (name needs to be unique).</summary>

</details>

<details>
<summary>Part 2: Enable Flow logs for both both Network Security Groups (NSGs).</summary>

</details>

<details>
<summary>Part 3: Configure Data Collection Rules within our Log Analytics Workspace.</summary>

</details>

<details>
<summary>Part 4: Manually install the Log Analytics Agent on both the windows-vm and linux-vm.</summary>

</details>




![Architecture Diagram](https://imgur.com/Qa1MyLI.png)
### Tenant Level Logging
> [!NOTE]
> In this lab, we will continue ingesting logs into our central log repository, the Log Analytics Workspace. This time, we will import audit and sign-in logs from Microsoft Entra ID (formerly Azure Active Directory).



<details>
<summary>Part 1: Setup logging for Microsoft Entra ID and generate some logs.</summary>

</details>

<details>
<summary>Part 2: Create Diagnostic Settings to ingest Microsoft Entra ID Logs.</summary>

</details>

<details>
<summary>Part 3: Create a dummy user, username “dummy_user”.</summary>

</details>

<details>
<summary>Part 4: Observe Audit Logs logs in Log Analytics Workspace.</summary>

</details>

<details>
<summary>Part 5: Simulate Brute Force Attack against Microsoft Entra ID.</summary>

</details>

<details>
<summary>Part 6: Observe SigninLogs in Log Analytics Workspace.</summary>

</details>



![Architecture Diagram](https://imgur.com/Qa1MyLI.png)
### Subscription Level Logging (Activity Log)
> [!NOTE]
> In this lab, we will ingest subscription-level logs, specifically the Activity Log. We’ll create, modify, and delete resource groups to later query these actions in our Log Analytics Workspace.



<details>
<summary>Part 1: Export Azure Activity Logs to Log Analytics Workspace.</summary>

</details>

<details>
<summary>Part 2: Create a new Resource Group named “Scratch-Resource-Group”.</summary>

</details>

<details>
<summary>Part 3: Create another new Resource Group named “Critical-Infrastructure-Wastewater”.</summary>
(DO NOT ACCIDENTALLY DELETE YOUR LAB RESOURCE GROUP)

</details>

<details>
<summary>Part 4: Delete both “Scratch-Resource-Group” and “Critical-Infrastructure”.</summary>

</details>

<details>
<summary>Part 5: Generate some logs.</summary>

</details>



![Architecture Diagram](https://imgur.com/Qa1MyLI.png)
### Resource Level Logging Diagnostic Settings)
> [!NOTE]
> This is the final lab focused on ingesting logs into our Log Analytics Workspace. In this lab, we’ll complete the process by enabling logging for our storage account and forwarding the logs to the Log Analytics Workspace. Next, we’ll create a Key Vault, an enterprise password manager, enable logging for it, and send those logs to the workspace as well. Finally, we’ll query the logs to verify the setup.



<details>
<summary>Part 1: Configure Logging for Azure Storage.</summary>

</details>

<details>
<summary>Part 2: Create a Key Vault Instance.</summary>

</details>

<details>
<summary>Part 3: Configure logging for your Key Vault instance by enabling diagnostic settings.</summary>

</details>

<details>
<summary>Part 4: Add a secret to Key Vault called “Tenant-Global-Admin-Password” with a made up password.</summary>

</details>

<details>
<summary>Part 5: Add another secret to Key Vault called “Super-Secret-Password-1” with a made up password.</summary>

</details>

<details>
<summary>Part 6: Observe the key in key vault.</summary>

</details>

<details>
<summary>Part 7: Collect the audit log and send to your Log Analytics Workspace instance.</summary>

</details>

<details>
<summary>Part 8: Generate some Logs for Azure Storage (read some blobs/files).</summary>

</details>
