---
title: Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı
author: rick-anderson
description: Oluşturmayı Visual Studio'da yayımlama profilleri ve ASP.NET Core uygulama dağıtımlarını çeşitli hedeflere yönetmek için kullanın.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/18/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: ac243a3898553b2e14a6c15d311afaf62f112a24
ms.sourcegitcommit: a1283d486ac1dcedfc7ea302e1cc882833e2c515
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207823"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="85129-103">Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı</span><span class="sxs-lookup"><span data-stu-id="85129-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="85129-104">Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="85129-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="85129-105">Bu belge, Visual Studio 2017 kullanarak veya daha sonra oluşturma ve kullanma odaklanır yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="85129-105">This document focuses on using Visual Studio 2017 or later to create and use publish profiles.</span></span> <span data-ttu-id="85129-106">Visual Studio ile oluşturulan yayımlama profillerine MSBuild ve Visual Studio çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85129-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio.</span></span> <span data-ttu-id="85129-107">Bkz: [Visual Studio kullanarak Azure App Service'e bir ASP.NET Core web uygulaması yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) Azure'da yayımlamak için yönergeler.</span><span class="sxs-lookup"><span data-stu-id="85129-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="85129-108">`dotnet new mvc` Komutu, aşağıdaki üst düzey içeren bir proje dosyası üretir `<Project>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="85129-108">The `dotnet new mvc` command produces a project file containing the following top-level `<Project>` element:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

<span data-ttu-id="85129-109">Önceki `<Project>` öğenin `Sdk` özniteliği alır MSBuild [özellikleri](/visualstudio/msbuild/msbuild-properties) ve [hedefleri](/visualstudio/msbuild/msbuild-targets) gelen *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\ SDK.props* ve *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="85129-109">The preceding `<Project>` element's `Sdk` attribute imports the MSBuild [properties](/visualstudio/msbuild/msbuild-properties) and [targets](/visualstudio/msbuild/msbuild-targets) from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.props* and *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, respectively.</span></span> <span data-ttu-id="85129-110">Varsayılan konumu `$(MSBuildSDKsPath)` (Visual Studio 2019 Enterprise ile) *% programfiles (x86) %\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* klasör.</span><span class="sxs-lookup"><span data-stu-id="85129-110">The default location for `$(MSBuildSDKsPath)` (with Visual Studio 2019 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="85129-111">`Microsoft.NET.Sdk.Web` (Web SDK'sı) dahil olmak üzere diğer SDK'lar üzerinde bağlıdır `Microsoft.NET.Sdk` (.NET Core SDK) ve `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)).</span><span class="sxs-lookup"><span data-stu-id="85129-111">`Microsoft.NET.Sdk.Web` (Web SDK) depends on other SDKs, including `Microsoft.NET.Sdk` (.NET Core SDK) and `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)).</span></span> <span data-ttu-id="85129-112">Bağımlı her SDK ile ilişkili hedefler ve MSBuild özellikleri içeri aktarılır.</span><span class="sxs-lookup"><span data-stu-id="85129-112">The MSBuild properties and targets associated with each dependent SDK are imported.</span></span> <span data-ttu-id="85129-113">Hedefler içe hedefleri kullanılan Yayımla yöntemine göre uygun kümesini yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="85129-113">Publish targets import the appropriate set of targets based on the publish method used.</span></span>

<span data-ttu-id="85129-114">MSBuild veya Visual Studio bir projeyi yüklediğinde, aşağıdaki üst düzey eylemler gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="85129-114">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="85129-115">Proje derleme</span><span class="sxs-lookup"><span data-stu-id="85129-115">Build project</span></span>
* <span data-ttu-id="85129-116">Yayımlamak için dosyaların işlem</span><span class="sxs-lookup"><span data-stu-id="85129-116">Compute files to publish</span></span>
* <span data-ttu-id="85129-117">Hedef dosya yayımlama</span><span class="sxs-lookup"><span data-stu-id="85129-117">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="85129-118">Proje öğeleri işlem</span><span class="sxs-lookup"><span data-stu-id="85129-118">Compute project items</span></span>

<span data-ttu-id="85129-119">Proje yüklendiğinde [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) (dosyalar) hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="85129-119">When the project is loaded, the [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) (files) are computed.</span></span> <span data-ttu-id="85129-120">Öğe türü, dosyanın nasıl işleneceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="85129-120">The item type determines how the file is processed.</span></span> <span data-ttu-id="85129-121">Varsayılan olarak, *.cs* dosyaları dahil edilecek `Compile` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="85129-121">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="85129-122">Dosyalar `Compile` öğe listesi derlenir.</span><span class="sxs-lookup"><span data-stu-id="85129-122">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="85129-123">`Content` Öğesi listesinin yanı sıra derleme çıktılarını yayımlanan dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="85129-123">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="85129-124">Varsayılan olarak, dosyaları desenlerle eşleşen `wwwroot\**`, `**\*.config`, ve `**\*.json` dahil `Content` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="85129-124">By default, files matching the patterns `wwwroot\**`, `**\*.config`, and `**\*.json` are included in the `Content` item list.</span></span> <span data-ttu-id="85129-125">Örneğin, `wwwroot\**` [Glob deseni](https://gruntjs.com/configuring-tasks#globbing-patterns) eşleşen tüm dosyaları *wwwroot* klasör **ve** klasörlerinden.</span><span class="sxs-lookup"><span data-stu-id="85129-125">For example, the `wwwroot\**` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** its subfolders.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="85129-126">Web SDK'sı aktarır [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="85129-126">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="85129-127">Bunun sonucunda, dosyaları desenlerle eşleşen `**\*.cshtml` ve `**\*.razor` de dahil edilir `Content` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="85129-127">As a result, files matching the patterns `**\*.cshtml` and `**\*.razor` are also included in the `Content` item list.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="85129-128">Web SDK'sı aktarır [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="85129-128">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="85129-129">Bunun sonucunda, dosyaları eşleşen `**\*.cshtml` düzeni de dahil edilecek `Content` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="85129-129">As a result, files matching the `**\*.cshtml` pattern are also included in the `Content` item list.</span></span>

::: moniker-end

<span data-ttu-id="85129-130">Açıkça Yayımla listesine bir dosya eklemek için dosyanın doğrudan ekleme *.csproj* gösterildiği gibi dosya [dosyaları içerir](#include-files) bölümü.</span><span class="sxs-lookup"><span data-stu-id="85129-130">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in the [Include Files](#include-files) section.</span></span>

<span data-ttu-id="85129-131">Seçerken **Yayımla** düğme Visual Studio'da veya komut satırından yayımlama sırasında:</span><span class="sxs-lookup"><span data-stu-id="85129-131">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="85129-132">Özellikler/öğeleri hesaplanır (oluşturmak için gereken dosyaları).</span><span class="sxs-lookup"><span data-stu-id="85129-132">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="85129-133">**Yalnızca Visual Studio**: NuGet paketlerini geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="85129-133">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="85129-134">(Geri yükleme CLI kullanıcı tarafından açık olması gerekir.)</span><span class="sxs-lookup"><span data-stu-id="85129-134">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="85129-135">Projeyi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="85129-135">The project builds.</span></span>
* <span data-ttu-id="85129-136">Yayımlama öğeleri hesaplanır (yayımlama için gerekli dosyaları).</span><span class="sxs-lookup"><span data-stu-id="85129-136">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="85129-137">Proje yayımlandığında (hesaplanan dosyalar Yayımla hedefe kopyalanır).</span><span class="sxs-lookup"><span data-stu-id="85129-137">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="85129-138">Bir ASP.NET Core projesi başvurduğunda `Microsoft.NET.Sdk.Web` proje dosyasında bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="85129-138">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="85129-139">Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="85129-139">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="85129-140">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="85129-140">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="85129-141">Temel bir komut satırı yayımlama</span><span class="sxs-lookup"><span data-stu-id="85129-141">Basic command-line publishing</span></span>

<span data-ttu-id="85129-142">Komut satırı yayımlama, .NET Core tarafından desteklenen tüm platformlarda çalışır ve Visual Studio gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="85129-142">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="85129-143">Aşağıdaki örnekte [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu proje dizininden çalıştırın (içeren *.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="85129-143">In the following samples, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="85129-144">Aksi durumda proje klasöründe proje dosya yolu açıkça geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85129-144">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="85129-145">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="85129-145">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="85129-146">Oluşturma ve bir web uygulaması yayımlamak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="85129-146">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet publish
```

<span data-ttu-id="85129-147">[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komut aşağıdakine benzer bir çıktı üretir:</span><span class="sxs-lookup"><span data-stu-id="85129-147">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {version} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\publish\
```

<span data-ttu-id="85129-148">Varsayılan publish klasörüne olan `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="85129-148">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="85129-149">İçin varsayılan `$(Configuration)` olduğu *hata ayıklama*.</span><span class="sxs-lookup"><span data-stu-id="85129-149">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="85129-150">Önceki örnekte, `<TargetFramework>` olduğu `netcoreapp{X.Y}`.</span><span class="sxs-lookup"><span data-stu-id="85129-150">In the preceding sample, the `<TargetFramework>` is `netcoreapp{X.Y}`.</span></span>

<span data-ttu-id="85129-151">`dotnet publish -h` bilgi yayımlama için yardımı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="85129-151">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="85129-152">Aşağıdaki komut belirtir bir `Release` derleme ve yayımlama dizini:</span><span class="sxs-lookup"><span data-stu-id="85129-152">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="85129-153">[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) çağrıları çağıran MSBuild komut `Publish` hedef.</span><span class="sxs-lookup"><span data-stu-id="85129-153">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="85129-154">Herhangi bir parametre geçirilen `dotnet publish` MSBuild'e geçirilir.</span><span class="sxs-lookup"><span data-stu-id="85129-154">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="85129-155">`-c` Parametre eşlenen `Configuration` MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="85129-155">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="85129-156">`-o` Parametre eşlenen `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="85129-156">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="85129-157">MSBuild özellikleri, aşağıdaki biçimlerden birini kullanarak geçirilebilir:</span><span class="sxs-lookup"><span data-stu-id="85129-157">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="85129-158">Aşağıdaki komutu yayımlar bir `Release` bir ağ paylaşımına oluşturun:</span><span class="sxs-lookup"><span data-stu-id="85129-158">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="85129-159">Ağ paylaşımı eğik çizgi ile belirtilen ( *//r8/* ) ve .NET Core desteklenen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="85129-159">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="85129-160">Yayımlanan uygulama dağıtımı için çalışmadığından emin onaylayın.</span><span class="sxs-lookup"><span data-stu-id="85129-160">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="85129-161">Dosyalar *yayımlama* klasörü, uygulama çalışırken kilitlenir.</span><span class="sxs-lookup"><span data-stu-id="85129-161">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="85129-162">Dağıtım kilitli olduğundan dosya kopyalanamadı oluşamaz.</span><span class="sxs-lookup"><span data-stu-id="85129-162">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="85129-163">Yayımlama profilleri</span><span class="sxs-lookup"><span data-stu-id="85129-163">Publish profiles</span></span>

<span data-ttu-id="85129-164">Bu bölümde Visual Studio 2017 veya sonraki bir yayımlama profili oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="85129-164">This section uses Visual Studio 2017 or later to create a publishing profile.</span></span> <span data-ttu-id="85129-165">Visual Studio ya da komut satırından yayımlama profili oluşturulduktan sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="85129-165">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="85129-166">Yayımlama profilleri yayımlama işlemini basitleştirmek ve herhangi bir sayıda profilleri bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="85129-166">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="85129-167">Bir yayımlama profili, aşağıdaki yollardan birini seçerek Visual Studio'da oluşturun:</span><span class="sxs-lookup"><span data-stu-id="85129-167">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="85129-168">Projeye sağ **Çözüm Gezgini** seçip **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="85129-168">Right-click the project in **Solution Explorer** and select **Publish**.</span></span>
* <span data-ttu-id="85129-169">Seçin **yayımlama {proje adı}** gelen **derleme** menüsü.</span><span class="sxs-lookup"><span data-stu-id="85129-169">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="85129-170">**Yayımla** uygulama kapasiteler sayfasının sekmesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="85129-170">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="85129-171">Proje bir yayımlama profili sahip değilse, aşağıdaki sayfası görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="85129-171">If the project lacks a publish profile, the following page is displayed:</span></span>

![Yayımlama sekmesindeki uygulama kapasiteler sayfasının](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="85129-173">Zaman **klasör** olduğu belirlenirse, yayımlanan varlıkları depolamak için bir klasör yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="85129-173">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="85129-174">Varsayılan klasör *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="85129-174">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="85129-175">Tıklayın **profili oluştur** düğmesini tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="85129-175">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="85129-176">Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekmesinde değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="85129-176">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="85129-177">Yeni oluşturulan profil aşağı açılan listede görünür.</span><span class="sxs-lookup"><span data-stu-id="85129-177">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="85129-178">Tıklayın **yeni profil oluşturma** başka bir yeni profili oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="85129-178">Click **Create new profile** to create another new profile.</span></span>

![Yayımlama sekmesindeki FolderProfile gösteren uygulama kapasiteler sayfasının](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="85129-180">Yayımlama Sihirbazı'nı aşağıdaki Yayımla hedeflerini destekler:</span><span class="sxs-lookup"><span data-stu-id="85129-180">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="85129-181">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="85129-181">Azure App Service</span></span>
* <span data-ttu-id="85129-182">Azure sanal makineleri</span><span class="sxs-lookup"><span data-stu-id="85129-182">Azure Virtual Machines</span></span>
* <span data-ttu-id="85129-183">IIS, FTP, vb. (için herhangi bir web sunucusu)</span><span class="sxs-lookup"><span data-stu-id="85129-183">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="85129-184">Klasör</span><span class="sxs-lookup"><span data-stu-id="85129-184">Folder</span></span>
* <span data-ttu-id="85129-185">Profili içeri aktar</span><span class="sxs-lookup"><span data-stu-id="85129-185">Import Profile</span></span>

<span data-ttu-id="85129-186">Daha fazla bilgi için [hangi Yayımlama seçenekleri benim için en uygun](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="85129-186">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="85129-187">Visual Studio ile bir yayımlama profili oluştururken, bir *özellikleri/PublishProfiles / {PROFİL adı} .pubxml* MSBuild dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="85129-187">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="85129-188">*.Pubxml* dosyanın MSBuild dosyası ve içeren yapılandırma ayarlarını yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="85129-188">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="85129-189">Bu dosya, yapı özelleştirme ve yayımlama işlemi için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="85129-189">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="85129-190">Bu dosya yayımlama işlemi tarafından okunur.</span><span class="sxs-lookup"><span data-stu-id="85129-190">This file is read by the publishing process.</span></span> <span data-ttu-id="85129-191">`<LastUsedBuildConfiguration>` Genel bir özellik olduğundan ve yapı alınan herhangi bir dosya olmamalıdır özeldir.</span><span class="sxs-lookup"><span data-stu-id="85129-191">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="85129-192">Bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="85129-192">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="85129-193">Azure bir hedefe, yayımlama sırasında *.pubxml* dosyası, Azure abonelik tanımlayıcısı içeriyor.</span><span class="sxs-lookup"><span data-stu-id="85129-193">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="85129-194">Bu dosya kaynak denetimine eklemeye, hedef türüyle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="85129-194">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="85129-195">Azure dışı hedef yayımlama sırasında iade etmeye güvenli *.pubxml* dosya.</span><span class="sxs-lookup"><span data-stu-id="85129-195">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="85129-196">(Yayımlama parola gibi) hassas bilgiler şifreli bir kullanıcı/makine düzeyinde başına.</span><span class="sxs-lookup"><span data-stu-id="85129-196">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="85129-197">İçinde depolanan *özellikleri/PublishProfiles / {PROFİL adı}.pubxml.user* dosya.</span><span class="sxs-lookup"><span data-stu-id="85129-197">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="85129-198">Bu dosya, hassas bilgileri depolayabileceğiniz için kaynak denetimine iade olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="85129-198">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="85129-199">Nasıl bir ASP.NET Core web uygulaması yayımlamak genel bakış için bkz. [konak dağıtıp](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="85129-199">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="85129-200">MSBuild görevleri ve hedefleri ASP.NET Core uygulaması yayımlamak için gerekli açık kaynaklı, [aspnet/websdk depo](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="85129-200">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at the [aspnet/websdk repository](https://github.com/aspnet/websdk).</span></span>

<span data-ttu-id="85129-201">`dotnet publish` MSDeploy, klasörü kullanabilirsiniz ve [Kudu](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri:</span><span class="sxs-lookup"><span data-stu-id="85129-201">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="85129-202">(Çalışan platformlar arası) klasörü:</span><span class="sxs-lookup"><span data-stu-id="85129-202">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="85129-203">MSDeploy (şu anda bu tek çalışır platformlar arası MSDeploy olmadığından bu yana Windows):</span><span class="sxs-lookup"><span data-stu-id="85129-203">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="85129-204">MSDeploy paket (şu anda bu tek çalışır platformlar arası MSDeploy olmadığından bu yana Windows):</span><span class="sxs-lookup"><span data-stu-id="85129-204">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="85129-205">Yukarıdaki örneklerde geçirmezseniz `deployonbuild` için `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="85129-205">In the preceding examples, don't pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="85129-206">Daha fazla bilgi için [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="85129-206">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="85129-207">`dotnet publish` her platformda dizinden Azure'da yayımlamak için Kudu API'leri destekler.</span><span class="sxs-lookup"><span data-stu-id="85129-207">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="85129-208">Visual Studio yayımlama Kudu API'leri, ancak desteklenen WebSDK ile platformlar arası yayımlamak için Azure'a destekler.</span><span class="sxs-lookup"><span data-stu-id="85129-208">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="85129-209">Eklemek için bir yayımlama profili *özellikleri/PublishProfiles* klasöründe aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="85129-209">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="85129-210">Yayımla içerikleri zip ve Kudu API'lerini kullanarak Azure'da yayımlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="85129-210">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="85129-211">Bir yayımlama profili kullanırken aşağıdaki MSBuild özellikleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="85129-211">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

<span data-ttu-id="85129-212">Adlı bir profille yayımlarken *FolderProfile*, aşağıdaki komutlardan birini yürütülebilir:</span><span class="sxs-lookup"><span data-stu-id="85129-212">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="85129-213">Çağrılırken [dotnet derleme](/dotnet/core/tools/dotnet-build), çağrı `msbuild` yapıyı çalıştırmak ve işlem yayınlamak için.</span><span class="sxs-lookup"><span data-stu-id="85129-213">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="85129-214">Ya da çağırma `dotnet build` veya `msbuild` klasör profilinde geçerken eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="85129-214">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="85129-215">MSBuild doğrudan Windows üzerinde çağrıldığında, MSBuild .NET Framework sürümü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="85129-215">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="85129-216">MSDeploy yayımlama için Windows makineleri için şu anda sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="85129-216">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="85129-217">Çağırma `dotnet build` klasördeki olmayan profili çağırır MSBuild ve MSBuild olmayan klasör profillerde MSDeploy kullanır.</span><span class="sxs-lookup"><span data-stu-id="85129-217">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="85129-218">Çağırma `dotnet build` klasörü olmayan profilinde (MSDeploy kullanarak) MSBuild çağırır ve (hatta Windows platformunda çalışırken) içinde bir hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="85129-218">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="85129-219">Bir klasörü olmayan profili ile yayımlamak için MSBuild doğrudan çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="85129-219">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="85129-220">Aşağıdaki klasörü yayımlama profili Visual Studio ile oluşturulmuş ve bir ağ paylaşımına yayımlar:</span><span class="sxs-lookup"><span data-stu-id="85129-220">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="85129-221">Önceki örnekte `<LastUsedBuildConfiguration>` ayarlanır `Release`.</span><span class="sxs-lookup"><span data-stu-id="85129-221">In the preceding example, `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="85129-222">Visual Studio'dan yayımlama sırasında `<LastUsedBuildConfiguration>` yapılandırma özellik değeri, yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="85129-222">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="85129-223">`<LastUsedBuildConfiguration>` Yapılandırma özelliği özeldir ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınan olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="85129-223">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="85129-224">Bu özellik komut satırından geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="85129-224">This property can be overridden from the command line.</span></span>

<span data-ttu-id="85129-225">.NET Core CLI kullanarak:</span><span class="sxs-lookup"><span data-stu-id="85129-225">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="85129-226">MSBuild kullanarak:</span><span class="sxs-lookup"><span data-stu-id="85129-226">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="85129-227">Komut satırından bir MSDeploy uç noktasına yayımlama</span><span class="sxs-lookup"><span data-stu-id="85129-227">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="85129-228">Aşağıdaki örnekte adlı Visual Studio tarafından oluşturulan bir ASP.NET Core web uygulaması *AzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="85129-228">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="85129-229">Azure uygulamaları yayımlama profili Visual Studio ile eklenir.</span><span class="sxs-lookup"><span data-stu-id="85129-229">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="85129-230">Profil oluşturma hakkında daha fazla bilgi için bkz. [yayımlama profillerini](#publish-profiles) bölümü.</span><span class="sxs-lookup"><span data-stu-id="85129-230">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="85129-231">Bir yayımlama profili kullanarak uygulama dağıtmak için yürütme `msbuild` Visual Studio'dan komutunu **Geliştirici komut istemi**.</span><span class="sxs-lookup"><span data-stu-id="85129-231">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="85129-232">Komut istemi kullanılabilir *Visual Studio* klasörü **Başlat** Windows görev çubuğundaki menü.</span><span class="sxs-lookup"><span data-stu-id="85129-232">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="85129-233">Daha kolay erişim için komut satırına ekleyebilirsiniz **Araçları** Visual Studio'daki menü.</span><span class="sxs-lookup"><span data-stu-id="85129-233">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="85129-234">Daha fazla bilgi için [Visual Studio için geliştirici komut istemi](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="85129-234">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="85129-235">MSBuild, şu komut söz dizimini kullanır:</span><span class="sxs-lookup"><span data-stu-id="85129-235">MSBuild uses the following command syntax:</span></span>

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="85129-236">{PATH} &ndash; Uygulamanın proje dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="85129-236">{PATH} &ndash; Path to the app's project file.</span></span>
* <span data-ttu-id="85129-237">{} PROFİLİ &ndash; Yayımlama profilinin adı.</span><span class="sxs-lookup"><span data-stu-id="85129-237">{PROFILE} &ndash; Name of the publish profile.</span></span>
* <span data-ttu-id="85129-238">{USERNAME} &ndash; MSDeploy kullanıcı adı.</span><span class="sxs-lookup"><span data-stu-id="85129-238">{USERNAME} &ndash; MSDeploy username.</span></span> <span data-ttu-id="85129-239">{USERNAME} yayımlama profilinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="85129-239">The {USERNAME} can be found in the publish profile.</span></span>
* <span data-ttu-id="85129-240">{PASSWORD} &ndash; MSDeploy parola.</span><span class="sxs-lookup"><span data-stu-id="85129-240">{PASSWORD} &ndash; MSDeploy password.</span></span> <span data-ttu-id="85129-241">{PASSWORD} almak *{profili}. PublishSettings* dosya.</span><span class="sxs-lookup"><span data-stu-id="85129-241">Obtain the {PASSWORD} from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="85129-242">İndirme *. PublishSettings* dosyasından ya da:</span><span class="sxs-lookup"><span data-stu-id="85129-242">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="85129-243">Çözüm Gezgini için: Seçin **görünümü** > **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="85129-243">Solution Explorer: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="85129-244">Azure aboneliğinizle bağlanın.</span><span class="sxs-lookup"><span data-stu-id="85129-244">Connect with your Azure subscription.</span></span> <span data-ttu-id="85129-245">Açık **uygulama hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="85129-245">Open **App Services**.</span></span> <span data-ttu-id="85129-246">Uygulamaya sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="85129-246">Right-click the app.</span></span> <span data-ttu-id="85129-247">Seçin **yayımlama profili indir**.</span><span class="sxs-lookup"><span data-stu-id="85129-247">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="85129-248">Azure portalı: Seçin **yayımlama profili Al** web uygulamasının **genel bakış** paneli.</span><span class="sxs-lookup"><span data-stu-id="85129-248">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="85129-249">Aşağıdaki örnekte adlı bir yayımlama profili *AzureWebApp - Web dağıtımı*:</span><span class="sxs-lookup"><span data-stu-id="85129-249">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="85129-250">Bir yayımlama profili, .NET Core CLI ile de kullanılabilir [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) bir Windows komut isteminden komutu:</span><span class="sxs-lookup"><span data-stu-id="85129-250">A publish profile can also be used with the .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command prompt:</span></span>

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> <span data-ttu-id="85129-251">[Dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) komut kullanılabilir platformlar arası ve macOS ve Linux'ta ASP.NET Core uygulamaları derleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85129-251">The [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command is available cross-platform and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="85129-252">Ancak, MSBuild MacOS ve Linux Azure veya başka bir MSDeploy uç noktası için bir uygulama dağıtmaya yeteneğine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="85129-252">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoint.</span></span> <span data-ttu-id="85129-253">MSDeploy, yalnızca Windows üzerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="85129-253">MSDeploy is only available on Windows.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="85129-254">Ortamı ayarlama</span><span class="sxs-lookup"><span data-stu-id="85129-254">Set the environment</span></span>

<span data-ttu-id="85129-255">Dahil `<EnvironmentName>` yayımlama profilini özelliğinde ( *.pubxml*) veya uygulamanın ayarlamak için proje dosyasını [ortam](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="85129-255">Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file to set the app's [environment](xref:fundamentals/environments):</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="85129-256">Gerektiriyorsa *web.config* Dönüşümleri (yapılandırma, profili veya ortama göre örneğin, ortam değişkenlerini ayarlama), bkz: <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="85129-256">If you require *web.config* transformations (for example, setting environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="85129-257">Dosyaları dışarıda bırak</span><span class="sxs-lookup"><span data-stu-id="85129-257">Exclude files</span></span>

<span data-ttu-id="85129-258">ASP.NET Core web uygulamaları yayımlarken, aşağıdaki varlıklar dahildir:</span><span class="sxs-lookup"><span data-stu-id="85129-258">When publishing ASP.NET Core web apps, the following assets are included:</span></span>

* <span data-ttu-id="85129-259">Derleme yapıları</span><span class="sxs-lookup"><span data-stu-id="85129-259">Build artifacts</span></span>
* <span data-ttu-id="85129-260">Dosya ve klasörleri aşağıdaki Glob desenlerinin eşleşen:</span><span class="sxs-lookup"><span data-stu-id="85129-260">Folders and files matching the following globbing patterns:</span></span>
  * <span data-ttu-id="85129-261">`**\*.config` (örneğin, *web.config*)</span><span class="sxs-lookup"><span data-stu-id="85129-261">`**\*.config` (for example, *web.config*)</span></span>
  * <span data-ttu-id="85129-262">`**\*.json` (örneğin, *appsettings.json*)</span><span class="sxs-lookup"><span data-stu-id="85129-262">`**\*.json` (for example, *appsettings.json*)</span></span>
  * `wwwroot\**`

<span data-ttu-id="85129-263">MSBuild destekler [Glob desenlerinin](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="85129-263">MSBuild supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="85129-264">Örneğin, aşağıdaki `<Content>` öğesi metnin kopyalanmasını engeller ( *.txt*) dosyalarını *wwwroot\content* klasör ve alt klasörleri:</span><span class="sxs-lookup"><span data-stu-id="85129-264">For example, the following `<Content>` element suppresses the copying of text (*.txt*) files in the *wwwroot\content* folder and its subfolders:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="85129-265">Önceki işaretleme için bir yayımlama profili eklenebilir veya *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="85129-265">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="85129-266">Eklenen *.csproj* dosya, kural için eklenir projedeki tüm yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="85129-266">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="85129-267">Aşağıdaki `<MsDeploySkipRules>` öğe tüm dosyaları dışlar *wwwroot\content* klasörü:</span><span class="sxs-lookup"><span data-stu-id="85129-267">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot\content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="85129-268">`<MsDeploySkipRules>` silmez *atla* dağıtım sitesinden hedefler.</span><span class="sxs-lookup"><span data-stu-id="85129-268">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="85129-269">`<Content>` hedef dosyalar ve klasörler dağıtım site veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="85129-269">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="85129-270">Örneğin, aşağıdaki dosyaları dağıtılan web uygulaması olduğu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="85129-270">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="85129-271">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="85129-271">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="85129-272">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="85129-272">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="85129-273">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="85129-273">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="85129-274">Aşağıdaki `<MsDeploySkipRules>` öğeleri eklenir, dağıtım sitesinde bu dosyaları silseniz mıydı.</span><span class="sxs-lookup"><span data-stu-id="85129-274">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="85129-275">Önceki `<MsDeploySkipRules>` öğeleri önlemek *atlandı* dosyalarının dağıtılıyor.</span><span class="sxs-lookup"><span data-stu-id="85129-275">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="85129-276">Bunlar dağıttıktan sonra bu dosyaları silinmez.</span><span class="sxs-lookup"><span data-stu-id="85129-276">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="85129-277">Aşağıdaki `<Content>` öğesi dağıtım sitede hedeflenen dosyaları siler:</span><span class="sxs-lookup"><span data-stu-id="85129-277">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="85129-278">Önceki komut satırı dağıtımı kullanarak `<Content>` öğesi aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="85129-278">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a><span data-ttu-id="85129-279">Dosyaları Ekle</span><span class="sxs-lookup"><span data-stu-id="85129-279">Include files</span></span>

<span data-ttu-id="85129-280">Aşağıdaki biçimlendirmede:</span><span class="sxs-lookup"><span data-stu-id="85129-280">The following markup:</span></span>

* <span data-ttu-id="85129-281">İçeren bir *görüntüleri* klasörü için proje dizininin dışına *wwwroot/görüntülerinden* Yayımla sitenin klasörü.</span><span class="sxs-lookup"><span data-stu-id="85129-281">Includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>
* <span data-ttu-id="85129-282">Eklenebilir *.csproj* dosya veya yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="85129-282">Can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="85129-283">Kümeye eklenirse *.csproj* dosyası, onu eklendi projedeki her yayımlama profilinde.</span><span class="sxs-lookup"><span data-stu-id="85129-283">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="85129-284">Aşağıdaki biçimlendirme gösterir nasıl vurgulanmış için:</span><span class="sxs-lookup"><span data-stu-id="85129-284">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="85129-285">Projeye dışında dosyasından kopyalama *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="85129-285">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="85129-286">Dışlama *wwwroot\Content* klasör.</span><span class="sxs-lookup"><span data-stu-id="85129-286">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="85129-287">Dışlama *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="85129-287">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="85129-288">Bkz: [Web SDK'sı depoya Benioku](https://github.com/aspnet/websdk) daha fazla dağıtım örneği için.</span><span class="sxs-lookup"><span data-stu-id="85129-288">See the [Web SDK repository Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="85129-289">Bir hedef önce veya sonra yayımlama çalıştırın</span><span class="sxs-lookup"><span data-stu-id="85129-289">Run a target before or after publishing</span></span>

<span data-ttu-id="85129-290">Yerleşik `BeforePublish` ve `AfterPublish` hedefleri yürütmenizi hedef önce veya sonra yayımlama hedefi.</span><span class="sxs-lookup"><span data-stu-id="85129-290">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="85129-291">Yayımlama profili öncesinde ve sonrasında yayımlama konsolu iletilerini günlüğe kaydetmek için aşağıdaki öğeleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="85129-291">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="85129-292">Güvenilmeyen bir sertifika kullanarak bir sunucuda yayımlayın</span><span class="sxs-lookup"><span data-stu-id="85129-292">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="85129-293">Ekleme `<AllowUntrustedCertificate>` özellik değeriyle `True` yayımlama profili için:</span><span class="sxs-lookup"><span data-stu-id="85129-293">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="85129-294">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="85129-294">The Kudu service</span></span>

<span data-ttu-id="85129-295">Bir Azure App Service web uygulaması dağıtımı ' dosyaları görüntülemek için kullanın [Kudu hizmet](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="85129-295">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="85129-296">Append `scm` belirteç için web uygulaması adı.</span><span class="sxs-lookup"><span data-stu-id="85129-296">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="85129-297">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="85129-297">For example:</span></span>

| <span data-ttu-id="85129-298">URL</span><span class="sxs-lookup"><span data-stu-id="85129-298">URL</span></span>                                    | <span data-ttu-id="85129-299">Sonuç</span><span class="sxs-lookup"><span data-stu-id="85129-299">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="85129-300">Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="85129-300">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="85129-301">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="85129-301">Kudu service</span></span> |

<span data-ttu-id="85129-302">Seçin [hata ayıklama konsolunu](https://github.com/projectkudu/kudu/wiki/Kudu-console) görüntülemek, düzenlemek, silmek veya dosya eklemek için menü öğesi.</span><span class="sxs-lookup"><span data-stu-id="85129-302">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="85129-303">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="85129-303">Additional resources</span></span>

* <span data-ttu-id="85129-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) web uygulamaları ve IIS sunucuları için Web siteleri dağıtımı (MSDeploy) basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="85129-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="85129-305">[Web SDK'sı GitHub deposu](https://github.com/aspnet/websdk/issues): Dosya sorunları ve istek için dağıtım özellikleri.</span><span class="sxs-lookup"><span data-stu-id="85129-305">[Web SDK GitHub repository](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="85129-306">Visual Studio'dan Azure VM için bir ASP.NET Web uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="85129-306">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
