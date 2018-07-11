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
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="54efb-103">Komut satırı araçları ile Azure'da ASP.NET Core uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="54efb-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="54efb-104">Tarafından [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="54efb-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="54efb-105">Bu öğreticide komut satırı araçlarını kullanarak derlemek ve Microsoft Azure App Service'e bir ASP.NET Core uygulaması dağıtmak nasıl gösterilecek.</span><span class="sxs-lookup"><span data-stu-id="54efb-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="54efb-106">İşiniz bittiğinde, yerleşik ASP.NET Core web uygulamasını Azure App Service Web uygulaması barındırılan bir Razor sayfaları sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="54efb-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="54efb-107">Bu öğretici, Windows komut satırı araçlarını kullanarak yazılır, ancak macOS ve Linux ortamları, de uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="54efb-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="54efb-108">Bu öğreticide, şunların nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="54efb-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="54efb-109">Azure CLI kullanarak bir Azure App Service Web sitesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="54efb-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="54efb-110">Azure App Service'e Git komut satırı aracını kullanarak ASP.NET Core uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="54efb-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54efb-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="54efb-111">Prerequisites</span></span>

<span data-ttu-id="54efb-112">Bu öğreticiyi tamamlamak için ihtiyacınız olacak:</span><span class="sxs-lookup"><span data-stu-id="54efb-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="54efb-113">A [Microsoft Azure aboneliği](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="54efb-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="54efb-114">[Git](https://www.git-scm.com/) komut satırı istemcisi</span><span class="sxs-lookup"><span data-stu-id="54efb-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="54efb-115">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="54efb-115">Create a web app</span></span>

<span data-ttu-id="54efb-116">Web uygulaması için yeni bir dizin oluşturmak, yeni bir ASP.NET Core Razor sayfalar uygulaması oluşturma ve ardından Web sitesini yerel olarak çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="54efb-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="54efb-117">Windows</span><span class="sxs-lookup"><span data-stu-id="54efb-117">Windows</span></span>](#tab/windows)

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

# <a name="othertabother"></a>[<span data-ttu-id="54efb-118">Diğer</span><span class="sxs-lookup"><span data-stu-id="54efb-118">Other</span></span>](#tab/other)

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

<span data-ttu-id="54efb-120">Uygulamayı test etme göz atarak `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="54efb-120">Test the app by browsing to `http://localhost:5000`.</span></span>

![Yerel olarak çalışan bir Web sitesi](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="54efb-122">Azure App Service örneği oluşturma</span><span class="sxs-lookup"><span data-stu-id="54efb-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="54efb-123">Kullanarak [Azure Cloud Shell](/azure/cloud-shell/quickstart), bir kaynak grubu, App Service planı ve App Service web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="54efb-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="54efb-124">Dağıtımdan önce aşağıdaki komutu kullanarak hesap düzeyinde dağıtım kimlik bilgilerini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="54efb-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="54efb-125">Git kullanarak uygulamayı dağıtmak için dağıtım URL'si gereklidir.</span><span class="sxs-lookup"><span data-stu-id="54efb-125">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="54efb-126">Bu gibi URL'sini alın.</span><span class="sxs-lookup"><span data-stu-id="54efb-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="54efb-127">Görüntülenen URL sonu Not `.git`.</span><span class="sxs-lookup"><span data-stu-id="54efb-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="54efb-128">Sonraki adımda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="54efb-128">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="54efb-129">Git kullanarak uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="54efb-129">Deploy the app using Git</span></span>

<span data-ttu-id="54efb-130">Git kullanarak yerel makinenizde dağıtmaya hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="54efb-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="54efb-131">Satır sonları hakkında Git herhangi bir uyarı yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="54efb-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="54efb-132">Windows</span><span class="sxs-lookup"><span data-stu-id="54efb-132">Windows</span></span>](#tab/windows)

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

# <a name="othertabother"></a>[<span data-ttu-id="54efb-133">Diğer</span><span class="sxs-lookup"><span data-stu-id="54efb-133">Other</span></span>](#tab/other)

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

<span data-ttu-id="54efb-134">Git, daha önce ayarlanan dağıtım kimlik bilgilerini ister.</span><span class="sxs-lookup"><span data-stu-id="54efb-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="54efb-135">Kimlik doğrulandıktan sonra uygulama uzak konuma gönderildi, oluşturulan ve dağıtılan.</span><span class="sxs-lookup"><span data-stu-id="54efb-135">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Git dağıtımı çıkış](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="54efb-137">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="54efb-137">Test the app</span></span>

<span data-ttu-id="54efb-138">Uygulamayı test etme göz atarak `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="54efb-138">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="54efb-139">Cloud Shell (veya Azure CLI) adresini görüntülemek için aşağıdakini kullanın:</span><span class="sxs-lookup"><span data-stu-id="54efb-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Azure'da çalışan uygulama](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="54efb-141">Temizleme</span><span class="sxs-lookup"><span data-stu-id="54efb-141">Clean up</span></span>

<span data-ttu-id="54efb-142">Uygulamayı test etme ve kaynaklar ve kod İnceleme işiniz bittiğinde web uygulaması ve planı kaynak grubunu silerek silin.</span><span class="sxs-lookup"><span data-stu-id="54efb-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="54efb-143">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="54efb-143">Next steps</span></span>

<span data-ttu-id="54efb-144">Bu öğreticide şunları öğrendiniz: nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="54efb-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="54efb-145">Azure CLI kullanarak bir Azure App Service Web sitesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="54efb-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="54efb-146">Azure App Service'e Git komut satırı aracını kullanarak ASP.NET Core uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="54efb-146">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="54efb-147">Ardından, CosmosDB kullanan mevcut bir web uygulamasını dağıtmak için komut satırını kullanmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="54efb-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="54efb-148">Azure'da .NET Core ile komut satırından dağıtma</span><span class="sxs-lookup"><span data-stu-id="54efb-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
