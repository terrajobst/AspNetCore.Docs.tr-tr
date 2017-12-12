---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: "Senaryo: bir Test ortamı için Web dağıtımı yapılandırma | Microsoft Docs"
author: jrjlee
description: "Bu konu, geliştirici için tipik web dağıtım senaryosu açıklanmaktadır veya sınama ortamlarında ve si ayarlamak için tamamlanması gereken görevler açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 008b9cd081152e6a378d0fa2e08497a6771fd9b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="09812-103">Senaryo: bir Test ortamı için Web dağıtımı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="09812-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="09812-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="09812-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="09812-105">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="09812-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="09812-106">Bu konu, geliştirici için tipik web dağıtım senaryosu açıklanmaktadır veya sınama ortamlarında ve benzer bir ortamı kurmanız için tamamlanması gereken görevler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="09812-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="09812-107">Geliştiricilerin web uygulamaları üzerinde çalışırken, bunlar genellikle erişim uygulamalarını değişiklikler gerçekçi ayarında test etmek için kullanabileceğiniz bir sunucu ortamı için verilir.</span><span class="sxs-lookup"><span data-stu-id="09812-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="09812-108">Bu tür bir geliştirme veya test ortamına genellikle şu özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="09812-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="09812-109">Ortamında bir tek bir web sunucusu ve tek veritabanı sunucusu oluşur.</span><span class="sxs-lookup"><span data-stu-id="09812-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="09812-110">Geliştiriciler genellikle uygulamalarını gereksinimleri için ortamı yapılandırmak bildirmek için sunucularında yönetici ayrıcalıklarına sahip.</span><span class="sxs-lookup"><span data-stu-id="09812-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="09812-111">Uygulamaları değişiklikleri düzenli aralıklarla üzerinde dağıtılır, bu nedenle ortamı tek adımlı desteklemesi gerekir veya otomatik dağıtım.</span><span class="sxs-lookup"><span data-stu-id="09812-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="09812-112">Örneğin, bizim [öğretici senaryo](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink olan bir geliştirici, Fabrikam, Inc. Matt Contact Manager çözüm üzerinde çalışmaktadır ve değişiklikleri bir test ortamını dağıtmak düzenli olarak gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="09812-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="09812-113">Matt test web sunucusunda ve test veritabanı sunucusunda bir yöneticidir.</span><span class="sxs-lookup"><span data-stu-id="09812-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="09812-114">Başlangıçta, Matt çözümü doğrudan sınama ortamında dağıtmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="09812-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="09812-115">İş ilerler ve geliştiricilerin kişi çözüm sürekli tümleştirme (CI) Team Foundation Server (TFS) için yapılandırılmış Yöneticisi'ni takım katılın.</span><span class="sxs-lookup"><span data-stu-id="09812-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="09812-116">İçeriğinde bir geliştirici denetler her ekip çözümü oluşturun, tüm birim testleri çalıştırma ve otomatik olarak çözümü sınama ortamında dağıtmak.</span><span class="sxs-lookup"><span data-stu-id="09812-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="09812-117">Çözüme genel bakış</span><span class="sxs-lookup"><span data-stu-id="09812-117">Solution Overview</span></span>

<span data-ttu-id="09812-118">Sınama ortamı tek adımlı desteklemesi gerekir veya uzak bir bilgisayardan dağıtım iki ana yaklaşım seçmesini şekilde otomatik.</span><span class="sxs-lookup"><span data-stu-id="09812-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="09812-119">Şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="09812-119">You can:</span></span>

- <span data-ttu-id="09812-120">Web dağıtım aracı hizmetini ("Uzak Aracı") kullanarak Dağıtımınızı desteklemek için test web sunucusunu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="09812-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="09812-121">Web dağıtımı işleyici kullanarak dağıtımı desteklemek üzere test web sunucusunu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="09812-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="09812-122">De kullanabilirsiniz [Web dağıtımı isteğe bağlı](https://technet.microsoft.com/en-us/library/ee517345(WS.10).aspx) ("geçici Aracısı").</span><span class="sxs-lookup"><span data-stu-id="09812-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/en-us/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="09812-123">Bu gereksinimleri ve kısıtlamaları bakımından uzak aracı yaklaşım benzer.</span><span class="sxs-lookup"><span data-stu-id="09812-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="09812-124">Bu durumda, geliştiriciler, hedef sunucular üzerinde yönetici ayrıcalıklarına sahip ve mantıksal seçimi uzak aracı kullanarak dağıtımı desteklemek üzere test web sunucusunu yapılandırma olacak şekilde test ortamı sıkı güvenlik kısıtlamaları tabi değil.</span><span class="sxs-lookup"><span data-stu-id="09812-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="09812-125">Bu daha az karmaşıktır ve Web dağıtımı işleyicisi yaklaşımı daha az başlangıç yapılandırmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="09812-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="09812-126">Uzaktan erişim ve dağıtımını desteklemek için veritabanı sunucunuzu yapılandırmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="09812-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="09812-127">Bu konular bu görevleri tamamlamak için gereken tüm bilgileri sağlar:</span><span class="sxs-lookup"><span data-stu-id="09812-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="09812-128">[Web dağıtımı için yayımlama (Uzak Aracı) bir Web sunucusu yapılandırma](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="09812-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="09812-129">Bu konu, temiz bir Windows Server 2008 R2 derleme başlatma yayımlama, uzak aracı yaklaşımı kullanarak Web dağıtımını destekleyen bir web sunucusu oluşturmak açıklar.</span><span class="sxs-lookup"><span data-stu-id="09812-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="09812-130">[Web dağıtımı yayımlama için veritabanı sunucusunu yapılandırın](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="09812-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="09812-131">Bu konuda, uzaktan erişim ve dağıtım, bir SQL Server 2008 R2'in varsayılan yüklemesinden başlatma desteklemek için veritabanı sunucusunun nasıl yapılandırılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="09812-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="09812-132">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="09812-132">Further Reading</span></span>

<span data-ttu-id="09812-133">Tipik bir hazırlama ortamını yapılandırma ile ilgili yönergeler için bkz: [senaryo: Web dağıtımı için bir hazırlama ortamını yapılandırma](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="09812-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="09812-134">Tipik bir üretim ortamını yapılandırma ile ilgili yönergeler için bkz: [senaryo: bir üretim ortamında Web dağıtım için yapılandırma](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="09812-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="09812-135">[Önceki](choosing-the-right-approach-to-web-deployment.md)
[sonraki](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="09812-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
