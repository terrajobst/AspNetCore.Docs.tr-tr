---
title: ASP.NET Core Blazor barındırma modeli yapılandırması
author: guardrex
description: Razor bileşenlerini Razor Pages ve MVC uygulamalarına tümleştirme dahil olmak üzere Blazor barındırma modeli yapılandırması hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: bd44643877e45c5b48b0972bcc2f637fbc5d98f2
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658307"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="de979-103">ASP.NET Core Blazor barındırma modeli yapılandırması</span><span class="sxs-lookup"><span data-stu-id="de979-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="de979-104">[Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="de979-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="de979-105">Bu makalede barındırma modeli yapılandırması ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="de979-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="de979-106">Blazor Server</span><span class="sxs-lookup"><span data-stu-id="de979-106">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="de979-107">Kullanıcı arabirimindeki bağlantı durumunu yansıtır</span><span class="sxs-lookup"><span data-stu-id="de979-107">Reflect the connection state in the UI</span></span>

<span data-ttu-id="de979-108">İstemci bağlantının kaybolduğunu algıladığında, istemci yeniden bağlanmayı denediğinde kullanıcıya varsayılan bir kullanıcı arabirimi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="de979-108">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="de979-109">Yeniden bağlantı başarısız olursa, kullanıcıya yeniden deneme seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="de979-109">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="de979-110">Kullanıcı arabirimini özelleştirmek için *_Host. cshtml* Razor sayfasının `<body>` `components-reconnect-modal` `id` bir öğe tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="de979-110">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="de979-111">Aşağıdaki tabloda `components-reconnect-modal` öğesine uygulanan CSS sınıfları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="de979-111">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="de979-112">CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="de979-112">CSS class</span></span>                       | <span data-ttu-id="de979-113">&hellip; gösterir</span><span class="sxs-lookup"><span data-stu-id="de979-113">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="de979-114">Kayıp bir bağlantı.</span><span class="sxs-lookup"><span data-stu-id="de979-114">A lost connection.</span></span> <span data-ttu-id="de979-115">İstemci yeniden bağlanmaya çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="de979-115">The client is attempting to reconnect.</span></span> <span data-ttu-id="de979-116">Kalıcı olarak göster.</span><span class="sxs-lookup"><span data-stu-id="de979-116">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="de979-117">Etkin bir bağlantı sunucuya yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="de979-117">An active connection is re-established to the server.</span></span> <span data-ttu-id="de979-118">Kalıcı olarak gizleyin.</span><span class="sxs-lookup"><span data-stu-id="de979-118">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="de979-119">Muhtemelen bir ağ hatasından dolayı yeniden bağlantı başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="de979-119">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="de979-120">Yeniden bağlanmayı denemek için `window.Blazor.reconnect()`çağırın.</span><span class="sxs-lookup"><span data-stu-id="de979-120">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="de979-121">Yeniden bağlantı reddedildi.</span><span class="sxs-lookup"><span data-stu-id="de979-121">Reconnection rejected.</span></span> <span data-ttu-id="de979-122">Sunucuya ulaşıldı ancak bağlantı reddedildi ve kullanıcının sunucudaki durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="de979-122">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="de979-123">Uygulamayı yeniden yüklemek için `location.reload()`çağırın.</span><span class="sxs-lookup"><span data-stu-id="de979-123">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="de979-124">Bu bağlantı durumu şu durumlarda oluşabilir:</span><span class="sxs-lookup"><span data-stu-id="de979-124">This connection state may result when:</span></span><ul><li><span data-ttu-id="de979-125">Sunucu tarafında devre dışı bir kilitlenme oluşur.</span><span class="sxs-lookup"><span data-stu-id="de979-125">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="de979-126">Sunucunun kullanıcının durumunu bırakması için istemcinin bağlantısı yeterince uzun değil.</span><span class="sxs-lookup"><span data-stu-id="de979-126">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="de979-127">Kullanıcının etkileşimde bulunduğu bileşenlerin örnekleri atıldı.</span><span class="sxs-lookup"><span data-stu-id="de979-127">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="de979-128">Sunucu yeniden başlatıldı veya uygulamanın çalışan işlemi geri dönüştürüldü.</span><span class="sxs-lookup"><span data-stu-id="de979-128">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="de979-129">Oluşturma modu</span><span class="sxs-lookup"><span data-stu-id="de979-129">Render mode</span></span>

<span data-ttu-id="de979-130">Blazor sunucu uygulamaları, sunucu bağlantısı oluşturulmadan önce sunucudaki kullanıcı arabirimini varsayılan olarak PreRender 'a ayarlar.</span><span class="sxs-lookup"><span data-stu-id="de979-130">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="de979-131">Bu, *_Host. cshtml* Razor sayfasında ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="de979-131">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="de979-132">`RenderMode`, bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="de979-132">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="de979-133">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="de979-133">Is prerendered into the page.</span></span>
* <span data-ttu-id="de979-134">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="de979-134">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="de979-135">Açıklama</span><span class="sxs-lookup"><span data-stu-id="de979-135">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="de979-136">Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="de979-136">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="de979-137">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de979-137">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="de979-138">Blazor sunucusu uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="de979-138">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="de979-139">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="de979-139">Output from the component isn't included.</span></span> <span data-ttu-id="de979-140">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de979-140">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="de979-141">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="de979-141">Renders the component into static HTML.</span></span> |

<span data-ttu-id="de979-142">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="de979-142">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="de979-143">Razor sayfaları ve görünümlerinden durum bilgisi olan etkileşimli bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="de979-143">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="de979-144">Durum bilgisi olan etkileşimli bileşenler Razor sayfasına veya görünümüne eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="de979-144">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="de979-145">Sayfa veya görünüm şunları işler:</span><span class="sxs-lookup"><span data-stu-id="de979-145">When the page or view renders:</span></span>

* <span data-ttu-id="de979-146">Bileşen sayfa veya görünümle birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de979-146">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="de979-147">Prerendering için kullanılan ilk bileşen durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="de979-147">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="de979-148">SignalR bağlantısı oluşturulduğunda yeni bileşen durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="de979-148">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="de979-149">Aşağıdaki Razor sayfası bir `Counter` bileşeni işler:</span><span class="sxs-lookup"><span data-stu-id="de979-149">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="de979-150">Razor sayfaları ve görünümlerinden etkileşimsiz bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="de979-150">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="de979-151">Aşağıdaki Razor sayfasında, `Counter` bileşen bir form kullanılarak belirtilen bir başlangıç değeri ile statik olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="de979-151">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="de979-152">`MyComponent` statik olarak işlendiğinde, bileşen etkileşimli olamaz.</span><span class="sxs-lookup"><span data-stu-id="de979-152">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="de979-153">Blazor Server uygulamaları için SignalR istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="de979-153">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="de979-154">Bazen Blazor Server uygulamaları tarafından kullanılan SignalR istemcisini yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="de979-154">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="de979-155">Örneğin, bir bağlantı sorununu tanılamak için SignalR istemcisinde günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="de979-155">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="de979-156">*Pages/_Host. cshtml* dosyasında SignalR istemcisini yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="de979-156">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="de979-157">`blazor.server.js` betiği için `<script>` etiketine bir `autostart="false"` özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="de979-157">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="de979-158">`Blazor.start` çağırın ve SignalR oluşturucuyu belirten bir yapılandırma nesnesini geçirin.</span><span class="sxs-lookup"><span data-stu-id="de979-158">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```
