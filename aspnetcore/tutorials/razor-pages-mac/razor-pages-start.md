---
title: ASP.NET Core Razor sayfalarında Visual Studio ile macOS üzerinde Mac için kullanmaya başlama
author: rick-anderson
description: Mac için Visual Studio kullanarak ASP.NET Core Razor sayfalarında kullanmaya başlamak nasıl Bul
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 29eb11d0195f483b144394e505dd63fb6016161b
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252340"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a><span data-ttu-id="33dc3-103">ASP.NET Core Razor sayfalarında Visual Studio ile macOS üzerinde Mac için kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="33dc3-103">Get started with Razor Pages in ASP.NET Core on macOS with Visual Studio for Mac</span></span>

<span data-ttu-id="33dc3-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="33dc3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="33dc3-105">Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir.</span><span class="sxs-lookup"><span data-stu-id="33dc3-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="33dc3-106">Gözden geçirmenizi öneririz [Razor sayfalarının giriş](xref:mvc/razor-pages/index) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="33dc3-106">We recommend you review [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="33dc3-107">Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.</span><span class="sxs-lookup"><span data-stu-id="33dc3-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33dc3-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="33dc3-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="33dc3-109">Bir Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="33dc3-109">Create a Razor web app</span></span>

<span data-ttu-id="33dc3-110">Terminal durumundan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="33dc3-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="33dc3-112">Yukarıdaki komutları kullanım [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) oluşturun ve Razor sayfalarının proje çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="33dc3-112">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="33dc3-113">Bir tarayıcıda http://localhost:5000 uygulamayı görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="33dc3-113">Open a browser to http://localhost:5000 to view the application.</span></span>

![Giriş veya dizin sayfası](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="33dc3-115">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="33dc3-115">Open the project</span></span>

<span data-ttu-id="33dc3-116">Uygulamayı kapatın CTRL + C tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="33dc3-116">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="33dc3-117">Visual Studio'dan seçin **Dosya > Aç**ve ardından *RazorPagesMovie.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="33dc3-117">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="33dc3-118">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="33dc3-118">Launch the app</span></span>

<span data-ttu-id="33dc3-119">Visual Studio'da seçin **Çalıştır > hata ayıklama olmadan Başlat** uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="33dc3-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="33dc3-120">Visual Studio başlatır [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatılır ve gider `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="33dc3-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="33dc3-121">Sonraki öğreticide biz projenize bir model ekleyin.</span><span class="sxs-lookup"><span data-stu-id="33dc3-121">In the next tutorial, we add a model to the project.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="33dc3-122">Sonraki: bir modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="33dc3-122">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
