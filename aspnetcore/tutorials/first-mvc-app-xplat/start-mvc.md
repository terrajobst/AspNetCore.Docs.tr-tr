---
title: ASP.NET Core MVC giriş macOS, Linux veya Windows
author: rick-anderson
description: MacOS, Linux ve Windows ASP.NET Core MVC ve Visual Studio Code ile çalışmaya başlama öğrenin
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275281"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="fd4a7-103">ASP.NET Core MVC giriş macOS, Linux veya Windows</span><span class="sxs-lookup"><span data-stu-id="fd4a7-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="fd4a7-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fd4a7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fd4a7-105">Bu öğretici bir ASP.NET Core MVC web uygulaması kullanılarak oluşturmaya temellerini öğretmek [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="fd4a7-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="fd4a7-106">Öğretici VS Code ile familarity varsayar.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="fd4a7-107">Bkz: [VS Code ile çalışmaya başlama](https://code.visualstudio.com/docs) ve [Visual Studio Code Yardım](#visual-studio-code-help) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="fd4a7-108">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="fd4a7-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="fd4a7-109">macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulama oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="fd4a7-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="fd4a7-110">Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="fd4a7-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="fd4a7-111">macOS, Linux ve Windows: [Visual Studio Code ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="fd4a7-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="fd4a7-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="fd4a7-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="fd4a7-113">Dotnet ile bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="fd4a7-113">Create a web app with dotnet</span></span>

<span data-ttu-id="fd4a7-114">Terminal durumundan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="fd4a7-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="fd4a7-115">Açık *MvcMovie* Visual Studio Code (VS Code) ve select klasöründe *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="fd4a7-116">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'MvcMovie' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="fd4a7-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="fd4a7-117">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="fd4a7-117">Add them?"</span></span>
- <span data-ttu-id="fd4a7-118">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![Derleme ve hata ayıklama VS koduyla gerekli uyar varlıklar 'MvcMovie' eksik.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="fd4a7-122">Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5).</span><span class="sxs-lookup"><span data-stu-id="fd4a7-122">Press **Debug** (F5) to build and run the program.</span></span>

![Uygulamayı çalıştırma](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="fd4a7-124">VS Code başlatır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="fd4a7-125">Adres çubuğunun bildirim `localhost:5000` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="fd4a7-126">Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="fd4a7-127">Varsayılan şablonu, çalışma sunar **hakkında giriş** ve **kişi** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="fd4a7-128">Yukarıdaki tarayıcı resimde bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="fd4a7-129">Tarayıcınız boyutuna bağlı olarak, bunları göstermek için Gezinti simgesini gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![sağ üstteki gezinti simgesi](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="fd4a7-131">Bu öğreticinin sonraki bölümünde, biz MVC hakkında bilgi edinin ve biraz kod yazmaya başlamadan.</span><span class="sxs-lookup"><span data-stu-id="fd4a7-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="fd4a7-132">Visual Studio Code Yardım</span><span class="sxs-lookup"><span data-stu-id="fd4a7-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="fd4a7-133">Başlarken</span><span class="sxs-lookup"><span data-stu-id="fd4a7-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="fd4a7-134">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="fd4a7-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="fd4a7-135">Tümleşik terminal</span><span class="sxs-lookup"><span data-stu-id="fd4a7-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="fd4a7-136">Klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="fd4a7-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="fd4a7-137">macOS klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="fd4a7-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="fd4a7-138">Linux klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="fd4a7-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="fd4a7-139">Windows klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="fd4a7-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="fd4a7-140">Sonraki - denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="fd4a7-140">Next - Add a controller</span></span>](adding-controller.md)
