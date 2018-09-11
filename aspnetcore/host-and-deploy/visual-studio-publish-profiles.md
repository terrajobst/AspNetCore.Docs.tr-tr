---
title: Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı
author: rick-anderson
description: Oluşturmayı Visual Studio'da yayımlama profilleri ve ASP.NET Core uygulama dağıtımlarını çeşitli hedeflere yönetmek için kullanın.
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 34223dab0fe14d6e4e7a8383c66f965abf28e15c
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373390"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="ff9e8-103">Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı</span><span class="sxs-lookup"><span data-stu-id="ff9e8-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="ff9e8-104">Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ff9e8-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ff9e8-105">Bu belge oluşturma ve kullanma için Visual Studio 2017'yi kullanmaya odaklanmıştır yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="ff9e8-106">Visual Studio ile oluşturulan yayımlama profillerine MSBuild ve Visual Studio 2017 ' çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="ff9e8-107">Bkz: [Visual Studio kullanarak Azure App Service'e bir ASP.NET Core web uygulaması yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) Azure'da yayımlamak için yönergeler.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="ff9e8-108">Aşağıdaki proje dosyasını komutu ile oluşturulmuş `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ff9e8-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ff9e8-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.1.3" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ff9e8-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ff9e8-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="ff9e8-111">`<Project>` Öğenin `Sdk` özniteliği aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="ff9e8-112">Özellikleri dosyasından içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* başında.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="ff9e8-113">Hedefleri dosyasından içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* sonunda.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="ff9e8-114">Varsayılan konumu `MSBuildSDKsPath` (Visual Studio 2017 Enterprise ile) *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* klasör.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="ff9e8-115">`Microsoft.NET.Sdk.Web` SDK bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="ff9e8-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="ff9e8-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="ff9e8-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="ff9e8-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="ff9e8-118">Neden olan aşağıdaki özellikleri ve içeri aktarılacak hedefler:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="ff9e8-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="ff9e8-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="ff9e8-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="ff9e8-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="ff9e8-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="ff9e8-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="ff9e8-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="ff9e8-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="ff9e8-123">Kullanılan yayımlama yöntemi temel hedefleri sağ kümesini hedefler içe yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="ff9e8-124">MSBuild veya Visual Studio bir projeyi yüklediğinde, aşağıdaki üst düzey eylemler gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="ff9e8-125">Proje derleme</span><span class="sxs-lookup"><span data-stu-id="ff9e8-125">Build project</span></span>
* <span data-ttu-id="ff9e8-126">Yayımlamak için dosyaların işlem</span><span class="sxs-lookup"><span data-stu-id="ff9e8-126">Compute files to publish</span></span>
* <span data-ttu-id="ff9e8-127">Hedef dosya yayımlama</span><span class="sxs-lookup"><span data-stu-id="ff9e8-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="ff9e8-128">Proje öğeleri işlem</span><span class="sxs-lookup"><span data-stu-id="ff9e8-128">Compute project items</span></span>

<span data-ttu-id="ff9e8-129">Proje yüklendiğinde, proje öğeler (dosyalar) hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="ff9e8-130">`item type` Özniteliği dosyanın nasıl işleneceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="ff9e8-131">Varsayılan olarak, *.cs* dosyaları dahil edilecek `Compile` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="ff9e8-132">Dosyalar `Compile` öğe listesi derlenir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="ff9e8-133">`Content` Öğesi listesinin yanı sıra derleme çıktılarını yayımlanan dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="ff9e8-134">Varsayılan olarak, dosyaları desen eşleştirme `wwwroot/**` dahil `Content` öğesi.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="ff9e8-135">`wwwroot/\*\*` [Glob deseni](https://gruntjs.com/configuring-tasks#globbing-patterns) eşleşen tüm dosyaları *wwwroot* klasör **ve** alt.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="ff9e8-136">Açıkça Yayımla listesine bir dosya eklemek için dosyanın doğrudan ekleme *.csproj* gösterildiği gibi dosya [dosyaları içerir](#include-files).</span><span class="sxs-lookup"><span data-stu-id="ff9e8-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="ff9e8-137">Seçerken **Yayımla** düğme Visual Studio'da veya komut satırından yayımlama sırasında:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="ff9e8-138">Özellikler/öğeleri hesaplanır (oluşturmak için gereken dosyaları).</span><span class="sxs-lookup"><span data-stu-id="ff9e8-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="ff9e8-139">**Yalnızca Visual Studio**: NuGet paketlerini geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="ff9e8-140">(Geri yükleme CLI kullanıcı tarafından açık olması gerekir.)</span><span class="sxs-lookup"><span data-stu-id="ff9e8-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="ff9e8-141">Projeyi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-141">The project builds.</span></span>
* <span data-ttu-id="ff9e8-142">Yayımlama öğeleri hesaplanır (yayımlama için gerekli dosyaları).</span><span class="sxs-lookup"><span data-stu-id="ff9e8-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="ff9e8-143">Proje yayımlandığında (hesaplanan dosyalar Yayımla hedefe kopyalanır).</span><span class="sxs-lookup"><span data-stu-id="ff9e8-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="ff9e8-144">Bir ASP.NET Core projesi başvurduğunda `Microsoft.NET.Sdk.Web` proje dosyasında bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="ff9e8-145">Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="ff9e8-146">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="ff9e8-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="ff9e8-147">Temel bir komut satırı yayımlama</span><span class="sxs-lookup"><span data-stu-id="ff9e8-147">Basic command-line publishing</span></span>

<span data-ttu-id="ff9e8-148">Komut satırı yayımlama, .NET Core tarafından desteklenen tüm platformlarda çalışır ve Visual Studio gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="ff9e8-149">Aşağıdaki örnekte [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu proje dizininden çalıştırın (içeren *.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="ff9e8-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="ff9e8-150">Aksi durumda proje klasöründe proje dosya yolu açıkça geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="ff9e8-151">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="ff9e8-152">Oluşturma ve bir web uygulaması yayımlamak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ff9e8-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ff9e8-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ff9e8-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ff9e8-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="ff9e8-155">[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komut aşağıdakine benzer bir çıktı üretir:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="ff9e8-156">Varsayılan publish klasörüne olan `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="ff9e8-157">İçin varsayılan `$(Configuration)` olduğu *hata ayıklama*.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="ff9e8-158">Önceki örnekte, `<TargetFramework>` olduğu `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="ff9e8-159">`dotnet publish -h` bilgi yayımlama için yardımı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="ff9e8-160">Aşağıdaki komut belirtir bir `Release` derleme ve yayımlama dizini:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="ff9e8-161">[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) çağrıları çağıran MSBuild komut `Publish` hedef.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="ff9e8-162">Herhangi bir parametre geçirilen `dotnet publish` MSBuild'e geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="ff9e8-163">`-c` Parametre eşlenen `Configuration` MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="ff9e8-164">`-o` Parametre eşlenen `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="ff9e8-165">MSBuild özellikleri, aşağıdaki biçimlerden birini kullanarak geçirilebilir:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="ff9e8-166">Aşağıdaki komutu yayımlar bir `Release` bir ağ paylaşımına oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="ff9e8-167">Ağ paylaşımı eğik çizgi ile belirtilen (*//r8/*) ve .NET Core desteklenen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="ff9e8-168">Yayımlanan uygulama dağıtımı için çalışmadığından emin onaylayın.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="ff9e8-169">Dosyalar *yayımlama* klasörü, uygulama çalışırken kilitlenir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="ff9e8-170">Dağıtım kilitli olduğundan dosya kopyalanamadı oluşamaz.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="ff9e8-171">Yayımlama profilleri</span><span class="sxs-lookup"><span data-stu-id="ff9e8-171">Publish profiles</span></span>

<span data-ttu-id="ff9e8-172">Bu bölümde, bir yayımlama profili oluşturmak için Visual Studio 2017 kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="ff9e8-173">Visual Studio ya da komut satırından yayımlama oluşturulduktan sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="ff9e8-174">Yayımlama profilleri yayımlama işlemini basitleştirmek ve herhangi bir sayıda profilleri bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="ff9e8-175">Bir yayımlama profili, aşağıdaki yollardan birini seçerek Visual Studio'da oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="ff9e8-176">Çözüm Gezgini'nde projeye sağ tıklayıp **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="ff9e8-177">Seçin **Yayımla &lt;project_name&gt;**  gelen **derleme** menüsü.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="ff9e8-178">**Yayımla** uygulama kapasiteler sayfasının sekmesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="ff9e8-179">Proje bir yayımlama profili sahip değilse, aşağıdaki sayfası görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![Yayımlama sekmesindeki uygulama kapasiteler sayfasının](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="ff9e8-181">Zaman **klasör** olduğu belirlenirse, yayımlanan varlıkları depolamak için bir klasör yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="ff9e8-182">Varsayılan klasör *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="ff9e8-183">Tıklayın **profili oluştur** düğmesini tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="ff9e8-184">Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekmesinde değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="ff9e8-185">Yeni oluşturulan profil aşağı açılan listede görünür.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="ff9e8-186">Tıklayın **yeni profil oluşturma** başka bir yeni profili oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-186">Click **Create new profile** to create another new profile.</span></span>

![Yayımlama sekmesindeki FolderProfile gösteren uygulama kapasiteler sayfasının](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="ff9e8-188">Yayımlama Sihirbazı'nı aşağıdaki Yayımla hedeflerini destekler:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="ff9e8-189">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="ff9e8-189">Azure App Service</span></span>
* <span data-ttu-id="ff9e8-190">Azure sanal makineleri</span><span class="sxs-lookup"><span data-stu-id="ff9e8-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="ff9e8-191">IIS, FTP, vb. (için herhangi bir web sunucusu)</span><span class="sxs-lookup"><span data-stu-id="ff9e8-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="ff9e8-192">Klasör</span><span class="sxs-lookup"><span data-stu-id="ff9e8-192">Folder</span></span>
* <span data-ttu-id="ff9e8-193">Profili içeri aktar</span><span class="sxs-lookup"><span data-stu-id="ff9e8-193">Import Profile</span></span>

<span data-ttu-id="ff9e8-194">Daha fazla bilgi için [hangi Yayımlama seçenekleri benim için en uygun](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="ff9e8-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="ff9e8-195">Visual Studio ile bir yayımlama profili oluştururken, bir *özellikleri/PublishProfiles/&lt;Profil_adı&gt;.pubxml* MSBuild dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="ff9e8-196">*.Pubxml* dosyanın MSBuild dosyası ve içeren yapılandırma ayarlarını yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="ff9e8-197">Bu dosya, yapı özelleştirme ve yayımlama işlemi için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="ff9e8-198">Bu dosya yayımlama işlemi tarafından okunur.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-198">This file is read by the publishing process.</span></span> <span data-ttu-id="ff9e8-199">`<LastUsedBuildConfiguration>` Genel bir özellik olduğundan ve yapı alınan herhangi bir dosya olmamalıdır özeldir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="ff9e8-200">Bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="ff9e8-201">Azure bir hedefe, yayımlama sırasında *.pubxml* dosyası, Azure abonelik tanımlayıcısı içeriyor.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="ff9e8-202">Bu dosya kaynak denetimine eklemeye, hedef türüyle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="ff9e8-203">Azure dışı hedef yayımlama sırasında iade etmeye güvenli *.pubxml* dosya.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="ff9e8-204">(Yayımlama parola gibi) hassas bilgiler şifreli bir kullanıcı/makine düzeyinde başına.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="ff9e8-205">İçinde depolanan *özellikleri/PublishProfiles/&lt;Profil_adı&gt;. pubxml.user* dosya.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="ff9e8-206">Bu dosya, hassas bilgileri depolayabileceğiniz için kaynak denetimine iade olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="ff9e8-207">Nasıl bir ASP.NET Core web uygulaması yayımlamak genel bakış için bkz. [konak dağıtıp](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="ff9e8-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="ff9e8-208">MSBuild görevleri ve hedefleri ASP.NET Core uygulaması yayımlamak için gerekli açık kaynaklı, https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="ff9e8-209">`dotnet publish` MSDeploy, klasörü kullanabilirsiniz ve [Kudu](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="ff9e8-210">(Çalışan platformlar arası) klasörü:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="ff9e8-211">MSDeploy (şu anda bu tek çalışır platformlar arası MSDeploy olmadığından bu yana Windows):</span><span class="sxs-lookup"><span data-stu-id="ff9e8-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="ff9e8-212">MSDeploy paket (şu anda bu tek çalışır platformlar arası MSDeploy olmadığından bu yana Windows):</span><span class="sxs-lookup"><span data-stu-id="ff9e8-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="ff9e8-213">Önceki örneklerdeki **yoksa** geçirmek `deployonbuild` için `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="ff9e8-214">Daha fazla bilgi için [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="ff9e8-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="ff9e8-215">`dotnet publish` her platformda dizinden Azure'da yayımlamak için Kudu API'leri destekler.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="ff9e8-216">Visual Studio yayımlama Kudu API'leri, ancak desteklenen WebSDK ile platformlar arası yayımlamak için Azure'a destekler.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="ff9e8-217">Eklemek için bir yayımlama profili *özellikleri/PublishProfiles* klasöründe aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="ff9e8-218">Yayımla içerikleri zip ve Kudu API'lerini kullanarak Azure'da yayımlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="ff9e8-219">Bir yayımlama profili kullanırken aşağıdaki MSBuild özellikleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="ff9e8-220">Adlı bir profille yayımlarken *FolderProfile*, aşağıdaki komutlardan birini yürütülebilir:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="ff9e8-221">Çağrılırken [dotnet derleme](/dotnet/core/tools/dotnet-build), çağrı `msbuild` yapıyı çalıştırmak ve işlem yayınlamak için.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="ff9e8-222">Ya da çağırma `dotnet build` veya `msbuild` klasör profilinde geçerken eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="ff9e8-223">MSBuild doğrudan Windows üzerinde çağrıldığında, MSBuild .NET Framework sürümü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="ff9e8-224">MSDeploy yayımlama için Windows makineleri için şu anda sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="ff9e8-225">Çağırma `dotnet build` klasördeki olmayan profili çağırır MSBuild ve MSBuild olmayan klasör profillerde MSDeploy kullanır.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="ff9e8-226">Çağırma `dotnet build` klasörü olmayan profilinde (MSDeploy kullanarak) MSBuild çağırır ve (hatta Windows platformunda çalışırken) içinde bir hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="ff9e8-227">Bir klasörü olmayan profili ile yayımlamak için MSBuild doğrudan çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="ff9e8-228">Aşağıdaki klasörü yayımlama profili Visual Studio ile oluşturulmuş ve bir ağ paylaşımına yayımlar:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="ff9e8-229">Not `<LastUsedBuildConfiguration>` ayarlanır `Release`.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="ff9e8-230">Visual Studio'dan yayımlama sırasında `<LastUsedBuildConfiguration>` yapılandırma özellik değeri, yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="ff9e8-231">`<LastUsedBuildConfiguration>` Yapılandırma özelliği özeldir ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınan olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="ff9e8-232">Bu özellik komut satırından geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="ff9e8-233">.NET Core CLI kullanarak:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="ff9e8-234">MSBuild kullanarak:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="ff9e8-235">Komut satırından bir MSDeploy uç noktasına yayımlama</span><span class="sxs-lookup"><span data-stu-id="ff9e8-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="ff9e8-236">Yayımlama, .NET Core CLI veya MSBuild'ı kullanarak gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="ff9e8-237">`dotnet publish` .NET Core bağlamında çalışır.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="ff9e8-238">`msbuild` Komutu Windows ortamları için sınırlar .NET Framework gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="ff9e8-239">MSDeploy yayımlama için en kolay yolu, ilk Visual Studio 2017'de bir yayımlama profili oluşturun ve komut satırından profil sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="ff9e8-240">Aşağıdaki örnekte, bir ASP.NET Core web uygulaması oluşturulur (kullanarak `dotnet new mvc`), ve bir Azure yayımlama profili, Visual Studio ile eklenir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="ff9e8-241">Çalıştırma `msbuild` gelen bir **VS 2017 için geliştirici komut istemi**.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="ff9e8-242">Geliştirici komut istemi doğru sahip *msbuild.exe* yolundaki bazı MSBuild değişkenleri kümesi.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="ff9e8-243">MSBuild aşağıdaki sözdizimini kullanır:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="ff9e8-244">Alma `Password` gelen  *\<Yayımla adı >. PublishSettings* dosya.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="ff9e8-245">İndirme *. PublishSettings* dosyasından ya da:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="ff9e8-246">Çözüm Gezgini: Web uygulaması üzerinde sağ tıklayın ve **yayımlama profili indirme**.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="ff9e8-247">Azure portalı: tıklayın **yayımlama profili Al** Web uygulamasının üzerinde **genel bakış** paneli.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="ff9e8-248">`Username` Yayımlama profilinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="ff9e8-249">Aşağıdaki örnek kullanımları *Web11112 - Web dağıtımı* yayımlama profili:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="ff9e8-250">Dosyaları dışarıda bırak</span><span class="sxs-lookup"><span data-stu-id="ff9e8-250">Exclude files</span></span>

<span data-ttu-id="ff9e8-251">ASP.NET Core web uygulamaları, yapılar ve içeriğini yayımlarken *wwwroot* klasörü dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="ff9e8-252">`msbuild` destekleyen [Glob desenlerinin](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="ff9e8-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="ff9e8-253">Örneğin, aşağıdaki `<Content>` öğesi hariç tüm metni (*.txt*) dosyalarını *wwwroot/içerik* klasör ve tüm alt klasörleri.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="ff9e8-254">Önceki işaretleme için bir yayımlama profili eklenebilir veya *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="ff9e8-255">Eklenen *.csproj* dosya, kural için eklenir projedeki tüm yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="ff9e8-256">Aşağıdaki `<MsDeploySkipRules>` öğe tüm dosyaları dışlar *wwwroot/içerik* klasörü:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="ff9e8-257">`<MsDeploySkipRules>` silmez *atla* dağıtım sitesinden hedefler.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="ff9e8-258">`<Content>` hedef dosyalar ve klasörler dağıtım site veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="ff9e8-259">Örneğin, aşağıdaki dosyaları dağıtılan web uygulaması olduğu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="ff9e8-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ff9e8-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="ff9e8-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ff9e8-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="ff9e8-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ff9e8-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="ff9e8-263">Aşağıdaki `<MsDeploySkipRules>` öğeleri eklenir, dağıtım sitesinde bu dosyaları silseniz mıydı.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="ff9e8-264">Önceki `<MsDeploySkipRules>` öğeleri önlemek *atlandı* dosyalarının dağıtılıyor.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="ff9e8-265">Bunlar dağıttıktan sonra bu dosyaları silinmez.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="ff9e8-266">Aşağıdaki `<Content>` öğesi dağıtım sitede hedeflenen dosyaları siler:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="ff9e8-267">Önceki komut satırı dağıtımı kullanarak `<Content>` öğesi aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="ff9e8-268">Dosyaları Ekle</span><span class="sxs-lookup"><span data-stu-id="ff9e8-268">Include files</span></span>

<span data-ttu-id="ff9e8-269">Aşağıdaki biçimlendirme içeren bir *görüntüleri* klasörü için proje dizininin dışına *wwwroot/görüntülerinden* Yayımla sitenin klasörü:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="ff9e8-270">Biçimlendirme eklenebilir *.csproj* dosya veya yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="ff9e8-271">Kümeye eklenirse *.csproj* dosyası, onu eklendi projedeki her yayımlama profilinde.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="ff9e8-272">Aşağıdaki biçimlendirme gösterir nasıl vurgulanmış için:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="ff9e8-273">Projeye dışında dosyasından kopyalama *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="ff9e8-274">Dışlama *wwwroot\Content* klasör.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="ff9e8-275">Dışlama *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-275">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="ff9e8-276">Bkz: [WebSDK Benioku](https://github.com/aspnet/websdk) daha fazla dağıtım örneği için.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="ff9e8-277">Bir hedef önce veya sonra yayımlama çalıştırın</span><span class="sxs-lookup"><span data-stu-id="ff9e8-277">Run a target before or after publishing</span></span>

<span data-ttu-id="ff9e8-278">Yerleşik `BeforePublish` ve `AfterPublish` hedefleri yürütmenizi hedef önce veya sonra yayımlama hedefi.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="ff9e8-279">Yayımlama profili öncesinde ve sonrasında yayımlama konsolu iletilerini günlüğe kaydetmek için aşağıdaki öğeleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="ff9e8-280">Güvenilmeyen bir sertifika kullanarak bir sunucuda yayımlayın</span><span class="sxs-lookup"><span data-stu-id="ff9e8-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="ff9e8-281">Ekleme `<AllowUntrustedCertificate>` özellik değeriyle `True` yayımlama profili için:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="ff9e8-282">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="ff9e8-282">The Kudu service</span></span>

<span data-ttu-id="ff9e8-283">Bir Azure App Service web uygulaması dağıtımı ' dosyaları görüntülemek için kullanın [Kudu hizmet](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="ff9e8-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="ff9e8-284">Append `scm` belirteç için web uygulaması adı.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="ff9e8-285">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ff9e8-285">For example:</span></span>

| <span data-ttu-id="ff9e8-286">URL</span><span class="sxs-lookup"><span data-stu-id="ff9e8-286">URL</span></span>                                    | <span data-ttu-id="ff9e8-287">Sonuç</span><span class="sxs-lookup"><span data-stu-id="ff9e8-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="ff9e8-288">Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="ff9e8-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="ff9e8-289">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="ff9e8-289">Kudu service</span></span> |

<span data-ttu-id="ff9e8-290">Seçin [hata ayıklama konsolunu](https://github.com/projectkudu/kudu/wiki/Kudu-console) görüntülemek, düzenlemek, silmek veya dosya eklemek için menü öğesi.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff9e8-291">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ff9e8-291">Additional resources</span></span>

* <span data-ttu-id="ff9e8-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) web uygulamaları ve IIS sunucuları için Web siteleri dağıtımı (MSDeploy) basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="ff9e8-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Dosya sorunları ve istek için dağıtım özellikleri.</span><span class="sxs-lookup"><span data-stu-id="ff9e8-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="ff9e8-294">Visual Studio'dan Azure VM için bir ASP.NET Web uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="ff9e8-294">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
