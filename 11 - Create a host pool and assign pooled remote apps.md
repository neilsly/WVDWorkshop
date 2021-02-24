# Step 11: Create a host pool and assign pooled remote apps.

Duration:  30 minutes

In this exercise we will be creating a non-persistent host pool for publishing remote apps. This enables you to assign users access to specific applications rather than an entire desktop. This type of application deployment serves many purposes and is not new to WVD, but has existed in Windows Server Remote Desktop Services for many years.

**Additional Resources**

  |              |            |  
|----------|:-------------:|
| Description | Links |
| Publish built-in apps in Windows Virtual Desktop | https://docs.microsoft.com/en-us/azure/virtual-desktop/publish-apps |
| Manage app groups with the Azure portal | https://docs.microsoft.com/en-us/azure/virtual-desktop/manage-app-groups |
  |              |            | 

### Task 1: Create a new host pool and workspace

1.  Sign in to the [Azure Portal](https://portal.azure.com/).

2.  Search for **Windows Virtual Desktop** and select it from the list.

    ![From the Azure portal search bar, search for windows virtual desktop and select the service.](images/searchwvd.png "Search for Windows Virtual Desktop")

3.  Under Manage, select **Host pools** and Select **+ Add**.

    ![Select host pools under manage and select add to add a new host pool.](images/wvdHostPool.png "Windows Virtual Desktop blade")

4.  On the Basics page, refer to the following screenshot to fill in the required fields. Selecting **Pooled** for host pool type. Once complete, Select **Next: Virtual Machines**.

- For **Resource Group** select the resource group you created when provisioning the template to create your domain controller

- For **Location** select the Azure Region you selected when provisioning the template to create your domain controller

    ![In this blade, enter in the information for the virtual machines that will host the remote apps and select next for workspace.](images/remoteapppool.png)

5.  Select **Yes** to **Add virtual machines**  When you configure **Virtual machine settings**, select **Browse all images and disks** and then select the tab option for **My Items** then **Shared Images** to select the image that was created.

    ![This is where you will find your custom image to add to the host pool.](images/hostpoolcustom.png)

- **Number of VMs** should be **2**

- **Virtual Network** should match the one created when demploying the domain controller template and should end with wvd1-vnet

- **Network Security Group** should be **None**

- **Vritual Machine Administrator Account** username should be **locadmin** and the password should match the other passwords used previously


>**Note:** this is being done for ease of use, please do not do this in production

![In this blade, enter in the information for the host pool name and select next for virtual machines.](images/nextworkspace.png)

6.  On the Workspace page, select **Yes** to register a new desktop app group. Select **Create new** and provide a **Workspace name**. Select **OK** and **Review + create**.

    ![In this blade, select yes and create a new workspace.  Select review and create when complete.](images/newworkspaceremoteapps.png)

7.  On the Create a host pool page, Select **Create**.

### Task 2: Create a friendly name for the workspace

The name of the Workspace is displayed when the user signs in. Available resources are organized by Workspace. For a better user experience, we will provide a friendly name for our new Workspace.

1.  Sign in to the [Azure Portal](https://portal.azure.com/).

2.  Search for **Windows Virtual Desktop** and select it from the list.

    ![From the Azure portal search bar, search for windows virtual desktop and select the service.](images/searchwvd.png "Search for Windows Virtual Desktop")

3.  Under Manage, select **Workspaces**. Locate the Workspace that was created for remote apps and Select on the name.

    ![Locate the workspace that was created in Task 1 and select it.](images/workspaceproperties.png)

4.  Under Settings, select **Properties**.

5.  Update the **Friendly name** field to your desired name.

    ![Under properties of the workspace, enter a name under friendly name and save.](images/savefriendlyname.png)

6.  Select **Save**.

    ![From the workspace properties tab, view the workspace that you created.](images/workspaceFriendlyName.png "workspace properties tab")


### Task 3: Add Remote Apps to your Host Pool

1.  Sign in to the [Azure Portal](https://portal.azure.com/).

2.  Search for **Windows Virtual Desktop** and select it from the list.

3.  Under Manage, select **Host pools** and select the host pool that you created in Task 1.  Select **Application groups** and select **Add** to create a new application group.
   
    ![From the Windows Virtual Desktop blade, select the host pool and then add to add an application groups.](images/newappgroup.png "Manage Application groups")

4.  In the Basics tab, name the application group and select **Next: Assignments**.
   
    ![From this blade, enter a name for the application group.](images/appgroupname.png)

5.  On the assignments tab, select **Add assignments**.  Search for the **WVD Remote App All Users** and **AAD DC Administrators** created earlier in this guide and choose **Select**.  
    >**Note**: AAD DC Administrators will allow you to use your Azure tenant login to access resources in Exercise 7.

6.  Select **Next: Applications**.

    ![From the application group blade, you will select to add users or user groups and select the WVD Remote App All users from the blade that opens next.](images/assigngroup.png)

7.  On the Applications page, Select **+ Add Application**.

8.  On the Add Application fly out, next to Application source, select **Start Menu**. add the following applications, Selecting **Save** between selections.

    - Outlook

    - Microsoft Edge

    - Microsoft Teams

    - Word

    - PowerPoint

    - Excel

    ![After selecting and saving each application, it will be populated in the list of applications.](images/selectapps.png)

    ![The final list of applications will look like this.](images/listofapps.png)

9.  Select **Next: Workspace**.

10. On the Workspace page, select **Yes** to register the application group.

    >**Note**: The **Register application group** field will automatically populate with the workspace name.

11. Select **Review + Create**.

    ![The workspace name will auto-populate and you will select review and create.](images/remoteappws.png)

12. Select **Create**.

You have successfully created a Remote App non-persistent Host Pool with published apps. You can validate this configuration when we connect to the environment in a later exercise.
