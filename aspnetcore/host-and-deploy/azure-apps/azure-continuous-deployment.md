---
title: "Visual Studio ile Azure ve Git ASP.NET Core ile sürekli dağıtım"
author: rick-anderson
description: "Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtımı için Azure App Service'e dağıtın."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 7302de1ace62dba53b317039aac7f4763314aa19
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="ba22f-103">Visual Studio ile Azure ve Git ASP.NET Core ile sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="ba22f-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="ba22f-104">Tarafından [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="ba22f-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="ba22f-105">Bu öğretici, sürekli dağıtımı kullanarak Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturma ve Visual Studio'dan Azure App Service'e dağıtma gösterir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="ba22f-106">Ayrıca bkz. [oluşturma ve sürekli dağıtımı olan bir Azure Web uygulamasına yayımlamak için kullanım VSTS](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), kesintisiz teslim (CD) iş akışı için yapılandırma gösterilir [Azure uygulama hizmeti](/azure/app-service/app-service-web-overview) Visual Studio Team kullanarak Hizmetler.</span><span class="sxs-lookup"><span data-stu-id="ba22f-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="ba22f-107">Azure sürekli teslim Team Services, Azure App Service içinde barındırılan uygulamalar için güncelleştirmeleri yayımlamak için sağlam dağıtım ardışık düzen ayarlama basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="ba22f-108">Ardışık düzen oluşturmak, testleri çalıştırmak, bir hazırlama yuvasını dağıtmak ve üretime dağıtmak için Azure portalından yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="ba22f-109">Bu öğreticiyi tamamlamak için bir Microsoft Azure hesabı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="ba22f-110">Bir hesap almak için [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) veya [ücretsiz deneme için kaydolun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="ba22f-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba22f-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="ba22f-111">Prerequisites</span></span>

<span data-ttu-id="ba22f-112">Bu öğretici aşağıdaki yazılımın yüklü olduğu varsayılır:</span><span class="sxs-lookup"><span data-stu-id="ba22f-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="ba22f-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ba22f-113">Visual Studio</span></span>](https://www.visualstudio.com)
* <span data-ttu-id="ba22f-114">[.NET core SDK](https://www.microsoft.com/net/download/core) (çalışma zamanı ve araçları)</span><span class="sxs-lookup"><span data-stu-id="ba22f-114">[.NET Core SDK](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>
* <span data-ttu-id="ba22f-115">[Git](https://git-scm.com/downloads) Windows için</span><span class="sxs-lookup"><span data-stu-id="ba22f-115">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="ba22f-116">Bir ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="ba22f-116">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="ba22f-117">Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="ba22f-117">Start Visual Studio.</span></span>

1. <span data-ttu-id="ba22f-118">Gelen **dosya** menüsünde, select **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-118">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="ba22f-119">Seçin **ASP.NET çekirdek Web uygulaması** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="ba22f-119">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="ba22f-120">Altında görünür **yüklü** > **şablonları** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-120">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="ba22f-121">Proje adı `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="ba22f-121">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="ba22f-122">Seçin **oluştur yeni Git deposu** seçeneğini ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-122">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Yeni Proje iletişim kutusu](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="ba22f-124">İçinde **yeni ASP.NET Core projesi** iletişim kutusunda, ASP.NET Core seçin **boş** şablonu, ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-124">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Yeni ASP.NET projesi iletişim kutusu](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="ba22f-126">En son .NET Core 2.0 sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="ba22f-126">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="ba22f-127">Web uygulamasını yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="ba22f-127">Running the web app locally</span></span>

1. <span data-ttu-id="ba22f-128">Visual Studio uygulama oluşturduktan sonra uygulama seçerek çalıştırma **hata ayıklama** > **hata ayıklamayı Başlat**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="ba22f-129">Alternatif olarak, basın **F5**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-129">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="ba22f-130">Visual Studio ve yeni uygulamayı başlatmak için saat sürebilir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="ba22f-131">Tamamlandığında, tarayıcı çalışan uygulamanın gösterir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-131">Once it's complete, the browser shows the running app.</span></span>

   !['Hello World!' görüntüler uygulamayı çalıştıran gösteren tarayıcı penceresi](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="ba22f-133">Çalışan Web uygulaması inceledikten sonra Tarayıcıyı kapatın ve uygulama durdurmak için Visual Studio araç çubuğunda "Hata ayıklamayı Durdur" simgesini seçin.</span><span class="sxs-lookup"><span data-stu-id="ba22f-133">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="ba22f-134">Azure Portalı'nda bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="ba22f-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="ba22f-135">Aşağıdaki adımlar, Azure Portalı'nda bir web uygulaması oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ba22f-135">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="ba22f-136">Oturum [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ba22f-136">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="ba22f-137">Seçin **yeni** en sol üst portal arabirimi.</span><span class="sxs-lookup"><span data-stu-id="ba22f-137">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="ba22f-138">Seçin **Web + mobil** > **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-138">Select **Web + Mobile** > **Web App**.</span></span>

   ![Microsoft Azure Portal: Yeni düğmesi: Web + mobil Market altında: Web uygulaması düğmesi altında öne çıkan uygulamalar](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="ba22f-140">İçinde **Web uygulaması** dikey penceresinde için benzersiz bir değer girin **uygulama hizmet adı**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Web uygulaması dikey](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="ba22f-142">**Uygulama hizmeti adını** adının benzersiz olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-142">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="ba22f-143">Adı sağlandığında portal bu kural zorlar.</span><span class="sxs-lookup"><span data-stu-id="ba22f-143">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="ba22f-144">Farklı bir değer sağlayarak, her örneği için bu değeri yerine **SampleWebAppDemo** bu öğreticideki.</span><span class="sxs-lookup"><span data-stu-id="ba22f-144">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="ba22f-145">Ayrıca, **Web uygulaması** dikey penceresinde, var olan seçin **App Service planı/konumu** veya yeni bir tane oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ba22f-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="ba22f-146">Yeni bir plan oluşturuyorsanız, fiyatlandırma katmanı, konum ve diğer seçenekleri seçin.</span><span class="sxs-lookup"><span data-stu-id="ba22f-146">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="ba22f-147">Uygulama hizmeti planları hakkında daha fazla bilgi için bkz: [Azure App Service planlarına ayrıntılı genel bakış](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="ba22f-147">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="ba22f-148">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-148">Select **Create**.</span></span> <span data-ttu-id="ba22f-149">Azure sağlamak ve web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="ba22f-149">Azure will provision and start the web app.</span></span>

   ![Azure Portal: Örnek Web uygulamasını Demo 01 Essentials dikey penceresi](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="ba22f-151">Yeni web uygulaması için Git yayımlamayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="ba22f-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="ba22f-152">Git Azure App Service web uygulama dağıtmak için kullanılan bir dağıtılmış sürüm denetim sistemidir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-152">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="ba22f-153">Web uygulama kodu yerel bir Git deposunda depolanır ve kod uzak depoya ileterek Azure'a dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="ba22f-153">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="ba22f-154">İçine oturum [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ba22f-154">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="ba22f-155">Seçin **uygulama hizmetleri** Azure aboneliği ile ilişkili uygulama hizmetleri listesini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="ba22f-155">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="ba22f-156">Bu öğreticinin önceki bölümde oluşturduğunuz web uygulaması seçin.</span><span class="sxs-lookup"><span data-stu-id="ba22f-156">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="ba22f-157">İçinde **dağıtım** dikey penceresinde, select **dağıtım seçenekleri** > **Kaynağı Seç** > **yerel Git deposu**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-157">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Ayarlar dikey: dağıtım kaynak dikey: kaynak dikey seçin](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="ba22f-159">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-159">Select **OK**.</span></span>

1. <span data-ttu-id="ba22f-160">Bir web uygulaması veya diğer App Service uygulama yayımlamak için dağıtım kimlik bilgileri önceden ayarlanmış yüklemediyseniz, bunları şimdi kurun:</span><span class="sxs-lookup"><span data-stu-id="ba22f-160">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="ba22f-161">Seçin **ayarları** > **dağıtım kimlik bilgileri**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-161">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="ba22f-162">**Dağıtım kimlik bilgilerini ayarla** dikey penceresi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-162">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="ba22f-163">Bir kullanıcı adı ve parola oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ba22f-163">Create a user name and password.</span></span> <span data-ttu-id="ba22f-164">Daha sonra kullanmak için parola Git ayarlarken kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ba22f-164">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="ba22f-165">Seçin **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-165">Select **Save**.</span></span>

1. <span data-ttu-id="ba22f-166">İçinde **Web uygulaması** dikey penceresinde, select **ayarları** > **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-166">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="ba22f-167">Dağıtmak için Uzak Git deposu URL'sini altında gösterilen **GIT URL'si**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-167">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="ba22f-168">Kopya **GIT URL'si** öğreticide daha sonra kullanmak için değer.</span><span class="sxs-lookup"><span data-stu-id="ba22f-168">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure Portal: uygulama özellikleri dikey penceresi](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="ba22f-170">Azure App Service'te web uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="ba22f-170">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="ba22f-171">Bu bölümde, Visual Studio ve anında iletme bu depodan web uygulaması dağıtma için Azure kullanarak yerel bir Git deposu oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="ba22f-171">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="ba22f-172">Adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ba22f-172">The steps involved include the following:</span></span>

* <span data-ttu-id="ba22f-173">Yerel deposu Azure'da dağıtılabilmesi amacıyla GIT URL'si değerini kullanarak uzak depo ayarı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ba22f-173">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="ba22f-174">Proje değişiklikleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="ba22f-174">Commit project changes.</span></span>
* <span data-ttu-id="ba22f-175">Proje değişiklikleri yerel depodan Azure'da uzak deponuza iletin.</span><span class="sxs-lookup"><span data-stu-id="ba22f-175">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="ba22f-176">İçinde **Çözüm Gezgini** sağ **çözüm 'SampleWebAppDemo'** seçip **yürütme**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-176">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="ba22f-177">**Takım Gezgini** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-177">The **Team Explorer** is displayed.</span></span>

   ![Takım Gezgini bağlanmak sekmesi](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="ba22f-179">İçinde **Takım Gezgini**seçin **ev** (ev simgesi) > **ayarları** > **deposu ayarları**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-179">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="ba22f-180">İçinde **uzaktan kumandalar** bölümünü **deposu ayarları**seçin **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-180">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="ba22f-181">**Eklemek uzak** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-181">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="ba22f-182">Ayarlama **adı** uzak **Azure ÖrnekUyg**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-182">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="ba22f-183">Değeri ayarlanamıyor **Fetch** için **Git URL'si** Bu öğreticide daha önce Azure'dan kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="ba22f-183">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="ba22f-184">Bu ile biten URL olduğuna dikkat edin **.git**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-184">Note that this is the URL that ends with **.git**.</span></span>

   ![Uzaktan iletişim Düzenle](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="ba22f-186">Alternatif olarak, uzak depodan belirtin **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve komutunu girerek.</span><span class="sxs-lookup"><span data-stu-id="ba22f-186">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="ba22f-187">Örnek:</span><span class="sxs-lookup"><span data-stu-id="ba22f-187">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="ba22f-188">Seçin **ev** (ev simgesi) > **ayarları** > **genel ayarları**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-188">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="ba22f-189">Ad ve e-posta adresi olarak ayarlanmadığından onaylayın.</span><span class="sxs-lookup"><span data-stu-id="ba22f-189">Confirm that the name and email address are set.</span></span> <span data-ttu-id="ba22f-190">Seçin **güncelleştirme** gerekiyorsa.</span><span class="sxs-lookup"><span data-stu-id="ba22f-190">Select **Update** if required.</span></span>

1. <span data-ttu-id="ba22f-191">Seçin **giriş** > **değişiklikleri** geri dönmek için **değişiklikleri** görünümü.</span><span class="sxs-lookup"><span data-stu-id="ba22f-191">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="ba22f-192">Bir kaydetme iletisi gibi girin **ilk anında #1** seçip **tamamlama**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-192">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="ba22f-193">Bu eylem oluşturur bir *tamamlama* yerel olarak.</span><span class="sxs-lookup"><span data-stu-id="ba22f-193">This action creates a *commit* locally.</span></span>

   ![Takım Gezgini bağlanmak sekmesi](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="ba22f-195">Kaydetme işleminin alternatifi olarak, değişiklikleri **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutları girerek.</span><span class="sxs-lookup"><span data-stu-id="ba22f-195">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="ba22f-196">Örnek:</span><span class="sxs-lookup"><span data-stu-id="ba22f-196">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="ba22f-197">Seçin **giriş** > **eşitleme** > **Eylemler** > **komut istemi açın**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-197">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="ba22f-198">Komut istemi proje dizinine açılır.</span><span class="sxs-lookup"><span data-stu-id="ba22f-198">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="ba22f-199">Komut penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="ba22f-199">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="ba22f-200">Azure girin **dağıtım kimlik bilgileri** Azure'da daha önce oluşturulan parola.</span><span class="sxs-lookup"><span data-stu-id="ba22f-200">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="ba22f-201">Bu komut, yerel proje dosyalarını Azure'a dağıtmaya işlemi başlatır.</span><span class="sxs-lookup"><span data-stu-id="ba22f-201">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="ba22f-202">Yukarıdaki komut çıktısı dağıtımının başarılı bir ileti ile sona erer.</span><span class="sxs-lookup"><span data-stu-id="ba22f-202">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="ba22f-203">Projede işbirliği gerekiyorsa, için itme göz önünde bulundurun [GitHub](https://github.com) Azure'a dağıtmaya önce.</span><span class="sxs-lookup"><span data-stu-id="ba22f-203">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="ba22f-204">Etkin dağıtımı doğrulama</span><span class="sxs-lookup"><span data-stu-id="ba22f-204">Verify the Active Deployment</span></span>

<span data-ttu-id="ba22f-205">Azure web uygulaması aktarımı yerel ortamından başarılı olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ba22f-205">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="ba22f-206">İçinde [Azure Portal](https://portal.azure.com), web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="ba22f-206">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="ba22f-207">Seçin **dağıtım** > **dağıtım seçenekleri**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-207">Select **Deployment** > **Deployment options**.</span></span>

![Azure Portal: Ayarlar dikey: dağıtımları dikey penceresi başarılı dağıtımı gösterme](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="ba22f-209">Uygulamayı Azure'da çalıştırma</span><span class="sxs-lookup"><span data-stu-id="ba22f-209">Run the app in Azure</span></span>

<span data-ttu-id="ba22f-210">Web uygulaması için Azure dağıtılır, uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ba22f-210">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="ba22f-211">Bu iki yolla gerçekleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="ba22f-211">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="ba22f-212">Azure Portalı'nda, web uygulaması için web uygulaması dikey bulun.</span><span class="sxs-lookup"><span data-stu-id="ba22f-212">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="ba22f-213">Seçin **Gözat** uygulamayı varsayılan tarayıcıda görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="ba22f-213">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="ba22f-214">Bir tarayıcı açın ve web uygulaması için URL'yi girin.</span><span class="sxs-lookup"><span data-stu-id="ba22f-214">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="ba22f-215">Örnek: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="ba22f-215">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="ba22f-216">Web uygulaması güncelleştirin ve yeniden yayımlamanız</span><span class="sxs-lookup"><span data-stu-id="ba22f-216">Update the web app and republish</span></span>

<span data-ttu-id="ba22f-217">Yerel kod değişiklikleri yaptıktan sonra yeniden yayımlayın:</span><span class="sxs-lookup"><span data-stu-id="ba22f-217">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="ba22f-218">İçinde **Çözüm Gezgini** Visual Studio açık *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="ba22f-218">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="ba22f-219">İçinde `Configure` yöntemini değiştirme `Response.WriteAsync` şekilde aşağıdaki gibi görünmesi yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ba22f-219">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="ba22f-220">Değişiklikleri kaydetmek *haline*.</span><span class="sxs-lookup"><span data-stu-id="ba22f-220">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="ba22f-221">İçinde **Çözüm Gezgini**, sağ **çözüm 'SampleWebAppDemo'** seçip **yürütme**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-221">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="ba22f-222">**Takım Gezgini** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ba22f-222">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="ba22f-223">Bir kaydetme iletisi gibi girin `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="ba22f-223">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="ba22f-224">Tuşuna **tamamlama** proje değişiklikleri yürütmek için düğmesi.</span><span class="sxs-lookup"><span data-stu-id="ba22f-224">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="ba22f-225">Seçin **giriş** > **eşitleme** > **Eylemler** > **anında**.</span><span class="sxs-lookup"><span data-stu-id="ba22f-225">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="ba22f-226">Alternatif olarak, değişiklikleri anında **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutunu girerek.</span><span class="sxs-lookup"><span data-stu-id="ba22f-226">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="ba22f-227">Örnek:</span><span class="sxs-lookup"><span data-stu-id="ba22f-227">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="ba22f-228">Görünüm Azure güncelleştirilmiş web uygulaması</span><span class="sxs-lookup"><span data-stu-id="ba22f-228">View the updated web app in Azure</span></span>

<span data-ttu-id="ba22f-229">Güncelleştirilmiş web uygulaması seçerek görüntülemek **Gözat** Azure Portalı'nda veya bir tarayıcı açarak ve web uygulaması için URL girerek web uygulaması dikey penceresinden.</span><span class="sxs-lookup"><span data-stu-id="ba22f-229">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="ba22f-230">Örnek: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="ba22f-230">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ba22f-231">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ba22f-231">Additional resources</span></span>

* [<span data-ttu-id="ba22f-232">Derleme ve sürekli dağıtımı olan bir Azure Web uygulamasına yayımlamak için VSTS kullanın</span><span class="sxs-lookup"><span data-stu-id="ba22f-232">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="ba22f-233">Project Kudu</span><span class="sxs-lookup"><span data-stu-id="ba22f-233">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
