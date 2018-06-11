---
title: ASP.NET Core Razor sayfalarında, Visual Studio Code ile çalışmaya başlama
author: rick-anderson
description: Visual Studio Code ile bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temellerini öğrenin.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 17ab52b80a40f6204e2bf2cf9048071c55c0a708
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252223"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="55a4d-103">ASP.NET Core Razor sayfalarında, Visual Studio Code ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="55a4d-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="55a4d-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="55a4d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="55a4d-105">Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir.</span><span class="sxs-lookup"><span data-stu-id="55a4d-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="55a4d-106">Tamamlamanız önerilir [Razor sayfalarının giriş](xref:mvc/razor-pages/index) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="55a4d-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="55a4d-107">Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.</span><span class="sxs-lookup"><span data-stu-id="55a4d-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55a4d-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="55a4d-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="55a4d-109">Bir Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="55a4d-109">Create a Razor web app</span></span>

<span data-ttu-id="55a4d-110">Terminal durumundan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="55a4d-110">From a terminal, run the following commands:</span></span>

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

<span data-ttu-id="55a4d-112">Yukarıdaki komutları kullanım [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) oluşturun ve Razor sayfalarının proje çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="55a4d-112">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="55a4d-113">Bir tarayıcıda http://localhost:5000 uygulamayı görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="55a4d-113">Open a browser to http://localhost:5000 to view the application.</span></span>

![Giriş veya dizin sayfası](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="55a4d-115">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="55a4d-115">Open the project</span></span>

<span data-ttu-id="55a4d-116">Uygulamayı kapatın CTRL + C tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="55a4d-116">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="55a4d-117">Visual Studio kodundan (VS Code) seçin **Dosya > Klasör Aç**ve ardından *RazorPagesMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="55a4d-117">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="55a4d-118">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'RazorPagesMovie' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="55a4d-118">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="55a4d-119">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="55a4d-119">Add them?"</span></span>
- <span data-ttu-id="55a4d-120">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="55a4d-120">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="55a4d-121">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="55a4d-121">Launch the app</span></span>

<span data-ttu-id="55a4d-122">Hata ayıklama olmadan uygulamayı başlatmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="55a4d-122">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="55a4d-123">Alternatif olarak, gelen **hata ayıklama** menüsünde, select **hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="55a4d-123">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="55a4d-124">Sonraki öğreticide biz projenize bir model ekleyin.</span><span class="sxs-lookup"><span data-stu-id="55a4d-124">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="55a4d-125">Sonraki: bir modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="55a4d-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
