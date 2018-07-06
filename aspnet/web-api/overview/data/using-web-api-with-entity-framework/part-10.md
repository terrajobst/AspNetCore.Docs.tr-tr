---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Azure App Service'e Azure uygulama yayımlama | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 66dde7b54ce084eed873afae56fd686d0dc8795f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808233"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="01900-102">Uygulamayı Azure Azure App Service'e yayımlama</span><span class="sxs-lookup"><span data-stu-id="01900-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="01900-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="01900-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="01900-104">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="01900-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="01900-105">Son adım olarak, uygulamayı azure'a yayımlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="01900-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="01900-106">Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="01900-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="01900-107">Tıklayarak **Yayımla** çağırır **Web'i Yayımla** iletişim.</span><span class="sxs-lookup"><span data-stu-id="01900-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="01900-108">İşaretlediyseniz **buluttaki konak** zaman önce proje sonra bağlantı oluşturduğunuz ve ayarları zaten yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="01900-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="01900-109">Bu durumda, tıklamanız yeterlidir **ayarları** denetleyin ve sekme &quot;yürütme Code First Migrations&quot;.</span><span class="sxs-lookup"><span data-stu-id="01900-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="01900-110">(Siz işaretlerseniz **buluttaki konak** başında, ardından adımları izleyin [sonraki bölümde](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="01900-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="01900-111">Uygulamayı dağıtmak için **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="01900-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="01900-112">Yayımlama ilerleme durumunu görüntüleyebileceğiniz **Web yayımlama etkinliği** penceresi.</span><span class="sxs-lookup"><span data-stu-id="01900-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="01900-113">(Gelen **görünümü** menüsünde **diğer Windows**, ardından **Web yayımlama etkinliği**.)</span><span class="sxs-lookup"><span data-stu-id="01900-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="01900-114">Visual Studio uygulama dağıtımı tamamlandığında, varsayılan tarayıcı otomatik olarak dağıtılan Web sitesinin URL'sini açar ve oluşturduğunuz uygulama artık bulutta çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="01900-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="01900-115">Tarayıcı adres çubuğundaki URL, Internet'ten site yüklendiği gösterir.</span><span class="sxs-lookup"><span data-stu-id="01900-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="01900-116">Yeni bir Web sitesine dağıtma</span><span class="sxs-lookup"><span data-stu-id="01900-116">Deploying to a New Website</span></span>

<span data-ttu-id="01900-117">Değil kontrol ettiniz, **buluttaki konak** proje ilk oluşturulduğunda, yeni bir web uygulaması artık yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="01900-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="01900-118">Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="01900-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="01900-119">Seçin **profili** sekmesine **Microsoft Azure Web siteleri**.</span><span class="sxs-lookup"><span data-stu-id="01900-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="01900-120">Azure'da şu anda oturum açmadıysanız, oturum açmanız istenir.</span><span class="sxs-lookup"><span data-stu-id="01900-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="01900-121">İçinde **var olan Web siteleri** iletişim kutusunda, tıklayın **yeni**.</span><span class="sxs-lookup"><span data-stu-id="01900-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="01900-122">Bir site adı girin.</span><span class="sxs-lookup"><span data-stu-id="01900-122">Enter a site name.</span></span> <span data-ttu-id="01900-123">Azure aboneliğinizi ve bölgeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="01900-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="01900-124">Altında **veritabanı sunucusu**seçin **yeni sunucu oluşturma**, veya mevcut bir sunucu seçin.</span><span class="sxs-lookup"><span data-stu-id="01900-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="01900-125">**Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="01900-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="01900-126">Tıklayın **ayarları** denetleyin ve sekme &quot;Code First Migrations yürütme&quot;.</span><span class="sxs-lookup"><span data-stu-id="01900-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="01900-127">Ardından **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="01900-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="01900-128">Önceki</span><span class="sxs-lookup"><span data-stu-id="01900-128">Previous</span></span>](part-9.md)
