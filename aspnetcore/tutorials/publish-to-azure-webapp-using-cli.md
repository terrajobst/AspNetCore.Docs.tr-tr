---
title: ASP.NET Core uygulama için Azure komut satırı araçları ile yayımlama
author: camsoper
description: Azure App Service'e Git komut satırı İstemcisi'ni kullanarak bir ASP.NET Core uygulamayı yayımlamak öğrenin.
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: a393e9dc80fdbf646c52342b13e3cda5c725aa93
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688263"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="908cc-103">ASP.NET Core uygulama için Azure komut satırı araçları ile yayımlama</span><span class="sxs-lookup"><span data-stu-id="908cc-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="908cc-104">Tarafından [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="908cc-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="908cc-105">Bu öğretici komut satırı araçlarını kullanarak nasıl oluşturulacağı ve ASP.NET Core uygulama Microsoft Azure App Service'e dağıtmak gösterir.</span><span class="sxs-lookup"><span data-stu-id="908cc-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="908cc-106">Tamamlandığında, ASP.NET Core yerleşik web uygulamasını Azure App Service Web uygulaması barındırılan bir Razor sayfalarının sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="908cc-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="908cc-107">Bu öğretici Windows komut satırı araçları kullanılarak yazılmış ancak macOS hem de Linux ortamlarında uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="908cc-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="908cc-108">Bu öğreticide, bilgi nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="908cc-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="908cc-109">Azure CLI kullanarak Azure App Service Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="908cc-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="908cc-110">Azure App Service'e Git komut satırı aracını kullanarak ASP.NET Core uygulama dağıtma</span><span class="sxs-lookup"><span data-stu-id="908cc-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="908cc-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="908cc-111">Prerequisites</span></span>

<span data-ttu-id="908cc-112">Bu öğreticiyi tamamlamak için ihtiyacınız vardır:</span><span class="sxs-lookup"><span data-stu-id="908cc-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="908cc-113">A [Microsoft Azure aboneliği](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="908cc-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="908cc-114">[Git](https://www.git-scm.com/) komut satırı istemcisi</span><span class="sxs-lookup"><span data-stu-id="908cc-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="908cc-115">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="908cc-115">Create a web app</span></span>

<span data-ttu-id="908cc-116">Web uygulaması için yeni bir dizin oluşturma, yeni bir ASP.NET Core Razor sayfalarının uygulaması oluşturma ve Web sitesi yerel olarak çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="908cc-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="908cc-117">Windows</span><span class="sxs-lookup"><span data-stu-id="908cc-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

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

# <a name="othertabother"></a>[<span data-ttu-id="908cc-118">Diğer</span><span class="sxs-lookup"><span data-stu-id="908cc-118">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

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

<span data-ttu-id="908cc-120">Göz atarak uygulamayı test etme `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="908cc-120">Test the app by browsing to `http://localhost:5000`.</span></span>

![Yerel olarak çalışan Web sitesi](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="908cc-122">Azure App Service örneği oluşturma</span><span class="sxs-lookup"><span data-stu-id="908cc-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="908cc-123">Kullanarak [Azure bulut Kabuk](/azure/cloud-shell/quickstart), bir kaynak grubu, uygulama hizmeti planı ve bir App Service web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="908cc-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="908cc-124">Dağıtım öncesinde aşağıdaki komutu kullanarak hesap düzeyinde dağıtım kimlik bilgilerini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="908cc-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="908cc-125">Bir dağıtım URL'si uygulamasını Git kullanarak dağıtmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="908cc-125">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="908cc-126">Bu gibi URL'sini alın.</span><span class="sxs-lookup"><span data-stu-id="908cc-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="908cc-127">Biten görüntülenen URL'yi Not `.git`.</span><span class="sxs-lookup"><span data-stu-id="908cc-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="908cc-128">Sonraki adımda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="908cc-128">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="908cc-129">Git kullanarak uygulamayı dağıtın</span><span class="sxs-lookup"><span data-stu-id="908cc-129">Deploy the app using Git</span></span>

<span data-ttu-id="908cc-130">Yerel makinenizden Git kullanarak dağıtmak hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="908cc-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="908cc-131">Satır sonları ilgili Git tüm uyarılar yoksaymak güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="908cc-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="908cc-132">Windows</span><span class="sxs-lookup"><span data-stu-id="908cc-132">Windows</span></span>](#tab/windows)

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

# <a name="othertabother"></a>[<span data-ttu-id="908cc-133">Diğer</span><span class="sxs-lookup"><span data-stu-id="908cc-133">Other</span></span>](#tab/other)

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

<span data-ttu-id="908cc-134">Git önceki ayarlanan dağıtım kimlik bilgilerini ister.</span><span class="sxs-lookup"><span data-stu-id="908cc-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="908cc-135">Kimlik doğrulandıktan sonra uygulama uzak konuma gönderilir, oluşturulur ve dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="908cc-135">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Git dağıtımı çıktı](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="908cc-137">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="908cc-137">Test the app</span></span>

<span data-ttu-id="908cc-138">Göz atarak uygulamayı test etme `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="908cc-138">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="908cc-139">Bulut Kabuğu (veya Azure CLI) adresini görüntülemek için aşağıdakini kullanın:</span><span class="sxs-lookup"><span data-stu-id="908cc-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Azure'da çalışan uygulama](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="908cc-141">Temizleme</span><span class="sxs-lookup"><span data-stu-id="908cc-141">Clean up</span></span>

<span data-ttu-id="908cc-142">Uygulamayı test etme ve kod ve kaynakları İnceleme tamamlandığında, web app ve planı kaynak grubunu silerek silin.</span><span class="sxs-lookup"><span data-stu-id="908cc-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="908cc-143">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="908cc-143">Next steps</span></span>

<span data-ttu-id="908cc-144">Bu öğreticide, öğrenilen nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="908cc-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="908cc-145">Azure CLI kullanarak Azure App Service Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="908cc-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="908cc-146">Azure App Service'e Git komut satırı aracını kullanarak ASP.NET Core uygulama dağıtma</span><span class="sxs-lookup"><span data-stu-id="908cc-146">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="908cc-147">Ardından, CosmosDB kullanan mevcut bir web uygulamasına dağıtmak için komut satırını kullanmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="908cc-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="908cc-148">.NET Core ile komut satırından Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="908cc-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
