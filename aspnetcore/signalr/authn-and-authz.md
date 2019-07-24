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
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a>ASP.NET Core SignalR 'de kimlik doğrulaması ve yetkilendirme

, [Andrew Stanton-nurte](https://twitter.com/anurse)

[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(indirme)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a>Bir SignalR hub 'ına bağlanan kullanıcıların kimliğini doğrulama

SignalR, bir kullanıcıyı her bağlantıyla ilişkilendirmek için [ASP.NET Core kimlik doğrulamasıyla](xref:security/authentication/identity) birlikte kullanılabilir. Bir hub 'da, kimlik doğrulama verilerine [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) özelliğinden erişilebilir. Kimlik doğrulaması, hub 'ın kullanıcıyla ilişkili tüm bağlantılar üzerinde Yöntemler çağırmasını sağlar (daha fazla bilgi için bkz. [SignalR 'de kullanıcıları ve grupları yönetme](xref:signalr/groups) ). Birden çok bağlantı tek bir kullanıcıyla ilişkilendirilebilir.

Aşağıda, SignalR ve ASP.NET Core `Startup.Configure` kimlik doğrulaması kullanan bir örnek verilmiştir:

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
> SignalR ve ASP.NET Core kimlik doğrulama ara yazılımını kaydetme sırası önemli. SignalR `UseAuthentication` 'nin `UseSignalR` üzerinde bir kullanıcısı olması için `HttpContext`önce her zaman ' i çağırın.

### <a name="cookie-authentication"></a>Tanımlama bilgisi kimlik doğrulaması

Tarayıcı tabanlı bir uygulamada, tanımlama bilgisi kimlik doğrulaması, mevcut kullanıcı kimlik bilgilerinizin SignalR bağlantılarına otomatik olarak akmasını sağlar. Tarayıcı istemcisi kullanılırken ek yapılandırma gerekmez. Kullanıcı uygulamanızda oturum açtıysa, SignalR bağlantısı bu kimlik doğrulamasını otomatik olarak devralır.

Tanımlama bilgileri, erişim belirteçleri göndermek için tarayıcıya özgü bir yoldur, ancak tarayıcı olmayan istemciler bunları gönderebilir. [.NET istemcisi](xref:signalr/dotnet-client)kullanılırken, `Cookies` özelliği bir tanımlama bilgisi sağlamak için `.WithUrl` çağrıda yapılandırılabilir. Ancak, .NET Istemcisinden tanımlama bilgisi kimlik doğrulamasını kullanmak, uygulamanın bir tanımlama bilgisi için kimlik doğrulama verilerini Exchange için bir API sağlamasını gerektirir.

### <a name="bearer-token-authentication"></a>Taşıyıcı belirteç kimlik doğrulaması

İstemci, tanımlama bilgisi kullanmak yerine bir erişim belirteci sağlayabilir. Sunucu belirteci doğrular ve kullanıcıyı tanımlamak için kullanır. Bu doğrulama yalnızca bağlantı kurulduunda yapılır. Bağlantının kullanım ömrü boyunca, sunucu otomatik olarak yeniden doğrulamadan belirteç iptalini kontrol etmez.

Sunucusunda, taşıyıcı belirteç kimlik doğrulaması [JWT taşıyıcı ara yazılımı](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)kullanılarak yapılandırılır.

JavaScript istemcisinde, belirteç [Accesstokenfactory](xref:signalr/configuration#configure-bearer-authentication) seçeneği kullanılarak sağlanabilirler.

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

.NET istemcisinde, belirteci yapılandırmak için kullanılabilecek benzer bir [Accesstokenprovider](xref:signalr/configuration#configure-bearer-authentication) özelliği vardır:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> Sağladığınız erişim belirteci işlevi, SignalR tarafından yapılan **her** http isteğinden önce çağırılır. Bağlantıyı etkin tutmak için belirteci yenilemeniz gerekiyorsa (bağlantı sırasında süresi dolarsa), bunu bu işlevin içinden yapın ve güncelleştirilmiş belirteci döndürün.

Standart Web API 'Lerinde, taşıyıcı belirteçleri bir HTTP üst bilgisinde gönderilir. Ancak, SignalR bazı aktarımlar kullanılırken tarayıcılarda bu üst bilgileri kuramıyor. WebSockets ve sunucu tarafından gönderilen olaylar kullanılırken, belirteç bir sorgu dizesi parametresi olarak iletilir. Bunu sunucuda desteklemek için ek yapılandırma gerekir:

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a>Tanımlama bilgileri ve taşıyıcı belirteçleri karşılaştırması 

Tanımlama bilgileri tarayıcılara özgü olduğundan, bunları diğer istemci türlerinden göndermek, taşıyıcı belirteçleri göndermeye kıyasla karmaşıklık ekler. Bu nedenle, uygulamanın yalnızca tarayıcı istemcisinden kullanıcıların kimliğini doğrulaması gerekmediği takdirde tanımlama bilgisi kimlik doğrulaması önerilmez. Taşıyıcı belirteç kimlik doğrulaması, tarayıcı istemcisi dışındaki istemcileri kullanırken önerilen yaklaşımdır.

### <a name="windows-authentication"></a>Windows kimlik doğrulaması

Uygulamanızda [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) yapılandırıldıysa, SignalR hub 'ları güvenli hale getirmek için bu kimliği kullanabilir. Ancak, bireysel kullanıcılara ileti göndermek için özel bir kullanıcı KIMLIĞI sağlayıcısı eklemeniz gerekir. Bunun nedeni, Windows kimlik doğrulama sisteminin kullanıcı adını belirlemede kullandığı "ad tanımlayıcı" talebini sağlamasıdır.

Kullanıcı tarafından tanımlayıcı olarak kullanılacak taleplerden birini uygulayan `IUserIdProvider` ve alan yeni bir sınıf ekleyin. Örneğin, "ad" talebini (formdaki `[Domain]\[Username]`Windows Kullanıcı adı) kullanmak için aşağıdaki sınıfı oluşturun:

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

Yerine, `User` ' den herhangi bir değer kullanabilirsiniz (örneğin, Windows SID tanımlayıcısı, vb.). `ClaimTypes.Name`

> [!NOTE]
> Seçtiğiniz değer, sisteminizdeki tüm kullanıcılar arasında benzersiz olmalıdır. Aksi takdirde, bir kullanıcı için tasarlanan bir ileti farklı bir kullanıcıya gidiyor olabilir.

Bu bileşeni `Startup.ConfigureServices` yönteminizin içine kaydedin.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

.NET Istemcisinde, [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) özelliği ayarlanarak Windows kimlik doğrulamasının etkinleştirilmesi gerekir:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

Windows kimlik doğrulaması yalnızca Microsoft Internet Explorer veya Microsoft Edge kullanılırken tarayıcı istemcisi tarafından desteklenir.

### <a name="use-claims-to-customize-identity-handling"></a>Kimlik işlemeyi özelleştirmek için talepler kullanma

Kullanıcıların kimliğini doğrulayan bir uygulama, Kullanıcı taleplerinden bir SignalR Kullanıcı kimliği türetilebilir. SignalR 'nin Kullanıcı kimlikleri nasıl oluşturduğunu belirtmek için, `IUserIdProvider` uygulamayı uygulayın ve kaydedin.

Örnek kod, tanımlama özelliği olarak kullanıcının e-posta adresini seçmek için talepleri nasıl kullanacağınızı gösterir. 

> [!NOTE]
> Seçtiğiniz değer, sisteminizdeki tüm kullanıcılar arasında benzersiz olmalıdır. Aksi takdirde, bir kullanıcı için tasarlanan bir ileti farklı bir kullanıcıya gidiyor olabilir.

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

Hesap kaydı, ASP.NET Identity veritabanına türünde `ClaimsTypes.Email` bir talep ekler.

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

Bu bileşeni ' `Startup.ConfigureServices`ye kaydedin.

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a>Kullanıcılara, hub 'lara ve hub yöntemlerine erişim yetkisi verme

Varsayılan olarak, bir hub 'daki tüm yöntemler kimliği doğrulanmamış bir kullanıcı tarafından çağrılabilir. Kimlik doğrulaması gerektirmek için, [Yetkilendir](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) özniteliğini hub 'a uygulayın:

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

Yalnızca belirli `[Authorize]` [Yetkilendirme ilkeleriyle](xref:security/authorization/policies)eşleşen kullanıcılara erişimi kısıtlamak için özniteliğinin Oluşturucu bağımsız değişkenlerini ve özelliklerini kullanabilirsiniz. Örneğin, adlı `MyAuthorizationPolicy` bir özel yetkilendirme ilkeniz varsa, aşağıdaki kodu kullanarak yalnızca bu ilkeyle eşleşen kullanıcıların hub 'a erişmesini sağlayabilirsiniz:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub : Hub
{
}
```

Tek bir hub yöntemi `[Authorize]` özniteliğe de uygulanmış olabilir. Geçerli Kullanıcı, yönteme uygulanan ilkeyle eşleşmezse, çağırana bir hata döndürülür:

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

### <a name="use-authorization-handlers-to-customize-hub-method-authorization"></a>Hub yöntemi yetkilendirmesini özelleştirmek için yetkilendirme işleyicilerini kullanma

SignalR, bir hub yöntemi yetkilendirme gerektirdiğinde, yetkilendirme işleyicilerine özel bir kaynak sağlar. Kaynak bir örneğidir `HubInvocationContext`. , Çağrılan hub `HubCallerContext`yönteminin adını ve hub yönteminin bağımsız değişkenlerini içerir.`HubInvocationContext`

Azure Active Directory aracılığıyla birden çok kuruluşun oturum açmasına izin veren bir sohbet odası örneğini göz önünde bulundurun. Microsoft hesabı olan herkes sohbet için oturum açabilir, ancak yalnızca sahip olunan kuruluşun üyeleri kullanıcıları yasaklatabilecek veya kullanıcıların sohbet geçmişlerini görüntüleyebilmelidir. Ayrıca, belirli kullanıcılardan belirli işlevleri kısıtlamak isteyebilirsiniz. ASP.NET Core 3,0 ' deki güncelleştirilmiş özellikleri kullanarak bu tamamen mümkündür. Nasıl `DomainRestrictedRequirement` özel`IAuthorizationRequirement`olarak işlev gördüğüne göz önünde kalın. `HubInvocationContext` Kaynak parametresi geçirilmeye hazır olduğuna göre, iç mantık hub 'ın çağrıldığı bağlamı inceleyebilir ve kullanıcının bireysel hub yöntemlerini yürütmesine izin verirken kararlar verebilir.

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

İçinde `Startup.ConfigureServices`, `DomainRestricted` ilkeyi oluşturmak için özel `DomainRestrictedRequirement` gereksinimi bir parametre olarak sağlayarak yeni ilkeyi ekleyin.

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

Yukarıdaki örnekte, `DomainRestrictedRequirement` sınıfı, bu gereksinim için hem bir `IAuthorizationRequirement` ve kendisi `AuthorizationHandler` olur. Bu iki bileşeni birbirinden ayrı ayrı sınıflara bölmek kabul edilebilir. Örneğin yaklaşımın bir avantajı, gereksinim ve işleyicinin aynı şey olduğu için başlangıç `AuthorizationHandler` sırasında ekleme zorunluluktur.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core 'de taşıyıcı belirteç kimlik doğrulaması](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [Kaynak tabanlı yetkilendirme](xref:security/authorization/resourcebased)
