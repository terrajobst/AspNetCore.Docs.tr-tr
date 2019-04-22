---
title: Visual Studio Code ile azure'da ASP.NET Core uygulaması yayımlama
author: ricardoserradas
description: Visual Studio Code kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin
ms.author: ricardoserradas
ms.custom: mvc
ms.date: 04/16/2019
uid: tutorials/publish-to-azure-webapp-using-vscode
ms.openlocfilehash: 64d82835f6a47a458802692c99658b964c07f807
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59711557"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio-code"></a><span data-ttu-id="accc1-103">Visual Studio Code ile azure'da ASP.NET Core uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="accc1-103">Publish an ASP.NET Core app to Azure with Visual Studio Code</span></span>

<span data-ttu-id="accc1-104">Tarafından [Ricardo Serradas](https://twitter.com/ricardoserradas)</span><span class="sxs-lookup"><span data-stu-id="accc1-104">By [Ricardo Serradas](https://twitter.com/ricardoserradas)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="accc1-105">App Service dağıtım sorun gidermek için bkz: <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="accc1-105">To troubleshoot an App Service deployment issue, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="intro"></a><span data-ttu-id="accc1-106">Giriş</span><span class="sxs-lookup"><span data-stu-id="accc1-106">Intro</span></span>

<span data-ttu-id="accc1-107">Bu öğreticide bir ASP.Net Core MVC uygulaması oluşturun ve Visual Studio Code içinde dağıtma öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="accc1-107">With this tutorial, you'll learn how to create an ASP.Net Core MVC Application and deploy it within Visual Studio Code.</span></span>

## <a name="set-up"></a><span data-ttu-id="accc1-108">Ayarlama</span><span class="sxs-lookup"><span data-stu-id="accc1-108">Set up</span></span>

- <span data-ttu-id="accc1-109">Açık bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/dotnet/) tane yoksa.</span><span class="sxs-lookup"><span data-stu-id="accc1-109">Open a [free Azure account](https://azure.microsoft.com/free/dotnet/) if you don't have one.</span></span>
- <span data-ttu-id="accc1-110">Yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download)</span><span class="sxs-lookup"><span data-stu-id="accc1-110">Install [.NET Core SDK](https://dotnet.microsoft.com/download)</span></span>
- <span data-ttu-id="accc1-111">Yükleme [Visual Studio kodu](https://code.visualstudio.com/Download)</span><span class="sxs-lookup"><span data-stu-id="accc1-111">Install [Visual Studio Code](https://code.visualstudio.com/Download)</span></span>
  - <span data-ttu-id="accc1-112">Yükleme [ C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) Visual Studio code'da</span><span class="sxs-lookup"><span data-stu-id="accc1-112">Install the [C# Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) to Visual Studio Code</span></span>
  - <span data-ttu-id="accc1-113">Tümünü Yükle [Azure uygulama hizmeti uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) Visual Studio Code için ve devam etmeden önce yapılandırma</span><span class="sxs-lookup"><span data-stu-id="accc1-113">Instal the [Azure App Service Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) to Visual Studio Code and configure it before proceeding</span></span>

## <a name="create-an-aspnet-core-mvc-project"></a><span data-ttu-id="accc1-114">Bir ASP.Net Core MVC projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="accc1-114">Create an ASP.Net Core MVC project</span></span>

<span data-ttu-id="accc1-115">Bir terminal kullanarak klasöre gidin, istediğiniz proje üzerinde oluşturulmuş ve aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="accc1-115">Using a terminal, navigate to the folder you want the project to be created on and use the following command:</span></span>

```cmd
> dotnet new mvc
```

<span data-ttu-id="accc1-116">Aşağıdakine benzer bir klasör yapısı vardır:</span><span class="sxs-lookup"><span data-stu-id="accc1-116">You'll have a folder structure similar to the following:</span></span>

```cmd
      appsettings.Development.json
      appsettings.json
<DIR> Controllers
<DIR> Models
      netcore-vscode.csproj
<DIR> obj
      Program.cs
<DIR> Properties
      Startup.cs
<DIR> Views
<DIR> wwwroot
```

## <a name="open-it-with-visual-studio-code"></a><span data-ttu-id="accc1-117">Visual Studio Code ile Aç</span><span class="sxs-lookup"><span data-stu-id="accc1-117">Open it with Visual Studio Code</span></span>

<span data-ttu-id="accc1-118">Projeniz oluşturulduktan sonra aşağıdaki seçeneklerden birini kullanarak Visual Studio Code ile açabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="accc1-118">After your project is created, you can open it with Visual Studio Code by using one of the options below:</span></span>

### <a name="through-the-command-line"></a><span data-ttu-id="accc1-119">Komut satırı aracılığıyla</span><span class="sxs-lookup"><span data-stu-id="accc1-119">Through the command line</span></span>

<span data-ttu-id="accc1-120">Projeyi oluşturduğunuzda, klasörde aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="accc1-120">Use the following command within the folder you created the project:</span></span>

```cmd
> code .
```

<span data-ttu-id="accc1-121">Aşağıdaki komut çalışmazsa başvurarak yüklemenizi düzgün şekilde yapılandırıldığını denetlemek [bu bağlantıyı](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform).</span><span class="sxs-lookup"><span data-stu-id="accc1-121">If the command below does not work, check if your installation is configured properly by referencing [this link](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform).</span></span>

### <a name="through-visual-studio-code-interface"></a><span data-ttu-id="accc1-122">Visual Studio Code arabirimi aracılığıyla</span><span class="sxs-lookup"><span data-stu-id="accc1-122">Through Visual Studio Code interface</span></span>

- <span data-ttu-id="accc1-123">Açık Visual Studio kodu</span><span class="sxs-lookup"><span data-stu-id="accc1-123">Open Visual Studio Code</span></span>
- <span data-ttu-id="accc1-124">Menüde seçin `File > Open Folder`</span><span class="sxs-lookup"><span data-stu-id="accc1-124">On the menu, select `File > Open Folder`</span></span>
- <span data-ttu-id="accc1-125">Kök seçin MVC projesini oluşturduğunuz klasörü</span><span class="sxs-lookup"><span data-stu-id="accc1-125">Select the root of the folder you created the MVC Project</span></span>

<span data-ttu-id="accc1-126">Proje klasörü açtığınızda, derleme ve hata ayıklamak için gerekli varlıkları eksik olduğunu bildiren bir ileti alırsınız.</span><span class="sxs-lookup"><span data-stu-id="accc1-126">When you open the project folder, you'll receive a message saying that required assets to build and debug are missing.</span></span> <span data-ttu-id="accc1-127">Bunları eklemek için Yardım'ı kabul edin.</span><span class="sxs-lookup"><span data-stu-id="accc1-127">Accept the help to add them.</span></span>

![Proje yüklendi ile Visual Studio Code arabirimi](publish-to-azure-webapp-using-vscode/_static/folder-structure-restore-netcore.jpg)

<span data-ttu-id="accc1-129">A `.vscode` klasör Proje yapısı altında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="accc1-129">A `.vscode` folder will be created under the project structure.</span></span> <span data-ttu-id="accc1-130">Aşağıdaki dosyaları içerir:</span><span class="sxs-lookup"><span data-stu-id="accc1-130">It will contain the following files:</span></span>

```cmd
launch.json
tasks.json
```

<span data-ttu-id="accc1-131">Bu yapı ve .NET Core Web uygulamanızı hata ayıklama yardımcı olması için yardımcı programı dosyalarıdır.</span><span class="sxs-lookup"><span data-stu-id="accc1-131">These are utility files to help you build and debug your .NET Core Web App.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="accc1-132">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="accc1-132">Run the app</span></span>

<span data-ttu-id="accc1-133">Biz uygulamayı Azure'a dağıtmadan önce yerel makinenizde düzgün biçimde çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="accc1-133">Before we deploy the app to Azure, make sure it is running properly on your local machine.</span></span>

- <span data-ttu-id="accc1-134">Projeyi çalıştırmak için F5 tuşuna basın</span><span class="sxs-lookup"><span data-stu-id="accc1-134">Press F5 to run the project</span></span>

<span data-ttu-id="accc1-135">Web uygulamanızı çalıştıran yeni bir varsayılan tarayıcı sekmesinde başlar.</span><span class="sxs-lookup"><span data-stu-id="accc1-135">Your web app will start running on a new tab of your default browser.</span></span> <span data-ttu-id="accc1-136">Başladıktan hemen sonra bir Gizlilik uyarısı fark edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="accc1-136">You may notice a privacy warning as soon as it starts.</span></span> <span data-ttu-id="accc1-137">Ya da uygulamanızı başlatacak nedeni HTTP ve HTTPS ve kullanarak, HTTPS uç noktasına varsayılan olarak gider.</span><span class="sxs-lookup"><span data-stu-id="accc1-137">This is because your app will start either using HTTP and HTTPS, and it navigates to the HTTPS endpoint by default.</span></span>

![Uygulamayı yerel olarak hata ayıklama sırasında Gizlilik uyarısı](publish-to-azure-webapp-using-vscode/_static/run-webapp-https-warning.jpg)

<span data-ttu-id="accc1-139">Hata ayıklama oturumu korumak için tıklayın `Advanced` ardından `Continue to localhost (unsafe)`.</span><span class="sxs-lookup"><span data-stu-id="accc1-139">To keep the debugging session, click `Advanced` and then `Continue to localhost (unsafe)`.</span></span>

## <a name="generate-the-deployment-package-locally"></a><span data-ttu-id="accc1-140">Yerel olarak dağıtım paketi oluşturun</span><span class="sxs-lookup"><span data-stu-id="accc1-140">Generate the deployment package locally</span></span>

- <span data-ttu-id="accc1-141">Visual Studio Code Terminali açın</span><span class="sxs-lookup"><span data-stu-id="accc1-141">Open Visual Studio Code terminal</span></span>
- <span data-ttu-id="accc1-142">Oluşturmak için aşağıdaki komutu kullanın. bir `Release` adlı bir alt klasörü paketine `publish`:</span><span class="sxs-lookup"><span data-stu-id="accc1-142">Use the following command to generate a `Release` package to a sub folder called `publish`:</span></span>
  - `dotnet publish -c Release -o ./publish`
- <span data-ttu-id="accc1-143">Yeni bir `publish` klasör Proje yapısı altında oluşturulacak</span><span class="sxs-lookup"><span data-stu-id="accc1-143">A new `publish` folder will be created under the project structure</span></span>

![Klasör yapısını yayımlama](publish-to-azure-webapp-using-vscode/_static/publish-folder.jpg)

## <a name="publish-to-azure-app-service"></a><span data-ttu-id="accc1-145">Azure App Service’e yayımlama</span><span class="sxs-lookup"><span data-stu-id="accc1-145">Publish to Azure App Service</span></span>

<span data-ttu-id="accc1-146">Visual Studio Code için Azure App Service uzantısı yararlanarak, doğrudan Azure App Service Web sitesine yayımlamak için aşağıdaki adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="accc1-146">Leveraging the Azure App Service extension for Visual Studio Code, follow the steps below to publish the website directly to the Azure App Service.</span></span>

### <a name="if-youre-creating-a-new-web-app"></a><span data-ttu-id="accc1-147">Yeni bir Web uygulaması oluşturuyorsanız</span><span class="sxs-lookup"><span data-stu-id="accc1-147">If you're creating a new Web App</span></span>

- <span data-ttu-id="accc1-148">Sağ tıklayın `publish` klasörü ve seçin `Deploy to Web App...`</span><span class="sxs-lookup"><span data-stu-id="accc1-148">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="accc1-149">Web uygulamasını oluşturmak istediğiniz aboneliği seçin</span><span class="sxs-lookup"><span data-stu-id="accc1-149">Select the subscription you want to create the Web App</span></span>
- <span data-ttu-id="accc1-150">Seçin `Create New Web App`</span><span class="sxs-lookup"><span data-stu-id="accc1-150">Select `Create New Web App`</span></span>
- <span data-ttu-id="accc1-151">Web uygulaması için bir ad girin</span><span class="sxs-lookup"><span data-stu-id="accc1-151">Enter a name for the Web App</span></span>

<span data-ttu-id="accc1-152">Uzantı, yeni Web uygulaması oluşturur ve paket dağıttıktan otomatik olarak başlatılacak.</span><span class="sxs-lookup"><span data-stu-id="accc1-152">The extension will create the new Web App and will automatically start deploying the package to it.</span></span> <span data-ttu-id="accc1-153">Dağıtım tamamlandıktan sonra tıklayın `Browse Website` dağıtımını doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="accc1-153">Once the deployment is finished, click `Browse Website` to validate the deployment.</span></span>

![Dağıtım başarılı iletisi](publish-to-azure-webapp-using-vscode/_static/deployment-succeeded-message.jpg)

<span data-ttu-id="accc1-155">' A tıkladığınızda `Browse Website`, varsayılan tarayıcınızı kullanarak ona gitmeniz:</span><span class="sxs-lookup"><span data-stu-id="accc1-155">Once you click `Browse Website`, you'll navigate to it using your default browser:</span></span>

![Yeni Web uygulaması başarıyla dağıtıldı](publish-to-azure-webapp-using-vscode/_static/new-webapp-deployed.jpg)

### <a name="if-youre-deploying-to-an-existing-web-app"></a><span data-ttu-id="accc1-157">Mevcut bir Web uygulamasına dağıtım yapıyorsanız</span><span class="sxs-lookup"><span data-stu-id="accc1-157">If you're deploying to an existing Web App</span></span>

- <span data-ttu-id="accc1-158">Sağ tıklayın `publish` klasörü ve seçin `Deploy to Web App...`</span><span class="sxs-lookup"><span data-stu-id="accc1-158">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="accc1-159">Mevcut Web uygulamasının bulunduğu abonelik seçin</span><span class="sxs-lookup"><span data-stu-id="accc1-159">Select the subscription the existing Web App resides</span></span>
- <span data-ttu-id="accc1-160">Web uygulaması listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="accc1-160">Select the Web App from the list</span></span>
- <span data-ttu-id="accc1-161">Visual Studio Code varolan içeriğin üzerine yazmak isteyip istemediğinizi sorar.</span><span class="sxs-lookup"><span data-stu-id="accc1-161">Visual Studio Code will ask you if you want to overwrite the existing content.</span></span> <span data-ttu-id="accc1-162">Tıklayın `Deploy` onaylamak için</span><span class="sxs-lookup"><span data-stu-id="accc1-162">Click `Deploy` to confirm</span></span>

<span data-ttu-id="accc1-163">Uzantının güncelleştirilmiş içeriği Web uygulamasına dağıtır.</span><span class="sxs-lookup"><span data-stu-id="accc1-163">The extension will deploy the updated content to the Web App.</span></span> <span data-ttu-id="accc1-164">Bunu yaptıktan sonra tıklayın `Browse Website` dağıtımını doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="accc1-164">Once it's done, click `Browse Website` to validate the deployment.</span></span>

![Mevcut Web uygulaması başarıyla dağıtıldı](publish-to-azure-webapp-using-vscode/_static/existing-webapp-deployed.jpg)

## <a name="next-steps"></a><span data-ttu-id="accc1-166">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="accc1-166">Next steps</span></span>

- [<span data-ttu-id="accc1-167">İlk Azure DevOps işlem hattınızı oluşturun</span><span class="sxs-lookup"><span data-stu-id="accc1-167">Create your first Azure DevOps pipeline</span></span>](/azure/devops/pipelines/create-first-pipeline)

## <a name="additional-resources"></a><span data-ttu-id="accc1-168">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="accc1-168">Additional resources</span></span>

- [<span data-ttu-id="accc1-169">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="accc1-169">Azure App Service</span></span>](/azure/app-service/app-service-web-overview)
- [<span data-ttu-id="accc1-170">Azure kaynak grupları</span><span class="sxs-lookup"><span data-stu-id="accc1-170">Azure resource groups</span></span>](/azure/azure-resource-manager/resource-group-overview#resource-groups)