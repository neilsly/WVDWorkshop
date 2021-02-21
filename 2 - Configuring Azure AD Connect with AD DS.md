## Step 2: Configuring Azure AD Connect with AD DS

Duration:  60 minutes

In this exercise you will be configuring [Azure AD Connect](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-azure-ad-connect). With Windows Virtual Desktop, all session host VMs within the WVD tenant environment are required to be domain joined to AD DS, and the domain must be synchronized with Azure AD. To manage the synchronization of objects, you will configure Azure AD Connect on the domain controller deployed in Azure.

>**Note**: RDP access to a domain controller using a public IP address is not a best practice and is only done to simplify this lab. Better security practices such as removing the PIP, enabling just-in-time access and/or leveraging a bastion host should be applied enhance security.

**Additional Resources**

|                                                             |                                                              |
| ----------------------------------------------------------- | :----------------------------------------------------------: |
| Description                                                 |                            Links                             |
| Windows Virtual Desktop Spring Update enters Public Preview | https://techcommunity.microsoft.com/t5/itops-talk-blog/windows-virtual-desktop-spring-update-enters-public-preview/ba-p/1340245 |
| ARM-based model public preview) deployment walk through     | https://www.christiaanbrinkhoff.com/2020/05/01/windows-virtual-desktop-technical-2020-spring-update-arm-based-model-deployment-walkthrough/#NewAzurePortal-Dashboard |
|                                                             |                                                              |

### Task 1: Connecting to the domain controller

1. Sign in to the [Azure Portal](https://portal.azure.com/).

2. Type **Resource groups** in the search field and select it from the list.

3. On the Resource groups blade, Select on the resource group name that you created in the **Before the HOL** template deployment.

4. On the Infra Resource group blade, review the list of available resources. Locate the resource named **AdPubIP1** and Select on it. Note that the resource type should be **Public IP address**.

   ![Find the public IP address for the domain controller VM](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/publicip.png "Public IP address for Domain Controller VM")

5. On the Overview page for AdPubIP1, locate the **IP address** field. Copy the IP address to a safe location.

6. On your local machine, open the **RUN** dialog window, type **MSTSC** and hit enter.

   ![Open the Run dialog window to run MSTSC](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/run.png "Run on Windows") 

7. In the **Remote Desktop Connection** window, paste in the public IP address from the previous step. Select **Connect**.

   ![The Window for Remote Desktop Connection will open to enter the public IP address for the domain controller VM.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/remoteDesktop.png "Window for Remote Desktop Connection") 

8. When prompted, sign in with the AD domain UPN credentials. For example, if you used the ARM template from step 1, the credentials will be something along the lines of: [adadmin\@MyADDomain.com](mailto:adadmin@MyADDomain.com) with the password you specified whlie deploying the teamplte. If prompted, Select **Yes** to accept the RDP certification warning.

   >**Note**: This is the Active Directory account from the ARM template, not the Azure AD Global Admin account. If you have trouble signing in, try typing the credentials in manually, as copy and paste may include an unnecessary space, which will cause authentication to fail.

### Task 2: Disabling IE Enhanced Security

In an effort to simplify tasks in this lab, we will start by disabling [IE Enhanced Security](https://docs.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-ie-esc).

1. Once connected to the domain controller, open Server Manager if it does not start automatically.

2. In Server Manager, select **Local Server** on the left.

3. Locate the **IE Enhanced Security Configuration** option and Select **On**.

   ![Within the Local Server properties in server manager, locate Enhanced Security configuration.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/IEESC.png "Local Server properties within server manager") 

4. On the Internet Explorer Enhanced Security Configuration window, under **Administrators**, select the **Off** radio button and Select **OK**.

   ![When you select the current configuration, a new window will open that will allow you to disable the enhanced security configuration.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/disablesecurity.png "Disable enhanced security configuration")

### Task 3: Creating a domain admin account

By default, Azure AD Connect does not synchronize the built-in domain administrator account [ADAdmin\@MyADDomain.com](mailto:ADAdmin@MyADDomain.com). This system account has the attribute isCriticalSystemObject set to *true*, preventing it from being synchronized. While it is possible to modify this, it is not a best practice to do so.

1. In Server Manager, Select **Tools** in the upper right corner and select **Active Directory Users and Computers**.

   ![Find Tools on the uppert right corner to access the Server Manager Tools.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/serverMangerTools.png "Server Manager Tools") 

2. In Active Directory Users and Computers, right-click the **Users** organization unit and select **New \> User** from the menu.

   ![Find the folder path for users, and right-click to add a new user](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/newUser.png "Folder path for new user") 

3. Complete the New User wizard.

   ![A window will open with the fields to complete for a new user.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/newuserobject.png "Create a new user")


    ![The next window will allow you to assign a password.](images/newUserWizard.png "New User Wizard window") 
    
    ![The final screen of the wizard will allow you to review and finish the new user setup.](images/finishnewuser.png "Finish new user setup")
    
    >**Note**: This account will be important in future tasks. Make a note of the username and password you create. When setting the password, uncheck the box **User must change password at next logon**.

4. In Active Directory Users and Computers, right-click on the new user account object and select **Add to a group**.

   ![When the new user is created, we will find that user name and right-click to add the user to a group.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/addusertogroup.png "Add new user to a group")

5. On the Select Groups dialog window, type **Domain Admins** and Select **OK**.

   >**Note**: This account will be used during the host pool creation process for joining the hosts to the domain. Granting Domain Admin permissions will simplify the lab. However, any Active Directory account that has the following permissions will suffice. This can be done using [Active Directory Delegate Control](https://danielengberg.com/domain-join-permissions-delegate-active-directory/). 

   ![In the next window, we will add this user to the Domain Admins group.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/addusertodomainadmins.png "Add user to Domain Admins group")

### Task 4: Configuring Azure AD Connect

1. On the desktop of the domain controller, locate the icon for **Azure AD Connect** and open it.

   ![Find the Azure AD Connect icon on the Domain controller VM desktop.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/azureadconnect.png "Azure AD Connect desktop icon")

2. Accept the license terms and privacy notice, then select continue. On the next screen select **Use express settings**. The required components will install.

   ![This will take you to the Azure AD connect set up screen.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/AzureADconnectExpressSetting.png "Azure AD connect set up screen") 

3. On the Connect to Azure AD page, enter in the Azure AD Global Admin credentials. For example: [azadmin\@MyAADdomain.onmicrosoft.com](mailto:azadmin@MyAADdomain.onmicrosoft.com) and the correct password. Select **Next**.

   ![After selecting "Use express settings", the next window will require you to enter your Azure Active Directory username and password.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/adconnectazuresub.png "Azure AD Connect - Azure AD login")

   >**Note**: This is the account associated with your Azure subscription.

4. On the Connect to AD DS page, enter in the Active Directory credentials for a Domain Admin account. For example, if you used the ARM template deployment for the domain controller, the credentials will be something along the lines of: **[[MyADDomain.com]](http://myaddomain.com/) \\ADadmin** with the password: **WVD\@zureL\@b2019!**. Select **Next**.

   ![In the next window, enter the AD DS domain and admin username and password.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/azureadconnectdclogin.png "Azure AD Connect - Domain login")

   >**Note**: If you copy and paste the password, please ensure that there are no trailing spaces, as that will cause the verification to fail.

5. Select **Install** to start the configuration and synchronization.

   ![In the next window, select the box to continue without matching all UPN suffixes and select next to continue.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/azureadsigninconfig.png "Azure AD sign-in configuration")

   ![In the final setup window, select the box to start the synchronization process and select install.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/azureadready.png "Azure AD Connect Ready to configure")

6. After a few minutes the Azure AD Connect installation will complete.
   Select **Exit**.

   ![Once installation is complete the Configuration complete window will be present.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/AADCcomplete.png "The Configuration is completed window")

7. Minimize the RDP session for the domain controller and wait a few minutes for the AD accounts to be synchronized to Azure AD.

8. Sign in to the [Azure Portal](https://portal.azure.com/).

9. Type **Azure Active Directory** in the search field and select it from the list.

10. On the Azure Active Directory blade, under **Manage**, select **Users**.

11. Review the list of user account objects and confirm the test accounts have synchronized.  

    ![This image shows the list of users that you should see in Azure Active Directory that were synchronized from Active Directory with Azure AD Connect](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/adconnectsync.png "Synchronized users list")

    >**Note**: It can take up to 15 minutes for the Active Directory objects to be synchronized to the Azure AD tenant.


## Exercise 2: Create Azure AD groups for WVD

Duration:  30 minutes

In this exercise you will be working with groups in Azure Active Directory (Azure AD) to assist in managing access assignment to your application groups in WVD. The new ARM portal for WVD supports access assignment using Azure AD groups. This capability greatly simplifies access management. Groups will also be leveraged in this guide to manage
share permissions in Azure Files for FSLogix.

You will be creating three Azure AD groups to manage access to the different application groups; Personal, Pooled, and RemoteApp. For this guide we will only create a single group for RemoteApps, but in a production scenario it is more common to use separate groups based on the app or persona defined by the customer. Be sure to make note of the groups you create, as they will be used in later exercises.

It is also important to keep in mind that these groups can also originate from the Windows Active Directory environment and synchronize via Azure AD Connect. This will be another common scenario for customers that already have processes defined on-premises for group management.

**Additional Resources**

|                                                  |                                                              |
| ------------------------------------------------ | :----------------------------------------------------------: |
| Description                                      |                            Links                             |
| Create a basic group and add members in Azure AD | https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal |
| Azure AD Connect sync                            | https://docs.microsoft.com/en-us/azure/active-directory/hybrid/concept-azure-ad-connect-sync-user-and-contacts |
|                                                  |                                                              |

### Task 1: Creating Azure AD groups

1. Sign in to the [Azure Portal](https://portal.azure.com/).

2. At the top of the page, in the **Search resources** field, type **Azure Active Directory**. Select **Azure Active Directory** from the list.

3. On the Azure Active Directory page, select **Groups** on the left and select **+ New group**.

4. On the New Group page, fill in the following options and Select **Create**.

   -    **Group type:** Security

   -    **Group name:** WVD Pooled Desktop User

   -    **Membership type:** Assigned

   ![Create a new security group type and provide the WVD Pooled Desktop user for the group name.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/newGroup2.png "New Group Window")

5. Select **+ New group** again, fill in the following options and Select **Create**.

   -    **Group type:** Security

   -    **Group name:** WVD Remote App All Users

   -    **Membership type:** Assigned

   ![Create a new security group type and provide the WVD Remote App All users for the group name.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/newGroup1.png "New Group Window")

6. Select **+ New group** again, fill in the following options and Select **Create**.

   -    **Group type:** Security

   -    **Group name:** WVD Persistent Desktop User

   -    **Membership type:** Assigned

   ![Create a new security group type and provide the WVD Persistent Desktop user for the group name.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/newGroup3.png "New Group Window")

7. Confirm that the groups have been added by going to **Azure Active Directory**, selecting **Groups**.  Scroll down to the bottom of the list of groups and the three groups that you created should be listed.

   ![Go to Azure Active Directory Groups to view the list of groups.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/aadgroups.png "Azure Active Directory Groups")

   ![Scroll to the bottom of the list to view the three new groups that were created.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/aadnewgroups.png "Azure Active Directory Groups")

### Task 2: Assign users to groups

Now that the Azure AD groups are in place, we will assign users for testing. Once the groups are populated, we can leverage them for assigning access to WVD resources once they are created.

1. Sign in to the [Azure Portal](https://portal.azure.com/).

2. At the top of the page, in the **Search resources** field, type **Azure Active Directory**. Select **Azure Active Directory** from the list.

3. On the **Azure Active Directory** page, select **Groups** on the left and select the **WVD Persistent Desktop User** group.

4. Select **Members** and **+ Add Members**

   ![Add members to the persistent desktop user group from within the Azure AD blade.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/newMember.png "Azure AD blade")

5. In the search field, enter the name of a User to add **Select** to add them to the group.

6. Repeat steps 4-6 for the **WVD Pooled Desktop User** and **WVD Remote App All Users** groups.

   At this point you have three new Azure AD groups with members assigned. Make a note of the group names and accounts you added for use later in this guide. These groups will be used to assign access to WVD application groups.

   ![Here is the list of users that you should be adding to each of the groups.](/Users/neil/Documents/GitHub/MCW-Implementing-Windows-Virtual-Desktop-in-the-enterprise/Hands-on lab/images/aadwvdusers.png "Azure AD groups")


## 