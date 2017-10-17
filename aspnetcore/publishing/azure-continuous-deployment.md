---
title: "Visual Studio ve Git ile Azure için sürekli dağıtım"
author: rick-anderson
description: "Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtımı için Azure App Service'e dağıtın."
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: a9efad38b1c75bd3a186b4ec85861357ecf744b9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="1adde-104">Visual Studio ve Git ile ASP.NET Core için Azure için sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="1adde-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="1adde-105">Tarafından [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="1adde-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="1adde-106">Bu öğretici sürekli dağıtımı kullanarak Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturma ve Visual Studio'dan Azure App Service'e dağıtma gösterir.</span><span class="sxs-lookup"><span data-stu-id="1adde-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="1adde-107">Ayrıca bkz. [oluşturma ve sürekli dağıtımı olan bir Azure Web uygulamasına yayımlamak için kullanım VSTS](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), kesintisiz teslim (CD) iş akışı için yapılandırma gösterilir [Azure uygulama hizmeti](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) Visual Studio Team kullanarak Hizmetler.</span><span class="sxs-lookup"><span data-stu-id="1adde-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="1adde-108">Azure sürekli teslim Team Services, Azure App Service uygulamanız için güncelleştirmeleri yayımlamak için sağlam dağıtım ardışık düzen ayarlama basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="1adde-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="1adde-109">Ardışık düzen oluşturmak, testleri çalıştırmak, bir hazırlama yuvasını dağıtmak ve üretime dağıtmak için Azure portalından yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1adde-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="1adde-110">Bu öğreticiyi tamamlamak için Microsoft Azure hesabınızın olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1adde-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="1adde-111">Bir hesabınız yoksa, şunları yapabilirsiniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) veya [ücretsiz deneme için kaydolun](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="1adde-111">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1adde-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="1adde-112">Prerequisites</span></span>

<span data-ttu-id="1adde-113">Bu öğretici zaten aşağıdaki yüklemiş olduğunuz varsayılır:</span><span class="sxs-lookup"><span data-stu-id="1adde-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="1adde-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1adde-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="1adde-115">[ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (çalışma zamanı ve araçları)</span><span class="sxs-lookup"><span data-stu-id="1adde-115">[ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (runtime and tooling)</span></span>

* <span data-ttu-id="1adde-116">[Git](https://git-scm.com/downloads) Windows için</span><span class="sxs-lookup"><span data-stu-id="1adde-116">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="1adde-117">Bir ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="1adde-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="1adde-118">Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="1adde-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="1adde-119">Gelen **dosya** menüsünde, select **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="1adde-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="1adde-120">Seçin **ASP.NET Web uygulaması** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="1adde-120">Select the **ASP.NET Web Application** project template.</span></span> <span data-ttu-id="1adde-121">Altında görünür **yüklü** > **şablonları** > **Visual C#** > **Web**.</span><span class="sxs-lookup"><span data-stu-id="1adde-121">It appears under **Installed** > **Templates** > **Visual C#** > **Web**.</span></span> <span data-ttu-id="1adde-122">Proje adı `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="1adde-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="1adde-123">Seçin **yeni Git deposunun oluşturma** seçeneğini ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1adde-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Yeni Proje iletişim kutusu](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="1adde-125">İçinde **yeni ASP.NET projesi** iletişim kutusunda, ASP.NET Core seçin **boş** şablonu, ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1adde-125">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Yeni ASP.NET projesi iletişim kutusu](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a><span data-ttu-id="1adde-127">Web uygulamasını yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="1adde-127">Running the web app locally</span></span>

1. <span data-ttu-id="1adde-128">Visual Studio uygulama oluşturduktan sonra uygulama seçerek çalıştırma **hata ayıklama** -> **hata ayıklamayı Başlat**.</span><span class="sxs-lookup"><span data-stu-id="1adde-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="1adde-129">Alternatif olarak, basabilirsiniz **F5**.</span><span class="sxs-lookup"><span data-stu-id="1adde-129">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="1adde-130">Visual Studio ve yeni uygulamayı başlatmak için saat sürebilir.</span><span class="sxs-lookup"><span data-stu-id="1adde-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="1adde-131">Tamamlandığında, tarayıcı çalışan uygulamanın gösterir.</span><span class="sxs-lookup"><span data-stu-id="1adde-131">Once it is complete, the browser will show the running app.</span></span>

   !['Hello World!' görüntüler uygulamayı çalıştıran gösteren tarayıcı penceresi](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="1adde-133">Çalışan Web uygulaması inceledikten sonra Tarayıcıyı kapatın ve uygulama durdurmak için Visual Studio'nun araç çubuğundaki "Hata ayıklamayı Durdur" simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1adde-133">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="1adde-134">Azure Portalı'nda bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="1adde-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="1adde-135">Aşağıdaki adımlar bir web uygulamasını Azure Portalı'nda oluşturmada size yol gösterecektir.</span><span class="sxs-lookup"><span data-stu-id="1adde-135">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="1adde-136">Oturum [Azure portalı](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="1adde-136">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="1adde-137">DOKUNUN **yeni** en üstünde portalın sol</span><span class="sxs-lookup"><span data-stu-id="1adde-137">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="1adde-138">DOKUNUN **Web + mobil** > **Web uygulaması**</span><span class="sxs-lookup"><span data-stu-id="1adde-138">TAP **Web + Mobile** > **Web App**</span></span>

    ![Microsoft Azure Portal: Yeni düğmesi: Web + mobil Market altında: Web uygulaması düğmesi altında öne çıkan uygulamalar](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="1adde-140">İçinde **Web uygulaması** dikey penceresinde için benzersiz bir değer girin **uygulama hizmet adı**.</span><span class="sxs-lookup"><span data-stu-id="1adde-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![Web uygulaması dikey](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="1adde-142">**Uygulama hizmeti adını** adının benzersiz olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1adde-142">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="1adde-143">Adı girmek çalıştığınızda portal bu kural zorlar.</span><span class="sxs-lookup"><span data-stu-id="1adde-143">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="1adde-144">Farklı bir değer girdikten sonra her örneği için bu değeri yerine gerekir **SampleWebAppDemo** Bu öğreticide gördüğünüz.</span><span class="sxs-lookup"><span data-stu-id="1adde-144">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="1adde-145">Ayrıca, **Web uygulaması** dikey penceresinde, var olan seçin **App Service planı/konumu** veya yeni bir tane oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1adde-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="1adde-146">Yeni bir plan oluşturursanız, fiyatlandırma katmanı, konum ve diğer seçenekleri seçin.</span><span class="sxs-lookup"><span data-stu-id="1adde-146">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="1adde-147">Uygulama hizmeti planları hakkında daha fazla bilgi için [Azure App Service planlarına ayrıntılı genel bakış](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span><span class="sxs-lookup"><span data-stu-id="1adde-147">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="1adde-148">
              **Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1adde-148">Click **Create**.</span></span> <span data-ttu-id="1adde-149">Azure sağlamak ve web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="1adde-149">Azure will provision and start your web app.</span></span>

    ![Azure Portal: Örnek Web uygulamasını Demo 01 Essentials dikey penceresi](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="1adde-151">Yeni web uygulaması için Git yayımlamayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="1adde-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="1adde-152">Git Azure App Service web uygulamanızı dağıtmak için kullanabileceğiniz bir dağıtılmış sürüm denetim sistemidir.</span><span class="sxs-lookup"><span data-stu-id="1adde-152">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="1adde-153">Yerel bir Git deposu içinde web uygulamanız için yazdığınız kodu depolayacak ve kodunuzu uzak depoya ileterek Azure'a dağıtacaksınız.</span><span class="sxs-lookup"><span data-stu-id="1adde-153">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="1adde-154">İçine oturum [Azure Portal](https://portal.azure.com), zaten oturum açtınız.</span><span class="sxs-lookup"><span data-stu-id="1adde-154">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="1adde-155">Tıklatın **Gözat**, Gezinti bölmesinin konumunda bulunur.</span><span class="sxs-lookup"><span data-stu-id="1adde-155">Click **Browse**, located at the bottom of the navigation pane.</span></span>

3. <span data-ttu-id="1adde-156">Tıklatın **Web Apps** Azure aboneliğinizle ilişkili web uygulamalarının listesini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="1adde-156">Click **Web Apps** to view a list of the web apps associated with your Azure subscription.</span></span>

4. <span data-ttu-id="1adde-157">Bu öğreticinin önceki bölümde oluşturduğunuz web uygulaması seçin.</span><span class="sxs-lookup"><span data-stu-id="1adde-157">Select the web app you created in the previous section of this tutorial.</span></span>

5. <span data-ttu-id="1adde-158">Varsa **ayarları** dikey gösterilmez, seçin **ayarları** içinde **Web uygulaması** dikey.</span><span class="sxs-lookup"><span data-stu-id="1adde-158">If the **Settings** blade is not shown, select **Settings** in the **Web App** blade.</span></span>

6. <span data-ttu-id="1adde-159">İçinde **ayarları** dikey penceresinde, select **dağıtım kaynağı** > **Kaynağı Seç** > **yerel Git deposu**.</span><span class="sxs-lookup"><span data-stu-id="1adde-159">In the **Settings** blade, select **Deployment source** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Ayarlar dikey: dağıtım kaynak dikey: kaynak dikey seçin](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. <span data-ttu-id="1adde-161">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1adde-161">Click **OK**.</span></span>

8. <span data-ttu-id="1adde-162">Daha önce bir web uygulaması veya diğer App Service uygulama yayımlamak için dağıtım kimlik bilgileri ayarlamadıysanız, bunları şimdi kurun:</span><span class="sxs-lookup"><span data-stu-id="1adde-162">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="1adde-163">Tıklatın **ayarları** > **dağıtım kimlik bilgileri**.</span><span class="sxs-lookup"><span data-stu-id="1adde-163">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="1adde-164">**Dağıtım kimlik bilgilerini ayarla** dikey penceresinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1adde-164">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="1adde-165">Bir kullanıcı adı ve parola oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1adde-165">Create a user name and password.</span></span>  <span data-ttu-id="1adde-166">Git ayarlarken daha sonra bu parola gerekir.</span><span class="sxs-lookup"><span data-stu-id="1adde-166">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="1adde-167">**Kaydet**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1adde-167">Click **Save**.</span></span>

9. <span data-ttu-id="1adde-168">İçinde **Web uygulaması** dikey penceresinde tıklatın **ayarları** > **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="1adde-168">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="1adde-169">İçin dağıtacaksınız uzak Git deposu URL'sini altında gösterilen **GIT URL'si**.</span><span class="sxs-lookup"><span data-stu-id="1adde-169">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

10. <span data-ttu-id="1adde-170">Kopya **GIT URL'si** öğreticide daha sonra kullanmak için değer.</span><span class="sxs-lookup"><span data-stu-id="1adde-170">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure Portal: uygulama özellikleri dikey penceresi](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="1adde-172">Web uygulamanızı Azure App Service'te yayımlama</span><span class="sxs-lookup"><span data-stu-id="1adde-172">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="1adde-173">Bu bölümde, Visual Studio ve anında iletme bu depodan web uygulamanızı dağıtmak için Azure'a kullanarak yerel bir Git deposu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1adde-173">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="1adde-174">Adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="1adde-174">The steps involved include the following:</span></span>

   * <span data-ttu-id="1adde-175">Azure için yerel deponuza dağıtabilmek için GIT URL'si değerini kullanarak uzak depo ayarı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1adde-175">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="1adde-176">Proje değişiklikleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="1adde-176">Commit your project changes.</span></span>

   * <span data-ttu-id="1adde-177">Proje değişikliklerinizi yerel depodan Azure'da uzak deponuza iletin.</span><span class="sxs-lookup"><span data-stu-id="1adde-177">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="1adde-178">İçinde **Çözüm Gezgini** sağ **çözüm 'SampleWebAppDemo'** seçip **yürütme**.</span><span class="sxs-lookup"><span data-stu-id="1adde-178">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="1adde-179">**Takım Gezgini** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1adde-179">The **Team Explorer** will be displayed.</span></span>

    ![Takım Gezgini bağlanmak sekmesi](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="1adde-181">İçinde **Takım Gezgini**seçin **ev** (ev simgesi) > **ayarları** > **deposu ayarları**.</span><span class="sxs-lookup"><span data-stu-id="1adde-181">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="1adde-182">İçinde **uzaktan kumandalar** bölümünü **deposu ayarları** seçin **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="1adde-182">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="1adde-183">**Eklemek uzak** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1adde-183">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="1adde-184">Ayarlama **adı** uzak **Azure ÖrnekUyg**.</span><span class="sxs-lookup"><span data-stu-id="1adde-184">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="1adde-185">Değeri ayarlanamıyor **Fetch** için **Git URL'si** Azure'dan Bu öğreticide daha önce kopyaladığınız.</span><span class="sxs-lookup"><span data-stu-id="1adde-185">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="1adde-186">Bu ile biten URL olduğuna dikkat edin **.git**.</span><span class="sxs-lookup"><span data-stu-id="1adde-186">Note that this is the URL that ends with **.git**.</span></span>

    ![Uzaktan iletişim Düzenle](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="1adde-188">Alternatif olarak, uzak depodan belirtebilirsiniz **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve komutunu girerek.</span><span class="sxs-lookup"><span data-stu-id="1adde-188">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="1adde-189">Örneğin:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="1adde-189">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="1adde-190">Seçin **ev** (ev simgesi) > **ayarları** > **genel ayarları**.</span><span class="sxs-lookup"><span data-stu-id="1adde-190">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="1adde-191">Adınızı ve e-posta adresinizi ayarlanmış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="1adde-191">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="1adde-192">Seçmeniz gerekebilir **güncelleştirme**.</span><span class="sxs-lookup"><span data-stu-id="1adde-192">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="1adde-193">Seçin **giriş** > **değişiklikleri** geri dönmek için **değişiklikleri** görünümü.</span><span class="sxs-lookup"><span data-stu-id="1adde-193">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="1adde-194">Bir kaydetme iletisi gibi girin **ilk anında #1** tıklatıp **tamamlama**.</span><span class="sxs-lookup"><span data-stu-id="1adde-194">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="1adde-195">Bu eylem oluşturacak bir *tamamlama* yerel olarak.</span><span class="sxs-lookup"><span data-stu-id="1adde-195">This action will create a *commit* locally.</span></span> <span data-ttu-id="1adde-196">Sonra *eşitleme* Azure ile.</span><span class="sxs-lookup"><span data-stu-id="1adde-196">Next, you need to *sync* with Azure.</span></span>

    ![Takım Gezgini bağlanmak sekmesi](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="1adde-198">Alternatif olarak, değişikliklerinizi gelen onaylayabilirsiniz **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutları girerek.</span><span class="sxs-lookup"><span data-stu-id="1adde-198">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="1adde-199">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1adde-199">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="1adde-200">Seçin **giriş** > **eşitleme** > **Eylemler** > **komut istemi açın**.</span><span class="sxs-lookup"><span data-stu-id="1adde-200">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="1adde-201">Komut isteminde, proje dizinine açılır.</span><span class="sxs-lookup"><span data-stu-id="1adde-201">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="1adde-202">Komut penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="1adde-202">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="1adde-203">Azure girin **dağıtım kimlik bilgileri** Azure'da daha önce oluşturulan parola.</span><span class="sxs-lookup"><span data-stu-id="1adde-203">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="1adde-204">Girdiğiniz parola görünür olmaz.</span><span class="sxs-lookup"><span data-stu-id="1adde-204">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="1adde-205">Bu komut, yerel proje dosyalarınızın Azure'a dağıtmaya işlemi başlar.</span><span class="sxs-lookup"><span data-stu-id="1adde-205">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="1adde-206">Yukarıdaki komut çıktısı dağıtımının başarılı bir ileti ile sona erer.</span><span class="sxs-lookup"><span data-stu-id="1adde-206">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="1adde-207">Bir proje üzerinde işbirliği yapmak gereksinim duyarsanız için itme düşünmelisiniz [GitHub](https://github.com) arasında Azure'a dağıtmaya.</span><span class="sxs-lookup"><span data-stu-id="1adde-207">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="1adde-208">Etkin dağıtımı doğrulama</span><span class="sxs-lookup"><span data-stu-id="1adde-208">Verify the Active Deployment</span></span>

<span data-ttu-id="1adde-209">Başarılı bir şekilde web uygulamasını yerel ortamınızdan Azure'a aktarılan olduğunu doğrulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1adde-209">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="1adde-210">Listelenen başarılı dağıtım görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="1adde-210">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="1adde-211">İçinde [Azure Portal](https://portal.azure.com), web uygulamanızı seçin.</span><span class="sxs-lookup"><span data-stu-id="1adde-211">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="1adde-212">Ardından, seçin **ayarları** > **sürekli dağıtım**.</span><span class="sxs-lookup"><span data-stu-id="1adde-212">Then, select **Settings** > **Continuous deployment**.</span></span>

   ![Azure Portal: Ayarlar dikey: dağıtımları dikey penceresi başarılı dağıtımı gösterme](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="1adde-214">Uygulamayı Azure'da çalıştırma</span><span class="sxs-lookup"><span data-stu-id="1adde-214">Run the app in Azure</span></span>

<span data-ttu-id="1adde-215">Web uygulamanız için Azure dağıttıysanız, uygulamayı çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1adde-215">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="1adde-216">Bu iki yolla yapılabilir:</span><span class="sxs-lookup"><span data-stu-id="1adde-216">This can be done in two ways:</span></span>

* <span data-ttu-id="1adde-217">Azure portalında web uygulamanız için web uygulaması dikey bulun ve tıklatın **Gözat** varsayılan tarayıcınızda uygulamanızı görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="1adde-217">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="1adde-218">Bir tarayıcı açın ve web uygulamanız için URL'yi girin.</span><span class="sxs-lookup"><span data-stu-id="1adde-218">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="1adde-219">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1adde-219">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="1adde-220">Web uygulamanızı güncelleştirin ve yeniden yayımlamanız</span><span class="sxs-lookup"><span data-stu-id="1adde-220">Update your web app and republish</span></span>

<span data-ttu-id="1adde-221">Yerel koda değişiklikleri yaptıktan sonra yayımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1adde-221">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="1adde-222">İçinde **Çözüm Gezgini** Visual Studio açık *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="1adde-222">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="1adde-223">İçinde `Configure` yöntemini değiştirme `Response.WriteAsync` şekilde aşağıdaki gibi görünmesi yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1adde-223">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="1adde-224">Değişiklikleri kaydedilsin *haline*.</span><span class="sxs-lookup"><span data-stu-id="1adde-224">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="1adde-225">İçinde **Çözüm Gezgini**, sağ **çözüm 'SampleWebAppDemo'** seçip **yürütme**.</span><span class="sxs-lookup"><span data-stu-id="1adde-225">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="1adde-226">**Takım Gezgini** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1adde-226">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="1adde-227">Bir kaydetme iletisi gibi girin:</span><span class="sxs-lookup"><span data-stu-id="1adde-227">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="1adde-228">Tuşuna **tamamlama** proje değişiklikleri yürütmek için düğmesi.</span><span class="sxs-lookup"><span data-stu-id="1adde-228">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="1adde-229">Seçin **giriş** > **eşitleme** > **Eylemler** > **anında**.</span><span class="sxs-lookup"><span data-stu-id="1adde-229">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="1adde-230">Alternatif olarak, değişikliklerden gönderebilir **komut penceresi** açarak **komut penceresi**, proje dizinine değiştirme ve git komutunu girerek.</span><span class="sxs-lookup"><span data-stu-id="1adde-230">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="1adde-231">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1adde-231">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="1adde-232">Görünüm Azure güncelleştirilmiş web uygulaması</span><span class="sxs-lookup"><span data-stu-id="1adde-232">View the updated web app in Azure</span></span>

<span data-ttu-id="1adde-233">Güncelleştirilen web uygulamanızı seçerek görüntülemek **Gözat** Azure Portalı'nda veya bir tarayıcı açıp web uygulamanız için URL girerek web uygulaması dikey penceresinden.</span><span class="sxs-lookup"><span data-stu-id="1adde-233">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="1adde-234">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1adde-234">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="1adde-235">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1adde-235">Additional Resources</span></span>

* [<span data-ttu-id="1adde-236">Yayımlama ve dağıtım</span><span class="sxs-lookup"><span data-stu-id="1adde-236">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="1adde-237">Proje Kudu</span><span class="sxs-lookup"><span data-stu-id="1adde-237">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
