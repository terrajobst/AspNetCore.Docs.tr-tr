---
title: ASP.NET Core Blazor barındırma modeli yapılandırması
author: guardrex
description: Razor bileşenlerini Razor Pages ve MVC uygulamalarına tümleştirme dahil olmak üzere Blazor barındırma modeli yapılandırması hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 91dd1aa32bb5165845eb4794377a50ccbd01adc3
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215062"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="555cf-103">ASP.NET Core Blazor barındırma modeli yapılandırması</span><span class="sxs-lookup"><span data-stu-id="555cf-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="555cf-104">[Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="555cf-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="555cf-105">Bu makalede barındırma modeli yapılandırması ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="555cf-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="555cf-106">Blazor Server</span><span class="sxs-lookup"><span data-stu-id="555cf-106">Blazor Server</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="555cf-107">Razor bileşenlerini Razor Pages ve MVC uygulamalarıyla tümleştirin</span><span class="sxs-lookup"><span data-stu-id="555cf-107">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="555cf-108">Sayfalar ve görünümlerde bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="555cf-108">Use components in pages and views</span></span>

<span data-ttu-id="555cf-109">Mevcut bir Razor Pages veya MVC uygulaması, Razor bileşenlerini sayfalarla ve görünümleriyle tümleştirebilir:</span><span class="sxs-lookup"><span data-stu-id="555cf-109">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="555cf-110">Uygulamanın düzen dosyasında ( *_Layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="555cf-110">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="555cf-111">Aşağıdaki `<base>` etiketini `<head>` öğesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="555cf-111">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="555cf-112">Yukarıdaki örnekteki `href` değeri ( *uygulama temel yolu*), UYGULAMANıN kök URL yolunda (`/`) bulunduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="555cf-112">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="555cf-113">Uygulama bir alt uygulama ise, <xref:host-and-deploy/blazor/index#app-base-path> makalesinin *uygulama temel yolu* bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="555cf-113">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="555cf-114">*_Layout. cshtml* dosyası, bir MVC uygulamasında bir Razor Pages uygulamasının veya *görünümlerinin/paylaşılan* klasörünün *Sayfalar/paylaşılan* klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="555cf-114">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="555cf-115">Kapanış `</body>` etiketinden hemen önce *blazor. Server. js* betiği için bir `<script>` etiketi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="555cf-115">Add a `<script>` tag for the *blazor.server.js* script right before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="555cf-116">Framework, *blazor. Server. js* betiğini uygulamaya ekler.</span><span class="sxs-lookup"><span data-stu-id="555cf-116">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="555cf-117">Betiği uygulamaya el ile eklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="555cf-117">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="555cf-118">Aşağıdaki içerikle projenin kök klasörüne bir *_Imports. Razor* dosyası ekleyin (son ad alanını, `MyAppNamespace`, uygulamanın ad alanına değiştirin):</span><span class="sxs-lookup"><span data-stu-id="555cf-118">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="555cf-119">`Startup.ConfigureServices`, Blazor sunucu hizmetini kaydedin:</span><span class="sxs-lookup"><span data-stu-id="555cf-119">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="555cf-120">`Startup.Configure`, Blazor hub uç noktasını `app.UseEndpoints`öğesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="555cf-120">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="555cf-121">Bileşenleri herhangi bir sayfa veya görünümle tümleştirin.</span><span class="sxs-lookup"><span data-stu-id="555cf-121">Integrate components into any page or view.</span></span> <span data-ttu-id="555cf-122">Daha fazla bilgi için, <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> makalesinin *bileşenleri Razor Pages ve MVC uygulamalarına tümleştirme* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="555cf-122">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="555cf-123">Razor Pages uygulamasında yönlendirilebilir bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="555cf-123">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="555cf-124">Razor Pages uygulamalarda yönlendirilebilir Razor bileşenlerini desteklemek için:</span><span class="sxs-lookup"><span data-stu-id="555cf-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="555cf-125">[Sayfalar ve görünümlerde bileşenleri kullanma](#use-components-in-pages-and-views) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="555cf-125">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="555cf-126">Aşağıdaki içeriğe sahip projenin köküne bir *app. Razor* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="555cf-126">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="555cf-127">*Sayfalar* klasörüne aşağıdaki içeriğe sahip bir *_Host. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="555cf-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="555cf-128">Bileşenler, düzeni için paylaşılan *_Layout. cshtml* dosyasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="555cf-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="555cf-129">`Startup.Configure`' de uç nokta yapılandırmasına *_Host. cshtml* sayfası için düşük öncelikli bir yol ekleyin:</span><span class="sxs-lookup"><span data-stu-id="555cf-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="555cf-130">Uygulamaya yönlendirilebilir bileşenler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="555cf-130">Add routable components to the app.</span></span> <span data-ttu-id="555cf-131">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="555cf-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="555cf-132">Uygulamanın bileşenlerini tutmak için özel bir klasör kullanırken, klasörü *sayfaları/_ViewImports. cshtml* dosyasına temsil eden ad alanını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="555cf-132">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="555cf-133">Daha fazla bilgi için bkz. <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="555cf-133">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="555cf-134">MVC uygulamasında yönlendirilebilir bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="555cf-134">Use routable components in an MVC app</span></span>

<span data-ttu-id="555cf-135">MVC uygulamalarında yönlendirilebilir Razor bileşenlerini desteklemek için:</span><span class="sxs-lookup"><span data-stu-id="555cf-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="555cf-136">[Sayfalar ve görünümlerde bileşenleri kullanma](#use-components-in-pages-and-views) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="555cf-136">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="555cf-137">Aşağıdaki içeriğe sahip projenin köküne bir *app. Razor* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="555cf-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="555cf-138">Aşağıdaki içeriğe sahip *Görünümler/giriş* klasörüne bir *_Host. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="555cf-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="555cf-139">Bileşenler, düzeni için paylaşılan *_Layout. cshtml* dosyasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="555cf-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="555cf-140">Ana denetleyiciye bir eylem ekleyin:</span><span class="sxs-lookup"><span data-stu-id="555cf-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="555cf-141">`Startup.Configure`' deki uç nokta yapılandırmasına *_Host. cshtml* görünümünü döndüren denetleyici eylemi için düşük öncelikli bir yol ekleyin:</span><span class="sxs-lookup"><span data-stu-id="555cf-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="555cf-142">Bir *Sayfalar* klasörü oluşturun ve uygulamaya yönlendirilebilir bileşenler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="555cf-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="555cf-143">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="555cf-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="555cf-144">Uygulamanın bileşenlerini tutmak için özel bir klasör kullanırken, klasörü *görüntüleme/_ViewImports. cshtml* dosyasına temsil eden ad alanını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="555cf-144">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="555cf-145">Daha fazla bilgi için bkz. <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="555cf-145">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="555cf-146">Kullanıcı arabirimindeki bağlantı durumunu yansıtır</span><span class="sxs-lookup"><span data-stu-id="555cf-146">Reflect the connection state in the UI</span></span>

<span data-ttu-id="555cf-147">İstemci bağlantının kaybolduğunu algıladığında, istemci yeniden bağlanmayı denediğinde kullanıcıya varsayılan bir kullanıcı arabirimi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="555cf-147">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="555cf-148">Yeniden bağlantı başarısız olursa, kullanıcıya yeniden deneme seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="555cf-148">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="555cf-149">Kullanıcı arabirimini özelleştirmek için *_Host. cshtml* Razor sayfasının `<body>` `components-reconnect-modal` `id` bir öğe tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="555cf-149">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="555cf-150">Aşağıdaki tabloda `components-reconnect-modal` öğesine uygulanan CSS sınıfları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="555cf-150">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="555cf-151">CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="555cf-151">CSS class</span></span>                       | <span data-ttu-id="555cf-152">&hellip; gösterir</span><span class="sxs-lookup"><span data-stu-id="555cf-152">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="555cf-153">Kayıp bir bağlantı.</span><span class="sxs-lookup"><span data-stu-id="555cf-153">A lost connection.</span></span> <span data-ttu-id="555cf-154">İstemci yeniden bağlanmaya çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="555cf-154">The client is attempting to reconnect.</span></span> <span data-ttu-id="555cf-155">Kalıcı olarak göster.</span><span class="sxs-lookup"><span data-stu-id="555cf-155">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="555cf-156">Etkin bir bağlantı sunucuya yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="555cf-156">An active connection is re-established to the server.</span></span> <span data-ttu-id="555cf-157">Kalıcı olarak gizleyin.</span><span class="sxs-lookup"><span data-stu-id="555cf-157">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="555cf-158">Muhtemelen bir ağ hatasından dolayı yeniden bağlantı başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="555cf-158">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="555cf-159">Yeniden bağlanmayı denemek için `window.Blazor.reconnect()`çağırın.</span><span class="sxs-lookup"><span data-stu-id="555cf-159">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="555cf-160">Yeniden bağlantı reddedildi.</span><span class="sxs-lookup"><span data-stu-id="555cf-160">Reconnection rejected.</span></span> <span data-ttu-id="555cf-161">Sunucuya ulaşıldı ancak bağlantı reddedildi ve kullanıcının sunucudaki durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="555cf-161">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="555cf-162">Uygulamayı yeniden yüklemek için `location.reload()`çağırın.</span><span class="sxs-lookup"><span data-stu-id="555cf-162">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="555cf-163">Bu bağlantı durumu şu durumlarda oluşabilir:</span><span class="sxs-lookup"><span data-stu-id="555cf-163">This connection state may result when:</span></span><ul><li><span data-ttu-id="555cf-164">Sunucu tarafında devre dışı bir kilitlenme oluşur.</span><span class="sxs-lookup"><span data-stu-id="555cf-164">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="555cf-165">Sunucunun kullanıcının durumunu bırakması için istemcinin bağlantısı yeterince uzun değil.</span><span class="sxs-lookup"><span data-stu-id="555cf-165">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="555cf-166">Kullanıcının etkileşimde bulunduğu bileşenlerin örnekleri atıldı.</span><span class="sxs-lookup"><span data-stu-id="555cf-166">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="555cf-167">Sunucu yeniden başlatıldı veya uygulamanın çalışan işlemi geri dönüştürüldü.</span><span class="sxs-lookup"><span data-stu-id="555cf-167">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="555cf-168">Oluşturma modu</span><span class="sxs-lookup"><span data-stu-id="555cf-168">Render mode</span></span>

<span data-ttu-id="555cf-169">Blazor sunucu uygulamaları, sunucu bağlantısı oluşturulmadan önce sunucudaki kullanıcı arabirimini varsayılan olarak PreRender 'a ayarlar.</span><span class="sxs-lookup"><span data-stu-id="555cf-169">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="555cf-170">Bu, *_Host. cshtml* Razor sayfasında ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="555cf-170">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="555cf-171">`RenderMode`, bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="555cf-171">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="555cf-172">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="555cf-172">Is prerendered into the page.</span></span>
* <span data-ttu-id="555cf-173">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="555cf-173">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="555cf-174">Açıklama</span><span class="sxs-lookup"><span data-stu-id="555cf-174">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="555cf-175">Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="555cf-175">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="555cf-176">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="555cf-176">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="555cf-177">Blazor sunucusu uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="555cf-177">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="555cf-178">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="555cf-178">Output from the component isn't included.</span></span> <span data-ttu-id="555cf-179">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="555cf-179">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="555cf-180">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="555cf-180">Renders the component into static HTML.</span></span> |

<span data-ttu-id="555cf-181">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="555cf-181">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="555cf-182">Razor sayfaları ve görünümlerinden durum bilgisi olan etkileşimli bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="555cf-182">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="555cf-183">Durum bilgisi olan etkileşimli bileşenler Razor sayfasına veya görünümüne eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="555cf-183">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="555cf-184">Sayfa veya görünüm şunları işler:</span><span class="sxs-lookup"><span data-stu-id="555cf-184">When the page or view renders:</span></span>

* <span data-ttu-id="555cf-185">Bileşen sayfa veya görünümle birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="555cf-185">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="555cf-186">Prerendering için kullanılan ilk bileşen durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="555cf-186">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="555cf-187">SignalR bağlantısı oluşturulduğunda yeni bileşen durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="555cf-187">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="555cf-188">Aşağıdaki Razor sayfası bir `Counter` bileşeni işler:</span><span class="sxs-lookup"><span data-stu-id="555cf-188">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="555cf-189">Razor sayfaları ve görünümlerinden etkileşimsiz bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="555cf-189">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="555cf-190">Aşağıdaki Razor sayfasında, `Counter` bileşen bir form kullanılarak belirtilen bir başlangıç değeri ile statik olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="555cf-190">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="555cf-191">`MyComponent` statik olarak işlendiğinde, bileşen etkileşimli olamaz.</span><span class="sxs-lookup"><span data-stu-id="555cf-191">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="555cf-192">Blazor Server uygulamaları için SignalR istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="555cf-192">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="555cf-193">Bazen Blazor Server uygulamaları tarafından kullanılan SignalR istemcisini yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="555cf-193">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="555cf-194">Örneğin, bir bağlantı sorununu tanılamak için SignalR istemcisinde günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="555cf-194">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="555cf-195">*Pages/_Host. cshtml* dosyasında SignalR istemcisini yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="555cf-195">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="555cf-196">`blazor.server.js` betiği için `<script>` etiketine bir `autostart="false"` özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="555cf-196">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="555cf-197">`Blazor.start` çağırın ve SignalR oluşturucuyu belirten bir yapılandırma nesnesini geçirin.</span><span class="sxs-lookup"><span data-stu-id="555cf-197">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
