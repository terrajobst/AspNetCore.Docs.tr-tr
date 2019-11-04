---
title: ASP.NET Core 3,0 ' deki yenilikler
author: rick-anderson
description: ASP.NET Core 3,0 ' deki yeni özellikler hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: aspnetcore-3.0
ms.openlocfilehash: 8c53d8a9fa222ca40f26dc713ec3b70ddde76539
ms.sourcegitcommit: eb2fe5ad2e82fab86ca952463af8d017ba659b25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73416117"
---
# <a name="whats-new-in-aspnet-core-30"></a><span data-ttu-id="1d2ab-103">ASP.NET Core 3,0 ' deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="1d2ab-103">What's new in ASP.NET Core 3.0</span></span>

<span data-ttu-id="1d2ab-104">Bu makalede, ASP.NET Core 3,0 ' deki en önemli değişiklikler ilgili belgelerin bağlantılarıyla vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-104">This article highlights the most significant changes in ASP.NET Core 3.0 with links to relevant documentation.</span></span>

## <a name="blazor"></a><span data-ttu-id="1d2ab-105">Blazor</span><span class="sxs-lookup"><span data-stu-id="1d2ab-105">Blazor</span></span>

<span data-ttu-id="1d2ab-106">Blazor, .NET ile etkileşimli istemci tarafı Web Kullanıcı arabirimi oluşturmak için ASP.NET Core yeni bir çerçevedir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-106">Blazor is a new framework in ASP.NET Core for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="1d2ab-107">JavaScript yerine zengin etkileşimli Uıusing C# oluşturma.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="1d2ab-108">.NET ' te yazılmış sunucu tarafı ve istemci tarafı uygulama mantığını paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="1d2ab-109">Mobil tarayıcılar dahil olmak üzere geniş tarayıcı desteği için Kullanıcı arabirimini HTML ve CSS olarak işleme.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="1d2ab-110">Blazor Framework desteklenen senaryolar:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-110">Blazor framework supported scenarios:</span></span>

* <span data-ttu-id="1d2ab-111">Yeniden kullanılabilir kullanıcı arabirimi bileşenleri (Razor bileşenleri)</span><span class="sxs-lookup"><span data-stu-id="1d2ab-111">Reusable UI components (Razor components)</span></span>
* <span data-ttu-id="1d2ab-112">İstemci tarafı yönlendirme</span><span class="sxs-lookup"><span data-stu-id="1d2ab-112">Client-side routing</span></span>
* <span data-ttu-id="1d2ab-113">Bileşen düzenleri</span><span class="sxs-lookup"><span data-stu-id="1d2ab-113">Component layouts</span></span>
* <span data-ttu-id="1d2ab-114">Bağımlılık ekleme desteği</span><span class="sxs-lookup"><span data-stu-id="1d2ab-114">Support for dependency injection</span></span>
* <span data-ttu-id="1d2ab-115">Formlar ve doğrulama</span><span class="sxs-lookup"><span data-stu-id="1d2ab-115">Forms and validation</span></span>
* <span data-ttu-id="1d2ab-116">Razor sınıf kitaplıkları ile bileşen kitaplıkları derleme</span><span class="sxs-lookup"><span data-stu-id="1d2ab-116">Build component libraries with Razor class libraries</span></span>
* <span data-ttu-id="1d2ab-117">JavaScript ile birlikte çalışma</span><span class="sxs-lookup"><span data-stu-id="1d2ab-117">JavaScript interop</span></span>

<span data-ttu-id="1d2ab-118">Daha fazla bilgi için bkz. <xref:blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-118">For more information, see <xref:blazor/index>.</span></span>

### <a name="blazor-server"></a><span data-ttu-id="1d2ab-119">Blazor Server</span><span class="sxs-lookup"><span data-stu-id="1d2ab-119">Blazor Server</span></span>

<span data-ttu-id="1d2ab-120">Blazor, Kullanıcı arabirimi güncelleştirmelerinin uygulanma, bileşen işleme mantığını ayırır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-120">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="1d2ab-121">Blazor Server, Razor bileşenlerini bir ASP.NET Core uygulamasındaki sunucuda barındırmak için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-121">Blazor Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="1d2ab-122">Kullanıcı Arabirimi güncelleştirmeleri bir SignalR bağlantısı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-122">UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="1d2ab-123">Blazor sunucusu ASP.NET Core 3,0 ' de desteklenir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-123">Blazor Server is supported in ASP.NET Core 3.0.</span></span>

### <a name="blazor-webassembly-preview"></a><span data-ttu-id="1d2ab-124">Blazor WebAssembly (Önizleme)</span><span class="sxs-lookup"><span data-stu-id="1d2ab-124">Blazor WebAssembly (Preview)</span></span>

<span data-ttu-id="1d2ab-125">Blazor uygulamalar, bir WebAssembly tabanlı .NET çalışma zamanı kullanarak doğrudan tarayıcıda da çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-125">Blazor apps can also be run directly in the browser using a WebAssembly-based .NET runtime.</span></span> <span data-ttu-id="1d2ab-126">Blazor WebAssembly Önizleme *aşamasındadır ve ASP.NET Core 3,0 ' de* desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-126">Blazor WebAssembly is in preview and *not* supported in ASP.NET Core 3.0.</span></span> <span data-ttu-id="1d2ab-127">Blazor WebAssembly ASP.NET Core gelecek bir sürümünde desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-127">Blazor WebAssembly will be supported in a future release of ASP.NET Core.</span></span>

### <a name="razor-components"></a><span data-ttu-id="1d2ab-128">Razor bileşenleri</span><span class="sxs-lookup"><span data-stu-id="1d2ab-128">Razor components</span></span>

<span data-ttu-id="1d2ab-129">Blazor uygulamaları bileşenlerinden oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-129">Blazor apps are built from components.</span></span> <span data-ttu-id="1d2ab-130">Bileşenler, bir sayfa, iletişim kutusu veya form gibi kullanıcı arabirimi (UI) için kendi içinde yer alan öbeklerdir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-130">Components are self-contained chunks of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="1d2ab-131">Bileşenler, Kullanıcı arabirimi işleme mantığını ve istemci tarafı olay işleyicilerini tanımlayan normal .NET sınıflarıdır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-131">Components are normal .NET classes that define UI rendering logic and client-side event handlers.</span></span> <span data-ttu-id="1d2ab-132">JavaScript olmadan zengin etkileşimli Web uygulamaları oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-132">You can create rich interactive web apps without JavaScript.</span></span>

<span data-ttu-id="1d2ab-133">Blazor içindeki bileşenler genellikle HTML ve C#doğal bir Blend Razor söz dizimi kullanılarak yazılır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-133">Components in Blazor are typically authored using Razor syntax, a natural blend of HTML and C#.</span></span> <span data-ttu-id="1d2ab-134">Razor bileşenleri, her ikisi de Razor kullanan Razor Pages ve MVC görünümlerine benzerdir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-134">Razor components are similar to Razor Pages and MVC views in that they both use Razor.</span></span> <span data-ttu-id="1d2ab-135">Bir istek-yanıt modelini temel alan sayfaların ve görünümlerin aksine, bileşenler Kullanıcı arabirimi oluşturmayı işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-135">Unlike pages and views, which are based on a request-response model, components are used specifically for handling UI composition.</span></span>

## <a name="grpc"></a><span data-ttu-id="1d2ab-136">gRPC</span><span class="sxs-lookup"><span data-stu-id="1d2ab-136">gRPC</span></span>

<span data-ttu-id="1d2ab-137">[GRPC](https://grpc.io/):</span><span class="sxs-lookup"><span data-stu-id="1d2ab-137">[gRPC](https://grpc.io/):</span></span>

* <span data-ttu-id="1d2ab-138">Popüler, yüksek performanslı bir RPC (uzak yordam çağrısı) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-138">Is a popular, high-performance RPC (remote procedure call) framework.</span></span>
* <span data-ttu-id="1d2ab-139">, API geliştirmeye yönelik olarak yapılan bir sözleşmenin ilk yaklaşımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-139">Offers an opinionated contract-first approach to API development.</span></span>
* <span data-ttu-id="1d2ab-140">, Gibi modern teknolojiler kullanır:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-140">Uses modern technologies such as:</span></span>

  * <span data-ttu-id="1d2ab-141">Taşıma için HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-141">HTTP/2 for transport.</span></span>
  * <span data-ttu-id="1d2ab-142">Arabirim açıklaması dili olarak protokol arabellekleri.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-142">Protocol Buffers as the interface description language.</span></span>
  * <span data-ttu-id="1d2ab-143">İkili serileştirme biçimi.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-143">Binary serialization format.</span></span>
* <span data-ttu-id="1d2ab-144">Şöyle özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-144">Provides features such as:</span></span>

  * <span data-ttu-id="1d2ab-145">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="1d2ab-145">Authentication</span></span>
  * <span data-ttu-id="1d2ab-146">Çift yönlü akış ve akış denetimi.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-146">Bidirectional streaming and flow control.</span></span>
  * <span data-ttu-id="1d2ab-147">İptal ve zaman aşımları.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-147">Cancellation and timeouts.</span></span>

<span data-ttu-id="1d2ab-148">ASP.NET Core 3,0 ' deki gRPC işlevselliği şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-148">gRPC functionality in ASP.NET Core 3.0 includes:</span></span>

* <span data-ttu-id="1d2ab-149">[GRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) , GRPC hizmetlerini barındırmak için bir ASP.NET Core çerçevesini &ndash;.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-149">[Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; An ASP.NET Core framework for hosting gRPC services.</span></span> <span data-ttu-id="1d2ab-150">gRPC on ASP.NET Core, günlüğe kaydetme, bağımlılık ekleme (dı), kimlik doğrulama ve yetkilendirme gibi standart ASP.NET Core özelliklerle tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-150">gRPC on ASP.NET Core integrates with standard ASP.NET Core features like logging, dependency injection (DI), authentication, and authorization.</span></span>
* <span data-ttu-id="1d2ab-151">[GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; .NET Core Için bir GRPC istemcisini tanıdık `HttpClient`üzerinde oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-151">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; A gRPC client for .NET Core that builds upon the familiar `HttpClient`.</span></span>
* <span data-ttu-id="1d2ab-152">[GRPC .net. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; GRPC istemci tümleştirmesi `HttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-152">[Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; gRPC client integration with `HttpClientFactory`.</span></span>

<span data-ttu-id="1d2ab-153">Daha fazla bilgi için bkz. <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-153">For more information, see <xref:grpc/index>.</span></span>

## <a name="signalr"></a><span data-ttu-id="1d2ab-154">SignalR</span><span class="sxs-lookup"><span data-stu-id="1d2ab-154">SignalR</span></span>

<span data-ttu-id="1d2ab-155">Bkz. geçiş yönergeleri için [SignalR kodunu güncelleştirme](xref:migration/22-to-30#signalr) .</span><span class="sxs-lookup"><span data-stu-id="1d2ab-155">See [Update SignalR code](xref:migration/22-to-30#signalr) for migration instructions.</span></span> <span data-ttu-id="1d2ab-156">SignalR artık JSON iletilerini serileştirmek/seri durumdan çıkarmak için `System.Text.Json` kullanır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-156">SignalR now uses `System.Text.Json` to serialize/deserialize JSON messages.</span></span> <span data-ttu-id="1d2ab-157">`Newtonsoft.Json`tabanlı serileştiriciyi geri yükleme yönergeleri için [Newtonsoft. JSON anahtarına geçin](xref:migration/22-to-30#switch-to-newtonsoftjson) .</span><span class="sxs-lookup"><span data-stu-id="1d2ab-157">See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson) for instructions to restore the `Newtonsoft.Json`-based serializer.</span></span>

<span data-ttu-id="1d2ab-158">SignalR için JavaScript ve .NET Istemcilerinde otomatik yeniden bağlantı için destek eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-158">In the JavaScript and .NET Clients for SignalR, support was added for automatic reconnection.</span></span> <span data-ttu-id="1d2ab-159">Varsayılan olarak, istemci hemen yeniden bağlanmaya çalışır ve gerekirse 2, 10 ve 30 saniye sonra yeniden dener.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-159">By default, the client tries to reconnect immediately and retry after 2, 10, and 30 seconds if necessary.</span></span> <span data-ttu-id="1d2ab-160">İstemci başarıyla yeniden bağlanırsa, yeni bir bağlantı KIMLIĞI alır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-160">If the client successfully reconnects, it receives a new connection ID.</span></span> <span data-ttu-id="1d2ab-161">Otomatik yeniden bağlanma kabul etme:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-161">Automatic reconnect is opt-in:</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="1d2ab-162">Yeniden bağlantı aralıkları bir dizi milisaniyelik tabanlı süre geçirerek belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-162">The reconnection intervals can be specified by passing an array of millisecond-based durations:</span></span>

```javascript
.withAutomaticReconnect([0, 3000, 5000, 10000, 15000, 30000])
//.withAutomaticReconnect([0, 2000, 10000, 30000]) The default intervals.
```

<span data-ttu-id="1d2ab-163">Yeniden bağlantı aralıklarının tam denetimi için özel bir uygulama geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-163">A custom implementation can be passed in for full control of the reconnection intervals.</span></span>

<span data-ttu-id="1d2ab-164">Son yeniden bağlanma aralığından sonra yeniden bağlantı başarısız olursa:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-164">If the reconnection fails after the last reconnect interval:</span></span>

* <span data-ttu-id="1d2ab-165">İstemci, bağlantının çevrimdışı olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-165">The client considers the connection is offline.</span></span>
* <span data-ttu-id="1d2ab-166">İstemci yeniden bağlanmayı denemeyi durduruyor.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-166">The client stops trying to reconnect.</span></span>

<span data-ttu-id="1d2ab-167">Yeniden bağlanma denemeleri sırasında, kullanıcıya yeniden bağlantı denenmekte olduğunu bildirmek için uygulama kullanıcı arabirimini güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-167">During reconnection attempts, update the app UI to notify the user that the reconnection is being attempted.</span></span>

<span data-ttu-id="1d2ab-168">Bağlantı kesildiğinde UI geri bildirimi sağlamak için, SignalR istemci API 'SI aşağıdaki olay işleyicilerini içerecek şekilde genişletilir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-168">To provide UI feedback when the connection is interrupted, the SignalR client API has been expanded to include the following event handlers:</span></span>

* <span data-ttu-id="1d2ab-169">`onreconnecting`: geliştiricilere Kullanıcı arabirimini devre dışı bırakma veya kullanıcıların uygulamanın çevrimdışı olduğunu bilmesini sağlayan bir fırsat sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-169">`onreconnecting`:  Gives developers an opportunity to disable UI or to let users know the app is offline.</span></span>
* <span data-ttu-id="1d2ab-170">`onreconnected`: bağlantı kurulduktan sonra geliştiricilere Kullanıcı arabirimini güncelleştirme fırsatı verir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-170">`onreconnected`: Gives developers an opportunity to update the UI once the connection is reestablished.</span></span>

<span data-ttu-id="1d2ab-171">Aşağıdaki kod, bağlanmaya çalışırken kullanıcı arabirimini güncelleştirmek için `onreconnecting` kullanır:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-171">The following code uses `onreconnecting` to update the UI while trying to connect:</span></span>

```javascript
connection.onreconnecting((error) => {
    const status = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messageInput").disabled = true;
    document.getElementById("sendButton").disabled = true;
    document.getElementById("connectionStatus").innerText = status;
});
```

<span data-ttu-id="1d2ab-172">Aşağıdaki kod, bağlantıda Kullanıcı arabirimini güncelleştirmek için `onreconnected` kullanır:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-172">The following code uses `onreconnected` to update the UI on connection:</span></span>

```javascript
connection.onreconnected((connectionId) => {
    const status = `Connection reestablished. Connected.`;
    document.getElementById("messageInput").disabled = false;
    document.getElementById("sendButton").disabled = false;
    document.getElementById("connectionStatus").innerText = status;
});
```

<span data-ttu-id="1d2ab-173">SignalR 3,0 ve üzeri, bir hub yöntemi yetkilendirme gerektirdiğinde, yetkilendirme işleyicilerine özel bir kaynak sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-173">SignalR 3.0 and later provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="1d2ab-174">Kaynak bir `HubInvocationContext` örneğidir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-174">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="1d2ab-175">`HubInvocationContext` şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-175">The `HubInvocationContext` includes the:</span></span>

* `HubCallerContext`
* <span data-ttu-id="1d2ab-176">Çağrılan hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-176">Name of the hub method being invoked.</span></span>
* <span data-ttu-id="1d2ab-177">Hub yöntemi için bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-177">Arguments to the hub method.</span></span>

<span data-ttu-id="1d2ab-178">Azure Active Directory aracılığıyla birden çok kuruluşun oturum açmasına izin veren bir sohbet odası uygulamasının aşağıdaki örneğini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-178">Consider the following example of a chat room app allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="1d2ab-179">Microsoft hesabı herkes sohbet için oturum açabilir, ancak yalnızca sahip olunan kuruluşun üyeleri kullanıcıları veya kullanıcıların sohbet geçmişlerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-179">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization can ban users or view users’ chat histories.</span></span> <span data-ttu-id="1d2ab-180">Uygulama belirli kullanıcılardan belirli işlevleri kısıtlayabilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-180">The app could restrict certain functionality from specific users.</span></span>

```csharp
public class DomainRestrictedRequirement :
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>,
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement,
        HubInvocationContext resource)
    {
        if (context.User?.Identity?.Name == null)
        {
            return Task.CompletedTask;
        }

        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name))
        {
            context.Succeed(requirement);
        }

        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName, string currentUsername)
    {
        if (hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase))
        {
            return currentUsername.Equals("bob42@jabbr.net", StringComparison.OrdinalIgnoreCase);
        }

        return currentUsername.EndsWith("@jabbr.net", StringComparison.OrdinalIgnoreCase));
    }
}
```

<span data-ttu-id="1d2ab-181">Yukarıdaki kodda, `DomainRestrictedRequirement` özel bir `IAuthorizationRequirement`işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-181">In the preceding code, `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="1d2ab-182">`HubInvocationContext` kaynak parametresi geçirildiğinden iç mantık şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-182">Because the `HubInvocationContext` resource parameter is being passed in, the internal logic can:</span></span>

* <span data-ttu-id="1d2ab-183">Hub 'ın çağrıldığı bağlamı inceleyin.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-183">Inspect the context in which the Hub is being called.</span></span>
* <span data-ttu-id="1d2ab-184">Kullanıcının bireysel hub yöntemlerini yürütmesine izin verirken kararlar alın.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-184">Make decisions on allowing the user to execute individual Hub methods.</span></span>

<span data-ttu-id="1d2ab-185">Tek tek hub yöntemleri, çalışma zamanında kodun denetlediği ilkenin adı ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-185">Individual Hub methods can be decorated with the name of the policy the code checks at run-time.</span></span> <span data-ttu-id="1d2ab-186">İstemciler tek tek hub yöntemlerini çağırmayı denediğinden `DomainRestrictedRequirement` işleyicisi çalışır ve yöntemlere erişimi denetler.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-186">As clients attempt to call individual Hub methods, the `DomainRestrictedRequirement` handler runs and controls access to the methods.</span></span> <span data-ttu-id="1d2ab-187">`DomainRestrictedRequirement` erişimi denetim altına göre:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-187">Based on the way the `DomainRestrictedRequirement` controls access:</span></span>

* <span data-ttu-id="1d2ab-188">Tüm oturum açmış kullanıcılar `SendMessage` yöntemini çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-188">All logged-in users can call the `SendMessage` method.</span></span>
* <span data-ttu-id="1d2ab-189">Yalnızca bir `@jabbr.net` e-posta adresiyle oturum açan kullanıcılar, kullanıcıların geçmişlerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-189">Only users who have logged in with a `@jabbr.net` email address can view users’ histories.</span></span>
* <span data-ttu-id="1d2ab-190">Yalnızca `bob42@jabbr.net` sohbet odasından kullanıcıları ban edebilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-190">Only `bob42@jabbr.net` can ban users from the chat room.</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}
```

<span data-ttu-id="1d2ab-191">`DomainRestricted` ilkesi oluşturmak şunları içerebilir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-191">Creating the `DomainRestricted` policy might involve:</span></span>

* <span data-ttu-id="1d2ab-192">*Startup.cs*' de, yeni ilkeyi ekleme.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-192">In *Startup.cs*, adding the new policy.</span></span>
* <span data-ttu-id="1d2ab-193">Özel `DomainRestrictedRequirement` gereksinimini parametre olarak sağlayın.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-193">Provide the custom `DomainRestrictedRequirement` requirement as a parameter.</span></span>
* <span data-ttu-id="1d2ab-194">Yetkilendirme ara yazılımı ile `DomainRestricted` kaydetme.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-194">Registering `DomainRestricted` with the authorization middleware.</span></span>

```csharp
services
    .AddAuthorization(options =>
    {
        options.AddPolicy("DomainRestricted", policy =>
        {
            policy.Requirements.Add(new DomainRestrictedRequirement());
        });
    });
```

<span data-ttu-id="1d2ab-195">SignalR hub 'ları [Endpoint Routing](xref:fundamentals/routing)kullanır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-195">SignalR hubs use [Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="1d2ab-196">SignalR Hub bağlantısı daha önce açık şekilde yapıldı:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-196">SignalR hub connection was previously done explicitly:</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="1d2ab-197">Önceki sürümde, geliştiriciler, Razor sayfaları ve hub 'ları çeşitli yerlerde bağlamak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-197">In the previous version, developers needed to wire up controllers, Razor pages, and hubs in a variety of places.</span></span> <span data-ttu-id="1d2ab-198">Açık bağlantı, neredeyse aynı bir dizi yönlendirme segmentine neden olur:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-198">Explicit connection results in a series of nearly-identical routing segments:</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});

app.UseRouting(routes =>
{
    routes.MapRazorPages();
});
```

<span data-ttu-id="1d2ab-199">SignalR 3,0 hub 'ları, uç nokta yönlendirme aracılığıyla yönlendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-199">SignalR 3.0 hubs can be routed via endpoint routing.</span></span> <span data-ttu-id="1d2ab-200">Uç nokta yönlendirme ile, genellikle tüm yönlendirme `UseRouting`yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-200">With endpoint routing, typically all routing can be configured in `UseRouting`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="1d2ab-201">ASP.NET Core 3,0 SignalR eklendi:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-201">ASP.NET Core 3.0 SignalR added:</span></span>

<span data-ttu-id="1d2ab-202">İstemciden sunucuya akış.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-202">Client-to-server streaming.</span></span> <span data-ttu-id="1d2ab-203">İstemci-sunucu akışı ile, sunucu tarafı yöntemleri bir `IAsyncEnumerable<T>` veya `ChannelReader<T>`örnekleri alabilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-203">With client-to-server streaming, server-side methods can take instances of either an `IAsyncEnumerable<T>` or `ChannelReader<T>`.</span></span> <span data-ttu-id="1d2ab-204">Aşağıdaki C# örnekte, Hub 'daki `UploadStream` yöntemi istemcisinden dizelerin akışını alacaktır:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-204">In the following C# sample, the `UploadStream` method on the Hub will receive a stream of strings from the client:</span></span>

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        // process content
    }
}
```

<span data-ttu-id="1d2ab-205">.NET istemci uygulamaları, yukarıdaki `UploadStream` hub yönteminin `stream` bağımsız değişkeni olarak bir `IAsyncEnumerable<T>` veya `ChannelReader<T>` örneğini geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-205">.NET client apps can pass either an `IAsyncEnumerable<T>` or `ChannelReader<T>` instance as the `stream` argument of the `UploadStream` Hub method above.</span></span>

<span data-ttu-id="1d2ab-206">`for` döngüsü tamamlandıktan ve yerel işlev çıktıktan sonra, akış tamamlama gönderilir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-206">After the `for` loop has completed and the local function exits, the stream completion is sent:</span></span>

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
}

await connection.SendAsync("UploadStream", clientStreamData());
```

<span data-ttu-id="1d2ab-207">JavaScript istemci uygulamaları, yukarıdaki `UploadStream` hub yönteminin `stream` bağımsız değişkeni için SignalR `Subject` (veya bir [Rxjs konusu](https://rxjs.dev/api/index/class/Subject)) kullanır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-207">JavaScript client apps use the SignalR `Subject` (or an [RxJS Subject](https://rxjs.dev/api/index/class/Subject)) for the `stream` argument of the `UploadStream` Hub method above.</span></span>

```javascript
let subject = new signalR.Subject();
await connection.send("StartStream", "MyAsciiArtStream", subject);
```

<span data-ttu-id="1d2ab-208">JavaScript kodu, yakalandıkları ve sunucuya gönderilmeye hazırlanıyor gibi dizeleri işlemek için `subject.next` yöntemini kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-208">The JavaScript code could use the `subject.next` method to handle strings as they are captured and ready to be sent to the server.</span></span>

```javascript
subject.next("example");
subject.complete();
```

<span data-ttu-id="1d2ab-209">Önceki iki kod parçacığı gibi kodu kullanarak gerçek zamanlı akış deneyimleri oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-209">Using code like the two preceding snippets, real-time streaming experiences can be created.</span></span>

## <a name="new-json-serialization"></a><span data-ttu-id="1d2ab-210">Yeni JSON serileştirmesi</span><span class="sxs-lookup"><span data-stu-id="1d2ab-210">New JSON serialization</span></span>

<span data-ttu-id="1d2ab-211">ASP.NET Core 3,0 artık JSON serileştirme için varsayılan olarak <xref:System.Text.Json> kullanır:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-211">ASP.NET Core 3.0 now uses <xref:System.Text.Json> by default for JSON serialization:</span></span>

* <span data-ttu-id="1d2ab-212">JSON 'yi zaman uyumsuz olarak okur ve yazar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-212">Reads and writes JSON asynchronously.</span></span>
* <span data-ttu-id="1d2ab-213">UTF-8 metni için iyileştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-213">Is optimized for UTF-8 text.</span></span>
* <span data-ttu-id="1d2ab-214">Genellikle `Newtonsoft.Json` ' dan daha yüksek performans.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-214">Typically higher performance than `Newtonsoft.Json`.</span></span>

<span data-ttu-id="1d2ab-215">ASP.NET Core 3,0 ' ye Json.NET eklemek için bkz. [Newtonsoft. JSON tabanlı JSON biçimi desteği ekleme](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-215">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span>

## <a name="new-razor-directives"></a><span data-ttu-id="1d2ab-216">Yeni Razor yönergeleri</span><span class="sxs-lookup"><span data-stu-id="1d2ab-216">New Razor directives</span></span>

<span data-ttu-id="1d2ab-217">Aşağıdaki listede yeni Razor yönergeleri yer almaktadır:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-217">The following list contains new Razor directives:</span></span>

* <span data-ttu-id="1d2ab-218">[@attribute](xref:mvc/views/razor#attribute) &ndash; `@attribute` yönergesinin verilen özniteliği oluşturulan sayfanın veya görünümün sınıfına uygular.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-218">[@attribute](xref:mvc/views/razor#attribute) &ndash; The `@attribute` directive applies the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="1d2ab-219">Örneğin, `@attribute [Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-219">For example, `@attribute [Authorize]`.</span></span>
* <span data-ttu-id="1d2ab-220">[@implements](xref:mvc/views/razor#implements) &ndash; `@implements` yönergesi oluşturulan sınıf için bir arabirim uygular.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-220">[@implements](xref:mvc/views/razor#implements) &ndash; The `@implements` directive implements an interface for the generated class.</span></span> <span data-ttu-id="1d2ab-221">Örneğin, `@implements IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-221">For example, `@implements IDisposable`.</span></span>

## <a name="identityserver4-supports-authentication-and-authorization-for-web-apis-and-spas"></a><span data-ttu-id="1d2ab-222">Identityserver4, Web API 'Leri ve maça 'Ları için kimlik doğrulama ve yetkilendirmeyi destekler</span><span class="sxs-lookup"><span data-stu-id="1d2ab-222">IdentityServer4 supports authentication and authorization for web APIs and SPAs</span></span>

<span data-ttu-id="1d2ab-223">ASP.NET Core 3,0, Web API yetkilendirmesi desteğini kullanarak tek sayfalı uygulamalarda (Spaon) kimlik doğrulaması sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-223">ASP.NET Core 3.0 offers authentication in Single Page Apps (SPAs) using the support for web API authorization.</span></span> <span data-ttu-id="1d2ab-224">Kullanıcıların kimliğini doğrulamak ve depolamak için ASP.NET Core kimliği, açık KIMLIK Connect 'i uygulamak için [ıdentityserver4](https://identityserver.io/) ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-224">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer4](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="1d2ab-225">Identityserver4, ASP.NET Core 3,0 için bir OpenID Connect ve OAuth 2,0 çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-225">IdentityServer4 is an OpenID Connect and OAuth 2.0 framework for ASP.NET Core 3.0.</span></span> <span data-ttu-id="1d2ab-226">Aşağıdaki güvenlik özelliklerini sunar:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-226">It enables the following security features:</span></span>

* <span data-ttu-id="1d2ab-227">Hizmet olarak kimlik doğrulaması (AaaS)</span><span class="sxs-lookup"><span data-stu-id="1d2ab-227">Authentication as a Service (AaaS)</span></span>
* <span data-ttu-id="1d2ab-228">Birden çok uygulama türü üzerinde çoklu oturum açma/kapatma (SSO)</span><span class="sxs-lookup"><span data-stu-id="1d2ab-228">Single sign-on/off (SSO) over multiple application types</span></span>
* <span data-ttu-id="1d2ab-229">API 'Ler için erişim denetimi</span><span class="sxs-lookup"><span data-stu-id="1d2ab-229">Access control for APIs</span></span>
* <span data-ttu-id="1d2ab-230">Federasyon ağ geçidi</span><span class="sxs-lookup"><span data-stu-id="1d2ab-230">Federation Gateway</span></span>

<span data-ttu-id="1d2ab-231">Daha fazla bilgi için bkz. [ıdentityserver4 belgeleri](http://docs.identityserver.io/en/latest/index.html) veya [kimlik doğrulaması ve kimlik doğrulama ve yetkilendirme](xref:security/authentication/identity/spa).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-231">For more information, see [the IdentityServer4 documentation](http://docs.identityserver.io/en/latest/index.html) or [Authentication and authorization for SPAs](xref:security/authentication/identity/spa).</span></span>

## <a name="certificate-and-kerberos-authentication"></a><span data-ttu-id="1d2ab-232">Sertifika ve Kerberos kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="1d2ab-232">Certificate and Kerberos authentication</span></span>

<span data-ttu-id="1d2ab-233">Sertifika kimlik doğrulaması şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-233">Certificate authentication requires:</span></span>

* <span data-ttu-id="1d2ab-234">Sunucu, sertifikaları kabul edecek şekilde yapılandırılıyor.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-234">Configuring the server to accept certificates.</span></span>
* <span data-ttu-id="1d2ab-235">Kimlik doğrulama ara yazılımı `Startup.Configure`' ye ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-235">Adding the authentication middleware in `Startup.Configure`.</span></span>
* <span data-ttu-id="1d2ab-236">Sertifika kimlik doğrulama hizmeti `Startup.ConfigureServices`' de ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-236">Adding the certificate authentication service in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

<span data-ttu-id="1d2ab-237">Sertifika kimlik doğrulaması seçenekleri şunları yapabilme:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-237">Options for certificate authentication include the ability to:</span></span>

* <span data-ttu-id="1d2ab-238">Otomatik olarak imzalanan sertifikaları kabul edin.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-238">Accept self-signed certificates.</span></span>
* <span data-ttu-id="1d2ab-239">Sertifika iptali olup olmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-239">Check for certificate revocation.</span></span>
* <span data-ttu-id="1d2ab-240">Profili oluşturulan sertifikanın, içinde doğru kullanım bayrakları olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-240">Check that the proffered certificate has the right usage flags in it.</span></span>

<span data-ttu-id="1d2ab-241">Varsayılan bir Kullanıcı sorumlusu, sertifika özelliklerinden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-241">A default user principal is constructed from the certificate properties.</span></span> <span data-ttu-id="1d2ab-242">Kullanıcı sorumlusu, sorumlunun takıma veya değiştirilmesine izin veren bir olay içerir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-242">The user principal contains an event that enables supplementing or replacing the principal.</span></span> <span data-ttu-id="1d2ab-243">Daha fazla bilgi için bkz. <xref:security/authentication/certauth>.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-243">For more information, see <xref:security/authentication/certauth>.</span></span>

<span data-ttu-id="1d2ab-244">[Windows kimlik doğrulaması](/windows-server/security/windows-authentication/windows-authentication-overview) , Linux ve MacOS üzerine genişletildi.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-244">[Windows Authentication](/windows-server/security/windows-authentication/windows-authentication-overview) has been extended onto Linux and macOS.</span></span> <span data-ttu-id="1d2ab-245">Önceki sürümlerde, Windows kimlik doğrulaması [IIS](xref:host-and-deploy/iis/index) ve [httpsys](xref:fundamentals/servers/httpsys)ile sınırlandırıldı.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-245">In previous versions, Windows Authentication was limited to [IIS](xref:host-and-deploy/iis/index) and [HttpSys](xref:fundamentals/servers/httpsys).</span></span> <span data-ttu-id="1d2ab-246">ASP.NET Core 3,0 ' de [Kestrel](xref:fundamentals/servers/kestrel) , Windows etki alanına katılmış konaklar için Windows, Linux ve MacOS üzerinde Negotiate, [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview)ve [NTLM](/windows-server/security/kerberos/ntlm-overview)kullanma olanağına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-246">In ASP.NET Core 3.0, [Kestrel](xref:fundamentals/servers/kestrel) has the ability to use Negotiate, [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview), and [NTLM on Windows](/windows-server/security/kerberos/ntlm-overview), Linux, and macOS for Windows domain-joined hosts.</span></span> <span data-ttu-id="1d2ab-247">Bu kimlik doğrulama düzenlerinin Kestrel desteği [Microsoft. AspNetCore. Authentication. Negotiate NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-247">Kestrel support of these authentication schemes is provided by the [Microsoft.AspNetCore.Authentication.Negotiate NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package.</span></span> <span data-ttu-id="1d2ab-248">Diğer kimlik doğrulama hizmetlerinde olduğu gibi, kimlik doğrulama uygulaması genelinde yapılandırın ve ardından hizmeti yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-248">As with the other authentication services, configure authentication app wide, then configure the service:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
        .AddNegotiate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

<span data-ttu-id="1d2ab-249">Ana bilgisayar gereksinimleri:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-249">Host requirements:</span></span>

* <span data-ttu-id="1d2ab-250">Windows Konakları, uygulamayı barındıran Kullanıcı hesabına eklenmiş [hizmet sorumlusu adlarına](/windows/win32/ad/service-principal-names) (SPN) sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-250">Windows hosts must have [Service Principal Names](/windows/win32/ad/service-principal-names) (SPNs) added to the user account hosting the app.</span></span>
* <span data-ttu-id="1d2ab-251">Linux ve macOS makineleri etki alanına katılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-251">Linux and macOS machines must be joined to the domain.</span></span>
  * <span data-ttu-id="1d2ab-252">Web işlemi için SPN oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-252">SPNs must be created for the web process.</span></span>
  * <span data-ttu-id="1d2ab-253">Ana makinede [keytab dosyalarının](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) oluşturulup yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-253">[Keytab files](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) must be generated and configured on the host machine.</span></span>

<span data-ttu-id="1d2ab-254">Daha fazla bilgi için bkz. <xref:security/authentication/windowsauth>.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-254">For more information, see <xref:security/authentication/windowsauth>.</span></span>

## <a name="template-changes"></a><span data-ttu-id="1d2ab-255">Şablon değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="1d2ab-255">Template changes</span></span>

<span data-ttu-id="1d2ab-256">Web UI şablonları (Razor Pages, denetleyici ve görünümlerle MVC) şu şekilde kaldırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-256">The web UI templates (Razor Pages, MVC with controller and views) have the following removed:</span></span>

* <span data-ttu-id="1d2ab-257">Tanımlama bilgisi onayı Kullanıcı arabirimi artık dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-257">The cookie consent UI is no longer included.</span></span> <span data-ttu-id="1d2ab-258">ASP.NET Core 3,0 şablon tarafından oluşturulan bir uygulamada tanımlama bilgisi onay özelliğini etkinleştirmek için, bkz. <xref:security/gdpr>.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-258">To enable the cookie consent feature in an ASP.NET Core 3.0 template-generated app, see <xref:security/gdpr>.</span></span>
* <span data-ttu-id="1d2ab-259">Betiklerin ve ilgili statik varlıkların artık CDNs kullanmak yerine yerel dosyalar olarak başvuruluyor.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-259">Scripts and related static assets are now referenced as local files instead of using CDNs.</span></span> <span data-ttu-id="1d2ab-260">Daha fazla bilgi için, bkz. [betiklerin ve ilgili statik varlıkların artık geçerli ortama göre CDNs kullanmak yerine yerel dosyalar olarak başvuruluyor (ASPNET/AspNetCore. Docs #14350)](https://github.com/aspnet/AspNetCore.Docs/issues/14350).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-260">For more information, see [Scripts and related static assets are now referenced as local files instead of using CDNs based on the current environment (aspnet/AspNetCore.Docs #14350)](https://github.com/aspnet/AspNetCore.Docs/issues/14350).</span></span>

<span data-ttu-id="1d2ab-261">Angular şablonu, angular 8 ' i kullanacak şekilde güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-261">The Angular template updated to use Angular 8.</span></span>

<span data-ttu-id="1d2ab-262">Razor sınıf kitaplığı (RCL) şablonu varsayılan olarak Razor bileşen geliştirmeyi varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-262">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="1d2ab-263">Visual Studio 'da yeni bir şablon seçeneği, sayfalar ve görünümler için şablon desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-263">A new template option in Visual Studio provides template support for pages and views.</span></span> <span data-ttu-id="1d2ab-264">Bir komut kabuğunda şablondan RCL oluştururken `--support-pages-and-views` seçeneğini (`dotnet new razorclasslib --support-pages-and-views`) geçirin.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-264">When creating an RCL from the template in a command shell, pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`).</span></span>

## <a name="generic-host"></a><span data-ttu-id="1d2ab-265">Genel Konak</span><span class="sxs-lookup"><span data-stu-id="1d2ab-265">Generic Host</span></span>

<span data-ttu-id="1d2ab-266">ASP.NET Core 3,0 şablonları <xref:fundamentals/host/generic-host>kullanır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-266">The ASP.NET Core 3.0 templates use <xref:fundamentals/host/generic-host>.</span></span> <span data-ttu-id="1d2ab-267">Önceki sürümler <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>kullanıldı.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-267">Previous versions used <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="1d2ab-268">.NET Core genel ana bilgisayarı 'nı (<xref:Microsoft.Extensions.Hosting.HostBuilder>) kullanarak, Web 'e özgü olmayan diğer sunucu senaryolarıyla ASP.NET Core uygulamaları daha iyi tümleştirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-268">Using the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) provides better integration of ASP.NET Core apps with other server scenarios that aren't web-specific.</span></span> <span data-ttu-id="1d2ab-269">Daha fazla bilgi için bkz. [Hostbuilder WebHostBuilder 'ın yerini alır](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-269">For more information, see [HostBuilder replaces WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder).</span></span>

### <a name="host-configuration"></a><span data-ttu-id="1d2ab-270">Konak yapılandırması</span><span class="sxs-lookup"><span data-stu-id="1d2ab-270">Host configuration</span></span>

<span data-ttu-id="1d2ab-271">ASP.NET Core 3,0 ' nin yayınlanmasından önce, `ASPNETCORE_` önekli ortam değişkenleri Web konağının ana bilgisayar yapılandırması için yüklendi.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-271">Prior to the release of ASP.NET Core 3.0, environment variables prefixed with `ASPNETCORE_` were loaded for host configuration of the Web Host.</span></span> <span data-ttu-id="1d2ab-272">3,0 ' de `AddEnvironmentVariables`, `CreateDefaultBuilder`ile konak yapılandırması için `DOTNET_` ön eki eklenmiş ortam değişkenlerini yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-272">In 3.0, `AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for host configuration with `CreateDefaultBuilder`.</span></span>

### <a name="changes-to-startup-constructor-injection"></a><span data-ttu-id="1d2ab-273">Başlangıç Oluşturucu Ekleme değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="1d2ab-273">Changes to Startup constructor injection</span></span>

<span data-ttu-id="1d2ab-274">Genel ana bilgisayar yalnızca `Startup` Oluşturucu ekleme için aşağıdaki türleri destekler:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-274">The Generic Host only supports the following types for `Startup` constructor injection:</span></span>

* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="1d2ab-275">Tüm hizmetler yine de `Startup.Configure` metoduna bağımsız değişken olarak eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-275">All services can still be injected directly as arguments to the `Startup.Configure` method.</span></span> <span data-ttu-id="1d2ab-276">Daha fazla bilgi için bkz. [genel ana bilgisayar başlangıç Oluşturucu Ekleme (ASPNET/duyurular #353)](https://github.com/aspnet/Announcements/issues/353).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-276">For more information, see [Generic Host restricts Startup constructor injection (aspnet/Announcements #353)](https://github.com/aspnet/Announcements/issues/353).</span></span>

## <a name="kestrel"></a><span data-ttu-id="1d2ab-277">Kestrel</span><span class="sxs-lookup"><span data-stu-id="1d2ab-277">Kestrel</span></span>

* <span data-ttu-id="1d2ab-278">Kestrel yapılandırması, genel konağa geçiş için güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-278">Kestrel configuration has been updated for the migration to the Generic Host.</span></span> <span data-ttu-id="1d2ab-279">3,0 ' de Kestrel, `ConfigureWebHostDefaults`tarafından belirtilen Web ana bilgisayar Oluşturucu üzerinde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-279">In 3.0, Kestrel is configured on the web host builder provided by `ConfigureWebHostDefaults`.</span></span>
* <span data-ttu-id="1d2ab-280">Bağlantı bağdaştırıcıları, Kestrel adresinden kaldırılmıştır ve bağlantı ara yazılımı ile değiştirilmiştir ve bu, ASP.NET Core işlem hattındaki HTTP ara hattına benzer ancak alt düzey bağlantılar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-280">Connection Adapters have been removed from Kestrel and replaced with Connection Middleware, which is similar to HTTP Middleware in the ASP.NET Core pipeline but for lower-level connections.</span></span>
* <span data-ttu-id="1d2ab-281">Kestrel aktarım katmanı `Connections.Abstractions` ' da ortak arabirim olarak kullanıma sunuldu.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-281">The Kestrel transport layer has been exposed as a public interface in `Connections.Abstractions`.</span></span>
* <span data-ttu-id="1d2ab-282">Sondaki üstbilgiler yeni bir koleksiyona taşınarak üstbilgiler ve tanıtımları arasındaki belirsizlik çözüldü.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-282">Ambiguity between headers and trailers has been resolved by moving trailing headers to a new collection.</span></span>
* <span data-ttu-id="1d2ab-283">`HttpRequest.Body.Read`gibi zaman uyumlu GÇ API 'Leri, uygulama kilitlenmelerine neden olan yaygın bir iş parçacığı kaynağıdır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-283">Synchronous IO APIs, such as `HttpRequest.Body.Read`, are a common source of thread starvation leading to app crashes.</span></span> <span data-ttu-id="1d2ab-284">3,0 ' de, `AllowSynchronousIO` varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-284">In 3.0, `AllowSynchronousIO` is disabled by default.</span></span>

<span data-ttu-id="1d2ab-285">Daha fazla bilgi için bkz. <xref:migration/22-to-30#kestrel>.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-285">For more information, see <xref:migration/22-to-30#kestrel>.</span></span>

## <a name="http2-enabled-by-default"></a><span data-ttu-id="1d2ab-286">Varsayılan olarak etkin HTTP/2</span><span class="sxs-lookup"><span data-stu-id="1d2ab-286">HTTP/2 enabled by default</span></span>

<span data-ttu-id="1d2ab-287">HTTP/2, HTTPS uç noktaları için Kestrel içinde varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-287">HTTP/2 is enabled by default in Kestrel for HTTPS endpoints.</span></span> <span data-ttu-id="1d2ab-288">IIS veya HTTP. sys için HTTP/2 desteği, işletim sistemi tarafından desteklendikleri zaman etkindir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-288">HTTP/2 support for IIS or HTTP.sys is enabled when supported by the operating system.</span></span>

## <a name="eventcounters-on-request"></a><span data-ttu-id="1d2ab-289">İstek üzerine EventCounters</span><span class="sxs-lookup"><span data-stu-id="1d2ab-289">EventCounters on request</span></span>

<span data-ttu-id="1d2ab-290">`Microsoft.AspNetCore.Hosting`barındırma EventSource, gelen isteklerle ilgili aşağıdaki yeni <xref:System.Diagnostics.Tracing.EventCounter> türlerini yayar:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-290">The Hosting EventSource, `Microsoft.AspNetCore.Hosting`, emits the following new <xref:System.Diagnostics.Tracing.EventCounter> types related to incoming requests:</span></span>

* `requests-per-second`
* `total-requests`
* `current-requests`
* `failed-requests`

## <a name="endpoint-routing"></a><span data-ttu-id="1d2ab-291">Uç nokta yönlendirme</span><span class="sxs-lookup"><span data-stu-id="1d2ab-291">Endpoint routing</span></span>

<span data-ttu-id="1d2ab-292">Çerçeveler 'in (örneğin, MVC) ara yazılım ile iyi çalışmasına izin veren uç nokta yönlendirme geliştirildi:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-292">Endpoint Routing, which allows frameworks (for example, MVC) to work well with middleware, is enhanced:</span></span>

* <span data-ttu-id="1d2ab-293">Ara yazılım ve uç noktaların sırası, `Startup.Configure`istek işleme ardışık düzeninde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-293">The order of middleware and endpoints is configurable in the request processing pipeline of `Startup.Configure`.</span></span>
* <span data-ttu-id="1d2ab-294">Uç noktalar ve ara yazılımlar, sistem durumu denetimleri gibi diğer ASP.NET Core tabanlı teknolojilerle birlikte.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-294">Endpoints and middleware compose well with other ASP.NET Core-based technologies, such as Health Checks.</span></span>
* <span data-ttu-id="1d2ab-295">Uç noktalar, hem yazılım hem de MVC 'de CORS veya yetkilendirme gibi bir ilke uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-295">Endpoints can implement a policy, such as CORS or authorization, in both middleware and MVC.</span></span>
* <span data-ttu-id="1d2ab-296">Filtreler ve öznitelikler, denetleyicilerde yöntemler üzerine yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-296">Filters and attributes can be placed on methods in controllers.</span></span>

<span data-ttu-id="1d2ab-297">Daha fazla bilgi için bkz. <xref:fundamentals/routing#routing-basics>.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-297">For more information, see <xref:fundamentals/routing#routing-basics>.</span></span>

## <a name="health-checks"></a><span data-ttu-id="1d2ab-298">Sistem durumu denetimleri</span><span class="sxs-lookup"><span data-stu-id="1d2ab-298">Health Checks</span></span>

<span data-ttu-id="1d2ab-299">Sistem durumu denetimleri, genel ana bilgisayar ile Endpoint Routing kullanır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-299">Health Checks use endpoint routing with the Generic Host.</span></span> <span data-ttu-id="1d2ab-300">`Startup.Configure`, uç nokta URL 'SI veya göreli yol ile Endpoint Builder üzerinde `MapHealthChecks` çağırın:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-300">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

<span data-ttu-id="1d2ab-301">Durum denetimleri uç noktaları şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-301">Health Checks endpoints can:</span></span>

* <span data-ttu-id="1d2ab-302">İzin verilen bir veya daha fazla Konakları/bağlantı noktasını belirtin.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-302">Specify one or more permitted hosts/ports.</span></span>
* <span data-ttu-id="1d2ab-303">Yetkilendirme gerektir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-303">Require authorization.</span></span>
* <span data-ttu-id="1d2ab-304">CORS gerektir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-304">Require CORS.</span></span>

<span data-ttu-id="1d2ab-305">Daha fazla bilgi için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-305">For more information, see the following articles:</span></span>

* <xref:migration/22-to-30#health-checks>
* <xref:host-and-deploy/health-checks>

## <a name="pipes-on-httpcontext"></a><span data-ttu-id="1d2ab-306">HttpContext üzerindeki kanallar</span><span class="sxs-lookup"><span data-stu-id="1d2ab-306">Pipes on HttpContext</span></span>

<span data-ttu-id="1d2ab-307">Artık istek gövdesini okumak ve <xref:System.IO.Pipelines> API 'sini kullanarak yanıt gövdesini yazmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-307">It's now possible to read the request body and write the response body using the <xref:System.IO.Pipelines> API.</span></span> <span data-ttu-id="1d2ab-308">Bu</span><span class="sxs-lookup"><span data-stu-id="1d2ab-308">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.HttpRequest.BodyReader> --> <span data-ttu-id="1d2ab-309">`HttpRequest.BodyReader` özellik, istek gövdesini okumak için kullanılabilecek bir <xref:System.IO.Pipelines.PipeReader> sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-309">`HttpRequest.BodyReader` property provides a <xref:System.IO.Pipelines.PipeReader> that can be used to read the request body.</span></span> <span data-ttu-id="1d2ab-310">Bu</span><span class="sxs-lookup"><span data-stu-id="1d2ab-310">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.> --> <span data-ttu-id="1d2ab-311">`HttpResponse.BodyWriter` özellik, yanıt gövdesini yazmak için kullanılabilecek bir <xref:System.IO.Pipelines.PipeWriter> sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-311">`HttpResponse.BodyWriter` property provides a <xref:System.IO.Pipelines.PipeWriter> that can be used to write the response body.</span></span> <span data-ttu-id="1d2ab-312">`HttpRequest.BodyReader`, `HttpRequest.Body` akışının analogıdır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-312">`HttpRequest.BodyReader` is an analogue of the `HttpRequest.Body` stream.</span></span> <span data-ttu-id="1d2ab-313">`HttpResponse.BodyWriter`, `HttpResponse.Body` akışının analogıdır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-313">`HttpResponse.BodyWriter` is an analogue of the `HttpResponse.Body` stream.</span></span>

<!-- indirectly related, https://github.com/dotnet/docs/pull/14414 won't be published by 9/23  -->

## <a name="improved-error-reporting-in-iis"></a><span data-ttu-id="1d2ab-314">IIS 'de geliştirilmiş hata raporlama</span><span class="sxs-lookup"><span data-stu-id="1d2ab-314">Improved error reporting in IIS</span></span>

<span data-ttu-id="1d2ab-315">IIS 'de ASP.NET Core uygulamalar barındırırken başlatma hataları artık daha zengin Tanılama verileri oluşturuyor.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-315">Startup errors when hosting ASP.NET Core apps in IIS now produce richer diagnostic data.</span></span> <span data-ttu-id="1d2ab-316">Bu hatalar, geçerli her yerde yığın izlemelerle Windows olay günlüğü 'ne bildirilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-316">These errors are reported to the Windows Event Log with stack traces wherever applicable.</span></span> <span data-ttu-id="1d2ab-317">Ayrıca, tüm uyarılar, hatalar ve işlenmemiş özel durumlar Windows olay günlüğü 'ne kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-317">In addition, all warnings, errors, and unhandled exceptions are logged to the Windows Event Log.</span></span>

## <a name="worker-service-and-worker-sdk"></a><span data-ttu-id="1d2ab-318">Çalışan hizmeti ve çalışan SDK 'Sı</span><span class="sxs-lookup"><span data-stu-id="1d2ab-318">Worker Service and Worker SDK</span></span>

<span data-ttu-id="1d2ab-319">.NET Core 3,0 yeni çalışan hizmeti uygulama şablonunu tanıtır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-319">.NET Core 3.0 introduces the new Worker Service app template.</span></span> <span data-ttu-id="1d2ab-320">Bu şablon, .NET Core 'da uzun süre çalışan hizmetler yazmak için bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-320">This template provides a starting point for writing long running services in .NET Core.</span></span>

<span data-ttu-id="1d2ab-321">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-321">For more information, see:</span></span>

* [<span data-ttu-id="1d2ab-322">Windows Hizmetleri olarak .NET Core çalışanları</span><span class="sxs-lookup"><span data-stu-id="1d2ab-322">.NET Core Workers as Windows Services</span></span>](https://devblogs.microsoft.com/aspnet/net-core-workers-as-windows-services/)
* <xref:fundamentals/host/hosted-services>
* <xref:host-and-deploy/windows-service>

## <a name="forwarded-headers-middleware-improvements"></a><span data-ttu-id="1d2ab-323">İletilen üstbilgiler ara yazılımı geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="1d2ab-323">Forwarded Headers Middleware improvements</span></span>

<span data-ttu-id="1d2ab-324">Önceki ASP.NET Core sürümlerinde, <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> ve <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> çağırmak, bir Azure Linux 'a veya IIS dışında herhangi bir ters proxy 'nin arkasına dağıtıldığında sorunlu bir sorun oluştu.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-324">In previous versions of ASP.NET Core, calling <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> and  <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> were problematic when deployed to an Azure Linux or behind any reverse proxy other than IIS.</span></span> <span data-ttu-id="1d2ab-325">Önceki sürümlere yönelik düzeltmeler, [Linux ve IIS olmayan ters proxy 'lerin düzenini iletme](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies)bölümünde belgelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-325">The fix for previous versions is documented in [Forward the scheme for Linux and non-IIS reverse proxies](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies).</span></span>

<span data-ttu-id="1d2ab-326">Bu senaryo ASP.NET Core 3,0 ' de düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-326">This scenario is fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="1d2ab-327">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` ortam değişkeni `true`olarak ayarlandığında, ana bilgisayar [Iletilen üstbilgiler ara yazılımını](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options) sağlar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-327">The host enables the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options) when the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span> <span data-ttu-id="1d2ab-328">`ASPNETCORE_FORWARDEDHEADERS_ENABLED`, kapsayıcı görüntülerimizde `true` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-328">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` is set to `true` in our container images.</span></span>

## <a name="performance-improvements"></a><span data-ttu-id="1d2ab-329">Performans geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="1d2ab-329">Performance improvements</span></span>

<span data-ttu-id="1d2ab-330">ASP.NET Core 3,0, bellek kullanımını azaltan ve üretilen işi geliştiren birçok geliştirme içerir:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-330">ASP.NET Core 3.0 includes many improvements that reduce memory usage and improve throughput:</span></span>

* <span data-ttu-id="1d2ab-331">Kapsamlı hizmetler için yerleşik bağımlılık ekleme kapsayıcısını kullanırken bellek kullanımında azaltma.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-331">Reduction in memory usage when using the built-in dependency injection container for scoped services.</span></span>
* <span data-ttu-id="1d2ab-332">Ara yazılım senaryoları ve yönlendirme dahil olmak üzere çerçeve genelinde ayırmalarda azaltma.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-332">Reduction in allocations across the framework, including middleware scenarios and routing.</span></span>
* <span data-ttu-id="1d2ab-333">WebSocket bağlantıları için bellek kullanımında azaltma.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-333">Reduction in memory usage for WebSocket connections.</span></span>
* <span data-ttu-id="1d2ab-334">HTTPS bağlantıları için bellek azaltma ve verimlilik geliştirmeleri.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-334">Memory reduction and throughput improvements for HTTPS connections.</span></span>
* <span data-ttu-id="1d2ab-335">Yeni iyileştirilmiş ve tamamen zaman uyumsuz JSON serileştiricisi.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-335">New optimized and fully asynchronous JSON serializer.</span></span>
* <span data-ttu-id="1d2ab-336">Form ayrıştırılırken bellek kullanımı ve üretilen iş iyileştirmeleri azaltılması.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-336">Reduction in memory usage and throughput improvements in form parsing.</span></span>

## <a name="aspnet-core-30-only-runs-on-net-core-30"></a><span data-ttu-id="1d2ab-337">ASP.NET Core 3,0 yalnızca .NET Core 3,0 üzerinde çalışır</span><span class="sxs-lookup"><span data-stu-id="1d2ab-337">ASP.NET Core 3.0 only runs on .NET Core 3.0</span></span>

<span data-ttu-id="1d2ab-338">ASP.NET Core 3,0 itibariyle, .NET Framework artık desteklenen bir hedef çerçeve değildir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-338">As of ASP.NET Core 3.0, .NET Framework is no longer a supported target framework.</span></span> <span data-ttu-id="1d2ab-339">.NET Framework hedefleyen projeler [.NET Core 2,1 LTS sürümünü](https://www.microsoft.com/net/download/dotnet-core/2.1)kullanarak tam olarak desteklenen bir biçimde devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-339">Projects targeting .NET Framework can continue in a fully supported fashion using the [.NET Core 2.1 LTS release](https://www.microsoft.com/net/download/dotnet-core/2.1).</span></span> <span data-ttu-id="1d2ab-340">ASP.NET Core 2.1. x ile ilgili paketlerin çoğu, .NET Core 2,1 için üç yıllık LTS döneminin ötesinde süresiz olarak desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-340">Most ASP.NET Core 2.1.x related packages will be supported indefinitely, beyond the three-year LTS period for .NET Core 2.1.</span></span>

<span data-ttu-id="1d2ab-341">Geçiş bilgileri için bkz. [kodunuzu .NET Core 'a .NET Framework](/dotnet/core/porting/).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-341">For migration information, see [Port your code from .NET Framework to .NET Core](/dotnet/core/porting/).</span></span>

## <a name="use-the-aspnet-core-shared-framework"></a><span data-ttu-id="1d2ab-342">ASP.NET Core paylaşılan çerçevesini kullanma</span><span class="sxs-lookup"><span data-stu-id="1d2ab-342">Use the ASP.NET Core shared framework</span></span>

<span data-ttu-id="1d2ab-343">[Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunan ASP.NET Core 3,0 paylaşılan çerçevesi artık proje dosyasında açık bir `<PackageReference />` öğesi gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-343">The ASP.NET Core 3.0 shared framework, contained in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), no longer requires an explicit `<PackageReference />` element in the project file.</span></span> <span data-ttu-id="1d2ab-344">Paylaşılan çerçeveye proje dosyasında `Microsoft.NET.Sdk.Web` SDK kullanılırken otomatik olarak başvurulur:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-344">The shared framework is automatically referenced when using the `Microsoft.NET.Sdk.Web` SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

## <a name="assemblies-removed-from-the-aspnet-core-shared-framework"></a><span data-ttu-id="1d2ab-345">ASP.NET Core paylaşılan çerçevesinden kaldırılan derlemeler</span><span class="sxs-lookup"><span data-stu-id="1d2ab-345">Assemblies removed from the ASP.NET Core shared framework</span></span>

<span data-ttu-id="1d2ab-346">ASP.NET Core 3,0 paylaşılan çerçevesinden çıkarılan en önemli derlemeler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-346">The most notable assemblies removed from the ASP.NET Core 3.0 shared framework are:</span></span>

* <span data-ttu-id="1d2ab-347">[Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/) (JSON.net).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-347">[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) (Json.NET).</span></span> <span data-ttu-id="1d2ab-348">ASP.NET Core 3,0 ' ye Json.NET eklemek için bkz. [Newtonsoft. JSON tabanlı JSON biçimi desteği ekleme](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-348">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span> <span data-ttu-id="1d2ab-349">ASP.NET Core 3,0, JSON okuma ve yazma için `System.Text.Json` tanıtır.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-349">ASP.NET Core 3.0 introduces `System.Text.Json` for reading and writing JSON.</span></span> <span data-ttu-id="1d2ab-350">Daha fazla bilgi için bu belgede [yenı JSON serileştirmesi](#new-json-serialization) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-350">For more information, see [New JSON serialization](#new-json-serialization) in this document.</span></span>
* [<span data-ttu-id="1d2ab-351">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1d2ab-351">Entity Framework Core</span></span>](/ef/core/)

<span data-ttu-id="1d2ab-352">Paylaşılan çerçeveden kaldırılan derlemelerin tamamen listesi için bkz [. Microsoft. AspNetCore. App 3,0 ' den kaldırılan derlemeler](https://github.com/aspnet/AspNetCore/issues/3755).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-352">For a complete list of assemblies removed from the shared framework, see [Assemblies being removed from Microsoft.AspNetCore.App 3.0](https://github.com/aspnet/AspNetCore/issues/3755).</span></span> <span data-ttu-id="1d2ab-353">Bu değişiklik için mosyon hakkında daha fazla bilgi için bkz. [3,0 'de Microsoft. AspNetCore. app 'e yönelik son değişiklikler](https://github.com/aspnet/Announcements/issues/325) ve [ASP.NET Core 3,0 ' de gelen değişikliklere ilk bakış](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-353">For more information on the motivation for this change, see [Breaking changes to Microsoft.AspNetCore.App in 3.0](https://github.com/aspnet/Announcements/issues/325) and [A first look at changes coming in ASP.NET Core 3.0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<!-- 
## Additional information
For the complete list of changes, see the [ASP.NET Core 3.0 Release Notes](WHERE IS THIS????).
-->
