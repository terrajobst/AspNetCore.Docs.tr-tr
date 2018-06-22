---
title: Visual Studio yayımlama profilleri ASP.NET Core uygulama dağıtımı için
author: rick-anderson
description: Oluşturmayı öğrenin Visual Studio'da yayımlama profillerini ve ASP.NET Core uygulama dağıtımlarını çeşitli hedeflere yönetmek için kullanın.
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 280599ab4b4f0a70d154cc4408e7232aaf766d8e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279564"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="1df8a-103">Visual Studio yayımlama profilleri ASP.NET Core uygulama dağıtımı için</span><span class="sxs-lookup"><span data-stu-id="1df8a-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="1df8a-104">Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1df8a-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1df8a-105">Bu belgeyi oluşturmak ve kullanmak için Visual Studio 2017 kullanmaya odaklanır yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="1df8a-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="1df8a-106">Visual Studio ile oluşturulan yayımlama profilleri MSBuild ve Visual Studio 2017 çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1df8a-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="1df8a-107">Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) yayımlama için yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="1df8a-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="1df8a-108">Aşağıdaki proje dosyası komutuyla oluşturulan `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="1df8a-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1df8a-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1df8a-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1df8a-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1df8a-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="1df8a-111">`<Project>` Öğenin `Sdk` özniteliği aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="1df8a-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="1df8a-112">Özellikler dosyasından içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* başında.</span><span class="sxs-lookup"><span data-stu-id="1df8a-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="1df8a-113">Hedef dosyadan içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* sonunda.</span><span class="sxs-lookup"><span data-stu-id="1df8a-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="1df8a-114">Varsayılan konumu `MSBuildSDKsPath` (Visual Studio 2017 kuruluş ile) *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* klasör.</span><span class="sxs-lookup"><span data-stu-id="1df8a-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="1df8a-115">`Microsoft.NET.Sdk.Web` SDK bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="1df8a-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="1df8a-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="1df8a-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="1df8a-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="1df8a-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="1df8a-118">Aşağıdaki özellikler ve içeri aktarılacak hedefleri neden olur:</span><span class="sxs-lookup"><span data-stu-id="1df8a-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="1df8a-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="1df8a-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="1df8a-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="1df8a-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="1df8a-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="1df8a-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="1df8a-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="1df8a-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="1df8a-123">Kullanılan Yayımla yöntemine göre hedefler hakkını ayarlayın hedefleri alma yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="1df8a-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="1df8a-124">MSBuild veya Visual Studio Proje yüklediğinde, aşağıdaki üst düzey eylemler gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="1df8a-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="1df8a-125">Proje derleme</span><span class="sxs-lookup"><span data-stu-id="1df8a-125">Build project</span></span>
* <span data-ttu-id="1df8a-126">Yayımlanacak dosyaları işlem</span><span class="sxs-lookup"><span data-stu-id="1df8a-126">Compute files to publish</span></span>
* <span data-ttu-id="1df8a-127">Dosyaları hedefe yayımlama</span><span class="sxs-lookup"><span data-stu-id="1df8a-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="1df8a-128">Proje öğeleri işlem</span><span class="sxs-lookup"><span data-stu-id="1df8a-128">Compute project items</span></span>

<span data-ttu-id="1df8a-129">Proje yüklendiğinde, proje öğeleri (dosyaları) hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="1df8a-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="1df8a-130">`item type` Özniteliği dosya nasıl işleneceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="1df8a-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="1df8a-131">Varsayılan olarak, *.cs* dosyaları dahil edilmiştir `Compile` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="1df8a-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="1df8a-132">Dosyalar `Compile` öğe listesi derlenir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="1df8a-133">`Content` Öğesi listesinin yanı sıra yapı çıkışları yayımlanan dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="1df8a-134">Varsayılan olarak, dosyaları desen eşleştirme `wwwroot/**` içinde yer alan `Content` öğesi.</span><span class="sxs-lookup"><span data-stu-id="1df8a-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="1df8a-135">`wwwroot/\*\*` [Genelleme düzeni](https://gruntjs.com/configuring-tasks#globbing-patterns) tüm dosyalarda eşleşen *wwwroot* klasörü **ve** alt klasörler.</span><span class="sxs-lookup"><span data-stu-id="1df8a-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="1df8a-136">Açıkça Yayımla listesine bir dosya eklemek için doğrudan dosyasına ekleyin *.csproj* dosya gösterildiği gibi [dosyaları içerir](#include-files).</span><span class="sxs-lookup"><span data-stu-id="1df8a-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="1df8a-137">Seçerken **Yayımla** düğmesi Visual Studio'da veya komut satırından yayımlama sırasında:</span><span class="sxs-lookup"><span data-stu-id="1df8a-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="1df8a-138">Özellikler/öğeleri hesaplanır (oluşturmak için gereken dosyaları).</span><span class="sxs-lookup"><span data-stu-id="1df8a-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="1df8a-139">**Yalnızca Visual Studio**: NuGet paketleri geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="1df8a-140">(Geri yükleme CLI kullanıcı tarafından açık olması gerekir.)</span><span class="sxs-lookup"><span data-stu-id="1df8a-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="1df8a-141">Proje oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1df8a-141">The project builds.</span></span>
* <span data-ttu-id="1df8a-142">Yayımla öğeleri hesaplanır (yayımlama için gerekli olan dosyalar).</span><span class="sxs-lookup"><span data-stu-id="1df8a-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="1df8a-143">Proje yayımlanan (hesaplanan dosyalar Yayımla hedefe kopyalanır).</span><span class="sxs-lookup"><span data-stu-id="1df8a-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="1df8a-144">ASP.NET Core projesinde başvurduğunda `Microsoft.NET.Sdk.Web` proje dosyasında bir *app_offline.htm* dosyası, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="1df8a-145">Dosyanın mevcut olduğunda, ASP.NET Core modülü düzgün biçimde uygulamasını kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="1df8a-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="1df8a-146">Daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="1df8a-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="1df8a-147">Temel komut satırı yayımlama</span><span class="sxs-lookup"><span data-stu-id="1df8a-147">Basic command-line publishing</span></span>

<span data-ttu-id="1df8a-148">Komut satırı yayımlama tüm .NET Core desteklenen platformlarda çalışır ve Visual Studio gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="1df8a-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="1df8a-149">Aşağıdaki örnekte [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu proje dizinden çalıştırın (içeren *.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="1df8a-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="1df8a-150">Aksi durumda proje klasöründe açıkça proje dosya yolunda geçirin.</span><span class="sxs-lookup"><span data-stu-id="1df8a-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="1df8a-151">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1df8a-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="1df8a-152">Oluşturma ve bir web uygulaması yayımlama için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1df8a-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1df8a-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1df8a-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1df8a-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1df8a-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="1df8a-155">[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu, aşağıdakine benzer bir çıktı üretir:</span><span class="sxs-lookup"><span data-stu-id="1df8a-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="1df8a-156">Varsayılan klasör yayımlama olduğu `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="1df8a-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="1df8a-157">İçin varsayılan `$(Configuration)` olan *hata ayıklama*.</span><span class="sxs-lookup"><span data-stu-id="1df8a-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="1df8a-158">Önceki örnekte, `<TargetFramework>` olan `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="1df8a-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="1df8a-159">`dotnet publish -h` bilgi yayımlama için yardımı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1df8a-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="1df8a-160">Aşağıdaki komut belirtir bir `Release` oluşturma ve yayımlama dizini:</span><span class="sxs-lookup"><span data-stu-id="1df8a-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="1df8a-161">[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) çağrıları çağırır MSBuild komut `Publish` hedef.</span><span class="sxs-lookup"><span data-stu-id="1df8a-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="1df8a-162">Herhangi bir parametre geçirilen `dotnet publish` MSBuild için geçirilir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="1df8a-163">`-c` Parametresi eşlendiğini `Configuration` MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="1df8a-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="1df8a-164">`-o` Parametresi eşlendiğini `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="1df8a-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="1df8a-165">MSBuild özellikleri aşağıdaki biçimlerden birini kullanarak geçirilebilir:</span><span class="sxs-lookup"><span data-stu-id="1df8a-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="1df8a-166">Aşağıdaki komutu yayımlayan bir `Release` bir ağ paylaşımına oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1df8a-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="1df8a-167">Ağ paylaşımı eğik çizgi ile belirtilir (*//r8/*) ve tüm desteklenen .NET Core platformlarında çalışır.</span><span class="sxs-lookup"><span data-stu-id="1df8a-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="1df8a-168">Yayımlanan uygulama dağıtımı için çalışmayan onaylayın.</span><span class="sxs-lookup"><span data-stu-id="1df8a-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="1df8a-169">Dosyalar *yayımlama* uygulama çalışırken klasörü kilitlenir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="1df8a-170">Dağıtım kilitli olduğundan dosyaları kopyalanamaz gerçekleşemez.</span><span class="sxs-lookup"><span data-stu-id="1df8a-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="1df8a-171">Yayımlama profilleri</span><span class="sxs-lookup"><span data-stu-id="1df8a-171">Publish profiles</span></span>

<span data-ttu-id="1df8a-172">Bu bölümde, bir yayımlama profili oluşturmak için Visual Studio 2017 kullanır.</span><span class="sxs-lookup"><span data-stu-id="1df8a-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="1df8a-173">Visual Studio veya komut satırı yayımlama oluşturulduktan sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="1df8a-174">Yayımlama profilleri yayımlama işlemini basitleştirmek ve herhangi bir sayıda profilleri bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="1df8a-175">Bir yayımlama profili Visual Studio aşağıdaki yollardan birini seçerek oluşturun:</span><span class="sxs-lookup"><span data-stu-id="1df8a-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="1df8a-176">Çözüm Gezgini'nde projeye sağ tıklayıp **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="1df8a-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="1df8a-177">Seçin **Yayımla &lt;project_name&gt;**  gelen **yapı** menüsü.</span><span class="sxs-lookup"><span data-stu-id="1df8a-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="1df8a-178">**Yayımla** uygulama kapasiteleri sayfasında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="1df8a-179">Bir yayımlama profili proje sahip değilse, aşağıdaki sayfayı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="1df8a-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![Uygulama kapasiteleri sayfasında Yayımla sekmesi](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="1df8a-181">Zaman **klasörü** olduğunu belirlenirse, yayımlanan varlıkları depolamak için bir klasör yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="1df8a-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="1df8a-182">Varsayılan klasör *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="1df8a-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="1df8a-183">Tıklatın **profili oluştur** tamamlanması düğmesi.</span><span class="sxs-lookup"><span data-stu-id="1df8a-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="1df8a-184">Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekmesinde değişiklikleri.</span><span class="sxs-lookup"><span data-stu-id="1df8a-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="1df8a-185">Yeni oluşturulan profil açılır listede görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="1df8a-186">Tıklatın **yeni profil oluşturmak** başka bir yeni profili oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="1df8a-186">Click **Create new profile** to create another new profile.</span></span>

![FolderProfile gösteren uygulama kapasiteleri sayfasına Yayımla sekmesi](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="1df8a-188">Yayımlama Sihirbazı'nı aşağıdaki Yayımla hedefleri destekler:</span><span class="sxs-lookup"><span data-stu-id="1df8a-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="1df8a-189">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="1df8a-189">Azure App Service</span></span>
* <span data-ttu-id="1df8a-190">Azure sanal makineler</span><span class="sxs-lookup"><span data-stu-id="1df8a-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="1df8a-191">IIS, FTP, vb. (için herhangi bir web sunucusu)</span><span class="sxs-lookup"><span data-stu-id="1df8a-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="1df8a-192">Klasör</span><span class="sxs-lookup"><span data-stu-id="1df8a-192">Folder</span></span>
* <span data-ttu-id="1df8a-193">Profilini içeri aktarma</span><span class="sxs-lookup"><span data-stu-id="1df8a-193">Import Profile</span></span>

<span data-ttu-id="1df8a-194">Daha fazla bilgi için bkz: [hangi yayımlama seçeneklerini benim için en uygun](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="1df8a-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="1df8a-195">Bir yayımlama profili Visual Studio ile oluştururken bir *özellikleri/PublishProfiles/&lt;Profil_adı&gt;.pubxml* MSBuild dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1df8a-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="1df8a-196">*.Pubxml* dosyası MSBuild dosyadır ve içeren yapılandırma ayarlarını yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="1df8a-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="1df8a-197">Bu dosya, yapı özelleştirmek ve işlem yayımlamak için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="1df8a-198">Bu dosya yayımlama işlemi tarafından okunur.</span><span class="sxs-lookup"><span data-stu-id="1df8a-198">This file is read by the publishing process.</span></span> <span data-ttu-id="1df8a-199">`<LastUsedBuildConfiguration>` Genel bir özellik olduğundan ve yapı alınan herhangi bir dosya olmamalıdır özeldir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="1df8a-200">Bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1df8a-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="1df8a-201">Bir Azure hedefine yayımlarken *.pubxml* dosyası, Azure abonelik tanımlayıcısı içeriyor.</span><span class="sxs-lookup"><span data-stu-id="1df8a-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="1df8a-202">Bu hedef türü ile bu dosya kaynak denetimine ekleme önerilmez.</span><span class="sxs-lookup"><span data-stu-id="1df8a-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="1df8a-203">Bir Azure olmayan hedefine yayımlarken iade güvenli *.pubxml* dosya.</span><span class="sxs-lookup"><span data-stu-id="1df8a-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="1df8a-204">(Yayımla parola gibi) hassas bilgileri şifrelenir bir kullanıcı/makine gerçekleştiriliyordu.</span><span class="sxs-lookup"><span data-stu-id="1df8a-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="1df8a-205">İçinde depolanan *özellikleri/PublishProfiles/&lt;Profil_adı&gt;. pubxml.user* dosya.</span><span class="sxs-lookup"><span data-stu-id="1df8a-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="1df8a-206">Bu dosya hassas bilgileri depolayabileceğiniz için kaynak denetimine iade döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="1df8a-207">ASP.NET Core üzerinde bir web uygulaması yayımlama genel bakış için bkz: [konak dağıtıp](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="1df8a-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="1df8a-208">MSBuild görevleri ve ASP.NET Core uygulama yayımlamak için gerekli hedefleri açık kaynak adresindeki https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="1df8a-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="1df8a-209">`dotnet publish` klasörü, MSDeploy, kullanabilirsiniz ve [Kudu](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri:</span><span class="sxs-lookup"><span data-stu-id="1df8a-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="1df8a-210">Klasör (platformlar arası çalışır):</span><span class="sxs-lookup"><span data-stu-id="1df8a-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="1df8a-211">MSDeploy (şu anda bu yalnızca çalışır MSDeploy platformlar arası değil bu yana Windows'ta):</span><span class="sxs-lookup"><span data-stu-id="1df8a-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="1df8a-212">MSDeploy paketi (şu anda bu yalnızca çalışır MSDeploy platformlar arası değil bu yana Windows'ta):</span><span class="sxs-lookup"><span data-stu-id="1df8a-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="1df8a-213">Önceki örnekte **yok** geçirmek `deployonbuild` için `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="1df8a-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="1df8a-214">Daha fazla bilgi için bkz: [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="1df8a-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="1df8a-215">`dotnet publish` Kudu herhangi bir platform için Azure yayımlama için API'ler destekler.</span><span class="sxs-lookup"><span data-stu-id="1df8a-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="1df8a-216">Visual Studio yayımlama Kudu API'ları, ancak desteklenen tarafından WebSDK platformlar arası yayımlama için Azure'a destekler.</span><span class="sxs-lookup"><span data-stu-id="1df8a-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="1df8a-217">Bir yayımlama profili Ekle *özellikleri/PublishProfiles* klasöründe aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="1df8a-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="1df8a-218">Yayımla içerikleri zip ve Kudu API'lerini kullanarak Azure yayımlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1df8a-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="1df8a-219">Bir yayımlama profili kullanırken aşağıdaki MSBuild özellikleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="1df8a-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="1df8a-220">Adlı bir profille yayımlarken *FolderProfile*, aşağıdaki komutlardan birini çalıştırılabilir:</span><span class="sxs-lookup"><span data-stu-id="1df8a-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="1df8a-221">Çağrılırken, [dotnet yapı](/dotnet/core/tools/dotnet-build), çağırır `msbuild` yapı çalıştırın ve işlem yayınlamak için.</span><span class="sxs-lookup"><span data-stu-id="1df8a-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="1df8a-222">Ya da çağırma `dotnet build` veya `msbuild` bir klasör profilinde geçirilirken eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="1df8a-223">MSBuild doğrudan Windows çağrılırken MSBuild .NET Framework sürümü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1df8a-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="1df8a-224">MSDeploy, Windows makineler yayımlama için şu anda sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="1df8a-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="1df8a-225">Çağırma `dotnet build` klasörde olmayan profili MSBuild çağırır ve MSBuild klasörü olmayan profilleri MSDeploy kullanır.</span><span class="sxs-lookup"><span data-stu-id="1df8a-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="1df8a-226">Çağırma `dotnet build` bir klasör olmayan profilindeki (MSDeploy kullanarak) MSBuild çağırır ve (bile Windows platformu üzerinde çalışırken) bir hatayla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="1df8a-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="1df8a-227">Bir klasör olmayan profili ile yayımlamak için MSBuild doğrudan çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="1df8a-228">Aşağıdaki klasörü yayımlama profili Visual Studio ile oluşturulmuş ve bir ağ paylaşımına yayımlar:</span><span class="sxs-lookup"><span data-stu-id="1df8a-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="1df8a-229">Not `<LastUsedBuildConfiguration>` ayarlanır `Release`.</span><span class="sxs-lookup"><span data-stu-id="1df8a-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="1df8a-230">Visual Studio'dan yayımlarken `<LastUsedBuildConfiguration>` yapılandırma özellik değeri, yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="1df8a-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="1df8a-231">`<LastUsedBuildConfiguration>` Yapılandırma özelliği özeldir ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınmış döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="1df8a-232">Bu özellik, komut satırında geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="1df8a-233">.NET Core CLI kullanarak:</span><span class="sxs-lookup"><span data-stu-id="1df8a-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="1df8a-234">MSBuild kullanma:</span><span class="sxs-lookup"><span data-stu-id="1df8a-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="1df8a-235">MSDeploy uç noktasına komut satırından yayımlama</span><span class="sxs-lookup"><span data-stu-id="1df8a-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="1df8a-236">Yayımlama .NET Core CLI veya MSBuild kullanılarak gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="1df8a-237">`dotnet publish` .NET Core bağlamında çalışır.</span><span class="sxs-lookup"><span data-stu-id="1df8a-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="1df8a-238">`msbuild` Komutu Windows ortamları için sınırlar .NET Framework gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="1df8a-239">En kolay yolu MSDeploy ile yayımlamak için önce bir yayımlama profili Visual Studio 2017 içinde oluşturmak ve komut satırından profil kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="1df8a-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="1df8a-240">Aşağıdaki örnekte, bir ASP.NET Core web uygulaması oluşturuldu (kullanarak `dotnet new mvc`), ve bir Azure yayımlama profili Visual Studio ile eklenir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="1df8a-241">Çalıştırma `msbuild` gelen bir **VS 2017 için geliştirici komut istemi**.</span><span class="sxs-lookup"><span data-stu-id="1df8a-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="1df8a-242">Geliştirici komut istemi doğru sahip *msbuild.exe* yolundaki bazı MSBuild değişkenler kümesine sahip.</span><span class="sxs-lookup"><span data-stu-id="1df8a-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="1df8a-243">MSBuild aşağıdaki söz dizimini kullanır:</span><span class="sxs-lookup"><span data-stu-id="1df8a-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="1df8a-244">Alma `Password` gelen  *\<Yayımla adı >. PublishSettings* dosya.</span><span class="sxs-lookup"><span data-stu-id="1df8a-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="1df8a-245">Karşıdan *. PublishSettings* ya da dosyasından:</span><span class="sxs-lookup"><span data-stu-id="1df8a-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="1df8a-246">Çözüm Gezgini: Web uygulamasında sağ tıklatın ve seçin **yayımlama profili indirme**.</span><span class="sxs-lookup"><span data-stu-id="1df8a-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="1df8a-247">Azure portal: tıklatın **Get yayımlama profili** Web uygulamanızın üzerinde **genel bakış** paneli.</span><span class="sxs-lookup"><span data-stu-id="1df8a-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="1df8a-248">`Username` Yayımlama profili bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="1df8a-249">Aşağıdaki örnek kullanır *Web11112 - Web dağıtımı* yayımlama profili:</span><span class="sxs-lookup"><span data-stu-id="1df8a-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="1df8a-250">Dosyaları dışarıda bırak</span><span class="sxs-lookup"><span data-stu-id="1df8a-250">Exclude files</span></span>

<span data-ttu-id="1df8a-251">ASP.NET Core web uygulamaları, derleme yapıları ve içeriğini yayımlarken *wwwroot* klasörü dahil.</span><span class="sxs-lookup"><span data-stu-id="1df8a-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="1df8a-252">`msbuild` destekleyen [genelleme desenleri](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="1df8a-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="1df8a-253">Örneğin, aşağıdaki `<Content>` öğesi hariç tüm metni (*.txt*) dosyaları buradan *wwwroot/içerik* klasör ve tüm alt klasörlerini.</span><span class="sxs-lookup"><span data-stu-id="1df8a-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="1df8a-254">Önceki biçimlendirme bir yayımlama profili eklenebilir veya *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="1df8a-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="1df8a-255">Eklendiğinde *.csproj* dosya, kural eklenmiş olup projedeki tüm yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="1df8a-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="1df8a-256">Aşağıdaki `<MsDeploySkipRules>` öğesi hariç tüm dosyaları *wwwroot/içerik* klasörü:</span><span class="sxs-lookup"><span data-stu-id="1df8a-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="1df8a-257">`<MsDeploySkipRules>` silmez *atla* dağıtım sitesinden hedefler.</span><span class="sxs-lookup"><span data-stu-id="1df8a-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="1df8a-258">`<Content>` hedef dosyalar ve klasörler dağıtım site veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="1df8a-259">Örneğin, aşağıdaki dosyaları dağıtılan web uygulamasının vardı varsayın:</span><span class="sxs-lookup"><span data-stu-id="1df8a-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="1df8a-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1df8a-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="1df8a-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1df8a-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="1df8a-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1df8a-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="1df8a-263">Aşağıdaki `<MsDeploySkipRules>` öğeleri eklendiğinde, bu dosyaları dağıtım sitesinde silinmiş olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="1df8a-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="1df8a-264">Yukarıdaki `<MsDeploySkipRules>` öğeleri engellemek *atlandı* dağıtılan dosyaları.</span><span class="sxs-lookup"><span data-stu-id="1df8a-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="1df8a-265">Dağıtmış sonra bu dosyaları silmez.</span><span class="sxs-lookup"><span data-stu-id="1df8a-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="1df8a-266">Aşağıdaki `<Content>` öğesi dağıtım sitede hedeflenen dosya siler:</span><span class="sxs-lookup"><span data-stu-id="1df8a-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="1df8a-267">Önceki komut satırı dağıtım kullanarak `<Content>` öğesi şu çıkışı üretir:</span><span class="sxs-lookup"><span data-stu-id="1df8a-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="1df8a-268">Dosyaları dahil etme</span><span class="sxs-lookup"><span data-stu-id="1df8a-268">Include files</span></span>

<span data-ttu-id="1df8a-269">Aşağıdaki biçimlendirme içeren bir *görüntüleri* klasörünün dışında proje dizinine *wwwroot/görüntüleri* Yayımla sitesinin klasörüne:</span><span class="sxs-lookup"><span data-stu-id="1df8a-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="1df8a-270">İşaretleme eklenebilir *.csproj* dosya veya yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="1df8a-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="1df8a-271">İçin eklediyseniz *.csproj* dosyası, onu eklendi projesinde her yayımlama profilinde.</span><span class="sxs-lookup"><span data-stu-id="1df8a-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="1df8a-272">Aşağıdaki biçimlendirme gösterir nasıl vurgulanmış için:</span><span class="sxs-lookup"><span data-stu-id="1df8a-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="1df8a-273">Projeye dışında dosyasından kopyalama *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="1df8a-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="1df8a-274">Dışlama *wwwroot\Content* klasör.</span><span class="sxs-lookup"><span data-stu-id="1df8a-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="1df8a-275">Dışlama *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1df8a-275">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="1df8a-276">Bkz: [WebSDK Benioku](https://github.com/aspnet/websdk) daha fazla dağıtım örnekleri için.</span><span class="sxs-lookup"><span data-stu-id="1df8a-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="1df8a-277">Bir hedef önce veya sonra yayımlama çalıştırın</span><span class="sxs-lookup"><span data-stu-id="1df8a-277">Run a target before or after publishing</span></span>

<span data-ttu-id="1df8a-278">Yerleşik `BeforePublish` ve `AfterPublish` hedefleri önce veya sonra yayımlama hedefi bir hedef yürütün.</span><span class="sxs-lookup"><span data-stu-id="1df8a-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="1df8a-279">Yayımlama profili öncesinde ve sonrasında yayımlama konsol iletilerini günlüğe kaydetmek için aşağıdaki öğeleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1df8a-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="1df8a-280">Güvenilmeyen bir sertifika kullanarak bir sunucuya yayımlayın</span><span class="sxs-lookup"><span data-stu-id="1df8a-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="1df8a-281">Ekleme `<AllowUntrustedCertificate>` özellik değerini `True` yayımlama profili için:</span><span class="sxs-lookup"><span data-stu-id="1df8a-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="1df8a-282">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="1df8a-282">The Kudu service</span></span>

<span data-ttu-id="1df8a-283">Azure App Service web uygulama dağıtımı dosyaları görüntülemek için kullanın [Kudu hizmet](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="1df8a-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="1df8a-284">Append `scm` web uygulaması adı belirteci.</span><span class="sxs-lookup"><span data-stu-id="1df8a-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="1df8a-285">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1df8a-285">For example:</span></span>

| <span data-ttu-id="1df8a-286">URL</span><span class="sxs-lookup"><span data-stu-id="1df8a-286">URL</span></span>                                    | <span data-ttu-id="1df8a-287">Sonuç</span><span class="sxs-lookup"><span data-stu-id="1df8a-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="1df8a-288">Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="1df8a-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="1df8a-289">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="1df8a-289">Kudu service</span></span> |

<span data-ttu-id="1df8a-290">Seçin [hata ayıklama Konsolu'nda](https://github.com/projectkudu/kudu/wiki/Kudu-console) görüntüleme, düzenleme, silme veya dosyaları eklemek için kullanılan menü öğesi.</span><span class="sxs-lookup"><span data-stu-id="1df8a-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1df8a-291">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1df8a-291">Additional resources</span></span>

* <span data-ttu-id="1df8a-292">[Web dağıtımı](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) web uygulamaları ve IIS sunucuları için Web siteleri dağıtımını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="1df8a-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="1df8a-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Dosya sorunları ve dağıtım için özelliklerini isteyin.</span><span class="sxs-lookup"><span data-stu-id="1df8a-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
