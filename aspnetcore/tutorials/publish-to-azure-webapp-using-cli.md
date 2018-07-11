---
title: Komut satırı araçları ile Azure'da ASP.NET Core uygulaması yayımlama
author: camsoper
description: Azure App Service'e Git komut satırı istemcisini kullanarak bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 526ceef469d473706f39cdc3ee645280e99315b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126966"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a>Komut satırı araçları ile Azure'da ASP.NET Core uygulaması yayımlama

Tarafından [Cam Soper](https://twitter.com/camsoper)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

Bu öğreticide komut satırı araçlarını kullanarak derlemek ve Microsoft Azure App Service'e bir ASP.NET Core uygulaması dağıtmak nasıl gösterilecek. İşiniz bittiğinde, yerleşik ASP.NET Core web uygulamasını Azure App Service Web uygulaması barındırılan bir Razor sayfaları sahip olacaksınız. Bu öğretici, Windows komut satırı araçlarını kullanarak yazılır, ancak macOS ve Linux ortamları, de uygulanabilir.

Bu öğreticide, şunların nasıl yapılır:

> [!div class="checklist"]
> * Azure CLI kullanarak bir Azure App Service Web sitesi oluşturun
> * Azure App Service'e Git komut satırı aracını kullanarak ASP.NET Core uygulaması dağıtma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için ihtiyacınız olacak:

* A [Microsoft Azure aboneliği](https://azure.microsoft.com/free/)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Git](https://www.git-scm.com/) komut satırı istemcisi

## <a name="create-a-web-app"></a>Bir web uygulaması oluşturma

Web uygulaması için yeni bir dizin oluşturmak, yeni bir ASP.NET Core Razor sayfalar uygulaması oluşturma ve ardından Web sitesini yerel olarak çalıştırın.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[Diğer](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![Komut satırı çıkışı](publish-to-azure-webapp-using-cli/_static/new_prj.png)

Uygulamayı test etme göz atarak `http://localhost:5000`.

![Yerel olarak çalışan bir Web sitesi](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a>Azure App Service örneği oluşturma

Kullanarak [Azure Cloud Shell](/azure/cloud-shell/quickstart), bir kaynak grubu, App Service planı ve App Service web uygulaması oluşturun.

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

Dağıtımdan önce aşağıdaki komutu kullanarak hesap düzeyinde dağıtım kimlik bilgilerini ayarlayın:

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

Git kullanarak uygulamayı dağıtmak için dağıtım URL'si gereklidir. Bu gibi URL'sini alın.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

Görüntülenen URL sonu Not `.git`. Sonraki adımda kullanılır.

## <a name="deploy-the-app-using-git"></a>Git kullanarak uygulamayı dağıtma

Git kullanarak yerel makinenizde dağıtmaya hazırsınız.

> [!NOTE]
> Satır sonları hakkında Git herhangi bir uyarı yoksayılabilir.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[Diğer](#tab/other)

```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```

---

Git, daha önce ayarlanan dağıtım kimlik bilgilerini ister. Kimlik doğrulandıktan sonra uygulama uzak konuma gönderildi, oluşturulan ve dağıtılan.

![Git dağıtımı çıkış](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a>Uygulamayı test etme

Uygulamayı test etme göz atarak `https://<web app name>.azurewebsites.net`. Cloud Shell (veya Azure CLI) adresini görüntülemek için aşağıdakini kullanın:

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Azure'da çalışan uygulama](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>Temizleme

Uygulamayı test etme ve kaynaklar ve kod İnceleme işiniz bittiğinde web uygulaması ve planı kaynak grubunu silerek silin.

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz: nasıl yapılır:

> [!div class="checklist"]
> * Azure CLI kullanarak bir Azure App Service Web sitesi oluşturun
> * Azure App Service'e Git komut satırı aracını kullanarak ASP.NET Core uygulaması dağıtma

Ardından, CosmosDB kullanan mevcut bir web uygulamasını dağıtmak için komut satırını kullanmayı öğrenin.

> [!div class="nextstepaction"]
> [Azure'da .NET Core ile komut satırından dağıtma](/dotnet/azure/dotnet-quickstart-xplat)
