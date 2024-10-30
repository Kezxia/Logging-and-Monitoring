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

1. Go to Log Analytics Workspace → select ‘LAW-Cyber-Lab-0x’.<br>
![Azure image](https://imgur.com/9dkslq8.png)
2. Click on logs and paste the query ‘_GetWatchlist("geoip")’ → select Run.<br>
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

1. Enable Defender Plans for VMs and SQL Instances on VMs.<br>
Search Microsoft Defender and click on ‘Environment settings → select the toggle & click on the three dots to edit settings for ‘LAW-Cyber-Lab-0x’.
![Azure image](https://imgur.com/IBYBBLy.png)

</details>

<details>
<summary>Part 2: Enable Defender Plans for VMs and SQL Instances on VMs.</summary>

1. Turn ‘Servers’ & ‘SQL servers on machines’ ON → select ‘save’.<br>
![Azure image](https://imgur.com/Vv21I7L.png)
</details>

<details>
<summary>Part 3: Enable Data Collection (All Events).</summary>

1. Select ‘Data Collection’ & ‘All Events’ → select ‘save’.<br>
![Azure image](https://imgur.com/aZJLqCZ.png)
2. Go back to Microsoft Defender, refresh & you will see it now has 2 plans.<br>
![Azure image](https://imgur.com/zdtQQ53.png)
</details>

<details>
<summary>Part 4: Enable Microsoft Defender for Cloud for Subscription.</summary>

1. Search Microsoft Defender and click on ‘Environment settings’ → click down on the toggle & click on the three dots to edit settings for ‘Azure subscription 1’.
![Azure image](https://imgur.com/c89rJYA.png)
2. Turn status ON for Servers, Databases, Storage, & Key Vault.<br>
![Azure image](https://imgur.com/6nr1oQS.png)
3. Got to settings under Servers.<br>
![Azure image](https://imgur.com/YALcrZ7.png)
4. Select Edit configuration.<br>
![Azure image](https://imgur.com/2iruTO7.png)
5. Change workspace to custom → select ‘LAW-Cyber-Lab-0x’, Apply → select continue & save.<br>
![Azure image](https://imgur.com/t9oOVnM.png)
![Azure image](https://imgur.com/gxpthlW.png)
</details>

<details>
<summary>Part 5: Enable Microsoft Defender for Cloud Continuous Export in Environment Settings.</summary>

1. Go to ‘Continuous export’ → select ‘Log Analytics workspace’ → select all the Exported data types.<br>
![Azure image](https://imgur.com/1FK2HIB.png)
3. Select RG-Cyber-Lab for the resource group & ‘LAW-Cyber-Lab-0x’ for the workspace → select save.<br>
![Azure image](https://imgur.com/IzuL9wD.png)
</details>

<details>
<summary>Part 6: Ensure MDC didn’t create another Log Analytics Workspace, delete it if so.</summary>

1. Go to Log Analytics workspaces and delete the Default workspace that was created.<br>
![Azure image](https://imgur.com/afkAlBI.png)
![Azure image](https://imgur.com/fS4CfSq.png)
</details>


![Architecture Diagram](https://imgur.com/Qa1MyLI.png)
### Enable Log Collection for VMs and Network Security Groups
> [!NOTE]
> In this lab, we will complete the configuration to enable logs from the virtual machine and Network Security Groups (NSGs) to be sent to our Log Analytics Workspace. By the end, we will query the Log Analytics Workspace for logs from the Linux VM, Windows VM, and NSGs, ensuring all logs are properly routed to our central repository.



<details>
<summary>Part 1: Create an Azure Storage Account (sacyberlab2x, name needs to be unique).</summary>

1. Search for Storage account and create a new one.<br>
![Azure image](https://imgur.com/ziv3uAd.png)
2. Put it in our RG-Cyber-Lab group → Name it sacyberlab2x → Keep it in the same region as our vms (East US 2) → Keep everything else the same → Review and Create it.<br>
![Azure image](https://imgur.com/nyhuo9z.png)
</details>

<details>
<summary>Part 2: Enable Flow logs for both both Network Security Groups (NSGs).</summary>

1. Search NSG and click the windows-vm-nsg → select NSG flow logs → select Create a NSG Flow Log.<br>
![Azure image](https://imgur.com/CfqPOoj.png)
2. Click Select target resource and select Network Security Group → select both the windows and linux vm → confirm selection.<br>
![Azure image](https://imgur.com/ul17vSq.png)
![Azure image](https://imgur.com/bdw9PLk.png)
3. Storage account should be sacyberlab2x and retention days 0 → select Next: Analytics.<br>
![Azure image](https://imgur.com/g6AHkEB.png)
4. Select Version 2 → enable traffic analytics → set to every 10 minutes → make sure the Log analytics workspace is correct → review+create.<br>
![Azure image](https://imgur.com/uNlzhni.png)
</details>

<details>
<summary>Part 3: Configure Data Collection Rules within our Log Analytics Workspace.</summary>

1. Configure Linux Data Sources (auth only).<br>
   a. Search for Log Analytics Workspace → select LAW-Cyber-Lab-0x → select Agents and Data Collection Rules → Create.<br>
   ![Azure image](https://imgur.com/WtagJYM.png)
   ![Azure image](https://imgur.com/vBNu72t.png)
   b. Name the Rule: dcr-all-vms (data collection rule) → make sure the region is East US 2 (the same as the vms) → select all for platform type → select        Next: Resources.<br>
   ![Azure image](https://imgur.com/fH1Lwyu.png)
   c. Select Add resources → expand RG-Cyber-Lab resource group → select both vms → select apply → Next: Collect and deliver.<br>
   ![Azure image](https://imgur.com/OiNzxBB.png)
   ![Azure image](https://imgur.com/50RLI6C.png)
   d. Select Add data source → select Linux Syslog as data source type → select LOG_AUTH → leave the minimum log level at LOG_DEBUG → select none for the        rest of the facilities → select Next: Destination → select Add destination → select Destination Type as Azure Monitor Logs and Destination Details as your    Logs Analytics Workspace → Select Add data source.<br>
   ![Azure image](https://imgur.com/oN0ZfYV.png)
   ![Azure image](https://imgur.com/fqiQDLM.png)
3. Configure Windows Data Sources (Application (information only), Security (All)).<br>
   a. We will add another data source for the windows event logs → select the Information log for Application → select Audit success & Audit failure for         Security → select Next: Destination → select Destination Type as Azure Monitor Logs and Destination Details as your Logs Analytics Workspace → Select Add     data source.<br>
   ![Azure image](https://imgur.com/tub722G.png)
   ![Azure image](https://imgur.com/TklDdb1.png)
   b. Review + create the data collection rules.<br>
   ![Azure image](https://imgur.com/OEUAO97.png)
5. Configure Special Windows Event Data Collection (Defender and Windows Firewall).<br>
   a. Go back to see that the data collection rules were created → select it → go to Data sources → select Windows Event Logs → Change Basic to Custom → copy    this XPath query: Microsoft-Windows-Windows Defender/Operational!*[System[(EventID=1116 or EventID=1117)]] → paste and add it → copy this XPath query:        Microsoft-Windows-Windows Firewall With Advanced Security/Firewall!*[System[(EventID=2003)]] → paste, add it and select Save.
   ![Azure image](https://imgur.com/YuKhr21.png)
   ![Azure image](https://imgur.com/Nx3awQU.png)
   ![Azure image](https://imgur.com/aBCBOYB.png)
   ![Azure image](https://imgur.com/tyLAKYA.png)
   b. Check that both XPath queries have been added.<br>
   ![Azure image](https://imgur.com/stCdnBf.png)
</details>



<details>
<summary>Part 4: Manually install the Log Analytics Agent on both the windows-vm and linux-vm.</summary>

1. Install the Log Analytics Agent on the windows-vm.<br>
2. Install the Log Analytics Agent on the linux-vm.<br>
</details>


<details>
<summary>Part 5: Begin querying logs in Log Analytics from the VMs and NSGs.</summary>

1. Query Syslog (linux).<br>
   a. Go to Logs in your Logs Analytics Workspace → start querying the logs → type Syslog for Linux & click Run.
2. Query SecurityEvent (windows).<br>
   a. Type SecurityEvent for Windows & click Run.
3. Query AzureNetworkAnalytics_CL (Network Security Groups/NSGs).<br>
   a. Type AzureNetworkAnalytics_CL for the NSG logs & click Run.
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
