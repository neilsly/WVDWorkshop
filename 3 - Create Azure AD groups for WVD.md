## Step 3: Create Azure AD groups for WVD

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