.NET CORE 2.0 Example Web App
------------------------------

To deploy this Web App to Azure:

First log in and select a subscription.

```
Login-AzureRmAccount -Environment <AzureCloud or AzureUSGovernment>
Select-AzureRmSubscription -SubscriptionName "<NAME OF SUBSCRIPTION>"
```

Then deploy Web App:

```
$rgName = "dotnetTest"
$webappName = "example" + $(Get-Random).ToString()
$location = "<LOCATION>"

$rg = New-AzureRmResourceGroup -Name $rgName -Location $location
$asp = New-AzureRmAppServicePlan -Name $webAppName -ResourceGroupName $rgName -Location $location -Tier Standard
$app = New-AzureRmWebApp -Name $webappName -ResourceGroupName $rg.ResourceGroupName -Location $location -AppServicePlan $asp.Id

# Defines a variable for a dotnet get started web app repository location
$gitrepo="https://github.com/hansenms/DotNetCore20Example.git"

# Configure GitHub deployment from your GitHub repo and deploy once to web app.
$PropertiesObject = @{ repoUrl = "$gitrepo"; branch = "master"; isManualIntegration = "true"; }

Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName $rg.ResourceGroupName -ResourceType Microsoft.Web/sites/sourcecontrols -ResourceName $webappName/web -ApiVersion 2015-08-01 -Force

$appUrl = "https://" + $app.DefaultHostName

Write-Host "Browse App on: $appUrl"
```

