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
ms.openlocfilehash: 0462a4cf18bba23643ed3b1b4e6b76bdbceb24a8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="cb20b-103">ASP.NET Core uygulama için Azure komut satırı araçları ile yayımlama</span><span class="sxs-lookup"><span data-stu-id="cb20b-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="cb20b-104">Tarafından [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="cb20b-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="cb20b-105">Bu öğretici komut satırı araçlarını kullanarak oluşturmak ve Microsoft Azure App Service ASP.NET Core uygulama dağıtmak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="cb20b-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="cb20b-106">Tamamlandığında, ASP.NET MVC Azure App Service Web uygulaması barındırılan çekirdek yerleşik bir web uygulamasına sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="cb20b-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="cb20b-107">Bu öğretici Windows komut satırı araçları kullanılarak yazılmış ancak macOS hem de Linux ortamlarında uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="cb20b-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="cb20b-108">Bu öğreticide, bilgi nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="cb20b-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cb20b-109">Azure CLI kullanarak Azure App Service Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="cb20b-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="cb20b-110">Azure App Service Git komut satırı aracını kullanarak bir ASP.NET Core uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="cb20b-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb20b-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cb20b-111">Prerequisites</span></span>

<span data-ttu-id="cb20b-112">Bu öğreticiyi tamamlamak için ihtiyacınız vardır:</span><span class="sxs-lookup"><span data-stu-id="cb20b-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="cb20b-113">A [Microsoft Azure aboneliği](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="cb20b-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="cb20b-114">[Git](https://www.git-scm.com/) komut satırı istemcisi</span><span class="sxs-lookup"><span data-stu-id="cb20b-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="cb20b-115">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="cb20b-115">Create a web application</span></span>

<span data-ttu-id="cb20b-116">Web uygulaması için yeni bir dizin oluşturma, yeni bir ASP.NET Core MVC uygulaması oluşturma ve Web sitesi yerel olarak çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cb20b-116">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="cb20b-117">Windows</span><span class="sxs-lookup"><span data-stu-id="cb20b-117">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="cb20b-118">Diğer</span><span class="sxs-lookup"><span data-stu-id="cb20b-118">Other</span></span>](#tab/other)
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

<span data-ttu-id="cb20b-120">Göz atarak uygulamayı test http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="cb20b-120">Test the application by browsing to http://localhost:5000.</span></span>

![Yerel olarak çalışan Web sitesi](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="cb20b-122">Azure App Service örneği oluşturma</span><span class="sxs-lookup"><span data-stu-id="cb20b-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="cb20b-123">Kullanarak [Azure bulut Kabuk](/azure/cloud-shell/quickstart), bir kaynak grubu, uygulama hizmeti planı ve bir App Service web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cb20b-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="cb20b-124">Dağıtım öncesinde aşağıdaki komutu kullanarak hesap düzeyinde dağıtım kimlik bilgilerini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="cb20b-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="cb20b-125">Bir dağıtım URL'si Git kullanarak uygulamayı dağıtmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="cb20b-125">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="cb20b-126">Bu gibi URL'sini alın.</span><span class="sxs-lookup"><span data-stu-id="cb20b-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="cb20b-127">Biten görüntülenen URL'yi Not `.git`.</span><span class="sxs-lookup"><span data-stu-id="cb20b-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="cb20b-128">Sonraki adımda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cb20b-128">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="cb20b-129">Git kullanarak uygulamayı dağıtın</span><span class="sxs-lookup"><span data-stu-id="cb20b-129">Deploy the application using Git</span></span>

<span data-ttu-id="cb20b-130">Yerel makinenizden Git kullanarak dağıtmak hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="cb20b-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="cb20b-131">Satır sonları ilgili Git tüm uyarılar yoksaymak güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="cb20b-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="cb20b-132">Windows</span><span class="sxs-lookup"><span data-stu-id="cb20b-132">Windows</span></span>](#tab/windows)
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

# <a name="othertabother"></a>[<span data-ttu-id="cb20b-133">Diğer</span><span class="sxs-lookup"><span data-stu-id="cb20b-133">Other</span></span>](#tab/other)
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

<span data-ttu-id="cb20b-134">Git önceki ayarlanan dağıtım kimlik bilgilerini ister.</span><span class="sxs-lookup"><span data-stu-id="cb20b-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="cb20b-135">Kimlik doğrulandıktan sonra uygulama uzak konuma gönderilir, oluşturulur ve dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="cb20b-135">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Git dağıtımı çıktı](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="cb20b-137">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="cb20b-137">Test the application</span></span>

<span data-ttu-id="cb20b-138">Göz atarak uygulamayı test `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="cb20b-138">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="cb20b-139">Bulut Kabuğu (veya Azure CLI) adresini görüntülemek için aşağıdakini kullanın:</span><span class="sxs-lookup"><span data-stu-id="cb20b-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Azure'da çalışan uygulama](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="cb20b-141">Temizleme</span><span class="sxs-lookup"><span data-stu-id="cb20b-141">Clean up</span></span>

<span data-ttu-id="cb20b-142">Uygulamayı test etme ve kod ve kaynakları İnceleme tamamlandığında, web app ve planı kaynak grubunu silerek silin.</span><span class="sxs-lookup"><span data-stu-id="cb20b-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="cb20b-143">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="cb20b-143">Next steps</span></span>

<span data-ttu-id="cb20b-144">Bu öğreticide, öğrenilen nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="cb20b-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cb20b-145">Azure CLI kullanarak Azure App Service Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="cb20b-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="cb20b-146">Azure App Service Git komut satırı aracını kullanarak bir ASP.NET Core uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="cb20b-146">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="cb20b-147">Ardından, CosmosDB kullanan mevcut bir web uygulamasına dağıtmak için komut satırını kullanmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="cb20b-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cb20b-148">.NET Core ile komut satırından Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="cb20b-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
