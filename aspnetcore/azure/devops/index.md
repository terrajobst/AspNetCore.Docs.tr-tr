---
title: ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: Azure'da barındırılan bir ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturmaya uçtan uca yönergeler sağlar. bir kılavuz.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722731"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="33fe0-103">ASP.NET Core ve Azure ile DevOps</span><span class="sxs-lookup"><span data-stu-id="33fe0-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="33fe0-104">.NET için Azure geliştirme yaşam döngüsü kılavuzuna Hoş Geldiniz!</span><span class="sxs-lookup"><span data-stu-id="33fe0-104">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="33fe0-105">Bu kılavuzda, .NET araçları ve işlemleri kullanarak Azure'da geçici bir geliştirme yaşam döngüsü oluşturmanın temel kavramları tanıtır.</span><span class="sxs-lookup"><span data-stu-id="33fe0-105">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="33fe0-106">Bu kılavuzu tamamladıktan sonra olgun bir DevOps araç zincirinde avantajlarını yararlanabileceksiniz.</span><span class="sxs-lookup"><span data-stu-id="33fe0-106">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="33fe0-107">Bu kılavuz için olan</span><span class="sxs-lookup"><span data-stu-id="33fe0-107">Who this guide is for</span></span>

<span data-ttu-id="33fe0-108">Deneyimli bir ASP.NET geliştiricisi (200-300 düzeyi) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="33fe0-108">You should be an experienced ASP.NET developer (200-300 level).</span></span> <span data-ttu-id="33fe0-109">Biz, bu bölümde ele alacağız gibi Azure hakkında her şeyi bilmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="33fe0-109">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="33fe0-110">Bu kılavuz, ayrıca geliştirme işlemleri daha fazla odaklanan DevOps mühendisleri için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="33fe0-110">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="33fe0-111">Bu kılavuz, Windows geliştiricileri hedefler.</span><span class="sxs-lookup"><span data-stu-id="33fe0-111">This guide targets Windows developers.</span></span> <span data-ttu-id="33fe0-112">Ancak, Linux ve Macos'ta .NET Core tarafından tam olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="33fe0-112">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="33fe0-113">Linux/macOS farklar için çağrılar için bu kılavuzu Linux/macOS için uyarlamak üzere izleyin.</span><span class="sxs-lookup"><span data-stu-id="33fe0-113">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="33fe0-114">Bu kılavuzda ele alınmamıştır</span><span class="sxs-lookup"><span data-stu-id="33fe0-114">What this guide doesn't cover</span></span>

<span data-ttu-id="33fe0-115">Bu kılavuzda, .NET geliştiricileri için bir uçtan uca sürekli dağıtım deneyimi üzerinde odaklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="33fe0-115">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="33fe0-116">Her şey Azure için ayrıntılı bir kılavuz değildir ve, kapsamlı bir şekilde .NET API'leri Azure Hizmetleri için odak değil.</span><span class="sxs-lookup"><span data-stu-id="33fe0-116">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="33fe0-117">Vurgu tüm sürekli tümleştirme, dağıtım, izleme ve hata ayıklama ' dir.</span><span class="sxs-lookup"><span data-stu-id="33fe0-117">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="33fe0-118">Kılavuzu sonuna, sonraki adımlar için öneriler sunulur.</span><span class="sxs-lookup"><span data-stu-id="33fe0-118">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="33fe0-119">Öneri ASP.NET geliştiricileri için kullanışlı olan bir Azure platform hizmetlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="33fe0-119">Included in the suggestions are Azure platform services that are useful to ASP.NET developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="33fe0-120">Bu kılavuzda nedir</span><span class="sxs-lookup"><span data-stu-id="33fe0-120">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="33fe0-121">Araçlar ve indirmeler</span><span class="sxs-lookup"><span data-stu-id="33fe0-121">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="33fe0-122">Bu kılavuzda kullanılan araçları almayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="33fe0-122">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="33fe0-123">App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="33fe0-123">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="33fe0-124">Azure App Service'e bir ASP.NET Core uygulaması dağıtmak için çeşitli yöntemler hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="33fe0-124">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="33fe0-125">Sürekli tümleştirme ve dağıtım</span><span class="sxs-lookup"><span data-stu-id="33fe0-125">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="33fe0-126">Bir uçtan uca sürekli tümleştirme ve dağıtım çözümü için GitHub, VSTS ve Azure ile ASP.NET Core uygulamanızı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="33fe0-126">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, VSTS, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="33fe0-127">İzleme ve hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="33fe0-127">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="33fe0-128">İzleme, sorun giderme ve uygulamanızı ayarlamak için Azure'nın araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="33fe0-128">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="33fe0-129">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="33fe0-129">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="33fe0-130">Azure Öğrenme ASP.NET Core Geliştirici diğer öğrenme yolları.</span><span class="sxs-lookup"><span data-stu-id="33fe0-130">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="33fe0-131">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="33fe0-131">Acknowledgments</span></span>

<span data-ttu-id="33fe0-132">Bu kılavuzda yararlı önerileriniz katkıda .NET topluluk herkese teşekkür ederiz!</span><span class="sxs-lookup"><span data-stu-id="33fe0-132">Thank you to everyone in the .NET community who contributed to this guide with helpful suggestions!</span></span> <span data-ttu-id="33fe0-133">Özellikle bu yazıda son gözden geçirme için katkıda bulunan aşağıdaki topluluk üyeleri teşekkür istiyoruz:</span><span class="sxs-lookup"><span data-stu-id="33fe0-133">We'd like to specifically thank the following community members who contributed to the final review of this material:</span></span>

* [<span data-ttu-id="33fe0-134">SAM Wronski</span><span class="sxs-lookup"><span data-stu-id="33fe0-134">Sam Wronski</span></span>](https://www.youtube.com/c/worldofzerodevelopment)
* [<span data-ttu-id="33fe0-135">Jeffrey Palermo</span><span class="sxs-lookup"><span data-stu-id="33fe0-135">Jeffrey Palermo</span></span>](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a><span data-ttu-id="33fe0-136">Sonuç</span><span class="sxs-lookup"><span data-stu-id="33fe0-136">Conclusion</span></span>

<span data-ttu-id="33fe0-137">Bu kılavuz, ASP.NET Core ve Azure App Service geçici olarak oluşturulmuş bir sürekli tümleştirme geliştirme yaşam döngüsü oluşturmak için hazırlar.</span><span class="sxs-lookup"><span data-stu-id="33fe0-137">This guide prepares you to build a continuous integration development lifecycle built around ASP.NET Core and Azure App Service.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="33fe0-138">Ek okuma</span><span class="sxs-lookup"><span data-stu-id="33fe0-138">Additional reading</span></span>

* [<span data-ttu-id="33fe0-139">Bulut bilgi işlem nedir?</span><span class="sxs-lookup"><span data-stu-id="33fe0-139">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="33fe0-140">Örnekler bulut bilgi işlem</span><span class="sxs-lookup"><span data-stu-id="33fe0-140">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="33fe0-141">Iaas nedir?</span><span class="sxs-lookup"><span data-stu-id="33fe0-141">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="33fe0-142">PaaS nedir?</span><span class="sxs-lookup"><span data-stu-id="33fe0-142">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
