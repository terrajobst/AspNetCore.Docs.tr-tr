---
title: "Komut satırı araçlarını kullanarak Azure ASP.NET Core uygulama yayımlama | Microsoft Docs"
description: "Azure App Service'e Git komut satırı İstemcisi'ni kullanarak bir ASP.NET Core uygulamayı yayımlamak öğrenin."
services: multiple
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: de05c1688d7de6126434395042103d803ee3064e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="b3fef-103">Azure uygulama hizmeti komut satırından bir ASP.NET Core uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="b3fef-103">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="b3fef-104">Tarafından [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="b3fef-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="b3fef-105">Bu öğretici komut satırı araçlarını kullanarak oluşturmak ve Microsoft Azure App Service ASP.NET Core uygulama dağıtmak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="b3fef-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="b3fef-106">Tamamlandığında, ASP.NET MVC Azure App Service Web uygulaması barındırılan çekirdek yerleşik bir web uygulamasına sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b3fef-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="b3fef-107">Bu öğretici Windows komut satırı araçları kullanılarak yazılmış ancak macOS hem de Linux ortamlarında uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fef-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="b3fef-108">Bu öğreticide, bilgi nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="b3fef-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b3fef-109">Azure CLI kullanarak Azure App Service Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="b3fef-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="b3fef-110">Azure App Service Git komut satırı aracını kullanarak bir ASP.NET Core uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="b3fef-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b3fef-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b3fef-111">Prerequisites</span></span>

<span data-ttu-id="b3fef-112">Bu öğreticiyi tamamlamak için ihtiyacınız vardır:</span><span class="sxs-lookup"><span data-stu-id="b3fef-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="b3fef-113">A [Microsoft Azure aboneliği](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="b3fef-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="b3fef-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3fef-114">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="b3fef-115">[Git](https://www.git-scm.com/) komut satırı istemcisi</span><span class="sxs-lookup"><span data-stu-id="b3fef-115">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="b3fef-116">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="b3fef-116">Create a web application</span></span>

<span data-ttu-id="b3fef-117">Web uygulaması için yeni bir dizin oluşturma, yeni bir ASP.NET Core MVC uygulaması oluşturma ve Web sitesi yerel olarak çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b3fef-117">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b3fef-118">Windows</span><span class="sxs-lookup"><span data-stu-id="b3fef-118">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="b3fef-119">Diğer</span><span class="sxs-lookup"><span data-stu-id="b3fef-119">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Komut satırı çıkışı](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="b3fef-121">Http://localhost: 5000 için göz atarak uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="b3fef-121">Test the application by browsing to http://localhost:5000.</span></span>

![Yerel olarak çalışan Web sitesi](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="b3fef-123">Azure App Service örneği oluşturma</span><span class="sxs-lookup"><span data-stu-id="b3fef-123">Create the Azure App Service instance</span></span>

<span data-ttu-id="b3fef-124">Kullanarak [Azure bulut Kabuk](/azure/cloud-shell/quickstart), bir kaynak grubu, uygulama hizmeti planı ve bir App Service web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b3fef-124">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="b3fef-125">Dağıtım öncesinde aşağıdaki komutu kullanarak hesap düzeyinde dağıtım kimlik bilgilerini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="b3fef-125">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="b3fef-126">Bir dağıtım URL'si Git kullanarak uygulamayı dağıtmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b3fef-126">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="b3fef-127">Bu gibi URL'sini alın.</span><span class="sxs-lookup"><span data-stu-id="b3fef-127">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="b3fef-128">Biten görüntülenen URL'yi Not `.git`.</span><span class="sxs-lookup"><span data-stu-id="b3fef-128">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="b3fef-129">Sonraki adımda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fef-129">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="b3fef-130">Git kullanarak uygulamayı dağıtın</span><span class="sxs-lookup"><span data-stu-id="b3fef-130">Deploy the application using Git</span></span>

<span data-ttu-id="b3fef-131">Yerel makinenizden Git kullanarak dağıtmak hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="b3fef-131">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="b3fef-132">Satır sonları ilgili Git tüm uyarılar yoksaymak güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="b3fef-132">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b3fef-133">Windows</span><span class="sxs-lookup"><span data-stu-id="b3fef-133">Windows</span></span>](#tab/windows)
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

# <a name="othertabother"></a>[<span data-ttu-id="b3fef-134">Diğer</span><span class="sxs-lookup"><span data-stu-id="b3fef-134">Other</span></span>](#tab/other)
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

<span data-ttu-id="b3fef-135">Git önceki ayarlanan dağıtım kimlik bilgilerini ister.</span><span class="sxs-lookup"><span data-stu-id="b3fef-135">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="b3fef-136">Kimlik doğrulandıktan sonra uygulama uzak konuma gönderilir, oluşturulur ve dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="b3fef-136">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Git dağıtımı çıktı](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="b3fef-138">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="b3fef-138">Test the application</span></span>

<span data-ttu-id="b3fef-139">Göz atarak uygulamayı test `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="b3fef-139">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="b3fef-140">Bulut Kabuğu (veya Azure CLI) adresini görüntülemek için aşağıdakini kullanın:</span><span class="sxs-lookup"><span data-stu-id="b3fef-140">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Azure'da çalışan uygulama](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="b3fef-142">Temizleme</span><span class="sxs-lookup"><span data-stu-id="b3fef-142">Clean up</span></span>

<span data-ttu-id="b3fef-143">Uygulamayı test etme ve kod ve kaynakları İnceleme tamamlandığında, web app ve planı kaynak grubunu silerek silin.</span><span class="sxs-lookup"><span data-stu-id="b3fef-143">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="b3fef-144">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="b3fef-144">Next steps</span></span>

<span data-ttu-id="b3fef-145">Bu öğreticide, öğrenilen nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="b3fef-145">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b3fef-146">Azure CLI kullanarak Azure App Service Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="b3fef-146">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="b3fef-147">Azure App Service Git komut satırı aracını kullanarak bir ASP.NET Core uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="b3fef-147">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="b3fef-148">Ardından, CosmosDB kullanan mevcut bir web uygulamasına dağıtmak için komut satırını kullanmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="b3fef-148">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b3fef-149">.NET Core ile komut satırından Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="b3fef-149">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
