---
title: "Oluşturmak için Visual Studio ve MSBuild yayımlama profilleri"
author: rick-anderson
description: "Visual Studio Web yayımlama açıklanır."
keywords: "ASP.NET Core web yayımlama, yayınlama, msbuild, web dağıtımı, dotnet yayımlama, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: f33fb9d648a611bb7b2b96291dd2176b230a9a43
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a><span data-ttu-id="796b3-104">Oluşturma yayımlama profillerini Visual Studio ve MSBuild, ASP.NET Core uygulamaları dağıtmak</span><span class="sxs-lookup"><span data-stu-id="796b3-104">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>

<span data-ttu-id="796b3-105">Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="796b3-105">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="796b3-106">Bu makalede oluşturmak için Visual Studio 2017 kullanmaya odaklanır yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="796b3-106">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="796b3-107">Visual Studio ile oluşturulan yayımlama profilleri MSBuild ve Visual Studio 2017 çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="796b3-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="796b3-108">Makale yayımlama işleminin ayrıntıları sağlar.</span><span class="sxs-lookup"><span data-stu-id="796b3-108">The article provides details of the publishing process.</span></span> <span data-ttu-id="796b3-109">Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) yayımlama için yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="796b3-109">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="796b3-110">Aşağıdaki *.csproj* dosyası komutuyla oluşturuldu `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="796b3-110">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="796b3-111">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="796b3-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="796b3-112">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="796b3-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="796b3-113">`Sdk` Özniteliğini `<Project>` öğesi (ilk satırı) yukarıdaki biçimlendirme aşağıdakileri yapar:</span><span class="sxs-lookup"><span data-stu-id="796b3-113">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="796b3-114">İçeri aktarmalar `props` dosya *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* başında.</span><span class="sxs-lookup"><span data-stu-id="796b3-114">Imports the `props` file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="796b3-115">Hedef dosyadan içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* sonunda.</span><span class="sxs-lookup"><span data-stu-id="796b3-115">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="796b3-116">Varsayılan konumu `MSBuildSDKsPath` (Visual Studio 2017 kuruluş ile) *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* klasör.</span><span class="sxs-lookup"><span data-stu-id="796b3-116">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="796b3-117">`Microsoft.NET.Sdk.Web`aşağıdakilere bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="796b3-117">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="796b3-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="796b3-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="796b3-119">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="796b3-119">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="796b3-120">Aşağıdaki neden olan `props` ve `targets` içeri aktarılacak:</span><span class="sxs-lookup"><span data-stu-id="796b3-120">Which causes the following `props` and `targets` to be imported:</span></span>

* <span data-ttu-id="796b3-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="796b3-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="796b3-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="796b3-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="796b3-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="796b3-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="796b3-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="796b3-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="796b3-125">Hedefleri alma hedefleri doğru yetenek kümesini kullanılan Yayımla yöntemine göre yeniden yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="796b3-125">Publish targets will again import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="796b3-126">MSBuild veya Visual Studio Proje yüklediğinde, aşağıdaki yüksek düzey eylemleri gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="796b3-126">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="796b3-127">Proje derleme</span><span class="sxs-lookup"><span data-stu-id="796b3-127">Build project</span></span>
* <span data-ttu-id="796b3-128">Yayımlanacak dosyaları işlem</span><span class="sxs-lookup"><span data-stu-id="796b3-128">Compute files to publish</span></span>
* <span data-ttu-id="796b3-129">Dosyaları hedefe yayımlama</span><span class="sxs-lookup"><span data-stu-id="796b3-129">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="796b3-130">Proje öğeleri işlem</span><span class="sxs-lookup"><span data-stu-id="796b3-130">Compute project items</span></span>

<span data-ttu-id="796b3-131">Proje yüklendiğinde, proje öğeleri (dosyaları) hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="796b3-131">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="796b3-132">`item type` Özniteliği dosya nasıl işleneceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="796b3-132">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="796b3-133">Varsayılan olarak, *.cs* dosyaları dahil edilmiştir `Compile` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="796b3-133">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="796b3-134">Dosyalar `Compile` öğe listesi derlenir.</span><span class="sxs-lookup"><span data-stu-id="796b3-134">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="796b3-135">`Content` Öğesi listesinin yanı sıra yapı çıkışları yayımlanan dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="796b3-135">The `Content` item list contains files that will be published in addition to the build outputs.</span></span> <span data-ttu-id="796b3-136">Varsayılan olarak, dosyaları düzeni wwwroot eşleşen / ** dahil edilecek `Content` öğesi.</span><span class="sxs-lookup"><span data-stu-id="796b3-136">By default, files matching the pattern wwwroot/** will be included in the `Content` item.</span></span> <span data-ttu-id="796b3-137">[wwwroot / ** genelleme deseni](https://gruntjs.com/configuring-tasks#globbing-patterns) tüm dosyalarda belirtir *wwwroot* klasörü **ve** alt klasörler.</span><span class="sxs-lookup"><span data-stu-id="796b3-137">[wwwroot/** is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="796b3-138">Açıkça Yayımla listesine bir dosya eklemek için doğrudan dosyasına ekleyin *.csproj* dosya gösterildiği gibi [dosyaları dahil](#including-files).</span><span class="sxs-lookup"><span data-stu-id="796b3-138">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="796b3-139">Seçtiğinizde, **Yayımla** Visual Studio veya komut satırından yayımladığınızda düğmesini:</span><span class="sxs-lookup"><span data-stu-id="796b3-139">When you select the **Publish** button in Visual Studio or when you publish from command line:</span></span>

- <span data-ttu-id="796b3-140">Özellikler/öğeleri hesaplanır (oluşturmak için gereken dosyaları).</span><span class="sxs-lookup"><span data-stu-id="796b3-140">The properties/items are computed (the files that are needed to build).</span></span>
- <span data-ttu-id="796b3-141">Yalnızca Visual Studio: NuGet paketleri geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="796b3-141">Visual Studio only: NuGet packages are restored.</span></span>  <span data-ttu-id="796b3-142">(Geri yükleme CLI kullanıcı tarafından açık olması gerekir.)</span><span class="sxs-lookup"><span data-stu-id="796b3-142">(Restore needs to be explicit by the user on the CLI.)</span></span>
- <span data-ttu-id="796b3-143">Proje oluşturur.</span><span class="sxs-lookup"><span data-stu-id="796b3-143">The project builds.</span></span>
- <span data-ttu-id="796b3-144">Yayımla öğeleri hesaplanır (yayımlama için gerekli olan dosyalar).</span><span class="sxs-lookup"><span data-stu-id="796b3-144">The publish items are computed (the files that are needed to publish).</span></span>
- <span data-ttu-id="796b3-145">Proje yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="796b3-145">The project is published.</span></span> <span data-ttu-id="796b3-146">(Hesaplanan dosyalar Yayımla hedefe kopyalanır.)</span><span class="sxs-lookup"><span data-stu-id="796b3-146">(The computed files are copied to the publish destination.)</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="796b3-147">Temel komut satırı yayımlama</span><span class="sxs-lookup"><span data-stu-id="796b3-147">Basic command line publishing</span></span>

<span data-ttu-id="796b3-148">Bu bölümde, tüm .NET Core desteklenen platformlarda çalışır ve Visual Studio gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="796b3-148">This section works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="796b3-149">Aşağıdaki örnekte `dotnet publish` komutu proje dizinden çalıştırın (içeren *.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="796b3-149">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="796b3-150">Proje klasöründe değilseniz, proje dosya yolunda açıkça geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="796b3-150">If you're not in the project folder, you can explicitly pass in the project file path.</span></span> <span data-ttu-id="796b3-151">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="796b3-151">For example:</span></span>

```console
dotnet publish  c:/webs/web1
```

<span data-ttu-id="796b3-152">Oluşturma ve bir web uygulaması yayımlama için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="796b3-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="796b3-153">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="796b3-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="796b3-154">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="796b3-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

--------------

<span data-ttu-id="796b3-155">`dotnet publish` Aşağıdakine benzer bir çıktı üretir:</span><span class="sxs-lookup"><span data-stu-id="796b3-155">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="796b3-156">Varsayılan klasör yayımlama olduğu `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="796b3-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="796b3-157">İçin varsayılan `$(Configuration)` Hata Ayıkla'dır.</span><span class="sxs-lookup"><span data-stu-id="796b3-157">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="796b3-158">Yukarıdaki örnekteki `<TargetFramework>` olan `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="796b3-158">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="796b3-159">`dotnet publish -h`bilgi yayımlama için yardımı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="796b3-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="796b3-160">Aşağıdaki komut belirtir bir `Release` oluşturma ve yayımlama dizini:</span><span class="sxs-lookup"><span data-stu-id="796b3-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="796b3-161">`dotnet publish` Komutu çağrıları `MSBuild` hangi çağırır `Publish` hedef.</span><span class="sxs-lookup"><span data-stu-id="796b3-161">The `dotnet publish` command calls `MSBuild` which invokes the `Publish` target.</span></span> <span data-ttu-id="796b3-162">Herhangi bir parametre geçirilen `dotnet publish` geçirilen `MSBuild`.</span><span class="sxs-lookup"><span data-stu-id="796b3-162">Any parameters passed to `dotnet publish` are passed to `MSBuild`.</span></span> <span data-ttu-id="796b3-163">`-c` Parametresi eşlendiğini `Configuration` MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="796b3-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="796b3-164">`-o` Parametresi eşlendiğini `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="796b3-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="796b3-165">MSBuild özellikleri aşağıdaki biçimlerden birini kullanarak geçirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="796b3-165">You can pass MSBuild properties using either of the following formats:</span></span>

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

<span data-ttu-id="796b3-166">Aşağıdaki komutu yayımlayan bir `Release` bir ağ paylaşımına oluşturun:</span><span class="sxs-lookup"><span data-stu-id="796b3-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="796b3-167">Ağ paylaşımı eğik çizgi ile belirtilir (*//r8/*) ve tüm desteklenen .NET Core platformlarında çalışır.</span><span class="sxs-lookup"><span data-stu-id="796b3-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="796b3-168">Yayımlanan uygulama dağıtımı için çalışmayan onaylayın.</span><span class="sxs-lookup"><span data-stu-id="796b3-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="796b3-169">Dosyalar *yayımlama* uygulama çalışırken klasörü kilitlenir.</span><span class="sxs-lookup"><span data-stu-id="796b3-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="796b3-170">Dağıtım kilitli olduğundan dosyaları kopyalanamaz gerçekleşemez.</span><span class="sxs-lookup"><span data-stu-id="796b3-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="796b3-171">Yayımlama profilleri</span><span class="sxs-lookup"><span data-stu-id="796b3-171">Publish profiles</span></span>

<span data-ttu-id="796b3-172">Bu bölümde Visual Studio 2017 ve daha yüksek yayımlama profillerini oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="796b3-172">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="796b3-173">Oluşturduktan sonra Visual Studio veya komut satırından yayımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="796b3-173">Once created, you can publish from Visual Studio or the command line.</span></span>

<span data-ttu-id="796b3-174">Yayımlama profilleri yayımlama işlemini basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="796b3-174">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="796b3-175">Yayımlama profilleri birden fazla olabilir.</span><span class="sxs-lookup"><span data-stu-id="796b3-175">You can have multiple publish profiles.</span></span> <span data-ttu-id="796b3-176">Visual Studio'da bir yayımlama profili oluşturmak için çözüm keşfedin projeye sağ tıklayın ve seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="796b3-176">To create a publish profile in Visual Studio, right click on the project in Solution Explore and select **Publish**.</span></span> <span data-ttu-id="796b3-177">Alternatif olarak, seçebileceğiniz **Yayımla \<proje adı >** yapı menüsünde.</span><span class="sxs-lookup"><span data-stu-id="796b3-177">Alternatively, you can select **Publish     \<project name>** from the build menu.</span></span> <span data-ttu-id="796b3-178">**Yayımla** uygulama kapasiteleri sayfasında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="796b3-178">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="796b3-179">Proje bir yayımlama profili içermiyorsa, aşağıdaki sayfayı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="796b3-179">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![** Yayımla ** sekmesinde Azure, IIS, FTB, seçili Azure ile klasör gösteren uygulama kapasiteleri sayfası.](web-publishing-vs/_static/az.png)

<span data-ttu-id="796b3-182">Zaman **klasörü** seçili **Yayımla** düğmesi, bir klasör oluşturur yayımlama profili ve yayımlar.</span><span class="sxs-lookup"><span data-stu-id="796b3-182">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![** Gösteren Azure, IIS, FTB, klasör uygulama kapasiteleri sayfasının sekmesinde Yayımla **](web-publishing-vs/_static/pub1.png)

<span data-ttu-id="796b3-184">Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekme değişiklikleri ve seçin **yeni profil oluşturmak** yeni bir profil oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="796b3-184">Once a publish profile is created, the **Publish** tab changes, and you select **Create new profile** to create a new profile.</span></span>

![** FolderProfile gösteren uygulama kapasiteleri sayfasının sekmesinde Yayımla **-oluşturulan son adımı ve yeni profil bağlantısını oluşturma](web-publishing-vs/_static/create_new.png)

<span data-ttu-id="796b3-186">Yayımlama Sihirbazı'nı aşağıdaki Yayımla hedefleri destekler:</span><span class="sxs-lookup"><span data-stu-id="796b3-186">The Publish wizard supports the following publish targets:</span></span>

- <span data-ttu-id="796b3-187">Microsoft Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="796b3-187">Microsoft Azure App Service</span></span>
- <span data-ttu-id="796b3-188">IIS, FTP, vb. (için herhangi bir web sunucusu)</span><span class="sxs-lookup"><span data-stu-id="796b3-188">IIS, FTP, etc (for any web server)</span></span>
- <span data-ttu-id="796b3-189">Klasör</span><span class="sxs-lookup"><span data-stu-id="796b3-189">Folder</span></span>
- <span data-ttu-id="796b3-190">Profil alma (içeri aktarılacak bir profili sağlar).</span><span class="sxs-lookup"><span data-stu-id="796b3-190">Import profile (allows you to import a profile).</span></span>
- <span data-ttu-id="796b3-191">Microsoft Azure sanal makineler</span><span class="sxs-lookup"><span data-stu-id="796b3-191">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="796b3-192">Bkz: [hangi yayımlama seçeneklerini benim için en uygun?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="796b3-192">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="796b3-193">Visual Studio ile bir yayımlama profili oluşturduğunuzda, bir *özellikleri/PublishProfiles/\<Yayımla adı > .pubxml* MSBuild dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="796b3-193">When you create a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="796b3-194">Bu *.pubxml* dosyası MSBuild dosyadır ve içeren yapılandırma ayarlarını yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="796b3-194">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="796b3-195">Yapı özelleştirme ve yayımlama işlemi için bu dosyayı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="796b3-195">You can change this file to customize the build and publish process.</span></span> <span data-ttu-id="796b3-196">Bu dosya yayımlama işlemi tarafından okunur.</span><span class="sxs-lookup"><span data-stu-id="796b3-196">This file is read by the publishing process.</span></span> <span data-ttu-id="796b3-197">`<LastUsedBuildConfiguration>`Genel bir özellik olduğundan ve yapı alınan herhangi bir dosya olmamalıdır özeldir.</span><span class="sxs-lookup"><span data-stu-id="796b3-197">`<LastUsedBuildConfiguration>` is special because it’s a global property and shouldn’t be in any file that’s imported in the build.</span></span> <span data-ttu-id="796b3-198">Bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="796b3-198">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="796b3-199">*.Pubxml* dosyası değil işaretlenmelidir kaynak denetimine bağımlı olduğundan dolayı *.kullanıcı* dosya.</span><span class="sxs-lookup"><span data-stu-id="796b3-199">The *.pubxml* file should not be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="796b3-200">*.Kullanıcı* dosya hiç işaretlenmelidir kaynak denetimine hassas bilgileri içerebileceği ve yalnızca bir kullanıcı ve makine için geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="796b3-200">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="796b3-201">(Yayımla parola gibi) hassas bilgileri şifrelenir bir kullanıcı/makine düzeyi ve depolanan başına *özellikleri/PublishProfiles/\<Yayımla adı >. pubxml.user* dosya.</span><span class="sxs-lookup"><span data-stu-id="796b3-201">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="796b3-202">Bu dosya hassas bilgileri içerdiğinden gereken **değil** kaynak denetimine iade edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="796b3-202">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="796b3-203">ASP.NET Core üzerinde bir web uygulaması yayımlama genel bakış için bkz: [yayımlama ve dağıtım](index.md).</span><span class="sxs-lookup"><span data-stu-id="796b3-203">For an overview of how to publish a web app on ASP.NET Core see [Publishing and Deployment](index.md).</span></span> <span data-ttu-id="796b3-204">[Yayımlama ve dağıtım](index.md) https://github.com/aspnet/websdk adresindeki bir açık kaynaklı proje.</span><span class="sxs-lookup"><span data-stu-id="796b3-204">[Publishing and Deployment](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

 <span data-ttu-id="796b3-205">`dotnet publish`klasörü, Msdeploy, kullanabilirsiniz ve [KUDU](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri:</span><span class="sxs-lookup"><span data-stu-id="796b3-205">`dotnet publish` can use folder, Msdeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="796b3-206">Klasör (platformlar arası çalışır)`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="796b3-206">Folder (works cross-platform) `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="796b3-207">Msdeploy (şu anda bu yalnızca çalışır msdeploy platformlar arası olmadığından Windows'ta):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="796b3-207">Msdeploy (currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="796b3-208">Msdeploy paketi (şu anda bu yalnızca çalışır msdeploy platformlar arası olmadığından Windows'ta):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="796b3-208">Msdeploy package(currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="796b3-209">Önceki örnekte **yok** geçirmek `deployonbuild` için `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="796b3-209">In the preceeding samples, **don’t** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="796b3-210">Daha fazla bilgi için bkz: [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)</span><span class="sxs-lookup"><span data-stu-id="796b3-210">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)</span></span>

<span data-ttu-id="796b3-211">`dotnet publish`herhangi bir platform için Azure yayımlama için KUDU API'lerini destekler.</span><span class="sxs-lookup"><span data-stu-id="796b3-211">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="796b3-212">Visual Studio KUDU API'leri ancak desteklenen websdk tarafından çapraz plat yayımlama için Azure'a mu destek yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="796b3-212">Visual Studio publish does support the KUDU APIs but it is supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="796b3-213">Bir yayımlama profili Ekle *özellikleri/PublishProfiles* klasöründe aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="796b3-213">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="796b3-214">Aşağıdaki komutu çalıştırarak Yayımla içerikleri zip ve KUDU API'lerini kullanarak Azure yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="796b3-214">Running the following command will zip up the publish contents and publish it to Azure using the KUDU APIs.</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="796b3-215">Bir yayımlama profili kullanırken aşağıdaki MSBuild özellikleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="796b3-215">Set the following MSBuild properties when using a publish profile:</span></span>

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

<span data-ttu-id="796b3-216">Örneğin, adlandırılmış bir profille yayımlarken *FolderProfile* aşağıdaki komutlardan herhangi birini çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="796b3-216">For example, when publishing with a profile named *FolderProfile* you can execute either of the commands below.</span></span>

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="796b3-217">Ne zaman çağırmayı `dotnet build` çağırır `msbuild` yapı çalıştırın ve işlem yayınlamak için.</span><span class="sxs-lookup"><span data-stu-id="796b3-217">When you invoke `dotnet build` it will call `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="796b3-218">Çağırma `dotnet build` veya `msbuild` bir klasör profilinde geçirdiğinizde temelde eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="796b3-218">Calling `dotnet build` or `msbuild` is essentially equivalent when you pass in a folder profile.</span></span> <span data-ttu-id="796b3-219">MSBuild doğrudan Windows çağrılırken MSBuild .NET Framework sürümünü alır.</span><span class="sxs-lookup"><span data-stu-id="796b3-219">When calling MSBuild directly on Windows you get the .NET Framework version of MSBuild.</span></span>  <span data-ttu-id="796b3-220">MSDeploy, Windows makineler yayımlama için şu anda sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="796b3-220">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="796b3-221">Çağırma `dotnet build` klasörde olmayan profili MSBuild çağırır ve MSBuild klasörü olmayan profilleri MSDeploy kullanır.</span><span class="sxs-lookup"><span data-stu-id="796b3-221">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="796b3-222">Çağırma `dotnet build` bir klasör olmayan profilindeki (MSDeploy kullanarak) MSBuild çağırır ve (bile Windows platformu üzerinde çalışırken) bir hatayla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="796b3-222">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="796b3-223">Bir klasör olmayan profili ile yayımlamak için MSBuild doğrudan çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="796b3-223">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="796b3-224">Aşağıdaki klasörü yayımlama profili Visual Studio ile oluşturulmuş ve bir ağ paylaşımına yayımlar:</span><span class="sxs-lookup"><span data-stu-id="796b3-224">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

<span data-ttu-id="796b3-225">Not `<LastUsedBuildConfiguration>` ayarlanır `Release`.</span><span class="sxs-lookup"><span data-stu-id="796b3-225">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span>  <span data-ttu-id="796b3-226">Visual Studio'dan yayımlarken `<LastUsedBuildConfiguration>` yapılandırma özellik değeri, yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="796b3-226">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="796b3-227">`<LastUsedBuildConfiguration>` Yapılandırma özelliği özeldir ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınmış döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="796b3-227">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn’t be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="796b3-228">Bu özellik komut satırından kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="796b3-228">You can override this property from the command line.</span></span> <span data-ttu-id="796b3-229">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="796b3-229">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="796b3-230">MSBuild kullanma:</span><span class="sxs-lookup"><span data-stu-id="796b3-230">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="796b3-231">MSDeploy uç noktasına komut satırından yayımlama</span><span class="sxs-lookup"><span data-stu-id="796b3-231">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="796b3-232">Daha önce belirtildiği gibi kullanarak yayımlayabilirsiniz `dotnet publish` veya `msbuild` komutu.</span><span class="sxs-lookup"><span data-stu-id="796b3-232">As previously mentioned, you can publish using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="796b3-233">`dotnet publish`.NET Core bağlamında çalışır.</span><span class="sxs-lookup"><span data-stu-id="796b3-233">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="796b3-234">`msbuild`.NET framework gerektirir ve bu nedenle Windows ortamları için sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="796b3-234">`msbuild` requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="796b3-235">En kolay yolu MSDeploy ile yayımlamak için önce bir yayımlama profili Visual Studio 2017 içinde oluşturmak ve komut satırından profil kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="796b3-235">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="796b3-236">Aşağıdaki örnekte, bir ASP.NET Core web uygulaması oluşturuldu (kullanarak `dotnet new mvc`) ve Azure yayımlama profili Visual Studio ile eklenir.</span><span class="sxs-lookup"><span data-stu-id="796b3-236">In the following sample, an ASP.NET Core web app is created ( using `dotnet new mvc`) and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="796b3-237">Çalıştırmadan `msbuild` gelen bir **VS 2017 için geliştirici komut istemi**.</span><span class="sxs-lookup"><span data-stu-id="796b3-237">You run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="796b3-238">Geliştirici komut istemi doğru olacaktır *msbuild.exe* yolundaki ve bazı MSBuild değişkenleri ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="796b3-238">The Developer Command Prompt will have the correct *msbuild.exe* in its path and set some MSBuild variables.</span></span>

<span data-ttu-id="796b3-239">MSBuild aşağıdaki söz dizimini kullanır:</span><span class="sxs-lookup"><span data-stu-id="796b3-239">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="796b3-240">Alma `Password` gelen  *\<Yayımla adı >. PublishSettings* dosya.</span><span class="sxs-lookup"><span data-stu-id="796b3-240">You can get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="796b3-241">İndirebilirsiniz *. PublishSettings* dosya:</span><span class="sxs-lookup"><span data-stu-id="796b3-241">You can download the *.PublishSettings* file from:</span></span>

- <span data-ttu-id="796b3-242">Çözüm Gezgini: Web uygulamasında sağ tıklayın ve **yayımlama profili indirme**.</span><span class="sxs-lookup"><span data-stu-id="796b3-242">Solution Explorer: Right click on the Web App and select **Download Publish Profile**.</span></span>
- <span data-ttu-id="796b3-243">Azure Yönetim Portalı: Seçin **Get yayımlama profili** Web uygulaması dikey penceresinden.</span><span class="sxs-lookup"><span data-stu-id="796b3-243">The Azure Management Portal: Select **Get publish profile** from the  Web App blade.</span></span>

<span data-ttu-id="796b3-244">`Username`Yayımlama profili bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="796b3-244">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="796b3-245">Aşağıdaki örnek kullanır "Web11112 – Web Dağıtımı" yayımlama profili:</span><span class="sxs-lookup"><span data-stu-id="796b3-245">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="796b3-246">Dosyaları dışlama</span><span class="sxs-lookup"><span data-stu-id="796b3-246">Excluding files</span></span>

<span data-ttu-id="796b3-247">ASP.NET Core web uygulamaları, derleme yapıları ve içeriğini yayımlarken *wwwroot* klasörü dahil.</span><span class="sxs-lookup"><span data-stu-id="796b3-247">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="796b3-248">`msbuild`destekleyen [genelleme desenleri](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="796b3-248">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="796b3-249">Örneğin, aşağıdaki `<Content>` öğesi biçimlendirme hariç tutmak tüm metni (*.txt*) dosyaları buradan *wwwroot/içerik* klasör ve tüm alt klasörlerini.</span><span class="sxs-lookup"><span data-stu-id="796b3-249">For example, the following `<Content>` element markup will exclude all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="796b3-250">Yukarıdaki biçimlendirme bir yayımlama profili eklenebilir veya *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="796b3-250">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="796b3-251">Eklendiğinde *.csproj* dosya, kural eklenmiş olup projedeki tüm yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="796b3-251">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="796b3-252">Aşağıdaki `<MsDeploySkipRules>` öğesi biçimlendirme içerenleri tüm dosyaları *wwwroot/içerik* klasörü:</span><span class="sxs-lookup"><span data-stu-id="796b3-252">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="796b3-253">`<MsDeploySkipRules>`değil silecek *atla* dağıtım sitesinden hedefler.</span><span class="sxs-lookup"><span data-stu-id="796b3-253">`<MsDeploySkipRules>`  will not delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="796b3-254">`<Content>`hedef dosyalar ve klasörler dağıtım site veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="796b3-254">`<Content>` targeted files and folders will be deleted from the deployment site.</span></span> <span data-ttu-id="796b3-255">Örneğin, bir web uygulaması şu dosyaları dağıttığınız varsayın:</span><span class="sxs-lookup"><span data-stu-id="796b3-255">For example, suppose you had deployed a web app with the following files:</span></span>

- <span data-ttu-id="796b3-256">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="796b3-256">*Views/Home/About1.cshtml*</span></span>
- <span data-ttu-id="796b3-257">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="796b3-257">*Views/Home/About2.cshtml*</span></span>
- <span data-ttu-id="796b3-258">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="796b3-258">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="796b3-259">Aşağıdaki eklediyseniz `<MsDeploySkipRules>` biçimlendirme, bu dosyaları dağıtım sitesinde silinmez.</span><span class="sxs-lookup"><span data-stu-id="796b3-259">If you added the following `<MsDeploySkipRules>` markup, those files would not be deleted on the deployment site.</span></span>

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

<span data-ttu-id="796b3-260">`<MsDeploySkipRules>` Yukarıda gösterilen biçimlendirme engeller *atlandı* dosyaları ancak bunlar dağıtıldığında dosyaları silmeyin depoyed engeller.</span><span class="sxs-lookup"><span data-stu-id="796b3-260">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed, but will not delete those files once they are deployed.</span></span>

<span data-ttu-id="796b3-261">Aşağıdaki `<Content>` biçimlendirme dağıtım sitede hedeflenen dosyaları silin:</span><span class="sxs-lookup"><span data-stu-id="796b3-261">The following `<Content>` markup would delete the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="796b3-262">Komut satırı dağıtımı ile kullanarak `<Content>` biçimlendirme yukarıdaki aşağıdakine benzer bir çıktı neden:</span><span class="sxs-lookup"><span data-stu-id="796b3-262">Using command line deployment with the `<Content>` markup above would result in output similar to the following:</span></span>

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

## <a name="including-files"></a><span data-ttu-id="796b3-263">Dosyaları dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="796b3-263">Including files</span></span>

<span data-ttu-id="796b3-264">Aşağıdaki biçimlendirmede dahil etmek için kullanılan bir *görüntüleri* klasörünün dışında proje dizinine *wwwroot/görüntüleri* Yayımla sitesinin klasörüne.</span><span class="sxs-lookup"><span data-stu-id="796b3-264">The following markup can be used to include an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="796b3-265">İşaretleme eklenebilir *.csproj* dosya veya yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="796b3-265">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="796b3-266">İçin eklediyseniz *.csproj* dosyası, projenin her yayımlama profilinde eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="796b3-266">If it's added to the *.csproj* file, it will be included in each publish profile in the project.</span></span>

<span data-ttu-id="796b3-267">Aşağıdaki biçimlendirme gösterir nasıl vurgulanmış için:</span><span class="sxs-lookup"><span data-stu-id="796b3-267">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="796b3-268">Projeye dışında dosyasından kopyalama *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="796b3-268">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="796b3-269">Dışlama *wwwroot\Content* klasör.</span><span class="sxs-lookup"><span data-stu-id="796b3-269">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="796b3-270">Dışlama *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="796b3-270">Exclude *Views\Home\About2.cshtml*.</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

<span data-ttu-id="796b3-271">Bkz: [WebSDK Benioku](https://github.com/aspnet/websdk) daha fazla dağıtım örnekleri için.</span><span class="sxs-lookup"><span data-stu-id="796b3-271">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="796b3-272">Bir hedef önce veya sonra yayımlama çalıştırın</span><span class="sxs-lookup"><span data-stu-id="796b3-272">Run a target before or after publishing</span></span>

<span data-ttu-id="796b3-273">Yerleşik `BeforePublish` ve `AfterPublish` hedefleri, bir hedef önce veya sonra yayımlama hedefi yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="796b3-273">The builtin `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="796b3-274">Aşağıdaki biçimlendirmede önce ve sonra yayımlama konsol çıktısı iletilerini günlüğe kaydetmek için yayımlama profili eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="796b3-274">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="796b3-275">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="796b3-275">The Kudu service</span></span>

<span data-ttu-id="796b3-276">Azure Web uygulamanızın dosyalarını görüntülemek için kullanın [kudu hizmet](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="796b3-276">To view the files on your Azure Web App, use the [kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="796b3-277">Append `scm` adı ya da Web uygulamanızı belirteci.</span><span class="sxs-lookup"><span data-stu-id="796b3-277">Append the `scm` token to the name or your Web App.</span></span> <span data-ttu-id="796b3-278">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="796b3-278">For example:</span></span>

| <span data-ttu-id="796b3-279">URL</span><span class="sxs-lookup"><span data-stu-id="796b3-279">URL</span></span>               | <span data-ttu-id="796b3-280">Sonuç</span><span class="sxs-lookup"><span data-stu-id="796b3-280">Result</span></span>|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | <span data-ttu-id="796b3-281">Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="796b3-281">Web App</span></span> |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="796b3-282">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="796b3-282">Kudu sevice</span></span> |

<span data-ttu-id="796b3-283">Seçin [hata ayıklama Konsolu'nda](https://github.com/projectkudu/kudu/wiki/Kudu-console) dosyaları görünüm/düzenleme/silme/Ekle menü öğesine.</span><span class="sxs-lookup"><span data-stu-id="796b3-283">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="796b3-284">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="796b3-284">Additional resources</span></span>

- <span data-ttu-id="796b3-285">[Web dağıtımı](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) IIS sunucuları için Web uygulamaları ve Web siteleri dağıtımını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="796b3-285">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy)  (msdeploy) simplifies deployment of Web applications and Web sites to IIS servers.</span></span>

- <span data-ttu-id="796b3-286">[https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues): dosya sorunları ve dağıtım için özelliklerini isteyin.</span><span class="sxs-lookup"><span data-stu-id="796b3-286">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
