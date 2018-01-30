---
title: "Visual Studio yayımlama profilleri ASP.NET Core uygulama dağıtımı için"
author: rick-anderson
description: "Nasıl oluşturulacağını Bul Visual Studio'da yayımlama profillerini ASP.NET Core uygulamaları için."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 138b60d0e7c2a3d8848d534ffed854feaf0f5661
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="011af-103">Visual Studio yayımlama profilleri ASP.NET Core uygulama dağıtımı için</span><span class="sxs-lookup"><span data-stu-id="011af-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="011af-104">Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="011af-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="011af-105">Bu makalede oluşturmak için Visual Studio 2017 kullanmaya odaklanır yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="011af-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="011af-106">Visual Studio ile oluşturulan yayımlama profilleri MSBuild ve Visual Studio 2017 çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="011af-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="011af-107">Makale yayımlama işleminin ayrıntıları sağlar.</span><span class="sxs-lookup"><span data-stu-id="011af-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="011af-108">Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) yayımlama için yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="011af-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="011af-109">Aşağıdaki *.csproj* dosyası komutuyla oluşturuldu `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="011af-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="011af-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="011af-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="011af-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="011af-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="011af-112">`Sdk` Özniteliğini `<Project>` öğesi (ilk satırı) yukarıdaki biçimlendirme aşağıdakileri yapar:</span><span class="sxs-lookup"><span data-stu-id="011af-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="011af-113">Özellikler dosyasından içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* başında.</span><span class="sxs-lookup"><span data-stu-id="011af-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="011af-114">Hedef dosyadan içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* sonunda.</span><span class="sxs-lookup"><span data-stu-id="011af-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="011af-115">Varsayılan konumu `MSBuildSDKsPath` (Visual Studio 2017 kuruluş ile) *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* klasör.</span><span class="sxs-lookup"><span data-stu-id="011af-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="011af-116">`Microsoft.NET.Sdk.Web`aşağıdakilere bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="011af-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="011af-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="011af-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="011af-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="011af-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="011af-119">Aşağıdaki özellikler ve içeri aktarılacak hedefleri neden olur:</span><span class="sxs-lookup"><span data-stu-id="011af-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="011af-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="011af-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="011af-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="011af-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="011af-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="011af-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="011af-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="011af-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="011af-124">Kullanılan Yayımla yöntemine göre hedefler hakkını ayarlayın hedefleri alma yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="011af-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="011af-125">MSBuild veya Visual Studio Proje yüklediğinde, aşağıdaki yüksek düzey eylemleri gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="011af-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="011af-126">Proje derleme</span><span class="sxs-lookup"><span data-stu-id="011af-126">Build project</span></span>
* <span data-ttu-id="011af-127">Yayımlanacak dosyaları işlem</span><span class="sxs-lookup"><span data-stu-id="011af-127">Compute files to publish</span></span>
* <span data-ttu-id="011af-128">Dosyaları hedefe yayımlama</span><span class="sxs-lookup"><span data-stu-id="011af-128">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="011af-129">Proje öğeleri işlem</span><span class="sxs-lookup"><span data-stu-id="011af-129">Compute project items</span></span>

<span data-ttu-id="011af-130">Proje yüklendiğinde, proje öğeleri (dosyaları) hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="011af-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="011af-131">`item type` Özniteliği dosya nasıl işleneceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="011af-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="011af-132">Varsayılan olarak, *.cs* dosyaları dahil edilmiştir `Compile` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="011af-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="011af-133">Dosyalar `Compile` öğe listesi derlenir.</span><span class="sxs-lookup"><span data-stu-id="011af-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="011af-134">`Content` Öğesi listesinin yanı sıra yapı çıkışları yayımlanan dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="011af-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="011af-135">Varsayılan olarak, dosyaları desen eşleştirme `wwwroot/**` içinde yer alan `Content` öğesi.</span><span class="sxs-lookup"><span data-stu-id="011af-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="011af-136">[wwwroot /\* \* genelleme deseni](https://gruntjs.com/configuring-tasks#globbing-patterns) tüm dosyalarda belirtir *wwwroot* klasörü **ve** alt klasörler.</span><span class="sxs-lookup"><span data-stu-id="011af-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="011af-137">Açıkça Yayımla listesine bir dosya eklemek için doğrudan dosyasına ekleyin *.csproj* dosya gösterildiği gibi [dosyaları dahil](#including-files).</span><span class="sxs-lookup"><span data-stu-id="011af-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="011af-138">Seçerken **Yayımla** düğmesi Visual Studio'da veya komut satırından yayımlama sırasında:</span><span class="sxs-lookup"><span data-stu-id="011af-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="011af-139">Özellikler/öğeleri hesaplanır (oluşturmak için gereken dosyaları).</span><span class="sxs-lookup"><span data-stu-id="011af-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="011af-140">Yalnızca Visual Studio: NuGet paketleri geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="011af-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="011af-141">(Geri yükleme CLI kullanıcı tarafından açık olması gerekir.)</span><span class="sxs-lookup"><span data-stu-id="011af-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="011af-142">Proje oluşturur.</span><span class="sxs-lookup"><span data-stu-id="011af-142">The project builds.</span></span>
* <span data-ttu-id="011af-143">Yayımla öğeleri hesaplanır (yayımlama için gerekli olan dosyalar).</span><span class="sxs-lookup"><span data-stu-id="011af-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="011af-144">Proje yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="011af-144">The project is published.</span></span> <span data-ttu-id="011af-145">(Hesaplanan dosyalar Yayımla hedefe kopyalanır.)</span><span class="sxs-lookup"><span data-stu-id="011af-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="011af-146">ASP.NET Core projesinde başvurduğunda `Microsoft.NET.Sdk.Web` proje dosyasında bir *app_offline.htm* dosyası, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="011af-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="011af-147">Dosyanın mevcut olduğunda, ASP.NET Core modülü düzgün biçimde uygulamasını kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="011af-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="011af-148">Daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="011af-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="011af-149">Temel komut satırı yayımlama</span><span class="sxs-lookup"><span data-stu-id="011af-149">Basic command-line publishing</span></span>

<span data-ttu-id="011af-150">Komut satırı yayımlama tüm .NET Core desteklenen platformlarda çalışır ve Visual Studio gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="011af-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="011af-151">Aşağıdaki örnekte `dotnet publish` komutu proje dizinden çalıştırın (içeren *.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="011af-151">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="011af-152">Aksi durumda proje klasöründe açıkça proje dosya yolunda geçirin.</span><span class="sxs-lookup"><span data-stu-id="011af-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="011af-153">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="011af-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="011af-154">Oluşturma ve bir web uygulaması yayımlama için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="011af-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="011af-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="011af-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="011af-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="011af-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="011af-157">`dotnet publish` Aşağıdakine benzer bir çıktı üretir:</span><span class="sxs-lookup"><span data-stu-id="011af-157">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="011af-158">Varsayılan klasör yayımlama olduğu `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="011af-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="011af-159">İçin varsayılan `$(Configuration)` Hata Ayıkla'dır.</span><span class="sxs-lookup"><span data-stu-id="011af-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="011af-160">Yukarıdaki örnekteki `<TargetFramework>` olan `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="011af-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="011af-161">`dotnet publish -h`bilgi yayımlama için yardımı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="011af-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="011af-162">Aşağıdaki komut belirtir bir `Release` oluşturma ve yayımlama dizini:</span><span class="sxs-lookup"><span data-stu-id="011af-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="011af-163">`dotnet publish` Komutu çağıran çağıran MSBuild `Publish` hedef.</span><span class="sxs-lookup"><span data-stu-id="011af-163">The `dotnet publish` command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="011af-164">Herhangi bir parametre geçirilen `dotnet publish` MSBuild için geçirilir.</span><span class="sxs-lookup"><span data-stu-id="011af-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="011af-165">`-c` Parametresi eşlendiğini `Configuration` MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="011af-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="011af-166">`-o` Parametresi eşlendiğini `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="011af-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="011af-167">MSBuild özellikleri aşağıdaki biçimlerden birini kullanarak geçirilebilir:</span><span class="sxs-lookup"><span data-stu-id="011af-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="011af-168">Aşağıdaki komutu yayımlayan bir `Release` bir ağ paylaşımına oluşturun:</span><span class="sxs-lookup"><span data-stu-id="011af-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="011af-169">Ağ paylaşımı eğik çizgi ile belirtilir (*//r8/*) ve tüm desteklenen .NET Core platformlarında çalışır.</span><span class="sxs-lookup"><span data-stu-id="011af-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="011af-170">Yayımlanan uygulama dağıtımı için çalışmayan onaylayın.</span><span class="sxs-lookup"><span data-stu-id="011af-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="011af-171">Dosyalar *yayımlama* uygulama çalışırken klasörü kilitlenir.</span><span class="sxs-lookup"><span data-stu-id="011af-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="011af-172">Dağıtım kilitli olduğundan dosyaları kopyalanamaz gerçekleşemez.</span><span class="sxs-lookup"><span data-stu-id="011af-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="011af-173">Yayımlama profilleri</span><span class="sxs-lookup"><span data-stu-id="011af-173">Publish profiles</span></span>

<span data-ttu-id="011af-174">Bu bölümde Visual Studio 2017 ve daha yüksek yayımlama profillerini oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="011af-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="011af-175">Visual Studio veya komut satırı yayımlama oluşturulduktan sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="011af-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="011af-176">Yayımlama profilleri yayımlama işlemini basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="011af-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="011af-177">Birden fazla yayımlama profili bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="011af-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="011af-178">Visual Studio'da bir yayımlama profili oluşturmak için Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="011af-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="011af-179">Alternatif olarak, seçin **Yayımla \<proje adı >** yapı menüsünde.</span><span class="sxs-lookup"><span data-stu-id="011af-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="011af-180">**Yayımla** uygulama kapasiteleri sayfasında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="011af-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="011af-181">Proje bir yayımlama profili içermiyorsa, aşağıdaki sayfayı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="011af-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Azure, IIS, FTB, Azure klasörüyle gösteren uygulama kapasiteleri sayfası Yayımla sekmesi seçili.](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="011af-184">Zaman **klasörü** seçili **Yayımla** düğmesi, bir klasör oluşturur yayımlama profili ve yayımlar.</span><span class="sxs-lookup"><span data-stu-id="011af-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![** Gösteren Azure, IIS, FTB, klasör uygulama kapasiteleri sayfasının sekmesinde Yayımla **](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="011af-186">Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekmesinde değişiklikleri ve seçin **yeni profil oluşturmak** yeni bir profil oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="011af-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![** FolderProfile gösteren uygulama kapasiteleri sayfasının sekmesinde Yayımla **-oluşturulan son adımı ve yeni profil bağlantısını oluşturma](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="011af-188">Yayımlama Sihirbazı'nı aşağıdaki Yayımla hedefleri destekler:</span><span class="sxs-lookup"><span data-stu-id="011af-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="011af-189">Microsoft Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="011af-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="011af-190">IIS, FTP, vb. (için herhangi bir web sunucusu)</span><span class="sxs-lookup"><span data-stu-id="011af-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="011af-191">Klasör</span><span class="sxs-lookup"><span data-stu-id="011af-191">Folder</span></span>
* <span data-ttu-id="011af-192">Profilini içeri aktarma (Profil alma izin verir).</span><span class="sxs-lookup"><span data-stu-id="011af-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="011af-193">Microsoft Azure sanal makineler</span><span class="sxs-lookup"><span data-stu-id="011af-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="011af-194">Bkz: [hangi yayımlama seçeneklerini benim için en uygun?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="011af-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="011af-195">Bir yayımlama profili Visual Studio ile oluştururken bir *özellikleri/PublishProfiles/\<Yayımla adı > .pubxml* MSBuild dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="011af-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="011af-196">Bu *.pubxml* dosyası MSBuild dosyadır ve içeren yapılandırma ayarlarını yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="011af-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="011af-197">Bu dosya, yapı özelleştirmek ve işlem yayımlamak için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="011af-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="011af-198">Bu dosya yayımlama işlemi tarafından okunur.</span><span class="sxs-lookup"><span data-stu-id="011af-198">This file is read by the publishing process.</span></span> <span data-ttu-id="011af-199">`<LastUsedBuildConfiguration>`Genel bir özellik olduğundan ve yapı alınan herhangi bir dosya olmamalıdır özeldir.</span><span class="sxs-lookup"><span data-stu-id="011af-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="011af-200">Bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="011af-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="011af-201">*.Pubxml* dosya döndürmemelidir kontrol edileceği kaynak denetimine bağımlı olduğundan dolayı *.kullanıcı* dosya.</span><span class="sxs-lookup"><span data-stu-id="011af-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="011af-202">*.Kullanıcı* dosya hiç işaretlenmelidir kaynak denetimine hassas bilgileri içerebileceği ve yalnızca bir kullanıcı ve makine için geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="011af-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="011af-203">(Yayımla parola gibi) hassas bilgileri şifrelenir bir kullanıcı/makine düzeyi ve depolanan başına *özellikleri/PublishProfiles/\<Yayımla adı >. pubxml.user* dosya.</span><span class="sxs-lookup"><span data-stu-id="011af-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="011af-204">Bu dosya hassas bilgileri içerdiğinden gereken **değil** kaynak denetimine iade edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="011af-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="011af-205">ASP.NET Core üzerinde bir web uygulaması yayımlama genel bakış için bkz: [konak dağıtıp](index.md).</span><span class="sxs-lookup"><span data-stu-id="011af-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="011af-206">[Ana bilgisayar ve dağıtma](index.md) https://github.com/aspnet/websdk adresindeki bir açık kaynaklı proje.</span><span class="sxs-lookup"><span data-stu-id="011af-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="011af-207">`dotnet publish`klasörü, MSDeploy, kullanabilirsiniz ve [KUDU](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri:</span><span class="sxs-lookup"><span data-stu-id="011af-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="011af-208">Klasör (platformlar arası çalışır):`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="011af-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="011af-209">MSDeploy (şu anda bu yalnızca çalışır MSDeploy platformlar arası değil bu yana Windows'ta):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="011af-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="011af-210">MSDeploy paketi (şu anda bu yalnızca çalışır MSDeploy platformlar arası değil bu yana Windows'ta):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="011af-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="011af-211">Önceki örnekte **yok** geçirmek `deployonbuild` için `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="011af-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="011af-212">Daha fazla bilgi için bkz: [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="011af-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="011af-213">`dotnet publish`herhangi bir platform için Azure yayımlama için KUDU API'lerini destekler.</span><span class="sxs-lookup"><span data-stu-id="011af-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="011af-214">Visual Studio KUDU API'leri ancak desteklenen websdk tarafından çapraz plat yayımlama için Azure'a mu destek yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="011af-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="011af-215">Bir yayımlama profili Ekle *özellikleri/PublishProfiles* klasöründe aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="011af-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="011af-216">Aşağıdaki komutu çalıştırarak Yayımla içerikleri zips ve KUDU API'lerini kullanarak Azure yayımlama:</span><span class="sxs-lookup"><span data-stu-id="011af-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="011af-217">Bir yayımlama profili kullanırken aşağıdaki MSBuild özellikleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="011af-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="011af-218">Adlı bir profille yayımlarken *FolderProfile*, aşağıdaki komutlardan birini çalıştırılabilir:</span><span class="sxs-lookup"><span data-stu-id="011af-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="011af-219">Çağrılırken, `dotnet build`, çağırır `msbuild` yapı çalıştırın ve işlem yayınlamak için.</span><span class="sxs-lookup"><span data-stu-id="011af-219">When invoking `dotnet build`, it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="011af-220">Çağırma `dotnet build` veya `msbuild` bir klasör profilinde geçirilirken temelde eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="011af-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="011af-221">MSBuild doğrudan Windows çağrılırken MSBuild .NET Framework sürümü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="011af-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="011af-222">MSDeploy, Windows makineler yayımlama için şu anda sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="011af-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="011af-223">Çağırma `dotnet build` klasörde olmayan profili MSBuild çağırır ve MSBuild klasörü olmayan profilleri MSDeploy kullanır.</span><span class="sxs-lookup"><span data-stu-id="011af-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="011af-224">Çağırma `dotnet build` bir klasör olmayan profilindeki (MSDeploy kullanarak) MSBuild çağırır ve (bile Windows platformu üzerinde çalışırken) bir hatayla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="011af-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="011af-225">Bir klasör olmayan profili ile yayımlamak için MSBuild doğrudan çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="011af-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="011af-226">Aşağıdaki klasörü yayımlama profili Visual Studio ile oluşturulmuş ve bir ağ paylaşımına yayımlar:</span><span class="sxs-lookup"><span data-stu-id="011af-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="011af-227">Not `<LastUsedBuildConfiguration>` ayarlanır `Release`.</span><span class="sxs-lookup"><span data-stu-id="011af-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="011af-228">Visual Studio'dan yayımlarken `<LastUsedBuildConfiguration>` yapılandırma özellik değeri, yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="011af-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="011af-229">`<LastUsedBuildConfiguration>` Yapılandırma özelliği özeldir ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınmış döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="011af-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="011af-230">Bu özellik, komut satırında geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="011af-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="011af-231">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="011af-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="011af-232">MSBuild kullanma:</span><span class="sxs-lookup"><span data-stu-id="011af-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="011af-233">MSDeploy uç noktasına komut satırından yayımlama</span><span class="sxs-lookup"><span data-stu-id="011af-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="011af-234">Daha önce belirtildiği gibi yayımlama kullanılarak gerçekleştirilebilir `dotnet publish` veya `msbuild` komutu.</span><span class="sxs-lookup"><span data-stu-id="011af-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="011af-235">`dotnet publish`.NET Core bağlamında çalışır.</span><span class="sxs-lookup"><span data-stu-id="011af-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="011af-236">`msbuild` Komutu .NET framework gerektirir ve bu nedenle Windows ortamları sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="011af-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="011af-237">En kolay yolu MSDeploy ile yayımlamak için önce bir yayımlama profili Visual Studio 2017 içinde oluşturmak ve komut satırından profil kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="011af-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="011af-238">Aşağıdaki örnekte, bir ASP.NET Core web uygulaması oluşturuldu (kullanarak `dotnet new mvc`), ve bir Azure yayımlama profili Visual Studio ile eklenir.</span><span class="sxs-lookup"><span data-stu-id="011af-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="011af-239">Çalıştırma `msbuild` gelen bir **VS 2017 için geliştirici komut istemi**.</span><span class="sxs-lookup"><span data-stu-id="011af-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="011af-240">Geliştirici komut istemi doğru sahip *msbuild.exe* yolundaki bazı MSBuild değişkenler kümesine sahip.</span><span class="sxs-lookup"><span data-stu-id="011af-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="011af-241">MSBuild aşağıdaki söz dizimini kullanır:</span><span class="sxs-lookup"><span data-stu-id="011af-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="011af-242">Alma `Password` gelen  *\<Yayımla adı >. PublishSettings* dosya.</span><span class="sxs-lookup"><span data-stu-id="011af-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="011af-243">Karşıdan *. PublishSettings* ya da dosyasından:</span><span class="sxs-lookup"><span data-stu-id="011af-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="011af-244">Çözüm Gezgini: Web uygulamasında sağ tıklatın ve seçin **yayımlama profili indirme**.</span><span class="sxs-lookup"><span data-stu-id="011af-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="011af-245">Azure Yönetim Portalı: Seçin **Get yayımlama profili** Web uygulaması dikey penceresinden.</span><span class="sxs-lookup"><span data-stu-id="011af-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="011af-246">`Username`Yayımlama profili bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="011af-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="011af-247">Aşağıdaki örnek kullanır "Web11112 – Web Dağıtımı" yayımlama profili:</span><span class="sxs-lookup"><span data-stu-id="011af-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="011af-248">Dosyaları dışlama</span><span class="sxs-lookup"><span data-stu-id="011af-248">Excluding files</span></span>

<span data-ttu-id="011af-249">ASP.NET Core web uygulamaları, derleme yapıları ve içeriğini yayımlarken *wwwroot* klasörü dahil.</span><span class="sxs-lookup"><span data-stu-id="011af-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="011af-250">`msbuild`destekleyen [genelleme desenleri](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="011af-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="011af-251">Örneğin, aşağıdaki `<Content>` öğesi biçimlendirme hariç tüm metni (*.txt*) dosyaları buradan *wwwroot/içerik* klasör ve tüm alt klasörlerini.</span><span class="sxs-lookup"><span data-stu-id="011af-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="011af-252">Yukarıdaki biçimlendirme bir yayımlama profili eklenebilir veya *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="011af-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="011af-253">Eklendiğinde *.csproj* dosya, kural eklenmiş olup projedeki tüm yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="011af-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="011af-254">Aşağıdaki `<MsDeploySkipRules>` öğesi biçimlendirme içerenleri tüm dosyaları *wwwroot/içerik* klasörü:</span><span class="sxs-lookup"><span data-stu-id="011af-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="011af-255">`<MsDeploySkipRules>`silmez *atla* dağıtım sitesinden hedefler.</span><span class="sxs-lookup"><span data-stu-id="011af-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="011af-256">`<Content>`hedef dosyalar ve klasörler dağıtım site veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="011af-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="011af-257">Örneğin, aşağıdaki dosyaları dağıtılan web uygulamasının vardı varsayın:</span><span class="sxs-lookup"><span data-stu-id="011af-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="011af-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="011af-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="011af-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="011af-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="011af-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="011af-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="011af-261">Aşağıdaki `<MsDeploySkipRules>` biçimlendirme eklenir, dağıtım sitesinde bu dosyaların silinmesi olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="011af-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

``` xml
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

<span data-ttu-id="011af-262">`<MsDeploySkipRules>` Yukarıda gösterilen biçimlendirme engeller *atlandı* depoyed engeller dosyaları dağıtmış sonra bu dosyaları silmez ancak.</span><span class="sxs-lookup"><span data-stu-id="011af-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="011af-263">Aşağıdaki `<Content>` biçimlendirme dağıtım sitede hedeflenen dosya siler:</span><span class="sxs-lookup"><span data-stu-id="011af-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="011af-264">Komut satırı dağıtımı ile kullanarak `<Content>` aşağıdakine benzer bir çıktı sonuçlarında yukarıda biçimlendirme:</span><span class="sxs-lookup"><span data-stu-id="011af-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

``` console
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

## <a name="including-files"></a><span data-ttu-id="011af-265">Dosyaları dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="011af-265">Including files</span></span>

<span data-ttu-id="011af-266">Aşağıdaki biçimlendirme içeren bir *görüntüleri* klasörünün dışında proje dizinine *wwwroot/görüntüleri* Yayımla sitesinin klasörüne:</span><span class="sxs-lookup"><span data-stu-id="011af-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="011af-267">İşaretleme eklenebilir *.csproj* dosya veya yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="011af-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="011af-268">İçin eklediyseniz *.csproj* dosyası, onu eklendi projesinde her yayımlama profilinde.</span><span class="sxs-lookup"><span data-stu-id="011af-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="011af-269">Aşağıdaki biçimlendirme gösterir nasıl vurgulanmış için:</span><span class="sxs-lookup"><span data-stu-id="011af-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="011af-270">Projeye dışında dosyasından kopyalama *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="011af-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="011af-271">Dışlama *wwwroot\Content* klasör.</span><span class="sxs-lookup"><span data-stu-id="011af-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="011af-272">Dışlama *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="011af-272">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="011af-273">Bkz: [WebSDK Benioku](https://github.com/aspnet/websdk) daha fazla dağıtım örnekleri için.</span><span class="sxs-lookup"><span data-stu-id="011af-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="011af-274">Bir hedef önce veya sonra yayımlama çalıştırın</span><span class="sxs-lookup"><span data-stu-id="011af-274">Run a target before or after publishing</span></span>

<span data-ttu-id="011af-275">Yerleşik `BeforePublish` ve `AfterPublish` hedefleri, bir hedef önce veya sonra yayımlama hedefi yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="011af-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="011af-276">Aşağıdaki biçimlendirmede önce ve sonra yayımlama konsol çıktısı iletilerini günlüğe kaydetmek için yayımlama profili eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="011af-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="011af-277">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="011af-277">The Kudu service</span></span>

<span data-ttu-id="011af-278">Dosyaları görüntülemek için bir Azure uygulama hizmeti web uygulama dağıtımı, kullanım [Kudu hizmet](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="011af-278">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="011af-279">Append `scm` belirteci web uygulamasının adı.</span><span class="sxs-lookup"><span data-stu-id="011af-279">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="011af-280">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="011af-280">For example:</span></span>

| <span data-ttu-id="011af-281">URL</span><span class="sxs-lookup"><span data-stu-id="011af-281">URL</span></span>                                    | <span data-ttu-id="011af-282">Sonuç</span><span class="sxs-lookup"><span data-stu-id="011af-282">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="011af-283">Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="011af-283">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="011af-284">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="011af-284">Kudu sevice</span></span> |

<span data-ttu-id="011af-285">Seçin [hata ayıklama Konsolu'nda](https://github.com/projectkudu/kudu/wiki/Kudu-console) dosyaları görünüm/düzenleme/silme/Ekle menü öğesine.</span><span class="sxs-lookup"><span data-stu-id="011af-285">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="011af-286">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="011af-286">Additional resources</span></span>

* <span data-ttu-id="011af-287">[Web dağıtımı](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) web uygulamaları ve IIS sunucuları için Web siteleri dağıtımını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="011af-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="011af-288">[https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues): dosya sorunları ve dağıtım için özelliklerini isteyin.</span><span class="sxs-lookup"><span data-stu-id="011af-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
