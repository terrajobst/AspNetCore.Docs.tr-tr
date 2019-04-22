---
title: Bir dosya İzleyicisi'ni kullanarak ASP.NET Core uygulamaları geliştirin
author: rick-anderson
description: Bu öğreticide, aracı yükleme ve .NET Core CLI'ın dosya İzleyicisi (dotnet watch) ASP.NET Core uygulaması kullanma gösterilmektedir.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: 40ecca1c6f9d519b24649d0c28946d95b820c07c
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068202"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="cd195-103">Bir dosya İzleyicisi'ni kullanarak ASP.NET Core uygulamaları geliştirin</span><span class="sxs-lookup"><span data-stu-id="cd195-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="cd195-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="cd195-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="cd195-105">`dotnet watch` çalıştıran bir aracıdır bir [.NET Core CLI](/dotnet/core/tools) değişikliği kaynak dosyaları komutu.</span><span class="sxs-lookup"><span data-stu-id="cd195-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="cd195-106">Örneğin, derleme, test yürütme ya da dağıtımı dosya değişikliği tetikleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd195-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="cd195-107">Bu öğreticide iki uç nokta ile mevcut bir web API'si:, toplam ve bir ürün döndüren bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="cd195-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="cd195-108">Bu öğreticide sabit bir hata, ürün yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="cd195-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="cd195-109">İndirme [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="cd195-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="cd195-110">Bu iki projeden oluşan: *WebApp* (bir ASP.NET Core web API'si) ve *WebAppTests* (web API'si için birim testleri).</span><span class="sxs-lookup"><span data-stu-id="cd195-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="cd195-111">Komut kabuğu'na gidin *WebApp* klasör.</span><span class="sxs-lookup"><span data-stu-id="cd195-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="cd195-112">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cd195-112">Run the following command:</span></span>

```console
dotnet run
```

> [!NOTE]
> <span data-ttu-id="cd195-113">Kullanabileceğiniz `dotnet run --project <PROJECT>` çalıştırmak için bir projeyi belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="cd195-113">You can use `dotnet run --project <PROJECT>` to specify a project to run.</span></span> <span data-ttu-id="cd195-114">Örneğin, çalışan `dotnet run --project WebApp` örnek kökünden uygulama da çalıştırılır *WebApp* proje.</span><span class="sxs-lookup"><span data-stu-id="cd195-114">For example, running `dotnet run --project WebApp` from the root of the sample app will also run the *WebApp* project.</span></span>

<span data-ttu-id="cd195-115">Konsol çıktısı aşağıdakine benzer iletiler gösterir (uygulama çalıştıran ve istekleri bekleyen gösterir):</span><span class="sxs-lookup"><span data-stu-id="cd195-115">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="cd195-116">Bir web tarayıcısında gidin `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="cd195-116">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="cd195-117">Sonucu görmeniz gerekir `9`.</span><span class="sxs-lookup"><span data-stu-id="cd195-117">You should see the result of `9`.</span></span>

<span data-ttu-id="cd195-118">Ürüne API gidin (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="cd195-118">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="cd195-119">Döndürür `9`değil `20` beklediğiniz gibi.</span><span class="sxs-lookup"><span data-stu-id="cd195-119">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="cd195-120">Öğreticide daha sonra bu sorun düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cd195-120">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="cd195-121">Ekleme `dotnet watch` projeye</span><span class="sxs-lookup"><span data-stu-id="cd195-121">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="cd195-122">`dotnet watch` Dosya İzleyici Aracı sürümüyle .NET Core SDK'sını 2.1.300 dahildir.</span><span class="sxs-lookup"><span data-stu-id="cd195-122">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="cd195-123">Aşağıdaki adımlar, .NET Core SDK'ın önceki bir sürümünü kullanırken gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="cd195-123">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="cd195-124">Ekleme bir `Microsoft.DotNet.Watcher.Tools` paketini başvuru *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cd195-124">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="cd195-125">Yükleme `Microsoft.DotNet.Watcher.Tools` aşağıdaki komutu çalıştırarak paketi:</span><span class="sxs-lookup"><span data-stu-id="cd195-125">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="cd195-126">.NET Core CLI komutları kullanarak çalıştırın `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="cd195-126">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="cd195-127">Tüm [.NET Core CLI komutunu](/dotnet/core/tools#cli-commands) ile çalıştırılabilir `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="cd195-127">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="cd195-128">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cd195-128">For example:</span></span>

| <span data-ttu-id="cd195-129">Komut</span><span class="sxs-lookup"><span data-stu-id="cd195-129">Command</span></span> | <span data-ttu-id="cd195-130">İzleme komutu</span><span class="sxs-lookup"><span data-stu-id="cd195-130">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="cd195-131">dotnet çalıştırın</span><span class="sxs-lookup"><span data-stu-id="cd195-131">dotnet run</span></span> | <span data-ttu-id="cd195-132">dotnet watch çalıştırın</span><span class="sxs-lookup"><span data-stu-id="cd195-132">dotnet watch run</span></span> |
| <span data-ttu-id="cd195-133">dotnet -f netcoreapp2.0 çalıştırın</span><span class="sxs-lookup"><span data-stu-id="cd195-133">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="cd195-134">dotnet izleyin -f netcoreapp2.0 çalıştırın</span><span class="sxs-lookup"><span data-stu-id="cd195-134">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="cd195-135">dotnet -f netcoreapp2.0--çalıştırma--arg1</span><span class="sxs-lookup"><span data-stu-id="cd195-135">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="cd195-136">dotnet izleyin -f netcoreapp2.0--çalıştırma--arg1</span><span class="sxs-lookup"><span data-stu-id="cd195-136">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="cd195-137">DotNet testi</span><span class="sxs-lookup"><span data-stu-id="cd195-137">dotnet test</span></span> | <span data-ttu-id="cd195-138">DotNet izleme testi</span><span class="sxs-lookup"><span data-stu-id="cd195-138">dotnet watch test</span></span> |

<span data-ttu-id="cd195-139">Çalıştırma `dotnet watch run` içinde *WebApp* klasör.</span><span class="sxs-lookup"><span data-stu-id="cd195-139">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="cd195-140">Konsol çıkışını gösterir `watch` başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="cd195-140">The console output indicates `watch` has started.</span></span>

> [!NOTE]
> <span data-ttu-id="cd195-141">Kullanabileceğiniz `dotnet watch --project <PROJECT>` izlemek için bir projeyi belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="cd195-141">You can use `dotnet watch --project <PROJECT>` to specify a project to watch.</span></span> <span data-ttu-id="cd195-142">Örneğin, çalışan `dotnet watch --project WebApp run` örnek kökünden uygulama de izleyin ve çalıştırılır *WebApp* proje.</span><span class="sxs-lookup"><span data-stu-id="cd195-142">For example, running `dotnet watch --project WebApp run` from the root of the sample app will also run and watch the *WebApp* project.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="cd195-143">İle değişiklik `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="cd195-143">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="cd195-144">Emin `dotnet watch` çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="cd195-144">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="cd195-145">Hatayı düzeltmek `Product` yöntemi *MathController.cs* ürün ve toplamı döndürecek şekilde:</span><span class="sxs-lookup"><span data-stu-id="cd195-145">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
    return a * b;
}
```

<span data-ttu-id="cd195-146">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="cd195-146">Save the file.</span></span> <span data-ttu-id="cd195-147">Konsol çıkışını gösterir `dotnet watch` dosya değişikliği algılandı ve uygulamayı yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="cd195-147">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="cd195-148">Doğrulama `http://localhost:<port number>/api/math/product?a=4&b=5` doğru sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="cd195-148">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="cd195-149">Kullanarak testleri çalıştırma `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="cd195-149">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="cd195-150">Değişiklik `Product` yöntemi *MathController.cs* toplamını döndüren geri dönün.</span><span class="sxs-lookup"><span data-stu-id="cd195-150">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="cd195-151">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="cd195-151">Save the file.</span></span>
1. <span data-ttu-id="cd195-152">Komut kabuğu'na gidin *WebAppTests* klasör.</span><span class="sxs-lookup"><span data-stu-id="cd195-152">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="cd195-153">Çalıştırma [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="cd195-153">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="cd195-154">`dotnet watch test`'i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cd195-154">Run `dotnet watch test`.</span></span> <span data-ttu-id="cd195-155">Bir testin başarısız olduğunu ve İzleyici dosya değişiklikleri bekliyor çıktısını gösterir:</span><span class="sxs-lookup"><span data-stu-id="cd195-155">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="cd195-156">Düzeltme `Product` yöntemi kod ürün döndürecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="cd195-156">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="cd195-157">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="cd195-157">Save the file.</span></span>

<span data-ttu-id="cd195-158">`dotnet watch` dosya değişikliği algılar ve testleri yeniden çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="cd195-158">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="cd195-159">Konsol çıkışını sınamalarını geçtiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cd195-159">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="cd195-160">İzlemek için dosyaları listesini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="cd195-160">Customize files list to watch</span></span>

<span data-ttu-id="cd195-161">Varsayılan olarak, `dotnet-watch` aşağıdaki glob desenlerle eşleşen tüm dosyaları izler:</span><span class="sxs-lookup"><span data-stu-id="cd195-161">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="cd195-162">Daha fazla öğe düzenleyerek izleme listesine eklenebilir *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="cd195-162">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="cd195-163">Öğeleri tek tek veya glob desenleri kullanılarak belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="cd195-163">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="cd195-164">İzlemeniz gereken dosyaların çevirme</span><span class="sxs-lookup"><span data-stu-id="cd195-164">Opt-out of files to be watched</span></span>

<span data-ttu-id="cd195-165">`dotnet-watch` varsayılan ayarlarını yok saymak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="cd195-165">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="cd195-166">Belirli dosyaları yoksaymak için ekleme `Watch="false"` öznitelik bir öğenin tanımına *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cd195-166">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

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

## <a name="custom-watch-projects"></a><span data-ttu-id="cd195-167">Özel İzleme projeleri</span><span class="sxs-lookup"><span data-stu-id="cd195-167">Custom watch projects</span></span>

<span data-ttu-id="cd195-168">`dotnet-watch` C# projeleri için sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="cd195-168">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="cd195-169">Özel İzleme projeleri farklı senaryolar işlemek için oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="cd195-169">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="cd195-170">Aşağıdaki proje düzeni göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="cd195-170">Consider the following project layout:</span></span>

* <span data-ttu-id="cd195-171">**Test /**</span><span class="sxs-lookup"><span data-stu-id="cd195-171">**test/**</span></span>
  * <span data-ttu-id="cd195-172">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="cd195-172">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="cd195-173">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="cd195-173">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="cd195-174">Hedefi, her iki proje izlemek için ise, her iki proje izleme için yapılandırılmış bir özel proje dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="cd195-174">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

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

<span data-ttu-id="cd195-175">Dosya hem projelerde izlemeye başlamak için değiştirme *test* klasör.</span><span class="sxs-lookup"><span data-stu-id="cd195-175">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="cd195-176">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="cd195-176">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="cd195-177">Herhangi bir dosya ya da test projesinde değiştiğinde VSTest yürütür.</span><span class="sxs-lookup"><span data-stu-id="cd195-177">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="cd195-178">`dotnet-watch` Github'da</span><span class="sxs-lookup"><span data-stu-id="cd195-178">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="cd195-179">`dotnet-watch` GitHub parçasıdır [aspnet/AspNetCore depo](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="cd195-179">`dotnet-watch` is part of the GitHub [aspnet/AspNetCore repository](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span></span>
