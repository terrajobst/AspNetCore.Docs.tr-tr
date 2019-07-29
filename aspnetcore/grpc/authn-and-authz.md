---
title: ASP.NET Core için gRPC 'de kimlik doğrulaması ve yetkilendirme
author: jamesnk
description: ASP.NET Core için gRPC 'de kimlik doğrulama ve yetkilendirmeyi nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 07/26/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 34f7f8a5a22159329b3d6c4524943434c460c7fb
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602431"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="811fc-103">ASP.NET Core için gRPC 'de kimlik doğrulaması ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="811fc-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="811fc-104">, [James bAyKiNg](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="811fc-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="811fc-105">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(indirme)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="811fc-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="811fc-106">GRPC hizmetini çağıran kullanıcıların kimliğini doğrulama</span><span class="sxs-lookup"><span data-stu-id="811fc-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="811fc-107">gRPC, bir kullanıcıyı her çağrıyla ilişkilendirmek için [ASP.NET Core kimlik doğrulamasıyla](xref:security/authentication/identity) birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="811fc-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="811fc-108">Aşağıda, GRPC ve ASP.NET Core `Startup.Configure` kimlik doğrulaması kullanan bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="811fc-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

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
> <span data-ttu-id="811fc-109">ASP.NET Core kimlik doğrulama ara yazılımını kaydetme sırası önemli.</span><span class="sxs-lookup"><span data-stu-id="811fc-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="811fc-110">Her zaman `UseAuthentication` ve `UseAuthorization` sonra `UseRouting` ve`UseEndpoints`sonra çağırın.</span><span class="sxs-lookup"><span data-stu-id="811fc-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="811fc-111">Kimlik doğrulaması kurulduktan sonra, Kullanıcı aracılığıyla `ServerCallContext`bir GRPC hizmeti yöntemleriyle erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="811fc-111">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="811fc-112">Taşıyıcı belirteç kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="811fc-112">Bearer token authentication</span></span>

<span data-ttu-id="811fc-113">İstemci, kimlik doğrulaması için bir erişim belirteci sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="811fc-113">The client can provide an access token for authentication.</span></span> <span data-ttu-id="811fc-114">Sunucu belirteci doğrular ve kullanıcıyı tanımlamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="811fc-114">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="811fc-115">Sunucusunda, taşıyıcı belirteç kimlik doğrulaması [JWT taşıyıcı ara yazılımı](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="811fc-115">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="811fc-116">.NET gRPC istemcisinde, belirteç aramalar ile üst bilgi olarak gönderilebilir:</span><span class="sxs-lookup"><span data-stu-id="811fc-116">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

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

### <a name="client-certificate-authentication"></a><span data-ttu-id="811fc-117">İstemci sertifikası kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="811fc-117">Client certificate authentication</span></span>

<span data-ttu-id="811fc-118">İstemci alternatif olarak kimlik doğrulaması için bir istemci sertifikası sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="811fc-118">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="811fc-119">[Sertifika kimlik doğrulaması](https://tools.ietf.org/html/rfc5246#section-7.4.4) TLS düzeyinde gerçekleşir ve bu süre ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="811fc-119">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="811fc-120">İstek ASP.NET Core girdiğinde, [istemci sertifikası kimlik doğrulama paketi](xref:security/authentication/certauth) , sertifikayı bir `ClaimsPrincipal`ile çözmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="811fc-120">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="811fc-121">Konağın istemci sertifikalarını kabul edecek şekilde yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="811fc-121">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="811fc-122">Bkz. Kestrel, IIS ve Azure 'da istemci sertifikalarını kabul etme hakkında bilgi için bkz. [ana bilgisayarınızı yapılandırma](xref:security/authentication/certauth#configure-your-host-to-require-certificates) .</span><span class="sxs-lookup"><span data-stu-id="811fc-122">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="811fc-123">.Net GRPC istemcisinde, daha sonra GRPC istemcisini oluşturmak için kullanılan `HttpClientHandler` istemci sertifikası eklenir:</span><span class="sxs-lookup"><span data-stu-id="811fc-123">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

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

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="811fc-124">Diğer kimlik doğrulama mekanizmaları</span><span class="sxs-lookup"><span data-stu-id="811fc-124">Other authentication mechanisms</span></span>

<span data-ttu-id="811fc-125">Desteklenen birçok ASP.NET Core kimlik doğrulama mekanizması gRPC ile çalışır:</span><span class="sxs-lookup"><span data-stu-id="811fc-125">Many ASP.NET Core supported authentication mechanisms work with gRPC:</span></span>

* <span data-ttu-id="811fc-126">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="811fc-126">Azure Active Directory</span></span>
* <span data-ttu-id="811fc-127">İstemci sertifikası</span><span class="sxs-lookup"><span data-stu-id="811fc-127">Client Certificate</span></span>
* <span data-ttu-id="811fc-128">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="811fc-128">IdentityServer</span></span>
* <span data-ttu-id="811fc-129">JWT belirteci</span><span class="sxs-lookup"><span data-stu-id="811fc-129">JWT Token</span></span>
* <span data-ttu-id="811fc-130">OAuth 2,0</span><span class="sxs-lookup"><span data-stu-id="811fc-130">OAuth 2.0</span></span>
* <span data-ttu-id="811fc-131">OpenID Connect</span><span class="sxs-lookup"><span data-stu-id="811fc-131">OpenID Connect</span></span>
* <span data-ttu-id="811fc-132">WS-Federation</span><span class="sxs-lookup"><span data-stu-id="811fc-132">WS-Federation</span></span>

<span data-ttu-id="811fc-133">Sunucuda kimlik doğrulamasını yapılandırma hakkında daha fazla bilgi için, [ASP.NET Core kimlik doğrulaması](xref:security/authentication/identity)' na bakın.</span><span class="sxs-lookup"><span data-stu-id="811fc-133">For more information on configuring authentication on the server, see [ASP.NET Core authentication](xref:security/authentication/identity).</span></span>

<span data-ttu-id="811fc-134">GRPC istemcisini kimlik doğrulaması kullanacak şekilde yapılandırmak, kullanmakta olduğunuz kimlik doğrulama mekanizmasına bağlı olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="811fc-134">Configuring the gRPC client to use authentication will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="811fc-135">Önceki taşıyıcı belirteci ve istemci sertifikası örnekleri, GRPC istemcisinin, gRPC çağrılarına yönelik kimlik doğrulama meta verilerini gönderecek şekilde yapılandırılabilmesinin birkaç yolunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="811fc-135">The previous bearer token and client certificate examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="811fc-136">Türü kesin belirlenmiş GRPC istemcileri `HttpClient` dahili olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="811fc-136">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="811fc-137">Kimlik doğrulaması [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), veya için `HttpClient`özel [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) örnekler eklenerek yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="811fc-137">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="811fc-138">Her GRPC çağrısının isteğe bağlı `CallOptions` bir bağımsız değişkeni vardır.</span><span class="sxs-lookup"><span data-stu-id="811fc-138">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="811fc-139">Özel üstbilgiler, seçeneğin üstbilgiler koleksiyonu kullanılarak gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="811fc-139">Custom headers can be sent using the option's headers collection.</span></span>

> [!NOTE]
> <span data-ttu-id="811fc-140">Windows kimlik doğrulaması (NTLM/Kerberos/Negotiate), gRPC ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="811fc-140">Windows Authentication (NTLM/Kerberos/Negotiate) can't be used with gRPC.</span></span> <span data-ttu-id="811fc-141">gRPC için HTTP/2 ve HTTP/2 Windows kimlik doğrulamasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="811fc-141">gRPC requires HTTP/2, and HTTP/2 doesn't support Windows Authentication.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="811fc-142">Kullanıcılara hizmetlere ve hizmet yöntemlerine erişim yetkisi verme</span><span class="sxs-lookup"><span data-stu-id="811fc-142">Authorize users to access services and service methods</span></span>

<span data-ttu-id="811fc-143">Varsayılan olarak, bir hizmette tüm yöntemler kimliği doğrulanmamış kullanıcılar tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="811fc-143">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="811fc-144">Kimlik doğrulaması gerektirmek için, [[Yetkilendir]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) özniteliğini hizmete uygulayın:</span><span class="sxs-lookup"><span data-stu-id="811fc-144">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="811fc-145">Yalnızca belirli `[Authorize]` [Yetkilendirme ilkeleriyle](xref:security/authorization/policies)eşleşen kullanıcılara erişimi kısıtlamak için özniteliğinin Oluşturucu bağımsız değişkenlerini ve özelliklerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="811fc-145">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="811fc-146">Örneğin, adlı `MyAuthorizationPolicy`bir özel yetkilendirme ilkeniz varsa, aşağıdaki kodu kullanarak yalnızca bu ilkeyle eşleşen kullanıcıların hizmete erişebildiğinden emin olun:</span><span class="sxs-lookup"><span data-stu-id="811fc-146">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="811fc-147">Bağımsız hizmet yöntemlerinin `[Authorize]` özniteliği de uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="811fc-147">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="811fc-148">Geçerli Kullanıcı hem yönteme hem **de** sınıfa uygulanan ilkelerle eşleşmezse, çağırana bir hata döndürülür:</span><span class="sxs-lookup"><span data-stu-id="811fc-148">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="811fc-149">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="811fc-149">Additional resources</span></span>

* [<span data-ttu-id="811fc-150">ASP.NET Core 'de taşıyıcı belirteç kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="811fc-150">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="811fc-151">ASP.NET Core 'de Istemci sertifikası kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="811fc-151">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
