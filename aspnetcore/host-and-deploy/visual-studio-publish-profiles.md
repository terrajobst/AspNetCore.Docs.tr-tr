---
title: Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı
author: rick-anderson
description: Oluşturmayı Visual Studio'da yayımlama profilleri ve ASP.NET Core uygulama dağıtımlarını çeşitli hedeflere yönetmek için kullanın.
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: e1e8f99be18d6f395a146bda805f71c46cd0346d
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248361"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="cdb4a-103">Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı</span><span class="sxs-lookup"><span data-stu-id="cdb4a-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="cdb4a-104">Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cdb4a-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="cdb4a-105">Bu konuda 1.1 sürümü için indirme [Visual Studio yayımlama profilleri ASP.NET Core uygulaması dağıtımı'nı (sürüm 1.1, PDF) için](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-105">For the 1.1 version of this topic, download [Visual Studio publish profiles for ASP.NET Core app deployment (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="cdb4a-106">Bu belge, Visual Studio 2017 kullanarak veya daha sonra oluşturma ve kullanma odaklanır yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-106">This document focuses on using Visual Studio 2017 or later to create and use publish profiles.</span></span> <span data-ttu-id="cdb4a-107">Visual Studio ile oluşturulan yayımlama profillerine MSBuild ve Visual Studio çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio.</span></span> <span data-ttu-id="cdb4a-108">Bkz: [Visual Studio kullanarak Azure App Service'e bir ASP.NET Core web uygulaması yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) Azure'da yayımlamak için yönergeler.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="cdb4a-109">Aşağıdaki proje dosyasını komutu ile oluşturulmuş `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-109">The following project file was created with the command `dotnet new mvc`:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

<span data-ttu-id="cdb4a-110">`<Project>` Öğenin `Sdk` özniteliği aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-110">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="cdb4a-111">Özellikleri dosyasından içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* başında.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-111">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="cdb4a-112">Hedefleri dosyasından içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* sonunda.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-112">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="cdb4a-113">Varsayılan konumu `MSBuildSDKsPath` (Visual Studio 2017 Enterprise ile) *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* klasör.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-113">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="cdb4a-114">`Microsoft.NET.Sdk.Web` SDK bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-114">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="cdb4a-115">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="cdb4a-115">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="cdb4a-116">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="cdb4a-116">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="cdb4a-117">Neden olan aşağıdaki özellikleri ve içeri aktarılacak hedefler:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-117">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="cdb4a-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="cdb4a-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="cdb4a-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="cdb4a-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="cdb4a-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="cdb4a-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="cdb4a-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="cdb4a-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="cdb4a-122">Kullanılan yayımlama yöntemi temel hedefleri sağ kümesini hedefler içe yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-122">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="cdb4a-123">MSBuild veya Visual Studio bir projeyi yüklediğinde, aşağıdaki üst düzey eylemler gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-123">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="cdb4a-124">Proje derleme</span><span class="sxs-lookup"><span data-stu-id="cdb4a-124">Build project</span></span>
* <span data-ttu-id="cdb4a-125">Yayımlamak için dosyaların işlem</span><span class="sxs-lookup"><span data-stu-id="cdb4a-125">Compute files to publish</span></span>
* <span data-ttu-id="cdb4a-126">Hedef dosya yayımlama</span><span class="sxs-lookup"><span data-stu-id="cdb4a-126">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="cdb4a-127">Proje öğeleri işlem</span><span class="sxs-lookup"><span data-stu-id="cdb4a-127">Compute project items</span></span>

<span data-ttu-id="cdb4a-128">Proje yüklendiğinde, proje öğeler (dosyalar) hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-128">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="cdb4a-129">`item type` Özniteliği dosyanın nasıl işleneceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-129">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="cdb4a-130">Varsayılan olarak, *.cs* dosyaları dahil edilecek `Compile` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-130">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="cdb4a-131">Dosyalar `Compile` öğe listesi derlenir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-131">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="cdb4a-132">`Content` Öğesi listesinin yanı sıra derleme çıktılarını yayımlanan dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-132">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="cdb4a-133">Varsayılan olarak, dosyaları desen eşleştirme `wwwroot/**` dahil `Content` öğesi.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-133">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="cdb4a-134">`wwwroot/\*\*` [Glob deseni](https://gruntjs.com/configuring-tasks#globbing-patterns) eşleşen tüm dosyaları *wwwroot* klasör **ve** alt.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-134">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="cdb4a-135">Açıkça Yayımla listesine bir dosya eklemek için dosyanın doğrudan ekleme *.csproj* gösterildiği gibi dosya [dosyaları içerir](#include-files).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-135">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="cdb4a-136">Seçerken **Yayımla** düğme Visual Studio'da veya komut satırından yayımlama sırasında:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-136">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="cdb4a-137">Özellikler/öğeleri hesaplanır (oluşturmak için gereken dosyaları).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-137">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="cdb4a-138">**Yalnızca Visual Studio**: NuGet paketlerini geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-138">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="cdb4a-139">(Geri yükleme CLI kullanıcı tarafından açık olması gerekir.)</span><span class="sxs-lookup"><span data-stu-id="cdb4a-139">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="cdb4a-140">Projeyi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-140">The project builds.</span></span>
* <span data-ttu-id="cdb4a-141">Yayımlama öğeleri hesaplanır (yayımlama için gerekli dosyaları).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-141">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="cdb4a-142">Proje yayımlandığında (hesaplanan dosyalar Yayımla hedefe kopyalanır).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-142">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="cdb4a-143">Bir ASP.NET Core projesi başvurduğunda `Microsoft.NET.Sdk.Web` proje dosyasında bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-143">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="cdb4a-144">Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-144">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="cdb4a-145">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-145">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="cdb4a-146">Temel bir komut satırı yayımlama</span><span class="sxs-lookup"><span data-stu-id="cdb4a-146">Basic command-line publishing</span></span>

<span data-ttu-id="cdb4a-147">Komut satırı yayımlama, .NET Core tarafından desteklenen tüm platformlarda çalışır ve Visual Studio gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-147">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="cdb4a-148">Aşağıdaki örnekte [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu proje dizininden çalıştırın (içeren *.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-148">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="cdb4a-149">Aksi durumda proje klasöründe proje dosya yolu açıkça geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-149">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="cdb4a-150">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-150">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="cdb4a-151">Oluşturma ve bir web uygulaması yayımlamak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-151">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet publish
```

<span data-ttu-id="cdb4a-152">[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komut aşağıdakine benzer bir çıktı üretir:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-152">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="cdb4a-153">Varsayılan publish klasörüne olan `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-153">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="cdb4a-154">İçin varsayılan `$(Configuration)` olduğu *hata ayıklama*.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-154">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="cdb4a-155">Önceki örnekte, `<TargetFramework>` olduğu `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-155">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="cdb4a-156">`dotnet publish -h` bilgi yayımlama için yardımı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-156">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="cdb4a-157">Aşağıdaki komut belirtir bir `Release` derleme ve yayımlama dizini:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-157">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="cdb4a-158">[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) çağrıları çağıran MSBuild komut `Publish` hedef.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-158">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="cdb4a-159">Herhangi bir parametre geçirilen `dotnet publish` MSBuild'e geçirilir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-159">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="cdb4a-160">`-c` Parametre eşlenen `Configuration` MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-160">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="cdb4a-161">`-o` Parametre eşlenen `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-161">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="cdb4a-162">MSBuild özellikleri, aşağıdaki biçimlerden birini kullanarak geçirilebilir:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-162">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="cdb4a-163">Aşağıdaki komutu yayımlar bir `Release` bir ağ paylaşımına oluşturun:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-163">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="cdb4a-164">Ağ paylaşımı eğik çizgi ile belirtilen (*//r8/*) ve .NET Core desteklenen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-164">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="cdb4a-165">Yayımlanan uygulama dağıtımı için çalışmadığından emin onaylayın.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-165">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="cdb4a-166">Dosyalar *yayımlama* klasörü, uygulama çalışırken kilitlenir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-166">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="cdb4a-167">Dağıtım kilitli olduğundan dosya kopyalanamadı oluşamaz.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-167">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="cdb4a-168">Yayımlama profilleri</span><span class="sxs-lookup"><span data-stu-id="cdb4a-168">Publish profiles</span></span>

<span data-ttu-id="cdb4a-169">Bu bölümde Visual Studio 2017 veya sonraki bir yayımlama profili oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-169">This section uses Visual Studio 2017 or later to create a publishing profile.</span></span> <span data-ttu-id="cdb4a-170">Visual Studio ya da komut satırından yayımlama profili oluşturulduktan sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-170">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="cdb4a-171">Yayımlama profilleri yayımlama işlemini basitleştirmek ve herhangi bir sayıda profilleri bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-171">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="cdb4a-172">Bir yayımlama profili, aşağıdaki yollardan birini seçerek Visual Studio'da oluşturun:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-172">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="cdb4a-173">Çözüm Gezgini'nde projeye sağ tıklayıp **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-173">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="cdb4a-174">Seçin **yayımlama {proje adı}** gelen **derleme** menüsü.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-174">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="cdb4a-175">**Yayımla** uygulama kapasiteler sayfasının sekmesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-175">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="cdb4a-176">Proje bir yayımlama profili sahip değilse, aşağıdaki sayfası görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-176">If the project lacks a publish profile, the following page is displayed:</span></span>

![Yayımlama sekmesindeki uygulama kapasiteler sayfasının](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="cdb4a-178">Zaman **klasör** olduğu belirlenirse, yayımlanan varlıkları depolamak için bir klasör yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-178">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="cdb4a-179">Varsayılan klasör *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-179">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="cdb4a-180">Tıklayın **profili oluştur** düğmesini tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-180">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="cdb4a-181">Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekmesinde değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-181">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="cdb4a-182">Yeni oluşturulan profil aşağı açılan listede görünür.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-182">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="cdb4a-183">Tıklayın **yeni profil oluşturma** başka bir yeni profili oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-183">Click **Create new profile** to create another new profile.</span></span>

![Yayımlama sekmesindeki FolderProfile gösteren uygulama kapasiteler sayfasının](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="cdb4a-185">Yayımlama Sihirbazı'nı aşağıdaki Yayımla hedeflerini destekler:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-185">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="cdb4a-186">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="cdb4a-186">Azure App Service</span></span>
* <span data-ttu-id="cdb4a-187">Azure sanal makineleri</span><span class="sxs-lookup"><span data-stu-id="cdb4a-187">Azure Virtual Machines</span></span>
* <span data-ttu-id="cdb4a-188">IIS, FTP, vb. (için herhangi bir web sunucusu)</span><span class="sxs-lookup"><span data-stu-id="cdb4a-188">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="cdb4a-189">Klasör</span><span class="sxs-lookup"><span data-stu-id="cdb4a-189">Folder</span></span>
* <span data-ttu-id="cdb4a-190">Profili içeri aktar</span><span class="sxs-lookup"><span data-stu-id="cdb4a-190">Import Profile</span></span>

<span data-ttu-id="cdb4a-191">Daha fazla bilgi için [hangi Yayımlama seçenekleri benim için en uygun](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-191">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="cdb4a-192">Visual Studio ile bir yayımlama profili oluştururken, bir *özellikleri/PublishProfiles / {PROFİL adı} .pubxml* MSBuild dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-192">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="cdb4a-193">*.Pubxml* dosyanın MSBuild dosyası ve içeren yapılandırma ayarlarını yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-193">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="cdb4a-194">Bu dosya, yapı özelleştirme ve yayımlama işlemi için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-194">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="cdb4a-195">Bu dosya yayımlama işlemi tarafından okunur.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-195">This file is read by the publishing process.</span></span> <span data-ttu-id="cdb4a-196">`<LastUsedBuildConfiguration>` Genel bir özellik olduğundan ve yapı alınan herhangi bir dosya olmamalıdır özeldir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-196">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="cdb4a-197">Bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-197">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="cdb4a-198">Azure bir hedefe, yayımlama sırasında *.pubxml* dosyası, Azure abonelik tanımlayıcısı içeriyor.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-198">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="cdb4a-199">Bu dosya kaynak denetimine eklemeye, hedef türüyle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-199">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="cdb4a-200">Azure dışı hedef yayımlama sırasında iade etmeye güvenli *.pubxml* dosya.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-200">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="cdb4a-201">(Yayımlama parola gibi) hassas bilgiler şifreli bir kullanıcı/makine düzeyinde başına.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-201">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="cdb4a-202">İçinde depolanan *özellikleri/PublishProfiles / {PROFİL adı}.pubxml.user* dosya.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-202">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="cdb4a-203">Bu dosya, hassas bilgileri depolayabileceğiniz için kaynak denetimine iade olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-203">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="cdb4a-204">Nasıl bir ASP.NET Core web uygulaması yayımlamak genel bakış için bkz. [konak dağıtıp](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-204">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="cdb4a-205">MSBuild görevleri ve hedefleri ASP.NET Core uygulaması yayımlamak için gerekli açık kaynaklı, https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-205">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="cdb4a-206">`dotnet publish` MSDeploy, klasörü kullanabilirsiniz ve [Kudu](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-206">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="cdb4a-207">(Çalışan platformlar arası) klasörü:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-207">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="cdb4a-208">MSDeploy (şu anda bu tek çalışır platformlar arası MSDeploy olmadığından bu yana Windows):</span><span class="sxs-lookup"><span data-stu-id="cdb4a-208">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="cdb4a-209">MSDeploy paket (şu anda bu tek çalışır platformlar arası MSDeploy olmadığından bu yana Windows):</span><span class="sxs-lookup"><span data-stu-id="cdb4a-209">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="cdb4a-210">Önceki örneklerdeki **yoksa** geçirmek `deployonbuild` için `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-210">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="cdb4a-211">Daha fazla bilgi için [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-211">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="cdb4a-212">`dotnet publish` her platformda dizinden Azure'da yayımlamak için Kudu API'leri destekler.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-212">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="cdb4a-213">Visual Studio yayımlama Kudu API'leri, ancak desteklenen WebSDK ile platformlar arası yayımlamak için Azure'a destekler.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-213">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="cdb4a-214">Eklemek için bir yayımlama profili *özellikleri/PublishProfiles* klasöründe aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-214">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="cdb4a-215">Yayımla içerikleri zip ve Kudu API'lerini kullanarak Azure'da yayımlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-215">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="cdb4a-216">Bir yayımlama profili kullanırken aşağıdaki MSBuild özellikleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-216">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

<span data-ttu-id="cdb4a-217">Adlı bir profille yayımlarken *FolderProfile*, aşağıdaki komutlardan birini yürütülebilir:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-217">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="cdb4a-218">Çağrılırken [dotnet derleme](/dotnet/core/tools/dotnet-build), çağrı `msbuild` yapıyı çalıştırmak ve işlem yayınlamak için.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-218">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="cdb4a-219">Ya da çağırma `dotnet build` veya `msbuild` klasör profilinde geçerken eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-219">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="cdb4a-220">MSBuild doğrudan Windows üzerinde çağrıldığında, MSBuild .NET Framework sürümü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-220">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="cdb4a-221">MSDeploy yayımlama için Windows makineleri için şu anda sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-221">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="cdb4a-222">Çağırma `dotnet build` klasördeki olmayan profili çağırır MSBuild ve MSBuild olmayan klasör profillerde MSDeploy kullanır.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-222">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="cdb4a-223">Çağırma `dotnet build` klasörü olmayan profilinde (MSDeploy kullanarak) MSBuild çağırır ve (hatta Windows platformunda çalışırken) içinde bir hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-223">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="cdb4a-224">Bir klasörü olmayan profili ile yayımlamak için MSBuild doğrudan çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-224">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="cdb4a-225">Aşağıdaki klasörü yayımlama profili Visual Studio ile oluşturulmuş ve bir ağ paylaşımına yayımlar:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-225">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="cdb4a-226">Not `<LastUsedBuildConfiguration>` ayarlanır `Release`.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-226">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="cdb4a-227">Visual Studio'dan yayımlama sırasında `<LastUsedBuildConfiguration>` yapılandırma özellik değeri, yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-227">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="cdb4a-228">`<LastUsedBuildConfiguration>` Yapılandırma özelliği özeldir ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınan olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-228">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="cdb4a-229">Bu özellik komut satırından geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-229">This property can be overridden from the command line.</span></span>

<span data-ttu-id="cdb4a-230">.NET Core CLI kullanarak:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-230">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="cdb4a-231">MSBuild kullanarak:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-231">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="cdb4a-232">Komut satırından bir MSDeploy uç noktasına yayımlama</span><span class="sxs-lookup"><span data-stu-id="cdb4a-232">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="cdb4a-233">Aşağıdaki örnekte adlı Visual Studio tarafından oluşturulan bir ASP.NET Core web uygulaması *AzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-233">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="cdb4a-234">Azure uygulamaları yayımlama profili Visual Studio ile eklenir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-234">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="cdb4a-235">Profil oluşturma hakkında daha fazla bilgi için bkz. [yayımlama profillerini](#publish-profiles) bölümü.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-235">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="cdb4a-236">Bir yayımlama profili kullanarak uygulama dağıtmak için yürütme `msbuild` Visual Studio'dan komutunu **Geliştirici komut istemi**.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-236">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="cdb4a-237">Komut istemi kullanılabilir *Visual Studio* klasörü **Başlat** Windows görev çubuğundaki menü.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-237">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="cdb4a-238">Daha kolay erişim için komut satırına ekleyebilirsiniz **Araçları** Visual Studio'daki menü.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-238">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="cdb4a-239">Daha fazla bilgi için [Visual Studio için geliştirici komut istemi](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-239">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="cdb4a-240">MSBuild, şu komut söz dizimini kullanır:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-240">MSBuild uses the following command syntax:</span></span>

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="cdb4a-241">{PATH} &ndash; Uygulamanın proje dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-241">{PATH} &ndash; Path to the app's project file.</span></span>
* <span data-ttu-id="cdb4a-242">{} PROFİLİ &ndash; Yayımlama profilinin adı.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-242">{PROFILE} &ndash; Name of the publish profile.</span></span>
* <span data-ttu-id="cdb4a-243">{USERNAME} &ndash; MSDeploy kullanıcı adı.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-243">{USERNAME} &ndash; MSDeploy username.</span></span> <span data-ttu-id="cdb4a-244">{USERNAME} yayımlama profilinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-244">The {USERNAME} can be found in the publish profile.</span></span>
* <span data-ttu-id="cdb4a-245">{PASSWORD} &ndash; MSDeploy parola.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-245">{PASSWORD} &ndash; MSDeploy password.</span></span> <span data-ttu-id="cdb4a-246">{PASSWORD} almak *{profili}. PublishSettings* dosya.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-246">Obtain the {PASSWORD} from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="cdb4a-247">İndirme *. PublishSettings* dosyasından ya da:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-247">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="cdb4a-248">Çözüm Gezgini için: Seçin **görünümü** > **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-248">Solution Explorer: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="cdb4a-249">Azure aboneliğinizle bağlanın.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-249">Connect with your Azure subscription.</span></span> <span data-ttu-id="cdb4a-250">Açık **uygulama hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-250">Open **App Services**.</span></span> <span data-ttu-id="cdb4a-251">Uygulamaya sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-251">Right-click the app.</span></span> <span data-ttu-id="cdb4a-252">Seçin **yayımlama profili indir**.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-252">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="cdb4a-253">Azure portalı: Seçin **yayımlama profili Al** web uygulamasının **genel bakış** paneli.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-253">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="cdb4a-254">Aşağıdaki örnekte adlı bir yayımlama profili *AzureWebApp - Web dağıtımı*:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-254">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="cdb4a-255">Bir yayımlama profili, .NET Core CLI ile de kullanılabilir [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) bir Windows komut isteminden komutu:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-255">A publish profile can also be used with the .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command prompt:</span></span>

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> <span data-ttu-id="cdb4a-256">[Dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) komut kullanılabilir platformlar arası ve macOS ve Linux'ta ASP.NET Core uygulamaları derleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-256">The [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command is available cross-platform and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="cdb4a-257">Ancak, MSBuild MacOS ve Linux Azure veya başka bir MSDeploy uç noktası için bir uygulama dağıtmaya yeteneğine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-257">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoint.</span></span> <span data-ttu-id="cdb4a-258">MSDeploy, yalnızca Windows üzerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-258">MSDeploy is only available on Windows.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="cdb4a-259">Ortamı ayarlama</span><span class="sxs-lookup"><span data-stu-id="cdb4a-259">Set the environment</span></span>

<span data-ttu-id="cdb4a-260">Dahil `<EnvironmentName>` yayımlama profilini özelliğinde (*.pubxml*) veya uygulamanın ayarlamak için proje dosyasını [ortam](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="cdb4a-260">Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file to set the app's [environment](xref:fundamentals/environments):</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="cdb4a-261">Gerektiriyorsa *web.config* Dönüşümleri (yapılandırma, profili veya ortama göre örneğin, ortam değişkenlerini ayarlama), bkz: <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-261">If you require *web.config* transformations (for example, setting environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="cdb4a-262">Dosyaları dışarıda bırak</span><span class="sxs-lookup"><span data-stu-id="cdb4a-262">Exclude files</span></span>

<span data-ttu-id="cdb4a-263">ASP.NET Core web uygulamaları, yapılar ve içeriğini yayımlarken *wwwroot* klasörü dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-263">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="cdb4a-264">`msbuild` destekleyen [Glob desenlerinin](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-264">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="cdb4a-265">Örneğin, aşağıdaki `<Content>` öğesi hariç tüm metni (*.txt*) dosyalarını *wwwroot/içerik* klasör ve tüm alt klasörleri.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-265">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="cdb4a-266">Önceki işaretleme için bir yayımlama profili eklenebilir veya *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-266">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="cdb4a-267">Eklenen *.csproj* dosya, kural için eklenir projedeki tüm yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-267">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="cdb4a-268">Aşağıdaki `<MsDeploySkipRules>` öğe tüm dosyaları dışlar *wwwroot/içerik* klasörü:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-268">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="cdb4a-269">`<MsDeploySkipRules>` silmez *atla* dağıtım sitesinden hedefler.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-269">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="cdb4a-270">`<Content>` hedef dosyalar ve klasörler dağıtım site veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-270">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="cdb4a-271">Örneğin, aşağıdaki dosyaları dağıtılan web uygulaması olduğu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-271">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="cdb4a-272">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cdb4a-272">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="cdb4a-273">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cdb4a-273">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="cdb4a-274">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cdb4a-274">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="cdb4a-275">Aşağıdaki `<MsDeploySkipRules>` öğeleri eklenir, dağıtım sitesinde bu dosyaları silseniz mıydı.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-275">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="cdb4a-276">Önceki `<MsDeploySkipRules>` öğeleri önlemek *atlandı* dosyalarının dağıtılıyor.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-276">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="cdb4a-277">Bunlar dağıttıktan sonra bu dosyaları silinmez.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-277">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="cdb4a-278">Aşağıdaki `<Content>` öğesi dağıtım sitede hedeflenen dosyaları siler:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-278">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="cdb4a-279">Önceki komut satırı dağıtımı kullanarak `<Content>` öğesi aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-279">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="cdb4a-280">Dosyaları Ekle</span><span class="sxs-lookup"><span data-stu-id="cdb4a-280">Include files</span></span>

<span data-ttu-id="cdb4a-281">Aşağıdaki biçimlendirme içeren bir *görüntüleri* klasörü için proje dizininin dışına *wwwroot/görüntülerinden* Yayımla sitenin klasörü:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-281">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="cdb4a-282">Biçimlendirme eklenebilir *.csproj* dosya veya yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-282">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="cdb4a-283">Kümeye eklenirse *.csproj* dosyası, onu eklendi projedeki her yayımlama profilinde.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-283">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="cdb4a-284">Aşağıdaki biçimlendirme gösterir nasıl vurgulanmış için:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-284">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="cdb4a-285">Projeye dışında dosyasından kopyalama *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-285">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="cdb4a-286">Dışlama *wwwroot\Content* klasör.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-286">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="cdb4a-287">Dışlama *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-287">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="cdb4a-288">Bkz: [WebSDK Benioku](https://github.com/aspnet/websdk) daha fazla dağıtım örneği için.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-288">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="cdb4a-289">Bir hedef önce veya sonra yayımlama çalıştırın</span><span class="sxs-lookup"><span data-stu-id="cdb4a-289">Run a target before or after publishing</span></span>

<span data-ttu-id="cdb4a-290">Yerleşik `BeforePublish` ve `AfterPublish` hedefleri yürütmenizi hedef önce veya sonra yayımlama hedefi.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-290">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="cdb4a-291">Yayımlama profili öncesinde ve sonrasında yayımlama konsolu iletilerini günlüğe kaydetmek için aşağıdaki öğeleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-291">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="cdb4a-292">Güvenilmeyen bir sertifika kullanarak bir sunucuda yayımlayın</span><span class="sxs-lookup"><span data-stu-id="cdb4a-292">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="cdb4a-293">Ekleme `<AllowUntrustedCertificate>` özellik değeriyle `True` yayımlama profili için:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-293">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="cdb4a-294">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="cdb4a-294">The Kudu service</span></span>

<span data-ttu-id="cdb4a-295">Bir Azure App Service web uygulaması dağıtımı ' dosyaları görüntülemek için kullanın [Kudu hizmet](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="cdb4a-295">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="cdb4a-296">Append `scm` belirteç için web uygulaması adı.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-296">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="cdb4a-297">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cdb4a-297">For example:</span></span>

| <span data-ttu-id="cdb4a-298">URL</span><span class="sxs-lookup"><span data-stu-id="cdb4a-298">URL</span></span>                                    | <span data-ttu-id="cdb4a-299">Sonuç</span><span class="sxs-lookup"><span data-stu-id="cdb4a-299">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="cdb4a-300">Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="cdb4a-300">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="cdb4a-301">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="cdb4a-301">Kudu service</span></span> |

<span data-ttu-id="cdb4a-302">Seçin [hata ayıklama konsolunu](https://github.com/projectkudu/kudu/wiki/Kudu-console) görüntülemek, düzenlemek, silmek veya dosya eklemek için menü öğesi.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-302">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cdb4a-303">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cdb4a-303">Additional resources</span></span>

* <span data-ttu-id="cdb4a-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) web uygulamaları ve IIS sunucuları için Web siteleri dağıtımı (MSDeploy) basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="cdb4a-305">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Dosya sorunları ve istek için dağıtım özellikleri.</span><span class="sxs-lookup"><span data-stu-id="cdb4a-305">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="cdb4a-306">Visual Studio'dan Azure VM için bir ASP.NET Web uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="cdb4a-306">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
