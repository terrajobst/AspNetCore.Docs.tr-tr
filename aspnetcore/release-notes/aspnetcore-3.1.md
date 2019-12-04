---
title: ASP.NET Core 3,1 ' deki yenilikler
author: rick-anderson
description: ASP.NET Core 3,1 ' deki yeni özellikler hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.1
ms.openlocfilehash: 634c6937089a0a0fe1f862a83771aff65a1f8418
ms.sourcegitcommit: 5974e3e66dab3398ecf2324fbb82a9c5636f70de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74778849"
---
# <a name="whats-new-in-aspnet-core-31"></a><span data-ttu-id="46aad-103">ASP.NET Core 3,1 ' deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="46aad-103">What's new in ASP.NET Core 3.1</span></span>

<span data-ttu-id="46aad-104">Bu makalede, ASP.NET Core 3,1 ' deki en önemli değişiklikler ilgili belgelerin bağlantılarıyla vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="46aad-104">This article highlights the most significant changes in ASP.NET Core 3.1 with links to relevant documentation.</span></span>

## <a name="partial-class-support-for-razor-components"></a><span data-ttu-id="46aad-105">Razor bileşenleri için kısmi sınıf desteği</span><span class="sxs-lookup"><span data-stu-id="46aad-105">Partial class support for Razor components</span></span>

<span data-ttu-id="46aad-106">Razor bileşenleri artık kısmi sınıflar olarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="46aad-106">Razor components are now generated as partial classes.</span></span> <span data-ttu-id="46aad-107">Bir Razor bileşeni kodu, tek bir dosyada bileşen için tüm kodu tanımlamak yerine kısmi bir sınıf olarak tanımlanmış bir arka plan kod dosyası kullanılarak yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="46aad-107">Code for a Razor component can be written using a code-behind file defined as a partial class rather than defining all the code for the component in a single file.</span></span> <span data-ttu-id="46aad-108">Daha fazla bilgi için bkz. [kısmi sınıf desteği](xref:blazor/components#partial-class-support).</span><span class="sxs-lookup"><span data-stu-id="46aad-108">For more information, see [Partial class support](xref:blazor/components#partial-class-support).</span></span>

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a><span data-ttu-id="46aad-109">Bileşen etiketi yardımcısını Blazor ve parametreleri en üst düzey bileşenlere geçir</span><span class="sxs-lookup"><span data-stu-id="46aad-109">Blazor Component Tag Helper and pass parameters to top-level components</span></span>

<span data-ttu-id="46aad-110">ASP.NET Core 3,0 ile Blazor, bileşenler HTML Yardımcısı (`Html.RenderComponentAsync`) kullanılarak sayfalar ve görünümler halinde işlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="46aad-110">In Blazor with ASP.NET Core 3.0, components were rendered into pages and views using an HTML Helper (`Html.RenderComponentAsync`).</span></span> <span data-ttu-id="46aad-111">ASP.NET Core 3,1 ' de, yeni bileşen etiketi Yardımcısı ile bir sayfadan veya görünümden bir bileşeni işleme:</span><span class="sxs-lookup"><span data-stu-id="46aad-111">In ASP.NET Core 3.1, render a component from a page or view with the new Component Tag Helper:</span></span>

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="46aad-112">HTML Yardımcısı ASP.NET Core 3,1 ' de desteklenmeye devam eder, ancak bileşen etiketi Yardımcısı önerilir.</span><span class="sxs-lookup"><span data-stu-id="46aad-112">The HTML Helper remains supported in ASP.NET Core 3.1, but the Component Tag Helper is recommended.</span></span>

Blazor<span data-ttu-id="46aad-113"> Server uygulamaları artık ilk işleme sırasında parametreleri en üst düzey bileşenlere geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="46aad-113"> Server apps can now pass parameters to top-level components during the initial render.</span></span> <span data-ttu-id="46aad-114">Daha önce, parametreleri yalnızca [RenderMode. static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static)olan en üst düzey bileşene geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46aad-114">Previously you could only pass parameters to a top-level component with [RenderMode.Static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static).</span></span> <span data-ttu-id="46aad-115">Bu sürümle birlikte, hem [RenderMode. Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) hem de [Rendermodel. Serverprerenimli](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) desteklenir.</span><span class="sxs-lookup"><span data-stu-id="46aad-115">With this release, both [RenderMode.Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) and [RenderModel.ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) are supported.</span></span> <span data-ttu-id="46aad-116">Belirtilen parametre değerleri JSON olarak serileştirilir ve başlangıç yanıtına dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="46aad-116">Any specified parameter values are serialized as JSON and included in the initial response.</span></span>

<span data-ttu-id="46aad-117">Örneğin, PreRender bir artış miktarı olan `Counter` bileşen (`IncrementAmount`):</span><span class="sxs-lookup"><span data-stu-id="46aad-117">For example, prerender a `Counter` component with an increment amount (`IncrementAmount`):</span></span>

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="46aad-118">Daha fazla bilgi için bkz. [bileşenleri Razor Pages ve MVC uygulamalarıyla tümleştirme](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).</span><span class="sxs-lookup"><span data-stu-id="46aad-118">For more information, see [Integrate components into Razor Pages and MVC apps](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).</span></span>

## <a name="support-for-shared-queues-in-httpsys"></a><span data-ttu-id="46aad-119">HTTP. sys dosyasındaki paylaşılan sıralar için destek</span><span class="sxs-lookup"><span data-stu-id="46aad-119">Support for shared queues in HTTP.sys</span></span>

<span data-ttu-id="46aad-120">[Http. sys](xref:fundamentals/servers/httpsys) , anonim istek kuyrukları oluşturmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="46aad-120">[HTTP.sys](xref:fundamentals/servers/httpsys) supports creating anonymous request queues.</span></span> <span data-ttu-id="46aad-121">ASP.NET Core 3,1 ' de, var olan bir HTTP. sys istek kuyruğunu oluşturma veya ekleme yeteneğine ekledik.</span><span class="sxs-lookup"><span data-stu-id="46aad-121">In ASP.NET Core 3.1, we’ve added to ability to create or attach to an existing named HTTP.sys request queue.</span></span> <span data-ttu-id="46aad-122">Var olan adlandırılmış bir HTTP. sys istek kuyruğu oluşturma veya ekleme, HTTP 'nin bulunduğu senaryolara olanak sağlar. Kuyruğun sahibi olan sys denetleyicisi işlemi, dinleyici işleminden bağımsızdır.</span><span class="sxs-lookup"><span data-stu-id="46aad-122">Creating or attaching to an existing named HTTP.sys request queue enables scenarios where the HTTP.Sys controller process that owns the queue is independent of the listener process.</span></span> <span data-ttu-id="46aad-123">Bu bağımsızlık, var olan bağlantıları ve dinleyici işlemi yeniden başlatmalar arasında sıraya alınmış istekleri korumayı mümkün kılar:</span><span class="sxs-lookup"><span data-stu-id="46aad-123">This independence makes it possible to preserve existing connections and enqueued requests between listener process restarts:</span></span>

[!code-csharp[](sample/Program.cs?name=snippet)]

## <a name="breaking-changes-for-samesite-cookies"></a><span data-ttu-id="46aad-124">SameSite tanımlama bilgilerine yönelik son değişiklikler</span><span class="sxs-lookup"><span data-stu-id="46aad-124">Breaking changes for SameSite cookies</span></span>

<span data-ttu-id="46aad-125">SameSite tanımlama bilgilerinin davranışı yaklaşan tarayıcı değişikliklerini yansıtacak şekilde değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="46aad-125">The behavior of SameSite cookies has changed to reflect upcoming browser changes.</span></span> <span data-ttu-id="46aad-126">Bu, AzureAd, Openıdconnect veya WsFederation gibi kimlik doğrulama senaryolarını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="46aad-126">This may affect authentication scenarios like AzureAd, OpenIdConnect, or WsFederation.</span></span> <span data-ttu-id="46aad-127">Daha fazla bilgi için bkz. <xref:security/samesite>.</span><span class="sxs-lookup"><span data-stu-id="46aad-127">For more information, see <xref:security/samesite>.</span></span>

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a><span data-ttu-id="46aad-128">Blazor uygulamalardaki olaylara yönelik varsayılan eylemleri engelleyin</span><span class="sxs-lookup"><span data-stu-id="46aad-128">Prevent default actions for events in Blazor apps</span></span>

<span data-ttu-id="46aad-129">Bir olayın varsayılan eylemini engellemek için `@on{EVENT}:preventDefault` Directive özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="46aad-129">Use the `@on{EVENT}:preventDefault` directive attribute to prevent the default action for an event.</span></span> <span data-ttu-id="46aad-130">Aşağıdaki örnekte, metin kutusunda anahtarın karakterini görüntülemenin varsayılan eylemi engellenir:</span><span class="sxs-lookup"><span data-stu-id="46aad-130">In the following example, the default action of displaying the key's character in the text box is prevented:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

<span data-ttu-id="46aad-131">Daha fazla bilgi için bkz. [varsayılan eylemleri engelleme](xref:blazor/components#prevent-default-actions).</span><span class="sxs-lookup"><span data-stu-id="46aad-131">For more information, see [Prevent default actions](xref:blazor/components#prevent-default-actions).</span></span>

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a><span data-ttu-id="46aad-132">Blazor uygulamalarında olay yaymayı durdur</span><span class="sxs-lookup"><span data-stu-id="46aad-132">Stop event propagation in Blazor apps</span></span>

<span data-ttu-id="46aad-133">Olay yaymayı durdurmak için `@on{EVENT}:stopPropagation` Directive özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="46aad-133">Use the `@on{EVENT}:stopPropagation` directive attribute to stop event propagation.</span></span> <span data-ttu-id="46aad-134">Aşağıdaki örnekte, onay kutusunun seçilmesi alt `<div>` üst `<div>`yayılmadan alt öğe için tıklama olaylarını önler:</span><span class="sxs-lookup"><span data-stu-id="46aad-134">In the following example, selecting the check box prevents click events from the child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<input @bind="_stopPropagation" type="checkbox" />

<div @onclick="OnSelectParentDiv">
    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        ...
    </div>
</div>

@code {
    private bool _stopPropagation = false;
}
```

<span data-ttu-id="46aad-135">Daha fazla bilgi için bkz. [olay yaymayı durdurma](xref:blazor/components#stop-event-propagation).</span><span class="sxs-lookup"><span data-stu-id="46aad-135">For more information, see [Stop event propagation](xref:blazor/components#stop-event-propagation).</span></span>

## <a name="detailed-errors-during-opno-locblazor-app-development"></a><span data-ttu-id="46aad-136">Blazor uygulama geliştirme sırasında ayrıntılı hatalar</span><span class="sxs-lookup"><span data-stu-id="46aad-136">Detailed errors during Blazor app development</span></span>

<span data-ttu-id="46aad-137">Geliştirme sırasında Blazor bir uygulama düzgün bir şekilde çalışmadığı zaman, uygulamanın ayrıntılı hata bilgilerinin alınması sorunu gidermeye ve sorunun giderilmesine yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="46aad-137">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="46aad-138">Bir hata oluştuğunda, Blazor uygulamalar ekranın alt kısmında altın bir çubuk görüntüler:</span><span class="sxs-lookup"><span data-stu-id="46aad-138">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="46aad-139">Geliştirme sırasında altın çubuk, özel durumu görebileceğiniz tarayıcı konsoluna yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="46aad-139">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="46aad-140">Üretimde, altın çubuk kullanıcıya bir hata oluştuğunu bildirir ve tarayıcıyı yenilemeyi önerir.</span><span class="sxs-lookup"><span data-stu-id="46aad-140">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="46aad-141">Daha fazla bilgi için bkz. [geliştirme sırasında ayrıntılı hatalar](xref:blazor/handle-errors#detailed-errors-during-development).</span><span class="sxs-lookup"><span data-stu-id="46aad-141">For more information, see [Detailed errors during development](xref:blazor/handle-errors#detailed-errors-during-development).</span></span>
