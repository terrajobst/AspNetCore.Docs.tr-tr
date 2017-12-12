---
title: "DotNet Gözcü kullanarak ASP.NET Core uygulamaları geliştirme"
author: rick-anderson
description: "Bu öğretici, yükleme ve ASP.NET Core uygulamada .NET Core CLI dosya İzleyici (dotnet izleme) aracı kullanma gösterilmektedir."
keywords: ASP.NET Core, dotnet izleme kullanma
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 9baf2ce2a1270a728616a8a2ab45deca9a9cde6f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="51c4f-104">DotNet Gözcü kullanarak ASP.NET Core uygulamaları geliştirme</span><span class="sxs-lookup"><span data-stu-id="51c4f-104">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="51c4f-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [kazanan Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="51c4f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="51c4f-106">`dotnet watch`çalıştıran bir aracıdır bir [.NET Core CLI](/dotnet/core/tools) değişiklik kaynak dosyaları, komut.</span><span class="sxs-lookup"><span data-stu-id="51c4f-106">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="51c4f-107">Örneğin, bir dosya değişikliği derleme, test yürütmesi veya dağıtım tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="51c4f-107">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="51c4f-108">Bu öğreticide, var olan bir Web API uygulaması ile iki uç nokta kullanırız: toplam ve bir ürün döndüren bir döndüren bir.</span><span class="sxs-lookup"><span data-stu-id="51c4f-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="51c4f-109">Ürün yöntemi bu öğreticinin bir parçası olarak düzeltme bir hata içeriyor.</span><span class="sxs-lookup"><span data-stu-id="51c4f-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="51c4f-110">Karşıdan [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="51c4f-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="51c4f-111">İki proje içerir: *WebApp* (ASP.NET çekirdek Web API) ve *WebAppTests* (Web API için birim testleri).</span><span class="sxs-lookup"><span data-stu-id="51c4f-111">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="51c4f-112">Komut kabuğu'na gidin *WebApp* klasörü ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="51c4f-112">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="51c4f-113">Konsol çıktısı aşağıdakine benzer mesajlarını (uygulama çalıştıran ve istekleri bekleyen gösteren):</span><span class="sxs-lookup"><span data-stu-id="51c4f-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="51c4f-114">Bir web tarayıcısında gidin `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="51c4f-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="51c4f-115">Sonucu görmelisiniz `9`.</span><span class="sxs-lookup"><span data-stu-id="51c4f-115">You should see the result of `9`.</span></span>

<span data-ttu-id="51c4f-116">Ürüne API gidin (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="51c4f-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="51c4f-117">Döndürdüğü `9`değil `20` beklediğiniz gibi.</span><span class="sxs-lookup"><span data-stu-id="51c4f-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="51c4f-118">Biz bu öğreticide daha sonra düzeltmesi.</span><span class="sxs-lookup"><span data-stu-id="51c4f-118">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="51c4f-119">Ekleme `dotnet watch` bir proje</span><span class="sxs-lookup"><span data-stu-id="51c4f-119">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="51c4f-120">Ekleme bir `Microsoft.DotNet.Watcher.Tools` paketini referansı *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="51c4f-120">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="51c4f-121">Yükleme `Microsoft.DotNet.Watcher.Tools` aşağıdaki komutu çalıştırarak paketi:</span><span class="sxs-lookup"><span data-stu-id="51c4f-121">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="51c4f-122">.NET Core CLI komutları kullanarak çalıştırma`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="51c4f-122">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="51c4f-123">Tüm [.NET Core CLI komutu](/dotnet/core/tools#cli-commands) ile çalıştırılabilir `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="51c4f-123">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="51c4f-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="51c4f-124">For example:</span></span>

| <span data-ttu-id="51c4f-125">Komut</span><span class="sxs-lookup"><span data-stu-id="51c4f-125">Command</span></span> | <span data-ttu-id="51c4f-126">İzleme komut</span><span class="sxs-lookup"><span data-stu-id="51c4f-126">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="51c4f-127">dotnet çalıştırın</span><span class="sxs-lookup"><span data-stu-id="51c4f-127">dotnet run</span></span> | <span data-ttu-id="51c4f-128">dotnet izleme çalıştırın</span><span class="sxs-lookup"><span data-stu-id="51c4f-128">dotnet watch run</span></span> |
| <span data-ttu-id="51c4f-129">dotnet -f netcoreapp2.0 çalıştırın</span><span class="sxs-lookup"><span data-stu-id="51c4f-129">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="51c4f-130">-f netcoreapp2.0 çalıştırmak dotnet izleme</span><span class="sxs-lookup"><span data-stu-id="51c4f-130">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="51c4f-131">-f netcoreapp2.0--çalıştırmak dotnet--arg1</span><span class="sxs-lookup"><span data-stu-id="51c4f-131">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="51c4f-132">-f netcoreapp2.0--çalıştırmak dotnet izleme--arg1</span><span class="sxs-lookup"><span data-stu-id="51c4f-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="51c4f-133">DotNet test</span><span class="sxs-lookup"><span data-stu-id="51c4f-133">dotnet test</span></span> | <span data-ttu-id="51c4f-134">DotNet izleme test</span><span class="sxs-lookup"><span data-stu-id="51c4f-134">dotnet watch test</span></span> |

<span data-ttu-id="51c4f-135">Çalıştırma `dotnet watch run` içinde *WebApp* klasör.</span><span class="sxs-lookup"><span data-stu-id="51c4f-135">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="51c4f-136">Konsol çıktısı gösterir `watch` başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="51c4f-136">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="51c4f-137">Değişiklikleri yapma`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="51c4f-137">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="51c4f-138">Emin olun `dotnet watch` çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="51c4f-138">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="51c4f-139">Hata düzeltme `Product` yöntemi *MathController.cs* ürünü ve toplam döndürecek şekilde:</span><span class="sxs-lookup"><span data-stu-id="51c4f-139">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="51c4f-140">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="51c4f-140">Save the file.</span></span> <span data-ttu-id="51c4f-141">Konsol çıktısı belirten `dotnet watch` bir dosya değişiklik algılandı ve uygulamayı yeniden.</span><span class="sxs-lookup"><span data-stu-id="51c4f-141">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="51c4f-142">Doğrulama `http://localhost:<port number>/api/math/product?a=4&b=5` doğru sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="51c4f-142">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="51c4f-143">Kullanarak testleri çalıştırma`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="51c4f-143">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="51c4f-144">Değişiklik `Product` yöntemi *MathController.cs* geri toplamı döndürmek için ve dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="51c4f-144">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="51c4f-145">Komut kabuğu'na gidin *WebAppTests* klasör.</span><span class="sxs-lookup"><span data-stu-id="51c4f-145">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="51c4f-146">Çalıştırma `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="51c4f-146">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="51c4f-147">Çalıştırma `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="51c4f-147">Run `dotnet watch test`.</span></span> <span data-ttu-id="51c4f-148">Bir test başarısız oldu ve bu İzleyici dosya değişiklikleri bekliyor çıktısını gösterir:</span><span class="sxs-lookup"><span data-stu-id="51c4f-148">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="51c4f-149">Düzeltme `Product` yöntemi kod ürün döndürecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="51c4f-149">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="51c4f-150">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="51c4f-150">Save the file.</span></span>

<span data-ttu-id="51c4f-151">`dotnet watch`dosya değişikliği algılar ve testleri yeniden çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="51c4f-151">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="51c4f-152">Konsol çıktısı testleri geçirilen gösterir.</span><span class="sxs-lookup"><span data-stu-id="51c4f-152">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="51c4f-153">github'da dotnet izleme</span><span class="sxs-lookup"><span data-stu-id="51c4f-153">dotnet-watch in GitHub</span></span>

<span data-ttu-id="51c4f-154">DotNet izleme GitHub bir parçasıdır [DotNetTools depo](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span><span class="sxs-lookup"><span data-stu-id="51c4f-154">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="51c4f-155">[MSBuild bölüm](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) , [dotnet izleme Benioku](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) dotnet izleme izleniyor MSBuild proje dosyasından nasıl yapılandırılabilir açıklar.</span><span class="sxs-lookup"><span data-stu-id="51c4f-155">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="51c4f-156">[Dotnet izleme Benioku](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) Bu öğreticide kapsanmayan dotnet izleme hakkında bilgi içerir.</span><span class="sxs-lookup"><span data-stu-id="51c4f-156">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
