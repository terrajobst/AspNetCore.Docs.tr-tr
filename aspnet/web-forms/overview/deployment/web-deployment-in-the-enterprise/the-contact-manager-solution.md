---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Kişi Yöneticisi çözümü | Microsoft Docs
author: jrjlee
description: Bu öğretici serisinde, kullanan örnek bir çözüm&#x2014;Kişi Yöneticisi çözümünü&#x2014;kurumsal ölçekli uygulamayla gerçekçi leve temsil etmek için...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 8187766190da43ded52359892601f8129b9940ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388114"
---
<a name="the-contact-manager-solution"></a><span data-ttu-id="d5919-103">Kişi Yöneticisi çözümü</span><span class="sxs-lookup"><span data-stu-id="d5919-103">The Contact Manager Solution</span></span>
====================
<span data-ttu-id="d5919-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="d5919-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="d5919-105">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="d5919-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="d5919-106">Bu [öğretici serisinde,](web-deployment-in-the-enterprise.md) kullanan örnek bir çözüm&#x2014;Kişi Yöneticisi çözümünü&#x2014;kurumsal ölçekli uygulamayla gerçekçi bir karmaşıklık düzeyi temsil etmek için.</span><span class="sxs-lookup"><span data-stu-id="d5919-106">This [series of tutorials](web-deployment-in-the-enterprise.md) uses a sample solution&#x2014;the Contact Manager solution&#x2014;to represent an enterprise-scale application with a realistic level of complexity.</span></span> <span data-ttu-id="d5919-107">Bu konu, kişi yöneticisi çözümünü sunar, çözüm'in temel bileşenlerinden açıklar ve bu tür bir kuruluş ortamında çeşitli Hedef platformlar için uygulama dağıtma karşılaşılan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5919-107">This topic introduces the Contact Manager solution, describes the key components of the solution, and identifies the challenges in deploying this kind of application to various destination platforms in an enterprise environment.</span></span>
> 
> <span data-ttu-id="d5919-108">Aşağıdaki öğreticilerde konu başlıklarını çalışırken, kurumsal dağıtım senaryolarında özgü zorlukları nasıl karşıladığını gösteren bir başvuru uygulaması olarak kişi yöneticisi çözümünü kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d5919-108">As you work through the topics in these tutorials, you can use the Contact Manager solution as a reference implementation that demonstrates how you can meet specific challenges in enterprise deployment scenarios.</span></span> <span data-ttu-id="d5919-109">Bir sonraki konu [ayarı oluşturan kişi yöneticisi çözümü](setting-up-the-contact-manager-solution.md), indirmek ve çözümü Geliştirici iş istasyonunuzda çalıştırmak açıklar.</span><span class="sxs-lookup"><span data-stu-id="d5919-109">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>


## <a name="solution-overview"></a><span data-ttu-id="d5919-110">Çözüme genel bakış</span><span class="sxs-lookup"><span data-stu-id="d5919-110">Solution Overview</span></span>

<span data-ttu-id="d5919-111">Kişi Yöneticisi çözümünü dört ayrı ayrı projelerin oluşur:</span><span class="sxs-lookup"><span data-stu-id="d5919-111">The Contact Manager solution consists of four individual projects:</span></span>

![](the-contact-manager-solution/_static/image1.png)

- <span data-ttu-id="d5919-112">**ContactManager.Mvc**.</span><span class="sxs-lookup"><span data-stu-id="d5919-112">**ContactManager.Mvc**.</span></span> <span data-ttu-id="d5919-113">Çözüm için giriş noktasını temsil eden bir ASP.NET MVC 3 web uygulaması projesi budur.</span><span class="sxs-lookup"><span data-stu-id="d5919-113">This is an ASP.NET MVC 3 web application project that represents the entry point for the solution.</span></span> <span data-ttu-id="d5919-114">Kullanıcıları oluşturma ve kişi ayrıntılarını görüntüleme olanağı sağlama gibi bazı temel web uygulaması işlevselliği sunar.</span><span class="sxs-lookup"><span data-stu-id="d5919-114">It offers some basic web application functionality, like providing users with the ability to create and view contact details.</span></span> <span data-ttu-id="d5919-115">Uygulamanın, kişiler ve kimlik doğrulama ve yetkilendirme yönetmek için bir ASP.NET uygulama hizmetleri veritabanına yönetmek için bir Windows Communication Foundation (WCF) hizmeti kullanır.</span><span class="sxs-lookup"><span data-stu-id="d5919-115">The application relies on a Windows Communication Foundation (WCF) service to manage contacts and an ASP.NET application services database to manage authentication and authorization.</span></span>
- <span data-ttu-id="d5919-116">**ContactManager.Database**.</span><span class="sxs-lookup"><span data-stu-id="d5919-116">**ContactManager.Database**.</span></span> <span data-ttu-id="d5919-117">Visual Studio veritabanı projesi budur.</span><span class="sxs-lookup"><span data-stu-id="d5919-117">This is a Visual Studio database project.</span></span> <span data-ttu-id="d5919-118">Proje depoları ayrıntıları başvurun bir veritabanı için şema tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d5919-118">The project defines the schema for a database that stores contact details.</span></span>
- <span data-ttu-id="d5919-119">**ContactManager.Service**.</span><span class="sxs-lookup"><span data-stu-id="d5919-119">**ContactManager.Service**.</span></span> <span data-ttu-id="d5919-120">Bir WCF web hizmeti projesi budur.</span><span class="sxs-lookup"><span data-stu-id="d5919-120">This is a WCF web service project.</span></span> <span data-ttu-id="d5919-121">Alma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştirin izin veren bir uç nokta oluşturma, WCF Hizmeti kullanıma sunan **ContactManager** veritabanı.</span><span class="sxs-lookup"><span data-stu-id="d5919-121">The WCF service exposes an endpoint that allows callers to perform create, retrieve, update, and delete (CRUD) operations on the **ContactManager** database.</span></span> <span data-ttu-id="d5919-122">Hizmet **ContactManager** veritabanı ve **ContactManager.Common.dll** derleme.</span><span class="sxs-lookup"><span data-stu-id="d5919-122">The service relies on the **ContactManager** database and the **ContactManager.Common.dll** assembly.</span></span>
- <span data-ttu-id="d5919-123">**ContactManager.Common**.</span><span class="sxs-lookup"><span data-stu-id="d5919-123">**ContactManager.Common**.</span></span> <span data-ttu-id="d5919-124">Bir sınıf kitaplığı projesi budur.</span><span class="sxs-lookup"><span data-stu-id="d5919-124">This is a class library project.</span></span> <span data-ttu-id="d5919-125">Bu derlemede tanımlanan türlerin WCF Hizmeti kullanır.</span><span class="sxs-lookup"><span data-stu-id="d5919-125">The WCF service relies on types defined in this assembly.</span></span>

<span data-ttu-id="d5919-126">Çözüm ayrıca Yayımla adlı bir çözüm klasörü içerir.</span><span class="sxs-lookup"><span data-stu-id="d5919-126">The solution also includes a solution folder named Publish.</span></span> <span data-ttu-id="d5919-127">Bu, çeşitli özel proje dosyalarını ve nasıl kontrol edebilir ve düzenleme derleme ve dağıtım sürecini gösteren komut dosyalarını içerir.</span><span class="sxs-lookup"><span data-stu-id="d5919-127">This contains various custom project files and command files that demonstrate how you can control and manipulate the build and deployment process.</span></span> <span data-ttu-id="d5919-128">Bunlar, bu öğreticinin ilerleyen bölümlerinde daha ayrıntılı olarak ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d5919-128">These are covered in more detail later in this tutorial.</span></span>

<span data-ttu-id="d5919-129">Çözüm bileşenleri, kavramsal bir düzeyde şöyle araya getireceğinizi:</span><span class="sxs-lookup"><span data-stu-id="d5919-129">At a conceptual level, the components of the solution fit together like this:</span></span>

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="d5919-130">Web uygulaması kapsamındaki tüm sayfaları, ASP.NET MVC 3 web uygulamasına ASP.NET üyelik sağlayıcısını kullanırken, anonim erişime izin.</span><span class="sxs-lookup"><span data-stu-id="d5919-130">While the ASP.NET MVC 3 web application uses the ASP.NET membership provider, all the pages within the web application allow anonymous access.</span></span> <span data-ttu-id="d5919-131">Bu açıkça gerçekçi bir yapılandırma değildir.</span><span class="sxs-lookup"><span data-stu-id="d5919-131">This is clearly not a realistic configuration.</span></span> <span data-ttu-id="d5919-132">Ancak, çözümü dağıtın ve kullanıcı hesapları ve rollerini yapılandırmadan çözümü test kolaylaştırmak için bu şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d5919-132">However, the solution is set up in this way to make it easier for you to deploy and test the solution without configuring user accounts and roles.</span></span>


## <a name="deployment-challenges"></a><span data-ttu-id="d5919-133">Dağıtım zorlukları</span><span class="sxs-lookup"><span data-stu-id="d5919-133">Deployment Challenges</span></span>

<span data-ttu-id="d5919-134">Kişi Yöneticisi çözümü, kurumsal dağıtım senaryolarında çok sayıda için ortak olan birkaç dağıtım zorlukları gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="d5919-134">The Contact Manager solution illustrates several deployment challenges that are common to lots of enterprise deployment scenarios:</span></span>

- <span data-ttu-id="d5919-135">Çözüm, birden çok bağımlı projeler oluşur.</span><span class="sxs-lookup"><span data-stu-id="d5919-135">The solution consists of multiple dependent projects.</span></span> <span data-ttu-id="d5919-136">Bu projeler eş zamanlı olarak dağıtılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d5919-136">You need to deploy these projects simultaneously.</span></span>
- <span data-ttu-id="d5919-137">Bağlantı dizelerini ve hizmet uç noktaları her ortam için güncelleştirilmesi gereken ve çok durumlarda bu bilgiler geliştiriciler için kullanılabilir olmayacak.</span><span class="sxs-lookup"><span data-stu-id="d5919-137">Connection strings and service endpoints need to be updated for each environment, and in a lot of cases this information will not be available to the developer.</span></span>
- <span data-ttu-id="d5919-138">Dağıtırken **ContactManager** veritabanı sonraki dağıtımlarda mevcut verilerinizi korumak gereken hazırlık ve üretim ortamları için.</span><span class="sxs-lookup"><span data-stu-id="d5919-138">When you deploy the **ContactManager** database to staging and production environments, you need to preserve existing data on subsequent deployments.</span></span>
- <span data-ttu-id="d5919-139">ASP.NET uygulama hizmetleri veritabanına dağıttığınızda, bazı yapılandırma verilerini dağıtmak, ancak herhangi bir kullanıcı hesabı veri atlarsanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d5919-139">When you deploy the ASP.NET application services database, you need to deploy some configuration data but omit any user account data.</span></span>
- <span data-ttu-id="d5919-140">Bazı dosya ve klasörlerin değil dağıtılmalıdır projeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="d5919-140">The projects include some files and folders that should not be deployed.</span></span> <span data-ttu-id="d5919-141">Bu dosya ve klasörler, dağıtım işleminin dışında tutmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d5919-141">You need to exclude these files and folders from the deployment process.</span></span>
- <span data-ttu-id="d5919-142">Team Foundation Server (TFS) yapı sunucusu otomatik dağıtımı desteklemek çözüm gerekir.</span><span class="sxs-lookup"><span data-stu-id="d5919-142">The solution needs to support automated deployment from a Team Foundation Server (TFS) build server.</span></span>

## <a name="conclusion"></a><span data-ttu-id="d5919-143">Sonuç</span><span class="sxs-lookup"><span data-stu-id="d5919-143">Conclusion</span></span>

<span data-ttu-id="d5919-144">Bu konuda sağlanan Kişi Yöneticisi çözümünü üst düzey bir genel bakış ve birçok kurumsal dağıtım senaryoları için ortak olan doğal dağıtım zorlukları bazılarını belirledik.</span><span class="sxs-lookup"><span data-stu-id="d5919-144">This topic provided a high-level overview of the Contact Manager solution and identified some of the inherent deployment challenges that are common to lots of enterprise deployment scenarios.</span></span> <span data-ttu-id="d5919-145">Bu öğreticide kalan konularda bu zorluklar karşılamak için kullanabileceğiniz yöntemlerden bazıları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d5919-145">The remaining topics in this tutorial describe some of the techniques you can use to meet these challenges.</span></span>

<span data-ttu-id="d5919-146">Bir sonraki konu [ayarı oluşturan kişi yöneticisi çözümü](setting-up-the-contact-manager-solution.md), indirmek ve çözümü Geliştirici iş istasyonunuzda çalıştırmak açıklar.</span><span class="sxs-lookup"><span data-stu-id="d5919-146">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d5919-147">[Önceki](web-deployment-in-the-enterprise.md)
> [İleri](setting-up-the-contact-manager-solution.md)</span><span class="sxs-lookup"><span data-stu-id="d5919-147">[Previous](web-deployment-in-the-enterprise.md)
[Next](setting-up-the-contact-manager-solution.md)</span></span>
