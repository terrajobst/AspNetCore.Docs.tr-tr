---
title: ASP.NET Core SignalR 'de kimlik doğrulaması ve yetkilendirme
author: bradygaster
description: ASP.NET Core SignalR 'de kimlik doğrulama ve yetkilendirmeyi nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 07/15/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: e7e7a9fd537ba89b64c15594652a290357a00038
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412536"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="4c93f-103">ASP.NET Core SignalR 'de kimlik doğrulaması ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="4c93f-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="4c93f-104">, [Andrew Stanton-nurte](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="4c93f-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="4c93f-105">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(indirme)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="4c93f-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="4c93f-106">Bir SignalR hub 'ına bağlanan kullanıcıların kimliğini doğrulama</span><span class="sxs-lookup"><span data-stu-id="4c93f-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="4c93f-107">SignalR, bir kullanıcıyı her bağlantıyla ilişkilendirmek için [ASP.NET Core kimlik doğrulamasıyla](xref:security/authentication/identity) birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-107">SignalR can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="4c93f-108">Bir hub 'da, kimlik doğrulama verilerine [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) özelliğinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="4c93f-109">Kimlik doğrulaması, hub 'ın kullanıcıyla ilişkili tüm bağlantılar üzerinde Yöntemler çağırmasını sağlar (daha fazla bilgi için bkz. [SignalR 'de kullanıcıları ve grupları yönetme](xref:signalr/groups) ).</span><span class="sxs-lookup"><span data-stu-id="4c93f-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="4c93f-110">Birden çok bağlantı tek bir kullanıcıyla ilişkilendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-110">Multiple connections may be associated with a single user.</span></span>

<span data-ttu-id="4c93f-111">Aşağıda, SignalR ve ASP.NET Core `Startup.Configure` kimlik doğrulaması kullanan bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c93f-111">The following is an example of `Startup.Configure` which uses SignalR and ASP.NET Core authentication:</span></span>

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
> <span data-ttu-id="4c93f-112">SignalR ve ASP.NET Core kimlik doğrulama ara yazılımını kaydetme sırası önemli.</span><span class="sxs-lookup"><span data-stu-id="4c93f-112">The order in which you register the SignalR and ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="4c93f-113">SignalR `UseAuthentication` 'nin `UseSignalR` üzerinde bir kullanıcısı olması için `HttpContext`önce her zaman ' i çağırın.</span><span class="sxs-lookup"><span data-stu-id="4c93f-113">Always call `UseAuthentication` before `UseSignalR` so that SignalR has a user on the `HttpContext`.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="4c93f-114">Tanımlama bilgisi kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="4c93f-114">Cookie authentication</span></span>

<span data-ttu-id="4c93f-115">Tarayıcı tabanlı bir uygulamada, tanımlama bilgisi kimlik doğrulaması, mevcut kullanıcı kimlik bilgilerinizin SignalR bağlantılarına otomatik olarak akmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c93f-115">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="4c93f-116">Tarayıcı istemcisi kullanılırken ek yapılandırma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4c93f-116">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="4c93f-117">Kullanıcı uygulamanızda oturum açtıysa, SignalR bağlantısı bu kimlik doğrulamasını otomatik olarak devralır.</span><span class="sxs-lookup"><span data-stu-id="4c93f-117">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="4c93f-118">Tanımlama bilgileri, erişim belirteçleri göndermek için tarayıcıya özgü bir yoldur, ancak tarayıcı olmayan istemciler bunları gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-118">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="4c93f-119">[.NET istemcisi](xref:signalr/dotnet-client)kullanılırken, `Cookies` özelliği bir tanımlama bilgisi sağlamak için `.WithUrl` çağrıda yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-119">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="4c93f-120">Ancak, .NET Istemcisinden tanımlama bilgisi kimlik doğrulamasını kullanmak, uygulamanın bir tanımlama bilgisi için kimlik doğrulama verilerini Exchange için bir API sağlamasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-120">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="4c93f-121">Taşıyıcı belirteç kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="4c93f-121">Bearer token authentication</span></span>

<span data-ttu-id="4c93f-122">İstemci, tanımlama bilgisi kullanmak yerine bir erişim belirteci sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-122">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="4c93f-123">Sunucu belirteci doğrular ve kullanıcıyı tanımlamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="4c93f-123">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="4c93f-124">Bu doğrulama yalnızca bağlantı kurulduunda yapılır.</span><span class="sxs-lookup"><span data-stu-id="4c93f-124">This validation is done only when the connection is established.</span></span> <span data-ttu-id="4c93f-125">Bağlantının kullanım ömrü boyunca, sunucu otomatik olarak yeniden doğrulamadan belirteç iptalini kontrol etmez.</span><span class="sxs-lookup"><span data-stu-id="4c93f-125">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="4c93f-126">Sunucusunda, taşıyıcı belirteç kimlik doğrulaması [JWT taşıyıcı ara yazılımı](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="4c93f-126">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="4c93f-127">JavaScript istemcisinde, belirteç [Accesstokenfactory](xref:signalr/configuration#configure-bearer-authentication) seçeneği kullanılarak sağlanabilirler.</span><span class="sxs-lookup"><span data-stu-id="4c93f-127">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="4c93f-128">.NET istemcisinde, belirteci yapılandırmak için kullanılabilecek benzer bir [Accesstokenprovider](xref:signalr/configuration#configure-bearer-authentication) özelliği vardır:</span><span class="sxs-lookup"><span data-stu-id="4c93f-128">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="4c93f-129">Sağladığınız erişim belirteci işlevi, SignalR tarafından yapılan **her** http isteğinden önce çağırılır.</span><span class="sxs-lookup"><span data-stu-id="4c93f-129">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="4c93f-130">Bağlantıyı etkin tutmak için belirteci yenilemeniz gerekiyorsa (bağlantı sırasında süresi dolarsa), bunu bu işlevin içinden yapın ve güncelleştirilmiş belirteci döndürün.</span><span class="sxs-lookup"><span data-stu-id="4c93f-130">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="4c93f-131">Standart Web API 'Lerinde, taşıyıcı belirteçleri bir HTTP üst bilgisinde gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-131">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="4c93f-132">Ancak, SignalR bazı aktarımlar kullanılırken tarayıcılarda bu üst bilgileri kuramıyor.</span><span class="sxs-lookup"><span data-stu-id="4c93f-132">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="4c93f-133">WebSockets ve sunucu tarafından gönderilen olaylar kullanılırken, belirteç bir sorgu dizesi parametresi olarak iletilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-133">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="4c93f-134">Bunu sunucuda desteklemek için ek yapılandırma gerekir:</span><span class="sxs-lookup"><span data-stu-id="4c93f-134">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="4c93f-135">Tanımlama bilgileri ve taşıyıcı belirteçleri karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="4c93f-135">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="4c93f-136">Tanımlama bilgileri tarayıcılara özgü olduğundan, bunları diğer istemci türlerinden göndermek, taşıyıcı belirteçleri göndermeye kıyasla karmaşıklık ekler.</span><span class="sxs-lookup"><span data-stu-id="4c93f-136">Because cookies are specific to browsers, sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="4c93f-137">Bu nedenle, uygulamanın yalnızca tarayıcı istemcisinden kullanıcıların kimliğini doğrulaması gerekmediği takdirde tanımlama bilgisi kimlik doğrulaması önerilmez.</span><span class="sxs-lookup"><span data-stu-id="4c93f-137">For this reason, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="4c93f-138">Taşıyıcı belirteç kimlik doğrulaması, tarayıcı istemcisi dışındaki istemcileri kullanırken önerilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="4c93f-138">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="4c93f-139">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="4c93f-139">Windows authentication</span></span>

<span data-ttu-id="4c93f-140">Uygulamanızda [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) yapılandırıldıysa, SignalR hub 'ları güvenli hale getirmek için bu kimliği kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-140">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="4c93f-141">Ancak, bireysel kullanıcılara ileti göndermek için özel bir kullanıcı KIMLIĞI sağlayıcısı eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-141">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="4c93f-142">Bunun nedeni, Windows kimlik doğrulama sisteminin kullanıcı adını belirlemede kullandığı "ad tanımlayıcı" talebini sağlamasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c93f-142">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="4c93f-143">Kullanıcı tarafından tanımlayıcı olarak kullanılacak taleplerden birini uygulayan `IUserIdProvider` ve alan yeni bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4c93f-143">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="4c93f-144">Örneğin, "ad" talebini (formdaki `[Domain]\[Username]`Windows Kullanıcı adı) kullanmak için aşağıdaki sınıfı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="4c93f-144">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="4c93f-145">Yerine, `User` ' den herhangi bir değer kullanabilirsiniz (örneğin, Windows SID tanımlayıcısı, vb.). `ClaimTypes.Name`</span><span class="sxs-lookup"><span data-stu-id="4c93f-145">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="4c93f-146">Seçtiğiniz değer, sisteminizdeki tüm kullanıcılar arasında benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c93f-146">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="4c93f-147">Aksi takdirde, bir kullanıcı için tasarlanan bir ileti farklı bir kullanıcıya gidiyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-147">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="4c93f-148">Bu bileşeni `Startup.ConfigureServices` yönteminizin içine kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4c93f-148">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="4c93f-149">.NET Istemcisinde, [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) özelliği ayarlanarak Windows kimlik doğrulamasının etkinleştirilmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="4c93f-149">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="4c93f-150">Windows kimlik doğrulaması yalnızca Microsoft Internet Explorer veya Microsoft Edge kullanılırken tarayıcı istemcisi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-150">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="4c93f-151">Kimlik işlemeyi özelleştirmek için talepler kullanma</span><span class="sxs-lookup"><span data-stu-id="4c93f-151">Use claims to customize identity handling</span></span>

<span data-ttu-id="4c93f-152">Kullanıcıların kimliğini doğrulayan bir uygulama, Kullanıcı taleplerinden bir SignalR Kullanıcı kimliği türetilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-152">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="4c93f-153">SignalR 'nin Kullanıcı kimlikleri nasıl oluşturduğunu belirtmek için, `IUserIdProvider` uygulamayı uygulayın ve kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4c93f-153">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="4c93f-154">Örnek kod, tanımlama özelliği olarak kullanıcının e-posta adresini seçmek için talepleri nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-154">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="4c93f-155">Seçtiğiniz değer, sisteminizdeki tüm kullanıcılar arasında benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c93f-155">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="4c93f-156">Aksi takdirde, bir kullanıcı için tasarlanan bir ileti farklı bir kullanıcıya gidiyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-156">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="4c93f-157">Hesap kaydı, ASP.NET Identity veritabanına türünde `ClaimsTypes.Email` bir talep ekler.</span><span class="sxs-lookup"><span data-stu-id="4c93f-157">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="4c93f-158">Bu bileşeni ' `Startup.ConfigureServices`ye kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4c93f-158">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="4c93f-159">Kullanıcılara, hub 'lara ve hub yöntemlerine erişim yetkisi verme</span><span class="sxs-lookup"><span data-stu-id="4c93f-159">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="4c93f-160">Varsayılan olarak, bir hub 'daki tüm yöntemler kimliği doğrulanmamış bir kullanıcı tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-160">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="4c93f-161">Kimlik doğrulaması gerektirmek için, [Yetkilendir](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) özniteliğini hub 'a uygulayın:</span><span class="sxs-lookup"><span data-stu-id="4c93f-161">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="4c93f-162">Yalnızca belirli `[Authorize]` [Yetkilendirme ilkeleriyle](xref:security/authorization/policies)eşleşen kullanıcılara erişimi kısıtlamak için özniteliğinin Oluşturucu bağımsız değişkenlerini ve özelliklerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c93f-162">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="4c93f-163">Örneğin, adlı `MyAuthorizationPolicy` bir özel yetkilendirme ilkeniz varsa, aşağıdaki kodu kullanarak yalnızca bu ilkeyle eşleşen kullanıcıların hub 'a erişmesini sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4c93f-163">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub : Hub
{
}
```

<span data-ttu-id="4c93f-164">Tek bir hub yöntemi `[Authorize]` özniteliğe de uygulanmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-164">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="4c93f-165">Geçerli Kullanıcı, yönteme uygulanan ilkeyle eşleşmezse, çağırana bir hata döndürülür:</span><span class="sxs-lookup"><span data-stu-id="4c93f-165">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

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

### <a name="use-authorization-handlers-to-customize-hub-method-authorization"></a><span data-ttu-id="4c93f-166">Hub yöntemi yetkilendirmesini özelleştirmek için yetkilendirme işleyicilerini kullanma</span><span class="sxs-lookup"><span data-stu-id="4c93f-166">Use authorization handlers to customize hub method authorization</span></span>

<span data-ttu-id="4c93f-167">SignalR, bir hub yöntemi yetkilendirme gerektirdiğinde, yetkilendirme işleyicilerine özel bir kaynak sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c93f-167">SignalR provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="4c93f-168">Kaynak bir örneğidir `HubInvocationContext`.</span><span class="sxs-lookup"><span data-stu-id="4c93f-168">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="4c93f-169">, Çağrılan hub `HubCallerContext`yönteminin adını ve hub yönteminin bağımsız değişkenlerini içerir.`HubInvocationContext`</span><span class="sxs-lookup"><span data-stu-id="4c93f-169">The `HubInvocationContext` includes the `HubCallerContext`, the name of the hub method being invoked, and the arguments to the hub method.</span></span>

<span data-ttu-id="4c93f-170">Azure Active Directory aracılığıyla birden çok kuruluşun oturum açmasına izin veren bir sohbet odası örneğini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="4c93f-170">Consider the example of a chat room allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="4c93f-171">Microsoft hesabı olan herkes sohbet için oturum açabilir, ancak yalnızca sahip olunan kuruluşun üyeleri kullanıcıları yasaklatabilecek veya kullanıcıların sohbet geçmişlerini görüntüleyebilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-171">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization should be able to ban users or view users' chat histories.</span></span> <span data-ttu-id="4c93f-172">Ayrıca, belirli kullanıcılardan belirli işlevleri kısıtlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c93f-172">Furthermore, we might want to restrict certain functionality from certain users.</span></span> <span data-ttu-id="4c93f-173">ASP.NET Core 3,0 ' deki güncelleştirilmiş özellikleri kullanarak bu tamamen mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4c93f-173">Using the updated features in ASP.NET Core 3.0, this is entirely possible.</span></span> <span data-ttu-id="4c93f-174">Nasıl `DomainRestrictedRequirement` özel`IAuthorizationRequirement`olarak işlev gördüğüne göz önünde kalın.</span><span class="sxs-lookup"><span data-stu-id="4c93f-174">Note how the `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="4c93f-175">`HubInvocationContext` Kaynak parametresi geçirilmeye hazır olduğuna göre, iç mantık hub 'ın çağrıldığı bağlamı inceleyebilir ve kullanıcının bireysel hub yöntemlerini yürütmesine izin verirken kararlar verebilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-175">Now that the `HubInvocationContext` resource parameter is being passed in, the internal logic can inspect the context in which the Hub is being called and make decisions on allowing the user to execute individual Hub methods.</span></span>

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

<span data-ttu-id="4c93f-176">İçinde `Startup.ConfigureServices`, `DomainRestricted` ilkeyi oluşturmak için özel `DomainRestrictedRequirement` gereksinimi bir parametre olarak sağlayarak yeni ilkeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4c93f-176">In `Startup.ConfigureServices`, add the new policy, providing the custom `DomainRestrictedRequirement` requirement as a parameter to create the `DomainRestricted` policy.</span></span>

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

<span data-ttu-id="4c93f-177">Yukarıdaki örnekte, `DomainRestrictedRequirement` sınıfı, bu gereksinim için hem bir `IAuthorizationRequirement` ve kendisi `AuthorizationHandler` olur.</span><span class="sxs-lookup"><span data-stu-id="4c93f-177">In the preceding example, the `DomainRestrictedRequirement` class is both an `IAuthorizationRequirement` and its own `AuthorizationHandler` for that requirement.</span></span> <span data-ttu-id="4c93f-178">Bu iki bileşeni birbirinden ayrı ayrı sınıflara bölmek kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c93f-178">It's acceptable to split these two components into separate classes to separate concerns.</span></span> <span data-ttu-id="4c93f-179">Örneğin yaklaşımın bir avantajı, gereksinim ve işleyicinin aynı şey olduğu için başlangıç `AuthorizationHandler` sırasında ekleme zorunluluktur.</span><span class="sxs-lookup"><span data-stu-id="4c93f-179">A benefit of the example's approach is there's no need to inject the `AuthorizationHandler` during startup, as the requirement and the handler are the same thing.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4c93f-180">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4c93f-180">Additional resources</span></span>

* [<span data-ttu-id="4c93f-181">ASP.NET Core 'de taşıyıcı belirteç kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="4c93f-181">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="4c93f-182">Kaynak tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="4c93f-182">Resource-based Authorization</span></span>](xref:security/authorization/resourcebased)
