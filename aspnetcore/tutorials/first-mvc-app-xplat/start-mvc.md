---
title: MacOS, Linux veya Windows üzerinde ASP.NET Core MVC'ye giriş
author: rick-anderson
description: MacOS, Linux ve Windows üzerinde ASP.NET Core MVC ve Visual Studio Code ile çalışmaya başlama hakkında bilgi edinin
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 6b5835a7900c580cb88c004d3c8e7f70ad06c588
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861972"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="7c349-103">MacOS, Linux veya Windows üzerinde ASP.NET Core MVC'ye giriş</span><span class="sxs-lookup"><span data-stu-id="7c349-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="7c349-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c349-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7c349-105">Bu öğreticide bir ASP.NET Core MVC kullanarak web uygulamasını oluşturmaya ilişkin temel bilgileri öğretir [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="7c349-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="7c349-106">Bu öğretici, VS Code ile familarity varsayar.</span><span class="sxs-lookup"><span data-stu-id="7c349-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="7c349-107">Bkz: [VS Code ile çalışmaya başlama](https://code.visualstudio.com/docs) ve [Visual Studio Code Yardım](#visual-studio-code-help) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="7c349-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="7c349-108">Bu öğreticinin 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="7c349-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="7c349-109">macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7c349-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="7c349-110">Windows: [Visual Studio ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7c349-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="7c349-111">macOS, Linux ve Windows: [Visual Studio Code ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7c349-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7c349-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="7c349-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="7c349-113">Dotnet ile bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7c349-113">Create a web app with dotnet</span></span>

<span data-ttu-id="7c349-114">Bir terminalde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7c349-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="7c349-115">Açık *MvcMovie* klasör Visual Studio Code (VS Code) seçip *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="7c349-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

* <span data-ttu-id="7c349-116">Seçin **Evet** için **uyar** ileti "derleme ve hata ayıklamak için gerekli varlıkları 'MvcMovie' eksik.</span><span class="sxs-lookup"><span data-stu-id="7c349-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="7c349-117">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="7c349-117">Add them?"</span></span>
* <span data-ttu-id="7c349-118">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıklar vardır" iletisi.</span><span class="sxs-lookup"><span data-stu-id="7c349-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![Derleme ve hata ayıklamak için gerekli uyar varlıklar ile VS Code 'MvcMovie' eksik.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="7c349-122">Tuşuna **hata ayıklama** oluşturup programı çalıştırın (F5).</span><span class="sxs-lookup"><span data-stu-id="7c349-122">Press **Debug** (F5) to build and run the program.</span></span>

![Uygulamayı çalıştırma](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="7c349-124">VS Code başlar [Kestrel](xref:fundamentals/servers/kestrel) sunucu ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="7c349-124">VS Code starts [Kestrel](xref:fundamentals/servers/kestrel) server and runs your app.</span></span> <span data-ttu-id="7c349-125">Adres çubuğuna gösteren uyarı `localhost:5000` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="7c349-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="7c349-126">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="7c349-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="7c349-127">Varsayılan şablonu size çalışma **giriş**, **hakkında**, ve **kişi** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="7c349-127">The default template gives you working **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="7c349-128">Önceki tarayıcı resimde, bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="7c349-128">The preceding browser image doesn't show these links.</span></span> <span data-ttu-id="7c349-129">Tarayıcınız boyutuna bağlı olarak, gösterileceğinin Gezinti simgesine tıklamanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="7c349-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![sağ üstteki gezinti simgesi](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="7c349-131">Bu öğreticinin sonraki bölümünde, biz MVC konusunda bilgi ve biraz kod yazmaya.</span><span class="sxs-lookup"><span data-stu-id="7c349-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="7c349-132">Visual Studio Code Yardım</span><span class="sxs-lookup"><span data-stu-id="7c349-132">Visual Studio Code help</span></span>

* [<span data-ttu-id="7c349-133">Başlarken</span><span class="sxs-lookup"><span data-stu-id="7c349-133">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="7c349-134">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="7c349-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="7c349-135">Tümleşik terminal</span><span class="sxs-lookup"><span data-stu-id="7c349-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="7c349-136">Klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="7c349-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="7c349-137">macOS klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="7c349-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="7c349-138">Linux klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="7c349-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="7c349-139">Windows klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="7c349-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="7c349-140">Sonraki - denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="7c349-140">Next - Add a controller</span></span>](adding-controller.md)
