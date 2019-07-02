---
title: ASP.NET Core Blazor hata ayıklama
author: guardrex
description: Blazor uygulamalarında hata ayıklama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: blazor/debug
ms.openlocfilehash: 6d71296417c57f01e675bdbb31a0d4fe2fd7db63
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500431"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="cd272-103">ASP.NET Core Blazor hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="cd272-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="cd272-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="cd272-104">Daniel Roth</span></span>](https://github.com/danroth27)

<span data-ttu-id="cd272-105">*Erken* destek var. WebAssembly chrome'da çalışan Blazor istemci-tarafı uygulamalarındaki hataları ayıklamak için.</span><span class="sxs-lookup"><span data-stu-id="cd272-105">*Early* support exists for debugging Blazor client-side apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="cd272-106">Hata ayıklayıcı özellikleri sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="cd272-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="cd272-107">Kullanılabilir senaryolar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="cd272-107">Available scenarios include:</span></span>

* <span data-ttu-id="cd272-108">Ayarlayın ve kesme noktalarını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="cd272-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="cd272-109">Tek adımlı (`F10`) aracılığıyla kod veya sürdürme (`F8`) kod yürütme.</span><span class="sxs-lookup"><span data-stu-id="cd272-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="cd272-110">İçinde *Yereller* görüntülemek, tür yerel değişkenlerin değerlerini gözlemleyin `int`, `string`, ve `bool`.</span><span class="sxs-lookup"><span data-stu-id="cd272-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="cd272-111">.NET içinde JavaScript ve .NET, JavaScript için Git çağrı zincirleri dahil olmak üzere çağrı yığını bakın.</span><span class="sxs-lookup"><span data-stu-id="cd272-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="cd272-112">*Olamaz*:</span><span class="sxs-lookup"><span data-stu-id="cd272-112">You *can't*:</span></span>

* <span data-ttu-id="cd272-113">Değerlerin değil herhangi bir yerel öğeler gözlemleyin bir `int`, `string`, veya `bool`.</span><span class="sxs-lookup"><span data-stu-id="cd272-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="cd272-114">Herhangi bir sınıf özelliklerini veya alanlarını değerlerini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="cd272-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="cd272-115">Değerleri görmek için değişkenleri gelin.</span><span class="sxs-lookup"><span data-stu-id="cd272-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="cd272-116">Konsolunda ifadeleri değerlendirin.</span><span class="sxs-lookup"><span data-stu-id="cd272-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="cd272-117">Zaman uyumsuz çağrıları boyunca ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="cd272-117">Step across async calls.</span></span>
* <span data-ttu-id="cd272-118">Çoğu diğer normal hata ayıklama senaryoları gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="cd272-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="cd272-119">Daha fazla hata ayıklama senaryoları biri geliştirme bir devam eden mühendislik ekibinin biridir.</span><span class="sxs-lookup"><span data-stu-id="cd272-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="procedure"></a><span data-ttu-id="cd272-120">Yordam</span><span class="sxs-lookup"><span data-stu-id="cd272-120">Procedure</span></span>

<span data-ttu-id="cd272-121">Chrome Blazor istemci-tarafı uygulamada hata ayıklamak için:</span><span class="sxs-lookup"><span data-stu-id="cd272-121">To debug a Blazor client-side app in Chrome:</span></span>

* <span data-ttu-id="cd272-122">Blazor uygulama yapı içinde `Debug` yapılandırma (yayımlanmamış uygulamalar için varsayılan).</span><span class="sxs-lookup"><span data-stu-id="cd272-122">Build a Blazor app in `Debug` configuration (the default for unpublished apps).</span></span>
* <span data-ttu-id="cd272-123">(Sürüm 70 veya sonrası) Chrome'da Blazor uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cd272-123">Run the Blazor app in Chrome (version 70 or later).</span></span>
* <span data-ttu-id="cd272-124">Uygulamasında (daha az karmaşık bir hata ayıklama deneyimi için büyük olasılıkla kapatmalısınız değil geliştirici araçları paneli,) klavye odağı ile aşağıdaki Blazor özgü klavye kısayolunu seçin:</span><span class="sxs-lookup"><span data-stu-id="cd272-124">With the keyboard focus on the app (not in the developer tools panel, which you should probably close for a less confusing debugging experience), select the following Blazor-specific keyboard shortcut:</span></span>
  * <span data-ttu-id="cd272-125">`Shift+Alt+D` Windows/Linux üzerinde</span><span class="sxs-lookup"><span data-stu-id="cd272-125">`Shift+Alt+D` on Windows/Linux</span></span>
  * <span data-ttu-id="cd272-126">`Shift+Cmd+D` MacOS üzerinde</span><span class="sxs-lookup"><span data-stu-id="cd272-126">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="cd272-127">Uzaktan hata ayıklamayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="cd272-127">Enable remote debugging</span></span>

<span data-ttu-id="cd272-128">Uzaktan hata ayıklama devre dışı ise bir **hata ayıklanabilir tarayıcı sekmesinde bulunamıyor** hata sayfası, Chrome tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cd272-128">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="cd272-129">Hata sayfası Blazor hata ayıklama proxy'si uygulamasına bağlanabilmesi Chrome ile hata ayıklama bağlantı noktası açık çalıştırmak için yönergeler içerir.</span><span class="sxs-lookup"><span data-stu-id="cd272-129">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="cd272-130">*Tüm Chrome örneklerini kapatın* ve Chrome belirtildiği gibi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="cd272-130">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="cd272-131">Uygulamasında hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="cd272-131">Debug the app</span></span>

<span data-ttu-id="cd272-132">Uzaktan hata ayıklama etkin olan Chrome çalıştırıldıktan sonra hata ayıklama klavye kısayolunu yeni bir hata ayıklayıcı sekmesi açılır. Kısa bir süre sonra **kaynakları** sekmesi, uygulamada .NET derlemelerin bir listesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cd272-132">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="cd272-133">Her derleme genişletin ve bulma *.cs*/ *.razor* kaynak dosyaları, hata ayıklama için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cd272-133">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="cd272-134">Kod yürütüldüğünde kesme noktaları belirleyin, uygulamanın sekmesine dönün ve kesme noktaları isabet.</span><span class="sxs-lookup"><span data-stu-id="cd272-134">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="cd272-135">Sonra bir kesme noktası İsabeti, tek adımlı olduğu (`F10`) aracılığıyla kod veya sürdürme (`F8`) yürütme normalde kod.</span><span class="sxs-lookup"><span data-stu-id="cd272-135">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

<span data-ttu-id="cd272-136">Blazor uygulayan bir hata ayıklama proxy'si sağlar [Chrome DevTools Protokolü](https://chromedevtools.github.io/devtools-protocol/) ve protokolü ile artırmaktadır. NET özgü bilgileri.</span><span class="sxs-lookup"><span data-stu-id="cd272-136">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="cd272-137">Hata ayıklama klavye kısayolu basıldığında Blazor Chrome DevTools proxy işaret eder.</span><span class="sxs-lookup"><span data-stu-id="cd272-137">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="cd272-138">Hata ayıklamak için aradığınız tarayıcı penceresine proxy bağlar (Bu nedenle uzaktan hata ayıklamayı etkinleştirmek için gereken).</span><span class="sxs-lookup"><span data-stu-id="cd272-138">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="cd272-139">Tarayıcı kaynak eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="cd272-139">Browser source maps</span></span>

<span data-ttu-id="cd272-140">Tarayıcı kaynak eşlemeleri derlenmiş dosyalar geri orijinal kaynak dosyalarına eşlemek tarayıcısına izin ver ve istemci tarafı hata ayıklama için yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd272-140">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="cd272-141">Ancak, Blazor şu anda eşlemiyor C# JavaScript/WASM için doğrudan.</span><span class="sxs-lookup"><span data-stu-id="cd272-141">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="cd272-142">Bunun yerine, kaynak eşlemeleri uygun olmayan şekilde Blazor IL yorumlayıcısı içinde tarayıcı yapar.</span><span class="sxs-lookup"><span data-stu-id="cd272-142">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="cd272-143">Sorun giderme İpucu</span><span class="sxs-lookup"><span data-stu-id="cd272-143">Troubleshooting tip</span></span>

<span data-ttu-id="cd272-144">Hatalarla karşılaşırsanız çalıştırıyorsanız, aşağıdaki ipucu yardımcı olabilir:</span><span class="sxs-lookup"><span data-stu-id="cd272-144">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="cd272-145">İçinde **hata ayıklayıcı** sekmesinde, tarayıcınızda geliştirici araçlarını açın.</span><span class="sxs-lookup"><span data-stu-id="cd272-145">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="cd272-146">Konsolda, yürütme `localStorage.clear()` tüm kesme noktalarını kaldırmak için.</span><span class="sxs-lookup"><span data-stu-id="cd272-146">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
