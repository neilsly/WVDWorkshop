# Module 11: Connect to WVD with the web client

Duration:  15 minutes

In this exercise we are going to walk through connecting to your WVD environment using the HTML5 web client and validating your deployment. The following operating systems and browsers are officially supported:

- Microsoft Edge (All Versions)
- Internet Explorer (Vers 11 or later)
- Apple Safari 
- Mozilla Firefox on Windows, macOS, Linux (Version 55 or later)
- Google Chrome on Windows, macOS, Linux, Chrome OS

**Additional Resources**

There are multiple clients available for you to access WVD resources. Refer to the following Docs for more information about each client:
  |              |            |  
|----------|:-------------:|
| Description | Links |
|Connect with the Windows Desktop Client |  https://docs.microsoft.com/en-us/azure/virtual-desktop/connect-windows-7-and-10 |
| Connect with the HTML5 web client |  https://docs.microsoft.com/en-us/azure/virtual-desktop/connect-web |
| Connect with the Android client | https://docs.microsoft.com/en-us/azure/virtual-desktop/connect-android |
| Connect with the macOS client |  https://docs.microsoft.com/en-us/azure/virtual-desktop/connect-macos |
| Connect with the iOS client | https://docs.microsoft.com/en-us/azure/virtual-desktop/connect-ios |
  |              |            | 

### Task 1: Connecting with the HTML5 web client

1.  Open a supported web browser.

2.  Navigate to the https://rdweb.wvd.microsoft.com/arm/webclient.

    >**Note**: You will be asked to login when you access the above URL.  The credentials that you use are those that you created earlier.

3.  Sign in using a synchronized identity that has been assigned to an application group.

    >**Note**: If you added the **AAD DC Administrators** to the groups in the previous exercises, you will be able to use your Global Administrator information.  This **must** be a user that is synchronized with the AD DS with Azure AD Connect.  To verify, go to Azure Active Directory users and verify the directory sync users.

    ![Here is where you would verify that a user is syncronized with the domain controller.](images/confirmsync.png)

    ![Select to use another account to enter the login email.](images/useanotheraccount.png)

    ![Enter the email address for the workshop Azure tenant.](images/signinwithtenantadmin.png)

    ![Enter the password for the username that you entered.](images/enterpw.png)

4.  Select an available resource from the web client. In this example we will connect to a host pool containing pooled desktop.

    ![Once you login to the portal, you will see the apps that are available for you to use.](images/appsavailable.png)

5.  On the **Access local resources** prompt, review the available options for and Select **Allow**.

    ![Here you will select the default desktop and allow local resources.](images/allowlocal.png)

6.  On the **Enter your credentials** prompt, sign in using the same account from Step 3 and Select **Submit**. 
   
    >**Note**: The username and password to login to the WVD desktop will be credentials from the domain controller user name and password created upon initial deployment.  If you need the user email, RDP into the domain controller VM and find the user in the **Active Directory Users and Groups** and **OrgUsers**.

    ![On the domain controller VM, you can find the username here.](images/dcusername.png)

    ![Enter the username from the domain controller and the password created during initial deployment.](images/dccreds.png)

7.  Once connected, validate the components relative to your configuration. The desktop should show icons for Microsoft Edge and Microsoft Teams.  When you go to the Windows start menu, you can find the Office applications.

    ![The desktop WVD image should look like this.](images/wvddesktopimage.png)


**Troubleshooting**

**Web client stops responding or disconnects**

Try connecting using another browser or client.

If issues continue even after you\'ve switched browsers, the problem may not be with your browser, but with your network. We recommend you contact network support.

**Web client keeps prompting for credentials**

If the Web client keeps prompting for credentials, follow these instructions:

1.  Confirm the web client URL is correct.

2.  Confirm that the credentials you\'re using are for the Windows Virtual Desktop environment tied to the URL.

3.  Clear browser cookies.

4.  Clear browser cache.

5.  Open your browser in Private mode.
