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

1. Install the Windows Agent on the windows-vm.<br>
   a. Go to our Log Analytics Workspace and select it → select Agents → expand Log Analytics Agent instructions → Copy the Workspace ID and the Primary key.<br>
   ![Azure image](https://imgur.com/Pa4XBpG.png)
   b. RDT into the window vm → open the notepad and paste the Workspace ID and Primary key.<br>
   ![Azure image](https://imgur.com/rdUTgk3.png)
   ![Azure image](https://imgur.com/h2eP8w2.png)<br>
   c. Go back to Log Analytics Agent → copy the download Windows Agent (64 bit) link → open Edge in the windows vm → paste the download link in the search          bar and press enter to download.<br>
   ![Azure image](https://imgur.com/AO5vsLR.png)
   d. Open file → select next and you can see the agent was already installed, so we will add the Log Analytics Workspace through the already downloaded            agent.<br>
   ![Azure image](https://imgur.com/2fCrLF2.png)
   ![Azure image](https://imgur.com/OO3BNlX.png)
   e. Search for control panel → switch to large icons → select Microsoft Monitoring Agent → Go to Azure Log Analytics (OMS) tab → click Add → paste the         Workspace ID and the Primary key → Make sure it’s Azure Commercial → select Ok → select Apply → Status should show a green check to signify that the          connection is successful.<br>
   ![Azure image](https://imgur.com/grc8GHB.png)
   ![Azure image](https://imgur.com/Qx38xeK.png)
   ![Azure image](https://imgur.com/dCayHGV.png)
   ![Azure image](https://imgur.com/7EHAOlK.png)
   f. You should now see both windows servers are connected.<br>
   ![Azure image](https://imgur.com/xH4UqJ6.png)
3. Install the Log Analytics Agent on the linux-vm.<br>
    a. Go back to Log Analytics Agent → select Linux servers → expand Log Analytics Agent instructions → Copy the agent for Linux.<br>
    ![Azure image](https://imgur.com/HfcEJYj.png)
    b. Open the terminal and SSH into the linux vm: ssh labuser@172.206.42.40 → enter password: Cyberlab1234! → install the agent by pasting it and click         enter → you should get a status code 0 at the end.<br>
    ![Azure image](https://imgur.com/D1pnqNG.png)
    ![Azure image](https://imgur.com/ifFhtrE.png)<br>
    c. Go back to Log Analytics Agent to check and see both linux servers are connected.<br>
</details>


<details>
<summary>Part 5: Begin querying logs in Log Analytics from the VMs and NSGs.</summary>

1. Query Syslog (linux).<br>
   a. Go to Logs in your Logs Analytics Workspace → start querying the logs → type Syslog for Linux & click Run.<br>
   ![Azure image](https://imgur.com/SA32o44.png)
3. Query SecurityEvent (windows).<br>
   a. Type SecurityEvent for Windows & click Run.<br>
   ![Azure image](https://imgur.com/XPBGpgw.png)
5. Query AzureNetworkAnalytics_CL (Network Security Groups/NSGs).<br>
   a. Type AzureNetworkAnalytics_CL for the NSG logs & click Run.<br>
   ![Azure image](https://imgur.com/DOm0oiT.png)
</details>


![Architecture Diagram](https://imgur.com/Qa1MyLI.png)
### Tenant Level Logging
> [!NOTE]
> In this lab, we will continue ingesting logs into our central log repository, the Log Analytics Workspace. This time, we will import audit and sign-in logs from Microsoft Entra ID (formerly Azure Active Directory).



<details>
<summary>Part 1: Setup logging for Microsoft Entra ID and generate some logs.</summary>

1. Create Diagnostic Settings to ingest Azure AD Logs.<br>
    a. Go to Microsoft Entra ID → select Diagnostic setting → select Add diagnostic setting.<br>
    ![Azure image](https://imgur.com/nd6L8bz.png)
2. Name it ds-audit-signin → select AuditLogs and SigninLogs → Select Send to Log Analytics Workspace and Select the workspace → select Save.<br>
![Azure image](https://imgur.com/XhmXPJC.png)
3. Go back to the diagnostic setting to see the one we created.<br>
![Azure image](https://imgur.com/UyOB5q2.png)
4. Check Log Analytics Workspace to see that the tables have been created: “AuditLogs” “SigninLogs”.<br>
    a. Go to Log Analytics Workspace → select Tables → type audit & signin to see if they were created → we can still attempt to query it even if they don’t show up.<br>
    ![Azure image](https://imgur.com/7uhhbTk.png)
    ![Azure image](https://imgur.com/b7SfcI1.png)
</details>


<details>
<summary>Part 2: Create a dummy user, username “dummy_user” to generate audit logs.</summary>

1. Go to Microsoft Entra ID → select Users → select New User and Create new user → name it dummy_user → copy temp. password → select Review + create →        copy the user principal name → select Create.<br>
![Azure image](https://imgur.com/S2Wg3sH.png)
![Azure image](https://imgur.com/OXmCwOR.png)
![Azure image](https://imgur.com/TtFrEOI.png)
2. Refresh and you can see that the user has been created.<br>
![Azure image](https://imgur.com/NhS9n8w.png)
3. Login once with the dummy_user credentials to generate signin logs (incognito window).<br>
    a. Open a private tab and login using the dummy_user credentials and update password to Cyberlab123!.<br>
    ![Azure image](https://imgur.com/VTP9kwf.png)
    ![Azure image](https://imgur.com/POjWdqE.png)
5. Assign dummy_user the Role of Global Administrator.<br>
    a. Select the dummy_user → select Assigned roles → select Add assignments → type global in search bar → select Global Administrator → select Add.<br>
   ![Azure image](https://imgur.com/UoNs2cM.png)
   b. Refresh to see assigned role.<br>
   ![Azure image](https://imgur.com/kF5kyIm.png)
7. Delete dummy_user.<br>
    a. Delete the dummy_user to generate another audit log → select Overview and delete the user.<br>
    ![Azure image](https://imgur.com/R9l5JfL.png)
</details>

<details>
<summary>Part 3: Observe Audit Logs logs in Log Analytics Workspace.</summary>

1. Go to Log Analytics Workspace → select Logs → change time to local time → query AuditLogs.<br>
![Azure image](https://imgur.com/vXUslDr.png)
![Azure image](https://imgur.com/7i1DRsb.png)
</details>

<details>
    
<summary>Part 4: Simulate Brute Force Attack against Microsoft Entra ID.</summary>

1. Create the “attacker” user if does not exist already.<br>
    a. Go to Microsoft Entra ID → select Users → select New User and Create new user → name it attacker → copy temp. password → select Review + create → copy 
    the user principal name → select Create.<br>
    ![Azure image](https://imgur.com/mQPcPSb.png)
    ![Azure image](https://imgur.com/jXa2Xyf.png)
    ![Azure image](https://imgur.com/jXa2Xyf.png)
    ![Azure image](https://imgur.com/XOV7y7k.png)<br>
    b. refresh and you can see that the user has been created.<br>
    ![Azure image](https://imgur.com/eW6BUfs.png)
3. Produce 10-11 failed Logins with the portal.<br>
    a. Open a private tab, and login using the attacker user credentials and update password to Cyberlab123! → we will login correctly once, and then do a        failed login.<br>
    ![Azure image](https://imgur.com/Bv2HLXk.png)
    ![Azure image](https://imgur.com/QUcCFO7.png)<br>
    b. Once logged in, close and reopen the private browser and attempt to sign in with a wrong password 10 times → then login with the correct password →        close the private browser.<br>
    ![Azure image](https://imgur.com/4rOpKtA.png)
</details>

<details>
<summary>Part 5: Observe SigninLogs in Log Analytics Workspace.</summary>

1. Go to Log Analytics Workspace → select Logs → change time to local time → query SigninLogs.
![Azure image](https://imgur.com/5Dzu9lG.png)
![Azure image](https://imgur.com/ZEdsEO2.png)
</details>



![Architecture Diagram](https://imgur.com/Qa1MyLI.png)
### Subscription Level Logging (Activity Log)
> [!NOTE]
> In this lab, we will ingest subscription-level logs, specifically the Activity Log. We’ll create, modify, and delete resource groups to later query these actions in our Log Analytics Workspace.



<details>
<summary>Part 1: Export Azure Activity Logs to Log Analytics Workspace.</summary>

1. Go to Monitor → select Activity log → select Export Activity Logs → select Add diagnostic setting → name it ds-azure-activity → select all the Logs → select Send to Log Analytics Workspace → select LAW-Cyber-Lab-0x → select Save.<br>
![Azure image](https://imgur.com/JXgJhru.png)
![Azure image](https://imgur.com/QNTtPKS.png)
![Azure image](https://imgur.com/rY7wtHJ.png)
3. Go back to diagnostic setting to see what we created.<br>
![Azure image](https://imgur.com/p1TBbkt.png)
</details>

<details>
<summary>Part 2: Create a new Resource Group named “Scratch-Resource-Group”.</summary>

1. Go to Resource groups → select Create → name it Scratch-Resource-Group → put it in the same region as the other groups, US East US 2 → select Review + create → select Create.<br>
![Azure image](https://imgur.com/InQ3Eso.png)
![Azure image](https://imgur.com/Cp3P3jn.png)
![Azure image](https://imgur.com/qCvnA8T.png)
</details>

<details>
<summary>Part 3: Create another new Resource Group named “Critical-Infrastructure-Wastewater”.</summary>

1. Go to Resource groups → select Create → name it Critical-Infrastructure-Wastewater → put it in the same region as the other groups, US East US 2 → select Review + create → select Create.<br>
![Azure image](https://imgur.com/InQ3Eso.png)
![Azure image](https://imgur.com/h1oICre.png)
![Azure image](https://imgur.com/kNBAGIv.png)
</details>

<details>
<summary>Part 4: Delete both “Scratch-Resource-Group” and “Critical-Infrastructure”.</summary>
(DO NOT ACCIDENTALLY DELETE YOUR LAB RESOURCE GROUP)

1. Go to Resource groups → select Scratch-Resource-Group → select Delete resource group → copy resource group name → paste it to confirm deletion → select Delete.<br>
![Azure image](https://imgur.com/pRw7cye.png)
2. Go to Resource groups → select Critical-Infrastructure → select Delete resource group → copy resource group name → paste it to confirm deletion → select Delete.<br>
![Azure image](https://imgur.com/7vXTafA.png)
</details>

<details>
<summary>Part 5: Generate some logs.</summary>

1. Go to Log Analytics Workspace → select Logs → query AzureActivity to see if some logs are coming in.<br>
![Azure image](https://imgur.com/SoBpzKK.png)
2. Query for the deletion of the Critical Resource Groups.<br>
![Azure image](https://imgur.com/c2Mg0Tk.png)
3. Query for deletion activities within a certain timespan.<br>
![Azure image](https://imgur.com/CnP2AJA.png)
</details>



![Architecture Diagram](https://imgur.com/Qa1MyLI.png)
### Resource Level Logging Diagnostic Settings)
> [!NOTE]
> This is the final lab focused on ingesting logs into our Log Analytics Workspace. In this lab, we’ll complete the process by enabling logging for our storage account and forwarding the logs to the Log Analytics Workspace. Next, we’ll create a Key Vault, an enterprise password manager, enable logging for it, and send those logs to the workspace as well. Finally, we’ll query the logs to verify the setup.



<details>
<summary>Part 1: Configure Logging for Azure Storage.</summary>

1. Configure logging for your storage account by enabling diagnostic settings for blob storage.<br>
    a. Go to Storage account and select sacyberlab2x → select Diagnostic settings → select disabled for the blob storage account → select Add diagnostic          setting.<br>
    ![Azure image](https://imgur.com/DMyogKO.png)
    ![Azure image](https://imgur.com/FwW1O0u.png)
    b. Name it ds-storage-account → select audit → select Send to Log Analytics Workspace → select LAW-Cyber-Lab-0x → select Save.<br>
    ![Azure image](https://imgur.com/vJGlbOs.png)
3. Go back to diagnostic setting to see what we created.<br>
![Azure image](https://imgur.com/Zkv1ste.png)
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
