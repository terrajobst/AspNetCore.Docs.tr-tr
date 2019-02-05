---
title: Razor bileşenleri hata ayıklama
author: guardrex
description: Blazor ve Razor bileşenleri uygulamalarında hata ayıklama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/debug
ms.openlocfilehash: fb7ddcf3ae40ec28a372adf724a293b375be28a1
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668159"
---
# <a name="debug-razor-components"></a><span data-ttu-id="0713c-103">Razor bileşenleri hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="0713c-103">Debug Razor Components</span></span>

[<span data-ttu-id="0713c-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="0713c-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="0713c-105">Razor bileşenleri olan bazı *çok erken* WebAssembly chrome'da çalışan istemci-tarafı Blazor uygulamalarında hata ayıklamaya yönelik destek.</span><span class="sxs-lookup"><span data-stu-id="0713c-105">Razor Components has some *very early* support for debugging client-side Blazor apps running on WebAssembly in Chrome.</span></span> <span data-ttu-id="0713c-106">Bu ilk hata ayıklama desteği çok sınırlı ve unpolished olsa da, temel hata ayıklama altyapısını bir araya geldiği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0713c-106">While this initial debugging support is very limited and unpolished, it shows the basic debugging infrastructure coming together.</span></span>

<span data-ttu-id="0713c-107">Bir istemci-tarafı Blazor Chrome uygulamasında hata ayıklamak için:</span><span class="sxs-lookup"><span data-stu-id="0713c-107">To debug a client-side Blazor app in Chrome:</span></span>

* <span data-ttu-id="0713c-108">Blazor uygulama yapı içinde `Debug` yapılandırma (yayımlanmamış uygulamalar için varsayılan).</span><span class="sxs-lookup"><span data-stu-id="0713c-108">Build a Blazor app in `Debug` configuration (the default for non-published apps).</span></span>
* <span data-ttu-id="0713c-109">(Sürüm 70 veya sonrası) Chrome'da Blazor uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0713c-109">Run the Blazor app in Chrome (version 70 or later).</span></span>
* <span data-ttu-id="0713c-110">Uygulamasında (daha az karmaşık bir hata ayıklama deneyimi için büyük olasılıkla kapatmalısınız değil geliştirme araçları paneli,) klavye odağı ile aşağıdaki Blazor özgü klavye kısayolunu seçin:</span><span class="sxs-lookup"><span data-stu-id="0713c-110">With the keyboard focus on the app (not in the dev tools panel, which you should probably close for a less confusing debugging experience), select the following Blazor-specific keyboard shortcut:</span></span>
  * <span data-ttu-id="0713c-111">`Shift+Alt+D` Windows/Linux üzerinde</span><span class="sxs-lookup"><span data-stu-id="0713c-111">`Shift+Alt+D` on Windows/Linux</span></span>
  * <span data-ttu-id="0713c-112">`Shift+Cmd+D` MacOS üzerinde</span><span class="sxs-lookup"><span data-stu-id="0713c-112">`Shift+Cmd+D` on macOS</span></span>

<span data-ttu-id="0713c-113">Chrome uzak Blazor uygulamanın hatalarını ayıklamak için hata ayıklama etkin olan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0713c-113">Run Chrome with remote debugging enabled to debug the Blazor app.</span></span> <span data-ttu-id="0713c-114">Uzaktan hata ayıklama devre dışı bırakılırsa, bir hata sayfası Chrome tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0713c-114">If remote debugging is disabled, an error page is generated by Chrome.</span></span> <span data-ttu-id="0713c-115">Hata sayfası Blazor hata ayıklama proxy'si uygulamasına bağlanabilmesi Chrome ile hata ayıklama bağlantı noktası açık çalıştırmak için yönergeler içerir.</span><span class="sxs-lookup"><span data-stu-id="0713c-115">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="0713c-116">*Tüm Chrome örneklerini kapatın* ve Chrome belirtildiği gibi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="0713c-116">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

![Blazor hata ayıklama hata sayfası](https://user-images.githubusercontent.com/1874516/43123091-01ec0796-8ed8-11e8-844c-23b4e6e9d069.png)

<span data-ttu-id="0713c-118">Uzaktan hata ayıklama etkin olan Chrome çalıştırıldıktan sonra hata ayıklama klavye kısayolunu yeni bir hata ayıklayıcı sekmesi açılır. Kısa bir süre sonra *kaynakları* sekmesi, uygulamada .NET derlemelerin bir listesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="0713c-118">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the *Sources* tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="0713c-119">Her derleme genişletin ve bulma *.cs*/*.cshtml* kaynak dosyaları, hata ayıklama için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0713c-119">Expand each assembly and find the *.cs*/*.cshtml* source files available for debugging.</span></span> <span data-ttu-id="0713c-120">Kesme noktaları belirleyin, uygulamanın sekmesine dönün ve kesme noktaları isabet.</span><span class="sxs-lookup"><span data-stu-id="0713c-120">Set breakpoints, switch back to the app's tab, and the breakpoints are hit.</span></span> <span data-ttu-id="0713c-121">Sonra bir kesme noktası İsabeti, tek adımlı olduğu (`F10`) veya sürdürme (`F8`) normal şekilde.</span><span class="sxs-lookup"><span data-stu-id="0713c-121">After a breakpoint is hit, single-step (`F10`) or resume (`F8`) normally.</span></span>

![Blazor hata ayıklama](https://user-images.githubusercontent.com/1874516/43123060-efb0b3b0-8ed7-11e8-9ea5-97aa34247a0b.png)

<span data-ttu-id="0713c-123">Blazor uygulayan bir hata ayıklama proxy'si sağlar [Chrome DevTools Protokolü](https://chromedevtools.github.io/devtools-protocol/) ve protokolü ile artırmaktadır. NET özgü bilgileri.</span><span class="sxs-lookup"><span data-stu-id="0713c-123">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="0713c-124">Hata ayıklama klavye kısayolu basıldığında Blazor Chrome DevTools proxy işaret eder.</span><span class="sxs-lookup"><span data-stu-id="0713c-124">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="0713c-125">Hata ayıklamak için aradığınız tarayıcı penceresine proxy bağlar (Bu nedenle uzaktan hata ayıklamayı etkinleştirmek için gereken).</span><span class="sxs-lookup"><span data-stu-id="0713c-125">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

<span data-ttu-id="0713c-126">Neden biz yalnızca tarayıcı kaynak eşlemeleri kullanmayın merak ediyor olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0713c-126">You might be wondering why we don't just use browser source maps.</span></span> <span data-ttu-id="0713c-127">Kaynak eşlemeleri tarayıcının derlenmiş dosyalar geri orijinal kaynak dosyalarına eşleme izin verin.</span><span class="sxs-lookup"><span data-stu-id="0713c-127">Source maps allow the browser to map compiled files back to their original source files.</span></span> <span data-ttu-id="0713c-128">Blazor değil ancak harita C# JS/WASM (en az henüz) için doğrudan.</span><span class="sxs-lookup"><span data-stu-id="0713c-128">However, Blazor does not map C# directly to JS/WASM (at least not yet).</span></span> <span data-ttu-id="0713c-129">Bunun yerine, kaynak eşlemeleri uygun olmayan şekilde Blazor IL yorumlayıcısı içinde tarayıcı yapar.</span><span class="sxs-lookup"><span data-stu-id="0713c-129">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

<span data-ttu-id="0713c-130">Hata ayıklayıcı özellikleri Not **çok sınırlı**.</span><span class="sxs-lookup"><span data-stu-id="0713c-130">Note that the debugger capabilities are **very limited**.</span></span> <span data-ttu-id="0713c-131">Şu anda yalnızca yapabilecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="0713c-131">You can currently only:</span></span>

* <span data-ttu-id="0713c-132">Ayarlayın ve kesme noktalarını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="0713c-132">Set and remove breakpoints.</span></span>
* <span data-ttu-id="0713c-133">Tek-sürdürme ve kodu adımlayın (`F8`).</span><span class="sxs-lookup"><span data-stu-id="0713c-133">Single-step through the code or resume (`F8`).</span></span>
* <span data-ttu-id="0713c-134">İçinde *Yereller* görüntülemek, tür yerel değişkenlerin değerlerini gözlemleyin `int`, `string`, ve `bool`.</span><span class="sxs-lookup"><span data-stu-id="0713c-134">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="0713c-135">.NET içinde JavaScript ve .NET, JavaScript için Git çağrı zincirleri dahil olmak üzere çağrı yığını bakın.</span><span class="sxs-lookup"><span data-stu-id="0713c-135">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="0713c-136">*Olamaz*:</span><span class="sxs-lookup"><span data-stu-id="0713c-136">You *can't*:</span></span>

* <span data-ttu-id="0713c-137">Değerlerin değil herhangi bir yerel öğeler gözlemleyin bir `int`, `string`, veya `bool`.</span><span class="sxs-lookup"><span data-stu-id="0713c-137">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="0713c-138">Herhangi bir sınıf özelliklerini veya alanlarını değerlerini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="0713c-138">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="0713c-139">Değerleri görmek için değişkenler üzerinde gezdirin</span><span class="sxs-lookup"><span data-stu-id="0713c-139">Hover over variables to see their values</span></span>
* <span data-ttu-id="0713c-140">Konsolunda ifadeleri değerlendirin.</span><span class="sxs-lookup"><span data-stu-id="0713c-140">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="0713c-141">Zaman uyumsuz çağrıları boyunca ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="0713c-141">Step across async calls.</span></span>
* <span data-ttu-id="0713c-142">Çoğu diğer normal hata ayıklama senaryoları gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="0713c-142">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="0713c-143">Daha fazla hata ayıklama senaryoları biri geliştirme bir devam eden mühendislik ekibinin biridir.</span><span class="sxs-lookup"><span data-stu-id="0713c-143">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="0713c-144">Sorun giderme İpucu</span><span class="sxs-lookup"><span data-stu-id="0713c-144">Troubleshooting tip</span></span>

<span data-ttu-id="0713c-145">Hatalarla karşılaşırsanız çalıştırıyorsanız, aşağıdaki ipucu yardımcı olabilir:</span><span class="sxs-lookup"><span data-stu-id="0713c-145">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="0713c-146">İçinde **hata ayıklayıcı** sekmesinde, tarayıcınızda geliştirici araçlarını açın.</span><span class="sxs-lookup"><span data-stu-id="0713c-146">In the **debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="0713c-147">Konsolda, yürütme `localStorage.clear()` tüm kesme noktalarını kaldırmak için.</span><span class="sxs-lookup"><span data-stu-id="0713c-147">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>