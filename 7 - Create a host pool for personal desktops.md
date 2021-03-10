# Module 7: Create a host pool for personal desktops

Duration:  30 minutes

In this exercise we will be creating a Windows Virtual Desktop host pool for personal desktops. 

**Additional Resources**

  |              |            |  
|----------|:-------------:|
| Description | Links |
| Create a host pool with the Azure portal | https://docs.microsoft.com/en-us/azure/virtual-desktop/create-host-pools-azure-marketplace |
  |              |            | 

### Task 1: Create a new Host Pool and Workspace

1.  Sign in to the [Azure Portal](https://portal.azure.com/).

2.  Search for **Windows Virtual Desktop** and select it from the list.

    ![From the Azure portal search bar, search for windows virtual desktop and select the service.](images/searchwvd.png "Search for Windows Virtual Desktop")    

3.  Under Manage, select **Host pools** and Select **+ Add**.
   
    ![Select host pools under manage and select add to add a new host pool.](images/wvdHostPool.png "Windows Virtual Desktop blade")

4.  On the Basics page, refer to the following screenshot to fill in the required fields. Once complete, Select **Next: Virtual Machines**.

    ![Here is where you will enter the information for the host pool.](images/createpersonalhostpool.png "Create host pool page")

5.  Select **Yes** to **Add virtual machines**  When you configure **Virtual machine settings**: 

- **Number of VMs** should be **2**

- **Virtual Network** should match the one created when demploying the domain controller template and should end with wvd1-vnet

- **Network Security Group** should be **None**

- **Vritual Machine Administrator Account** username should be **locadmin** and the password should match the other passwords used previously
   
- Select **Browse all images and disks** and then select the tab option for **My Items** then **Shared Images** to select the image that was created.

    ![This is the image that you need for your host pool virtual machine.](images/hostpoolcustom.png)

    ![In this blade, enter in the information for the host pool name and select next for virtual machines.](images/nextworkspace.png)

6.  On the Workspace page, select **Yes** to register a new desktop app group. Select **Create new** and provide a **Workspace name**. Select **OK** and **Review + create**.

    ![From the create a host pool workspace tab, enter the required information.](images/hostpoolWorkspace.png "Create a host pool workspace tab")

7.  On the Create a host pool page, Select **Create**.

### Task 2: Create a friendly name for the workspace

The name of the Workspace is displayed when the user signs in. Available resources are organized by Workspace. For a better user experience, we will provide a friendly name for our new Workspace. 

>**Note**: The workspace will not appear until Task 1 has completed deployment. 

1.  Sign in to the [Azure Portal](https://portal.azure.com/).

2.  Search for **Windows Virtual Desktop** and select it from the list.

    ![From the Azure portal search bar, search for windows virtual desktop and select the service.](images/searchwvd.png "Search for Windows Virtual Desktop")

3.  Under Manage, select **Workspaces**. Locate the Workspace you want to update and Select on the name.

    ![Locate the workspace that was created in Task 1 and select it.](images/workspaceproperties.png)

4.  Under Settings, select **Properties**.

5.  Update the **Friendly name** field to your desired name.

    ![Under properties of the workspace, enter a name under friendly name and save.](images/savefriendlyname.png)

6.  Select **Save**.

    ![From the workspace properties tab, view the workspace that you created.](images/workspaceFriendlyName.png "workspace properties tab")

### Task 3: Assign an Azure AD group to an application group

In the new Windows Virtual Desktop ARM portal, we now have the ability to use Azure Active Directory groups to manage access to our host pools.

1.  Sign in to the [Azure Portal](https://portal.azure.com/).

2.  Search for **Windows Virtual Desktop** and select it from the list.

    ![From the Azure portal search bar, search for windows virtual desktop and select the service.](images/searchwvd.png "Search for Windows Virtual Desktop")

3.  Under Manage, select **Application groups**.
    
4.  Locate the Application group that was created as part of Task 1. Select on the name.

    ![Here is where you will find the application group created in Task 1.](images/wvdappgroups.png)

5.  Under Manage, select **Assignments** and Select **+ Add**.

    ![Find manage in the menu and select assignments and add.](images/addassignments.png)

6.  In the fly out, enter **WVD** in the search to find the name of your Azure AD group. In this exercise we will select **WVD Pooled Desktop Users** 

    ![Here are the groups that you need to select and save.](images/wvdpooleduseradd.png)

7.  Choose **Select** to save your changes.

    ![Find and select the WVD Pooled desktop users in the list of users and groups.](images/hostpoolusers.png "Host pool users for WVD")

With the assignment added, you can move on to the next exercise. The users in the Azure AD group can be used to validate access to the new host pool in a later exercise.
