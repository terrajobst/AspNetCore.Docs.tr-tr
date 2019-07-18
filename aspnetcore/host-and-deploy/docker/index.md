---
title: Docker kapsayıcılarında konak ASP.NET Core
author: rick-anderson
description: Docker kapsayıcılarında ASP.NET Core uygulamaları nasıl barındırdığınızı öğrenmek için kaynakların bağlantılarını bulun.
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
uid: host-and-deploy/docker/index
ms.openlocfilehash: cb5f774db5fab46a57f8ca4bbbca148f20f371ba
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308041"
---
# <a name="host-aspnet-core-in-docker-containers"></a><span data-ttu-id="cde58-103">Docker kapsayıcılarında konak ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cde58-103">Host ASP.NET Core in Docker containers</span></span>

<span data-ttu-id="cde58-104">Docker 'da ASP.NET Core uygulamalar barındırma hakkında bilgi edinmek için aşağıdaki makaleler sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="cde58-104">The following articles are available for learning about hosting ASP.NET Core apps in Docker:</span></span>

[<span data-ttu-id="cde58-105">Kapsayıcılar ve Docker’a Giriş</span><span class="sxs-lookup"><span data-stu-id="cde58-105">Introduction to Containers and Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
<span data-ttu-id="cde58-106">Kapsayıcının bir uygulama veya hizmetin, bağımlılıklarının ve yapılandırmasının bir kapsayıcı görüntüsü olarak birlikte paketlendiği yazılım geliştirme konusunda nasıl yaklaşım olduğunu öğrenin.</span><span class="sxs-lookup"><span data-stu-id="cde58-106">See how containerization is an approach to software development in which an application or service, its dependencies, and its configuration are packaged together as a container image.</span></span> <span data-ttu-id="cde58-107">Görüntü test edilebilir ve bir konağa dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="cde58-107">The image can be tested and then deployed to a host.</span></span>

[<span data-ttu-id="cde58-108">Docker nedir?</span><span class="sxs-lookup"><span data-stu-id="cde58-108">What is Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
<span data-ttu-id="cde58-109">Docker 'ın, bulutta veya şirket içinde çalışabilen, taşınabilir, kendi kendine yeterli kapsayıcı olarak uygulama dağıtımını otomatik hale getirmeye yönelik açık kaynaklı bir proje olduğunu öğrenin.</span><span class="sxs-lookup"><span data-stu-id="cde58-109">Discover how Docker is an open-source project for automating the deployment of apps as portable, self-sufficient containers that can run on the cloud or on-premises.</span></span>

[<span data-ttu-id="cde58-110">Docker terminolojisi</span><span class="sxs-lookup"><span data-stu-id="cde58-110">Docker Terminology</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
<span data-ttu-id="cde58-111">Docker teknolojisine yönelik hüküm ve tanımlar hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="cde58-111">Learn terms and definitions for Docker technology.</span></span>

[<span data-ttu-id="cde58-112">Docker kapsayıcıları, görüntüleri ve kayıt defterleri</span><span class="sxs-lookup"><span data-stu-id="cde58-112">Docker containers, images, and registries</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
<span data-ttu-id="cde58-113">Docker kapsayıcı görüntülerinin, ortamlar genelinde tutarlı dağıtım için bir görüntü kayıt defterinde nasıl depolandığını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="cde58-113">Find out how Docker container images are stored in an image registry for consistent deployment across environments.</span></span>

<span data-ttu-id="cde58-114"><xref:host-and-deploy/docker/building-net-docker-images>ASP.NET Core bir uygulamayı nasıl oluşturacağınızı ve kullanabileceğinizi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="cde58-114"><xref:host-and-deploy/docker/building-net-docker-images> Learn how to build and dockerize an ASP.NET Core app.</span></span> <span data-ttu-id="cde58-115">Microsoft tarafından tutulan Docker görüntülerini araştırın ve kullanım örneklerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="cde58-115">Explore Docker images maintained by Microsoft and examine use cases.</span></span>

[<span data-ttu-id="cde58-116">Visual Studio kapsayıcı araçları</span><span class="sxs-lookup"><span data-stu-id="cde58-116">Visual Studio Container Tools</span></span>](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
<span data-ttu-id="cde58-117">Visual Studio 'Nun Docker for Windows .NET Framework veya .NET Core 'u hedefleyen ASP.NET Core uygulamaları oluşturmayı, hata ayıklamayı ve çalıştırmayı nasıl desteklediğini öğrenin.</span><span class="sxs-lookup"><span data-stu-id="cde58-117">Discover how Visual Studio supports building, debugging, and running ASP.NET Core apps targeting either .NET Framework or .NET Core on Docker for Windows.</span></span> <span data-ttu-id="cde58-118">Hem Windows hem de Linux kapsayıcıları desteklenir.</span><span class="sxs-lookup"><span data-stu-id="cde58-118">Both Windows and Linux containers are supported.</span></span>

[<span data-ttu-id="cde58-119">Azure Container Registry yayımlama</span><span class="sxs-lookup"><span data-stu-id="cde58-119">Publish to Azure Container Registry</span></span>](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
<span data-ttu-id="cde58-120">Azure 'da PowerShell kullanarak bir ASP.NET Core uygulamasını Docker konağına dağıtmak için Visual Studio kapsayıcı araçları uzantısı 'nın nasıl kullanılacağını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="cde58-120">Find out how to use the Visual Studio Container Tools extension to deploy an ASP.NET Core app to a Docker host on Azure using PowerShell.</span></span>

[<span data-ttu-id="cde58-121">Proxy sunucularıyla ve yük dengeleyicilerle çalışacak ASP.NET Core yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cde58-121">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)  
<span data-ttu-id="cde58-122">Proxy sunucularının ve yük dengeleyiciler arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="cde58-122">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="cde58-123">İsteklerin bir ara sunucu üzerinden geçirilmesi, genellikle orijinal istekle ilgili olarak düzen ve istemci IP 'si gibi bilgileri gizler.</span><span class="sxs-lookup"><span data-stu-id="cde58-123">Passing requests through a proxy often obscures information about the original request, such as the scheme and client IP.</span></span> <span data-ttu-id="cde58-124">İstekle ilgili bazı bilgilerin uygulamaya el ile iletilmesi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="cde58-124">It might be necessary to forwarded some information about the request manually to the app.</span></span>
