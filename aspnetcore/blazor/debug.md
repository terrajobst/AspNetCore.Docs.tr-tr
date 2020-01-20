---
title: Hata ayıklama ASP.NET Core Blazor
author: guardrex
description: Blazor uygulamalarda hata ayıklamayı öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159995"
---
# <a name="debug-aspnet-core-opno-locblazor"></a><span data-ttu-id="a88bf-103">Hata ayıklama ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="a88bf-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="a88bf-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="a88bf-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a88bf-105">Kmıum tabanlı tarayıcılarda (Chrome/Edge) tarayıcı geliştirme araçlarını kullanarak Weelsembly Blazor hata ayıklama için *erken* destek mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="a88bf-105">*Early* support exists for debugging Blazor WebAssembly using the browser dev tools in Chromium-based browsers (Chrome/Edge).</span></span> <span data-ttu-id="a88bf-106">İş şu şekilde devam ediyor:</span><span class="sxs-lookup"><span data-stu-id="a88bf-106">Work is in progress to:</span></span>

* <span data-ttu-id="a88bf-107">Visual Studio 'da hata ayıklamayı tamamen etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-107">Fully enable debugging in Visual Studio.</span></span>
* <span data-ttu-id="a88bf-108">Visual Studio Code hata ayıklamayı etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="a88bf-108">Enable debugging in Visual Studio Code.</span></span>

<span data-ttu-id="a88bf-109">Hata ayıklayıcı özellikleri sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="a88bf-109">Debugger capabilities are limited.</span></span> <span data-ttu-id="a88bf-110">Kullanılabilir senaryolar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a88bf-110">Available scenarios include:</span></span>

* <span data-ttu-id="a88bf-111">Kesme noktaları ayarlayın ve kaldırın.</span><span class="sxs-lookup"><span data-stu-id="a88bf-111">Set and remove breakpoints.</span></span>
* <span data-ttu-id="a88bf-112">Kod yürütme (`F8`) kodu üzerinden tek adımlı (`F10`).</span><span class="sxs-lookup"><span data-stu-id="a88bf-112">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="a88bf-113">*Yereller* görünümü ' nde, `int`, `string`ve `bool`türündeki tüm yerel değişkenlerin değerlerini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-113">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="a88bf-114">JavaScript 'ten .NET 'e ve .NET 'ten JavaScript 'e gidecek çağrı zincirleri dahil olmak üzere çağrı yığınına bakın.</span><span class="sxs-lookup"><span data-stu-id="a88bf-114">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="a88bf-115">Şunları *yapamazsınız*:</span><span class="sxs-lookup"><span data-stu-id="a88bf-115">You *can't*:</span></span>

* <span data-ttu-id="a88bf-116">`int`, `string`veya `bool`olmayan herhangi bir yerelin değerlerini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-116">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="a88bf-117">Herhangi bir sınıf özelliklerinin veya alanının değerlerini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-117">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="a88bf-118">Değerlerini görmek için değişkenlerin üzerine gelin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-118">Hover over variables to see their values.</span></span>
* <span data-ttu-id="a88bf-119">Konsolunda ifadeleri değerlendirin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-119">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="a88bf-120">Zaman uyumsuz çağrılar arasında adımla.</span><span class="sxs-lookup"><span data-stu-id="a88bf-120">Step across async calls.</span></span>
* <span data-ttu-id="a88bf-121">Diğer birçok sıradan hata ayıklama senaryosunu gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-121">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="a88bf-122">Daha fazla hata ayıklama senaryosu geliştirmesi, mühendislik ekibinin bir odadır.</span><span class="sxs-lookup"><span data-stu-id="a88bf-122">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a88bf-123">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="a88bf-123">Prerequisites</span></span>

<span data-ttu-id="a88bf-124">Hata ayıklama aşağıdaki tarayıcılardan birini gerektirir:</span><span class="sxs-lookup"><span data-stu-id="a88bf-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="a88bf-125">Google Chrome (sürüm 70 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="a88bf-125">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="a88bf-126">Microsoft Edge Önizlemesi ([Edge geliştirme kanalı](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="a88bf-126">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="a88bf-127">Yordam</span><span class="sxs-lookup"><span data-stu-id="a88bf-127">Procedure</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a88bf-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a88bf-128">Visual Studio</span></span>](#tab/visual-studio)

> [!WARNING]
> <span data-ttu-id="a88bf-129">Visual Studio 'da hata ayıklama desteği, daha erken bir geliştirme aşamasıdır.</span><span class="sxs-lookup"><span data-stu-id="a88bf-129">Debugging support in Visual Studio is at an early stage of development.</span></span> <span data-ttu-id="a88bf-130">**F5** hata ayıklaması Şu anda desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="a88bf-130">**F5** debugging isn't currently supported.</span></span>

1. <span data-ttu-id="a88bf-131">Hata ayıklama olmadan `Debug` yapılandırmasında bir Blazor WebAssembly uygulaması çalıştırın ( **F5**yerine**F5+** **F5** ).</span><span class="sxs-lookup"><span data-stu-id="a88bf-131">Run a Blazor WebAssembly app in `Debug` configuration without debugging (**Ctrl**+**F5** instead of **F5**).</span></span>
1. <span data-ttu-id="a88bf-132">Uygulamanın hata ayıklama özelliklerini açın ( **hata ayıklama** menüsünde son giriş) ve http **Uygulama URL**'sini kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="a88bf-132">Open the Debug properties of the app (last entry in the **Debug** menu) and copy the HTTP **App URL**.</span></span> <span data-ttu-id="a88bf-133">Bir Kmıum tabanlı tarayıcı (Edge beta veya Chrome) kullanarak uygulamanın HTTP adresine (HTTPS adresine değil) göz atabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a88bf-133">Browse to the HTTP address (not the HTTPS address) of the app using a Chromium-based browser (Edge Beta or Chrome).</span></span>
1. <span data-ttu-id="a88bf-134">Klavye odağını geliştirici araçları paneline değil tarayıcı penceresinde uygulamaya yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-134">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span> <span data-ttu-id="a88bf-135">Geliştirici araçları bölmesinin Bu yordam için kapalı tutulması en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="a88bf-135">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="a88bf-136">Hata ayıklama başlatıldıktan sonra geliştirici araçları panelini yeniden açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a88bf-136">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="a88bf-137">Aşağıdaki Blazorözgü klavye kısayolunu seçin:</span><span class="sxs-lookup"><span data-stu-id="a88bf-137">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="a88bf-138">Windows üzerinde `Shift+Alt+D`</span><span class="sxs-lookup"><span data-stu-id="a88bf-138">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="a88bf-139">macOS üzerinde `Shift+Cmd+D`</span><span class="sxs-lookup"><span data-stu-id="a88bf-139">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="a88bf-140">**Hata ayıklanabilir tarayıcısı bulunamıyor sekmesine**sahipseniz bkz. [Uzaktan hata ayıklamayı etkinleştir](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="a88bf-140">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="a88bf-141">Uzaktan hata ayıklamayı etkinleştirdikten sonra:</span><span class="sxs-lookup"><span data-stu-id="a88bf-141">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="a88bf-142">1 \.</span><span class="sxs-lookup"><span data-stu-id="a88bf-142">1\.</span></span> <span data-ttu-id="a88bf-143">Yeni bir tarayıcı penceresi açılır.</span><span class="sxs-lookup"><span data-stu-id="a88bf-143">A new browser window opens.</span></span> <span data-ttu-id="a88bf-144">Önceki pencereyi kapatın.</span><span class="sxs-lookup"><span data-stu-id="a88bf-144">Close the prior window.</span></span>

   <span data-ttu-id="a88bf-145">2 \.</span><span class="sxs-lookup"><span data-stu-id="a88bf-145">2\.</span></span> <span data-ttu-id="a88bf-146">Klavye odağını uygulamayı tarayıcı penceresinde yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-146">Place the keyboard focus on the app in the browser window.</span></span>

   <span data-ttu-id="a88bf-147">3 \.</span><span class="sxs-lookup"><span data-stu-id="a88bf-147">3\.</span></span> <span data-ttu-id="a88bf-148">Yeni tarayıcı penceresinde Blazorözgü klavye kısayolunu seçin: Windows üzerinde `Shift+Alt+D` veya macOS üzerinde `Shift+Cmd+D`.</span><span class="sxs-lookup"><span data-stu-id="a88bf-148">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

   <span data-ttu-id="a88bf-149">4 \.</span><span class="sxs-lookup"><span data-stu-id="a88bf-149">4\.</span></span> <span data-ttu-id="a88bf-150">**Devtools** sekmesi tarayıcıda açılır.</span><span class="sxs-lookup"><span data-stu-id="a88bf-150">The **DevTools** tab opens in the browser.</span></span> <span data-ttu-id="a88bf-151">**Tarayıcı penceresinde uygulamanın sekmesini yeniden seçin.**</span><span class="sxs-lookup"><span data-stu-id="a88bf-151">**Reselect the app's tab in the browser window.**</span></span>

   <span data-ttu-id="a88bf-152">Uygulamayı Visual Studio 'ya eklemek için, [Visual Studio 'da Işleme iliştirme](#attach-to-process-in-visual-studio) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a88bf-152">To attach the app to Visual Studio, see the [Attach to process in Visual Studio](#attach-to-process-in-visual-studio) section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a88bf-153">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a88bf-153">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="a88bf-154">`--configuration Debug` seçeneğini [DotNet Run](/dotnet/core/tools/dotnet-run) komutuna geçirerek `Debug` yapılandırmasında bir Blazor WebAssembly uygulaması çalıştırın: `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="a88bf-154">Run a Blazor WebAssembly app in `Debug` configuration by passing the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="a88bf-155">Kabuk penceresinde gösterilen HTTP URL 'sindeki uygulamaya gidin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-155">Navigate to the app at the HTTP URL shown in the shell's window.</span></span>
1. <span data-ttu-id="a88bf-156">Klavye odağını geliştirici araçları paneline değil uygulamaya yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-156">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="a88bf-157">Geliştirici araçları bölmesinin Bu yordam için kapalı tutulması en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="a88bf-157">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="a88bf-158">Hata ayıklama başlatıldıktan sonra geliştirici araçları panelini yeniden açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a88bf-158">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="a88bf-159">Aşağıdaki Blazorözgü klavye kısayolunu seçin:</span><span class="sxs-lookup"><span data-stu-id="a88bf-159">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="a88bf-160">Windows üzerinde `Shift+Alt+D`</span><span class="sxs-lookup"><span data-stu-id="a88bf-160">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="a88bf-161">macOS üzerinde `Shift+Cmd+D`</span><span class="sxs-lookup"><span data-stu-id="a88bf-161">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="a88bf-162">**Hata ayıklanabilir tarayıcısı bulunamıyor sekmesine**sahipseniz bkz. [Uzaktan hata ayıklamayı etkinleştir](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="a88bf-162">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="a88bf-163">Uzaktan hata ayıklamayı etkinleştirdikten sonra:</span><span class="sxs-lookup"><span data-stu-id="a88bf-163">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="a88bf-164">1 \.</span><span class="sxs-lookup"><span data-stu-id="a88bf-164">1\.</span></span> <span data-ttu-id="a88bf-165">Yeni bir tarayıcı penceresi açılır.</span><span class="sxs-lookup"><span data-stu-id="a88bf-165">A new browser window opens.</span></span> <span data-ttu-id="a88bf-166">Önceki pencereyi kapatın.</span><span class="sxs-lookup"><span data-stu-id="a88bf-166">Close the prior window.</span></span>

   <span data-ttu-id="a88bf-167">2 \.</span><span class="sxs-lookup"><span data-stu-id="a88bf-167">2\.</span></span> <span data-ttu-id="a88bf-168">Klavye odağını geliştirici araçları paneline değil tarayıcı penceresinde uygulamaya yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-168">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span>

   <span data-ttu-id="a88bf-169">3 \.</span><span class="sxs-lookup"><span data-stu-id="a88bf-169">3\.</span></span> <span data-ttu-id="a88bf-170">Yeni tarayıcı penceresinde Blazorözgü klavye kısayolunu seçin: Windows üzerinde `Shift+Alt+D` veya macOS üzerinde `Shift+Cmd+D`.</span><span class="sxs-lookup"><span data-stu-id="a88bf-170">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

---

## <a name="enable-remote-debugging"></a><span data-ttu-id="a88bf-171">Uzaktan hata ayıklamayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="a88bf-171">Enable remote debugging</span></span>

<span data-ttu-id="a88bf-172">Uzaktan hata ayıklama devre dışıysa, **hata ayıklanabilir Browser sekmesi** hata sayfası Chrome tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a88bf-172">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="a88bf-173">Hata sayfası, hata ayıklama proxy 'sinin uygulamaya bağlanabilmesi için hata ayıklama bağlantı noktası Blazor açıkken Chrome çalıştırmaya yönelik yönergeler içerir.</span><span class="sxs-lookup"><span data-stu-id="a88bf-173">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="a88bf-174">*Tüm Chrome örneklerini kapatın* ve belirtildiği şekilde Chrome 'u yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="a88bf-174">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="a88bf-175">Uygulamada hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a88bf-175">Debug the app</span></span>

<span data-ttu-id="a88bf-176">Uzaktan hata ayıklama etkinken Chrome çalışmaya başladıktan sonra hata ayıklama klavye kısayolu yeni bir hata ayıklayıcı sekmesi açar. Bir süre sonra, **kaynaklar** sekmesi uygulamadaki .net derlemelerinin listesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a88bf-176">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="a88bf-177">Her bir derlemeyi genişletin ve hata ayıklama için kullanılabilen *. cs*/ *. Razor* kaynak dosyalarını bulun.</span><span class="sxs-lookup"><span data-stu-id="a88bf-177">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="a88bf-178">Kesme noktaları ayarlayın, uygulamanın sekmesine geri dönün ve kod yürütüldüğünde kesme noktaları isabet edilir.</span><span class="sxs-lookup"><span data-stu-id="a88bf-178">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="a88bf-179">Kesme noktası isabet ettikten sonra kod veya özgeçmişi (`F8`) kod yürütme aracılığıyla tek adımlı (`F10`).</span><span class="sxs-lookup"><span data-stu-id="a88bf-179">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="a88bf-180">, [Chrome DevTools protokolünü](https://chromedevtools.github.io/devtools-protocol/) uygulayan ve protokolünü ile genişletgetiren bir hata ayıklama proxy 'si sağlar. NET 'e özgü bilgiler.</span><span class="sxs-lookup"><span data-stu-id="a88bf-180"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="a88bf-181">Klavye kısayolunun hata ayıklaması basıldığında Blazor, ara sunucu üzerindeki Chrome DevTools 'u gösterir.</span><span class="sxs-lookup"><span data-stu-id="a88bf-181">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="a88bf-182">Proxy, hata ayıklama işlemini Aradığınız tarayıcı penceresine bağlanır (Bu nedenle, uzaktan hata ayıklamayı etkinleştirmeniz gerekir).</span><span class="sxs-lookup"><span data-stu-id="a88bf-182">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="attach-to-process-in-visual-studio"></a><span data-ttu-id="a88bf-183">Visual Studio 'da işleme iliştirme</span><span class="sxs-lookup"><span data-stu-id="a88bf-183">Attach to process in Visual Studio</span></span>

<span data-ttu-id="a88bf-184">Visual Studio 'da uygulamanın sürecine iliştirme, **F5** hata ayıklaması geliştirilirken Blazor WebAssembly için *geçici* bir hata ayıklama senaryosudur.</span><span class="sxs-lookup"><span data-stu-id="a88bf-184">Attaching to the app's process in Visual Studio is a *temporary* debugging scenario for Blazor WebAssembly while **F5** debugging is in development.</span></span>

<span data-ttu-id="a88bf-185">Çalışan uygulamanın işlemini Visual Studio 'ya eklemek için:</span><span class="sxs-lookup"><span data-stu-id="a88bf-185">To attach the running app's process to Visual Studio:</span></span>

1. <span data-ttu-id="a88bf-186">Visual Studio 'da, **Işleme eklemek** > **Hata Ayıkla** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-186">In Visual Studio, select **Debug** > **Attach to Process**.</span></span>
1. <span data-ttu-id="a88bf-187">**Bağlantı türü**için **Chrome DevTools protokol WebSocket (kimlik doğrulaması yok)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-187">For the **Connection type**, select **Chrome devtools protocol websocket (no authentication)**.</span></span>
1. <span data-ttu-id="a88bf-188">**Bağlantı hedefi**için, uygulamanın http ADRESINI (https adresini değil) yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="a88bf-188">For the **Connection target**, paste in the HTTP address (not the HTTPS address) of the app.</span></span>
1. <span data-ttu-id="a88bf-189">**Kullanılabilir süreçler**altındaki girdileri yenilemek için **Yenile** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-189">Select **Refresh** to refresh the entries under **Available processes**.</span></span>
1. <span data-ttu-id="a88bf-190">Hata ayıklama için tarayıcı işlemini seçin ve **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-190">Select the browser process to debug and select **Attach**.</span></span>
1. <span data-ttu-id="a88bf-191">**Kod türünü seç** iletişim kutusunda, iliştirmekte olduğunuz belirli tarayıcıya ait kod türünü seçin (kenar veya Chrome) ve ardından **Tamam**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="a88bf-191">In the **Select Code Type** dialog, select the code type for the specific browser you're attaching to (Edge or Chrome) and then select **OK**.</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="a88bf-192">Tarayıcı kaynağı eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="a88bf-192">Browser source maps</span></span>

<span data-ttu-id="a88bf-193">Tarayıcı kaynak haritaları tarayıcının derlenmiş dosyaları özgün kaynak dosyalarına geri eşlemesine ve istemci tarafı hata ayıklama için yaygın olarak kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="a88bf-193">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="a88bf-194">Ancak, Blazor Şu anda doğrudan C# JavaScript/, ile eşlenmiyor.</span><span class="sxs-lookup"><span data-stu-id="a88bf-194">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="a88bf-195">Bunun yerine, Blazor, kaynak haritaları ilgili değil, tarayıcı içinde Il yorumu yapar.</span><span class="sxs-lookup"><span data-stu-id="a88bf-195">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="a88bf-196">Sorunları Gider</span><span class="sxs-lookup"><span data-stu-id="a88bf-196">Troubleshoot</span></span>

<span data-ttu-id="a88bf-197">Hatalar halinde çalıştırıyorsanız, aşağıdaki ipucu yardımcı olabilir:</span><span class="sxs-lookup"><span data-stu-id="a88bf-197">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="a88bf-198">**Hata ayıklayıcı** sekmesinde, tarayıcınızda Geliştirici Araçları ' nı açın.</span><span class="sxs-lookup"><span data-stu-id="a88bf-198">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="a88bf-199">Konsolunda, tüm kesme noktalarını kaldırmak için `localStorage.clear()` yürütün.</span><span class="sxs-lookup"><span data-stu-id="a88bf-199">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
