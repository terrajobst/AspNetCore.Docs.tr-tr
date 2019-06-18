---
title: ASP.NET Core dizin yapısı
author: guardrex
description: Dizin yapısı, yayımlanmış ASP.NET Core uygulamaları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 06/17/2019
uid: host-and-deploy/directory-structure
ms.openlocfilehash: f1df047decc7a0a6b7dcee57a690c55eea428b05
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67166975"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="fe8ab-103">ASP.NET Core dizin yapısı</span><span class="sxs-lookup"><span data-stu-id="fe8ab-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="fe8ab-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fe8ab-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fe8ab-105">*Yayımlama* dizinini içeren uygulamanın dağıtılabilir varlıklar tarafından üretilen [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-105">The *publish* directory contains the app's deployable assets produced by the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="fe8ab-106">Dizini içerir:</span><span class="sxs-lookup"><span data-stu-id="fe8ab-106">The directory contains:</span></span>

* <span data-ttu-id="fe8ab-107">Uygulama dosyaları</span><span class="sxs-lookup"><span data-stu-id="fe8ab-107">Application files</span></span>
* <span data-ttu-id="fe8ab-108">Yapılandırma dosyaları</span><span class="sxs-lookup"><span data-stu-id="fe8ab-108">Configuration files</span></span>
* <span data-ttu-id="fe8ab-109">Statik varlıklar</span><span class="sxs-lookup"><span data-stu-id="fe8ab-109">Static assets</span></span>
* <span data-ttu-id="fe8ab-110">Paketler</span><span class="sxs-lookup"><span data-stu-id="fe8ab-110">Packages</span></span>
* <span data-ttu-id="fe8ab-111">Bir çalışma zamanı ([müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) yalnızca)</span><span class="sxs-lookup"><span data-stu-id="fe8ab-111">A runtime ([self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) only)</span></span>

| <span data-ttu-id="fe8ab-112">Uygulama türü</span><span class="sxs-lookup"><span data-stu-id="fe8ab-112">App Type</span></span> | <span data-ttu-id="fe8ab-113">Dizin yapısı</span><span class="sxs-lookup"><span data-stu-id="fe8ab-113">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="fe8ab-114">Framework bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="fe8ab-114">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="fe8ab-115">Yayımlama&dagger;</span><span class="sxs-lookup"><span data-stu-id="fe8ab-115">publish&dagger;</span></span><ul><li><span data-ttu-id="fe8ab-116">Görünümleri&dagger; (MVC uygulamaları; görünümleri önceden derlenmiş değil)</span><span class="sxs-lookup"><span data-stu-id="fe8ab-116">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="fe8ab-117">Sayfaları&dagger; (sayfaları önceden derlenmiş değil, MVC veya Razor sayfaları uygulamaları;)</span><span class="sxs-lookup"><span data-stu-id="fe8ab-117">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="fe8ab-118">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="fe8ab-118">wwwroot&dagger;</span></span></li><li><span data-ttu-id="fe8ab-119">\*\.DLL dosyaları</span><span class="sxs-lookup"><span data-stu-id="fe8ab-119">\*\.dll files</span></span></li><li><span data-ttu-id="fe8ab-120">{DERLEME adı}.deps.JSON</span><span class="sxs-lookup"><span data-stu-id="fe8ab-120">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="fe8ab-121">{DERLEME adı} .dll</span><span class="sxs-lookup"><span data-stu-id="fe8ab-121">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="fe8ab-122">{DERLEME adı} .pdb</span><span class="sxs-lookup"><span data-stu-id="fe8ab-122">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="fe8ab-123">{} DERLEME ADI. Views.dll</span><span class="sxs-lookup"><span data-stu-id="fe8ab-123">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="fe8ab-124">{} DERLEME ADI. Views.pdb</span><span class="sxs-lookup"><span data-stu-id="fe8ab-124">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="fe8ab-125">{DERLEME adı}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="fe8ab-125">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="fe8ab-126">Web.config (IIS dağıtımlar)</span><span class="sxs-lookup"><span data-stu-id="fe8ab-126">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="fe8ab-127">Kendi içinde dağıtım</span><span class="sxs-lookup"><span data-stu-id="fe8ab-127">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="fe8ab-128">Yayımlama&dagger;</span><span class="sxs-lookup"><span data-stu-id="fe8ab-128">publish&dagger;</span></span><ul><li><span data-ttu-id="fe8ab-129">Görünümleri&dagger; (MVC uygulamaları; görünümleri önceden derlenmiş değil)</span><span class="sxs-lookup"><span data-stu-id="fe8ab-129">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="fe8ab-130">Sayfaları&dagger; (sayfaları önceden derlenmiş değil, MVC veya Razor sayfaları uygulamaları;)</span><span class="sxs-lookup"><span data-stu-id="fe8ab-130">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="fe8ab-131">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="fe8ab-131">wwwroot&dagger;</span></span></li><li><span data-ttu-id="fe8ab-132">\*.dll dosyaları</span><span class="sxs-lookup"><span data-stu-id="fe8ab-132">\*.dll files</span></span></li><li><span data-ttu-id="fe8ab-133">{DERLEME adı}.deps.JSON</span><span class="sxs-lookup"><span data-stu-id="fe8ab-133">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="fe8ab-134">{DERLEME adı} .dll</span><span class="sxs-lookup"><span data-stu-id="fe8ab-134">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="fe8ab-135">{DERLEME adı} .exe</span><span class="sxs-lookup"><span data-stu-id="fe8ab-135">{ASSEMBLY NAME}.exe</span></span></li><li><span data-ttu-id="fe8ab-136">{DERLEME adı} .pdb</span><span class="sxs-lookup"><span data-stu-id="fe8ab-136">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="fe8ab-137">{} DERLEME ADI. Views.dll</span><span class="sxs-lookup"><span data-stu-id="fe8ab-137">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="fe8ab-138">{} DERLEME ADI. Views.pdb</span><span class="sxs-lookup"><span data-stu-id="fe8ab-138">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="fe8ab-139">{DERLEME adı}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="fe8ab-139">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="fe8ab-140">Web.config (IIS dağıtımlar)</span><span class="sxs-lookup"><span data-stu-id="fe8ab-140">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="fe8ab-141">&dagger;Bir dizini gösterir</span><span class="sxs-lookup"><span data-stu-id="fe8ab-141">&dagger;Indicates a directory</span></span>

<span data-ttu-id="fe8ab-142">*Yayımlama* dizini temsil eder *içerik kök yolu*ayrıca adlı *uygulama temel yolu*, dağıtım.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-142">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="fe8ab-143">Dilediğiniz adı verilir *yayımlama* dizin sunucusuna dağıtılan uygulamanın konumuna barındırılan uygulamasının fiziksel yolu sunucunun görür.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-143">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="fe8ab-144">*Wwwroot* dizini varsa yalnızca içeren statik varlıklar.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-144">The *wwwroot* directory, if present, only contains static assets.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fe8ab-145">Oluşturma bir *günlükleri* klasördür yararlı [ASP.NET Core modülü Gelişmiş hata ayıklama günlüğü](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="fe8ab-145">Creating a *Logs* folder is useful for [ASP.NET Core Module enhanced debug logging](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="fe8ab-146">Sağlanan yolda `<handlerSetting>` değer modülü tarafından otomatik olarak oluşturulmaz ve dağıtım hata ayıklama günlüğünü yazılacak modülüne izin verecek şekilde önceden mevcut olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-146">Folders in the path provided to the `<handlerSetting>` value aren't created by the module automatically and should pre-exist in the deployment to allow the module to write the debug log.</span></span>

<span data-ttu-id="fe8ab-147">A *günlükleri* aşağıdaki iki yaklaşımdan birini kullanarak dağıtım için dizin oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="fe8ab-147">A *Logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="fe8ab-148">Aşağıdaki `<Target>` proje dosyasına öğe:</span><span class="sxs-lookup"><span data-stu-id="fe8ab-148">Add the following `<Target>` element to the project file:</span></span>

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   <span data-ttu-id="fe8ab-149">`<MakeDir>` Öğesi boş bir oluşturur *günlükleri* yayımlanan çıkış klasöründe.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-149">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="fe8ab-150">Öğesini kullanan `PublishDir` özelliği klasörü oluşturmak için hedef konumu belirlenemiyor.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-150">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="fe8ab-151">Web dağıtımı gibi çeşitli dağıtım yöntemleri, dağıtım sırasında boş klasörler atlayın.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-151">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="fe8ab-152">`<WriteLinesToFile>` Öğesi bir dosya oluşturur *günlükleri* klasörü, sunucu klasörünün dağıtım garanti eder.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-152">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="fe8ab-153">Çalışan işlemi, hedef klasöre yazma erişimi yoksa, bu yaklaşımı kullanarak klasör oluşturma başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-153">Folder creation using this approach fails if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="fe8ab-154">Fiziksel olarak oluşturma *günlükleri* dağıtım sunucusunda dizin.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-154">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="fe8ab-155">Dağıtım dizini okuma/Yürütme izinleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-155">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="fe8ab-156">*Günlükleri* dizin okuma/yazma izinleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-156">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="fe8ab-157">Dosyaları yazılacağı ek dizinleri okuma/yazma izinleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fe8ab-157">Additional directories where files are written require Read/Write permissions.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="fe8ab-158">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fe8ab-158">Additional resources</span></span>

* [<span data-ttu-id="fe8ab-159">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="fe8ab-159">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* [<span data-ttu-id="fe8ab-160">.NET core uygulama dağıtımı</span><span class="sxs-lookup"><span data-stu-id="fe8ab-160">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* [<span data-ttu-id="fe8ab-161">Hedef çerçeveler</span><span class="sxs-lookup"><span data-stu-id="fe8ab-161">Target frameworks</span></span>](/dotnet/standard/frameworks)
* [<span data-ttu-id="fe8ab-162">.NET core RID Kataloğu</span><span class="sxs-lookup"><span data-stu-id="fe8ab-162">.NET Core RID Catalog</span></span>](/dotnet/core/rid-catalog)
