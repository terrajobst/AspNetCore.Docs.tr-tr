---
title: Visual Studio ile Azure ve Git ASP.NET Core ile sürekli dağıtım
author: rick-anderson
description: Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtımı için Azure App Service'e dağıtın.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 4de1893e8c1f7f2f4d9af7278a110067ea777c61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="c17f1-103">Visual Studio ile Azure ve Git ASP.NET Core ile sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="c17f1-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="c17f1-104">Tarafından [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="c17f1-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="c17f1-105">Bu öğretici, sürekli dağıtımı kullanarak Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturma ve Visual Studio'dan Azure App Service'e dağıtma gösterir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="c17f1-106">Ayrıca bkz. [oluşturma ve sürekli dağıtımı olan bir Azure Web uygulamasına yayımlamak için kullanım VSTS](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), kesintisiz teslim (CD) iş akışı için yapılandırma gösterilir [Azure uygulama hizmeti](/azure/app-service/app-service-web-overview) Visual Studio Team kullanarak Hizmetler.</span><span class="sxs-lookup"><span data-stu-id="c17f1-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="c17f1-107">Azure sürekli teslim Team Services, Azure App Service içinde barındırılan uygulamalar için güncelleştirmeleri yayımlamak için sağlam dağıtım ardışık düzen ayarlama basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="c17f1-108">Ardışık düzen oluşturmak, testleri çalıştırmak, bir hazırlama yuvasını dağıtmak ve üretime dağıtmak için Azure portalından yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="c17f1-109">Bu öğreticiyi tamamlamak için bir Microsoft Azure hesabı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="c17f1-110">Bir hesap almak için [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) veya [ücretsiz deneme için kaydolun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="c17f1-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c17f1-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="c17f1-111">Prerequisites</span></span>

<span data-ttu-id="c17f1-112">Bu öğretici aşağıdaki yazılımın yüklü olduğu varsayılır:</span><span class="sxs-lookup"><span data-stu-id="c17f1-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="c17f1-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c17f1-113">Visual Studio</span></span>](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="c17f1-114">[Git](https://git-scm.com/downloads) Windows için</span><span class="sxs-lookup"><span data-stu-id="c17f1-114">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="c17f1-115">Bir ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="c17f1-115">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="c17f1-116">Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="c17f1-116">Start Visual Studio.</span></span>

1. <span data-ttu-id="c17f1-117">Gelen **dosya** menüsünde, select **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-117">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="c17f1-118">Seçin **ASP.NET çekirdek Web uygulaması** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="c17f1-118">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="c17f1-119">Altında görünür **yüklü** > **şablonları** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-119">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="c17f1-120">Proje adı `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="c17f1-120">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="c17f1-121">Seçin **oluştur yeni Git deposu** seçeneğini ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-121">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Yeni Proje iletişim kutusu](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="c17f1-123">İçinde **yeni ASP.NET Core projesi** iletişim kutusunda, ASP.NET Core seçin **boş** şablonu, ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-123">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Yeni ASP.NET projesi iletişim kutusu](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="c17f1-125">En son .NET Core 2.0 sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="c17f1-125">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="c17f1-126">Web uygulamasını yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="c17f1-126">Running the web app locally</span></span>

1. <span data-ttu-id="c17f1-127">Visual Studio uygulama oluşturduktan sonra uygulama seçerek çalıştırma **hata ayıklama** > **hata ayıklamayı Başlat**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-127">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="c17f1-128">Alternatif olarak, basın **F5**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-128">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="c17f1-129">Visual Studio ve yeni uygulamayı başlatmak için saat sürebilir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-129">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="c17f1-130">Tamamlandığında, tarayıcı çalışan uygulamanın gösterir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-130">Once it's complete, the browser shows the running app.</span></span>

   !['Hello World!' görüntüler uygulamayı çalıştıran gösteren tarayıcı penceresi](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="c17f1-132">Çalışan Web uygulaması inceledikten sonra Tarayıcıyı kapatın ve uygulama durdurmak için Visual Studio araç çubuğunda "Hata ayıklamayı Durdur" simgesini seçin.</span><span class="sxs-lookup"><span data-stu-id="c17f1-132">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="c17f1-133">Azure Portalı'nda bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="c17f1-133">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="c17f1-134">Aşağıdaki adımlar, Azure Portalı'nda bir web uygulaması oluşturun:</span><span class="sxs-lookup"><span data-stu-id="c17f1-134">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="c17f1-135">Oturum [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c17f1-135">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="c17f1-136">Seçin **yeni** en sol üst portal arabirimi.</span><span class="sxs-lookup"><span data-stu-id="c17f1-136">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="c17f1-137">Seçin **Web + mobil** > **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-137">Select **Web + Mobile** > **Web App**.</span></span>

   ![Microsoft Azure Portal: Yeni düğmesi: Web + mobil Market altında: Web uygulaması düğmesi altında öne çıkan uygulamalar](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="c17f1-139">İçinde **Web uygulaması** dikey penceresinde için benzersiz bir değer girin **uygulama hizmet adı**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-139">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Web uygulaması dikey](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="c17f1-141">**Uygulama hizmeti adını** adının benzersiz olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-141">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="c17f1-142">Adı sağlandığında portal bu kural zorlar.</span><span class="sxs-lookup"><span data-stu-id="c17f1-142">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="c17f1-143">Farklı bir değer sağlayarak, her örneği için bu değeri yerine **SampleWebAppDemo** bu öğreticideki.</span><span class="sxs-lookup"><span data-stu-id="c17f1-143">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="c17f1-144">Ayrıca, **Web uygulaması** dikey penceresinde, var olan seçin **App Service planı/konumu** veya yeni bir tane oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c17f1-144">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="c17f1-145">Yeni bir plan oluşturuyorsanız, fiyatlandırma katmanı, konum ve diğer seçenekleri seçin.</span><span class="sxs-lookup"><span data-stu-id="c17f1-145">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="c17f1-146">Uygulama hizmeti planları hakkında daha fazla bilgi için bkz: [Azure App Service planlarına ayrıntılı genel bakış](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="c17f1-146">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="c17f1-147">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-147">Select **Create**.</span></span> <span data-ttu-id="c17f1-148">Azure sağlamak ve web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="c17f1-148">Azure will provision and start the web app.</span></span>

   ![Azure Portal: Örnek Web uygulamasını Demo 01 Essentials dikey penceresi](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="c17f1-150">Yeni web uygulaması için Git yayımlamayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="c17f1-150">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="c17f1-151">Git Azure App Service web uygulama dağıtmak için kullanılan bir dağıtılmış sürüm denetim sistemidir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-151">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="c17f1-152">Web uygulama kodu yerel bir Git deposunda depolanır ve kod uzak depoya ileterek Azure'a dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="c17f1-152">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="c17f1-153">İçine oturum [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c17f1-153">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="c17f1-154">Seçin **uygulama hizmetleri** Azure aboneliği ile ilişkili uygulama hizmetleri listesini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="c17f1-154">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="c17f1-155">Bu öğreticinin önceki bölümde oluşturduğunuz web uygulaması seçin.</span><span class="sxs-lookup"><span data-stu-id="c17f1-155">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="c17f1-156">İçinde **dağıtım** dikey penceresinde, select **dağıtım seçenekleri** > **Kaynağı Seç** > **yerel Git deposu**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-156">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Ayarlar dikey: dağıtım kaynak dikey: kaynak dikey seçin](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="c17f1-158">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-158">Select **OK**.</span></span>

1. <span data-ttu-id="c17f1-159">Bir web uygulaması veya diğer App Service uygulama yayımlamak için dağıtım kimlik bilgileri önceden ayarlanmış yüklemediyseniz, bunları şimdi kurun:</span><span class="sxs-lookup"><span data-stu-id="c17f1-159">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="c17f1-160">Seçin **ayarları** > **dağıtım kimlik bilgileri**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-160">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="c17f1-161">**Dağıtım kimlik bilgilerini ayarla** dikey penceresi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-161">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="c17f1-162">Bir kullanıcı adı ve parola oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c17f1-162">Create a user name and password.</span></span> <span data-ttu-id="c17f1-163">Daha sonra kullanmak için parola Git ayarlarken kaydedin.</span><span class="sxs-lookup"><span data-stu-id="c17f1-163">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="c17f1-164">Seçin **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-164">Select **Save**.</span></span>

1. <span data-ttu-id="c17f1-165">İçinde **Web uygulaması** dikey penceresinde, select **ayarları** > **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-165">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="c17f1-166">Dağıtmak için Uzak Git deposu URL'sini altında gösterilen **GIT URL'si**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-166">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="c17f1-167">Kopya **GIT URL'si** öğreticide daha sonra kullanmak için değer.</span><span class="sxs-lookup"><span data-stu-id="c17f1-167">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure Portal: uygulama özellikleri dikey penceresi](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="c17f1-169">Azure App Service'te web uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="c17f1-169">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="c17f1-170">Bu bölümde, Visual Studio ve anında iletme bu depodan web uygulaması dağıtma için Azure kullanarak yerel bir Git deposu oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="c17f1-170">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="c17f1-171">Adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="c17f1-171">The steps involved include the following:</span></span>

* <span data-ttu-id="c17f1-172">Yerel deposu Azure'da dağıtılabilmesi amacıyla GIT URL'si değerini kullanarak uzak depo ayarı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c17f1-172">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="c17f1-173">Proje değişiklikleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="c17f1-173">Commit project changes.</span></span>
* <span data-ttu-id="c17f1-174">Proje değişiklikleri yerel depodan Azure'da uzak deponuza iletin.</span><span class="sxs-lookup"><span data-stu-id="c17f1-174">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="c17f1-175">İçinde **Çözüm Gezgini** sağ **çözüm 'SampleWebAppDemo'** seçip **yürütme**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-175">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="c17f1-176">**Takım Gezgini** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-176">The **Team Explorer** is displayed.</span></span>

   ![Takım Gezgini bağlanmak sekmesi](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="c17f1-178">İçinde **Takım Gezgini**seçin **ev** (ev simgesi) > **ayarları** > **deposu ayarları**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-178">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="c17f1-179">İçinde **uzaktan kumandalar** bölümünü **deposu ayarları**seçin **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-179">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="c17f1-180">**Eklemek uzak** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-180">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="c17f1-181">Ayarlama **adı** uzak **Azure ÖrnekUyg**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-181">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="c17f1-182">Değeri ayarlanamıyor **Fetch** için **Git URL'si** Bu öğreticide daha önce Azure'dan kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="c17f1-182">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="c17f1-183">Bu ile biten URL olduğuna dikkat edin **.git**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-183">Note that this is the URL that ends with **.git**.</span></span>

   ![Uzaktan iletişim Düzenle](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="c17f1-185">Alternatif olarak, uzak depodan belirtin **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve komutunu girerek.</span><span class="sxs-lookup"><span data-stu-id="c17f1-185">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="c17f1-186">Örnek:</span><span class="sxs-lookup"><span data-stu-id="c17f1-186">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="c17f1-187">Seçin **ev** (ev simgesi) > **ayarları** > **genel ayarları**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-187">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="c17f1-188">Ad ve e-posta adresi olarak ayarlanmadığından onaylayın.</span><span class="sxs-lookup"><span data-stu-id="c17f1-188">Confirm that the name and email address are set.</span></span> <span data-ttu-id="c17f1-189">Seçin **güncelleştirme** gerekiyorsa.</span><span class="sxs-lookup"><span data-stu-id="c17f1-189">Select **Update** if required.</span></span>

1. <span data-ttu-id="c17f1-190">Seçin **giriş** > **değişiklikleri** geri dönmek için **değişiklikleri** görünümü.</span><span class="sxs-lookup"><span data-stu-id="c17f1-190">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="c17f1-191">Bir kaydetme iletisi gibi girin **ilk anında #1** seçip **tamamlama**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-191">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="c17f1-192">Bu eylem oluşturur bir *tamamlama* yerel olarak.</span><span class="sxs-lookup"><span data-stu-id="c17f1-192">This action creates a *commit* locally.</span></span>

   ![Takım Gezgini bağlanmak sekmesi](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="c17f1-194">Kaydetme işleminin alternatifi olarak, değişiklikleri **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutları girerek.</span><span class="sxs-lookup"><span data-stu-id="c17f1-194">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="c17f1-195">Örnek:</span><span class="sxs-lookup"><span data-stu-id="c17f1-195">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="c17f1-196">Seçin **giriş** > **eşitleme** > **Eylemler** > **komut istemi açın**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-196">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="c17f1-197">Komut istemi proje dizinine açılır.</span><span class="sxs-lookup"><span data-stu-id="c17f1-197">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="c17f1-198">Komut penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="c17f1-198">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="c17f1-199">Azure girin **dağıtım kimlik bilgileri** Azure'da daha önce oluşturulan parola.</span><span class="sxs-lookup"><span data-stu-id="c17f1-199">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="c17f1-200">Bu komut, yerel proje dosyalarını Azure'a dağıtmaya işlemi başlatır.</span><span class="sxs-lookup"><span data-stu-id="c17f1-200">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="c17f1-201">Yukarıdaki komut çıktısı dağıtımının başarılı bir ileti ile sona erer.</span><span class="sxs-lookup"><span data-stu-id="c17f1-201">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="c17f1-202">Projede işbirliği gerekiyorsa, için itme göz önünde bulundurun [GitHub](https://github.com) Azure'a dağıtmaya önce.</span><span class="sxs-lookup"><span data-stu-id="c17f1-202">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="c17f1-203">Etkin dağıtımı doğrulama</span><span class="sxs-lookup"><span data-stu-id="c17f1-203">Verify the Active Deployment</span></span>

<span data-ttu-id="c17f1-204">Azure web uygulaması aktarımı yerel ortamından başarılı olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="c17f1-204">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="c17f1-205">İçinde [Azure Portal](https://portal.azure.com), web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="c17f1-205">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="c17f1-206">Seçin **dağıtım** > **dağıtım seçenekleri**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-206">Select **Deployment** > **Deployment options**.</span></span>

![Azure Portal: Ayarlar dikey: dağıtımları dikey penceresi başarılı dağıtımı gösterme](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="c17f1-208">Uygulamayı Azure'da çalıştırma</span><span class="sxs-lookup"><span data-stu-id="c17f1-208">Run the app in Azure</span></span>

<span data-ttu-id="c17f1-209">Web uygulaması için Azure dağıtılır, uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c17f1-209">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="c17f1-210">Bu iki yolla gerçekleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="c17f1-210">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="c17f1-211">Azure Portalı'nda, web uygulaması için web uygulaması dikey bulun.</span><span class="sxs-lookup"><span data-stu-id="c17f1-211">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="c17f1-212">Seçin **Gözat** uygulamayı varsayılan tarayıcıda görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="c17f1-212">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="c17f1-213">Bir tarayıcı açın ve web uygulaması için URL'yi girin.</span><span class="sxs-lookup"><span data-stu-id="c17f1-213">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="c17f1-214">Örnek: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="c17f1-214">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="c17f1-215">Web uygulaması güncelleştirin ve yeniden yayımlamanız</span><span class="sxs-lookup"><span data-stu-id="c17f1-215">Update the web app and republish</span></span>

<span data-ttu-id="c17f1-216">Yerel kod değişiklikleri yaptıktan sonra yeniden yayımlayın:</span><span class="sxs-lookup"><span data-stu-id="c17f1-216">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="c17f1-217">İçinde **Çözüm Gezgini** Visual Studio açık *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="c17f1-217">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="c17f1-218">İçinde `Configure` yöntemini değiştirme `Response.WriteAsync` şekilde aşağıdaki gibi görünmesi yöntemi:</span><span class="sxs-lookup"><span data-stu-id="c17f1-218">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="c17f1-219">Değişiklikleri kaydetmek *haline*.</span><span class="sxs-lookup"><span data-stu-id="c17f1-219">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="c17f1-220">İçinde **Çözüm Gezgini**, sağ **çözüm 'SampleWebAppDemo'** seçip **yürütme**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-220">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="c17f1-221">**Takım Gezgini** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c17f1-221">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="c17f1-222">Bir kaydetme iletisi gibi girin `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="c17f1-222">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="c17f1-223">Tuşuna **tamamlama** proje değişiklikleri yürütmek için düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c17f1-223">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="c17f1-224">Seçin **giriş** > **eşitleme** > **Eylemler** > **anında**.</span><span class="sxs-lookup"><span data-stu-id="c17f1-224">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="c17f1-225">Alternatif olarak, değişiklikleri anında **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutunu girerek.</span><span class="sxs-lookup"><span data-stu-id="c17f1-225">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="c17f1-226">Örnek:</span><span class="sxs-lookup"><span data-stu-id="c17f1-226">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="c17f1-227">Görünüm Azure güncelleştirilmiş web uygulaması</span><span class="sxs-lookup"><span data-stu-id="c17f1-227">View the updated web app in Azure</span></span>

<span data-ttu-id="c17f1-228">Güncelleştirilmiş web uygulaması seçerek görüntülemek **Gözat** Azure Portalı'nda veya bir tarayıcı açarak ve web uygulaması için URL girerek web uygulaması dikey penceresinden.</span><span class="sxs-lookup"><span data-stu-id="c17f1-228">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="c17f1-229">Örnek: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="c17f1-229">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c17f1-230">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c17f1-230">Additional resources</span></span>

* [<span data-ttu-id="c17f1-231">Derleme ve sürekli dağıtımı olan bir Azure Web uygulamasına yayımlamak için VSTS kullanın</span><span class="sxs-lookup"><span data-stu-id="c17f1-231">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="c17f1-232">Proje Kudu</span><span class="sxs-lookup"><span data-stu-id="c17f1-232">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
