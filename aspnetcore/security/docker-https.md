---
title: HTTPS üzerinden Docker ile görüntüleri barındırma ASP.NET Core
author: rick-anderson
description: HTTPS üzerinden Docker ile ASP.NET Core görüntülerini nasıl barındıralabileceğinizi öğrenin
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
uid: security/docker-https
ms.openlocfilehash: f17a3abe1b00b39b7b6787be5b20ce65771190b8
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69619713"
---
# <a name="hosting-aspnet-core-images-with-docker-over-https"></a><span data-ttu-id="97129-103">HTTPS üzerinden Docker ile görüntüleri barındırma ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97129-103">Hosting ASP.NET Core images with Docker over HTTPS</span></span>

<span data-ttu-id="97129-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="97129-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="97129-105">ASP.NET Core [Varsayılan olarak https](/aspnet/core/security/enforcing-ssl)kullanır.</span><span class="sxs-lookup"><span data-stu-id="97129-105">ASP.NET Core uses [HTTPS by default](/aspnet/core/security/enforcing-ssl).</span></span> <span data-ttu-id="97129-106">[Https](https://en.wikipedia.org/wiki/HTTPS) , güven, kimlik ve şifreleme için [sertifikalara](https://en.wikipedia.org/wiki/Public_key_certificate) bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="97129-106">[HTTPS](https://en.wikipedia.org/wiki/HTTPS) relies on [certificates](https://en.wikipedia.org/wiki/Public_key_certificate) for trust, identity, and encryption.</span></span>

<span data-ttu-id="97129-107">Bu belgede önceden oluşturulmuş kapsayıcı görüntülerinin HTTPS ile nasıl çalıştırılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="97129-107">This document explains how to run pre-built container images with HTTPS.</span></span>

<span data-ttu-id="97129-108">Geliştirme senaryoları için bkz. [https üzerinden Docker ile ASP.NET Core uygulamaları geliştirme](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md) .</span><span class="sxs-lookup"><span data-stu-id="97129-108">See [Developing ASP.NET Core Applications with Docker over HTTPS](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md) for development scenarios.</span></span>

<span data-ttu-id="97129-109">Bu örnek, [Docker Istemcisinin](https://www.docker.com/products/docker) [Docker 17,06](https://docs.docker.com/release-notes/docker-ce) veya sonraki bir sürümünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="97129-109">This sample requires [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) or later of the [Docker client](https://www.docker.com/products/docker).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97129-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="97129-110">Prerequisites</span></span>

<span data-ttu-id="97129-111">Bu belgedeki bazı yönergeler için [.NET Core 2,2 SDK](https://www.microsoft.com/net/download) veya üzeri gereklidir.</span><span class="sxs-lookup"><span data-stu-id="97129-111">The [.NET Core 2.2 SDK](https://www.microsoft.com/net/download) or later is required for some of the instructions in this document.</span></span>

## <a name="certificates"></a><span data-ttu-id="97129-112">Sertifikalar</span><span class="sxs-lookup"><span data-stu-id="97129-112">Certificates</span></span>

<span data-ttu-id="97129-113">Bir etki alanı için [Üretim barındırma](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) için bir [sertifika yetkilisinden](https://en.wikipedia.org/wiki/Certificate_authority) bir sertifika gereklidir.</span><span class="sxs-lookup"><span data-stu-id="97129-113">A certificate from a [certificate authority](https://en.wikipedia.org/wiki/Certificate_authority) is required for [production hosting](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) for a domain.</span></span>  <span data-ttu-id="97129-114">[Şifrelemem](https://letsencrypt.org/) , ücretsiz sertifikalar sunan bir sertifika yetkilisindir.</span><span class="sxs-lookup"><span data-stu-id="97129-114">[Let's Encrypt](https://letsencrypt.org/) is a certificate authority that offers free certificates.</span></span>

<span data-ttu-id="97129-115">Bu belge, üzerine `localhost`önceden oluşturulmuş görüntüleri barındırmak için otomatik olarak [imzalanan geliştirme sertifikaları](https://en.wikipedia.org/wiki/Self-signed_certificate) kullanır.</span><span class="sxs-lookup"><span data-stu-id="97129-115">This document uses [self-signed development certificates](https://en.wikipedia.org/wiki/Self-signed_certificate) for hosting pre-built images over `localhost`.</span></span> <span data-ttu-id="97129-116">Yönergeler, üretim sertifikalarını kullanmaya benzerdir.</span><span class="sxs-lookup"><span data-stu-id="97129-116">The instructions are similar to using production certificates.</span></span>

<span data-ttu-id="97129-117">Üretim sertifikaları için:</span><span class="sxs-lookup"><span data-stu-id="97129-117">For production certs:</span></span>

* <span data-ttu-id="97129-118">`dotnet dev-certs` Araç gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="97129-118">The `dotnet dev-certs` tool is not required.</span></span>
* <span data-ttu-id="97129-119">Sertifikaların, yönergelerde kullanılan konumda depolanması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="97129-119">Certificates do not need to be stored in the location used in the instructions.</span></span> <span data-ttu-id="97129-120">Herhangi bir konumun çalışması gerekir, ancak bu sertifikalar site dizininizde depolanarak önerilmez.</span><span class="sxs-lookup"><span data-stu-id="97129-120">Any location should work, although storing certs within your site directory is not recommended.</span></span>

<span data-ttu-id="97129-121">Yönergeler birimi, sertifikaları kapsayıcılara bağlama.</span><span class="sxs-lookup"><span data-stu-id="97129-121">The instructions volume mount certificates into containers.</span></span> <span data-ttu-id="97129-122">Dockerfile içindeki bir `COPY` komutla kapsayıcı görüntülerine sertifika ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97129-122">You can add certificates into container images with a `COPY` command in a Dockerfile.</span></span> <span data-ttu-id="97129-123">Sertifikaların bir görüntüye kopyalanması önerilmez:</span><span class="sxs-lookup"><span data-stu-id="97129-123">Copying certificates into an image is not recommended:</span></span>

* <span data-ttu-id="97129-124">Geliştirici sertifikaları ile test etmek için aynı görüntünün kullanımını zorlaştırır.</span><span class="sxs-lookup"><span data-stu-id="97129-124">It makes difficult to use the same image for testing with developer certificates.</span></span>
* <span data-ttu-id="97129-125">Üretim sertifikaları ile barındırmak için aynı görüntünün kullanımını zorlaştırır.</span><span class="sxs-lookup"><span data-stu-id="97129-125">It makes difficult to use the same image for Hosting with production certificates.</span></span>
* <span data-ttu-id="97129-126">Sertifika açıklanmasının önemli bir riski vardır.</span><span class="sxs-lookup"><span data-stu-id="97129-126">There is significant risk of certificate disclosure.</span></span>

## <a name="running-pre-built-container-images-with-https"></a><span data-ttu-id="97129-127">HTTPS ile önceden oluşturulmuş kapsayıcı görüntülerini çalıştırma</span><span class="sxs-lookup"><span data-stu-id="97129-127">Running pre-built container images with HTTPS</span></span>

<span data-ttu-id="97129-128">İşletim sistemi yapılandırmanız için aşağıdaki yönergeleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="97129-128">Use the following instructions for your operating system configuration.</span></span>

### <a name="windows-using-linux-containers"></a><span data-ttu-id="97129-129">Linux kapsayıcıları kullanan Windows</span><span class="sxs-lookup"><span data-stu-id="97129-129">Windows using Linux containers</span></span>

<span data-ttu-id="97129-130">Sertifika Oluştur ve yerel makineyi Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="97129-130">Generate certificate and configure local machine:</span></span>

```console
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="97129-131">Yukarıdaki komutlarda, öğesini parolayla değiştirin `{ password here }` .</span><span class="sxs-lookup"><span data-stu-id="97129-131">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="97129-132">Kapsayıcı görüntüsünü HTTPS için yapılandırılan ASP.NET Core çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="97129-132">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="97129-133">Parolanın, sertifika için kullanılan parolayla eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="97129-133">The password must match the password used for the certificate.</span></span>

### <a name="macos-or-linux"></a><span data-ttu-id="97129-134">macOS veya Linux</span><span class="sxs-lookup"><span data-stu-id="97129-134">macOS or Linux</span></span>

<span data-ttu-id="97129-135">Sertifika Oluştur ve yerel makineyi Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="97129-135">Generate certificate and configure local machine:</span></span>

```console
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="97129-136">`dotnet dev-certs https --trust`yalnızca macOS ve Windows 'da desteklenir.</span><span class="sxs-lookup"><span data-stu-id="97129-136">`dotnet dev-certs https --trust` is only supported on macOS and Windows.</span></span> <span data-ttu-id="97129-137">Linux 'ta sertifikalarınızın desteklediği şekilde sertifika almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="97129-137">You need to trust certs on Linux in the way that is supported by your distro.</span></span> <span data-ttu-id="97129-138">Büyük olasılıkla, tarayıcınızda sertifikaya güvenmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="97129-138">It is likely that you need to trust the certificate in your browser.</span></span>

<span data-ttu-id="97129-139">Yukarıdaki komutlarda, öğesini parolayla değiştirin `{ password here }` .</span><span class="sxs-lookup"><span data-stu-id="97129-139">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="97129-140">Kapsayıcı görüntüsünü HTTPS için yapılandırılan ASP.NET Core çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="97129-140">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v ${HOME}/.aspnet/https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="97129-141">Parolanın, sertifika için kullanılan parolayla eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="97129-141">The password must match the password used for the certificate.</span></span>

### <a name="windows-using-windows-containers"></a><span data-ttu-id="97129-142">Windows kapsayıcıları kullanan pencereler</span><span class="sxs-lookup"><span data-stu-id="97129-142">Windows using Windows containers</span></span>

<span data-ttu-id="97129-143">Sertifika Oluştur ve yerel makineyi Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="97129-143">Generate certificate and configure local machine:</span></span>

```console
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="97129-144">Yukarıdaki komutlarda, öğesini parolayla değiştirin `{ password here }` .</span><span class="sxs-lookup"><span data-stu-id="97129-144">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="97129-145">Kapsayıcı görüntüsünü HTTPS için yapılandırılan ASP.NET Core çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="97129-145">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=\https\aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:C:\https\ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="97129-146">Parolanın, sertifika için kullanılan parolayla eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="97129-146">The password must match the password used for the certificate.</span></span>