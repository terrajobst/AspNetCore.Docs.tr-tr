---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: "İlgili Kişi Yöneticisi çözüm | Microsoft Docs"
author: jrjlee
description: "Bu öğreticiler dizi bir örnek çözümü #x 2014; Contact Manager çözüm & #x 2014; Kurumsal ölçekte uygulamayla gerçekçi leve temsil etmek için kullandığı..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: b7f691a1ee855788f6a57616aea35d960e4c85c7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
<a name="the-contact-manager-solution"></a><span data-ttu-id="88daa-103">Contact Manager çözümü</span><span class="sxs-lookup"><span data-stu-id="88daa-103">The Contact Manager Solution</span></span>
====================
<span data-ttu-id="88daa-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="88daa-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="88daa-105">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="88daa-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="88daa-106">Bu [eğitim serileri](web-deployment-in-the-enterprise.md) bir örnek çözümü #x 2014; Contact Manager çözüm & #x 2014; Kurumsal ölçekte uygulamayla gerçekçi bir karmaşıklık düzeyi temsil etmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="88daa-106">This [series of tutorials](web-deployment-in-the-enterprise.md) uses a sample solution&#x2014;the Contact Manager solution&#x2014;to represent an enterprise-scale application with a realistic level of complexity.</span></span> <span data-ttu-id="88daa-107">Bu konu, kişinin Yöneticisi çözüm sunar, çözümün anahtar bileşenlerinin açıklar ve bu tür bir uygulama bir kuruluş ortamında çeşitli hedef platformlara dağıtmanın zorluklar tanımlar.</span><span class="sxs-lookup"><span data-stu-id="88daa-107">This topic introduces the Contact Manager solution, describes the key components of the solution, and identifies the challenges in deploying this kind of application to various destination platforms in an enterprise environment.</span></span>
> 
> <span data-ttu-id="88daa-108">Bu öğreticileri konulara çalışırken, kurumsal dağıtım senaryolarında belirli zorluklar nasıl karşılayabileceğini gösteren bir başvuru uygulaması olarak Contact Manager çözümü kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="88daa-108">As you work through the topics in these tutorials, you can use the Contact Manager solution as a reference implementation that demonstrates how you can meet specific challenges in enterprise deployment scenarios.</span></span> <span data-ttu-id="88daa-109">Sonraki konuyu [ayarı yukarı Contact Manager çözüm](setting-up-the-contact-manager-solution.md), indirin ve geliştirici iş istasyonunuza çözümü çalıştırın açıklar.</span><span class="sxs-lookup"><span data-stu-id="88daa-109">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>


## <a name="solution-overview"></a><span data-ttu-id="88daa-110">Çözüme genel bakış</span><span class="sxs-lookup"><span data-stu-id="88daa-110">Solution Overview</span></span>

<span data-ttu-id="88daa-111">Contact Manager çözüm dört ayrı projelerin oluşur:</span><span class="sxs-lookup"><span data-stu-id="88daa-111">The Contact Manager solution consists of four individual projects:</span></span>

![](the-contact-manager-solution/_static/image1.png)

- <span data-ttu-id="88daa-112">**ContactManager.Mvc**.</span><span class="sxs-lookup"><span data-stu-id="88daa-112">**ContactManager.Mvc**.</span></span> <span data-ttu-id="88daa-113">Çözüm için giriş noktası temsil eden bir ASP.NET MVC 3 web uygulaması projesini budur.</span><span class="sxs-lookup"><span data-stu-id="88daa-113">This is an ASP.NET MVC 3 web application project that represents the entry point for the solution.</span></span> <span data-ttu-id="88daa-114">Kullanıcılar oluşturma ve kişi ayrıntılarını görüntüleme olanağı sağlamak gibi bazı temel web uygulaması işlevselliği sunar.</span><span class="sxs-lookup"><span data-stu-id="88daa-114">It offers some basic web application functionality, like providing users with the ability to create and view contact details.</span></span> <span data-ttu-id="88daa-115">Uygulamanın, kişiler ve kimlik doğrulama ve yetkilendirme yönetmek için bir ASP.NET uygulama hizmetleri veritabanına yönetmek için bir Windows Communication Foundation (WCF) Hizmeti'ne bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="88daa-115">The application relies on a Windows Communication Foundation (WCF) service to manage contacts and an ASP.NET application services database to manage authentication and authorization.</span></span>
- <span data-ttu-id="88daa-116">**ContactManager.Database**.</span><span class="sxs-lookup"><span data-stu-id="88daa-116">**ContactManager.Database**.</span></span> <span data-ttu-id="88daa-117">Visual Studio veritabanı projesi budur.</span><span class="sxs-lookup"><span data-stu-id="88daa-117">This is a Visual Studio database project.</span></span> <span data-ttu-id="88daa-118">Proje depoları başvurun ayrıntıları bir veritabanı için şema tanımlar.</span><span class="sxs-lookup"><span data-stu-id="88daa-118">The project defines the schema for a database that stores contact details.</span></span>
- <span data-ttu-id="88daa-119">**ContactManager.Service**.</span><span class="sxs-lookup"><span data-stu-id="88daa-119">**ContactManager.Service**.</span></span> <span data-ttu-id="88daa-120">Bir WCF web hizmeti projesi budur.</span><span class="sxs-lookup"><span data-stu-id="88daa-120">This is a WCF web service project.</span></span> <span data-ttu-id="88daa-121">Arayanlar gerçekleştirmek imkan tanıyan bir uç noktası oluşturma, WCF Hizmeti açığa çıkarır alma, güncelleştirme ve üzerinde Sil (CRUD) işlemleridir **ContactManager** veritabanı.</span><span class="sxs-lookup"><span data-stu-id="88daa-121">The WCF service exposes an endpoint that allows callers to perform create, retrieve, update, and delete (CRUD) operations on the **ContactManager** database.</span></span> <span data-ttu-id="88daa-122">Hizmetin kullandığı **ContactManager** veritabanı ve **ContactManager.Common.dll** derleme.</span><span class="sxs-lookup"><span data-stu-id="88daa-122">The service relies on the **ContactManager** database and the **ContactManager.Common.dll** assembly.</span></span>
- <span data-ttu-id="88daa-123">**ContactManager.Common**.</span><span class="sxs-lookup"><span data-stu-id="88daa-123">**ContactManager.Common**.</span></span> <span data-ttu-id="88daa-124">Bir sınıf kitaplığı proje budur.</span><span class="sxs-lookup"><span data-stu-id="88daa-124">This is a class library project.</span></span> <span data-ttu-id="88daa-125">Bu derlemede tanımlanan türü WCF Hizmeti kullanır.</span><span class="sxs-lookup"><span data-stu-id="88daa-125">The WCF service relies on types defined in this assembly.</span></span>

<span data-ttu-id="88daa-126">Çözüm ayrıca Yayımla adlı bir çözüm klasör içerir.</span><span class="sxs-lookup"><span data-stu-id="88daa-126">The solution also includes a solution folder named Publish.</span></span> <span data-ttu-id="88daa-127">Bu, çeşitli özel proje dosyalarını ve nasıl kontrol edebilir ve derleme ve dağıtım işlemini işlemek göstermek komut dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="88daa-127">This contains various custom project files and command files that demonstrate how you can control and manipulate the build and deployment process.</span></span> <span data-ttu-id="88daa-128">Bu, bu öğreticinin ilerleyen bölümlerinde daha ayrıntılı olarak ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="88daa-128">These are covered in more detail later in this tutorial.</span></span>

<span data-ttu-id="88daa-129">Çözüm bileşenlerden, kavramsal bir düzeyde, bu gibi araya getireceğinizi:</span><span class="sxs-lookup"><span data-stu-id="88daa-129">At a conceptual level, the components of the solution fit together like this:</span></span>

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="88daa-130">ASP.NET MVC 3 web uygulamasını ASP.NET üyelik sağlayıcısı kullanıyor, ancak web uygulaması kapsamındaki tüm sayfaları anonim erişime izin verin.</span><span class="sxs-lookup"><span data-stu-id="88daa-130">While the ASP.NET MVC 3 web application uses the ASP.NET membership provider, all the pages within the web application allow anonymous access.</span></span> <span data-ttu-id="88daa-131">Bu açıkça gerçekçi bir yapılandırma değildir.</span><span class="sxs-lookup"><span data-stu-id="88daa-131">This is clearly not a realistic configuration.</span></span> <span data-ttu-id="88daa-132">Bununla birlikte, çözümü dağıtma ve kullanıcı hesapları ve rolleri yapılandırmadan çözümü test kolaylaştırmak için bu şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="88daa-132">However, the solution is set up in this way to make it easier for you to deploy and test the solution without configuring user accounts and roles.</span></span>


## <a name="deployment-challenges"></a><span data-ttu-id="88daa-133">Dağıtım sorunları</span><span class="sxs-lookup"><span data-stu-id="88daa-133">Deployment Challenges</span></span>

<span data-ttu-id="88daa-134">Contact Manager çözüm kurumsal dağıtım senaryoları çok sayıda için ortak olan birkaç dağıtım zorluklar gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="88daa-134">The Contact Manager solution illustrates several deployment challenges that are common to lots of enterprise deployment scenarios:</span></span>

- <span data-ttu-id="88daa-135">Çözüm birden fazla bağımlı projelerin oluşur.</span><span class="sxs-lookup"><span data-stu-id="88daa-135">The solution consists of multiple dependent projects.</span></span> <span data-ttu-id="88daa-136">Bu projeleri eşzamanlı olarak dağıtmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="88daa-136">You need to deploy these projects simultaneously.</span></span>
- <span data-ttu-id="88daa-137">Bağlantı dizeleri ve hizmet uç noktaları her ortam için güncelleştirilmesi gereken ve çok durumlarda bu bilgileri geliştiriciler için kullanılabilir olmayacak.</span><span class="sxs-lookup"><span data-stu-id="88daa-137">Connection strings and service endpoints need to be updated for each environment, and in a lot of cases this information will not be available to the developer.</span></span>
- <span data-ttu-id="88daa-138">Dağıtırken **ContactManager** veritabanı hazırlama ve üretim ortamları için sonraki dağıtımlarda mevcut verilerinizi korumak gerekir.</span><span class="sxs-lookup"><span data-stu-id="88daa-138">When you deploy the **ContactManager** database to staging and production environments, you need to preserve existing data on subsequent deployments.</span></span>
- <span data-ttu-id="88daa-139">ASP.NET uygulama hizmetleri veritabanına dağıttığınızda, bazı yapılandırma verilerini dağıtmak ancak herhangi bir kullanıcı hesabı veri atlayın gerekir.</span><span class="sxs-lookup"><span data-stu-id="88daa-139">When you deploy the ASP.NET application services database, you need to deploy some configuration data but omit any user account data.</span></span>
- <span data-ttu-id="88daa-140">Bazı dosya ve klasörleri değil dağıtılmalıdır projeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="88daa-140">The projects include some files and folders that should not be deployed.</span></span> <span data-ttu-id="88daa-141">Bu dosyaları ve klasörleri dağıtım işleminin dışında tutmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="88daa-141">You need to exclude these files and folders from the deployment process.</span></span>
- <span data-ttu-id="88daa-142">Çözüm, Team Foundation Server (TFS) Yapı sunucusundan otomatik dağıtım desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="88daa-142">The solution needs to support automated deployment from a Team Foundation Server (TFS) build server.</span></span>

## <a name="conclusion"></a><span data-ttu-id="88daa-143">Sonuç</span><span class="sxs-lookup"><span data-stu-id="88daa-143">Conclusion</span></span>

<span data-ttu-id="88daa-144">Bu konuda Contact Manager çözüm üst düzey bir genel bakış sağlanmış ve bazı kurumsal dağıtım senaryoları çok sayıda için ortak olan devralınmış dağıtım zorlukların tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="88daa-144">This topic provided a high-level overview of the Contact Manager solution and identified some of the inherent deployment challenges that are common to lots of enterprise deployment scenarios.</span></span> <span data-ttu-id="88daa-145">Bu öğreticide diğer konular bu zorluklar karşılamak için kullanabileceğiniz yöntemlerden bazıları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="88daa-145">The remaining topics in this tutorial describe some of the techniques you can use to meet these challenges.</span></span>

<span data-ttu-id="88daa-146">Sonraki konuyu [ayarı yukarı Contact Manager çözüm](setting-up-the-contact-manager-solution.md), indirin ve geliştirici iş istasyonunuza çözümü çalıştırın açıklar.</span><span class="sxs-lookup"><span data-stu-id="88daa-146">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="88daa-147">[Önceki](web-deployment-in-the-enterprise.md)
[sonraki](setting-up-the-contact-manager-solution.md)</span><span class="sxs-lookup"><span data-stu-id="88daa-147">[Previous](web-deployment-in-the-enterprise.md)
[Next](setting-up-the-contact-manager-solution.md)</span></span>
