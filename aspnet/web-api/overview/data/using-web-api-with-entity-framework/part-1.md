---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Entity Framework 6 ile Web API 2 kullanma | Microsoft Docs
author: MikeWasson
description: Bu öğreticide, ASP.NET Web API ile bir web uygulaması oluşturma temelleri arka uç öğretir. Öğretici veri yerleşim için Entity Framework 6 kullanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 8e6d381509a121e3036ca3af91ea3b9bd0be33c2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871980"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="83915-104">Entity Framework 6 ile Web API 2 kullanma</span><span class="sxs-lookup"><span data-stu-id="83915-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="83915-105">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="83915-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="83915-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="83915-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="83915-107">Bu öğreticide, ASP.NET Web API ile bir web uygulaması oluşturma temelleri arka uç öğretir.</span><span class="sxs-lookup"><span data-stu-id="83915-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="83915-108">Öğretici, istemci tarafı JavaScript uygulama için Entity Framework 6 veri katmanı ve Knockout.js için kullanır.</span><span class="sxs-lookup"><span data-stu-id="83915-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="83915-109">Öğreticinin ayrıca Azure App Service Web Apps için uygulama dağıtmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="83915-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="83915-110">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="83915-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="83915-111">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="83915-111">Web API 2.1</span></span>
> - [<span data-ttu-id="83915-112">Visual Studio 2013 Güncelleştirme 2</span><span class="sxs-lookup"><span data-stu-id="83915-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="83915-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="83915-113">Entity Framework 6</span></span>
> - <span data-ttu-id="83915-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="83915-114">.NET 4.5</span></span>
> - <span data-ttu-id="83915-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="83915-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="83915-116">Bu öğretici, bir arka uç veritabanı işleyen bir web uygulaması oluşturmak için Entity Framework 6 ile ASP.NET Web API 2 kullanır.</span><span class="sxs-lookup"><span data-stu-id="83915-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="83915-117">Oluşturacağınız uygulama ekran görüntüsü aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="83915-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="83915-118">Uygulama bir tek sayfalı uygulama (SPA) tasarımı kullanır.</span><span class="sxs-lookup"><span data-stu-id="83915-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="83915-119">"Tek sayfalı uygulama" tek bir HTML sayfası yükler ve ardından sayfanın dinamik olarak yeni sayfa yüklenirken yerine güncelleştiren bir web uygulaması için genel bir terimdir.</span><span class="sxs-lookup"><span data-stu-id="83915-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="83915-120">İlk sayfa yükleme işleminden sonra uygulama AJAX istekleri aracılığıyla sunucusuyla alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="83915-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="83915-121">AJAX UI güncelleştirmek için uygulamanın kullandığı dönüş JSON verilerini ister.</span><span class="sxs-lookup"><span data-stu-id="83915-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="83915-122">AJAX yeni olmayan, ancak bugün oluşturmasına ve büyük ve karmaşık bir SPA uygulama korumasına kolaylaştırmak JavaScript çerçeveleri vardır.</span><span class="sxs-lookup"><span data-stu-id="83915-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="83915-123">Bu öğretici kullanır [Knockout.js](http://knockoutjs.com/), ancak herhangi bir JavaScript istemci çerçevesini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="83915-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="83915-124">Bu uygulama için temel yapı taşlarını şunlardır:</span><span class="sxs-lookup"><span data-stu-id="83915-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="83915-125">ASP.NET MVC HTML sayfası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="83915-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="83915-126">ASP.NET Web API AJAX isteği işler ve JSON verilerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="83915-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="83915-127">Knockout.js verileri-HTML öğeleri JSON verilerini bağlar.</span><span class="sxs-lookup"><span data-stu-id="83915-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="83915-128">Entity Framework veritabanına alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="83915-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="83915-129">Azure üzerinde çalışan bu uygulamayı bakın</span><span class="sxs-lookup"><span data-stu-id="83915-129">See this App Running on Azure</span></span>

<span data-ttu-id="83915-130">Canlı web uygulaması çalışan tamamlanmış site görmek ister misiniz?</span><span class="sxs-lookup"><span data-stu-id="83915-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="83915-131">Aşağıdaki düğmeyi tıklatarak, uygulama tam sürümü Azure hesabınızda dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="83915-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="83915-132">Bu çözüm Azure'a dağıtmak için bir Azure hesabınız olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="83915-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="83915-133">Bir hesap zaten yoksa, aşağıdaki seçenekler vardır:</span><span class="sxs-lookup"><span data-stu-id="83915-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="83915-134">[Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="83915-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="83915-135">[MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.</span><span class="sxs-lookup"><span data-stu-id="83915-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="83915-136">Proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="83915-136">Create the Project</span></span>

<span data-ttu-id="83915-137">Visual Studio'yu açın.</span><span class="sxs-lookup"><span data-stu-id="83915-137">Open Visual Studio.</span></span> <span data-ttu-id="83915-138">Gelen **dosya** menüsünde, select **yeni**seçeneğini belirleyip **proje**.</span><span class="sxs-lookup"><span data-stu-id="83915-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="83915-139">(Veya **yeni proje** başlangıç sayfasında.)</span><span class="sxs-lookup"><span data-stu-id="83915-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="83915-140">İçinde **yeni proje** iletişim kutusunda, tıklatın **Web** sol bölmede ve **ASP.NET Web uygulaması** Orta bölmede.</span><span class="sxs-lookup"><span data-stu-id="83915-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="83915-141">BookService adını verin ve projeyi tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="83915-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="83915-142">İçinde **yeni ASP.NET projesi** iletişim kutusunda **Web API** şablonu.</span><span class="sxs-lookup"><span data-stu-id="83915-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="83915-143">Azure App Service'te projesini barındırmak istiyorsanız, bırak **bulutta Barındır** kutusunu işaretli.</span><span class="sxs-lookup"><span data-stu-id="83915-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="83915-144">Tıklatın **Tamam** projesi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="83915-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="83915-145">(İsteğe bağlı) Azure ayarlarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="83915-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="83915-146">Bırakılırsa **buluttaki konağa** seçeneğinin işaretli, Visual Studio için Microsoft Azure oturum açmak için sorar</span><span class="sxs-lookup"><span data-stu-id="83915-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="83915-147">Azure'da oturum açtıktan sonra Visual Studio web uygulamasını yapılandırma ister.</span><span class="sxs-lookup"><span data-stu-id="83915-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="83915-148">Site için bir ad girin, Azure aboneliğinizi seçin ve bir coğrafi bölge seçin.</span><span class="sxs-lookup"><span data-stu-id="83915-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="83915-149">Altında **veritabanı sunucusu**seçin **oluştur yeni sunucu**.</span><span class="sxs-lookup"><span data-stu-id="83915-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="83915-150">Bir yönetici kullanıcı adı ve parola girin.</span><span class="sxs-lookup"><span data-stu-id="83915-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="83915-151">Next</span><span class="sxs-lookup"><span data-stu-id="83915-151">Next</span></span>](part-2.md)
