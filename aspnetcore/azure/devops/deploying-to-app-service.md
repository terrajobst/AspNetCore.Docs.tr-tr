---
title: ASP.NET Core ve Azure ile DevOps | Bir uygulamayı App Service'e dağıtma
author: CamSoper
description: Azure'da barındırılan bir ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturmaya uçtan uca yönergeler sağlar. bir kılavuz.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: 710e65a048fdc062219e90b0db323e8e96fd8e9d
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340140"
---
# <a name="deploy-an-app-to-app-service"></a><span data-ttu-id="da96a-103">Bir uygulamayı App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="da96a-103">Deploy an app to App Service</span></span>

<span data-ttu-id="da96a-104">[Azure App Service](https://docs.microsoft.com/azure/app-service/) olan Azure'nın web barındırma platformu.</span><span class="sxs-lookup"><span data-stu-id="da96a-104">[Azure App Service](https://docs.microsoft.com/azure/app-service/) is Azure's web hosting platform.</span></span> <span data-ttu-id="da96a-105">Web uygulamasını Azure App Service'e dağıtma el ile veya otomatik bir işlem yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="da96a-105">Deploying a web app to Azure App Service can be done manually or by an automated process.</span></span> <span data-ttu-id="da96a-106">Kılavuzu'nun bu bölümünde, el ile veya komut satırını kullanarak bir komut dosyası tarafından tetiklenebilir veya Visual Studio kullanarak el ile tetiklenen dağıtım yöntemlerini açıklar.</span><span class="sxs-lookup"><span data-stu-id="da96a-106">This section of the guide discusses deployment methods that can be triggered manually or by script using the command line, or triggered manually using Visual Studio.</span></span>

<span data-ttu-id="da96a-107">Bu bölümde, aşağıdaki görevleri yerine getirmek:</span><span class="sxs-lookup"><span data-stu-id="da96a-107">In this section, you'll accomplish the following tasks:</span></span>

* <span data-ttu-id="da96a-108">İndirin ve örnek uygulamayı derleyin.</span><span class="sxs-lookup"><span data-stu-id="da96a-108">Download and build the sample app.</span></span>
* <span data-ttu-id="da96a-109">Azure Cloud Shell'i kullanarak Azure App Service Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="da96a-109">Create an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="da96a-110">Örnek uygulamayı Git kullanarak Azure'a dağıtın.</span><span class="sxs-lookup"><span data-stu-id="da96a-110">Deploy the sample app to Azure using Git.</span></span>
* <span data-ttu-id="da96a-111">Bir değişiklik, Visual Studio kullanarak uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="da96a-111">Deploy a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="da96a-112">Hazırlama yuvası web uygulamasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="da96a-112">Add a staging slot to the web app.</span></span>
* <span data-ttu-id="da96a-113">Bir güncelleştirme hazırlama yuvasına dağıtın.</span><span class="sxs-lookup"><span data-stu-id="da96a-113">Deploy an update to the staging slot.</span></span>
* <span data-ttu-id="da96a-114">Hazırlama ve üretim yuvalarını Değiştir.</span><span class="sxs-lookup"><span data-stu-id="da96a-114">Swap the staging and production slots.</span></span>

## <a name="download-and-test-the-app"></a><span data-ttu-id="da96a-115">İndirin ve uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="da96a-115">Download and test the app</span></span>

<span data-ttu-id="da96a-116">Bu kılavuzda kullanılan önceden oluşturulmuş bir ASP.NET Core uygulaması uygulamadır [basit akış okuyucu](https://github.com/Azure-Samples/simple-feed-reader/).</span><span class="sxs-lookup"><span data-stu-id="da96a-116">The app used in this guide is a pre-built ASP.NET Core app, [Simple Feed Reader](https://github.com/Azure-Samples/simple-feed-reader/).</span></span> <span data-ttu-id="da96a-117">Kullanan Razor sayfaları uygulaması olan `Microsoft.SyndicationFeed.ReaderWriter` API, bir RSS/Atom akışı alma ve haber öğelerinin bir listede görüntüler.</span><span class="sxs-lookup"><span data-stu-id="da96a-117">It's a Razor Pages app that uses the `Microsoft.SyndicationFeed.ReaderWriter` API to retrieve an RSS/Atom feed and display the news items in a list.</span></span>

<span data-ttu-id="da96a-118">Kodu gözden çekinmeyin, ancak bu uygulama hakkında özel bir şey olduğunu anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="da96a-118">Feel free to review the code, but it's important to understand that there's nothing special about this app.</span></span> <span data-ttu-id="da96a-119">Bu sadece basit bir ASP.NET Core uygulaması yalnızca tanım amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="da96a-119">It's just a simple ASP.NET Core app for illustrative purposes.</span></span>

<span data-ttu-id="da96a-120">Bir komut kabuğundan, kodu indirmek, projeyi oluşturun ve aşağıdaki gibi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="da96a-120">From a command shell, download the code, build the project, and run it as follows.</span></span>

> <span data-ttu-id="da96a-121">*Not: Linux/macOS kullanıcılarını uygun yolları, örneğin, eğik çizgi kullanarak değişiklik (`/`) ters eğik çizgi yerine (`\`).*</span><span class="sxs-lookup"><span data-stu-id="da96a-121">*Note: Linux/macOS users should make appropriate changes for paths, e.g., using forward slash (`/`) rather than back slash (`\`).*</span></span>

1. <span data-ttu-id="da96a-122">Kodu yerel makinenizde bir klasöre kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="da96a-122">Clone the code to a folder on your local machine.</span></span>

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. <span data-ttu-id="da96a-123">Çalışma klasörünüzde değiştirme *basit akış okuyucu* oluşturulduğu klasör.</span><span class="sxs-lookup"><span data-stu-id="da96a-123">Change your working folder to the *simple-feed-reader* folder that was created.</span></span>

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. <span data-ttu-id="da96a-124">Paketleri geri yükle ve Çözümü derleyin.</span><span class="sxs-lookup"><span data-stu-id="da96a-124">Restore the packages, and build the solution.</span></span>

    ```console
    dotnet build
    ```

4. <span data-ttu-id="da96a-125">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="da96a-125">Run the app.</span></span>

    ```console
    dotnet run
    ```

    ![Komutu çalıştırın dotnet başarılı](./media/deploying-to-app-service/dotnet-run.png)

5. <span data-ttu-id="da96a-127">Bir tarayıcı açın ve gidin `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="da96a-127">Open a browser and navigate to `http://localhost:5000`.</span></span> <span data-ttu-id="da96a-128">Uygulama yazın veya bir dağıtım akış URL'sini yapıştırın olanak tanır ve haber öğelerinin bir listesini görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="da96a-128">The app allows you to type or paste a syndication feed URL and view a list of news items.</span></span>

     ![Uygulamayı bir RSS akışı içeriğini görüntüleme](./media/deploying-to-app-service/app-in-browser.png)

6. <span data-ttu-id="da96a-130">Memnun kaldıktan sonra uygulamanın düzgün çalıştığından, tuşlarına basarak kapatma **Ctrl**+**C** komut kabuğunda.</span><span class="sxs-lookup"><span data-stu-id="da96a-130">Once you're satisfied the app is working correctly, shut it down by pressing **Ctrl**+**C** in the command shell.</span></span>

## <a name="create-the-azure-app-service-web-app"></a><span data-ttu-id="da96a-131">Azure App Service Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="da96a-131">Create the Azure App Service Web App</span></span>

<span data-ttu-id="da96a-132">Uygulamayı dağıtmak için bir App Service oluşturma gerekecektir [Web uygulaması](https://docs.microsoft.com/azure/app-service/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="da96a-132">To deploy the app, you'll need to create an App Service [Web App](https://docs.microsoft.com/azure/app-service/app-service-web-overview).</span></span> <span data-ttu-id="da96a-133">Web uygulamasının oluşturulduktan sonra ona Git kullanarak yerel makinenizde dağıtacaksınız.</span><span class="sxs-lookup"><span data-stu-id="da96a-133">After creation of the Web App, you'll deploy to it from your local machine using Git.</span></span>

1. <span data-ttu-id="da96a-134">Oturum [Azure Cloud Shell'i](https://shell.azure.com/bash).</span><span class="sxs-lookup"><span data-stu-id="da96a-134">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="da96a-135">Not: yapılandırma dosyalarını bir depolama hesabı oluşturmak için Cloud Shell ilk kez oturum açtığınızda, ister.</span><span class="sxs-lookup"><span data-stu-id="da96a-135">Note: When you sign in for the first time, Cloud Shell prompts to create a storage account for configuration files.</span></span> <span data-ttu-id="da96a-136">Varsayılanları kabul edin veya benzersiz bir ad belirtin.</span><span class="sxs-lookup"><span data-stu-id="da96a-136">Accept the defaults or provide a unique name.</span></span>

2. <span data-ttu-id="da96a-137">Cloud Shell için aşağıdaki adımları kullanın.</span><span class="sxs-lookup"><span data-stu-id="da96a-137">Use the Cloud Shell for the following steps.</span></span>

    <span data-ttu-id="da96a-138">a.</span><span class="sxs-lookup"><span data-stu-id="da96a-138">a.</span></span> <span data-ttu-id="da96a-139">Web uygulamanızın adı depolamak için bir değişken bildirir.</span><span class="sxs-lookup"><span data-stu-id="da96a-139">Declare a variable to store your web app's name.</span></span> <span data-ttu-id="da96a-140">Adı, varsayılan URL'de kullanılacak benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="da96a-140">The name must be unique to be used in the default URL.</span></span> <span data-ttu-id="da96a-141">Kullanarak `$RANDOM` adı oluşturmak üzere Bash işlevi benzersizliği garanti eder ve sonuçları biçiminde `webappname99999`.</span><span class="sxs-lookup"><span data-stu-id="da96a-141">Using the `$RANDOM` Bash function to construct the name guarantees uniqueness and results in the format `webappname99999`.</span></span>

    ```console
    webappname=mywebapp$RANDOM
    ```

    <span data-ttu-id="da96a-142">b.</span><span class="sxs-lookup"><span data-stu-id="da96a-142">b.</span></span> <span data-ttu-id="da96a-143">Bir kaynak grubu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="da96a-143">Create a resource group.</span></span> <span data-ttu-id="da96a-144">Kaynak grupları, grup olarak yönetilen Azure kaynaklarını toplamak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="da96a-144">Resource groups provide a means to aggregate Azure resources to be managed as a group.</span></span>

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    <span data-ttu-id="da96a-145">`az` Komutu çağırır [Azure CLI](https://docs.microsoft.com/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="da96a-145">The `az` command invokes the [Azure CLI](https://docs.microsoft.com/cli/azure/).</span></span> <span data-ttu-id="da96a-146">CLI'yi yerel olarak çalıştırabilirsiniz, ancak zaman ve yapılandırma Cloud Shell'de kullanarak kaydeder.</span><span class="sxs-lookup"><span data-stu-id="da96a-146">The CLI can be run locally, but using it in the Cloud Shell saves time and configuration.</span></span>

    <span data-ttu-id="da96a-147">c.</span><span class="sxs-lookup"><span data-stu-id="da96a-147">c.</span></span> <span data-ttu-id="da96a-148">S1 katmanında bir App Service planı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="da96a-148">Create an App Service plan in the S1 tier.</span></span> <span data-ttu-id="da96a-149">Bir App Service planı, aynı fiyatlandırma katmanını paylaşan web apps gruplandırmasıdır.</span><span class="sxs-lookup"><span data-stu-id="da96a-149">An App Service plan is a grouping of web apps that share the same pricing tier.</span></span> <span data-ttu-id="da96a-150">S1 katmanı ücretsiz olarak değil, ancak hazırlama yuvaları özellik için zorunludur.</span><span class="sxs-lookup"><span data-stu-id="da96a-150">The S1 tier isn't free, but it's required for the staging slots feature.</span></span>

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    <span data-ttu-id="da96a-151">d.</span><span class="sxs-lookup"><span data-stu-id="da96a-151">d.</span></span> <span data-ttu-id="da96a-152">App Service planı aynı kaynak grubunda kullanarak web uygulama kaynağını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="da96a-152">Create the web app resource using the App Service plan in the same resource group.</span></span>

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    <span data-ttu-id="da96a-153">e.</span><span class="sxs-lookup"><span data-stu-id="da96a-153">e.</span></span> <span data-ttu-id="da96a-154">Dağıtım kimlik bilgilerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="da96a-154">Set the deployment credentials.</span></span> <span data-ttu-id="da96a-155">Bu dağıtım kimlik bilgileri, aboneliğinizdeki tüm web uygulamaları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="da96a-155">These deployment credentials apply to all the web apps in your subscription.</span></span> <span data-ttu-id="da96a-156">Kullanıcı adı özel karakterler kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="da96a-156">Don't use special characters in the user name.</span></span>

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    <span data-ttu-id="da96a-157">f.</span><span class="sxs-lookup"><span data-stu-id="da96a-157">f.</span></span> <span data-ttu-id="da96a-158">Yerel Git ve görüntü dağıtımları kabul etmek için web uygulaması yapılandırma *Git dağıtım URL'si*.</span><span class="sxs-lookup"><span data-stu-id="da96a-158">Configure the web app to accept deployments from local Git and display the *Git deployment URL*.</span></span> <span data-ttu-id="da96a-159">**Daha sonra başvuru için bu URL'yi Not**.</span><span class="sxs-lookup"><span data-stu-id="da96a-159">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    <span data-ttu-id="da96a-160">g.</span><span class="sxs-lookup"><span data-stu-id="da96a-160">g.</span></span> <span data-ttu-id="da96a-161">Görüntü *web app URL'si*.</span><span class="sxs-lookup"><span data-stu-id="da96a-161">Display the *web app URL*.</span></span> <span data-ttu-id="da96a-162">Boş bir web uygulamasını görmek için bu URL'sine gidin.</span><span class="sxs-lookup"><span data-stu-id="da96a-162">Browse to this URL to see the blank web app.</span></span> <span data-ttu-id="da96a-163">**Daha sonra başvuru için bu URL'yi Not**.</span><span class="sxs-lookup"><span data-stu-id="da96a-163">**Note this URL for reference later**.</span></span>

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. <span data-ttu-id="da96a-164">Yerel makinenizde bir komut kabuğu'nu kullanarak, web uygulamanızın proje klasörüne gidin (örneğin, `.\simple-feed-reader\SimpleFeedReader`).</span><span class="sxs-lookup"><span data-stu-id="da96a-164">Using a command shell on your local machine, navigate to the web app's project folder (for example, `.\simple-feed-reader\SimpleFeedReader`).</span></span> <span data-ttu-id="da96a-165">Dağıtım URL'sine göndermeye Git'i kurma ayarlamak için aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="da96a-165">Execute the following commands to set up Git to push to the deployment URL:</span></span>

    <span data-ttu-id="da96a-166">a.</span><span class="sxs-lookup"><span data-stu-id="da96a-166">a.</span></span> <span data-ttu-id="da96a-167">Uzak URL yerel depoya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="da96a-167">Add the remote URL to the local repository.</span></span>

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    <span data-ttu-id="da96a-168">b.</span><span class="sxs-lookup"><span data-stu-id="da96a-168">b.</span></span> <span data-ttu-id="da96a-169">Yerel anında iletme *ana* dala *azure ürün* uzaktan'ın *ana* dal.</span><span class="sxs-lookup"><span data-stu-id="da96a-169">Push the local *master* branch to the *azure-prod* remote's *master* branch.</span></span>

    ```console
    git push azure-prod master
    ```

    <span data-ttu-id="da96a-170">Daha önce oluşturduğunuz dağıtım kimlik bilgileri istenir.</span><span class="sxs-lookup"><span data-stu-id="da96a-170">You'll be prompted for the deployment credentials you created earlier.</span></span> <span data-ttu-id="da96a-171">Komut kabuğu'ndaki bir çıkış gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="da96a-171">Observe the output in the command shell.</span></span> <span data-ttu-id="da96a-172">Azure uzaktan ASP.NET Core uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="da96a-172">Azure builds the ASP.NET Core app remotely.</span></span>

4. <span data-ttu-id="da96a-173">Bir tarayıcıda gidin *Web uygulaması URL'si* ve uygulama oluşturulan ve dağıtılan not edin.</span><span class="sxs-lookup"><span data-stu-id="da96a-173">In a browser, navigate to the *Web app URL* and note the app has been built and deployed.</span></span> <span data-ttu-id="da96a-174">Ek değişiklikleri yerel Git deposu ile hassastır olabilir `git commit`.</span><span class="sxs-lookup"><span data-stu-id="da96a-174">Additional changes can be committed to the local Git repository with `git commit`.</span></span> <span data-ttu-id="da96a-175">Bu değişiklikler, önceki ile Azure'a itilir `git push` komutu.</span><span class="sxs-lookup"><span data-stu-id="da96a-175">These changes are pushed to Azure with the preceding `git push` command.</span></span>

## <a name="deployment-with-visual-studio"></a><span data-ttu-id="da96a-176">Visual Studio ile dağıtım</span><span class="sxs-lookup"><span data-stu-id="da96a-176">Deployment with Visual Studio</span></span>

> <span data-ttu-id="da96a-177">*Not: Bu bölüm yalnızca Windows için geçerlidir. Linux ve Macos'ta kullanıcılar, 2. adım açıklanan değişikliği yapmanız gerekir. Dosyayı kaydedin ve değişikliği ile yerel deponuza işleyin `git commit`. Son olarak, değişiklik ile anında iletme `git push`, ilk bölümde gibi.*</span><span class="sxs-lookup"><span data-stu-id="da96a-177">*Note: This section applies to Windows only. Linux and macOS users should make the change described in step 2 below. Save the file, and commit the change to the local repository with `git commit`. Finally, push the change with `git push`, as in the first section.*</span></span>

<span data-ttu-id="da96a-178">Uygulamayı komut kabuğu'ndan zaten dağıtıldı.</span><span class="sxs-lookup"><span data-stu-id="da96a-178">The app has already been deployed from the command shell.</span></span> <span data-ttu-id="da96a-179">Bir güncelleştirme uygulamasına dağıtmak için Visual Studio'nun tümleşik araçları kullanalım.</span><span class="sxs-lookup"><span data-stu-id="da96a-179">Let's use Visual Studio's integrated tools to deploy an update to the app.</span></span> <span data-ttu-id="da96a-180">Arka planda, Visual Studio Araçları komut satırı, ancak Visual Studio'nun alışık olduğunuz kullanıcı Arabirimi içinde aynı şeyi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="da96a-180">Behind the scenes, Visual Studio accomplishes the same thing as the command line tooling, but within Visual Studio's familiar UI.</span></span>

1. <span data-ttu-id="da96a-181">Açık *SimpleFeedReader.sln* Visual Studio'da.</span><span class="sxs-lookup"><span data-stu-id="da96a-181">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
2. <span data-ttu-id="da96a-182">Çözüm Gezgini'nde açın *Pages\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="da96a-182">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="da96a-183">Değişiklik `<h2>Simple Feed Reader</h2>` için `<h2>Simple Feed Reader - V2</h2>`.</span><span class="sxs-lookup"><span data-stu-id="da96a-183">Change `<h2>Simple Feed Reader</h2>` to `<h2>Simple Feed Reader - V2</h2>`.</span></span>
3. <span data-ttu-id="da96a-184">Tuşuna **Ctrl**+**Shift**+**B** uygulamayı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="da96a-184">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
4. <span data-ttu-id="da96a-185">Çözüm Gezgini'nde projeye sağ tıklayın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="da96a-185">In Solution Explorer, right-click on the project and click **Publish**.</span></span>

    ![Sağ tıklayın, yayımlama](./media/deploying-to-app-service/publish.png)
5. <span data-ttu-id="da96a-187">Visual Studio, yeni bir App Service kaynak oluşturabilirsiniz, ancak bu güncelleştirme, var olan dağıtım yayımlanacak.</span><span class="sxs-lookup"><span data-stu-id="da96a-187">Visual Studio can create a new App Service resource, but this update will be published over the existing deployment.</span></span> <span data-ttu-id="da96a-188">İçinde **yayımlama hedefi seçin** iletişim kutusunda **App Service** sol taraftaki listeden seçip **var olanı Seç**.</span><span class="sxs-lookup"><span data-stu-id="da96a-188">In the **Pick a publish target** dialog, select **App Service** from the list on the left, and then select **Select Existing**.</span></span> <span data-ttu-id="da96a-189">Tıklayın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="da96a-189">Click **Publish**.</span></span>
6. <span data-ttu-id="da96a-190">İçinde **App Service** iletişim kutusunda, Microsoft veya Azure aboneliğinizi oluşturmak için kullanılan Kurumsal hesap, sağ üst köşede görüntülendiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="da96a-190">In the **App Service** dialog, confirm that the Microsoft or Organizational account used to create your Azure subscription is displayed in the upper right.</span></span> <span data-ttu-id="da96a-191">Yüklü değilse, açılan tıklayın ve bunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="da96a-191">If it's not, click the drop-down and add it.</span></span>
7. <span data-ttu-id="da96a-192">Onaylayın doğru Azure **abonelik** seçilir.</span><span class="sxs-lookup"><span data-stu-id="da96a-192">Confirm that the correct Azure **Subscription** is selected.</span></span> <span data-ttu-id="da96a-193">İçin **görünümü**seçin **kaynak grubu**.</span><span class="sxs-lookup"><span data-stu-id="da96a-193">For **View**, select **Resource Group**.</span></span> <span data-ttu-id="da96a-194">Genişletin **AzureTutorial** kaynak grubunu ve mevcut web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="da96a-194">Expand the **AzureTutorial** resource group and then select the existing web app.</span></span> <span data-ttu-id="da96a-195">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="da96a-195">Click **OK**.</span></span>

    ![App Service iletişim yayımlama](./media/deploying-to-app-service/publish-dialog.png)

<span data-ttu-id="da96a-197">Visual Studio, derleme ve uygulamayı Azure'a dağıtır.</span><span class="sxs-lookup"><span data-stu-id="da96a-197">Visual Studio builds and deploys the app to Azure.</span></span> <span data-ttu-id="da96a-198">Web uygulaması URL'sine gidin.</span><span class="sxs-lookup"><span data-stu-id="da96a-198">Browse to the web app URL.</span></span> <span data-ttu-id="da96a-199">Doğrulamak `<h2>` öğesi değişiklik Canlı.</span><span class="sxs-lookup"><span data-stu-id="da96a-199">Validate that the `<h2>` element modification is live.</span></span>

![Başlığı değiştirdik uygulamayla](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a><span data-ttu-id="da96a-201">Dağıtım yuvaları</span><span class="sxs-lookup"><span data-stu-id="da96a-201">Deployment slots</span></span>

<span data-ttu-id="da96a-202">Dağıtım yuvaları, üretimde çalışan uygulama etkilemeden hazırlama değişiklikleri destekler.</span><span class="sxs-lookup"><span data-stu-id="da96a-202">Deployment slots support the staging of changes without impacting the app running in production.</span></span> <span data-ttu-id="da96a-203">Kalite güvencesi ekibi tarafından hazırlanan sürümünü doğrulandıktan sonra üretim ve hazırlama yuvası takas edilebilir.</span><span class="sxs-lookup"><span data-stu-id="da96a-203">Once the staged version of the app is validated by a quality assurance team, the production and staging slots can be swapped.</span></span> <span data-ttu-id="da96a-204">Uygulamayı hazırlama aşamasından üretime yalnızca bu şekilde yükseltilir.</span><span class="sxs-lookup"><span data-stu-id="da96a-204">The app in staging is promoted to production in this manner.</span></span> <span data-ttu-id="da96a-205">Aşağıdaki adımları bir hazırlama yuvası oluşturma, bazı değişiklikler dağıtma ve hazırlama yuvasını üretim sonra doğrulama ile.</span><span class="sxs-lookup"><span data-stu-id="da96a-205">The following steps create a staging slot, deploy some changes to it, and swap the staging slot with production after verification.</span></span>

1. <span data-ttu-id="da96a-206">Oturum [Azure Cloud Shell](https://shell.azure.com/bash), henüz oturum.</span><span class="sxs-lookup"><span data-stu-id="da96a-206">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash), if not already signed in.</span></span>
2. <span data-ttu-id="da96a-207">Hazırlama yuvası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="da96a-207">Create the staging slot.</span></span>

    <span data-ttu-id="da96a-208">a.</span><span class="sxs-lookup"><span data-stu-id="da96a-208">a.</span></span> <span data-ttu-id="da96a-209">Adıyla bir dağıtım yuvası Oluştur *hazırlama*.</span><span class="sxs-lookup"><span data-stu-id="da96a-209">Create a deployment slot with the name *staging*.</span></span>

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    <span data-ttu-id="da96a-210">b.</span><span class="sxs-lookup"><span data-stu-id="da96a-210">b.</span></span> <span data-ttu-id="da96a-211">Yerel Git ve get dağıtımı kullanmak için hazırlama yuvasına yapılandırma **hazırlama** dağıtım URL'si.</span><span class="sxs-lookup"><span data-stu-id="da96a-211">Configure the staging slot to use deployment from local Git and get the **staging** deployment URL.</span></span> <span data-ttu-id="da96a-212">**Daha sonra başvuru için bu URL'yi Not**.</span><span class="sxs-lookup"><span data-stu-id="da96a-212">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    <span data-ttu-id="da96a-213">c.</span><span class="sxs-lookup"><span data-stu-id="da96a-213">c.</span></span> <span data-ttu-id="da96a-214">Hazırlama yuvanın URL'si görüntüler.</span><span class="sxs-lookup"><span data-stu-id="da96a-214">Display the staging slot's URL.</span></span> <span data-ttu-id="da96a-215">Boş hazırlama yuvasına görmek için URL'sine gidin.</span><span class="sxs-lookup"><span data-stu-id="da96a-215">Browse to the URL to see the empty staging slot.</span></span> <span data-ttu-id="da96a-216">**Daha sonra başvuru için bu URL'yi Not**.</span><span class="sxs-lookup"><span data-stu-id="da96a-216">**Note this URL for reference later**.</span></span>

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. <span data-ttu-id="da96a-217">Bir metin düzenleyicisi veya Visual Studio değiştirme *Pages/Index.cshtml* yeniden böylece `<h2>` öğesi okur `<h2>Simple Feed Reader - V3</h2>` ve dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="da96a-217">In a text editor or Visual Studio, modify *Pages/Index.cshtml* again so that the `<h2>` element reads `<h2>Simple Feed Reader - V3</h2>` and save the file.</span></span>

4. <span data-ttu-id="da96a-218">Dosya kullanarak yerel Git deponuza işleyin **değişiklikleri** Visual Studio'nun sayfasında *Takım Gezgini* sekmesinde veya girerek aşağıdaki yerel makinenin komut kabuğu'nu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="da96a-218">Commit the file to the local Git repository, using either the **Changes** page in Visual Studio's *Team Explorer* tab, or by entering the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V3"
    ```
5. <span data-ttu-id="da96a-219">Yerel makinenin komut kabuğunu kullanma, hazırlık dağıtım URL'si Git remote olarak ekleyip Kaydettiğim değişiklikleri gönderin:</span><span class="sxs-lookup"><span data-stu-id="da96a-219">Using the local machine's command shell, add the staging deployment URL as a Git remote and push the committed changes:</span></span>

    <span data-ttu-id="da96a-220">a.</span><span class="sxs-lookup"><span data-stu-id="da96a-220">a.</span></span> <span data-ttu-id="da96a-221">Hazırlama için Uzak URL yerel Git deposuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="da96a-221">Add the remote URL for staging to the local Git repository.</span></span>

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    <span data-ttu-id="da96a-222">b.</span><span class="sxs-lookup"><span data-stu-id="da96a-222">b.</span></span> <span data-ttu-id="da96a-223">Yerel anında iletme *ana* dala *azure hazırlama* uzaktan'ın *ana* dal.</span><span class="sxs-lookup"><span data-stu-id="da96a-223">Push the local *master* branch to the *azure-staging* remote's *master* branch.</span></span>

    ```console
    git push azure-staging master
    ```

    <span data-ttu-id="da96a-224">Bekleme sırasında Azure derler ve uygulamayı dağıtır.</span><span class="sxs-lookup"><span data-stu-id="da96a-224">Wait while Azure builds and deploys the app.</span></span>

6. <span data-ttu-id="da96a-225">Hazırlama yuvasını v3 dağıtıldığını doğrulamak için iki tarayıcı penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="da96a-225">To verify that V3 has been deployed to the staging slot, open two browser windows.</span></span> <span data-ttu-id="da96a-226">Tek bir pencerede, özgün web uygulaması URL'sine gidin.</span><span class="sxs-lookup"><span data-stu-id="da96a-226">In one window, navigate to the original web app URL.</span></span> <span data-ttu-id="da96a-227">Diğer pencere hazırlama web uygulaması URL'sine gidin.</span><span class="sxs-lookup"><span data-stu-id="da96a-227">In the other window, navigate to the staging web app URL.</span></span> <span data-ttu-id="da96a-228">Üretim URL'si V2 uygulama hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="da96a-228">The production URL serves V2 of the app.</span></span> <span data-ttu-id="da96a-229">Hazırlama URL'si uygulamanın V3 işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="da96a-229">The staging URL serves V3 of the app.</span></span>

    ![Tarayıcı pencerelerini karşılaştırma](./media/deploying-to-app-service/ready-to-swap.png)

7. <span data-ttu-id="da96a-231">Cloud Shell'de doğrulandı/warmed'li hazırlama yuvasını üretime taşır.</span><span class="sxs-lookup"><span data-stu-id="da96a-231">In the Cloud Shell, swap the verified/warmed-up staging slot into production.</span></span>

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. <span data-ttu-id="da96a-232">Takas iki tarayıcı pencerelerini yenileyerek yapıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="da96a-232">Verify that the swap occurred by refreshing the two browser windows.</span></span>

    ![Değiştirme işleminden sonra tarayıcı pencerelerini karşılaştırma](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a><span data-ttu-id="da96a-234">Özet</span><span class="sxs-lookup"><span data-stu-id="da96a-234">Summary</span></span>

<span data-ttu-id="da96a-235">Bu bölümde, aşağıdaki görevleri tamamlandı:</span><span class="sxs-lookup"><span data-stu-id="da96a-235">In this section, the following tasks were completed:</span></span>

* <span data-ttu-id="da96a-236">İndirilen ve örnek uygulama yerleşik.</span><span class="sxs-lookup"><span data-stu-id="da96a-236">Downloaded and built the sample app.</span></span>
* <span data-ttu-id="da96a-237">Azure Cloud Shell'i kullanarak Azure App Service Web uygulaması oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="da96a-237">Created an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="da96a-238">Örnek uygulamayı Git kullanarak Azure'a dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="da96a-238">Deployed the sample app to Azure using Git.</span></span>
* <span data-ttu-id="da96a-239">Bir değişiklik, Visual Studio kullanarak uygulamaya dağıtılan.</span><span class="sxs-lookup"><span data-stu-id="da96a-239">Deployed a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="da96a-240">Hazırlama yuvası web uygulamasına eklendi.</span><span class="sxs-lookup"><span data-stu-id="da96a-240">Added a staging slot to the web app.</span></span>
* <span data-ttu-id="da96a-241">Bir güncelleştirme hazırlama yuvasına dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="da96a-241">Deployed an update to the staging slot.</span></span>
* <span data-ttu-id="da96a-242">Hazırlama ve üretim yuvası takas.</span><span class="sxs-lookup"><span data-stu-id="da96a-242">Swapped the staging and production slots.</span></span>

<span data-ttu-id="da96a-243">Sonraki bölümde, Azure işlem hatları ile bir DevOps işlem hattı oluşturmayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="da96a-243">In the next section, you'll learn how to build a DevOps pipeline with Azure Pipelines.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="da96a-244">Ek okuma</span><span class="sxs-lookup"><span data-stu-id="da96a-244">Additional reading</span></span>

* [<span data-ttu-id="da96a-245">Web Apps'e genel bakış</span><span class="sxs-lookup"><span data-stu-id="da96a-245">Web Apps overview</span></span>](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="da96a-246">Azure App Service'te .NET Core ve SQL veritabanı web uygulaması derleme</span><span class="sxs-lookup"><span data-stu-id="da96a-246">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [<span data-ttu-id="da96a-247">Azure App Service için dağıtım kimlik bilgilerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="da96a-247">Configure deployment credentials for Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service/app-service-deployment-credentials)
* [<span data-ttu-id="da96a-248">Azure App Service ortamlarında hazırlık ayarlama</span><span class="sxs-lookup"><span data-stu-id="da96a-248">Set up staging environments in Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service/web-sites-staged-publishing)
