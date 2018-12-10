---
title: ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: Azure'da barındırılan bir ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturmaya uçtan uca yönergeler sağlar. bir kılavuz.
ms.author: casoper
ms.date: 08/07/2018
ms.custom: seodec18
uid: azure/devops/index
ms.openlocfilehash: 6b6f17fd917f32b9e0682414febee10643cf3d13
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121199"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="2c507-103">ASP.NET Core ve Azure ile DevOps</span><span class="sxs-lookup"><span data-stu-id="2c507-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="2c507-104">[![Kapak resmi](./media/cover-large.png)](https://aka.ms/devopsbook)</span><span class="sxs-lookup"><span data-stu-id="2c507-104">[![Cover Image](./media/cover-large.png)](https://aka.ms/devopsbook)</span></span>

<span data-ttu-id="2c507-105">Tarafından [Cam Soper](https://twitter.com/camsoper) ve [Scott Addie](https://twitter.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="2c507-105">By [Cam Soper](https://twitter.com/camsoper) and [Scott Addie](https://twitter.com/scottaddie)</span></span>

<span data-ttu-id="2c507-106">Bu kılavuz olarak kullanılabilir bir [indirilebilir PDF e-kitap](https://aka.ms/devopsbook).</span><span class="sxs-lookup"><span data-stu-id="2c507-106">This guide is available as a [downloadable PDF e-book](https://aka.ms/devopsbook).</span></span>

## <a name="welcome"></a><span data-ttu-id="2c507-107">Hoş Geldiniz</span><span class="sxs-lookup"><span data-stu-id="2c507-107">Welcome</span></span> 

<span data-ttu-id="2c507-108">.NET için Azure geliştirme yaşam döngüsü kılavuzuna Hoş Geldiniz!</span><span class="sxs-lookup"><span data-stu-id="2c507-108">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="2c507-109">Bu kılavuzda, .NET araçları ve işlemleri kullanarak Azure'da geçici bir geliştirme yaşam döngüsü oluşturmanın temel kavramları tanıtır.</span><span class="sxs-lookup"><span data-stu-id="2c507-109">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="2c507-110">Bu kılavuzu tamamladıktan sonra olgun bir DevOps araç zincirinde avantajlarını yararlanabileceksiniz.</span><span class="sxs-lookup"><span data-stu-id="2c507-110">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="2c507-111">Bu kılavuz için olan</span><span class="sxs-lookup"><span data-stu-id="2c507-111">Who this guide is for</span></span>

<span data-ttu-id="2c507-112">Deneyimli bir ASP.NET Core geliştirici (200-300 düzeyi) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c507-112">You should be an experienced ASP.NET Core developer (200-300 level).</span></span> <span data-ttu-id="2c507-113">Biz, bu bölümde ele alacağız gibi Azure hakkında her şeyi bilmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2c507-113">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="2c507-114">Bu kılavuz, ayrıca geliştirme işlemleri daha fazla odaklanan DevOps mühendisleri için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2c507-114">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="2c507-115">Bu kılavuz, Windows geliştiricileri hedefler.</span><span class="sxs-lookup"><span data-stu-id="2c507-115">This guide targets Windows developers.</span></span> <span data-ttu-id="2c507-116">Ancak, Linux ve Macos'ta .NET Core tarafından tam olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="2c507-116">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="2c507-117">Linux/macOS farklar için çağrılar için bu kılavuzu Linux/macOS için uyarlamak üzere izleyin.</span><span class="sxs-lookup"><span data-stu-id="2c507-117">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="2c507-118">Bu kılavuzda ele alınmamıştır</span><span class="sxs-lookup"><span data-stu-id="2c507-118">What this guide doesn't cover</span></span>

<span data-ttu-id="2c507-119">Bu kılavuzda, .NET geliştiricileri için bir uçtan uca sürekli dağıtım deneyimi üzerinde odaklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2c507-119">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="2c507-120">Her şey Azure için ayrıntılı bir kılavuz değildir ve, kapsamlı bir şekilde .NET API'leri Azure Hizmetleri için odak değil.</span><span class="sxs-lookup"><span data-stu-id="2c507-120">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="2c507-121">Vurgu tüm sürekli tümleştirme, dağıtım, izleme ve hata ayıklama ' dir.</span><span class="sxs-lookup"><span data-stu-id="2c507-121">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="2c507-122">Kılavuzu sonuna, sonraki adımlar için öneriler sunulur.</span><span class="sxs-lookup"><span data-stu-id="2c507-122">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="2c507-123">Öneri, ASP.NET Core geliştiricileri için yararlı olan bir Azure platform hizmetlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="2c507-123">Included in the suggestions are Azure platform services that are useful to ASP.NET Core developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="2c507-124">Bu kılavuzda nedir</span><span class="sxs-lookup"><span data-stu-id="2c507-124">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="2c507-125">Araçlar ve indirmeler</span><span class="sxs-lookup"><span data-stu-id="2c507-125">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="2c507-126">Bu kılavuzda kullanılan araçları almayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="2c507-126">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="2c507-127">App Service’e dağıtma</span><span class="sxs-lookup"><span data-stu-id="2c507-127">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="2c507-128">Azure App Service'e bir ASP.NET Core uygulaması dağıtmak için çeşitli yöntemler hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="2c507-128">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="2c507-129">Sürekli tümleştirme ve dağıtım</span><span class="sxs-lookup"><span data-stu-id="2c507-129">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="2c507-130">Bir uçtan uca sürekli tümleştirme ve dağıtım çözümü için GitHub, Azure DevOps Services ve Azure ile ASP.NET Core uygulamanızı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2c507-130">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, Azure DevOps Services, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="2c507-131">İzleme ve hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="2c507-131">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="2c507-132">İzleme, sorun giderme ve uygulamanızı ayarlamak için Azure'nın araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c507-132">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="2c507-133">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="2c507-133">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="2c507-134">Azure Öğrenme ASP.NET Core Geliştirici diğer öğrenme yolları.</span><span class="sxs-lookup"><span data-stu-id="2c507-134">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="additional-introductory-reading"></a><span data-ttu-id="2c507-135">Ek giriş okuma</span><span class="sxs-lookup"><span data-stu-id="2c507-135">Additional introductory reading</span></span>

<span data-ttu-id="2c507-136">Bu bulut için ilk maruz kalma riskinizi ise, bu makaleler temellerini açıklar.</span><span class="sxs-lookup"><span data-stu-id="2c507-136">If this is your first exposure to cloud computing, these articles explain the basics.</span></span>

* [<span data-ttu-id="2c507-137">Bulut bilgi işlem nedir?</span><span class="sxs-lookup"><span data-stu-id="2c507-137">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="2c507-138">Örnekler bulut bilgi işlem</span><span class="sxs-lookup"><span data-stu-id="2c507-138">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="2c507-139">Iaas nedir?</span><span class="sxs-lookup"><span data-stu-id="2c507-139">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="2c507-140">PaaS nedir?</span><span class="sxs-lookup"><span data-stu-id="2c507-140">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
