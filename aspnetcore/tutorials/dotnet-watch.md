---
title: Dosya İzleyicisi kullanarak ASP.NET Core uygulamalar geliştirme
author: rick-anderson
description: Bu öğreticide, .NET Core CLI dosya İzleyicisi (DotNet Watch) aracının bir ASP.NET Core uygulamasında nasıl yükleneceği ve kullanılacağı gösterilmektedir.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: bedb3e6a65839db915ca7bc821a267a14d34bf30
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667414"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="8dc29-103">Dosya İzleyicisi kullanarak ASP.NET Core uygulamalar geliştirme</span><span class="sxs-lookup"><span data-stu-id="8dc29-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="8dc29-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="8dc29-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="8dc29-105">[DotNet Watch](https://www.nuget.org/packages/dotnet-watch) , kaynak dosyalar değiştiğinde [.NET Core CLI](/dotnet/core/tools) komutu çalıştıran bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="8dc29-105">[dotnet watch](https://www.nuget.org/packages/dotnet-watch) is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="8dc29-106">Örneğin, bir dosya değişikliği derleme, test yürütmesi veya dağıtımı tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="8dc29-107">Bu öğretici, iki uç nokta ile mevcut bir Web API 'SI kullanır: bir toplamı ve bir ürünü döndüren bir tane döndürür.</span><span class="sxs-lookup"><span data-stu-id="8dc29-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="8dc29-108">Ürün yönteminde, bu öğreticide düzeltilen bir hata vardır.</span><span class="sxs-lookup"><span data-stu-id="8dc29-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="8dc29-109">[Örnek uygulamayı](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)indirin.</span><span class="sxs-lookup"><span data-stu-id="8dc29-109">Download the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="8dc29-110">İki projeden oluşur: *WebApp* (bir ASP.NET Core Web API 'si) ve *webapptests* (Web API 'si için birim testleri).</span><span class="sxs-lookup"><span data-stu-id="8dc29-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="8dc29-111">Bir komut kabuğu 'nda *WebApp* klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="8dc29-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="8dc29-112">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8dc29-112">Run the following command:</span></span>

```dotnetcli
dotnet run
```

> [!NOTE]
> <span data-ttu-id="8dc29-113">Çalıştırmak için bir proje belirtmek üzere `dotnet run --project <PROJECT>` kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8dc29-113">You can use `dotnet run --project <PROJECT>` to specify a project to run.</span></span> <span data-ttu-id="8dc29-114">Örneğin, örnek uygulamanın kökünden `dotnet run --project WebApp` çalıştırmak *WebApp* projesini de çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="8dc29-114">For example, running `dotnet run --project WebApp` from the root of the sample app will also run the *WebApp* project.</span></span>

<span data-ttu-id="8dc29-115">Konsol çıktısı aşağıdakine benzer iletileri gösterir (uygulamanın çalıştığını ve istekleri beklediğini gösterir):</span><span class="sxs-lookup"><span data-stu-id="8dc29-115">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="8dc29-116">Bir Web tarayıcısında `http://localhost:<port number>/api/math/sum?a=4&b=5`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="8dc29-116">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="8dc29-117">`9`sonucunu görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-117">You should see the result of `9`.</span></span>

<span data-ttu-id="8dc29-118">Ürün API 'sine gidin (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="8dc29-118">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="8dc29-119">Beklendiğinde `20` değil `9`döndürür.</span><span class="sxs-lookup"><span data-stu-id="8dc29-119">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="8dc29-120">Bu sorun öğreticide daha sonra düzeltildi.</span><span class="sxs-lookup"><span data-stu-id="8dc29-120">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="8dc29-121">Projeye `dotnet watch` ekleme</span><span class="sxs-lookup"><span data-stu-id="8dc29-121">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="8dc29-122">`dotnet watch` dosya İzleyici Aracı, .NET Core SDK sürüm 2.1.300 eklenir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-122">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="8dc29-123">.NET Core SDK önceki bir sürümü kullanılırken aşağıdaki adımlar gereklidir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-123">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="8dc29-124">*. Csproj* dosyasına bir `Microsoft.DotNet.Watcher.Tools` paket başvurusu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8dc29-124">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="8dc29-125">Aşağıdaki komutu çalıştırarak `Microsoft.DotNet.Watcher.Tools` paketini yüklersiniz:</span><span class="sxs-lookup"><span data-stu-id="8dc29-125">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```dotnetcli
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="8dc29-126">`dotnet watch` kullanarak .NET Core CLI komutları çalıştırma</span><span class="sxs-lookup"><span data-stu-id="8dc29-126">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="8dc29-127">Tüm [.NET Core CLI komutları](/dotnet/core/tools#cli-commands) `dotnet watch`çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-127">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="8dc29-128">Örnek:</span><span class="sxs-lookup"><span data-stu-id="8dc29-128">For example:</span></span>

| <span data-ttu-id="8dc29-129">Komut</span><span class="sxs-lookup"><span data-stu-id="8dc29-129">Command</span></span> | <span data-ttu-id="8dc29-130">İzle komutu</span><span class="sxs-lookup"><span data-stu-id="8dc29-130">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="8dc29-131">dotnet run</span><span class="sxs-lookup"><span data-stu-id="8dc29-131">dotnet run</span></span> | <span data-ttu-id="8dc29-132">DotNet izleme çalıştırması</span><span class="sxs-lookup"><span data-stu-id="8dc29-132">dotnet watch run</span></span> |
| <span data-ttu-id="8dc29-133">DotNet Run-f netcoreapp 2.0</span><span class="sxs-lookup"><span data-stu-id="8dc29-133">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="8dc29-134">DotNet Watch çalıştırması-f netcoreapp 2.0</span><span class="sxs-lookup"><span data-stu-id="8dc29-134">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="8dc29-135">DotNet Run-f netcoreapp 2.0----arg1</span><span class="sxs-lookup"><span data-stu-id="8dc29-135">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="8dc29-136">DotNet Watch-f netcoreapp 2.0----arg1</span><span class="sxs-lookup"><span data-stu-id="8dc29-136">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="8dc29-137">dotnet test</span><span class="sxs-lookup"><span data-stu-id="8dc29-137">dotnet test</span></span> | <span data-ttu-id="8dc29-138">DotNet izleme testi</span><span class="sxs-lookup"><span data-stu-id="8dc29-138">dotnet watch test</span></span> |

<span data-ttu-id="8dc29-139">*WebApp* klasöründe `dotnet watch run` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8dc29-139">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="8dc29-140">Konsol çıktısı `watch` başlatıldığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-140">The console output indicates `watch` has started.</span></span>

> [!NOTE]
> <span data-ttu-id="8dc29-141">İzlenecek projeyi belirtmek için `dotnet watch --project <PROJECT>` kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8dc29-141">You can use `dotnet watch --project <PROJECT>` to specify a project to watch.</span></span> <span data-ttu-id="8dc29-142">Örneğin, örnek uygulamanın kökünden `dotnet watch --project WebApp run` çalıştırmak Ayrıca *WebApp* projesini de çalıştırır ve bu uygulamayı izleyebilir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-142">For example, running `dotnet watch --project WebApp run` from the root of the sample app will also run and watch the *WebApp* project.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="8dc29-143">`dotnet watch` değişiklik yap</span><span class="sxs-lookup"><span data-stu-id="8dc29-143">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="8dc29-144">`dotnet watch` çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="8dc29-144">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="8dc29-145">*MathController.cs* `Product` yöntemindeki hatayı düzeltir, bu nedenle ürünü döndürür ve toplamı değil:</span><span class="sxs-lookup"><span data-stu-id="8dc29-145">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
    return a * b;
}
```

<span data-ttu-id="8dc29-146">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8dc29-146">Save the file.</span></span> <span data-ttu-id="8dc29-147">Konsol çıktısı, `dotnet watch` bir dosya değişikliği algıladığını ve uygulamayı yeniden başlatıldığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-147">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="8dc29-148">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` doğru sonucu döndürür.</span><span class="sxs-lookup"><span data-stu-id="8dc29-148">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="8dc29-149">`dotnet watch` kullanarak testleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="8dc29-149">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="8dc29-150">*MathController.cs* `Product` yöntemini geri döndürmek için yeniden değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8dc29-150">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="8dc29-151">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8dc29-151">Save the file.</span></span>
1. <span data-ttu-id="8dc29-152">Bir komut kabuğunda *Webapptests* klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="8dc29-152">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="8dc29-153">[DotNet restore](/dotnet/core/tools/dotnet-restore)çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8dc29-153">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="8dc29-154">`dotnet watch test` öğesini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8dc29-154">Run `dotnet watch test`.</span></span> <span data-ttu-id="8dc29-155">Çıktısı bir testin başarısız olduğunu ve izleyicinin dosya değişikliklerini beklediğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="8dc29-155">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="8dc29-156">`Product` yöntemi kodunu, ürünü geri döndürdüğünden düzeltir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-156">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="8dc29-157">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8dc29-157">Save the file.</span></span>

<span data-ttu-id="8dc29-158">`dotnet watch` dosya değişikliğini algılar ve testleri yeniden çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="8dc29-158">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="8dc29-159">Konsol çıktısı, geçirilen testleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-159">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="8dc29-160">Dosya listesini izlemek için Özelleştir</span><span class="sxs-lookup"><span data-stu-id="8dc29-160">Customize files list to watch</span></span>

<span data-ttu-id="8dc29-161">Varsayılan olarak, `dotnet-watch` aşağıdaki glob desenleriyle eşleşen tüm dosyaları izler:</span><span class="sxs-lookup"><span data-stu-id="8dc29-161">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="8dc29-162">*. Csproj* dosyası düzenlenerek, izleme listesine daha fazla öğe eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-162">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="8dc29-163">Öğeler tek tek veya glob desenleri kullanılarak belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-163">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="8dc29-164">İzlenen dosyaları kabul etme</span><span class="sxs-lookup"><span data-stu-id="8dc29-164">Opt-out of files to be watched</span></span>

<span data-ttu-id="8dc29-165">`dotnet-watch`, varsayılan ayarlarını yoksayacak şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-165">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="8dc29-166">Belirli dosyaları yoksaymak için `Watch="false"` özniteliğini *. csproj* dosyasındaki bir öğenin tanımına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8dc29-166">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

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

## <a name="custom-watch-projects"></a><span data-ttu-id="8dc29-167">Özel izleme projeleri</span><span class="sxs-lookup"><span data-stu-id="8dc29-167">Custom watch projects</span></span>

<span data-ttu-id="8dc29-168">`dotnet-watch` C# projelerle sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-168">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="8dc29-169">Özel izleme projeleri, farklı senaryoları işlemek için oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="8dc29-169">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="8dc29-170">Aşağıdaki proje mizanpajını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8dc29-170">Consider the following project layout:</span></span>

* <span data-ttu-id="8dc29-171">**sınamanız**</span><span class="sxs-lookup"><span data-stu-id="8dc29-171">**test/**</span></span>
  * <span data-ttu-id="8dc29-172">*UnitTests/UnitTests. csproj*</span><span class="sxs-lookup"><span data-stu-id="8dc29-172">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="8dc29-173">*Integrationtests/ıntegrationtests. csproj*</span><span class="sxs-lookup"><span data-stu-id="8dc29-173">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="8dc29-174">Hedef her iki projeyi de seyretmek istiyorsanız, her iki projeyi de izlemek için yapılandırılmış özel bir proje dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="8dc29-174">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

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

<span data-ttu-id="8dc29-175">Her iki projede de dosya izlemeye başlamak için, *Test* klasörüne geçin.</span><span class="sxs-lookup"><span data-stu-id="8dc29-175">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="8dc29-176">Aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="8dc29-176">Execute the following command:</span></span>

```dotnetcli
dotnet watch msbuild /t:Test
```

<span data-ttu-id="8dc29-177">VSTest, her iki test projesinde herhangi bir dosya değiştiğinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="8dc29-177">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="8dc29-178">GitHub 'da `dotnet-watch`</span><span class="sxs-lookup"><span data-stu-id="8dc29-178">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="8dc29-179">`dotnet-watch` GitHub [DotNet/AspNetCore deposunun](https://github.com/dotnet/AspNetCore/tree/master/src/Tools/dotnet-watch)bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="8dc29-179">`dotnet-watch` is part of the GitHub [dotnet/AspNetCore repository](https://github.com/dotnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span></span>
