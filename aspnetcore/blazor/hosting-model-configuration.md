---
title: ASP.NET Core Blazor barındırma modeli yapılandırması
author: guardrex
description: Razor bileşenlerini Razor Pages ve MVC uygulamalarına tümleştirme dahil olmak üzere Blazor barındırma modeli yapılandırması hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 1f71ac63bbe9dc9d56cfca2ded19a5b863be828f
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306427"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="ed563-103">ASP.NET Core Blazor barındırma modeli yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ed563-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="ed563-104">[Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="ed563-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="ed563-105">Bu makalede barındırma modeli yapılandırması ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ed563-105">This article covers hosting model configuration.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="ed563-106">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="ed563-106">Blazor WebAssembly</span></span>

<span data-ttu-id="ed563-107">ASP.NET Core 3,2 Preview 3 sürümünden itibaren, Blazor WebAssembly yapılandırmayı şunları destekler:</span><span class="sxs-lookup"><span data-stu-id="ed563-107">As of the ASP.NET Core 3.2 Preview 3 release, Blazor WebAssembly supports configuration from:</span></span>

* <span data-ttu-id="ed563-108">*Wwwroot/appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="ed563-108">*wwwroot/appsettings.json*</span></span>
* <span data-ttu-id="ed563-109">*Wwwroot/appSettings. {ENVIRONMENT}. JSON*</span><span class="sxs-lookup"><span data-stu-id="ed563-109">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>

<span data-ttu-id="ed563-110">Blazor barındırılan bir uygulamada, [çalışma zamanı ortamı](xref:fundamentals/environments) , sunucu uygulamasının değeri ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="ed563-110">In a Blazor Hosted app, the [runtime environment](xref:fundamentals/environments) is the same as the server app's value.</span></span>

<span data-ttu-id="ed563-111">Uygulamayı yerel olarak çalıştırırken, ortam varsayılan olarak geliştirme aşamasındadır.</span><span class="sxs-lookup"><span data-stu-id="ed563-111">When running the app locally, the environment defaults to Development.</span></span> <span data-ttu-id="ed563-112">Uygulama yayımlandığında, ortam varsayılan olarak üretim olur.</span><span class="sxs-lookup"><span data-stu-id="ed563-112">When the app is published, the environment defaults to Production.</span></span> <span data-ttu-id="ed563-113">Ortamın nasıl yapılandırılacağı dahil olmak üzere daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="ed563-113">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

> [!WARNING]
> <span data-ttu-id="ed563-114">Blazor WebAssembly uygulamasındaki yapılandırma kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="ed563-114">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="ed563-115">**Yapılandırma bölümünde uygulama gizli dizilerini veya kimlik bilgilerini depolamamayın.**</span><span class="sxs-lookup"><span data-stu-id="ed563-115">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="ed563-116">Yapılandırma dosyaları çevrimdışı kullanım için önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="ed563-116">Configuration files are cached for offline use.</span></span> <span data-ttu-id="ed563-117">[Aşamalı Web uygulamaları (PWAs)](xref:blazor/progressive-web-app)ile, yalnızca yeni bir dağıtım oluştururken yapılandırma dosyalarını güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed563-117">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="ed563-118">Yapılandırma dosyalarının dağıtımlar arasında düzenlenmesinin hiçbir etkisi yoktur çünkü:</span><span class="sxs-lookup"><span data-stu-id="ed563-118">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="ed563-119">Kullanıcıların, kullanmaya devam ettikleri dosyaların önbelleğe alınmış sürümleri vardır.</span><span class="sxs-lookup"><span data-stu-id="ed563-119">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="ed563-120">PWA 'nın *Service-Worker. js* ve *Service-Worker-assets. js* dosyalarının derlemede yeniden oluşturulması gerekir. Bu, kullanıcının bir sonraki çevrimiçi sitesinde uygulamaya işaret eden uygulamanın yeniden dağıtıldığını belirten, derleme üzerinde yeniden oluşturulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ed563-120">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="ed563-121">Arka plan güncelleştirmelerinin PWAs tarafından nasıl işlendiği hakkında daha fazla bilgi için bkz. <xref:blazor/progressive-web-app#background-updates>.</span><span class="sxs-lookup"><span data-stu-id="ed563-121">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="ed563-122">Blazor Server</span><span class="sxs-lookup"><span data-stu-id="ed563-122">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="ed563-123">Kullanıcı arabirimindeki bağlantı durumunu yansıtır</span><span class="sxs-lookup"><span data-stu-id="ed563-123">Reflect the connection state in the UI</span></span>

<span data-ttu-id="ed563-124">İstemci bağlantının kaybolduğunu algıladığında, istemci yeniden bağlanmayı denediğinde kullanıcıya varsayılan bir kullanıcı arabirimi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ed563-124">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="ed563-125">Yeniden bağlantı başarısız olursa, kullanıcıya yeniden deneme seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ed563-125">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="ed563-126">Kullanıcı arabirimini özelleştirmek için *_Host. cshtml* Razor sayfasının `<body>` `components-reconnect-modal` `id` bir öğe tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="ed563-126">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="ed563-127">Aşağıdaki tabloda `components-reconnect-modal` öğesine uygulanan CSS sınıfları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ed563-127">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="ed563-128">CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="ed563-128">CSS class</span></span>                       | <span data-ttu-id="ed563-129">&hellip; gösterir</span><span class="sxs-lookup"><span data-stu-id="ed563-129">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="ed563-130">Kayıp bir bağlantı.</span><span class="sxs-lookup"><span data-stu-id="ed563-130">A lost connection.</span></span> <span data-ttu-id="ed563-131">İstemci yeniden bağlanmaya çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="ed563-131">The client is attempting to reconnect.</span></span> <span data-ttu-id="ed563-132">Kalıcı olarak göster.</span><span class="sxs-lookup"><span data-stu-id="ed563-132">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="ed563-133">Etkin bir bağlantı sunucuya yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ed563-133">An active connection is re-established to the server.</span></span> <span data-ttu-id="ed563-134">Kalıcı olarak gizleyin.</span><span class="sxs-lookup"><span data-stu-id="ed563-134">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="ed563-135">Muhtemelen bir ağ hatasından dolayı yeniden bağlantı başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="ed563-135">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="ed563-136">Yeniden bağlanmayı denemek için `window.Blazor.reconnect()`çağırın.</span><span class="sxs-lookup"><span data-stu-id="ed563-136">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="ed563-137">Yeniden bağlantı reddedildi.</span><span class="sxs-lookup"><span data-stu-id="ed563-137">Reconnection rejected.</span></span> <span data-ttu-id="ed563-138">Sunucuya ulaşıldı ancak bağlantı reddedildi ve kullanıcının sunucudaki durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="ed563-138">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="ed563-139">Uygulamayı yeniden yüklemek için `location.reload()`çağırın.</span><span class="sxs-lookup"><span data-stu-id="ed563-139">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="ed563-140">Bu bağlantı durumu şu durumlarda oluşabilir:</span><span class="sxs-lookup"><span data-stu-id="ed563-140">This connection state may result when:</span></span><ul><li><span data-ttu-id="ed563-141">Sunucu tarafında devre dışı bir kilitlenme oluşur.</span><span class="sxs-lookup"><span data-stu-id="ed563-141">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="ed563-142">Sunucunun kullanıcının durumunu bırakması için istemcinin bağlantısı yeterince uzun değil.</span><span class="sxs-lookup"><span data-stu-id="ed563-142">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="ed563-143">Kullanıcının etkileşimde bulunduğu bileşenlerin örnekleri atıldı.</span><span class="sxs-lookup"><span data-stu-id="ed563-143">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="ed563-144">Sunucu yeniden başlatıldı veya uygulamanın çalışan işlemi geri dönüştürüldü.</span><span class="sxs-lookup"><span data-stu-id="ed563-144">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="ed563-145">Oluşturma modu</span><span class="sxs-lookup"><span data-stu-id="ed563-145">Render mode</span></span>

<span data-ttu-id="ed563-146">Blazor sunucu uygulamaları, sunucu bağlantısı oluşturulmadan önce sunucudaki kullanıcı arabirimini varsayılan olarak PreRender 'a ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ed563-146">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="ed563-147">Bu, *_Host. cshtml* Razor sayfasında ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="ed563-147">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="ed563-148">`RenderMode`, bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="ed563-148">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="ed563-149">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ed563-149">Is prerendered into the page.</span></span>
* <span data-ttu-id="ed563-150">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="ed563-150">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="ed563-151">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ed563-151">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="ed563-152">Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="ed563-152">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="ed563-153">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ed563-153">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="ed563-154">Blazor sunucusu uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ed563-154">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="ed563-155">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="ed563-155">Output from the component isn't included.</span></span> <span data-ttu-id="ed563-156">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ed563-156">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="ed563-157">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="ed563-157">Renders the component into static HTML.</span></span> |

<span data-ttu-id="ed563-158">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="ed563-158">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="ed563-159">Razor sayfaları ve görünümlerinden durum bilgisi olan etkileşimli bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="ed563-159">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="ed563-160">Durum bilgisi olan etkileşimli bileşenler Razor sayfasına veya görünümüne eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="ed563-160">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="ed563-161">Sayfa veya görünüm şunları işler:</span><span class="sxs-lookup"><span data-stu-id="ed563-161">When the page or view renders:</span></span>

* <span data-ttu-id="ed563-162">Bileşen sayfa veya görünümle birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ed563-162">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="ed563-163">Prerendering için kullanılan ilk bileşen durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="ed563-163">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="ed563-164">SignalR bağlantısı oluşturulduğunda yeni bileşen durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ed563-164">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="ed563-165">Aşağıdaki Razor sayfası bir `Counter` bileşeni işler:</span><span class="sxs-lookup"><span data-stu-id="ed563-165">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="ed563-166">Razor sayfaları ve görünümlerinden etkileşimsiz bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="ed563-166">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="ed563-167">Aşağıdaki Razor sayfasında, `Counter` bileşen bir form kullanılarak belirtilen bir başlangıç değeri ile statik olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="ed563-167">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="ed563-168">`MyComponent` statik olarak işlendiğinde, bileşen etkileşimli olamaz.</span><span class="sxs-lookup"><span data-stu-id="ed563-168">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="ed563-169">Blazor Server uygulamaları için SignalR istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ed563-169">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="ed563-170">Bazen Blazor Server uygulamaları tarafından kullanılan SignalR istemcisini yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ed563-170">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="ed563-171">Örneğin, bir bağlantı sorununu tanılamak için SignalR istemcisinde günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed563-171">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="ed563-172">*Pages/_Host. cshtml* dosyasında SignalR istemcisini yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="ed563-172">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="ed563-173">`blazor.server.js` betiği için `<script>` etiketine bir `autostart="false"` özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ed563-173">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="ed563-174">`Blazor.start` çağırın ve SignalR oluşturucuyu belirten bir yapılandırma nesnesini geçirin.</span><span class="sxs-lookup"><span data-stu-id="ed563-174">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
