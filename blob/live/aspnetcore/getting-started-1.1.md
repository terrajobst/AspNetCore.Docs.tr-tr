---
title: "ASP.NET Core 1.1 ile çalışmaya başlama"
author: rick-anderson
description: "Oluşturur ve ASP.NET Core 1.1 kullanarak basit bir Hello World uygulamanın çalıştığı hızlı bir öğretici."
keywords: "ASP.NET Core, öğretici, kullanmaya başlama"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="97e60-104">ASP.NET Core 1.1 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="97e60-104">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="97e60-105">Bu yönergeler için ASP.NET Core 1.1 içindir.</span><span class="sxs-lookup"><span data-stu-id="97e60-105">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="97e60-106">En son sürümünü mü istiyorsunuz?</span><span class="sxs-lookup"><span data-stu-id="97e60-106">Looking for the latest version?</span></span> <span data-ttu-id="97e60-107">Bkz: [geçerli sürümü Bu öğretici,](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="97e60-107">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="97e60-108">.NET Core yükleme **SDK'sı yükleyicisi** 1.0.4 SDK'dan için [1.0.5 & 1.1.2 .NET Core SDK 1.0.4 indirmeler sayfası](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="97e60-108">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="97e60-109">Yeni bir .NET Core proje için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="97e60-109">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="97e60-110">MacOS ve Linux üzerinde bir terminal penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="97e60-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="97e60-111">Windows, bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="97e60-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="97e60-112">Makinenizde sonraki bir SDK sürümünü yüklediyseniz, oluşturma bir *global.json* 1.0.4 seçmek için dosya SDK.</span><span class="sxs-lookup"><span data-stu-id="97e60-112">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="97e60-113">Yeni bir .NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="97e60-113">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="97e60-114">Paketler geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="97e60-114">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="97e60-115">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="97e60-115">Run the app.</span></span>

   <span data-ttu-id="97e60-116">`dotnet run` Gerekirse komutu uygulamayı ilk kez oluşturur.</span><span class="sxs-lookup"><span data-stu-id="97e60-116">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="97e60-117">Göz atın`http://localhost:5000`</span><span class="sxs-lookup"><span data-stu-id="97e60-117">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="97e60-118">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="97e60-118">Next steps</span></span>

<span data-ttu-id="97e60-119">Başlama öğreticileri için bkz: [ASP.NET Core öğreticileri](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="97e60-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="97e60-120">ASP.NET temel kavramlar ve mimari bir giriş için bkz: [ASP.NET Core giriş](index.md) ve [ASP.NET Core Temelleri](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="97e60-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="97e60-121">Bir ASP.NET Core uygulama .NET Core veya .NET Framework temel sınıf kitaplığı ve çalışma zamanı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97e60-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="97e60-122">Daha fazla bilgi için bkz: [.NET Core ve .NET Framework arasında seçim yapma](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="97e60-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
