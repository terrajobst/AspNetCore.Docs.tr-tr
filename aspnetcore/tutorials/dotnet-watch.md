---
title: Dosya İzleyici kullanarak ASP.NET Core uygulamaları geliştirme
author: rick-anderson
description: Bu öğretici, aracı yükleme ve .NET Core CLI dosya İzleyici (dotnet izleme) bir ASP.NET Core uygulamada kullanma gösterilmektedir.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: 2a59267b36faf1e00ea2f0cc7e2b9ceb9828f791
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278859"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="b23ed-103">Dosya İzleyici kullanarak ASP.NET Core uygulamaları geliştirme</span><span class="sxs-lookup"><span data-stu-id="b23ed-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="b23ed-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [kazanan Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="b23ed-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="b23ed-105">`dotnet watch` çalıştıran bir aracıdır bir [.NET Core CLI](/dotnet/core/tools) değişiklik kaynak dosyaları, komut.</span><span class="sxs-lookup"><span data-stu-id="b23ed-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="b23ed-106">Örneğin, bir dosya değişikliği derleme, test yürütmesi veya dağıtım tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="b23ed-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="b23ed-107">Bu öğretici varolan bir web API iki uç nokta ile kullanır: biri toplam ve bir ürün döndüren bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="b23ed-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="b23ed-108">Ürün yöntemi Bu öğreticide sabit bir hata var.</span><span class="sxs-lookup"><span data-stu-id="b23ed-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="b23ed-109">Karşıdan [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="b23ed-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="b23ed-110">İki projeden oluşan: *WebApp* (bir ASP.NET Core web API) ve *WebAppTests* (web API'si için birim testleri).</span><span class="sxs-lookup"><span data-stu-id="b23ed-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="b23ed-111">Komut kabuğu'na gidin *WebApp* klasör.</span><span class="sxs-lookup"><span data-stu-id="b23ed-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="b23ed-112">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b23ed-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="b23ed-113">Konsol çıktısı aşağıdakine benzer mesajlarını (uygulama çalıştıran ve istekleri bekleyen gösteren):</span><span class="sxs-lookup"><span data-stu-id="b23ed-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="b23ed-114">Bir web tarayıcısında gidin `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="b23ed-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="b23ed-115">Sonucu görmelisiniz `9`.</span><span class="sxs-lookup"><span data-stu-id="b23ed-115">You should see the result of `9`.</span></span>

<span data-ttu-id="b23ed-116">Ürüne API gidin (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="b23ed-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="b23ed-117">Döndürdüğü `9`değil `20` beklediğiniz gibi.</span><span class="sxs-lookup"><span data-stu-id="b23ed-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="b23ed-118">Öğreticide daha sonra bu sorun düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b23ed-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="b23ed-119">Ekleme `dotnet watch` bir proje</span><span class="sxs-lookup"><span data-stu-id="b23ed-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="b23ed-120">`dotnet watch` Dosya İzleyici aracı .NET Core SDK 2.1.300 sürümü ile eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="b23ed-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="b23ed-121">Aşağıdaki adımlar .NET Core SDK'ın önceki bir sürümünü kullanırken gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="b23ed-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="b23ed-122">Ekleme bir `Microsoft.DotNet.Watcher.Tools` paketini referansı *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b23ed-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="b23ed-123">Yükleme `Microsoft.DotNet.Watcher.Tools` aşağıdaki komutu çalıştırarak paketi:</span><span class="sxs-lookup"><span data-stu-id="b23ed-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="b23ed-124">.NET Core CLI komutları kullanarak çalıştırın `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="b23ed-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="b23ed-125">Tüm [.NET Core CLI komutu](/dotnet/core/tools#cli-commands) ile çalıştırılabilir `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="b23ed-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="b23ed-126">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b23ed-126">For example:</span></span>

| <span data-ttu-id="b23ed-127">Komut</span><span class="sxs-lookup"><span data-stu-id="b23ed-127">Command</span></span> | <span data-ttu-id="b23ed-128">İzleme komut</span><span class="sxs-lookup"><span data-stu-id="b23ed-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="b23ed-129">dotnet çalıştırın</span><span class="sxs-lookup"><span data-stu-id="b23ed-129">dotnet run</span></span> | <span data-ttu-id="b23ed-130">dotnet izleme çalıştırın</span><span class="sxs-lookup"><span data-stu-id="b23ed-130">dotnet watch run</span></span> |
| <span data-ttu-id="b23ed-131">dotnet -f netcoreapp2.0 çalıştırın</span><span class="sxs-lookup"><span data-stu-id="b23ed-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="b23ed-132">-f netcoreapp2.0 çalıştırmak dotnet izleme</span><span class="sxs-lookup"><span data-stu-id="b23ed-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="b23ed-133">-f netcoreapp2.0--çalıştırmak dotnet--arg1</span><span class="sxs-lookup"><span data-stu-id="b23ed-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="b23ed-134">-f netcoreapp2.0--çalıştırmak dotnet izleme--arg1</span><span class="sxs-lookup"><span data-stu-id="b23ed-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="b23ed-135">DotNet test</span><span class="sxs-lookup"><span data-stu-id="b23ed-135">dotnet test</span></span> | <span data-ttu-id="b23ed-136">DotNet izleme test</span><span class="sxs-lookup"><span data-stu-id="b23ed-136">dotnet watch test</span></span> |

<span data-ttu-id="b23ed-137">Çalıştırma `dotnet watch run` içinde *WebApp* klasör.</span><span class="sxs-lookup"><span data-stu-id="b23ed-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="b23ed-138">Konsol çıktısı gösterir `watch` başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="b23ed-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="b23ed-139">İle değişiklik `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="b23ed-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="b23ed-140">Emin olun `dotnet watch` çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="b23ed-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="b23ed-141">Hata düzeltme `Product` yöntemi *MathController.cs* ürünü ve toplam döndürecek şekilde:</span><span class="sxs-lookup"><span data-stu-id="b23ed-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="b23ed-142">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="b23ed-142">Save the file.</span></span> <span data-ttu-id="b23ed-143">Konsol çıktısı belirten `dotnet watch` bir dosya değişiklik algılandı ve uygulamayı yeniden.</span><span class="sxs-lookup"><span data-stu-id="b23ed-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="b23ed-144">Doğrulama `http://localhost:<port number>/api/math/product?a=4&b=5` doğru sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="b23ed-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="b23ed-145">Kullanarak testleri çalıştırma `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="b23ed-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="b23ed-146">Değişiklik `Product` yöntemi *MathController.cs* toplamı döndürme geri dön.</span><span class="sxs-lookup"><span data-stu-id="b23ed-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="b23ed-147">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="b23ed-147">Save the file.</span></span>
1. <span data-ttu-id="b23ed-148">Komut kabuğu'na gidin *WebAppTests* klasör.</span><span class="sxs-lookup"><span data-stu-id="b23ed-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="b23ed-149">Çalıştırma [dotnet geri yükleme](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="b23ed-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="b23ed-150">Çalıştırma `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="b23ed-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="b23ed-151">Bir test başarısız olduğunu ve İzleyici dosya değişiklikleri bekliyor çıktısını gösterir:</span><span class="sxs-lookup"><span data-stu-id="b23ed-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="b23ed-152">Düzeltme `Product` yöntemi kod ürün döndürecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="b23ed-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="b23ed-153">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="b23ed-153">Save the file.</span></span>

<span data-ttu-id="b23ed-154">`dotnet watch` dosya değişikliği algılar ve testleri yeniden çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="b23ed-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="b23ed-155">Konsol çıktısı testleri geçirilen gösterir.</span><span class="sxs-lookup"><span data-stu-id="b23ed-155">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="b23ed-156">İzlemek için dosya listesini Özelleştir</span><span class="sxs-lookup"><span data-stu-id="b23ed-156">Customize files list to watch</span></span>

<span data-ttu-id="b23ed-157">Varsayılan olarak, `dotnet-watch` aşağıdaki glob desenleri eşleşen tüm dosyaları izler:</span><span class="sxs-lookup"><span data-stu-id="b23ed-157">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="b23ed-158">Daha fazla öğe düzenleyerek izleme listesine eklenebilir *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="b23ed-158">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="b23ed-159">Öğeleri tek tek veya glob desenler kullanılarak belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="b23ed-159">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="b23ed-160">İzlemeniz gereken dosyaları çevirin</span><span class="sxs-lookup"><span data-stu-id="b23ed-160">Opt-out of files to be watched</span></span>

<span data-ttu-id="b23ed-161">`dotnet-watch` varsayılan ayarlarını yoksaymak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b23ed-161">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="b23ed-162">Belirli dosyaları yoksaymak için ekleme `Watch="false"` öğe tanımında özniteliğini *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b23ed-162">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

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

## <a name="custom-watch-projects"></a><span data-ttu-id="b23ed-163">Özel İzleme projeleri</span><span class="sxs-lookup"><span data-stu-id="b23ed-163">Custom watch projects</span></span>

<span data-ttu-id="b23ed-164">`dotnet-watch` C# projeleri için sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="b23ed-164">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="b23ed-165">Özel İzleme projeleri farklı senaryolar işlemek için oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="b23ed-165">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="b23ed-166">Aşağıdaki proje düzeni göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="b23ed-166">Consider the following project layout:</span></span>

* <span data-ttu-id="b23ed-167">**Test /**</span><span class="sxs-lookup"><span data-stu-id="b23ed-167">**test/**</span></span>
  * <span data-ttu-id="b23ed-168">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="b23ed-168">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="b23ed-169">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="b23ed-169">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="b23ed-170">Her iki proje izlemek için hedef ise, her iki proje izlemek için yapılandırılmış bir özel proje dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b23ed-170">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

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

<span data-ttu-id="b23ed-171">Her iki projelerde izleme dosyasını başlatmak için değiştirin *test* klasör.</span><span class="sxs-lookup"><span data-stu-id="b23ed-171">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="b23ed-172">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="b23ed-172">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="b23ed-173">Herhangi bir dosya ya da test projesinde değiştiğinde VSTest yürütür.</span><span class="sxs-lookup"><span data-stu-id="b23ed-173">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="b23ed-174">`dotnet-watch` Github'da</span><span class="sxs-lookup"><span data-stu-id="b23ed-174">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="b23ed-175">`dotnet-watch` GitHub parçası olan [DotNetTools depo](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="b23ed-175">`dotnet-watch` is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>
