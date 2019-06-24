---
title: Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı
author: rick-anderson
description: Oluşturmayı Visual Studio'da yayımlama profilleri ve ASP.NET Core uygulama dağıtımlarını çeşitli hedeflere yönetmek için kullanın.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/21/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 50be5a20f6d927270ef2d9dbc6c1cbf24196978f
ms.sourcegitcommit: 28646e8ca62fb094db1557b5c0c02d5b45531824
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2019
ms.locfileid: "67333418"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="f04f1-103">Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı</span><span class="sxs-lookup"><span data-stu-id="f04f1-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="f04f1-104">Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f04f1-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f04f1-105">Bu belge, Visual Studio 2019 kullanma veya daha sonra oluşturma ve kullanma odaklanır yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="f04f1-105">This document focuses on using Visual Studio 2019 or later to create and use publish profiles.</span></span> <span data-ttu-id="f04f1-106">Visual Studio ile oluşturulan yayımlama profillerine, MSBuild ve Visual Studio ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-106">The publish profiles created with Visual Studio can be used with MSBuild and Visual Studio.</span></span> <span data-ttu-id="f04f1-107">Azure'da yayımlamak için yönergeler için bkz: <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="f04f1-107">For instructions on publishing to Azure, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

<span data-ttu-id="f04f1-108">`dotnet new mvc` Komutu, aşağıdaki kök düzeyinde içeren bir proje dosyası üretir [ \<Proje > öğesi](/visualstudio/msbuild/project-element-msbuild):</span><span class="sxs-lookup"><span data-stu-id="f04f1-108">The `dotnet new mvc` command produces a project file containing the following root-level [\<Project> element](/visualstudio/msbuild/project-element-msbuild):</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

<span data-ttu-id="f04f1-109">Önceki `<Project>` öğenin `Sdk` özniteliği alır MSBuild [özellikleri](/visualstudio/msbuild/msbuild-properties) ve [hedefleri](/visualstudio/msbuild/msbuild-targets) gelen *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\ SDK.props* ve *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="f04f1-109">The preceding `<Project>` element's `Sdk` attribute imports the MSBuild [properties](/visualstudio/msbuild/msbuild-properties) and [targets](/visualstudio/msbuild/msbuild-targets) from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.props* and *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, respectively.</span></span> <span data-ttu-id="f04f1-110">Varsayılan konumu `$(MSBuildSDKsPath)` (Visual Studio 2019 Enterprise ile) *% programfiles (x86) %\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* klasör.</span><span class="sxs-lookup"><span data-stu-id="f04f1-110">The default location for `$(MSBuildSDKsPath)` (with Visual Studio 2019 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="f04f1-111">`Microsoft.NET.Sdk.Web` (Web SDK'sı) dahil olmak üzere diğer SDK'lar üzerinde bağlıdır `Microsoft.NET.Sdk` (.NET Core SDK) ve `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)).</span><span class="sxs-lookup"><span data-stu-id="f04f1-111">`Microsoft.NET.Sdk.Web` (Web SDK) depends on other SDKs, including `Microsoft.NET.Sdk` (.NET Core SDK) and `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)).</span></span> <span data-ttu-id="f04f1-112">Bağımlı her SDK ile ilişkili hedefler ve MSBuild özellikleri içeri aktarılır.</span><span class="sxs-lookup"><span data-stu-id="f04f1-112">The MSBuild properties and targets associated with each dependent SDK are imported.</span></span> <span data-ttu-id="f04f1-113">Hedefler içe hedefleri kullanılan Yayımla yöntemine göre uygun kümesini yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="f04f1-113">Publish targets import the appropriate set of targets based on the publish method used.</span></span>

<span data-ttu-id="f04f1-114">MSBuild veya Visual Studio bir projeyi yüklediğinde, aşağıdaki üst düzey eylemler gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="f04f1-114">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="f04f1-115">Proje derleme</span><span class="sxs-lookup"><span data-stu-id="f04f1-115">Build project</span></span>
* <span data-ttu-id="f04f1-116">Yayımlamak için dosyaların işlem</span><span class="sxs-lookup"><span data-stu-id="f04f1-116">Compute files to publish</span></span>
* <span data-ttu-id="f04f1-117">Hedef dosya yayımlama</span><span class="sxs-lookup"><span data-stu-id="f04f1-117">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="f04f1-118">Proje öğeleri işlem</span><span class="sxs-lookup"><span data-stu-id="f04f1-118">Compute project items</span></span>

<span data-ttu-id="f04f1-119">Proje yüklendiğinde [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) (dosyalar) hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="f04f1-119">When the project is loaded, the [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) (files) are computed.</span></span> <span data-ttu-id="f04f1-120">Öğe türü, dosyanın nasıl işleneceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="f04f1-120">The item type determines how the file is processed.</span></span> <span data-ttu-id="f04f1-121">Varsayılan olarak, *.cs* dosyaları dahil edilecek `Compile` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="f04f1-121">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="f04f1-122">Dosyalar `Compile` öğe listesi derlenir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-122">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="f04f1-123">`Content` Öğesi listesinin yanı sıra derleme çıktılarını yayımlanan dosyaları içerir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-123">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="f04f1-124">Varsayılan olarak, dosyaları desenlerle eşleşen `wwwroot\**`, `**\*.config`, ve `**\*.json` dahil `Content` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="f04f1-124">By default, files matching the patterns `wwwroot\**`, `**\*.config`, and `**\*.json` are included in the `Content` item list.</span></span> <span data-ttu-id="f04f1-125">Örneğin, `wwwroot\**` [Glob deseni](https://gruntjs.com/configuring-tasks#globbing-patterns) eşleşen tüm dosyaları *wwwroot* klasör ve alt klasörleri.</span><span class="sxs-lookup"><span data-stu-id="f04f1-125">For example, the `wwwroot\**` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder and its subfolders.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f04f1-126">Web SDK'sı aktarır [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="f04f1-126">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="f04f1-127">Bunun sonucunda, dosyaları desenlerle eşleşen `**\*.cshtml` ve `**\*.razor` de dahil edilir `Content` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="f04f1-127">As a result, files matching the patterns `**\*.cshtml` and `**\*.razor` are also included in the `Content` item list.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="f04f1-128">Web SDK'sı aktarır [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="f04f1-128">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="f04f1-129">Bunun sonucunda, dosyaları eşleşen `**\*.cshtml` düzeni de dahil edilecek `Content` öğe listesi.</span><span class="sxs-lookup"><span data-stu-id="f04f1-129">As a result, files matching the `**\*.cshtml` pattern are also included in the `Content` item list.</span></span>

::: moniker-end

<span data-ttu-id="f04f1-130">Açıkça Yayımla listesine bir dosya eklemek için dosyanın doğrudan ekleme *.csproj* gösterildiği gibi dosya [dosyaları içerir](#include-files) bölümü.</span><span class="sxs-lookup"><span data-stu-id="f04f1-130">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in the [Include Files](#include-files) section.</span></span>

<span data-ttu-id="f04f1-131">Seçerken **Yayımla** düğme Visual Studio'da veya komut satırından yayımlama sırasında:</span><span class="sxs-lookup"><span data-stu-id="f04f1-131">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="f04f1-132">Özellikler/öğeleri hesaplanır (oluşturmak için gereken dosyaları).</span><span class="sxs-lookup"><span data-stu-id="f04f1-132">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="f04f1-133">**Yalnızca Visual Studio**: NuGet paketlerini geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-133">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="f04f1-134">(Geri yükleme CLI kullanıcı tarafından açık olması gerekir.)</span><span class="sxs-lookup"><span data-stu-id="f04f1-134">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="f04f1-135">Projeyi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f04f1-135">The project builds.</span></span>
* <span data-ttu-id="f04f1-136">Yayımlama öğeleri hesaplanır (yayımlama için gerekli dosyaları).</span><span class="sxs-lookup"><span data-stu-id="f04f1-136">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="f04f1-137">Proje yayımlandığında (hesaplanan dosyalar Yayımla hedefe kopyalanır).</span><span class="sxs-lookup"><span data-stu-id="f04f1-137">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="f04f1-138">Bir ASP.NET Core projesi başvurduğunda `Microsoft.NET.Sdk.Web` proje dosyasında bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-138">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="f04f1-139">Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="f04f1-139">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="f04f1-140">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="f04f1-140">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="f04f1-141">Temel bir komut satırı yayımlama</span><span class="sxs-lookup"><span data-stu-id="f04f1-141">Basic command-line publishing</span></span>

<span data-ttu-id="f04f1-142">Komut satırı yayımlama, .NET Core tarafından desteklenen tüm platformlarda çalışır ve Visual Studio gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="f04f1-142">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="f04f1-143">Aşağıdaki örneklerde, .NET Core CLI's [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu proje dizininden çalıştırın (içeren *.csproj* dosyası).</span><span class="sxs-lookup"><span data-stu-id="f04f1-143">In the following examples, the .NET Core CLI's [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="f04f1-144">Proje klasörü geçerli çalışma dizini değilse, proje dosyası yolu açıkça geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f04f1-144">If the project folder isn't the current working directory, explicitly pass in the project file path.</span></span> <span data-ttu-id="f04f1-145">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f04f1-145">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="f04f1-146">Oluşturma ve bir web uygulaması yayımlamak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f04f1-146">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet publish
```

<span data-ttu-id="f04f1-147">`dotnet publish` Komutu, bir çeşitlemesi aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="f04f1-147">The `dotnet publish` command produces a variation of the following output:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {VERSION} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\publish\
```

<span data-ttu-id="f04f1-148">Varsayılan klasör yayımlama biçimi *bin\Debug\\{hedef çerçeve adı} \publish\\* .</span><span class="sxs-lookup"><span data-stu-id="f04f1-148">The default publish folder format is *bin\Debug\\{TARGET FRAMEWORK MONIKER}\publish\\*.</span></span> <span data-ttu-id="f04f1-149">Örneğin, *bin\Debug\netcoreapp2.2\publish\\* .</span><span class="sxs-lookup"><span data-stu-id="f04f1-149">For example, *bin\Debug\netcoreapp2.2\publish\\*.</span></span>

<span data-ttu-id="f04f1-150">Aşağıdaki komut belirtir bir `Release` derleme ve yayımlama dizini:</span><span class="sxs-lookup"><span data-stu-id="f04f1-150">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="f04f1-151">`dotnet publish` Çağrıları çağıran MSBuild komut `Publish` hedef.</span><span class="sxs-lookup"><span data-stu-id="f04f1-151">The `dotnet publish` command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="f04f1-152">Herhangi bir parametre geçirilen `dotnet publish` MSBuild'e geçirilir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-152">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="f04f1-153">`-c` Ve `-o` MSBuild'e ait eşleme parametreleri `Configuration` ve `OutputPath` özellikleri, sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="f04f1-153">The `-c` and `-o` parameters map to MSBuild's `Configuration` and `OutputPath` properties, respectively.</span></span>

<span data-ttu-id="f04f1-154">MSBuild özellikleri, aşağıdaki biçimlerden birini kullanarak geçirilebilir:</span><span class="sxs-lookup"><span data-stu-id="f04f1-154">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="f04f1-155">Örneğin, aşağıdaki komutu yayımlar bir `Release` bir ağ paylaşımına oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f04f1-155">For example, the following command publishes a `Release` build to a network share.</span></span> <span data-ttu-id="f04f1-156">Ağ paylaşımı eğik çizgi ile belirtilen ( *//r8/* ) ve .NET Core desteklenen tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="f04f1-156">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="f04f1-157">Yayımlanan uygulama dağıtımı için çalışmadığından emin onaylayın.</span><span class="sxs-lookup"><span data-stu-id="f04f1-157">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="f04f1-158">Dosyalar *yayımlama* klasörü, uygulama çalışırken kilitlenir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-158">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="f04f1-159">Dağıtım kilitli olduğundan dosya kopyalanamadı oluşamaz.</span><span class="sxs-lookup"><span data-stu-id="f04f1-159">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="f04f1-160">Yayımlama profilleri</span><span class="sxs-lookup"><span data-stu-id="f04f1-160">Publish profiles</span></span>

<span data-ttu-id="f04f1-161">Bu bölümde Visual Studio 2019 veya üzeri bir yayımlama profili oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f04f1-161">This section uses Visual Studio 2019 or later to create a publishing profile.</span></span> <span data-ttu-id="f04f1-162">Visual Studio ya da komut satırından yayımlama profili oluşturulduktan sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-162">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span> <span data-ttu-id="f04f1-163">Yayımlama profilleri yayımlama işlemini basitleştirmek ve herhangi bir sayıda profilleri bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-163">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span>

<span data-ttu-id="f04f1-164">Bir yayımlama profili, aşağıdaki yollardan birini seçerek Visual Studio'da oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f04f1-164">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="f04f1-165">Projeye sağ **Çözüm Gezgini** seçip **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="f04f1-165">Right-click the project in **Solution Explorer** and select **Publish**.</span></span>
* <span data-ttu-id="f04f1-166">Seçin **yayımlama {proje adı}** gelen **derleme** menüsü.</span><span class="sxs-lookup"><span data-stu-id="f04f1-166">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="f04f1-167">**Yayımla** uygulama özellikleri sayfasının sekmesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-167">The **Publish** tab of the app capabilities page is displayed.</span></span> <span data-ttu-id="f04f1-168">Proje bir yayımlama profili yoksa **yayımlama hedefi seçin** sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-168">If the project lacks a publish profile, the **Pick a publish target** page is displayed.</span></span> <span data-ttu-id="f04f1-169">Aşağıdaki yayımlama hedeflerinden birini seçin istenir:</span><span class="sxs-lookup"><span data-stu-id="f04f1-169">You're asked to select one of the following publish targets:</span></span>

* <span data-ttu-id="f04f1-170">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="f04f1-170">Azure App Service</span></span>
* <span data-ttu-id="f04f1-171">Linux üzerinde Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f04f1-171">Azure App Service on Linux</span></span>
* <span data-ttu-id="f04f1-172">Azure sanal makineleri</span><span class="sxs-lookup"><span data-stu-id="f04f1-172">Azure Virtual Machines</span></span>
* <span data-ttu-id="f04f1-173">Klasör</span><span class="sxs-lookup"><span data-stu-id="f04f1-173">Folder</span></span>
* <span data-ttu-id="f04f1-174">IIS, FTP, Web dağıtımı (için herhangi bir web sunucusu)</span><span class="sxs-lookup"><span data-stu-id="f04f1-174">IIS, FTP, Web Deploy (for any web server)</span></span>
* <span data-ttu-id="f04f1-175">Profili içeri aktar</span><span class="sxs-lookup"><span data-stu-id="f04f1-175">Import Profile</span></span>

<span data-ttu-id="f04f1-176">En uygun yayımlama hedef belirlemek için bkz: [hangi Yayımlama seçenekleri benim için en uygun](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="f04f1-176">To determine the most appropriate publish target, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="f04f1-177">Zaman **klasör** yayımlama hedef seçildiğinde, yayımlanan varlıkları depolamak için bir klasör yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="f04f1-177">When the **Folder** publish target is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="f04f1-178">Varsayılan klasör yolu *bin\\{PROJECT Yapılandırması}\\{hedef çerçeve adı} \publish\\* .</span><span class="sxs-lookup"><span data-stu-id="f04f1-178">The default folder path is *bin\\{PROJECT CONFIGURATION}\\{TARGET FRAMEWORK MONIKER}\publish\\*.</span></span> <span data-ttu-id="f04f1-179">Örneğin, *bin\Release\netcoreapp2.2\publish\\* .</span><span class="sxs-lookup"><span data-stu-id="f04f1-179">For example, *bin\Release\netcoreapp2.2\publish\\*.</span></span> <span data-ttu-id="f04f1-180">Seçin **profili oluştur** düğmesini tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="f04f1-180">Select the **Create Profile** button to finish.</span></span>

<span data-ttu-id="f04f1-181">Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekmenin içerik değişiklikleri.</span><span class="sxs-lookup"><span data-stu-id="f04f1-181">Once a publish profile is created, the **Publish** tab's content changes.</span></span> <span data-ttu-id="f04f1-182">Yeni oluşturulan profil aşağı açılan listede görünür.</span><span class="sxs-lookup"><span data-stu-id="f04f1-182">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="f04f1-183">Açılır listede seçin **yeni profil oluşturma** başka bir yeni profili oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="f04f1-183">Below the drop-down list, select **Create new profile** to create another new profile.</span></span>

<span data-ttu-id="f04f1-184">Visual Studio'nun Yayımla aracı üreten bir *özellikleri/PublishProfiles / {PROFİL adı} .pubxml* yayımlama profili açıklayan MSBuild dosyası.</span><span class="sxs-lookup"><span data-stu-id="f04f1-184">Visual Studio's publish tool produces a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file describing the publish profile.</span></span> <span data-ttu-id="f04f1-185">*.Pubxml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f04f1-185">The *.pubxml* file:</span></span>

* <span data-ttu-id="f04f1-186">İçeren yapılandırma ayarlarını yayımlayın ve yayımlama işlemi tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f04f1-186">Contains publish configuration settings and is consumed by the publishing process.</span></span>
* <span data-ttu-id="f04f1-187">Yapı özelleştirme ve yayımlama işlemi için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-187">Can be modified to customize the build and publish process.</span></span>

<span data-ttu-id="f04f1-188">Azure bir hedefe, yayımlama sırasında *.pubxml* dosyası, Azure abonelik tanımlayıcısı içeriyor.</span><span class="sxs-lookup"><span data-stu-id="f04f1-188">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="f04f1-189">Bu dosya kaynak denetimine eklemeye, hedef türüyle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="f04f1-189">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="f04f1-190">Azure dışı hedef yayımlama sırasında iade etmeye güvenli *.pubxml* dosya.</span><span class="sxs-lookup"><span data-stu-id="f04f1-190">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="f04f1-191">(Yayımlama parola gibi) hassas bilgiler şifreli bir kullanıcı/makine düzeyinde başına.</span><span class="sxs-lookup"><span data-stu-id="f04f1-191">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="f04f1-192">İçinde depolanan *özellikleri/PublishProfiles / {PROFİL adı}.pubxml.user* dosya.</span><span class="sxs-lookup"><span data-stu-id="f04f1-192">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="f04f1-193">Bu dosya, hassas bilgileri depolayabileceğiniz için kaynak denetimine iade olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="f04f1-193">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="f04f1-194">Nasıl bir ASP.NET Core web uygulaması yayımlamak genel bakış için bkz. <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="f04f1-194">For an overview of how to publish an ASP.NET Core web app, see <xref:host-and-deploy/index>.</span></span> <span data-ttu-id="f04f1-195">Açık kaynaklı bir ASP.NET Core web uygulaması yayımlamak için gerekli olan hedefler ve MSBuild görevleri sırasında [aspnet/websdk depo](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="f04f1-195">The MSBuild tasks and targets necessary to publish an ASP.NET Core web app are open-source at the [aspnet/websdk repository](https://github.com/aspnet/websdk).</span></span>

<span data-ttu-id="f04f1-196">`dotnet publish` Komutu, klasör, MSDeploy, kullanabilir ve [Kudu](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="f04f1-196">The `dotnet publish` command can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles.</span></span> <span data-ttu-id="f04f1-197">Platformlar arası destek MSDeploy olmadığı için aşağıdaki MSDeploy seçenekleri yalnızca Windows üzerinde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-197">Because MSDeploy lacks cross-platform support, the following MSDeploy options are supported only on Windows.</span></span>

<span data-ttu-id="f04f1-198">**(Çalışan platformlar arası) klasörü:**</span><span class="sxs-lookup"><span data-stu-id="f04f1-198">**Folder (works cross-platform):**</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="f04f1-199">**MSDeploy:**</span><span class="sxs-lookup"><span data-stu-id="f04f1-199">**MSDeploy:**</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="f04f1-200">**MSDeploy paketi:**</span><span class="sxs-lookup"><span data-stu-id="f04f1-200">**MSDeploy package:**</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="f04f1-201">Yukarıdaki örneklerde geçirmezseniz `deployonbuild` için `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="f04f1-201">In the preceding examples, don't pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="f04f1-202">Daha fazla bilgi için [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="f04f1-202">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="f04f1-203">`dotnet publish` her platformda dizinden Azure'da yayımlamak için Kudu API'leri destekler.</span><span class="sxs-lookup"><span data-stu-id="f04f1-203">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="f04f1-204">Visual Studio yayımlama Kudu API'leri, ancak desteklenen WebSDK ile platformlar arası yayımlamak için Azure'a destekler.</span><span class="sxs-lookup"><span data-stu-id="f04f1-204">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="f04f1-205">Projenin bir yayımlama profili Ekle *özellikleri/PublishProfiles* klasöründe aşağıdaki içeriğe sahip:</span><span class="sxs-lookup"><span data-stu-id="f04f1-205">Add a publish profile to the project's *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="f04f1-206">Yayımla içerikleri zip ve Kudu API'lerini kullanarak Azure'da yayımlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f04f1-206">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="f04f1-207">Bir yayımlama profili kullanırken aşağıdaki MSBuild özellikleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="f04f1-207">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

<span data-ttu-id="f04f1-208">Adlı bir profille yayımlarken *FolderProfile*, aşağıdaki komutlardan birini yürütülebilir:</span><span class="sxs-lookup"><span data-stu-id="f04f1-208">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="f04f1-209">.NET Core CLI'ın [dotnet derleme](/dotnet/core/tools/dotnet-build) komut çağrıları `msbuild` yapıyı çalıştırmak ve işlem yayınlamak için.</span><span class="sxs-lookup"><span data-stu-id="f04f1-209">The .NET Core CLI's [dotnet build](/dotnet/core/tools/dotnet-build) command calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="f04f1-210">`dotnet build` Ve `msbuild` klasör profilinde geçerken komutlar eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-210">The `dotnet build` and `msbuild` commands are equivalent when passing in a folder profile.</span></span> <span data-ttu-id="f04f1-211">Çağrılırken `msbuild` doğrudan Windows üzerinde MSBuild .NET Framework sürümü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f04f1-211">When calling `msbuild` directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="f04f1-212">Çağırma `dotnet build` klasörü olmayan profilindeki:</span><span class="sxs-lookup"><span data-stu-id="f04f1-212">Calling `dotnet build` on a non-folder profile:</span></span>

* <span data-ttu-id="f04f1-213">Çağıran `msbuild`, MSDeploy kullanır.</span><span class="sxs-lookup"><span data-stu-id="f04f1-213">Invokes `msbuild`, which uses MSDeploy.</span></span>
* <span data-ttu-id="f04f1-214">(Hatta Windows üzerinde çalışırken) bir hatayla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="f04f1-214">Results in a failure (even when running on Windows).</span></span> <span data-ttu-id="f04f1-215">Bir klasörü olmayan profili ile yayımlamak için çağrı `msbuild` doğrudan.</span><span class="sxs-lookup"><span data-stu-id="f04f1-215">To publish with a non-folder profile, call `msbuild` directly.</span></span>

<span data-ttu-id="f04f1-216">Aşağıdaki klasörü yayımlama profili Visual Studio ile oluşturulmuş ve bir ağ paylaşımına yayımlar:</span><span class="sxs-lookup"><span data-stu-id="f04f1-216">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="f04f1-217">Önceki örnekte:</span><span class="sxs-lookup"><span data-stu-id="f04f1-217">In the preceding example:</span></span>

* <span data-ttu-id="f04f1-218">`<ExcludeApp_Data>` Özelliği yalnızca bir XML Şeması gereksinimi karşılamak için mevcut.</span><span class="sxs-lookup"><span data-stu-id="f04f1-218">The `<ExcludeApp_Data>` property is present merely to satisfy an XML schema requirement.</span></span> <span data-ttu-id="f04f1-219">`<ExcludeApp_Data>` Özellik üzerinde hiçbir etkisi yayımlama işlemi olsa bile bir *App_Data* proje kök klasöründe.</span><span class="sxs-lookup"><span data-stu-id="f04f1-219">The `<ExcludeApp_Data>` property has no effect on the publish process, even if there's an *App_Data* folder in the project root.</span></span> <span data-ttu-id="f04f1-220">*App_Data* ASP.NET 4.x projelerinde olduğu gibi klasör özel olarak değerlendirilmesi alma değil.</span><span class="sxs-lookup"><span data-stu-id="f04f1-220">The *App_Data* folder doesn't receive special treatment as it does in ASP.NET 4.x projects.</span></span>

* <span data-ttu-id="f04f1-221">`<LastUsedBuildConfiguration>` Özelliği `Release`.</span><span class="sxs-lookup"><span data-stu-id="f04f1-221">The `<LastUsedBuildConfiguration>` property is set to `Release`.</span></span> <span data-ttu-id="f04f1-222">Visual Studio'dan değerini yayımlarken `<LastUsedBuildConfiguration>` yayımlama işlemi başlatıldığında değeri kullanılarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="f04f1-222">When publishing from Visual Studio, the value of `<LastUsedBuildConfiguration>` is set using the value when the publish process is started.</span></span> <span data-ttu-id="f04f1-223">`<LastUsedBuildConfiguration>` Özel ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınan olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="f04f1-223">`<LastUsedBuildConfiguration>` is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="f04f1-224">Bu özellik ancak olabilir aşağıdaki yaklaşımlardan birini kullanarak komut satırından geçersiz kılındı.</span><span class="sxs-lookup"><span data-stu-id="f04f1-224">This property can, however, be overridden from the command line using one of the following approaches.</span></span>
  * <span data-ttu-id="f04f1-225">.NET Core CLI kullanarak:</span><span class="sxs-lookup"><span data-stu-id="f04f1-225">Using the .NET Core CLI:</span></span>

    ```console
    dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  * <span data-ttu-id="f04f1-226">MSBuild kullanarak:</span><span class="sxs-lookup"><span data-stu-id="f04f1-226">Using MSBuild:</span></span>

    ```console
    msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  <span data-ttu-id="f04f1-227">Daha fazla bilgi için [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).</span><span class="sxs-lookup"><span data-stu-id="f04f1-227">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).</span></span>

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="f04f1-228">Komut satırından bir MSDeploy uç noktasına yayımlama</span><span class="sxs-lookup"><span data-stu-id="f04f1-228">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="f04f1-229">Aşağıdaki örnekte adlı Visual Studio tarafından oluşturulan bir ASP.NET Core web uygulaması *AzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="f04f1-229">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="f04f1-230">Azure uygulamaları yayımlama profili Visual Studio ile eklenir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-230">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="f04f1-231">Profil oluşturma hakkında daha fazla bilgi için bkz. [yayımlama profillerini](#publish-profiles) bölümü.</span><span class="sxs-lookup"><span data-stu-id="f04f1-231">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="f04f1-232">Bir yayımlama profili kullanarak uygulama dağıtmak için yürütme `msbuild` Visual Studio'dan komutunu **Geliştirici komut istemi**.</span><span class="sxs-lookup"><span data-stu-id="f04f1-232">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="f04f1-233">Komut istemi kullanılabilir *Visual Studio* klasörü **Başlat** Windows görev çubuğundaki menü.</span><span class="sxs-lookup"><span data-stu-id="f04f1-233">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="f04f1-234">Daha kolay erişim için komut satırına ekleyebilirsiniz **Araçları** Visual Studio'daki menü.</span><span class="sxs-lookup"><span data-stu-id="f04f1-234">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="f04f1-235">Daha fazla bilgi için [Visual Studio için geliştirici komut istemi](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="f04f1-235">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="f04f1-236">MSBuild, şu komut söz dizimini kullanır:</span><span class="sxs-lookup"><span data-stu-id="f04f1-236">MSBuild uses the following command syntax:</span></span>

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="f04f1-237">{PATH} &ndash; Uygulamanın proje dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="f04f1-237">{PATH} &ndash; Path to the app's project file.</span></span>
* <span data-ttu-id="f04f1-238">{} PROFİLİ &ndash; Yayımlama profilinin adı.</span><span class="sxs-lookup"><span data-stu-id="f04f1-238">{PROFILE} &ndash; Name of the publish profile.</span></span>
* <span data-ttu-id="f04f1-239">{USERNAME} &ndash; MSDeploy kullanıcı adı.</span><span class="sxs-lookup"><span data-stu-id="f04f1-239">{USERNAME} &ndash; MSDeploy username.</span></span> <span data-ttu-id="f04f1-240">{USERNAME} yayımlama profilinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-240">The {USERNAME} can be found in the publish profile.</span></span>
* <span data-ttu-id="f04f1-241">{PASSWORD} &ndash; MSDeploy parola.</span><span class="sxs-lookup"><span data-stu-id="f04f1-241">{PASSWORD} &ndash; MSDeploy password.</span></span> <span data-ttu-id="f04f1-242">{PASSWORD} almak *{profili}. PublishSettings* dosya.</span><span class="sxs-lookup"><span data-stu-id="f04f1-242">Obtain the {PASSWORD} from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="f04f1-243">İndirme *. PublishSettings* dosyasından ya da:</span><span class="sxs-lookup"><span data-stu-id="f04f1-243">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="f04f1-244">**Çözüm Gezgini**: Seçin **görünümü** > **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="f04f1-244">**Solution Explorer**: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="f04f1-245">Azure aboneliğinizle bağlanın.</span><span class="sxs-lookup"><span data-stu-id="f04f1-245">Connect with your Azure subscription.</span></span> <span data-ttu-id="f04f1-246">Açık **uygulama hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="f04f1-246">Open **App Services**.</span></span> <span data-ttu-id="f04f1-247">Uygulamaya sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f04f1-247">Right-click the app.</span></span> <span data-ttu-id="f04f1-248">Seçin **yayımlama profili indir**.</span><span class="sxs-lookup"><span data-stu-id="f04f1-248">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="f04f1-249">Azure portalı: Seçin **yayımlama profili Al** web uygulamasının **genel bakış** paneli.</span><span class="sxs-lookup"><span data-stu-id="f04f1-249">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="f04f1-250">Aşağıdaki örnekte adlı bir yayımlama profili *AzureWebApp - Web dağıtımı*:</span><span class="sxs-lookup"><span data-stu-id="f04f1-250">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="f04f1-251">Bir yayımlama profili, .NET Core CLI ile de kullanılabilir [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) bir Windows komut kabuğu komutunu:</span><span class="sxs-lookup"><span data-stu-id="f04f1-251">A publish profile can also be used with the .NET Core CLI's [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command shell:</span></span>

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!IMPORTANT]
> <span data-ttu-id="f04f1-252">`dotnet msbuild` Komut bir platformlar arası komut ve macOS ve Linux'ta ASP.NET Core uygulamaları derleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f04f1-252">The `dotnet msbuild` command is a cross-platform command and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="f04f1-253">Ancak, MSBuild MacOS ve Linux Azure veya diğer MSDeploy uç noktalar için uygulama dağıtma uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="f04f1-253">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoints.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="f04f1-254">Ortamı ayarlama</span><span class="sxs-lookup"><span data-stu-id="f04f1-254">Set the environment</span></span>

<span data-ttu-id="f04f1-255">Dahil `<EnvironmentName>` yayımlama profilini özelliğinde ( *.pubxml*) veya uygulamanın ayarlamak için proje dosyasını [ortam](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="f04f1-255">Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file to set the app's [environment](xref:fundamentals/environments):</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="f04f1-256">Gerektiriyorsa *web.config* Dönüşümleri (yapılandırma, profili veya ortama göre örneğin, ortam değişkenlerini ayarlama), bkz: <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="f04f1-256">If you require *web.config* transformations (for example, setting environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="f04f1-257">Dosyaları dışarıda bırak</span><span class="sxs-lookup"><span data-stu-id="f04f1-257">Exclude files</span></span>

<span data-ttu-id="f04f1-258">ASP.NET Core web uygulamaları yayımlarken, aşağıdaki varlıklar dahildir:</span><span class="sxs-lookup"><span data-stu-id="f04f1-258">When publishing ASP.NET Core web apps, the following assets are included:</span></span>

* <span data-ttu-id="f04f1-259">Derleme yapıları</span><span class="sxs-lookup"><span data-stu-id="f04f1-259">Build artifacts</span></span>
* <span data-ttu-id="f04f1-260">Dosya ve klasörleri aşağıdaki Glob desenlerinin eşleşen:</span><span class="sxs-lookup"><span data-stu-id="f04f1-260">Folders and files matching the following globbing patterns:</span></span>
  * <span data-ttu-id="f04f1-261">`**\*.config` (örneğin, *web.config*)</span><span class="sxs-lookup"><span data-stu-id="f04f1-261">`**\*.config` (for example, *web.config*)</span></span>
  * <span data-ttu-id="f04f1-262">`**\*.json` (örneğin, *appsettings.json*)</span><span class="sxs-lookup"><span data-stu-id="f04f1-262">`**\*.json` (for example, *appsettings.json*)</span></span>
  * `wwwroot\**`

<span data-ttu-id="f04f1-263">MSBuild destekler [Glob desenlerinin](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="f04f1-263">MSBuild supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="f04f1-264">Örneğin, aşağıdaki `<Content>` öğesi metnin kopyalanmasını engeller ( *.txt*) dosyalarını *wwwroot\content* klasör ve alt klasörleri:</span><span class="sxs-lookup"><span data-stu-id="f04f1-264">For example, the following `<Content>` element suppresses the copying of text (*.txt*) files in the *wwwroot\content* folder and its subfolders:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="f04f1-265">Önceki işaretleme için bir yayımlama profili eklenebilir veya *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="f04f1-265">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="f04f1-266">Eklenen *.csproj* dosya, kural için eklenir projedeki tüm yayımlama profilleri.</span><span class="sxs-lookup"><span data-stu-id="f04f1-266">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="f04f1-267">Aşağıdaki `<MsDeploySkipRules>` öğe tüm dosyaları dışlar *wwwroot\content* klasörü:</span><span class="sxs-lookup"><span data-stu-id="f04f1-267">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot\content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="f04f1-268">`<MsDeploySkipRules>` silmez *atla* dağıtım sitesinden hedefler.</span><span class="sxs-lookup"><span data-stu-id="f04f1-268">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="f04f1-269">`<Content>` hedef dosyalar ve klasörler dağıtım site veritabanından silinir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-269">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="f04f1-270">Örneğin, aşağıdaki dosyaları dağıtılan web uygulaması olduğu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="f04f1-270">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="f04f1-271">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f04f1-271">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="f04f1-272">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f04f1-272">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="f04f1-273">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f04f1-273">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="f04f1-274">Aşağıdaki `<MsDeploySkipRules>` öğeleri eklenir, dağıtım sitesinde bu dosyaları silseniz mıydı.</span><span class="sxs-lookup"><span data-stu-id="f04f1-274">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="f04f1-275">Önceki `<MsDeploySkipRules>` öğeleri önlemek *atlandı* dosyalarının dağıtılıyor.</span><span class="sxs-lookup"><span data-stu-id="f04f1-275">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="f04f1-276">Bunlar dağıttıktan sonra bu dosyaları silinmez.</span><span class="sxs-lookup"><span data-stu-id="f04f1-276">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="f04f1-277">Aşağıdaki `<Content>` öğesi dağıtım sitede hedeflenen dosyaları siler:</span><span class="sxs-lookup"><span data-stu-id="f04f1-277">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="f04f1-278">Önceki komut satırı dağıtımı kullanarak `<Content>` öğesi çeşitlemesi aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="f04f1-278">Using command-line deployment with the preceding `<Content>` element yields a variation of the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\{TARGET FRAMEWORK MONIKER}\PubTmp\Web1.SourceManifest.
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

## <a name="include-files"></a><span data-ttu-id="f04f1-279">Dosyaları Ekle</span><span class="sxs-lookup"><span data-stu-id="f04f1-279">Include files</span></span>

<span data-ttu-id="f04f1-280">Aşağıdaki bölümlerde anahat farklı yaklaşımlara dosya eklemek için zaman yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="f04f1-280">The following sections outline different approaches for file inclusion at publish time.</span></span> <span data-ttu-id="f04f1-281">[Genel dosya ekleme](#general-file-inclusion) bölümünde kullanan `DotNetPublishFiles` Yayımla hedefleri dosyasında Web SDK'sı tarafından sağlanan öğesi.</span><span class="sxs-lookup"><span data-stu-id="f04f1-281">The [General file inclusion](#general-file-inclusion) section uses the `DotNetPublishFiles` item, which is provided by a publish targets file in the Web SDK.</span></span> <span data-ttu-id="f04f1-282">[Seçici dosya eklemeyi](#selective-file-inclusion) bölümünde kullanır `ResolvedFileToPublish` Yayımla hedefleri dosyasında .NET Core SDK'sı tarafından sağlanan öğesi.</span><span class="sxs-lookup"><span data-stu-id="f04f1-282">The [Selective file inclusion](#selective-file-inclusion) section uses the `ResolvedFileToPublish` item, which is provided by a publish targets file in the .NET Core SDK.</span></span> <span data-ttu-id="f04f1-283">Web SDK'sı üzerinde .NET Core SDK'sı bağlı olduğundan, bir ASP.NET Core projesi içinde her iki öğe kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-283">Because the Web SDK depends on the .NET Core SDK, either item can be used in an ASP.NET Core project.</span></span>

### <a name="general-file-inclusion"></a><span data-ttu-id="f04f1-284">Genel dosya ekleme</span><span class="sxs-lookup"><span data-stu-id="f04f1-284">General file inclusion</span></span>

<span data-ttu-id="f04f1-285">Aşağıdaki örnekteki `<ItemGroup>` öğesi yayımlanmış siteyi klasöre proje dizininin dışında bulunan bir klasöre kopyalama gösterir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-285">The following example's `<ItemGroup>` element demonstrates copying a folder located outside of the project directory to a folder of the published site.</span></span> <span data-ttu-id="f04f1-286">Aşağıdaki biçimlendirme için kullanıcının eklenen dosyaları `<ItemGroup>` varsayılan olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-286">Any files added to the following markup's `<ItemGroup>` are included by default.</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotNetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotNetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="f04f1-287">Önceki işaretlemesi:</span><span class="sxs-lookup"><span data-stu-id="f04f1-287">The preceding markup:</span></span>

* <span data-ttu-id="f04f1-288">Eklenebilir *.csproj* dosya veya yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="f04f1-288">Can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="f04f1-289">Kümeye eklenirse *.csproj* dosyası, onu eklendi projedeki her yayımlama profilinde.</span><span class="sxs-lookup"><span data-stu-id="f04f1-289">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>
* <span data-ttu-id="f04f1-290">Bildiren bir `_CustomFiles` depolamak için öğe dosyaları eşleşen `Include` özniteliğin Glob deseni.</span><span class="sxs-lookup"><span data-stu-id="f04f1-290">Declares a `_CustomFiles` item to store files matching the `Include` attribute's globbing pattern.</span></span> <span data-ttu-id="f04f1-291">*Görüntüleri* düzende başvurulan klasörü, proje dizininin dışında bulunur.</span><span class="sxs-lookup"><span data-stu-id="f04f1-291">The *images* folder referenced in the pattern is located outside of the project directory.</span></span> <span data-ttu-id="f04f1-292">A [ayrılmış özelliği](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties), adlandırılmış `$(MSBuildProjectDirectory)`, proje dosyasının mutlak yolu çözümler.</span><span class="sxs-lookup"><span data-stu-id="f04f1-292">A [reserved property](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties), named `$(MSBuildProjectDirectory)`, resolves to the project file's absolute path.</span></span>
* <span data-ttu-id="f04f1-293">Dosyaları bir listesini sağlar `DotNetPublishFiles` öğesi.</span><span class="sxs-lookup"><span data-stu-id="f04f1-293">Provides a list of files to the `DotNetPublishFiles` item.</span></span> <span data-ttu-id="f04f1-294">Varsayılan olarak, öğenin ait `<DestinationRelativePath>` öğesi boş.</span><span class="sxs-lookup"><span data-stu-id="f04f1-294">By default, the item's `<DestinationRelativePath>` element is empty.</span></span> <span data-ttu-id="f04f1-295">Varsayılan değer kullanır ve biçimlendirme içinde geçersiz [tanınmış öğe meta verileri](/visualstudio/msbuild/msbuild-well-known-item-metadata) gibi `%(RecursiveDir)`.</span><span class="sxs-lookup"><span data-stu-id="f04f1-295">The default value is overridden in the markup and uses [well-known item metadata](/visualstudio/msbuild/msbuild-well-known-item-metadata) such as `%(RecursiveDir)`.</span></span> <span data-ttu-id="f04f1-296">İç metni temsil eder *wwwroot/görüntülerinden* yayımlanmış siteyi klasörü.</span><span class="sxs-lookup"><span data-stu-id="f04f1-296">The inner text represents the *wwwroot/images* folder of the published site.</span></span>

### <a name="selective-file-inclusion"></a><span data-ttu-id="f04f1-297">Seçici dosya ekleme</span><span class="sxs-lookup"><span data-stu-id="f04f1-297">Selective file inclusion</span></span>

<span data-ttu-id="f04f1-298">Aşağıdaki örnekte vurgulanmış biçimlendirmeyi gösterir:</span><span class="sxs-lookup"><span data-stu-id="f04f1-298">The highlighted markup in the following example demonstrates:</span></span>

* <span data-ttu-id="f04f1-299">Projenin dışında yayımlanan site içinde bulunan bir dosya kopyalama *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="f04f1-299">Copying a file located outside of the project into the published site's *wwwroot* folder.</span></span> <span data-ttu-id="f04f1-300">Dosya adını *ReadMe2.md* korunur.</span><span class="sxs-lookup"><span data-stu-id="f04f1-300">The file name of *ReadMe2.md* is maintained.</span></span>
* <span data-ttu-id="f04f1-301">Hariç *wwwroot\Content* klasör.</span><span class="sxs-lookup"><span data-stu-id="f04f1-301">Excluding the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="f04f1-302">Hariç *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f04f1-302">Excluding *Views\Home\About2.cshtml*.</span></span>

[!code-xml[](visual-studio-publish-profiles/samples/Web1.pubxml?highlight=18-23)]

<span data-ttu-id="f04f1-303">Önceki örnekte `ResolvedFileToPublish` , varsayılan davranış, sağlanan dosyaları her zaman Kopyala öğesini `Include` özniteliği için yayımlanmış siteyi.</span><span class="sxs-lookup"><span data-stu-id="f04f1-303">The preceding example uses the `ResolvedFileToPublish` item, whose default behavior is to always copy the files provided in the `Include` attribute to the published site.</span></span> <span data-ttu-id="f04f1-304">Dahil ederek varsayılan davranışın üzerine bir `<CopyToPublishDirectory>` iç metni ya da alt öğesiyle `Never` veya `PreserveNewest`.</span><span class="sxs-lookup"><span data-stu-id="f04f1-304">Override the default behavior by including a `<CopyToPublishDirectory>` child element with inner text of either `Never` or `PreserveNewest`.</span></span> <span data-ttu-id="f04f1-305">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f04f1-305">For example:</span></span>

```xml
<ResolvedFileToPublish Include="..\ReadMe2.md">
  <RelativePath>wwwroot\ReadMe2.md</RelativePath>
  <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
</ResolvedFileToPublish>
```

<span data-ttu-id="f04f1-306">Daha fazla dağıtım örneği için bkz. [Web SDK'sı depoya Benioku](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="f04f1-306">For more deployment samples, see the [Web SDK repository Readme](https://github.com/aspnet/websdk).</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="f04f1-307">Bir hedef önce veya sonra yayımlama çalıştırın</span><span class="sxs-lookup"><span data-stu-id="f04f1-307">Run a target before or after publishing</span></span>

<span data-ttu-id="f04f1-308">Yerleşik `BeforePublish` ve `AfterPublish` hedefleri yürütmenizi hedef önce veya sonra yayımlama hedefi.</span><span class="sxs-lookup"><span data-stu-id="f04f1-308">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="f04f1-309">Yayımlama profili öncesinde ve sonrasında yayımlama konsolu iletilerini günlüğe kaydetmek için aşağıdaki öğeleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f04f1-309">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="f04f1-310">Güvenilmeyen bir sertifika kullanarak bir sunucuda yayımlayın</span><span class="sxs-lookup"><span data-stu-id="f04f1-310">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="f04f1-311">Ekleme `<AllowUntrustedCertificate>` özellik değeriyle `True` yayımlama profili için:</span><span class="sxs-lookup"><span data-stu-id="f04f1-311">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="f04f1-312">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="f04f1-312">The Kudu service</span></span>

<span data-ttu-id="f04f1-313">Bir Azure App Service web uygulaması dağıtımı ' dosyaları görüntülemek için kullanın [Kudu hizmet](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="f04f1-313">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="f04f1-314">Append `scm` belirteç için web uygulaması adı.</span><span class="sxs-lookup"><span data-stu-id="f04f1-314">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="f04f1-315">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f04f1-315">For example:</span></span>

| <span data-ttu-id="f04f1-316">URL</span><span class="sxs-lookup"><span data-stu-id="f04f1-316">URL</span></span>                                    | <span data-ttu-id="f04f1-317">Sonuç</span><span class="sxs-lookup"><span data-stu-id="f04f1-317">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="f04f1-318">Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="f04f1-318">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="f04f1-319">Kudu hizmeti</span><span class="sxs-lookup"><span data-stu-id="f04f1-319">Kudu service</span></span> |

<span data-ttu-id="f04f1-320">Seçin [hata ayıklama konsolunu](https://github.com/projectkudu/kudu/wiki/Kudu-console) görüntülemek, düzenlemek, silmek veya dosya eklemek için menü öğesi.</span><span class="sxs-lookup"><span data-stu-id="f04f1-320">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f04f1-321">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f04f1-321">Additional resources</span></span>

* <span data-ttu-id="f04f1-322">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) web uygulamaları ve IIS sunucuları için Web siteleri dağıtımı (MSDeploy) basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="f04f1-322">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="f04f1-323">[Web SDK'sı GitHub deposu](https://github.com/aspnet/websdk/issues): Dosya sorunları ve istek için dağıtım özellikleri.</span><span class="sxs-lookup"><span data-stu-id="f04f1-323">[Web SDK GitHub repository](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="f04f1-324">Visual Studio'dan Azure VM için bir ASP.NET Web uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="f04f1-324">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
