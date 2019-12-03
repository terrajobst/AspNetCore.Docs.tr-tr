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
ms.openlocfilehash: 4737747f84a59780fe70f63195f7580bd812e4de
ms.sourcegitcommit: 169ea5116de729c803685725d96450a270bc55b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74734028"
---
# <a name="whats-new-in-aspnet-core-31"></a><span data-ttu-id="efdb5-103">ASP.NET Core 3,1 ' deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="efdb5-103">What's new in ASP.NET Core 3.1</span></span>

<span data-ttu-id="efdb5-104">Bu makalede, ASP.NET Core 3,1 ' deki en önemli değişiklikler ilgili belgelerin bağlantılarıyla vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="efdb5-104">This article highlights the most significant changes in ASP.NET Core 3.1 with links to relevant documentation.</span></span>

## <a name="partial-class-support-for-razor-components"></a><span data-ttu-id="efdb5-105">Razor bileşenleri için kısmi sınıf desteği</span><span class="sxs-lookup"><span data-stu-id="efdb5-105">Partial class support for Razor components</span></span>

<span data-ttu-id="efdb5-106">Razor bileşenleri artık kısmi sınıflar olarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="efdb5-106">Razor components are now generated as partial classes.</span></span> <span data-ttu-id="efdb5-107">Bir Razor bileşeni kodu, tek bir dosyada bileşen için tüm kodu tanımlamak yerine kısmi bir sınıf olarak tanımlanmış bir arka plan kod dosyası kullanılarak yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="efdb5-107">Code for a Razor component can be written using a code-behind file defined as a partial class rather than defining all the code for the component in a single file.</span></span> <span data-ttu-id="efdb5-108">Daha fazla bilgi için bkz. [kısmi sınıf desteği](xref:blazor/components#partial-class-support).</span><span class="sxs-lookup"><span data-stu-id="efdb5-108">For more information, see [Partial class support](xref:blazor/components#partial-class-support).</span></span>

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a><span data-ttu-id="efdb5-109">Bileşen etiketi yardımcısını Blazor ve parametreleri en üst düzey bileşenlere geçir</span><span class="sxs-lookup"><span data-stu-id="efdb5-109">Blazor Component Tag Helper and pass parameters to top-level components</span></span>

<span data-ttu-id="efdb5-110">ASP.NET Core 3,0 ile Blazor, bileşenler HTML Yardımcısı (`Html.RenderComponentAsync`) kullanılarak sayfalar ve görünümler halinde işlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="efdb5-110">In Blazor with ASP.NET Core 3.0, components were rendered into pages and views using an HTML Helper (`Html.RenderComponentAsync`).</span></span> <span data-ttu-id="efdb5-111">ASP.NET Core 3,1 ' de, yeni bileşen etiketi Yardımcısı ile bir sayfadan veya görünümden bir bileşeni işleme:</span><span class="sxs-lookup"><span data-stu-id="efdb5-111">In ASP.NET Core 3.1, render a component from a page or view with the new Component Tag Helper:</span></span>

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="efdb5-112">HTML Yardımcısı ASP.NET Core 3,1 ' de desteklenmeye devam eder, ancak bileşen etiketi Yardımcısı önerilir.</span><span class="sxs-lookup"><span data-stu-id="efdb5-112">The HTML Helper remains supported in ASP.NET Core 3.1, but the Component Tag Helper is recommended.</span></span>

Blazor<span data-ttu-id="efdb5-113"> Server uygulamaları artık ilk işleme sırasında parametreleri en üst düzey bileşenlere geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="efdb5-113"> Server apps can now pass parameters to top-level components during the initial render.</span></span> <span data-ttu-id="efdb5-114">Daha önce, parametreleri yalnızca [RenderMode. static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static)olan en üst düzey bileşene geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="efdb5-114">Previously you could only pass parameters to a top-level component with [RenderMode.Static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static).</span></span> <span data-ttu-id="efdb5-115">Bu sürümle birlikte, hem [RenderMode. Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) hem de [Rendermodel. Serverprerenimli](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) desteklenir.</span><span class="sxs-lookup"><span data-stu-id="efdb5-115">With this release, both [RenderMode.Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) and [RenderModel.ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) are supported.</span></span> <span data-ttu-id="efdb5-116">Belirtilen parametre değerleri JSON olarak serileştirilir ve başlangıç yanıtına dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="efdb5-116">Any specified parameter values are serialized as JSON and included in the initial response.</span></span>

<span data-ttu-id="efdb5-117">Örneğin, PreRender bir artış miktarı olan `Counter` bileşen (`IncrementAmount`):</span><span class="sxs-lookup"><span data-stu-id="efdb5-117">For example, prerender a `Counter` component with an increment amount (`IncrementAmount`):</span></span>

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="efdb5-118">Daha fazla bilgi için bkz. [bileşenleri Razor Pages ve MVC uygulamalarıyla tümleştirme](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).</span><span class="sxs-lookup"><span data-stu-id="efdb5-118">For more information, see [Integrate components into Razor Pages and MVC apps](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).</span></span>

## <a name="support-for-shared-queues-in-httpsys"></a><span data-ttu-id="efdb5-119">HTTP. sys dosyasındaki paylaşılan sıralar için destek</span><span class="sxs-lookup"><span data-stu-id="efdb5-119">Support for shared queues in HTTP.sys</span></span>

<span data-ttu-id="efdb5-120">[Http. sys](xref:fundamentals/servers/httpsys) , anonim istek kuyrukları oluşturmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="efdb5-120">[HTTP.sys](xref:fundamentals/servers/httpsys) supports creating anonymous request queues.</span></span> <span data-ttu-id="efdb5-121">ASP.NET Core 3,1 ' de, var olan bir HTTP. sys istek kuyruğunu oluşturma veya ekleme yeteneğine ekledik.</span><span class="sxs-lookup"><span data-stu-id="efdb5-121">In ASP.NET Core 3.1, we’ve added to ability to create or attach to an existing named HTTP.sys request queue.</span></span> <span data-ttu-id="efdb5-122">Var olan adlandırılmış bir HTTP. sys istek kuyruğu oluşturma veya ekleme, HTTP 'nin bulunduğu senaryolara olanak sağlar. Kuyruğun sahibi olan sys denetleyicisi işlemi, dinleyici işleminden bağımsızdır.</span><span class="sxs-lookup"><span data-stu-id="efdb5-122">Creating or attaching to an existing named HTTP.sys request queue enables scenarios where the HTTP.Sys controller process that owns the queue is independent of the listener process.</span></span> <span data-ttu-id="efdb5-123">Bu bağımsızlık, var olan bağlantıları ve dinleyici işlemi yeniden başlatmalar arasında sıraya alınmış istekleri korumayı mümkün kılar:</span><span class="sxs-lookup"><span data-stu-id="efdb5-123">This independence makes it possible to preserve existing connections and enqueued requests between listener process restarts:</span></span>

[!code-csharp[](sample/Program.cs?name=snippet)]

<!-- TODO
## Breaking changes for SameSite cookies
-->

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a><span data-ttu-id="efdb5-124">Blazor uygulamalardaki olaylara yönelik varsayılan eylemleri engelleyin</span><span class="sxs-lookup"><span data-stu-id="efdb5-124">Prevent default actions for events in Blazor apps</span></span>

<span data-ttu-id="efdb5-125">Bir olayın varsayılan eylemini engellemek için `@on{EVENT}:preventDefault` Directive özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="efdb5-125">Use the `@on{EVENT}:preventDefault` directive attribute to prevent the default action for an event.</span></span> <span data-ttu-id="efdb5-126">Aşağıdaki örnekte, metin kutusunda anahtarın karakterini görüntülemenin varsayılan eylemi engellenir:</span><span class="sxs-lookup"><span data-stu-id="efdb5-126">In the following example, the default action of displaying the key's character in the text box is prevented:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

<span data-ttu-id="efdb5-127">Daha fazla bilgi için bkz. [varsayılan eylemleri engelleme](xref:blazor/components#prevent-default-actions).</span><span class="sxs-lookup"><span data-stu-id="efdb5-127">For more information, see [Prevent default actions](xref:blazor/components#prevent-default-actions).</span></span>

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a><span data-ttu-id="efdb5-128">Blazor uygulamalarında olay yaymayı durdur</span><span class="sxs-lookup"><span data-stu-id="efdb5-128">Stop event propagation in Blazor apps</span></span>

<span data-ttu-id="efdb5-129">Olay yaymayı durdurmak için `@on{EVENT}:stopPropagation` Directive özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="efdb5-129">Use the `@on{EVENT}:stopPropagation` directive attribute to stop event propagation.</span></span> <span data-ttu-id="efdb5-130">Aşağıdaki örnekte, onay kutusunun seçilmesi alt `<div>` üst `<div>`yayılmadan alt öğe için tıklama olaylarını önler:</span><span class="sxs-lookup"><span data-stu-id="efdb5-130">In the following example, selecting the check box prevents click events from the child `<div>` from propagating to the parent `<div>`:</span></span>

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

<span data-ttu-id="efdb5-131">Daha fazla bilgi için bkz. [olay yaymayı durdurma](xref:blazor/components#stop-event-propagation).</span><span class="sxs-lookup"><span data-stu-id="efdb5-131">For more information, see [Stop event propagation](xref:blazor/components#stop-event-propagation).</span></span>

## <a name="detailed-errors-during-opno-locblazor-app-development"></a><span data-ttu-id="efdb5-132">Blazor uygulama geliştirme sırasında ayrıntılı hatalar</span><span class="sxs-lookup"><span data-stu-id="efdb5-132">Detailed errors during Blazor app development</span></span>

<span data-ttu-id="efdb5-133">Geliştirme sırasında Blazor bir uygulama düzgün bir şekilde çalışmadığı zaman, uygulamanın ayrıntılı hata bilgilerinin alınması sorunu gidermeye ve sorunun giderilmesine yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="efdb5-133">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="efdb5-134">Bir hata oluştuğunda, Blazor uygulamalar ekranın alt kısmında altın bir çubuk görüntüler:</span><span class="sxs-lookup"><span data-stu-id="efdb5-134">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="efdb5-135">Geliştirme sırasında altın çubuk, özel durumu görebileceğiniz tarayıcı konsoluna yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="efdb5-135">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="efdb5-136">Üretimde, altın çubuk kullanıcıya bir hata oluştuğunu bildirir ve tarayıcıyı yenilemeyi önerir.</span><span class="sxs-lookup"><span data-stu-id="efdb5-136">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="efdb5-137">Daha fazla bilgi için bkz. [geliştirme sırasında ayrıntılı hatalar](xref:blazor/handle-errors#detailed-errors-during-development).</span><span class="sxs-lookup"><span data-stu-id="efdb5-137">For more information, see [Detailed errors during development](xref:blazor/handle-errors#detailed-errors-during-development).</span></span>
