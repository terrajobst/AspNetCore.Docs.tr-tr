---
title: ASP.NET Core SignalR kimlik doğrulaması ve yetkilendirme
author: bradygaster
description: ASP.NET Core SignalRkimlik doğrulama ve yetkilendirmeyi nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- SignalR
uid: signalr/authn-and-authz
ms.openlocfilehash: 091cc9b2adc1f6a8fac79519884695d1c1725d2a
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880411"
---
# <a name="authentication-and-authorization-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="d52be-103">ASP.NET Core SignalR kimlik doğrulaması ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="d52be-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="d52be-104">, [Andrew Stanton-nurte](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="d52be-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="d52be-105">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(nasıl indirileceği)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="d52be-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-opno-locsignalr-hub"></a><span data-ttu-id="d52be-106">SignalR hub 'ına bağlanan kullanıcıların kimliğini doğrulama</span><span class="sxs-lookup"><span data-stu-id="d52be-106">Authenticate users connecting to a SignalR hub</span></span>

SignalR<span data-ttu-id="d52be-107">, bir kullanıcıyı her bağlantıyla ilişkilendirmek için [ASP.NET Core kimlik doğrulamasıyla](xref:security/authentication/identity) birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-107"> can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="d52be-108">Bir hub 'da, [Hubconnectioncontext. User](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) özelliğinden kimlik doğrulama verilerine erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-108">In a hub, authentication data can be accessed from the [HubConnectionContext.User](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="d52be-109">Kimlik doğrulaması, hub 'ın bir kullanıcıyla ilişkili tüm bağlantılar üzerinde Yöntemler çağırmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d52be-109">Authentication allows the hub to call methods on all connections associated with a user.</span></span> <span data-ttu-id="d52be-110">Daha fazla bilgi için bkz. [SignalRkullanıcıları ve grupları yönetme ](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="d52be-110">For more information, see [Manage users and groups in SignalR](xref:signalr/groups).</span></span> <span data-ttu-id="d52be-111">Birden çok bağlantı tek bir kullanıcıyla ilişkilendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-111">Multiple connections may be associated with a single user.</span></span>

<span data-ttu-id="d52be-112">Aşağıda, SignalR ve ASP.NET Core kimlik doğrulaması kullanan `Startup.Configure` bir örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="d52be-112">The following is an example of `Startup.Configure` which uses SignalR and ASP.NET Core authentication:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    app.UseRouting();

    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chat");
        endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    app.UseAuthentication();

    app.UseSignalR(hubs =>
    {
        hubs.MapHub<ChatHub>("/chat");
    });

    app.UseMvc(routes =>
    {
        routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
    });
}
```

> [!NOTE]
> <span data-ttu-id="d52be-113">SignalR ve ASP.NET Core kimlik doğrulama ara yazılımını kaydetme sırası önemli.</span><span class="sxs-lookup"><span data-stu-id="d52be-113">The order in which you register the SignalR and ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="d52be-114">SignalR `HttpContext`bir kullanıcıya sahip olması için `UseSignalR` önce her zaman `UseAuthentication` çağırın.</span><span class="sxs-lookup"><span data-stu-id="d52be-114">Always call `UseAuthentication` before `UseSignalR` so that SignalR has a user on the `HttpContext`.</span></span>

::: moniker-end

### <a name="cookie-authentication"></a><span data-ttu-id="d52be-115">Tanımlama bilgisi kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="d52be-115">Cookie authentication</span></span>

<span data-ttu-id="d52be-116">Tarayıcı tabanlı bir uygulamada, tanımlama bilgisi kimlik doğrulaması, mevcut kullanıcı kimlik bilgilerinizin SignalR bağlantılarına otomatik olarak akmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d52be-116">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="d52be-117">Tarayıcı istemcisi kullanılırken ek yapılandırma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d52be-117">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="d52be-118">Kullanıcı uygulamanızda oturum açtıysa, SignalR bağlantı bu kimlik doğrulamasını otomatik olarak devralır.</span><span class="sxs-lookup"><span data-stu-id="d52be-118">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="d52be-119">Tanımlama bilgileri, erişim belirteçleri göndermek için tarayıcıya özgü bir yoldur, ancak tarayıcı olmayan istemciler bunları gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-119">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="d52be-120">[.NET istemcisi](xref:signalr/dotnet-client)kullanılırken, tanımlama bilgisi sağlamak için `.WithUrl` çağrısında `Cookies` özelliği yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-120">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call to provide a cookie.</span></span> <span data-ttu-id="d52be-121">Ancak, .NET istemcisinden tanımlama bilgisi kimlik doğrulamasını kullanmak, uygulamanın bir tanımlama bilgisi için kimlik doğrulama verilerini Exchange için bir API sağlamasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d52be-121">However, using cookie authentication from the .NET client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="d52be-122">Taşıyıcı belirteç kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="d52be-122">Bearer token authentication</span></span>

<span data-ttu-id="d52be-123">İstemci, tanımlama bilgisi kullanmak yerine bir erişim belirteci sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-123">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="d52be-124">Sunucu belirteci doğrular ve kullanıcıyı tanımlamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="d52be-124">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="d52be-125">Bu doğrulama yalnızca bağlantı kurulduunda yapılır.</span><span class="sxs-lookup"><span data-stu-id="d52be-125">This validation is done only when the connection is established.</span></span> <span data-ttu-id="d52be-126">Bağlantının kullanım ömrü boyunca, sunucu otomatik olarak yeniden doğrulamadan belirteç iptalini kontrol etmez.</span><span class="sxs-lookup"><span data-stu-id="d52be-126">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="d52be-127">Sunucusunda, taşıyıcı belirteç kimlik doğrulaması [JWT taşıyıcı ara yazılımı](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d52be-127">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="d52be-128">JavaScript istemcisinde, belirteç [Accesstokenfactory](xref:signalr/configuration#configure-bearer-authentication) seçeneği kullanılarak sağlanabilirler.</span><span class="sxs-lookup"><span data-stu-id="d52be-128">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=52-55)]

<span data-ttu-id="d52be-129">.NET istemcisinde, belirteci yapılandırmak için kullanılabilecek, benzer bir [Accesstokenprovider](xref:signalr/configuration#configure-bearer-authentication) özelliği vardır:</span><span class="sxs-lookup"><span data-stu-id="d52be-129">In the .NET client, there's a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="d52be-130">Sağladığınız erişim belirteci işlevi, SignalRtarafından yapılan **her** http isteğinin önüne çağırılır.</span><span class="sxs-lookup"><span data-stu-id="d52be-130">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="d52be-131">Bağlantıyı etkin tutmak için belirteci yenilemeniz gerekiyorsa (bağlantı sırasında süresi dolarsa), bunu bu işlevin içinden yapın ve güncelleştirilmiş belirteci döndürün.</span><span class="sxs-lookup"><span data-stu-id="d52be-131">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="d52be-132">Standart Web API 'Lerinde, taşıyıcı belirteçleri bir HTTP üst bilgisinde gönderilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-132">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="d52be-133">Ancak, bazı aktarımlar kullanılırken SignalR tarayıcılarda bu üst bilgileri ayarlayamadı.</span><span class="sxs-lookup"><span data-stu-id="d52be-133">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="d52be-134">WebSockets ve sunucu tarafından gönderilen olaylar kullanılırken, belirteç bir sorgu dizesi parametresi olarak iletilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-134">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="d52be-135">Bunu sunucuda desteklemek için ek yapılandırma gerekir:</span><span class="sxs-lookup"><span data-stu-id="d52be-135">To support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

> [!NOTE]
> <span data-ttu-id="d52be-136">Sorgu dizesi tarayıcı API 'SI sınırlamaları nedeniyle WebSockets ve sunucu tarafından gönderilen olaylarla bağlantı kurulurken tarayıcılarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d52be-136">The query string is used on browsers when connecting with WebSockets and Server-Sent Events due to browser API limitations.</span></span> <span data-ttu-id="d52be-137">HTTPS kullanılırken sorgu dizesi değerleri TLS bağlantısıyla korunmuş hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-137">When using HTTPS, query string values are secured by the TLS connection.</span></span> <span data-ttu-id="d52be-138">Ancak, birçok sunucu günlük sorgu dizesi değerlerini günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="d52be-138">However, many servers log query string values.</span></span> <span data-ttu-id="d52be-139">Daha fazla bilgi için [ASP.NET Core SignalRgüvenlik konuları ](xref:signalr/security)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="d52be-139">For more information, see [Security considerations in ASP.NET Core SignalR](xref:signalr/security).</span></span> SignalR<span data-ttu-id="d52be-140">, belirteçleri destekleyen ortamlarda (.NET ve Java istemcileri gibi) belirteçleri iletmek için üst bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="d52be-140"> uses headers to transmit tokens in environments which support them (such as the .NET and Java clients).</span></span>

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="d52be-141">Tanımlama bilgileri ve taşıyıcı belirteçleri karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="d52be-141">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="d52be-142">Tanımlama bilgileri tarayıcılara özeldir.</span><span class="sxs-lookup"><span data-stu-id="d52be-142">Cookies are specific to browsers.</span></span> <span data-ttu-id="d52be-143">Diğer istemci türlerinden gönderilmesi, taşıyıcı belirteçlerinin gönderilmesine kıyasla karmaşıklık ekler.</span><span class="sxs-lookup"><span data-stu-id="d52be-143">Sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="d52be-144">Sonuç olarak, uygulamanın yalnızca tarayıcı istemcisinden kullanıcıların kimliğini doğrulaması gerekmediği takdirde tanımlama bilgisi kimlik doğrulaması önerilmez.</span><span class="sxs-lookup"><span data-stu-id="d52be-144">Consequently, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="d52be-145">Taşıyıcı belirteç kimlik doğrulaması, tarayıcı istemcisi dışındaki istemcileri kullanırken önerilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="d52be-145">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="d52be-146">Windows kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="d52be-146">Windows authentication</span></span>

<span data-ttu-id="d52be-147">Uygulamanızda [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) yapılandırılırsa, SignalR hub 'ları güvenli hale getirmek için bu kimliği kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-147">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="d52be-148">Ancak, bireysel kullanıcılara ileti göndermek için özel bir kullanıcı KIMLIĞI sağlayıcısı eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d52be-148">However, to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="d52be-149">Windows kimlik doğrulama sistemi, "ad tanımlayıcı" talebi sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="d52be-149">The Windows authentication system doesn't provide the "Name Identifier" claim.</span></span> SignalR<span data-ttu-id="d52be-150">, Kullanıcı adını belirlemede talebi kullanır.</span><span class="sxs-lookup"><span data-stu-id="d52be-150"> uses the claim to determine the user name.</span></span>

<span data-ttu-id="d52be-151">`IUserIdProvider` uygulayan yeni bir sınıf ekleyin ve Kullanıcı tarafından tanımlayıcı olarak kullanılacak taleplerden birini alın.</span><span class="sxs-lookup"><span data-stu-id="d52be-151">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="d52be-152">Örneğin, "ad" talebini (`[Domain]\[Username]`biçimde Windows Kullanıcı adı) kullanmak için aşağıdaki sınıfı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d52be-152">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="d52be-153">`ClaimTypes.Name`yerine, `User` herhangi bir değeri (Windows SID tanımlayıcısı gibi) kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d52be-153">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, and so on).</span></span>

> [!NOTE]
> <span data-ttu-id="d52be-154">Seçtiğiniz değer, sisteminizdeki tüm kullanıcılar arasında benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d52be-154">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="d52be-155">Aksi takdirde, bir kullanıcı için tasarlanan bir ileti farklı bir kullanıcıya gidiyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-155">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="d52be-156">Bu bileşeni `Startup.ConfigureServices` yöntemine kaydedin.</span><span class="sxs-lookup"><span data-stu-id="d52be-156">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="d52be-157">.NET Istemcisinde, [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) özelliği ayarlanarak Windows kimlik doğrulamasının etkinleştirilmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="d52be-157">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="d52be-158">Windows kimlik doğrulaması yalnızca Microsoft Internet Explorer veya Microsoft Edge kullanılırken tarayıcı istemcisi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="d52be-158">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="d52be-159">Kimlik işlemeyi özelleştirmek için talepler kullanma</span><span class="sxs-lookup"><span data-stu-id="d52be-159">Use claims to customize identity handling</span></span>

<span data-ttu-id="d52be-160">Kullanıcıların kimliğini doğrulayan bir uygulama, Kullanıcı taleplerinden SignalR Kullanıcı kimliği türetilebilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-160">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="d52be-161">SignalR Kullanıcı kimliklerini nasıl oluşturduğunu belirtmek için, `IUserIdProvider` uygulayın ve uygulamayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="d52be-161">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="d52be-162">Örnek kod, tanımlama özelliği olarak kullanıcının e-posta adresini seçmek için talepleri nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="d52be-162">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="d52be-163">Seçtiğiniz değer, sisteminizdeki tüm kullanıcılar arasında benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d52be-163">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="d52be-164">Aksi takdirde, bir kullanıcı için tasarlanan bir ileti farklı bir kullanıcıya gidiyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-164">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="d52be-165">Hesap kaydı, ASP.NET Identity veritabanına `ClaimsTypes.Email` türünde bir talep ekler.</span><span class="sxs-lookup"><span data-stu-id="d52be-165">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="d52be-166">Bu bileşeni `Startup.ConfigureServices`kaydedin.</span><span class="sxs-lookup"><span data-stu-id="d52be-166">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="d52be-167">Kullanıcılara, hub 'lara ve hub yöntemlerine erişim yetkisi verme</span><span class="sxs-lookup"><span data-stu-id="d52be-167">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="d52be-168">Varsayılan olarak, bir hub 'daki tüm yöntemler kimliği doğrulanmamış bir kullanıcı tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-168">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="d52be-169">Kimlik doğrulaması gerektirmek için, [Yetkilendir](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) özniteliğini hub 'a uygulayın:</span><span class="sxs-lookup"><span data-stu-id="d52be-169">To require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="d52be-170">Yalnızca belirli [Yetkilendirme ilkeleriyle](xref:security/authorization/policies)eşleşen kullanıcılara erişimi kısıtlamak için `[Authorize]` özniteliğinin Oluşturucu bağımsız değişkenlerini ve özelliklerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d52be-170">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="d52be-171">Örneğin, `MyAuthorizationPolicy` adlı bir özel yetkilendirme ilkeniz varsa, aşağıdaki kodu kullanarak yalnızca bu ilkeyle eşleşen kullanıcıların hub 'a erişmesini sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d52be-171">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub : Hub
{
}
```

<span data-ttu-id="d52be-172">Tek tek hub yöntemlerinde `[Authorize]` özniteliği de uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-172">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="d52be-173">Geçerli Kullanıcı, yönteme uygulanan ilkeyle eşleşmezse, çağırana bir hata döndürülür:</span><span class="sxs-lookup"><span data-stu-id="d52be-173">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
{
    public async Task Send(string message)
    {
        // ... send a message to all users ...
    }

    [Authorize("Administrators")]
    public void BanUser(string userName)
    {
        // ... ban a user from the chat room (something only Administrators can do) ...
    }
}
```

::: moniker range=">= aspnetcore-3.0"

### <a name="use-authorization-handlers-to-customize-hub-method-authorization"></a><span data-ttu-id="d52be-174">Hub yöntemi yetkilendirmesini özelleştirmek için yetkilendirme işleyicilerini kullanma</span><span class="sxs-lookup"><span data-stu-id="d52be-174">Use authorization handlers to customize hub method authorization</span></span>

SignalR<span data-ttu-id="d52be-175">, bir hub yöntemi yetkilendirme gerektirdiğinde, yetkilendirme işleyicilerine özel bir kaynak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d52be-175"> provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="d52be-176">Kaynak bir `HubInvocationContext`örneğidir.</span><span class="sxs-lookup"><span data-stu-id="d52be-176">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="d52be-177">`HubInvocationContext`, `HubCallerContext`, çağrılan hub yönteminin adını ve hub yönteminin bağımsız değişkenlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="d52be-177">The `HubInvocationContext` includes the `HubCallerContext`, the name of the hub method being invoked, and the arguments to the hub method.</span></span>

<span data-ttu-id="d52be-178">Azure Active Directory aracılığıyla birden çok kuruluşun oturum açmasına izin veren bir sohbet odası örneğini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="d52be-178">Consider the example of a chat room allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="d52be-179">Microsoft hesabı olan herkes sohbet için oturum açabilir, ancak yalnızca sahip olunan kuruluşun üyeleri kullanıcıları yasaklatabilecek veya kullanıcıların sohbet geçmişlerini görüntüleyebilmelidir.</span><span class="sxs-lookup"><span data-stu-id="d52be-179">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization should be able to ban users or view users' chat histories.</span></span> <span data-ttu-id="d52be-180">Ayrıca, belirli kullanıcılardan belirli işlevleri kısıtlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d52be-180">Furthermore, we might want to restrict certain functionality from certain users.</span></span> <span data-ttu-id="d52be-181">ASP.NET Core 3,0 ' deki güncelleştirilmiş özellikleri kullanarak bu tamamen mümkündür.</span><span class="sxs-lookup"><span data-stu-id="d52be-181">Using the updated features in ASP.NET Core 3.0, this is entirely possible.</span></span> <span data-ttu-id="d52be-182">`DomainRestrictedRequirement` nasıl özel bir `IAuthorizationRequirement`görevi gördüğüne göz önünde edin.</span><span class="sxs-lookup"><span data-stu-id="d52be-182">Note how the `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="d52be-183">Artık `HubInvocationContext` kaynak parametresi geçirildiğinden, iç mantık hub 'ın çağrıldığı bağlamı inceleyebilir ve kullanıcının bireysel hub yöntemlerini yürütmesine izin verirken kararlar verebilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-183">Now that the `HubInvocationContext` resource parameter is being passed in, the internal logic can inspect the context in which the Hub is being called and make decisions on allowing the user to execute individual Hub methods.</span></span>

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

public class DomainRestrictedRequirement : 
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>, 
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement, 
        HubInvocationContext resource)
    {
        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name) && 
            context.User.Identity.Name.EndsWith("@microsoft.com"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName,
        string currentUsername)
    {
        return !(currentUsername.Equals("asdf42@microsoft.com") && 
            hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase));
    }
}
```

<span data-ttu-id="d52be-184">`Startup.ConfigureServices`, yeni ilkeyi ekleyerek `DomainRestricted` ilkesini oluşturmak için parametre olarak özel `DomainRestrictedRequirement` gereksinimini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d52be-184">In `Startup.ConfigureServices`, add the new policy, providing the custom `DomainRestrictedRequirement` requirement as a parameter to create the `DomainRestricted` policy.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services
        .AddAuthorization(options =>
        {
            options.AddPolicy("DomainRestricted", policy =>
            {
                policy.Requirements.Add(new DomainRestrictedRequirement());
            });
        });
}
```

<span data-ttu-id="d52be-185">Yukarıdaki örnekte `DomainRestrictedRequirement` sınıfı, bu gereksinim için hem `IAuthorizationRequirement` hem de kendi `AuthorizationHandler`.</span><span class="sxs-lookup"><span data-stu-id="d52be-185">In the preceding example, the `DomainRestrictedRequirement` class is both an `IAuthorizationRequirement` and its own `AuthorizationHandler` for that requirement.</span></span> <span data-ttu-id="d52be-186">Bu iki bileşeni birbirinden ayrı ayrı sınıflara bölmek kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="d52be-186">It's acceptable to split these two components into separate classes to separate concerns.</span></span> <span data-ttu-id="d52be-187">Örneğin yaklaşımın bir avantajı, gereksinim ve işleyicinin aynı şey olduğu için başlangıç sırasında `AuthorizationHandler` eklemeye gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="d52be-187">A benefit of the example's approach is there's no need to inject the `AuthorizationHandler` during startup, as the requirement and the handler are the same thing.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d52be-188">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d52be-188">Additional resources</span></span>

* [<span data-ttu-id="d52be-189">ASP.NET Core 'de taşıyıcı belirteç kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="d52be-189">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="d52be-190">Kaynak tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="d52be-190">Resource-based Authorization</span></span>](xref:security/authorization/resourcebased)
