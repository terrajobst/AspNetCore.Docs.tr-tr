---
title: ASP.NET Core ile Git ve Visual Studio ile Azure'a sürekli dağıtım
author: rick-anderson
description: Visual Studio kullanarak ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtım için Azure App Service'e dağıtın.
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 5ae8ce01610828417fc76ed6626e518c8493bd0f
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340205"
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="1a4cf-103">ASP.NET Core ile Git ve Visual Studio ile Azure'a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="1a4cf-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="1a4cf-104">tarafından [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="1a4cf-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="1a4cf-105">Bu öğreticide, sürekli dağıtım kullanarak, Visual Studio kullanarak ASP.NET Core web uygulaması oluşturma ve bunu Visual Studio'dan Azure App Service'e dağıtma gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="1a4cf-106">Ayrıca bkz: [Azure işlem hattı ile ilk işlem hattınızı oluşturma](/azure/devops/pipelines/get-started-yaml), nasıl bir sürekli teslim (CD) iş akışı yapılandırma [Azure App Service](/azure/app-service/app-service-web-overview) Azure DevOps Hizmetleri'ni kullanarak.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-106">See also [Create your first pipeline with Azure Pipelines](/azure/devops/pipelines/get-started-yaml), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Azure DevOps Services.</span></span> <span data-ttu-id="1a4cf-107">Azure işlem hatları (Azure DevOps Hizmetleri hizmeti), Azure App Service'te barındırılan uygulamalar için güncelleştirmeleri yayımlamak için sağlam bir dağıtım işlem hattı ayarlamayı basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-107">Azure Pipelines (an Azure DevOps Services service) simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="1a4cf-108">İşlem hattı, Azure portalından oluşturmak, testleri çalıştırmak, hazırlama yuvasına dağıtın ve sonra üretim ortamına dağıtmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="1a4cf-109">Bu öğreticiyi tamamlamak için Microsoft Azure hesabı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="1a4cf-110">Bir hesap almak için [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) veya [ücretsiz deneme için kaydolun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="1a4cf-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a4cf-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="1a4cf-111">Prerequisites</span></span>

<span data-ttu-id="1a4cf-112">Bu öğreticide, aşağıdaki yazılımın yüklü olduğu varsayılır:</span><span class="sxs-lookup"><span data-stu-id="1a4cf-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="1a4cf-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a4cf-113">Visual Studio</span></span>](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="1a4cf-114">[Git](https://git-scm.com/downloads) Windows için</span><span class="sxs-lookup"><span data-stu-id="1a4cf-114">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="1a4cf-115">Bir ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="1a4cf-115">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="1a4cf-116">Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-116">Start Visual Studio.</span></span>

1. <span data-ttu-id="1a4cf-117">Gelen **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-117">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="1a4cf-118">Seçin **ASP.NET Core Web uygulaması** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-118">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="1a4cf-119">Altında göründüğü **yüklü** > **şablonları** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-119">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="1a4cf-120">Projeyi adlandırın `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-120">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="1a4cf-121">Seçin **yeni Git deposu Oluştur** seçeneğini ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-121">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Yeni Proje iletişim kutusu](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="1a4cf-123">İçinde **yeni ASP.NET Core projesi** iletişim kutusunda, ASP.NET Core seçin **boş** şablonu, ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-123">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Yeni ASP.NET Core projesi iletişim kutusu](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="1a4cf-125">En son .NET Core 2.0 sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-125">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="1a4cf-126">Web uygulamasını yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="1a4cf-126">Running the web app locally</span></span>

1. <span data-ttu-id="1a4cf-127">Visual Studio uygulamayı oluşturmayı tamamladığında, uygulamayı seçerek çalıştırma **hata ayıklama** > **hata ayıklamayı Başlat**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-127">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="1a4cf-128">Alternatif olarak, basın **F5**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-128">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="1a4cf-129">Bu, Visual Studio ve yeni uygulamayı başlatmak için zaman alabilir.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-129">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="1a4cf-130">Tamamlandığında, tarayıcıyı çalışmakta olan uygulamayla gösterir.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-130">Once it's complete, the browser shows the running app.</span></span>

   !['Hello World!' görüntüler uygulama çalıştıran gösteren tarayıcı penceresi](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="1a4cf-132">Çalışan Web uygulamasına inceledikten sonra Tarayıcıyı kapatın ve uygulamanın durdurmak için Visual Studio araç çubuğunda "Hata ayıklamayı Durdur" simgesini seçin.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-132">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="1a4cf-133">Azure Portalı'nda bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="1a4cf-133">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="1a4cf-134">Aşağıdaki adımlar, Azure Portalı'nda bir web uygulaması oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1a4cf-134">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="1a4cf-135">Oturum [Azure portalında](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1a4cf-135">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="1a4cf-136">Seçin **yeni** en sol üst köşesindeki portal arabirimi.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-136">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="1a4cf-137">Seçin **Web + mobil** > **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-137">Select **Web + Mobile** > **Web App**.</span></span>

   ![Microsoft Azure portalı: Yeni düğme: Web + mobil Market altında: Web uygulaması düğmesi altında öne çıkan uygulamalar](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="1a4cf-139">İçinde **Web uygulaması** dikey penceresinde için benzersiz bir değer girin **uygulama hizmeti adı**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-139">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Web uygulaması dikey](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="1a4cf-141">**Uygulama hizmeti adı** adı benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-141">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="1a4cf-142">Adı sağlandığında portal bu kuralı uygular.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-142">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="1a4cf-143">Bu değer her oluşumu için farklı bir değer sağlayan, yerine **SampleWebAppDemo** bu öğreticideki.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-143">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="1a4cf-144">Ayrıca **Web uygulaması** dikey penceresinde, mevcut bir seçin **App Service planı/konumu** veya yeni bir tane oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-144">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="1a4cf-145">Yeni bir plan oluşturuyorsanız, fiyatlandırma katmanını, konum ve diğer seçenekleri'ni seçin.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-145">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="1a4cf-146">App Service planları hakkında daha fazla bilgi için bkz. [Azure App Service planlarına ayrıntılı genel bakış](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="1a4cf-146">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="1a4cf-147">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-147">Select **Create**.</span></span> <span data-ttu-id="1a4cf-148">Azure, sağlama ve web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-148">Azure will provision and start the web app.</span></span>

   ![Azure portalı: Örnek Web uygulamasını tanıtım 01 hakkında temel bilgiler dikey penceresi](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="1a4cf-150">Yeni web uygulaması için Git yayımlamayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="1a4cf-150">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="1a4cf-151">Git, bir Azure App Service web uygulaması dağıtmak için kullanılan bir dağıtılmış sürüm denetim sistemidir.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-151">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="1a4cf-152">Web uygulama kodu yerel bir Git deposunda depolanır ve kodu uzak depoya ileterek Azure'a dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-152">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="1a4cf-153">Oturum [Azure portalında](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1a4cf-153">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="1a4cf-154">Seçin **uygulama hizmetleri** Azure aboneliği ile ilişkili uygulama hizmetlerin bir listesini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-154">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="1a4cf-155">Bu öğreticinin önceki bölümünde oluşturduğunuz web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-155">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="1a4cf-156">İçinde **dağıtım** dikey penceresinde **dağıtım seçenekleri** > **Kaynağı Seç** > **yerel Git deposu**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-156">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Ayarlar dikey penceresi: dağıtım kaynağı dikey penceresi: kaynak dikey penceresini seçin](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="1a4cf-158">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-158">Select **OK**.</span></span>

1. <span data-ttu-id="1a4cf-159">Dağıtım kimlik bilgileri, bir web uygulaması veya diğer App Service uygulaması yayımlamak için önceden kurulmuş yüklemediyseniz, bunları şimdi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="1a4cf-159">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="1a4cf-160">Seçin **ayarları** > **dağıtım kimlik bilgileri**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-160">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="1a4cf-161">**Dağıtım kimlik bilgilerini ayarla** dikey penceresi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-161">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="1a4cf-162">Bir kullanıcı adı ve parola oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-162">Create a user name and password.</span></span> <span data-ttu-id="1a4cf-163">Git'i kurma ayarlarken daha sonra kullanmak için parola kaydedin.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-163">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="1a4cf-164">Seçin **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-164">Select **Save**.</span></span>

1. <span data-ttu-id="1a4cf-165">İçinde **Web uygulaması** dikey penceresinde **ayarları** > **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-165">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="1a4cf-166">Dağıtmak için Uzak Git deposunun URL'sini altında gösterilen **GIT URL'si**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-166">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="1a4cf-167">Kopyalama **GIT URL'si** öğreticide daha sonra kullanmak için değer.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-167">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure portalı: uygulama özellikleri dikey penceresi](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="1a4cf-169">Web uygulamasını Azure App Service'e yayımlama</span><span class="sxs-lookup"><span data-stu-id="1a4cf-169">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="1a4cf-170">Bu bölümde, Visual Studio ve anında iletme bu depodan web uygulamasına dağıtmak için Azure'da kullanarak yerel bir Git deposu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-170">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="1a4cf-171">Adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="1a4cf-171">The steps involved include the following:</span></span>

* <span data-ttu-id="1a4cf-172">GIT URL değeri, yerel depoya Azure'a dağıtılacak şekilde kullanarak uzak depo ayarı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-172">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="1a4cf-173">Proje değişiklikleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-173">Commit project changes.</span></span>
* <span data-ttu-id="1a4cf-174">Proje değişiklikleri yerel depodan Azure'da uzak depoya gönderin.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-174">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="1a4cf-175">İçinde **Çözüm Gezgini** sağ **çözüm 'SampleWebAppDemo'** seçip **işleme**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-175">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="1a4cf-176">**Takım Gezgini** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-176">The **Team Explorer** is displayed.</span></span>

   ![Takım Gezgini Bağlan sekmesinde](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="1a4cf-178">İçinde **Takım Gezgini**seçin **giriş** (giriş simgesi) > **ayarları** > **depo ayarları**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-178">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="1a4cf-179">İçinde **uzaktan kumandalar** bölümünü **depo ayarları**seçin **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-179">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="1a4cf-180">**Ekleme uzak** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-180">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="1a4cf-181">Ayarlama **adı** için uzaktan **Azure örnek uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-181">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="1a4cf-182">Değerini **Fetch** için **Git URL'si** Bu öğreticide daha önce Azure'dan kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-182">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="1a4cf-183">İle biten URL olduğunu unutmayın **.git**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-183">Note that this is the URL that ends with **.git**.</span></span>

   ![Uzaktan iletişim Düzenle](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="1a4cf-185">Alternatif olarak, uzak depodan belirtin **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve komutunu girerek.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-185">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="1a4cf-186">Örnek:</span><span class="sxs-lookup"><span data-stu-id="1a4cf-186">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="1a4cf-187">Seçin **giriş** (giriş simgesi) > **ayarları** > **genel ayarlar**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-187">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="1a4cf-188">Ad ve e-posta adresinin ayarlandığını onaylayın.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-188">Confirm that the name and email address are set.</span></span> <span data-ttu-id="1a4cf-189">Seçin **güncelleştirme** gerekirse.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-189">Select **Update** if required.</span></span>

1. <span data-ttu-id="1a4cf-190">Seçin **giriş** > **değişiklikleri** dönmek için **değişiklikleri** görünümü.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-190">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="1a4cf-191">Bir işleme iletisi girin **ilk anında iletme #1** seçip **işleme**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-191">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="1a4cf-192">Bu eylem, oluşturur bir *işleme* yerel olarak.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-192">This action creates a *commit* locally.</span></span>

   ![Takım Gezgini Bağlan sekmesinde](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="1a4cf-194">Alternatif olarak, işleme değişir **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutları girerek.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-194">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="1a4cf-195">Örnek:</span><span class="sxs-lookup"><span data-stu-id="1a4cf-195">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="1a4cf-196">Seçin **giriş** > **eşitleme** > **eylemleri** > **komut istemi açın**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-196">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="1a4cf-197">Komut istemi proje dizinine açılır.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-197">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="1a4cf-198">Komut penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="1a4cf-198">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="1a4cf-199">Azure girin **dağıtım kimlik bilgileri** Azure'da daha önce oluşturulan parola.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-199">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="1a4cf-200">Bu komut, yerel proje dosyalarını Azure'a gönderme işlemini başlatır.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-200">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="1a4cf-201">Yukarıdaki komut çıktısı, dağıtım başarılı bir ileti ile sona erer.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-201">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="1a4cf-202">Projede işbirliği gerekiyorsa, dala gönderim yapmasını istemeyiz göz önünde bulundurun [GitHub](https://github.com) Azure'a göndermeden önce.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-202">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="1a4cf-203">Etkin dağıtımı doğrulama</span><span class="sxs-lookup"><span data-stu-id="1a4cf-203">Verify the Active Deployment</span></span>

<span data-ttu-id="1a4cf-204">Azure web app aktarımı yerel bir ortamdan başarılı olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-204">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="1a4cf-205">İçinde [Azure portalı](https://portal.azure.com), web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-205">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="1a4cf-206">Seçin **dağıtım** > **dağıtım seçenekleri**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-206">Select **Deployment** > **Deployment options**.</span></span>

![Azure portalı: Ayarlar dikey penceresi: başarılı dağıtımı gösteren dağıtımları dikey penceresi](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="1a4cf-208">Uygulamayı Azure'da çalıştırın</span><span class="sxs-lookup"><span data-stu-id="1a4cf-208">Run the app in Azure</span></span>

<span data-ttu-id="1a4cf-209">Web uygulamasını Azure'a dağıtıldığına göre uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-209">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="1a4cf-210">Bu iki şekilde gerçekleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="1a4cf-210">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="1a4cf-211">Azure Portalı'nda web uygulaması için web uygulaması dikey bulun.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-211">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="1a4cf-212">Seçin **Gözat** uygulamayı varsayılan tarayıcıda görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-212">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="1a4cf-213">Bir tarayıcı açın ve web uygulamasının URL'sini girin.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-213">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="1a4cf-214">Örnek: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="1a4cf-214">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="1a4cf-215">Web uygulamasını güncelleştirme ve yeniden yayımlama</span><span class="sxs-lookup"><span data-stu-id="1a4cf-215">Update the web app and republish</span></span>

<span data-ttu-id="1a4cf-216">Yerel koda değişiklikleri yaptıktan sonra yeniden yayımlayın:</span><span class="sxs-lookup"><span data-stu-id="1a4cf-216">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="1a4cf-217">İçinde **Çözüm Gezgini** Visual Studio açık *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-217">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="1a4cf-218">İçinde `Configure` yöntemini, değiştirme `Response.WriteAsync` şekilde aşağıdaki gibi görünecek şekilde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1a4cf-218">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="1a4cf-219">Değişiklikleri kaydetmek *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-219">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="1a4cf-220">İçinde **Çözüm Gezgini**, sağ **çözüm 'SampleWebAppDemo'** seçip **işleme**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-220">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="1a4cf-221">**Takım Gezgini** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-221">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="1a4cf-222">Bir işleme iletisi girin `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-222">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="1a4cf-223">Tuşuna **işleme** proje değişiklikleri kaydetmek için düğme.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-223">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="1a4cf-224">Seçin **giriş** > **eşitleme** > **eylemleri** > **anında iletme**.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-224">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="1a4cf-225">Alternatif olarak, değişiklikleri anında iletme **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve bir git komutu girmeyi.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-225">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="1a4cf-226">Örnek:</span><span class="sxs-lookup"><span data-stu-id="1a4cf-226">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="1a4cf-227">Güncelleştirilen web uygulamasını Azure'da görüntüleme</span><span class="sxs-lookup"><span data-stu-id="1a4cf-227">View the updated web app in Azure</span></span>

<span data-ttu-id="1a4cf-228">Güncelleştirilen web uygulamasını seçerek görüntüleme **Gözat** bir tarayıcıyı açarak ve web uygulaması için URL girilerek veya Azure Portal'da web uygulaması dikey penceresinden.</span><span class="sxs-lookup"><span data-stu-id="1a4cf-228">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="1a4cf-229">Örnek: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="1a4cf-229">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1a4cf-230">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1a4cf-230">Additional resources</span></span>

* [<span data-ttu-id="1a4cf-231">Azure işlem hattı ile ilk işlem hattınızı oluşturun</span><span class="sxs-lookup"><span data-stu-id="1a4cf-231">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)
* [<span data-ttu-id="1a4cf-232">Kudu projesi</span><span class="sxs-lookup"><span data-stu-id="1a4cf-232">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
