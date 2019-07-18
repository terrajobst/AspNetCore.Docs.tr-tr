---
title: ASP.NET Core için gRPC 'de kimlik doğrulaması ve yetkilendirme
author: jamesnk
description: ASP.NET Core için gRPC 'de kimlik doğrulama ve yetkilendirmeyi nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 06/07/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 49024295e4db7976924397bb24567d92d6298562
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308817"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="16f25-103">ASP.NET Core için gRPC 'de kimlik doğrulaması ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="16f25-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="16f25-104">, [James bAyKiNg](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="16f25-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="16f25-105">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(indirme)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="16f25-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="16f25-106">GRPC hizmetini çağıran kullanıcıların kimliğini doğrulama</span><span class="sxs-lookup"><span data-stu-id="16f25-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="16f25-107">gRPC, bir kullanıcıyı her çağrıyla ilişkilendirmek için [ASP.NET Core kimlik doğrulamasıyla](xref:security/authentication/identity) birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="16f25-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="16f25-108">Aşağıda, GRPC ve ASP.NET Core `Startup.Configure` kimlik doğrulaması kullanan bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="16f25-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        routes.MapGrpcService<GreeterService>();
    });
}
```

> [!NOTE]
> <span data-ttu-id="16f25-109">ASP.NET Core kimlik doğrulama ara yazılımını kaydetme sırası önemli.</span><span class="sxs-lookup"><span data-stu-id="16f25-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="16f25-110">Her zaman `UseAuthentication` ve `UseAuthorization` sonra `UseRouting` ve`UseEndpoints`sonra çağırın.</span><span class="sxs-lookup"><span data-stu-id="16f25-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="16f25-111">Kimlik doğrulaması kurulduktan sonra, Kullanıcı aracılığıyla `ServerCallContext`bir GRPC hizmeti yöntemleriyle erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="16f25-111">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="16f25-112">Taşıyıcı belirteç kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="16f25-112">Bearer token authentication</span></span>

<span data-ttu-id="16f25-113">İstemci, kimlik doğrulaması için bir erişim belirteci sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="16f25-113">The client can provide an access token for authentication.</span></span> <span data-ttu-id="16f25-114">Sunucu belirteci doğrular ve kullanıcıyı tanımlamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="16f25-114">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="16f25-115">Sunucusunda, taşıyıcı belirteç kimlik doğrulaması [JWT taşıyıcı ara yazılımı](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="16f25-115">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="16f25-116">.NET gRPC istemcisinde, belirteç aramalar ile üst bilgi olarak gönderilebilir:</span><span class="sxs-lookup"><span data-stu-id="16f25-116">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

```csharp
public bool DoAuthenticatedCall(
    Ticketer.TicketerClient client, string token)
{
    var headers = new Metadata();
    headers.Add("Authorization", $"Bearer {token}");

    var request = new BuyTicketsRequest { Count = 1 };
    var response = await client.BuyTicketsAsync(request, headers);

    return response.Success;
}
```

### <a name="client-certificate-authentication"></a><span data-ttu-id="16f25-117">İstemci sertifikası kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="16f25-117">Client certificate authentication</span></span>

<span data-ttu-id="16f25-118">İstemci alternatif olarak kimlik doğrulaması için bir istemci sertifikası sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="16f25-118">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="16f25-119">[Sertifika kimlik doğrulaması](https://tools.ietf.org/html/rfc5246#section-7.4.4) TLS düzeyinde gerçekleşir ve bu süre ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="16f25-119">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="16f25-120">İstek ASP.NET Core girdiğinde, [istemci sertifikası kimlik doğrulama paketi](xref:security/authentication/certauth) , sertifikayı bir `ClaimsPrincipal`ile çözmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="16f25-120">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="16f25-121">Konağın istemci sertifikalarını kabul edecek şekilde yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="16f25-121">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="16f25-122">Bkz. Kestrel, IIS ve Azure 'da istemci sertifikalarını kabul etme hakkında bilgi için bkz. [ana bilgisayarınızı yapılandırma](xref:security/authentication/certauth#configure-your-host-to-require-certificates) .</span><span class="sxs-lookup"><span data-stu-id="16f25-122">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="16f25-123">.Net GRPC istemcisinde, daha sonra GRPC istemcisini oluşturmak için kullanılan `HttpClientHandler` istemci sertifikası eklenir:</span><span class="sxs-lookup"><span data-stu-id="16f25-123">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

```csharp
public Ticketer.TicketerClient CreateClientWithCert(
    string baseAddress,
    X509Certificate2 certificate)
{
    // Add client cert to the handler
    var handler = new HttpClientHandler();
    handler.ClientCertificates.Add(certificate);

    // Create the gRPC client
    var httpClient = new HttpClient(handler);
    httpClient.BaseAddress = new Uri(baseAddress);

    return GrpcClient.Create<Ticketer.TicketerClient>(httpClient);
}
```

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="16f25-124">Diğer kimlik doğrulama mekanizmaları</span><span class="sxs-lookup"><span data-stu-id="16f25-124">Other authentication mechanisms</span></span>

<span data-ttu-id="16f25-125">Taşıyıcı belirtecinin ve istemci sertifikası kimlik doğrulamasının yanı sıra, OAuth, OpenID ve Negotiate gibi ASP.NET Core tüm desteklenen kimlik doğrulama mekanizmaları gRPC ile çalışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="16f25-125">In addition to bearer token and client certificate authentication, all ASP.NET Core supported authentication mechanisms such as OAuth, OpenID and Negotiate should work with gRPC.</span></span> <span data-ttu-id="16f25-126">Sunucu tarafında kimlik doğrulamasını yapılandırma hakkında daha fazla bilgi için [ASP.NET Core kimlik doğrulamasını](xref:security/authentication/identity) ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="16f25-126">Visit [ASP.NET Core authentication](xref:security/authentication/identity) for more information for configuring authentication on the server side.</span></span>

<span data-ttu-id="16f25-127">İstemci tarafı yapılandırması, kullanmakta olduğunuz kimlik doğrulama mekanizmasına bağlı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="16f25-127">Client side configuration will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="16f25-128">Önceki taşıyıcı belirteç ve istemci sertifikası kimlik doğrulaması örnekleri, GRPC istemcisinin, gRPC çağrılarına kimlik doğrulama meta verileri gönderecek şekilde yapılandırılabilmesinin birkaç yolunu göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="16f25-128">The previous bearer token and client certificate authentication examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="16f25-129">Türü kesin belirlenmiş GRPC istemcileri `HttpClient` dahili olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="16f25-129">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="16f25-130">Kimlik doğrulaması [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), veya için `HttpClient`özel [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) örnekler eklenerek yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="16f25-130">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="16f25-131">Her GRPC çağrısının isteğe bağlı `CallOptions` bir bağımsız değişkeni vardır.</span><span class="sxs-lookup"><span data-stu-id="16f25-131">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="16f25-132">Özel üstbilgiler, seçeneğin üstbilgiler koleksiyonu kullanılarak gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="16f25-132">Custom headers can be sent using the option's headers collection.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="16f25-133">Kullanıcılara hizmetlere ve hizmet yöntemlerine erişim yetkisi verme</span><span class="sxs-lookup"><span data-stu-id="16f25-133">Authorize users to access services and service methods</span></span>

<span data-ttu-id="16f25-134">Varsayılan olarak, bir hizmette tüm yöntemler kimliği doğrulanmamış kullanıcılar tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="16f25-134">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="16f25-135">Kimlik doğrulaması gerektirmek için, [[Yetkilendir]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) özniteliğini hizmete uygulayın:</span><span class="sxs-lookup"><span data-stu-id="16f25-135">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="16f25-136">Yalnızca belirli `[Authorize]` [Yetkilendirme ilkeleriyle](xref:security/authorization/policies)eşleşen kullanıcılara erişimi kısıtlamak için özniteliğinin Oluşturucu bağımsız değişkenlerini ve özelliklerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16f25-136">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="16f25-137">Örneğin, adlı `MyAuthorizationPolicy`bir özel yetkilendirme ilkeniz varsa, aşağıdaki kodu kullanarak yalnızca bu ilkeyle eşleşen kullanıcıların hizmete erişebildiğinden emin olun:</span><span class="sxs-lookup"><span data-stu-id="16f25-137">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="16f25-138">Bağımsız hizmet yöntemlerinin `[Authorize]` özniteliği de uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="16f25-138">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="16f25-139">Geçerli Kullanıcı hem yönteme hem **de** sınıfa uygulanan ilkelerle eşleşmezse, çağırana bir hata döndürülür:</span><span class="sxs-lookup"><span data-stu-id="16f25-139">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
    public override Task<AvailableTicketsResponse> GetAvailableTickets(
        Empty request, ServerCallContext context)
    {
        // ... buy tickets for the current user ...
    }

    [Authorize("Administrators")]
    public override Task<BuyTicketsResponse> RefundTickets(
        BuyTicketsRequest request, ServerCallContext context)
    {
        // ... refund tickets (something only Administrators can do) ..
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="16f25-140">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="16f25-140">Additional resources</span></span>

* [<span data-ttu-id="16f25-141">ASP.NET Core 'de taşıyıcı belirteç kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="16f25-141">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="16f25-142">ASP.NET Core 'de Istemci sertifikası kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="16f25-142">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
