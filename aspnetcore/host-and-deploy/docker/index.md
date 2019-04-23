---
title: ASP.NET Core, Docker kapsayıcılarında barındırın
author: rick-anderson
description: ASP.NET Core uygulamalarınızı Docker kapsayıcılarında barındırmak öğrenme için kaynakların bağlantılarını keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
uid: host-and-deploy/docker/index
ms.openlocfilehash: 9189e1fbb21abcc8c8bdea947e672ee53b59bc4f
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982633"
---
# <a name="host-aspnet-core-in-docker-containers"></a><span data-ttu-id="0f3b2-103">ASP.NET Core, Docker kapsayıcılarında barındırın</span><span class="sxs-lookup"><span data-stu-id="0f3b2-103">Host ASP.NET Core in Docker containers</span></span>

<span data-ttu-id="0f3b2-104">Aşağıdaki makaleler, ASP.NET Core uygulamaları docker'da barındırma hakkında öğrenmeye yönelik kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="0f3b2-104">The following articles are available for learning about hosting ASP.NET Core apps in Docker:</span></span>

[<span data-ttu-id="0f3b2-105">Kapsayıcılar ve Docker’a Giriş</span><span class="sxs-lookup"><span data-stu-id="0f3b2-105">Introduction to Containers and Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
<span data-ttu-id="0f3b2-106">Kapsayıcı bir yaklaşım, bir uygulama veya hizmeti ve bağımlılıklarını yapılandırmasıyla birlikte bir kapsayıcı görüntüsü paketlenmiş yazılım geliştirme için nasıl olduğunu görün.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-106">See how containerization is an approach to software development in which an application or service, its dependencies, and its configuration are packaged together as a container image.</span></span> <span data-ttu-id="0f3b2-107">Görüntü, test ve ardından bir konağa dağıtılmış.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-107">The image can be tested and then deployed to a host.</span></span>

[<span data-ttu-id="0f3b2-108">Docker nedir</span><span class="sxs-lookup"><span data-stu-id="0f3b2-108">What is Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
<span data-ttu-id="0f3b2-109">Docker uygulamaları bulutta veya şirket içinde çalışan taşınabilir, kendi kendine yeterli kapsayıcılar olarak dağıtımını otomatik hale getirme için açık kaynak bir projedir nasıl olduğunu öğrenin.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-109">Discover how Docker is an open-source project for automating the deployment of apps as portable, self-sufficient containers that can run on the cloud or on-premises.</span></span>

[<span data-ttu-id="0f3b2-110">Docker terimleri</span><span class="sxs-lookup"><span data-stu-id="0f3b2-110">Docker Terminology</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
<span data-ttu-id="0f3b2-111">Terim ve tanımları için Docker teknolojisini öğrenin.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-111">Learn terms and definitions for Docker technology.</span></span>

[<span data-ttu-id="0f3b2-112">Docker kapsayıcıları, görüntüleri ve kayıt defterleri</span><span class="sxs-lookup"><span data-stu-id="0f3b2-112">Docker containers, images, and registries</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
<span data-ttu-id="0f3b2-113">Nasıl Docker kapsayıcı görüntüleri tutarlı dağıtım için bir görüntü kayıt defterinde ortamlar genelinde depolandığını bulun.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-113">Find out how Docker container images are stored in an image registry for consistent deployment across environments.</span></span>

<span data-ttu-id="0f3b2-114"><xref:host-and-deploy/docker/building-net-docker-images> Derleme ve ASP.NET Core uygulaması docker kapsayıcılarında çalıştırın hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-114"><xref:host-and-deploy/docker/building-net-docker-images> Learn how to build and dockerize an ASP.NET Core app.</span></span> <span data-ttu-id="0f3b2-115">Microsoft tarafından yönetilen Docker görüntüleri keşfedin ve kullanım örneklerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-115">Explore Docker images maintained by Microsoft and examine use cases.</span></span>

[<span data-ttu-id="0f3b2-116">Docker için Visual Studio Araçları</span><span class="sxs-lookup"><span data-stu-id="0f3b2-116">Visual Studio Tools for Docker</span></span>](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
<span data-ttu-id="0f3b2-117">Visual Studio 2017 derleme, hata ayıklama ve ASP.NET Core, .NET Framework veya .NET Core için Docker Windows üzerinde hedefleyen uygulamaları çalıştıran nasıl desteklediğini öğrenin.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-117">Discover how Visual Studio 2017 supports building, debugging, and running ASP.NET Core apps targeting either .NET Framework or .NET Core on Docker for Windows.</span></span> <span data-ttu-id="0f3b2-118">Hem Windows hem de Linux kapsayıcıları desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-118">Both Windows and Linux containers are supported.</span></span>

[<span data-ttu-id="0f3b2-119">Docker Görüntüsüne Yayımlama</span><span class="sxs-lookup"><span data-stu-id="0f3b2-119">Publish to a Docker Image</span></span>](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
<span data-ttu-id="0f3b2-120">PowerShell kullanarak azure'da bir Docker konağı için bir ASP.NET Core uygulaması dağıtmak için Visual Studio Araçları için Docker uzantısını kullanmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-120">Find out how to use the Visual Studio Tools for Docker extension to deploy an ASP.NET Core app to a Docker host on Azure using PowerShell.</span></span>

[<span data-ttu-id="0f3b2-121">ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0f3b2-121">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)  
<span data-ttu-id="0f3b2-122">Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-122">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="0f3b2-123">Bir ara sunucu aracılığıyla istekler genellikle geçirerek şema ve istemci IP gibi özgün isteğiyle ilgili bilgileri gizler.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-123">Passing requests through a proxy often obscures information about the original request, such as the scheme and client IP.</span></span> <span data-ttu-id="0f3b2-124">İletilen el ile uygulama isteği hakkında bazı bilgiler gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-124">It might be necessary to forwarded some information about the request manually to the app.</span></span>
