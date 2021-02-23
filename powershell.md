## Set up your PowerShell environment

To get started with using the module, first install the [latest version of PowerShell Core](/powershell/scripting/install/installing-powershell#powershell-core). Windows Virtual Desktop cmdlets currently only work with PowerShell Core.

Next, you'll need to install the DesktopVirtualization module to use in your PowerShell session.

Run the following PowerShell cmdlet in elevated mode to install the module:

```powershell
Install-Module -Name Az.DesktopVirtualization
```

>[!NOTE]
> If this cmdlet doesn't work, try running it again with elevated permissions.

Next, run the following cmdlet to connect to Azure:

```powershell
Connect-AzAccount
```

>[!IMPORTANT]
>If you're connecting to the US Gov portal, run this cmdlet instead:
> 
> ```powershell
> Connect-AzAccount -EnvironmentName AzureUSGovernment
> ```

Signing into your Azure account requires a code that's generated when you run the Connect cmdlet. To sign in, go to <https://microsoft.com/devicelogin>, enter the code, then sign in using your Azure admin credentials.

```powershell
Account SubscriptionName TenantId Environment

------- ---------------- -------- -----------

Youradminupn subscriptionname AzureADTenantID AzureCloud
```

This will sign you directly into the subscription that is default for your admin credentials.

## Change the default subscription

If you want to change the default subscription after you've signed in, run this cmdlet:

```powershell
Select-AzSubscription -Subscription <preferredsubscriptionname>
```

You can also select one from a list using the Out-GridView cmdlet:

```powershell
Get-AzSubscription | Out-GridView -PassThru | Select-AzSubscription
```

When you select a new subscription to use, you don't need to specify that subscription's ID in cmdlets you run afterwards. For example, the following cmdlet retrieves a specific session host without needing the subscription ID:

```powershell
Get-AzWvdSessionHost -HostPoolName <hostpoolname> -Name <sessionhostname> -ResourceGroupName <resourcegroupname>
```

You can also change subscriptions on a per-cmdlet basis by adding the desired subscription name as a parameter. The next cmdlet is the same as the previous example, except with the subscription ID added as a parameter to change which subscription the cmdlet uses.

```powershell
Get-AzWvdSessionHost -HostPoolName <hostpoolname> -Name <sessionhostname> -ResourceGroupName <resourcegroupname> -SubscriptionId <subscriptionGUID>
```

## Get locations

The location parameter is mandatory for all **New-AzWVD** cmdlets that create new objects.

Run the following cmdlet to get a list of locations your subscription supports:

```powershell
Get-AzLocation
```

The output for **Get-AzLocation** will look like this:

```powershell
Location : eastasia

DisplayName : East Asia

Providers : {Microsoft.RecoveryServices, Microsoft.ManagedIdentity,
Microsoft.SqlVirtualMachine, microsoft.insightsΓÇª}

Location : southeastasia

DisplayName : Southeast Asia

Providers : {Microsoft.RecoveryServices, Microsoft.ManagedIdentity,
Microsoft.SqlVirtualMachine, microsoft.insightsΓÇª}

Location : centralus

DisplayName : Central US

Providers : {Microsoft.RecoveryServices, Microsoft.DesktopVirtualization,
Microsoft.ManagedIdentity, Microsoft.SqlVirtualMachineΓÇª}

Location : eastus

DisplayName : East US

Providers : {Microsoft.RecoveryServices, Microsoft.DesktopVirtualization,
Microsoft.ManagedIdentity, Microsoft.SqlVirtualMachineΓÇª}
```

Once you know your account's location, you can use it in a cmdlet. For example, here's a cmdlet that creates a host pool in the "southeastasia" location:

```powershell
New-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -WorkspaceName <workspacename> -Location “southeastasia”
```