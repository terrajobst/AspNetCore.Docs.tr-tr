---
title: Bir dosya İzleyicisi'ni kullanarak ASP.NET Core uygulamaları geliştirin
author: rick-anderson
description: Bu öğreticide, aracı yükleme ve .NET Core CLI'ın dosya İzleyicisi (dotnet watch) ASP.NET Core uygulaması kullanma gösterilmektedir.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: fc08efa433f688a0b9009aed35fdee2b0c228619
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063305"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="62778-103">Bir dosya İzleyicisi'ni kullanarak ASP.NET Core uygulamaları geliştirin</span><span class="sxs-lookup"><span data-stu-id="62778-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="62778-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="62778-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="62778-105">`dotnet watch` çalıştıran bir aracıdır bir [.NET Core CLI](/dotnet/core/tools) değişikliği kaynak dosyaları komutu.</span><span class="sxs-lookup"><span data-stu-id="62778-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="62778-106">Örneğin, derleme, test yürütme ya da dağıtımı dosya değişikliği tetikleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="62778-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="62778-107">Bu öğreticide iki uç nokta ile mevcut bir web API'si:, toplam ve bir ürün döndüren bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="62778-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="62778-108">Bu öğreticide sabit bir hata, ürün yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="62778-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="62778-109">İndirme [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="62778-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="62778-110">İki projeden oluşan: *WebApp* (bir ASP.NET Core web API'si) ve *WebAppTests* (web API'si için birim testleri).</span><span class="sxs-lookup"><span data-stu-id="62778-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="62778-111">Komut kabuğu'na gidin *WebApp* klasör.</span><span class="sxs-lookup"><span data-stu-id="62778-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="62778-112">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="62778-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="62778-113">Konsol çıktısı aşağıdakine benzer iletiler gösterir (uygulama çalıştıran ve istekleri bekleyen gösterir):</span><span class="sxs-lookup"><span data-stu-id="62778-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="62778-114">Bir web tarayıcısında gidin `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="62778-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="62778-115">Sonucu görmeniz gerekir `9`.</span><span class="sxs-lookup"><span data-stu-id="62778-115">You should see the result of `9`.</span></span>

<span data-ttu-id="62778-116">Ürüne API gidin (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="62778-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="62778-117">Döndürür `9`değil `20` beklediğiniz gibi.</span><span class="sxs-lookup"><span data-stu-id="62778-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="62778-118">Öğreticide daha sonra bu sorun düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="62778-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="62778-119">Ekleme `dotnet watch` projeye</span><span class="sxs-lookup"><span data-stu-id="62778-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="62778-120">`dotnet watch` Dosya İzleyici Aracı sürümüyle .NET Core SDK'sını 2.1.300 dahildir.</span><span class="sxs-lookup"><span data-stu-id="62778-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="62778-121">Aşağıdaki adımlar, .NET Core SDK'ın önceki bir sürümünü kullanırken gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="62778-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="62778-122">Ekleme bir `Microsoft.DotNet.Watcher.Tools` paketini başvuru *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="62778-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="62778-123">Yükleme `Microsoft.DotNet.Watcher.Tools` aşağıdaki komutu çalıştırarak paketi:</span><span class="sxs-lookup"><span data-stu-id="62778-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="62778-124">.NET Core CLI komutları kullanarak çalıştırın `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="62778-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="62778-125">Tüm [.NET Core CLI komutunu](/dotnet/core/tools#cli-commands) ile çalıştırılabilir `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="62778-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="62778-126">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="62778-126">For example:</span></span>

| <span data-ttu-id="62778-127">Komut</span><span class="sxs-lookup"><span data-stu-id="62778-127">Command</span></span> | <span data-ttu-id="62778-128">İzleme komutu</span><span class="sxs-lookup"><span data-stu-id="62778-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="62778-129">dotnet çalıştırın</span><span class="sxs-lookup"><span data-stu-id="62778-129">dotnet run</span></span> | <span data-ttu-id="62778-130">dotnet watch çalıştırın</span><span class="sxs-lookup"><span data-stu-id="62778-130">dotnet watch run</span></span> |
| <span data-ttu-id="62778-131">dotnet -f netcoreapp2.0 çalıştırın</span><span class="sxs-lookup"><span data-stu-id="62778-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="62778-132">dotnet izleyin -f netcoreapp2.0 çalıştırın</span><span class="sxs-lookup"><span data-stu-id="62778-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="62778-133">dotnet -f netcoreapp2.0--çalıştırma--arg1</span><span class="sxs-lookup"><span data-stu-id="62778-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="62778-134">dotnet izleyin -f netcoreapp2.0--çalıştırma--arg1</span><span class="sxs-lookup"><span data-stu-id="62778-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="62778-135">DotNet testi</span><span class="sxs-lookup"><span data-stu-id="62778-135">dotnet test</span></span> | <span data-ttu-id="62778-136">DotNet izleme testi</span><span class="sxs-lookup"><span data-stu-id="62778-136">dotnet watch test</span></span> |

<span data-ttu-id="62778-137">Çalıştırma `dotnet watch run` içinde *WebApp* klasör.</span><span class="sxs-lookup"><span data-stu-id="62778-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="62778-138">Konsol çıkışını gösterir `watch` başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="62778-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="62778-139">İle değişiklik `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="62778-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="62778-140">Emin `dotnet watch` çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="62778-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="62778-141">Hatayı düzeltmek `Product` yöntemi *MathController.cs* ürün ve toplamı döndürecek şekilde:</span><span class="sxs-lookup"><span data-stu-id="62778-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="62778-142">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="62778-142">Save the file.</span></span> <span data-ttu-id="62778-143">Konsol çıkışını gösterir `dotnet watch` dosya değişikliği algılandı ve uygulamayı yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="62778-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="62778-144">Doğrulama `http://localhost:<port number>/api/math/product?a=4&b=5` doğru sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="62778-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="62778-145">Kullanarak testleri çalıştırma `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="62778-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="62778-146">Değişiklik `Product` yöntemi *MathController.cs* toplamını döndüren geri dönün.</span><span class="sxs-lookup"><span data-stu-id="62778-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="62778-147">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="62778-147">Save the file.</span></span>
1. <span data-ttu-id="62778-148">Komut kabuğu'na gidin *WebAppTests* klasör.</span><span class="sxs-lookup"><span data-stu-id="62778-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="62778-149">Çalıştırma [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="62778-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="62778-150">Çalıştırma `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="62778-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="62778-151">Bir testin başarısız olduğunu ve İzleyici dosya değişiklikleri bekliyor çıktısını gösterir:</span><span class="sxs-lookup"><span data-stu-id="62778-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="62778-152">Düzeltme `Product` yöntemi kod ürün döndürecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="62778-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="62778-153">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="62778-153">Save the file.</span></span>

<span data-ttu-id="62778-154">`dotnet watch` dosya değişikliği algılar ve testleri yeniden çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="62778-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="62778-155">Konsol çıkışını sınamalarını geçtiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="62778-155">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="62778-156">İzlemek için dosyaları listesini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="62778-156">Customize files list to watch</span></span>

<span data-ttu-id="62778-157">Varsayılan olarak, `dotnet-watch` aşağıdaki glob desenlerle eşleşen tüm dosyaları izler:</span><span class="sxs-lookup"><span data-stu-id="62778-157">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="62778-158">Daha fazla öğe düzenleyerek izleme listesine eklenebilir *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="62778-158">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="62778-159">Öğeleri tek tek veya glob desenleri kullanılarak belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="62778-159">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="62778-160">İzlemeniz gereken dosyaların çevirme</span><span class="sxs-lookup"><span data-stu-id="62778-160">Opt-out of files to be watched</span></span>

<span data-ttu-id="62778-161">`dotnet-watch` varsayılan ayarlarını yok saymak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="62778-161">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="62778-162">Belirli dosyaları yoksaymak için ekleme `Watch="false"` öznitelik bir öğenin tanımına *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="62778-162">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a><span data-ttu-id="62778-163">Özel İzleme projeleri</span><span class="sxs-lookup"><span data-stu-id="62778-163">Custom watch projects</span></span>

<span data-ttu-id="62778-164">`dotnet-watch` C# projeleri için sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="62778-164">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="62778-165">Özel İzleme projeleri farklı senaryolar işlemek için oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="62778-165">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="62778-166">Aşağıdaki proje düzeni göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="62778-166">Consider the following project layout:</span></span>

* <span data-ttu-id="62778-167">**Test /**</span><span class="sxs-lookup"><span data-stu-id="62778-167">**test/**</span></span>
  * <span data-ttu-id="62778-168">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="62778-168">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="62778-169">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="62778-169">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="62778-170">Hedefi, her iki proje izlemek için ise, her iki proje izleme için yapılandırılmış bir özel proje dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="62778-170">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

<span data-ttu-id="62778-171">Dosya hem projelerde izlemeye başlamak için değiştirme *test* klasör.</span><span class="sxs-lookup"><span data-stu-id="62778-171">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="62778-172">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="62778-172">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="62778-173">Herhangi bir dosya ya da test projesinde değiştiğinde VSTest yürütür.</span><span class="sxs-lookup"><span data-stu-id="62778-173">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="62778-174">`dotnet-watch` Github'da</span><span class="sxs-lookup"><span data-stu-id="62778-174">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="62778-175">`dotnet-watch` GitHub parçasıdır [DotNetTools depo](https://github.com/aspnet/DotNetTools/tree/master/src/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="62778-175">`dotnet-watch` is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/master/src/dotnet-watch).</span></span>
