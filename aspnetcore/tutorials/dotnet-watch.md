---
title: Dosya İzleyici kullanarak ASP.NET Core uygulamaları geliştirme
author: rick-anderson
description: Bu öğretici, aracı yükleme ve .NET Core CLI dosya İzleyici (dotnet izleme) bir ASP.NET Core uygulamada kullanma gösterilmektedir.
manager: wpickett
ms.author: riande
ms.date: 05/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: 29890640223fe533cca82fb8d39a5ef26e8c6639
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729982"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="15cdc-103">Dosya İzleyici kullanarak ASP.NET Core uygulamaları geliştirme</span><span class="sxs-lookup"><span data-stu-id="15cdc-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="15cdc-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [kazanan Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="15cdc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="15cdc-105">`dotnet watch` çalıştıran bir aracıdır bir [.NET Core CLI](/dotnet/core/tools) değişiklik kaynak dosyaları, komut.</span><span class="sxs-lookup"><span data-stu-id="15cdc-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="15cdc-106">Örneğin, bir dosya değişikliği derleme, test yürütmesi veya dağıtım tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="15cdc-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="15cdc-107">Bu öğretici varolan bir web API iki uç nokta ile kullanır: biri toplam ve bir ürün döndüren bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="15cdc-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="15cdc-108">Ürün yöntemi Bu öğreticide sabit bir hata var.</span><span class="sxs-lookup"><span data-stu-id="15cdc-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="15cdc-109">Karşıdan [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="15cdc-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="15cdc-110">İki projeden oluşan: *WebApp* (bir ASP.NET Core web API) ve *WebAppTests* (web API'si için birim testleri).</span><span class="sxs-lookup"><span data-stu-id="15cdc-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="15cdc-111">Komut kabuğu'na gidin *WebApp* klasör.</span><span class="sxs-lookup"><span data-stu-id="15cdc-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="15cdc-112">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="15cdc-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="15cdc-113">Konsol çıktısı aşağıdakine benzer mesajlarını (uygulama çalıştıran ve istekleri bekleyen gösteren):</span><span class="sxs-lookup"><span data-stu-id="15cdc-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="15cdc-114">Bir web tarayıcısında gidin `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="15cdc-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="15cdc-115">Sonucu görmelisiniz `9`.</span><span class="sxs-lookup"><span data-stu-id="15cdc-115">You should see the result of `9`.</span></span>

<span data-ttu-id="15cdc-116">Ürüne API gidin (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="15cdc-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="15cdc-117">Döndürdüğü `9`değil `20` beklediğiniz gibi.</span><span class="sxs-lookup"><span data-stu-id="15cdc-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="15cdc-118">Öğreticide daha sonra bu sorun düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="15cdc-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="15cdc-119">Ekleme `dotnet watch` bir proje</span><span class="sxs-lookup"><span data-stu-id="15cdc-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="15cdc-120">`dotnet watch` Dosya İzleyici aracı .NET Core SDK 2.1.300 sürümü ile eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="15cdc-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="15cdc-121">Aşağıdaki adımlar .NET Core SDK'ın önceki bir sürümünü kullanırken gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="15cdc-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="15cdc-122">Ekleme bir `Microsoft.DotNet.Watcher.Tools` paketini referansı *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="15cdc-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="15cdc-123">Yükleme `Microsoft.DotNet.Watcher.Tools` aşağıdaki komutu çalıştırarak paketi:</span><span class="sxs-lookup"><span data-stu-id="15cdc-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="15cdc-124">.NET Core CLI komutları kullanarak çalıştırın `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="15cdc-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="15cdc-125">Tüm [.NET Core CLI komutu](/dotnet/core/tools#cli-commands) ile çalıştırılabilir `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="15cdc-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="15cdc-126">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="15cdc-126">For example:</span></span>

| <span data-ttu-id="15cdc-127">Komut</span><span class="sxs-lookup"><span data-stu-id="15cdc-127">Command</span></span> | <span data-ttu-id="15cdc-128">İzleme komut</span><span class="sxs-lookup"><span data-stu-id="15cdc-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="15cdc-129">dotnet çalıştırın</span><span class="sxs-lookup"><span data-stu-id="15cdc-129">dotnet run</span></span> | <span data-ttu-id="15cdc-130">dotnet izleme çalıştırın</span><span class="sxs-lookup"><span data-stu-id="15cdc-130">dotnet watch run</span></span> |
| <span data-ttu-id="15cdc-131">dotnet -f netcoreapp2.0 çalıştırın</span><span class="sxs-lookup"><span data-stu-id="15cdc-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="15cdc-132">-f netcoreapp2.0 çalıştırmak dotnet izleme</span><span class="sxs-lookup"><span data-stu-id="15cdc-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="15cdc-133">-f netcoreapp2.0--çalıştırmak dotnet--arg1</span><span class="sxs-lookup"><span data-stu-id="15cdc-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="15cdc-134">-f netcoreapp2.0--çalıştırmak dotnet izleme--arg1</span><span class="sxs-lookup"><span data-stu-id="15cdc-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="15cdc-135">DotNet test</span><span class="sxs-lookup"><span data-stu-id="15cdc-135">dotnet test</span></span> | <span data-ttu-id="15cdc-136">DotNet izleme test</span><span class="sxs-lookup"><span data-stu-id="15cdc-136">dotnet watch test</span></span> |

<span data-ttu-id="15cdc-137">Çalıştırma `dotnet watch run` içinde *WebApp* klasör.</span><span class="sxs-lookup"><span data-stu-id="15cdc-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="15cdc-138">Konsol çıktısı gösterir `watch` başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="15cdc-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="15cdc-139">İle değişiklik `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="15cdc-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="15cdc-140">Emin olun `dotnet watch` çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="15cdc-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="15cdc-141">Hata düzeltme `Product` yöntemi *MathController.cs* ürünü ve toplam döndürecek şekilde:</span><span class="sxs-lookup"><span data-stu-id="15cdc-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="15cdc-142">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="15cdc-142">Save the file.</span></span> <span data-ttu-id="15cdc-143">Konsol çıktısı belirten `dotnet watch` bir dosya değişiklik algılandı ve uygulamayı yeniden.</span><span class="sxs-lookup"><span data-stu-id="15cdc-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="15cdc-144">Doğrulama `http://localhost:<port number>/api/math/product?a=4&b=5` doğru sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="15cdc-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="15cdc-145">Kullanarak testleri çalıştırma `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="15cdc-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="15cdc-146">Değişiklik `Product` yöntemi *MathController.cs* toplamı döndürme geri dön.</span><span class="sxs-lookup"><span data-stu-id="15cdc-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="15cdc-147">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="15cdc-147">Save the file.</span></span>
1. <span data-ttu-id="15cdc-148">Komut kabuğu'na gidin *WebAppTests* klasör.</span><span class="sxs-lookup"><span data-stu-id="15cdc-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="15cdc-149">Çalıştırma [dotnet geri yükleme](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="15cdc-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="15cdc-150">Çalıştırma `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="15cdc-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="15cdc-151">Bir test başarısız olduğunu ve İzleyici dosya değişiklikleri bekliyor çıktısını gösterir:</span><span class="sxs-lookup"><span data-stu-id="15cdc-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="15cdc-152">Düzeltme `Product` yöntemi kod ürün döndürecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="15cdc-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="15cdc-153">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="15cdc-153">Save the file.</span></span>

<span data-ttu-id="15cdc-154">`dotnet watch` dosya değişikliği algılar ve testleri yeniden çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="15cdc-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="15cdc-155">Konsol çıktısı testleri geçirilen gösterir.</span><span class="sxs-lookup"><span data-stu-id="15cdc-155">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="15cdc-156">github'da dotnet izleme</span><span class="sxs-lookup"><span data-stu-id="15cdc-156">dotnet-watch in GitHub</span></span>

<span data-ttu-id="15cdc-157">DotNet izleme GitHub bir parçasıdır [DotNetTools depo](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="15cdc-157">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="15cdc-158">[MSBuild bölüm](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) , [dotnet izleme Benioku](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) dotnet izleme izleniyor MSBuild proje dosyasından nasıl yapılandırılabilir açıklar.</span><span class="sxs-lookup"><span data-stu-id="15cdc-158">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="15cdc-159">[Dotnet izleme Benioku](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) sahip dotnet izleme, bu öğreticide kapsanmayan hakkında bilgi.</span><span class="sxs-lookup"><span data-stu-id="15cdc-159">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) has information on dotnet-watch not covered in this tutorial.</span></span>
