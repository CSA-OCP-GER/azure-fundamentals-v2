# Core Azure Services

## Content of this module

- Core Azure architectural components
- Core Azure services and products
- Azure solutions
- Azure management tools

## Slides

[slide deck](2_basic-azure-services.pptx)

## Exercise  Prerequisites

Before your can start into ehe exercises think about the following **prerequisites**:

You require need an Azure subscription to perform these steps. If you don't have one you can create one by following the steps outlined on the [Create your Azure free account](https://azure.microsoft.com/en-us/free/) today webpage

## Exercise 1 - First Steps in Azure, create a VM 

Let's deploy our first VM in Azure!

<details><summary>Step by Step Guide - click to open</summary><p>

1. Sign in to the Azure portal at https://portal.azure.com
2. Choose **Create a resource** in the upper left-hand corner of the Azure portal.
3. In the search box above the list of Azure Marketplace resources, search for and select **Windows Server 2016 Datacenter**, then choose **Create**.
4. In the **Basics** tab, under Project details, make sure the correct subscription is selected and then choose to **Create new resource group**. Type myResourceGroup for the name.
5. Under **Instance details**, type **myVM** for the Virtual machine name and choose **West Europe** for your Location. Leave the other defaults.
6. Under the **Administrator account** section, provide a username, such as **azureuser** and a password. The password must be at least 12 characters long and meet the defined complexity requirements.
7. Under **Inbound port** rules, choose **Allow selected ports** and then select **RDP (3389)** and **HTTP (80)** from the drop-down. These are to allow us to connect to the virtual machine using RDP over port 3389 and then to see a web page display over HTTP on port 80.
8. Go to the Management tab and under the **Monitoring** section under **Boot diagnostics** select **Off**
9. Leave the remaining defaults and then select the **Review + create** button at the bottom of the page.
10. Once Validation is passed click the **Create** button. It can take approx three to five minutes to deploy the virtual machine.
11. Once the virtual machine is created, go to the resource group you placed the virtual machine in, and open up the virtual machine, then click the **Connect** button on the virtual machine properties page.

>**Note:** The following directions tell you how to connect to your VM from a Windows computer. On a Mac, you need an RDP client such as this Remote Desktop Client from the Mac App Store and on Linux virtual machine you could connect directly from a bash shell using ssh.

12.  In the **Connect to virtual machine** page, keep the default options to connect by DNS name over port 3389 and click Download RDP File.
13.  Open the downloaded RDP file and click **Connect** when prompted.
14.  In the **Windows Security** window, select **More choices** and then Use a different account. Type the username as **localhost\username**, (you could also type .\azureuser) enter password you created for the virtual machine, and then click **OK**.
15.  You may receive a certificate warning during the sign-in process. Click **Yes** or to create the connection and connect to your deployed VM. You should connect successfully.

>**Congratulations!** You have deployed and connected to a >Windows Server virtual machine in Azure

If you wish and have time you could also make the deployed server a functioning web server and make a web page available publicly, by continuing with the following steps

16. Open up a PowerShell command prompt on the virtual machine, by clicking the **Start** button, typing **PowerShell** right clicking **Windows PowerShell** in the menu and selecting **Run as administrator**
17. Install the **Web-Server** feature in the virtual machine by running the following command in the PowerShell command prompt:

````PowerShell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools 
````

18.  When completed you should see a prompt stating **Success** with a value **True**, among other items in the output. You do not need to restart the virtual machine to complete the installation. Close the RDP connection to the VM.
19.  Back in the portal, select the VM and in the overview pane of the VM, use the **Click to copy** button to the right of the IP address to copy it and paste it into a browser tab.
20.  The default IIS Web Server welcome page will open, and is available to connect to publicly via this IP address, or via the fully qualified domain name.

>**Congratulations!** You have created a web server that can be connected to publicly via this IP address, or via the fully qualified domain name. If you had a web page to host you could deploy those source files to the virtual machine and host them for public access on the deployed virtual machine.
>**Note:** Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not incur unexpected costs. Remove unused resources by deleting the Resource Group that the unused resources belong to.
</p></details>



## Exercise 2 - Create a Storage Account

Let's our first storage account in  in Azure ans upload a file

<details><summary>Step by Step Guide - click to open</summary><p>

1. Sign in to the Azure portal at https://portal.azure.com
2. Select All services on the upper left hand side of the Azure Portal. In the **All services** filter box, type **Storage Accounts**. As you begin typing, the list filters based on your input. Select **Storage Accounts**.
3. On the **Storage Accounts** window that appears, if there are no storage accounts present you can select **Create storage account**, or if there are already storage accounts present, this option will nt be present and you can choose the option **+ Add**.
4. Complete the Create storage account blade with the following details:
   - Subscription=< Select your subscription >
   - Resource group=Select **Create new**, enter **strac-rg1**, then select **OK**.
   - Storage account name=< this must be between 3-24 characters in length, can be numbers and lowercase only, and must be unique across Azure >
   - Location=**West Europe**
   - Performance=**Standard**
   - Account kind=Leave the default value **StorageV2 (general purpose v2)**
   - Replication=**Locally redundant storage (LRS)**
   - Access tier (default) **Hot**
5. Select **Review + Create** to review your storage account settings and allow Azure to validate the configuration. Once validated select **Create**.
6. Verify its successful creation by going to the resource group just created and locate the storage account.
7. Open the storage account and scroll in the left menu for the storage account, scroll to the **Blob** service section, select **Blobs** and then select the **+ Container** button.
8. Configure the blob container as below and select **OK** when complete to create the blob container.
Setting Value Name i.e. **blob1** The container name must be lowercase, must start with a letter or number, and can include only letters, numbers, and the dash (-) character. public access level leave the default value i.e. The default level is **Private (no anonymous access)**
9. The container should be created and available
10. We will upload a block blob to your new container. Select the container to show a list of blobs it contains. Since this container is new, it won't yet contain any blobs
>**Note:** Block blobs consist of blocks of data assembled to make a blob. Most scenarios using Blob storage employ block blobs. Block blobs are ideal for storing text and binary data in the cloud, like files, images, and videos.
11. Create a .txt file on your local machine, named **blob1.txt**, and enter some text into it, such as this is a blob file or something like that.
12. Select the **Upload** button to upload a blob to the container. Browse your local file system to find the file you created in the previous steps to upload as a block blob, Click on the **Advanced** arrow, leave the default values as they are, just note them, and then select **Upload**.
>**Note:** You can upload as many blobs as you like in this way. You'll see that the new blobs are now listed within the container.
13. View the uploaded block blob by right clicking on the blob file that was uploaded and selecting **View/edit blob**
14. You can download a block blob by right clicking on the block blob and selecting **Download**. The blob file opens in a browser and is then downloadable by right clicking on the file and selecting save as

>**Congratulations!** You have created a storage account, created a blob storage container within that storage account, then uploaded a block bob, viewed and edited the block blob in the blob container and then downloaded the block blob.

>**Note:** Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not incur unexpected costs. Remove unused resources by deleting the Resource Group that the unused resources belong to.
</p></details>



## Exercise 3 - Create Network in Azure with two VMs 

Let's build our first Virtual Network in Azure

<details><summary>Step by Step Guide - click to open</summary><p>

1. Sign in to the Azure portal at https://portal.azure.com
2. Choose **Create a resource** in the upper left-hand corner of the Azure portal, then select **Networking > Virtual network**
3. In the List select  **virtual network** and in the New virtaul network pane create a network using the following settings and values:

   - Name=**vnet1**
   - Address space=**10.1.0.0/16**
   - Subscription=< Select your subscription >
   - Resource group= Select Create new, enter **vnet1-rg1**, then select OK.
   - Location=**West Europe**
   - Subnet – Name= **subnet1**
   - Subnet Address range=**10.1.0.0/24**
   - Leave the rest of the settings at their default values and select Create.

4. Verify the creation of the virtual network by going to the newly created resource group and viewing the virtual network is present, you can click on the virtual network and view its properties if you wish.
5. Create a virtual machine by going to the the upper-left side of the Azure Portal and selecting **Create a resource > Compute > Windows Server 2016 Datacenter** (see exercise 1 above)
6. In Create a **virtual machine - Basics** tab, enter or select this information:

    - Subscription=< Select your subscription >
    - Resource group=The resource group you created it in the last section, i.e. **vnet1-rg1**
    - Virtual machine name=**vm1**
    - Region=**West Europe**
    - Availability options=Leave the default **No infrastructure redundancy required**
    - Image=Leave the default **Windows Server 2016 Datacenter**
    - Size=Leave the default **Standard DS1 v2**
    - Username=**azureuser**
    - Password=enter a password that meets the complexity requirements.
    - Public inbound ports=Select **Allow selected ports**
    - Selected inbound ports=Select **HTTP, HTTPS, SSH** and **RDP**

7. Select **Next** : **Disks**, leave the default values.
8. Select **Next** : **Networking**, complete the following details

   - Virtual network=Leave the default **vnet1**
   - Subnet=Leave the default **subnet1 (10.1.0.0/24)**
   - Public IP=Leave the default (new) **vm1-ip**
   - NIC network security group=accept the default **Basic**
   - Public inbound ports=Select **Allow selected ports**
   - Select inbound ports=Select **HTTP, HTTPS, SSH** and **RDP**
  
9. Select **Next** : **Management**, accept all the defaut values except for the below settings:

   - Boot diagnostics=accept the default value i.e. **On***
   - Diagnostic storage account=accept the default value i.e. **vnet1rgdiag**

10.  Select **Review + create**. Azure will validate the configuration. When you see that Validation passed, select **Create**. Deployment times can vary but it can generally take between three to six minutes to deploy.
11.  Create a second Virtual machine by repeating steps **5 to 9** above, using the same values above above ensuring the below settings are set:

     - Virtual machine **name=vm2**
     - Public IP=Leave the default (new) **vm2-ip**
     - Diagnostic storage account=Leave the default value i.e. **vnet1rg1diag**

12.  When finished filling in the details, validate the configuration by clicking **Review + create** and once successfully validated click **Create**
13.  When both virtual machine have completed deployment connect to the first virtual machine, **vm1**, by going to the resource group you placed the virtual machine in, **vnet-rg1** and open up the virtual machine, then click the **Connect** button on the virtual machine properties page.

>**Note:** The following directions tell you how to connect to your VM from a Windows computer. On a Mac, you need an RDP client such as this Remote Desktop Client from the Mac App Store and on Linux virtual machine you could connect directly from a bash shell using ssh.

14.  In the **Connect to virtual machine** page, keep the default options to connect by DNS name over port 3389 and click **Download RDP** File.
15.  Open the downloaded RDP file and click **Connect** when prompted.
16.  In the **Windows Security** window, select **More choices** and then **Use a different account**. Type the username as **localhost\username**, (you could also type .\azureuser) enter password you created for the virtual machine, and then click **OK**.
17.  You may receive a certificate warning during the sign-in process. Click **Yes** or to create the connection and connect to your deployed VM. You should connect successfully.
18.  Open up a PowerShell command prompt on the virtual machine, by clicking the **Start** button, typing **PowerShell** right clicking **Windows PowerShell** in the menu and selecting **Run as administrator**
19.  Run the command ping vm2 
You receive an error, saying request timed out. The ping fails, because ping uses the **Internet Control Message Protocol (ICMP)**. By default, ICMP isn't allowed through the Windows firewall.
20. To allow *vm2* to ping *vm1* enter the below command. This command allows ICMP inbound through the Windows firewall:

````PowerShell
New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4 
````

21.  Connect to *VM2* as has been done for *VM1*, using rdp. i.e. open **vm2** properties and click the **Connect** button to download and then connect vis RDP
22.  Open up a PowerShell command prompt on the virtual machine, *VM2*, and run the command:

````PowerShell
ping vm1
````

You should now be able to *ping the vm1* virtual machine successfully, because ICMP has been configured to be allowed through the Windows firewall on the vm1 virtual machine in an earlier step.

>**Congratulations!** This ping is being done using the virtual network you created and deployed the two virtual machines into. The two virtual machines are communicating over this virtual network that was created.

>**Note:** Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not incur unexpected costs. Remove unused resources by deleting the Resource Group that the unused resources belong to.
</p></details>

## Additional information

Links

|Demo|Link|
|----|----|
|Deploy Storage|<https://www.youtube.com/watch?v=Y8hz0oIuWDs>|
|Deploy VM|<https://www.youtube.com/watch?v=rGGfRogOCJQ>|
|Customize the Azure Portal|<https://www.youtube.com/watch?v=rGGfRogOCJQ>|