---
title: Erstellen einer Web-App für den Dienstverwaltungsconnector
description: Erstellen Sie mithilfe eines automatisierten Skripts eine Service Manager-Web-App, um eine Verbindung mit dem ITSM-Connector in Azure herzustellen und die ITSM-Arbeitselemente zentral zu überwachen und zu verwalten.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 01/23/2018
ms.openlocfilehash: 16b48ac213168eaa58de5b0603025b2d48bb5ed5
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102041653"
---
# <a name="create-service-manager-web-app-using-the-automated-script"></a>Erstellen einer Service Manager-Web-App mithilfe des automatisierten Skripts

Führen Sie das folgende Skript aus, um die Web-App für Ihre Service Manager-Instanz zu erstellen. Weitere Informationen zur Service Manager-Verbindung finden Sie hier: [Service Manager-Web-App](./itsmc-connections-scsm.md)

Führen Sie das Skript nach Bereitstellung der folgenden erforderlichen Details aus:

- Details zum Azure-Abonnement
- Ressourcengruppenname
- Standort
- Details zum Service Manager-Server (Servername, Domäne, Benutzername und Kennwort)
- Präfix des Namens der Website für Ihre Web-App
- Service Bus-Namespace.

Das Skript erstellt die Web-App mit dem Namen, den Sie angegeben haben (neben einigen zusätzlichen Zeichenfolgen zum Sicherstellen der Eindeutigkeit). Es generiert die **Web-App-URL**, **Client-ID** und das **Clientgeheimnis**.

Speichern Sie diese Werte, die Sie benötigen, wenn Sie eine Verbindung mit dem IT Service Management-Connector herstellen.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

 Windows Management Framework 5.0 oder höher.
Windows 10 verfügt standardmäßig über Version 5.1. Sie können das Framework [hier](https://www.microsoft.com/download/details.aspx?id=50395) herunterladen:

Führen Sie das folgende Skript aus:

```powershell
####################################
# User Configuration Section Begins
####################################

# Subscription name in Azure account. Check in Azure Portal.
$azureSubscriptionName = ""

# Resource group name for resource deployment. Could be an existing resource group or a new one to be created.
$resourceGroupName = ""

# Location for existing resource group or new resource group deployment
################################### List of available regions #################################################
# centralus,eastasia,southeastasia,eastus,eastus2,westus,westus2,northcentralus,southcentralus,westcentralus,
# northeurope,westeurope,japaneast,japanwest,brazilsouth,australiasoutheast,australiaeast,westindia,southindia,
# centralindia,canadacentral,canadaeast,uksouth,ukwest.
###############################################################################################################
$location = ""

# Service Manager Authentication Settings
$serverName = ""
$domain = ""
$username = ""
$password = ""


# Azure site Name Prefix. Default is "smoc". It can be configured to any desired value.
$siteNamePrefix = ""

# Service Bus namespace. Please provide an already existing service bus namespace.
# If it doesn't exist, a new one will be created with name $siteName + "sbn" which can also be later reused for any other hybrid connections.
$serviceName = ""

##################################
# User Configuration Section Ends
##################################

################
# Installations
################

# Allowing the execution of the script for current user.  
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

Write-Host "Checking for required modules..."
if(!(Get-PackageProvider -Name NuGet))
{
   Write-Host "Installing NuGet Package Provider..."
   Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Scope CurrentUser -Force -WarningAction SilentlyContinue
}
$module = Get-Module -ListAvailable -Name Az

if(!$module -or ($module[0].Version.Major -lt 1))
{
    Write-Host "Installing Az Module..."  
    try
    {
        # In case of Win 10 Anniversary update
        Install-Module Az -MinimumVersion 1.0 -Scope CurrentUser -Force -WarningAction SilentlyContinue -AllowClobber
    }
    catch
    {
        Install-Module Az -MinimumVersion 1.0 -Scope CurrentUser -Force -WarningAction SilentlyContinue
    }

}

Write-Host "Requirement check complete!!"

#############
# Parameters
#############

$errorActionPreference = "Stop"

$templateUri = "https://raw.githubusercontent.com/Azure/SMOMSConnector/master/azuredeploy.json"

if(!$siteNamePrefix)
{
    $siteNamePrefix = "smoc"
}

Connect-AzAccount

$context = Set-AzContext -SubscriptionName $azureSubscriptionName -WarningAction SilentlyContinue

$resourceProvider = Get-AzResourceProvider -ProviderNamespace Microsoft.Web

if(!$resourceProvider -or $resourceProvider[0].RegistrationState -ne "Registered")
{
    try
    {
        Write-Host "Registering Microsoft.Web Resource Provider"
        Register-AzResourceProvider -ProviderNamespace Microsoft.Web
    }
    catch
    {
        Write-Host "Failed to Register Microsoft.Web Resource Provider. Please register it in Azure Portal."
        exit
    }
}
do
{
    $rand = Get-Random -Maximum 32000

    $siteName = $siteNamePrefix + $rand

    $resource = Get-AzResource -Name $siteName -ResourceType Microsoft.Web/sites

}while($resource)

$azureSite = "https://"+$siteName+".azurewebsites.net"

##############
# MAIN Begins
##############

# Web App Deployment
####################

$tenant = $context.Tenant.Id
if(!$tenant)
{
    #For backward compatibility with older versions
    $tenant = $context.Tenant.TenantId
}
try
{
    Get-AzResourceGroup -Name $resourceGroupName
}
catch
{
    New-AzResourceGroup -Location $location -Name $resourceGroupName
}

Write-Output "Web App Deployment in progress...."

New-AzResourceGroupDeployment -TemplateUri $templateUri -siteName $siteName -ResourceGroupName $resourceGroupName

Write-Output "Web App Deployed successfully!!"

# AAD Authentication
####################

Add-Type -AssemblyName System.Web

$secret = [System.Web.Security.Membership]::GeneratePassword(30,2).ToString()
$clientSecret = $secret | ConvertTo-SecureString -AsPlainText -Force


try
{

    Write-Host "Creating AzureAD application..."

    $adApp = New-AzADApplication -DisplayName $siteName -HomePage $azureSite -IdentifierUris $azureSite -Password $clientSecret

    Write-Host "AzureAD application created successfully!!"
}
catch
{
    # Delete the deployed web app if Azure AD application fails
    Remove-AzResource -ResourceGroupName $resourceGroupName -ResourceName $siteName -ResourceType Microsoft.Web/sites -Force

    Write-Host "Failure occurred in Azure AD application....Try again!!"

    exit

}

$clientId = $adApp.ApplicationId

$servicePrincipal = New-AzADServicePrincipal -ApplicationId $clientId

# Web App Configuration
#######################
try
{

    Write-Host "Configuring deployed Web-App..."
    $webApp = Get-AzWebAppSlot -ResourceGroupName $resourceGroupName -Name $siteName -Slot production -WarningAction SilentlyContinue

    $appSettingList = $webApp.SiteConfig.AppSettings

    $appSettings = @{}
    ForEach ($item in $appSettingList) {
        $appSettings[$item.Name] = $item.Value
    }

    $appSettings['ida:Tenant'] = $tenant
    $appSettings['ida:Audience'] = $azureSite
    $appSettings['ida:ServerName'] = $serverName
    $appSettings['ida:Domain'] = $domain
    $appSettings['ida:Username'] = $userName
    $appSettings['ida:WhitelistedClientId'] = $clientId

    $connStrings = @{}
    $kvp = @{"Type"="Custom"; "Value"=$password}
    $connStrings['ida:Password'] = $kvp

    Set-AzWebAppSlot -ResourceGroupName $resourceGroupName -Name $siteName -AppSettings $appSettings -ConnectionStrings $connStrings -Slot production -WarningAction SilentlyContinue

}
catch
{
    Write-Host "Web App configuration failed. Please ensure all values are provided in Service Manager Authentication Settings in User Configuration Section"

    # Delete the AzureRm AD Application if configuration fails
    Remove-AzADApplication -ObjectId $adApp.ObjectId -Force

    # Delete the deployed web app if configuration fails
    Remove-AzResource -ResourceGroupName $resourceGroupName -ResourceName $siteName -ResourceType Microsoft.Web/sites -Force

    exit
}


# Relay Namespace
###################

if(!$serviceName)
{
    $serviceName = $siteName + "sbn"
}

$resourceProvider = Get-AzResourceProvider -ProviderNamespace Microsoft.Relay

if(!$resourceProvider -or $resourceProvider[0].RegistrationState -ne "Registered")
{
    try
    {
        Write-Host "Registering Microsoft.Relay Resource Provider"
        Register-AzResourceProvider -ProviderNamespace Microsoft.Relay
    }
    catch
    {
        Write-Host "Failed to Register Microsoft.Relay Resource Provider. Please register it in Azure Portal."
    }
}

$resource = Get-AzResource -Name $serviceName -ResourceType Microsoft.Relay/namespaces

if(!$resource)
{
    $serviceName = $siteName + "sbn"
    $properties = @{
        "sku" = @{
            "name"= "Standard"
            "tier"= "Standard"
            "capacity"= 1
         }
    }
    try
    {
        Write-Host "Creating Service Bus namespace..."
        New-AzResource -ResourceName $serviceName -Location $location -PropertyObject $properties -ResourceGroupName $resourceGroupName -ResourceType Microsoft.Relay/namespaces -ApiVersion 2016-07-01 -Force
    }
    catch
    {
        $err = $TRUE
        Write-Host "Creation of Service Bus Namespace failed...Please create it manually from Azure Portal.`n"
    }

}

Write-Host "Note: Please Configure Hybrid connection in the Networking section of the web application in Azure Portal to link to the on-premises system.`n"
Write-Host "App Details"
Write-Host "============"
Write-Host "App Name:"  $siteName
Write-Host "Client Id:"  $clientId
Write-Host "Client Secret:"  $secret
Write-Host "URI:"  $azureSite
if(!$err)
{
    Write-Host "ServiceBus Namespace:"  $serviceName  
}
```

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Beheben von Problemen bei der Service Manager-Web-App-Bereitstellung

-   Stellen Sie bei Problemen mit der Web-App-Bereitstellung sicher, dass Sie im angegebenen Abonnement über ausreichende Berechtigungen zum Erstellen/Bereitstellen von Ressourcen verfügen.
-   Wenn die Fehlermeldung **Objektverweis ist nicht auf eine Instanz eines Objekts festgelegt** angezeigt wird, während Sie das [Skript](itsmc-service-manager-script.md) ausführen, vergewissern Sie sich, dass Sie im Abschnitt **Benutzerkonfiguration** gültige Werte eingegeben haben.
-   Wenn Sie den Service Bus-Relaynamespace nicht erstellen können, stellen Sie sicher, dass der erforderliche Ressourcenanbieter im Abonnement registriert ist. Wenn er nicht registriert ist, erstellen Sie den Service Bus-Relaynamespace manuell über das Azure-Portal. Sie können ihn auch beim [Erstellen der Hybridverbindung](./itsmc-connections-scsm.md#configure-the-hybrid-connection) über das Azure-Portal erstellen.

## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren der Hybridverbindung](./itsmc-connections-scsm.md#configure-the-hybrid-connection).
