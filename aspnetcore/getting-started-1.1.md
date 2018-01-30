---
title: "ASP.NET Core 1.1 ile çalışmaya başlama"
author: rick-anderson
description: "Oluşturur ve ASP.NET Core 1.1 kullanarak basit bir Hello World uygulamanın çalıştığı hızlı bir öğretici."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: 895e91efbba931923540e4cd182862cbc1851585
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="9ccde-103">ASP.NET Core 1.1 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="9ccde-103">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="9ccde-104">Bu yönergeler için ASP.NET Core 1.1 içindir.</span><span class="sxs-lookup"><span data-stu-id="9ccde-104">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="9ccde-105">En son sürümünü mü istiyorsunuz?</span><span class="sxs-lookup"><span data-stu-id="9ccde-105">Looking for the latest version?</span></span> <span data-ttu-id="9ccde-106">Bkz: [geçerli sürümü Bu öğretici,](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="9ccde-106">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="9ccde-107">.NET Core yükleme **SDK'sı yükleyicisi** 1.0.4 SDK'dan için [1.0.5 & 1.1.2 .NET Core SDK 1.0.4 indirmeler sayfası](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="9ccde-107">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="9ccde-108">Yeni bir .NET Core proje için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9ccde-108">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="9ccde-109">MacOS ve Linux üzerinde bir terminal penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="9ccde-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="9ccde-110">Windows, bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="9ccde-110">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="9ccde-111">Makinenizde sonraki bir SDK sürümünü yüklediyseniz, oluşturma bir *global.json* 1.0.4 seçmek için dosya SDK.</span><span class="sxs-lookup"><span data-stu-id="9ccde-111">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="9ccde-112">Yeni bir .NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9ccde-112">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="9ccde-113">Paketler geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="9ccde-113">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="9ccde-114">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9ccde-114">Run the app.</span></span>

   <span data-ttu-id="9ccde-115">`dotnet run` Gerekirse komutu uygulamayı ilk kez oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9ccde-115">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="9ccde-116">Göz atın`http://localhost:5000`</span><span class="sxs-lookup"><span data-stu-id="9ccde-116">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="9ccde-117">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="9ccde-117">Next steps</span></span>

<span data-ttu-id="9ccde-118">Başlama öğreticileri için bkz: [ASP.NET Core öğreticileri](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="9ccde-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="9ccde-119">ASP.NET temel kavramlar ve mimari bir giriş için bkz: [ASP.NET Core giriş](index.md) ve [ASP.NET Core Temelleri](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="9ccde-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="9ccde-120">Bir ASP.NET Core uygulama .NET Core veya .NET Framework temel sınıf kitaplığı ve çalışma zamanı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9ccde-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="9ccde-121">Daha fazla bilgi için bkz: [.NET Core ve .NET Framework arasında seçim yapma](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="9ccde-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
