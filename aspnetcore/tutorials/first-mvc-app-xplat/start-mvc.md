---
title: "ASP.NET Core Mac, Linux veya Windows MVC giriş"
author: rick-anderson
description: "ASP.NET Core MVC ve Mac, Linux ve Windows Visual Studio Code ile çalışmaya başlama"
keywords: ASP.NET Core, MVC, VS kodu, Visual Studio kodu, Mac, Linux, Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 87ce5dca011a7bc37d34799db159d933c158cba1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="700b4-104">ASP.NET Core MVC Mac, Linux veya Windows ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="700b4-104">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="700b4-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="700b4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="700b4-106">Bu öğretici bir ASP.NET Core MVC web uygulaması kullanılarak oluşturmaya temellerini öğretmek [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="700b4-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="700b4-107">Öğretici VS Code ile familarity varsayar.</span><span class="sxs-lookup"><span data-stu-id="700b4-107">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="700b4-108">Bkz: [VS Code ile çalışmaya başlama](https://code.visualstudio.com/docs) ve [Visual Studio Code Yardım](#visual-studio-code-help) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="700b4-108">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="700b4-109">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="700b4-109">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="700b4-110">macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulama oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="700b4-110">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="700b4-111">Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="700b4-111">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="700b4-112">macOS, Linux ve Windows: [Visual Studio Code ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="700b4-112">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="700b4-113">VS Code'da ve .NET Core yükleyin</span><span class="sxs-lookup"><span data-stu-id="700b4-113">Install VS Code and .NET Core</span></span>

<span data-ttu-id="700b4-114">Bu öğretici gerektirir [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="700b4-114">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="700b4-115">Bkz: [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) ASP.NET Core 1.1 sürümünün.</span><span class="sxs-lookup"><span data-stu-id="700b4-115">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="700b4-116">Aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="700b4-116">Install the following:</span></span>

* <span data-ttu-id="700b4-117">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="700b4-117">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="700b4-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="700b4-118">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="700b4-119">VS Code [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="700b4-119">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="700b4-120">Dotnet ile bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="700b4-120">Create a web app with dotnet</span></span>

<span data-ttu-id="700b4-121">Terminal durumundan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="700b4-121">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="700b4-122">Açık *MvcMovie* Visual Studio Code (VS Code) ve select klasöründe *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="700b4-122">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="700b4-123">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'MvcMovie' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="700b4-123">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="700b4-124">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="700b4-124">Add them?"</span></span>
- <span data-ttu-id="700b4-125">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="700b4-125">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![Derleme ve hata ayıklama VS koduyla gerekli uyar varlıklar 'MvcMovie' eksik.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="700b4-129">Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5).</span><span class="sxs-lookup"><span data-stu-id="700b4-129">Press **Debug** (F5) to build and run the program.</span></span>

![Uygulamayı çalıştırma](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="700b4-131">VS Code başlatır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="700b4-131">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="700b4-132">Adres çubuğunun bildirim `localhost:5000` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="700b4-132">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="700b4-133">Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="700b4-133">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="700b4-134">Varsayılan şablonu, çalışma sunar **hakkında giriş** ve **kişi** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="700b4-134">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="700b4-135">Yukarıdaki tarayıcı resimde bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="700b4-135">The browser image above doesn't show these links.</span></span> <span data-ttu-id="700b4-136">Tarayıcınız boyutuna bağlı olarak, bunları göstermek için Gezinti simgesini gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="700b4-136">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![sağ üstteki gezinti simgesi](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="700b4-138">Bu öğreticinin sonraki bölümünde, biz MVC hakkında bilgi edinin ve biraz kod yazmaya başlamadan.</span><span class="sxs-lookup"><span data-stu-id="700b4-138">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="700b4-139">Visual Studio Code Yardım</span><span class="sxs-lookup"><span data-stu-id="700b4-139">Visual Studio Code help</span></span>

- [<span data-ttu-id="700b4-140">Başlarken</span><span class="sxs-lookup"><span data-stu-id="700b4-140">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="700b4-141">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="700b4-141">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="700b4-142">Tümleşik terminal</span><span class="sxs-lookup"><span data-stu-id="700b4-142">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="700b4-143">Klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="700b4-143">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="700b4-144">Mac klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="700b4-144">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="700b4-145">Linux klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="700b4-145">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="700b4-146">Windows klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="700b4-146">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="700b4-147">Sonraki - denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="700b4-147">Next - Add a controller</span></span>](adding-controller.md)
