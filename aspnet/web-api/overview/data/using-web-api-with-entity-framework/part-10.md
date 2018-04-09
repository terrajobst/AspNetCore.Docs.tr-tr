---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Azure Azure App Service'e uygulama yayımlama | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="38e90-102">Azure Azure App Service'e uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="38e90-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="38e90-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="38e90-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="38e90-104">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="38e90-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="38e90-105">Son adım olarak, uygulamayı Azure'a yayımlayacak.</span><span class="sxs-lookup"><span data-stu-id="38e90-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="38e90-106">Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="38e90-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="38e90-107">Tıklatarak **Yayımla** çağırır **Web'i Yayımla** iletişim.</span><span class="sxs-lookup"><span data-stu-id="38e90-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="38e90-108">İşaretlendiğinde, **buluttaki konağa** zaman ilk proje sonra bağlantı oluşturulur ve ayarlar zaten yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="38e90-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="38e90-109">Bu durumda, yalnızca tıklatın **ayarları** sekmesinde ve denetleyin &quot;Code First Migrations yürütme&quot;.</span><span class="sxs-lookup"><span data-stu-id="38e90-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="38e90-110">(Onay kaydetmedi **buluttaki konağa** başında sonra adımları [sonraki bölümde](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="38e90-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="38e90-111">Uygulama dağıtmak için **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="38e90-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="38e90-112">Yayımlama sürüyor görüntüleyebilirsiniz **Web yayımlama etkinliğini** penceresi.</span><span class="sxs-lookup"><span data-stu-id="38e90-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="38e90-113">(Gelen **Görünüm** menüsünde, select **diğer pencereler**seçeneğini belirleyip **Web yayımlama etkinliğini**.)</span><span class="sxs-lookup"><span data-stu-id="38e90-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="38e90-114">Visual Studio uygulama dağıtma sona erdiğinde, varsayılan tarayıcı otomatik olarak dağıtılan Web sitesinin URL'sini açar ve oluşturduğunuz uygulama bulutta şu anda çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="38e90-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="38e90-115">Tarayıcı adres çubuğundaki URL'yi site Internet'ten yüklü olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="38e90-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="38e90-116">Yeni bir Web sitesine dağıtma</span><span class="sxs-lookup"><span data-stu-id="38e90-116">Deploying to a New Website</span></span>

<span data-ttu-id="38e90-117">Değil onay **buluttaki konağa** ilk proje oluşturduğunuzda, yeni bir web uygulaması artık yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38e90-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="38e90-118">Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="38e90-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="38e90-119">Seçin **profil** sekmesinde **Microsoft Azure Web siteleri**.</span><span class="sxs-lookup"><span data-stu-id="38e90-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="38e90-120">Azure için şu anda imzalı değil, oturum açmak için istenir.</span><span class="sxs-lookup"><span data-stu-id="38e90-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="38e90-121">İçinde **mevcut Web siteleri** iletişim kutusunda, tıklatın **yeni**.</span><span class="sxs-lookup"><span data-stu-id="38e90-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="38e90-122">Bir site adı girin.</span><span class="sxs-lookup"><span data-stu-id="38e90-122">Enter a site name.</span></span> <span data-ttu-id="38e90-123">Azure aboneliğinizi ve bölge seçin.</span><span class="sxs-lookup"><span data-stu-id="38e90-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="38e90-124">Altında **veritabanı sunucusu**seçin **Create New Server**, var olan bir sunucu veya seçin.</span><span class="sxs-lookup"><span data-stu-id="38e90-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="38e90-125">**Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="38e90-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="38e90-126">' I tıklatın **ayarları** sekmesinde ve denetleme &quot;Code First Migrations yürütme&quot;.</span><span class="sxs-lookup"><span data-stu-id="38e90-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="38e90-127">Ardından **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="38e90-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="38e90-128">Önceki</span><span class="sxs-lookup"><span data-stu-id="38e90-128">Previous</span></span>](part-9.md)
