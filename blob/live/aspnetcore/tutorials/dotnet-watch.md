---
title: "DotNet Gözcü kullanarak ASP.NET Core uygulamaları geliştirme"
author: rick-anderson
description: "Bu öğretici, yükleme ve ASP.NET Core uygulamada .NET Core CLI dosya İzleyici (dotnet izleme) aracı kullanma gösterilmektedir."
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: cadd4a6a78c29e2213c39a02729b5c32a2b93ebd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="4277e-103">DotNet Gözcü kullanarak ASP.NET Core uygulamaları geliştirme</span><span class="sxs-lookup"><span data-stu-id="4277e-103">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="4277e-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [kazanan Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="4277e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="4277e-105">`dotnet watch`çalıştıran bir aracıdır bir [.NET Core CLI](/dotnet/core/tools) değişiklik kaynak dosyaları, komut.</span><span class="sxs-lookup"><span data-stu-id="4277e-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="4277e-106">Örneğin, bir dosya değişikliği derleme, test yürütmesi veya dağıtım tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4277e-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="4277e-107">Bu öğreticide, var olan bir Web API uygulaması ile iki uç nokta kullanırız: toplam ve bir ürün döndüren bir döndüren bir.</span><span class="sxs-lookup"><span data-stu-id="4277e-107">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="4277e-108">Ürün yöntemi bu öğreticinin bir parçası olarak düzeltme bir hata içeriyor.</span><span class="sxs-lookup"><span data-stu-id="4277e-108">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="4277e-109">Karşıdan [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="4277e-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="4277e-110">İki proje içerir: *WebApp* (ASP.NET çekirdek Web API) ve *WebAppTests* (Web API için birim testleri).</span><span class="sxs-lookup"><span data-stu-id="4277e-110">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="4277e-111">Komut kabuğu'na gidin *WebApp* klasörü ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4277e-111">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="4277e-112">Konsol çıktısı aşağıdakine benzer mesajlarını (uygulama çalıştıran ve istekleri bekleyen gösteren):</span><span class="sxs-lookup"><span data-stu-id="4277e-112">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="4277e-113">Bir web tarayıcısında gidin `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="4277e-113">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="4277e-114">Sonucu görmelisiniz `9`.</span><span class="sxs-lookup"><span data-stu-id="4277e-114">You should see the result of `9`.</span></span>

<span data-ttu-id="4277e-115">Ürüne API gidin (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="4277e-115">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="4277e-116">Döndürdüğü `9`değil `20` beklediğiniz gibi.</span><span class="sxs-lookup"><span data-stu-id="4277e-116">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="4277e-117">Biz bu öğreticide daha sonra düzeltmesi.</span><span class="sxs-lookup"><span data-stu-id="4277e-117">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="4277e-118">Ekleme `dotnet watch` bir proje</span><span class="sxs-lookup"><span data-stu-id="4277e-118">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="4277e-119">Ekleme bir `Microsoft.DotNet.Watcher.Tools` paketini referansı *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="4277e-119">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="4277e-120">Yükleme `Microsoft.DotNet.Watcher.Tools` aşağıdaki komutu çalıştırarak paketi:</span><span class="sxs-lookup"><span data-stu-id="4277e-120">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="4277e-121">.NET Core CLI komutları kullanarak çalıştırma`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="4277e-121">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="4277e-122">Tüm [.NET Core CLI komutu](/dotnet/core/tools#cli-commands) ile çalıştırılabilir `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="4277e-122">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="4277e-123">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4277e-123">For example:</span></span>

| <span data-ttu-id="4277e-124">Komut</span><span class="sxs-lookup"><span data-stu-id="4277e-124">Command</span></span> | <span data-ttu-id="4277e-125">İzleme komut</span><span class="sxs-lookup"><span data-stu-id="4277e-125">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="4277e-126">dotnet çalıştırın</span><span class="sxs-lookup"><span data-stu-id="4277e-126">dotnet run</span></span> | <span data-ttu-id="4277e-127">dotnet izleme çalıştırın</span><span class="sxs-lookup"><span data-stu-id="4277e-127">dotnet watch run</span></span> |
| <span data-ttu-id="4277e-128">dotnet -f netcoreapp2.0 çalıştırın</span><span class="sxs-lookup"><span data-stu-id="4277e-128">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="4277e-129">-f netcoreapp2.0 çalıştırmak dotnet izleme</span><span class="sxs-lookup"><span data-stu-id="4277e-129">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="4277e-130">-f netcoreapp2.0--çalıştırmak dotnet--arg1</span><span class="sxs-lookup"><span data-stu-id="4277e-130">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="4277e-131">-f netcoreapp2.0--çalıştırmak dotnet izleme--arg1</span><span class="sxs-lookup"><span data-stu-id="4277e-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="4277e-132">DotNet test</span><span class="sxs-lookup"><span data-stu-id="4277e-132">dotnet test</span></span> | <span data-ttu-id="4277e-133">DotNet izleme test</span><span class="sxs-lookup"><span data-stu-id="4277e-133">dotnet watch test</span></span> |

<span data-ttu-id="4277e-134">Çalıştırma `dotnet watch run` içinde *WebApp* klasör.</span><span class="sxs-lookup"><span data-stu-id="4277e-134">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="4277e-135">Konsol çıktısı gösterir `watch` başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="4277e-135">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="4277e-136">Değişiklikleri yapma`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="4277e-136">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="4277e-137">Emin olun `dotnet watch` çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="4277e-137">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="4277e-138">Hata düzeltme `Product` yöntemi *MathController.cs* ürünü ve toplam döndürecek şekilde:</span><span class="sxs-lookup"><span data-stu-id="4277e-138">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="4277e-139">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4277e-139">Save the file.</span></span> <span data-ttu-id="4277e-140">Konsol çıktısı belirten `dotnet watch` bir dosya değişiklik algılandı ve uygulamayı yeniden.</span><span class="sxs-lookup"><span data-stu-id="4277e-140">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="4277e-141">Doğrulama `http://localhost:<port number>/api/math/product?a=4&b=5` doğru sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="4277e-141">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="4277e-142">Kullanarak testleri çalıştırma`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="4277e-142">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="4277e-143">Değişiklik `Product` yöntemi *MathController.cs* geri toplamı döndürmek için ve dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4277e-143">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="4277e-144">Komut kabuğu'na gidin *WebAppTests* klasör.</span><span class="sxs-lookup"><span data-stu-id="4277e-144">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="4277e-145">Çalıştırma `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="4277e-145">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="4277e-146">Çalıştırma `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="4277e-146">Run `dotnet watch test`.</span></span> <span data-ttu-id="4277e-147">Bir test başarısız oldu ve bu İzleyici dosya değişiklikleri bekliyor çıktısını gösterir:</span><span class="sxs-lookup"><span data-stu-id="4277e-147">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="4277e-148">Düzeltme `Product` yöntemi kod ürün döndürecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="4277e-148">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="4277e-149">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4277e-149">Save the file.</span></span>

<span data-ttu-id="4277e-150">`dotnet watch`dosya değişikliği algılar ve testleri yeniden çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="4277e-150">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="4277e-151">Konsol çıktısı testleri geçirilen gösterir.</span><span class="sxs-lookup"><span data-stu-id="4277e-151">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="4277e-152">dotnet-watch in GitHub</span><span class="sxs-lookup"><span data-stu-id="4277e-152">dotnet-watch in GitHub</span></span>

<span data-ttu-id="4277e-153">DotNet izleme GitHub bir parçasıdır [DotNetTools depo](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="4277e-153">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="4277e-154">[MSBuild bölüm](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) , [dotnet izleme Benioku](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) dotnet izleme izleniyor MSBuild proje dosyasından nasıl yapılandırılabilir açıklar.</span><span class="sxs-lookup"><span data-stu-id="4277e-154">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="4277e-155">[Dotnet izleme Benioku](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) Bu öğreticide kapsanmayan dotnet izleme hakkında bilgi içerir.</span><span class="sxs-lookup"><span data-stu-id="4277e-155">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
