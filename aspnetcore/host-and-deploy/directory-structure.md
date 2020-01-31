---
title: ASP.NET Core dizin yapısı
author: guardrex
description: Dizin yapısı, yayımlanmış ASP.NET Core uygulamaları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2020
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ba5cb96dfdcdca10034299e3bbe662ce056af791
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76870272"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="346c9-103">ASP.NET Core dizin yapısı</span><span class="sxs-lookup"><span data-stu-id="346c9-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="346c9-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="346c9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="346c9-105">*Yayımla* dizini, uygulamanın [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutu tarafından üretilen dağıtılabilir varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="346c9-105">The *publish* directory contains the app's deployable assets produced by the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="346c9-106">Dizin şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="346c9-106">The directory contains:</span></span>

* <span data-ttu-id="346c9-107">Uygulama dosyaları</span><span class="sxs-lookup"><span data-stu-id="346c9-107">Application files</span></span>
* <span data-ttu-id="346c9-108">Yapılandırma dosyaları</span><span class="sxs-lookup"><span data-stu-id="346c9-108">Configuration files</span></span>
* <span data-ttu-id="346c9-109">Statik varlıklar</span><span class="sxs-lookup"><span data-stu-id="346c9-109">Static assets</span></span>
* <span data-ttu-id="346c9-110">Paketler</span><span class="sxs-lookup"><span data-stu-id="346c9-110">Packages</span></span>
* <span data-ttu-id="346c9-111">Çalışma zamanı (yalnızca[kendi içindeki dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) )</span><span class="sxs-lookup"><span data-stu-id="346c9-111">A runtime ([self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) only)</span></span>

| <span data-ttu-id="346c9-112">Uygulama türü</span><span class="sxs-lookup"><span data-stu-id="346c9-112">App Type</span></span> | <span data-ttu-id="346c9-113">Dizin yapısı</span><span class="sxs-lookup"><span data-stu-id="346c9-113">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="346c9-114">Çerçeveye bağımlı yürütülebilir (FDE)</span><span class="sxs-lookup"><span data-stu-id="346c9-114">Framework-dependent Executable (FDE)</span></span>](/dotnet/core/deploying/#framework-dependent-executables-fde) | <ul><li><span data-ttu-id="346c9-115">&dagger; Yayımla</span><span class="sxs-lookup"><span data-stu-id="346c9-115">publish&dagger;</span></span><ul><li><span data-ttu-id="346c9-116">&dagger; MVC uygulamalarını görüntüler; Görünümler önceden derlenmiş değilse</span><span class="sxs-lookup"><span data-stu-id="346c9-116">Views&dagger; MVC apps; if views aren't precompiled</span></span></li><li><span data-ttu-id="346c9-117">Sayfalar önceden derlenmiş değilse MVC veya Razor Pages uygulamalar&dagger;.</span><span class="sxs-lookup"><span data-stu-id="346c9-117">Pages&dagger; MVC or Razor Pages apps, if pages aren't precompiled</span></span></li><li><span data-ttu-id="346c9-118">Wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="346c9-118">wwwroot&dagger;</span></span></li><li><span data-ttu-id="346c9-119">*. dll dosyaları</li><li>{Assembly Name}. Deps. json</li><li>{Assembly Name}. dll</li><li>{Assembly Name} {. UZANTı} *. exe* uzantısı Windows üzerinde, MacOS veya Linux üzerinde hiçbir uzantı</li><li>{Assembly Name}. pdb</li><li>{BÜTÜNLEŞTIRILMIŞ kod adı}. Views. dll <li>{bütünleştirilmiş kod adı}</li>. Görünümler. pdb</li><li>{ASSEMBLY NAME}. runtimeconfig. JSON</li><li>Web. config (IIS dağıtımları)</li><li>createdump ([Linux createdump Utility](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy))* </li><li>. bu (Linux paylaşılan nesne kitaplığı)</span><span class="sxs-lookup"><span data-stu-id="346c9-119">*.dll files</li><li>{ASSEMBLY NAME}.deps.json</li><li>{ASSEMBLY NAME}.dll</li><li>{ASSEMBLY NAME}{.EXTENSION} *.exe* extension on Windows, no extension on macOS or Linux</li><li>{ASSEMBLY NAME}.pdb</li><li>{ASSEMBLY NAME}.Views.dll</li><li>{ASSEMBLY NAME}.Views.pdb</li><li>{ASSEMBLY NAME}.runtimeconfig.json</li><li>web.config (IIS deployments)</li><li>createdump ([Linux createdump utility](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy))</li><li>*.so (Linux shared object library)</span></span></li><li><span data-ttu-id="346c9-120">*. a (MacOS Arşivi)</li><li>* . dylib (MacOS dinamik kitaplığı)</span><span class="sxs-lookup"><span data-stu-id="346c9-120">*.a (macOS archive)</li><li>*.dylib (macOS dynamic library)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="346c9-121">Kendi içinde dağıtım (SCD)</span><span class="sxs-lookup"><span data-stu-id="346c9-121">Self-contained Deployment (SCD)</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="346c9-122">&dagger; Yayımla</span><span class="sxs-lookup"><span data-stu-id="346c9-122">publish&dagger;</span></span><ul><li><span data-ttu-id="346c9-123">Görünümler&dagger; MVC uygulamalarını önceden derlenmiş değilse görüntüler</span><span class="sxs-lookup"><span data-stu-id="346c9-123">Views&dagger; MVC apps, if views aren't precompiled</span></span></li><li><span data-ttu-id="346c9-124">Sayfalar önceden derlenmiş değilse MVC veya Razor Pages uygulamalar&dagger;.</span><span class="sxs-lookup"><span data-stu-id="346c9-124">Pages&dagger; MVC or Razor Pages apps, if pages aren't precompiled</span></span></li><li><span data-ttu-id="346c9-125">Wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="346c9-125">wwwroot&dagger;</span></span></li><li><span data-ttu-id="346c9-126">\*. dll dosyaları</span><span class="sxs-lookup"><span data-stu-id="346c9-126">\*.dll files</span></span></li><li><span data-ttu-id="346c9-127">{ASSEMBLY NAME}. Deps. JSON</span><span class="sxs-lookup"><span data-stu-id="346c9-127">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="346c9-128">{Bütünleştirilmiş kod adı}. dll</span><span class="sxs-lookup"><span data-stu-id="346c9-128">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="346c9-129">{Bütünleştirilmiş kod adı}. exe</span><span class="sxs-lookup"><span data-stu-id="346c9-129">{ASSEMBLY NAME}.exe</span></span></li><li><span data-ttu-id="346c9-130">{Bütünleştirilmiş kod adı}. pdb</span><span class="sxs-lookup"><span data-stu-id="346c9-130">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="346c9-131">{BÜTÜNLEŞTIRILMIŞ KOD ADı}. Views. dll</span><span class="sxs-lookup"><span data-stu-id="346c9-131">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="346c9-132">{BÜTÜNLEŞTIRILMIŞ KOD ADı}. Views. pdb</span><span class="sxs-lookup"><span data-stu-id="346c9-132">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="346c9-133">{ASSEMBLY NAME}. runtimeconfig. JSON</span><span class="sxs-lookup"><span data-stu-id="346c9-133">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="346c9-134">Web. config (IIS dağıtımları)</span><span class="sxs-lookup"><span data-stu-id="346c9-134">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="346c9-135">&dagger;bir dizini belirtir</span><span class="sxs-lookup"><span data-stu-id="346c9-135">&dagger;Indicates a directory</span></span>

<span data-ttu-id="346c9-136">*Yayımla* dizini, dağıtımın *uygulama temel yolu*olarak da adlandırılan *içerik kök yolunu*temsil eder.</span><span class="sxs-lookup"><span data-stu-id="346c9-136">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="346c9-137">Sunucuda dağıtılan uygulamanın *Yayımlama* dizinine hangi ad verildiğinde, konumu sunucunun barındırılan uygulamanın fiziksel yolu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="346c9-137">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="346c9-138">Varsa, *Wwwroot* dizini yalnızca statik varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="346c9-138">The *wwwroot* directory, if present, only contains static assets.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="346c9-139">*Günlük* klasörü oluşturma, [ASP.NET Core modülü gelişmiş hata ayıklama günlüğü](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="346c9-139">Creating a *Logs* folder is useful for [ASP.NET Core Module enhanced debug logging](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="346c9-140">`<handlerSetting>` değere verilen yoldaki klasörler, modül tarafından otomatik olarak oluşturulmaz ve modülün hata ayıklama günlüğünü yazmasına izin vermek için dağıtımda önceden var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="346c9-140">Folders in the path provided to the `<handlerSetting>` value aren't created by the module automatically and should pre-exist in the deployment to allow the module to write the debug log.</span></span>

<span data-ttu-id="346c9-141">Aşağıdaki iki yaklaşımdan birini kullanarak dağıtım için bir *günlük* dizini oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="346c9-141">A *Logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="346c9-142">Aşağıdaki `<Target>` öğesini proje dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="346c9-142">Add the following `<Target>` element to the project file:</span></span>

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

   <span data-ttu-id="346c9-143">`<MakeDir>` öğesi yayımlanan çıktıda boş bir *Günlükler* klasörü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="346c9-143">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="346c9-144">Öğesi, klasörü oluşturmak için hedef konumu belirlemede `PublishDir` özelliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="346c9-144">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="346c9-145">Web Dağıtımı gibi birkaç dağıtım yöntemi, dağıtım sırasında boş klasörleri atlar.</span><span class="sxs-lookup"><span data-stu-id="346c9-145">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="346c9-146">`<WriteLinesToFile>` öğesi *Günlükler* klasöründe, klasörün sunucuya dağıtımını garanti eden bir dosya oluşturur.</span><span class="sxs-lookup"><span data-stu-id="346c9-146">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="346c9-147">Çalışan işleminin hedef klasöre yazma erişimi yoksa, bu yaklaşımı kullanarak klasör oluşturma işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="346c9-147">Folder creation using this approach fails if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="346c9-148">Dağıtımdaki sunucuda *günlük* dizinini fiziksel olarak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="346c9-148">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="346c9-149">Dağıtım dizini için okuma/yürütme izinleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="346c9-149">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="346c9-150">*Günlükler* dizini için okuma/yazma izinleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="346c9-150">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="346c9-151">Dosyaların yazıldığı ek dizinler okuma/yazma izinleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="346c9-151">Additional directories where files are written require Read/Write permissions.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="346c9-152">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="346c9-152">Additional resources</span></span>

* [<span data-ttu-id="346c9-153">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="346c9-153">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* [<span data-ttu-id="346c9-154">.NET core uygulama dağıtımı</span><span class="sxs-lookup"><span data-stu-id="346c9-154">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* [<span data-ttu-id="346c9-155">Hedef çerçeveler</span><span class="sxs-lookup"><span data-stu-id="346c9-155">Target frameworks</span></span>](/dotnet/standard/frameworks)
* [<span data-ttu-id="346c9-156">.NET Core RID kataloğu</span><span class="sxs-lookup"><span data-stu-id="346c9-156">.NET Core RID Catalog</span></span>](/dotnet/core/rid-catalog)
