---
title: "ASP.NET Core Mac, Linux veya Windows MVC giriş"
author: rick-anderson
description: "ASP.NET Core MVC ve Mac, Linux ve Windows Visual Studio Code ile çalışmaya başlama"
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 4771555b66f328a819f17a32eb3959f9ecf33d44
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="2a1d3-103">ASP.NET Core MVC Mac, Linux veya Windows ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="2a1d3-103">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="2a1d3-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2a1d3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2a1d3-105">Bu öğretici bir ASP.NET Core MVC web uygulaması kullanılarak oluşturmaya temellerini öğretmek [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="2a1d3-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="2a1d3-106">Öğretici VS Code ile familarity varsayar.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="2a1d3-107">Bkz: [VS Code ile çalışmaya başlama](https://code.visualstudio.com/docs) ve [Visual Studio Code Yardım](#visual-studio-code-help) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="2a1d3-108">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="2a1d3-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="2a1d3-109">macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulama oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2a1d3-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="2a1d3-110">Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2a1d3-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="2a1d3-111">macOS, Linux ve Windows: [Visual Studio Code ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2a1d3-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="2a1d3-112">VS Code'da ve .NET Core yükleyin</span><span class="sxs-lookup"><span data-stu-id="2a1d3-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="2a1d3-113">Bu öğretici gerektirir [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="2a1d3-114">Bkz: [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) ASP.NET Core 1.1 sürümünün.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="2a1d3-115">Aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="2a1d3-115">Install the following:</span></span>

* <span data-ttu-id="2a1d3-116">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="2a1d3-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2a1d3-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="2a1d3-118">VS Code [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="2a1d3-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="2a1d3-119">Dotnet ile bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="2a1d3-119">Create a web app with dotnet</span></span>

<span data-ttu-id="2a1d3-120">Terminal durumundan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="2a1d3-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="2a1d3-121">Açık *MvcMovie* Visual Studio Code (VS Code) ve select klasöründe *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="2a1d3-122">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'MvcMovie' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="2a1d3-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="2a1d3-123">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="2a1d3-123">Add them?"</span></span>
- <span data-ttu-id="2a1d3-124">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![Derleme ve hata ayıklama VS koduyla gerekli uyar varlıklar 'MvcMovie' eksik.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="2a1d3-128">Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5).</span><span class="sxs-lookup"><span data-stu-id="2a1d3-128">Press **Debug** (F5) to build and run the program.</span></span>

![Uygulamayı çalıştırma](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="2a1d3-130">VS Code başlatır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="2a1d3-131">Adres çubuğunun bildirim `localhost:5000` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="2a1d3-132">Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="2a1d3-133">Varsayılan şablonu, çalışma sunar **hakkında giriş** ve **kişi** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="2a1d3-134">Yukarıdaki tarayıcı resimde bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="2a1d3-135">Tarayıcınız boyutuna bağlı olarak, bunları göstermek için Gezinti simgesini gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![sağ üstteki gezinti simgesi](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="2a1d3-137">Bu öğreticinin sonraki bölümünde, biz MVC hakkında bilgi edinin ve biraz kod yazmaya başlamadan.</span><span class="sxs-lookup"><span data-stu-id="2a1d3-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="2a1d3-138">Visual Studio Code help</span><span class="sxs-lookup"><span data-stu-id="2a1d3-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="2a1d3-139">Başlarken</span><span class="sxs-lookup"><span data-stu-id="2a1d3-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="2a1d3-140">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="2a1d3-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="2a1d3-141">Tümleşik terminal</span><span class="sxs-lookup"><span data-stu-id="2a1d3-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="2a1d3-142">Klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="2a1d3-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="2a1d3-143">Mac klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="2a1d3-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="2a1d3-144">Linux klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="2a1d3-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="2a1d3-145">Windows klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="2a1d3-145">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="2a1d3-146">Sonraki - denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="2a1d3-146">Next - Add a controller</span></span>](adding-controller.md)
