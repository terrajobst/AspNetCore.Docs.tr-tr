---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Web API 2 Entity Framework 6 ile kullanma | Microsoft Docs
author: MikeWasson
description: Bu öğreticide arka ucu ASP.NET Web API'si ile bir web uygulaması oluşturma hakkındaki temel bilgileri sağlanır. Öğretici, verileri yerleşim için Entity Framework 6 kullanır...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236529"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="15303-104">Web API 2’yi Entity Framework 6 ile Kullanma</span><span class="sxs-lookup"><span data-stu-id="15303-104">Using Web API 2 with Entity Framework 6</span></span>
====================

[<span data-ttu-id="15303-105">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="15303-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="15303-106">Bu öğreticide bir ASP.NET Web API ile bir web uygulaması oluşturma hakkındaki temel bilgileri arka ucu öğretir.</span><span class="sxs-lookup"><span data-stu-id="15303-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="15303-107">Öğretici, istemci tarafı JavaScript uygulaması için Entity Framework 6 veri katmanı ve Knockout.js için kullanır.</span><span class="sxs-lookup"><span data-stu-id="15303-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="15303-108">Öğreticide ayrıca uygulamasını Azure App Service Web Apps'e dağıtma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="15303-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="15303-109">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="15303-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="15303-110">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="15303-110">Web API 2.1</span></span>
> - <span data-ttu-id="15303-111">Visual Studio 2017 (Visual Studio 2017'yi indirin [burada](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="15303-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="15303-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="15303-112">Entity Framework 6</span></span>
> - <span data-ttu-id="15303-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="15303-113">.NET 4.7</span></span>
> - <span data-ttu-id="15303-114">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="15303-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="15303-115">Bu öğreticide, bir arka uç veritabanı işleyen bir web uygulaması oluşturmak için Entity Framework 6 ile ASP.NET Web API 2 kullanılır.</span><span class="sxs-lookup"><span data-stu-id="15303-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="15303-116">Oluşturacağınız uygulama ekran görüntüsü aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="15303-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="15303-117">Uygulama, bir tek sayfalı uygulama (SPA) tasarım kullanır.</span><span class="sxs-lookup"><span data-stu-id="15303-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="15303-118">"Tek sayfalı uygulama" tek bir HTML sayfası yükler ve ardından sayfanın dinamik olarak yeni sayfa yükleniyor yerine güncelleştiren bir web uygulaması için genel bir terimdir.</span><span class="sxs-lookup"><span data-stu-id="15303-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="15303-119">Başlangıç sayfası yüklemeden sonra uygulama AJAX istekleri aracılığıyla sunucusuyla anlatıyor.</span><span class="sxs-lookup"><span data-stu-id="15303-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="15303-120">AJAX uygulamanın kullanıcı arabirimini güncelleştirmek için kullandığı dönüş JSON verilerini ister.</span><span class="sxs-lookup"><span data-stu-id="15303-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="15303-121">AJAX yeni değildir, ancak bugün oluşturun ve büyük ve karmaşık bir SPA uygulama sürdürmek kolaylaştıran yeni JavaScript çerçevesi vardır.</span><span class="sxs-lookup"><span data-stu-id="15303-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="15303-122">Bu öğreticide [Knockout.js](http://knockoutjs.com/), ancak herhangi bir JavaScript istemci çerçevesini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="15303-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="15303-123">Bu uygulama için temel yapı taşları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="15303-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="15303-124">ASP.NET MVC, HTML sayfası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="15303-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="15303-125">ASP.NET Web API AJAX istekleri işleyen ve JSON verilerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="15303-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="15303-126">Knockout.js veri-HTML öğeleri için JSON verilerini bağlar.</span><span class="sxs-lookup"><span data-stu-id="15303-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="15303-127">Entity Framework veritabanı ile iletişim kuran.</span><span class="sxs-lookup"><span data-stu-id="15303-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="15303-128">Azure üzerinde çalışan bu uygulamayı bakın</span><span class="sxs-lookup"><span data-stu-id="15303-128">See this app running on Azure</span></span>

<span data-ttu-id="15303-129">Canlı web uygulaması olarak çalışan tamamlanmış site görmek ister misiniz?</span><span class="sxs-lookup"><span data-stu-id="15303-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="15303-130">Aşağıdaki düğmesini seçerek Azure hesabınızda bir tam sürümü uygulama dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="15303-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="15303-131">Bu çözüm, Azure'a dağıtmak için bir Azure hesabına ihtiyacınız var.</span><span class="sxs-lookup"><span data-stu-id="15303-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="15303-132">Bir hesap zaten yoksa, aşağıdaki seçenekleriniz:</span><span class="sxs-lookup"><span data-stu-id="15303-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="15303-133">[Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="15303-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="15303-134">[MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.</span><span class="sxs-lookup"><span data-stu-id="15303-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="15303-135">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="15303-135">Create the project</span></span>

<span data-ttu-id="15303-136">Visual Studio'yu açın.</span><span class="sxs-lookup"><span data-stu-id="15303-136">Open Visual Studio.</span></span> <span data-ttu-id="15303-137">Gelen **dosya** menüsünde **yeni**, ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="15303-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="15303-138">(Veya **yeni proje** başlangıç sayfasında.)</span><span class="sxs-lookup"><span data-stu-id="15303-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="15303-139">İçinde **yeni proje** iletişim kutusunda **Web** sol bölmede ve **ASP.NET Web uygulaması (.NET Framework)** orta bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="15303-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="15303-140">Projeyi adlandırın **BookService** seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="15303-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="15303-141">İçinde **yeni ASP.NET projesi** iletişim kutusunda **Web API** şablonu.</span><span class="sxs-lookup"><span data-stu-id="15303-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


<span data-ttu-id="15303-142">Seçin **Tamam** projeyi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="15303-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="15303-143">(İsteğe bağlı) Azure ayarlarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="15303-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="15303-144">Projeyi oluşturduktan sonra Azure App Service Web Apps için herhangi bir zamanda dağıtmayı tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="15303-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="15303-145">Çözüm Gezgini'nde seçin ve proje üzerinde sağ **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="15303-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="15303-146">Açılan pencerede seçin **Başlat**.</span><span class="sxs-lookup"><span data-stu-id="15303-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="15303-147">**Yayımlama hedefi seçin** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="15303-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="15303-148">Seçin **profili oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="15303-148">Select **Create Profile**.</span></span> <span data-ttu-id="15303-149">**App Service Oluştur** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="15303-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="15303-150">Varsayılanları kabul edin veya ilgili uygulama adı, kaynak grubu, barındırma planı, Azure aboneliği ve coğrafi bölge için farklı değerler girebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="15303-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="15303-151">Seçin **SQL veritabanı oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="15303-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="15303-152">**SQL Server'ı Yapılandır** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="15303-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="15303-153">Varsayılanları kabul edin veya farklı değerler girebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="15303-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="15303-154">Girin bir **Yöneticisi kullanıcı adı** ve **yönetici parolası** yeni veritabanınız için.</span><span class="sxs-lookup"><span data-stu-id="15303-154">Enter an **Adminstrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="15303-155">Seçin **Tamam** bitirdiğinizde.</span><span class="sxs-lookup"><span data-stu-id="15303-155">Select **OK** when you're done.</span></span> <span data-ttu-id="15303-156">**App Service Oluştur** sayfası yeniden görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="15303-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="15303-157">Seçin **Oluştur** profilinizi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="15303-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="15303-158">Sağ alt köşedeki dağıtımın devam ettiğini belirten bir ileti görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="15303-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="15303-159">Kısa bir süre sonra **Yayımla** penceresi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="15303-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="15303-160">Uygulamayı dağıtmak için oluşturduğunuz profili artık kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="15303-160">The profile you created to deploy the app is now available.</span></span> 


> [!div class="step-by-step"]
> [<span data-ttu-id="15303-161">Next</span><span class="sxs-lookup"><span data-stu-id="15303-161">Next</span></span>](part-2.md)
