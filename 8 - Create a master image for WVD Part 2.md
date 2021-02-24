# Step 8: Create a master image for WVD (Part 2)

Duration: 30 minutes

>**Note: This exercise is a continuation of Step 6 - Create  master image for WVD Part 1.  Please ensure you have completed those steps prior to performing these.


1. After the script has completed, select the Window start icon and note that Office, Microsoft Edge Chromium, and Microsoft Teams have been installed.

     ![Here you can view the newly installed applications.](images/newapplications.png)

2. Once the script has completed execution, complete these final tasks:

- Delete the C:\\Customizations directory.

- Delete the .zip file on your desktop.

- Empty the Recycle Bin.

- Copy the C:\\Windows\\Logs\\ImagePrep\\LGPO directory to your local workstation.

- Reboot the VM.

     ![After the image preparation is complete, delete the downloaded files and empty the recycle bin](images/deletescripts.png)

     ![Navigate to the Windows start menu and reboot the Windows 10 VM.](images/win10reboot.png)

### Task 2: Run Sysprep

1. After the VM has rebooted, reconnect your RDP session and sign in.

2. Open an administrative command prompt.

3. Navigate to: **C:\\Windows\\System32\\Sysprep**.

   ```
   cd C:\Windows\System32\Sysprep
   ```

4. Run the following command to sysprep the VM and shutdown:

   ```
   sysprep.exe /oobe /generalize /shutdown
   ```

The system will automatically shut down and disconnect your RDP session.

### Task 3: Create a managed image from the Master Image VM

1. Sign in to the [Azure Portal](https://portal.azure.com/).

2. At the top of the page, in the **Search resources** field, type **virtual machines**. Select **Virtual machines** from the list.

   ![From the Azure portal search bar, search for virtual machines and select the service.](images/searchvm.png "Search Virtual Machines")

3. On the Virtual machines blade, locate the VM you used for your master image and **Select** on the name.

4. On the Overview blade for your VM, confirm the **Status** shows **Stopped**. Select **Stop** in the menu bar to move it to a deallocated state.

   ![This is what you will see if the VM is running.  Please select stop to deallocate the VM.](images/vmrunning.png)

   ![When the VM has been stopped, it will show the status of stopped, deallocated.](images/vmstopped.png)

5. Once complete, Select **Capture** in the menu bar.

   ![Once the VM is stopped, you can select capture to capture the VM image.](images/vmcapture.png)

6. On the Create image blade, fill in the required fields and Select **Create**.

 - Under **Target Image Definition** click **Create New**, input a name for **Image Definition name** and click **Ok** at the bottom

    ![This will display the Create Image blade in Azure.](images/imgdef.png "Create Image blade in Azure")

 - In **Version Number** input **1.0.0** 

   ![This will display the Create Image blade in Azure.](images/w10VMImage.png "Create Image blade in Azure")

7. Once complete, type **images** in the **Search resources field** at the top of the page. Select **Images** from the list.

8. On the Images blade, locate your image and **Select** on the name.

   ![When you search on images, this is the icon that you will need to select.](images/findimage.png)

9. On the Overview blade for your image, make note of the **Name** field and **Resource group** field. These attributes are needed when you provision your host pools.

   ![This is the information that you need to note for the name and resource group.](images/newimage.png)

### Task 4: Provision a Host Pool with a custom image

1. To start provisioning a host pool with your custom image, follow the instructions in [Exercise 6](#exercise-6-create-a-host-pool-and-assign-pooled-remote-apps).

2. When you get to step 5 to configure **Virtual machine settings**, select **Browse all images and disks** and then select the tab option for **My Items** to select the image that was created.

   ![This is where you will find your custom image to add to the host pool.](images/hostpoolcustom.png)

