# Security, Compliance and Trust

## Content

- Securing network connectivity in Azure
- Core Azure identity services
- Security tools and features
- Azure governance methodologies
- Monitoring and reporting in Azure
- Privacy, compliance and data protection standards in Azure

## Slides

[slide deck](3_security-compliance-trust.pptx)

## Prerequisites

You require need an Azure subscription to perform these steps. If you don't have one you can create one by following the steps outlined on the Create your Azure free account today webpage.

## Exercise 1 - Azure Key Vault

Let's deploy our first Key Vault and save a first secret

<details><summary>Step by Step Guide - click to open</summary><p>

### Firstly we will create a vault

1. Sign into the Azure Portal and go to **All services > Security** and then select **Key vaults.**
2. In the **Key vaults** pane click on **Create key vault**.
3. In the **Create key vault** blade, enter the details as below and click **Create**
   - Name: a name for your vault i.e. **akvtest1**
   - Subscription: < your subscription >
   - Resource Group: select Create new and enter a new resource group name i.e. **akvrg**
   - Location: < a Datacenter location near you i.e. **West Europe** >
   - Pricing Tier: **Standard**
   - Access policies: < accept default value i.e. **1 principal selected** >
   - Virtual Network Access: < accept default value i.e. **all networks can access** >
4. Go to the newly created Key vault and verify it is present. You can take a moment to browse through some of the options available within it, primarily under **Settings** and then options concerning **Keys, Secrets, Certificates, Access Policies, Firewalls and virtual networks.***
5. Take note of two values in the key vault
- **Vault Name:** In the example it is **akvtest1**
- **DNS name** (also sometimes referred to as the Vault URI): In this example it is `https://akvtest1.vault.azure.net/`. Applications that use your vault through its REST API must use this URI.
>**Note:** Your Azure account is the only one authorized to perform operations on this new vault. Yo can modify this if you wish in the **Settings > Access policies** section

### Add a secret to the Key Vault
We will now add a password that could be used by an application.
1. On the Key Vault properties pages select **Secrets**, then select **Generate/Import**.
2. On the **Create a secret** blade enter the below values, leave the other values at their defaults and then click **Create**.
   - **Upload options**: Manual
   - **Name**: ExamplePassword
   - **Value**: hVFkk965BuUv96!
3. Once the secret has been successfully created, on the **Secrets** pane, click on the **ExamplePassword**, and note it has a status of Enabled
4. Double click on the password and in the password pane, note the presence of the Secret Identifier. This is the url value that you can now use with applications. It provides a centrally managed and securely stored password for use with applications.
5. In the same pane click the button **Show Secret Value**, to display the password you specified earlier.
>**Note:** It is also possible to set time limitations on when a password is available for use, using the activation and expiration date settings.

>**Congratulations!** You have created an Azure Key vault and then created a password secret in that key vault, providing a securely stored, centrally managed password for use with applications.

>**Note:** Remember to delete the resources you have just deployed if you are no longer using them to ensure you do not incur costs for running resources. You can delete all deployed resources by deleting the resource group in which they all reside.

</p></details>

## Exercise 2 - RBAC

In this exercise task we will create some Azure resources that we can manage using Role-Based-Access-Control (RBAC), then we will view access control at subscription level, then view roles and permissions at resource group level for azure resources, and view individual user and all role assignments. You will then add a new role assignment for the virtual machine contributor role and then remove a role assignment for the resources you deployed.

<details><summary>Step by Stap Guide - click to open</summary><p>

### Create Azure resources to manage

Firstly, we will deploy some resources to Azure to provide us with some resources to manage.

1. Sign into the Azure Portal and click on the **Cloud Shell** icon in the top right hand corner
2. The **Cloud Shell** is launched in the bottom of the browser window. 
3. Create a resource group into which we will place our resources by running the following Azure CLI command. You can copy and paste the command from the below directly into the Cloud Shell console, then press **Enter** to run the command. This command will run fine in either **powershell** *or* **bash** console.

```azurecli
az group create `
--name rbacrg `
--location westeurope
```

4. Run the below Azure CLI command to create a virtual machine. Again, you can copy and paste the command from below directly into the Cloud Shell console and press Enter to run it.

```azurecli
az vm create `
--name vmrbac1 `
--resource-group rbacrg `
--image Win2019Datacenter `
--location westeurope `
--admin-username azureuser `
--admin-password Password0134!
```

>**Note:** The command will take 2 to 3 minutes to complete. The command will create a virtual machine and various resources associated with it such as storage, networking and security resources. You can close the Azure Cloud Shell once it is complete.

### View access control at subscription level

The next thing we need to do, in the context of access control, is to decide where to open the **Access control (IAM)** blade, through which we configure Role-Based-Access-Control (RBAC), and that depends on what resources you want to manage access for. i.e. do you want to manage access for everything in a management group, everything in a subscription, everything in a resource group, or a single resource? The **Access control (IAM)** blade is available at all of these levels and provides the same functionality in each. We will firstly have a look at the **Access control (IAM)** options for a subscription.

1. In the Azure portal, click **All services** and the **Subscriptions**, double click on a subscription from the subscriptions listed and then click on **Access control (IAM)**. See what is possible to configure.

### View roles and permissions

A role definition is a collection of permissions that you use for role assignments. Azure has over 70 built-in roles for Azure resources. Follow these steps to view the available roles and permissions for the resources we deployed earlier.

1. Go to **Resource groups** and choose *rbacrg* i.e. the resource group you created earlier.
2. Within the **rbacrg** resource group, click on **Access control (IAM)** and then select the **Roles** tab to see a list of all the built-in and custom roles.
    >**Note:** You can see the number of users and groups that are assigned to each role at the current scope.
3. Click on the **Owner** role to see who has been assigned this role and also view the permissions for the role.
    >**Note:** As per the screenshot, there are two users listed who are assigned the Owner role. Your list of users will be different.

### View individual user and all role assignments for a resource

When managing access, you want to know who has access, what are their permissions, and at what scope. To list access for a user, group, service principal, or managed identity, you view their role assignments.

1. In the resource group you created earlier i.e. **rbacrg** go to **Access control (IAM)** and select the **Check Access** tab
2. In the **Find** boxes enter the below values, to search the directory for for display names, email addresses, or object identifiers. The matching results are displayed below the **Find** boxes
   - Azure AD user, group, or service principal
   - < your own user name> 
    >**Note:** Your results will be different and related to your own user account.
3. Click the matching result to open the **< name > assignments - scope** pane. On this pane, you can see the roles assigned to the selected user and the scope. If there are any deny assignments at this scope or inherited to this scope, they will be listed. We can see the user has the role of **Owner** assigned and can manage everything.
4. Still on the resource group **Access control (IAM)** pane, click the **Role assignments** tab to view all the role assignments at this scope. On the **Role assignments** tab, you can see who has access at this scope. 
    >**Note:** Some of roles present, are listed as **(Inherited)**. This means they are assigned from another scope. Access, in general, is either assigned specifically to this resource, or inherited from an assignment to the parent scope. Your values will be different to those displayed here.

### Add a role assignment

In RBAC, to grant access, you assign a **Role** to a user, group, service principal, or managed identity. We will assign the a role to a user in the following steps.

1. Open the resource group **Access control (IAM)** and click the **Role assignments** tab, then click **Add** and choose **Add role assignment** to open the **Add role assignment** pane.
    >Note: If you don't have permissions to assign roles, the Add role assignment option will be disabled.
2. In the **Add role assignment** pane fill in the following values, then click Save to assign the role.
   - **Role:** select a Role from the drop down list i.e. *Virtual Machine Contributor*
   - **Assign access to:** Azure AD user, group, or service principal
   - **Select:** < type your own user name, and your user name should appear in the list, then click on a user name to select it >
3. The user is now assigned the specified role at the selected scope.

### Remove role assignments

In RBAC, to remove access, you remove a role assignment.

1. Open the resource group **Access control (IAM)** and click the **Role assignments** tab, 
2. Scroll down through the list of users until you find the user you just added as a **Virtual Machine Contributor**, click on the user, then select **Remove**
3. In the remove role assignment message that appears, click **Yes**.
    >**Note:** Inherited role assignments cannot be removed. If you need to remove an inherited role assignment, you must do it at the scope where the role assignment was created. In the Scope column, the column where (Inherited) appears, there is a link that takes you to the scope where this role was assigned in this case the subscription, then you can go to the Access control (IAM) blade and remove the role assignment there.

**Congratulations!** You have created some Azure resources that you can manage using Role-Based-Access-Control (RBAC), you have viewed access control at subscription level, and then viewed roles and permissions at resource group level for azure resources, and viewed individual user and all role assignments. You then added a new role assignment for the virtual machine contributor role and finally removed a role assignment for the resources you deployed.

**Note:** Remember to delete the resources you have just deployed if you are no longer using them to ensure you do not incur costs for running resources. You can delete all deployed resources by deleting the resource group in which they all reside.

</p></details>

## Additional information

|Topic|Link|
|-----|----|
|Governance|<https://docs.microsoft.com/de-de/learn/modules/intro-to-governance/>|
|Monitoring|<https://docs.microsoft.com/de-de/learn/modules/design-for-efficiency-and-operations-in-azure/3-use-monitoring-and-analytics-to-gain-operational-insights>|
