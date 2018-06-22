---
title: ASP.NET Core Razor sayfalarında, Visual Studio Code ile çalışmaya başlama
author: rick-anderson
description: Visual Studio Code ile bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temellerini öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: b1f1dcc1401d707cff79f3269ab70b900e9fc21c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275947"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="22e28-103">ASP.NET Core Razor sayfalarında, Visual Studio Code ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="22e28-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="22e28-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="22e28-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="22e28-105">Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir.</span><span class="sxs-lookup"><span data-stu-id="22e28-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="22e28-106">Tamamlamanız önerilir [Razor sayfalarının giriş](xref:razor-pages/index) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="22e28-106">We recommend you complete [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="22e28-107">Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.</span><span class="sxs-lookup"><span data-stu-id="22e28-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22e28-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="22e28-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="22e28-109">Bir Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="22e28-109">Create a Razor web app</span></span>

<span data-ttu-id="22e28-110">Terminal durumundan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="22e28-110">From a terminal, run the following commands:</span></span>

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

<span data-ttu-id="22e28-112">Yukarıdaki komutları kullanım [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) oluşturun ve Razor sayfalarının proje çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="22e28-112">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="22e28-113">Bir tarayıcıda http://localhost:5000 uygulamayı görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="22e28-113">Open a browser to http://localhost:5000 to view the application.</span></span>

![Giriş veya dizin sayfası](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="22e28-115">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="22e28-115">Open the project</span></span>

<span data-ttu-id="22e28-116">Uygulamayı kapatın CTRL + C tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="22e28-116">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="22e28-117">Visual Studio kodundan (VS Code) seçin **Dosya > Klasör Aç**ve ardından *RazorPagesMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="22e28-117">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="22e28-118">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'RazorPagesMovie' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="22e28-118">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="22e28-119">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="22e28-119">Add them?"</span></span>
- <span data-ttu-id="22e28-120">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="22e28-120">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="22e28-121">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="22e28-121">Launch the app</span></span>

<span data-ttu-id="22e28-122">Hata ayıklama olmadan uygulamayı başlatmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="22e28-122">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="22e28-123">Alternatif olarak, gelen **hata ayıklama** menüsünde, select **hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="22e28-123">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="22e28-124">Sonraki öğreticide biz projenize bir model ekleyin.</span><span class="sxs-lookup"><span data-stu-id="22e28-124">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="22e28-125">Sonraki: bir modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="22e28-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
