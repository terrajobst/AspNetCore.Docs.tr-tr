---
title: ASP.NET Core Razor bileşenlerini Razor Pages ve MVC uygulamalarıyla tümleştirin
author: guardrex
description: Blazor uygulamalarında bileşenler ve DOM öğeleri için veri bağlama senaryoları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: de1a37ffd9456c956e3d84fcc69431ecb794513c
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453298"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="64499-103">ASP.NET Core Razor bileşenlerini Razor Pages ve MVC uygulamalarıyla tümleştirin</span><span class="sxs-lookup"><span data-stu-id="64499-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="64499-104">, [Luke Latham](https://github.com/guardrex) ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="64499-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="64499-105">Razor bileşenleri, Razor Pages ve MVC uygulamalarıyla tümleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="64499-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="64499-106">Sayfa veya görünüm işlendiğinde, bileşenler aynı anda önceden alınabilir.</span><span class="sxs-lookup"><span data-stu-id="64499-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a><span data-ttu-id="64499-107">Uygulamayı sayfalar ve görünümlerde bileşenleri kullanmak üzere hazırlama</span><span class="sxs-lookup"><span data-stu-id="64499-107">Prepare the app to use components in pages and views</span></span>

<span data-ttu-id="64499-108">Mevcut bir Razor Pages veya MVC uygulaması, Razor bileşenlerini sayfalarla ve görünümleriyle tümleştirebilir:</span><span class="sxs-lookup"><span data-stu-id="64499-108">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="64499-109">Uygulamanın düzen dosyasında ( *_Layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="64499-109">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="64499-110">Aşağıdaki `<base>` etiketini `<head>` öğesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="64499-110">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="64499-111">Yukarıdaki örnekteki `href` değeri ( *uygulama temel yolu*), UYGULAMANıN kök URL yolunda (`/`) bulunduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="64499-111">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="64499-112">Uygulama bir alt uygulama ise, <xref:host-and-deploy/blazor/index#app-base-path> makalesinin *uygulama temel yolu* bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="64499-112">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="64499-113">*_Layout. cshtml* dosyası, bir MVC uygulamasında bir Razor Pages uygulamasının veya *görünümlerinin/paylaşılan* klasörünün *Sayfalar/paylaşılan* klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="64499-113">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="64499-114">Kapanış `</body>` etiketinden hemen önce *blazor. Server. js* betiği için bir `<script>` etiketi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="64499-114">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="64499-115">Framework, *blazor. Server. js* betiğini uygulamaya ekler.</span><span class="sxs-lookup"><span data-stu-id="64499-115">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="64499-116">Betiği uygulamaya el ile eklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="64499-116">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="64499-117">Aşağıdaki içerikle projenin kök klasörüne bir *_Imports. Razor* dosyası ekleyin (son ad alanını, `MyAppNamespace`, uygulamanın ad alanına değiştirin):</span><span class="sxs-lookup"><span data-stu-id="64499-117">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```razor
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="64499-118">`Startup.ConfigureServices`, Blazor sunucu hizmetini kaydedin:</span><span class="sxs-lookup"><span data-stu-id="64499-118">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="64499-119">`Startup.Configure`, Blazor hub uç noktasını `app.UseEndpoints`öğesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="64499-119">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="64499-120">Bileşenleri herhangi bir sayfa veya görünümle tümleştirin.</span><span class="sxs-lookup"><span data-stu-id="64499-120">Integrate components into any page or view.</span></span> <span data-ttu-id="64499-121">Daha fazla bilgi için, [bir sayfadan veya görünümden bileşenleri işleme](#render-components-from-a-page-or-view) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="64499-121">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="64499-122">Razor Pages uygulamasında yönlendirilebilir bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="64499-122">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="64499-123">*Bu bölüm, Kullanıcı isteklerinden doğrudan yönlendirilebilir bileşenleri eklemeye aittir.*</span><span class="sxs-lookup"><span data-stu-id="64499-123">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="64499-124">Razor Pages uygulamalarda yönlendirilebilir Razor bileşenlerini desteklemek için:</span><span class="sxs-lookup"><span data-stu-id="64499-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="64499-125">[Sayfaları sayfalar ve görünümler 'de kullanmak için uygulamayı hazırlama](#prepare-the-app-to-use-components-in-pages-and-views) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="64499-125">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="64499-126">Aşağıdaki içeriğe sahip proje köküne bir *app. Razor* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="64499-126">Add an *App.razor* file to the project root with the following content:</span></span>

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

1. <span data-ttu-id="64499-127">*Sayfalar* klasörüne aşağıdaki içeriğe sahip bir *_Host. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="64499-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="64499-128">Bileşenler, düzeni için paylaşılan *_Layout. cshtml* dosyasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="64499-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="64499-129">`Startup.Configure`' de uç nokta yapılandırmasına *_Host. cshtml* sayfası için düşük öncelikli bir yol ekleyin:</span><span class="sxs-lookup"><span data-stu-id="64499-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="64499-130">Uygulamaya yönlendirilebilir bileşenler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="64499-130">Add routable components to the app.</span></span> <span data-ttu-id="64499-131">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="64499-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="64499-132">Ad alanları hakkında daha fazla bilgi için [bileşen ad alanları](#component-namespaces) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="64499-132">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="64499-133">MVC uygulamasında yönlendirilebilir bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="64499-133">Use routable components in an MVC app</span></span>

<span data-ttu-id="64499-134">*Bu bölüm, Kullanıcı isteklerinden doğrudan yönlendirilebilir bileşenleri eklemeye aittir.*</span><span class="sxs-lookup"><span data-stu-id="64499-134">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="64499-135">MVC uygulamalarında yönlendirilebilir Razor bileşenlerini desteklemek için:</span><span class="sxs-lookup"><span data-stu-id="64499-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="64499-136">[Sayfaları sayfalar ve görünümler 'de kullanmak için uygulamayı hazırlama](#prepare-the-app-to-use-components-in-pages-and-views) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="64499-136">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="64499-137">Aşağıdaki içeriğe sahip projenin köküne bir *app. Razor* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="64499-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="64499-138">Aşağıdaki içeriğe sahip *Görünümler/giriş* klasörüne bir *_Host. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="64499-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="64499-139">Bileşenler, düzeni için paylaşılan *_Layout. cshtml* dosyasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="64499-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="64499-140">Ana denetleyiciye bir eylem ekleyin:</span><span class="sxs-lookup"><span data-stu-id="64499-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="64499-141">`Startup.Configure`' deki uç nokta yapılandırmasına *_Host. cshtml* görünümünü döndüren denetleyici eylemi için düşük öncelikli bir yol ekleyin:</span><span class="sxs-lookup"><span data-stu-id="64499-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="64499-142">Bir *Sayfalar* klasörü oluşturun ve uygulamaya yönlendirilebilir bileşenler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="64499-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="64499-143">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="64499-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="64499-144">Ad alanları hakkında daha fazla bilgi için [bileşen ad alanları](#component-namespaces) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="64499-144">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="64499-145">Bileşen ad alanları</span><span class="sxs-lookup"><span data-stu-id="64499-145">Component namespaces</span></span>

<span data-ttu-id="64499-146">Uygulamanın bileşenlerini tutmak için özel bir klasör kullanırken, klasörü/görünümü veya *_ViewImports. cshtml* dosyasını temsil eden ad alanını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="64499-146">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="64499-147">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="64499-147">In the following example:</span></span>

* <span data-ttu-id="64499-148">`MyAppNamespace` uygulamanın ad alanıyla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="64499-148">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="64499-149">*Bileşenler adlı bir* klasör bileşenleri tutmak için kullanılmazsa `Components`, bileşenlerin bulunduğu klasöre geçin.</span><span class="sxs-lookup"><span data-stu-id="64499-149">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="64499-150">*_ViewImports. cshtml* dosyası, bir Razor Pages uygulamasının *Sayfalar* klasöründe veya bir MVC uygulamasının *views* klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="64499-150">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="64499-151">Daha fazla bilgi için bkz. <xref:blazor/components#import-components>.</span><span class="sxs-lookup"><span data-stu-id="64499-151">For more information, see <xref:blazor/components#import-components>.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="64499-152">Bir sayfadan veya görünümden bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="64499-152">Render components from a page or view</span></span>

<span data-ttu-id="64499-153">*Bu bölüm, bileşenlerin Kullanıcı isteklerinden doğrudan yönlendirilemeyen sayfalara veya görünümlere bileşen eklenmesine aittir.*</span><span class="sxs-lookup"><span data-stu-id="64499-153">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="64499-154">Bir sayfadan veya görünümden bir bileşeni işlemek için `Component` etiketi yardımcısını kullanın:</span><span class="sxs-lookup"><span data-stu-id="64499-154">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="64499-155">Parametre türünün JSON serileştirilebilir olması gerekir, bu, genellikle türün bir varsayılan oluşturucuya ve ayarlanabilir özelliklere sahip olması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="64499-155">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="64499-156">Örneğin, `IncrementAmount` için bir değer belirtebilirsiniz çünkü `IncrementAmount` türü, JSON seri hale getirici tarafından desteklenen bir temel tür `int`.</span><span class="sxs-lookup"><span data-stu-id="64499-156">For example, you can specify a value for `IncrementAmount` because the type of `IncrementAmount` is an `int`, which is a primitive type supported by the JSON serializer.</span></span>

<span data-ttu-id="64499-157">`RenderMode`, bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="64499-157">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="64499-158">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="64499-158">Is prerendered into the page.</span></span>
* <span data-ttu-id="64499-159">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="64499-159">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="64499-160">Açıklama</span><span class="sxs-lookup"><span data-stu-id="64499-160">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="64499-161">Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="64499-161">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="64499-162">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="64499-162">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="64499-163">Blazor sunucusu uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="64499-163">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="64499-164">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="64499-164">Output from the component isn't included.</span></span> <span data-ttu-id="64499-165">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="64499-165">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="64499-166">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="64499-166">Renders the component into static HTML.</span></span> |

<span data-ttu-id="64499-167">Sayfalar ve görünümler bileşenleri kullanırken, listesiyse doğru değildir.</span><span class="sxs-lookup"><span data-stu-id="64499-167">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="64499-168">Bileşenler, kısmi görünümler ve bölümler gibi görüntüleme ve sayfaya özgü senaryolar kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="64499-168">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="64499-169">Bir bileşende kısmi görünümden mantığı kullanmak için kısmi görünüm mantığını bir bileşene ayırın.</span><span class="sxs-lookup"><span data-stu-id="64499-169">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="64499-170">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="64499-170">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="64499-171">Bileşenlerin nasıl işlendiği, bileşen durumu ve `Component` etiketi Yardımcısı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="64499-171">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
