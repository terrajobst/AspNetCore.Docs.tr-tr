---
title: ASP.NET Core için gRPC 'de kimlik doğrulaması ve yetkilendirme
author: jamesnk
description: ASP.NET Core için gRPC 'de kimlik doğrulama ve yetkilendirmeyi nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 12/05/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 258b34113f3c3d9ef2031a43295ea5806b1e22ff
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880686"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="b09ec-103">ASP.NET Core için gRPC 'de kimlik doğrulaması ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="b09ec-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="b09ec-104">, [James bAyKiNg](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="b09ec-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="b09ec-105">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(nasıl indirileceği)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="b09ec-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="b09ec-106">GRPC hizmetini çağıran kullanıcıların kimliğini doğrulama</span><span class="sxs-lookup"><span data-stu-id="b09ec-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="b09ec-107">gRPC, bir kullanıcıyı her çağrıyla ilişkilendirmek için [ASP.NET Core kimlik doğrulamasıyla](xref:security/authentication/identity) birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b09ec-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="b09ec-108">Aşağıda, gRPC ve ASP.NET Core kimlik doğrulaması kullanan `Startup.Configure` bir örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="b09ec-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

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
> <span data-ttu-id="b09ec-109">ASP.NET Core kimlik doğrulama ara yazılımını kaydetme sırası önemli.</span><span class="sxs-lookup"><span data-stu-id="b09ec-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="b09ec-110">`UseRouting` ve `UseEndpoints`önce `UseAuthentication` ve `UseAuthorization` her zaman çağırın.</span><span class="sxs-lookup"><span data-stu-id="b09ec-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="b09ec-111">Bir çağrı sırasında uygulamanızın kullandığı kimlik doğrulama mekanizması yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b09ec-111">The authentication mechanism your app uses during a call needs to be configured.</span></span> <span data-ttu-id="b09ec-112">Kimlik doğrulama yapılandırması `Startup.ConfigureServices` eklenir ve uygulamanızın kullandığı kimlik doğrulama mekanizmasına bağlı olarak farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="b09ec-112">Authentication configuration is added in `Startup.ConfigureServices` and will be different depending upon the authentication mechanism your app uses.</span></span> <span data-ttu-id="b09ec-113">ASP.NET Core uygulamaları güvenli hale getirmeye yönelik örnekler için bkz. [kimlik doğrulama örnekleri](xref:security/authentication/samples).</span><span class="sxs-lookup"><span data-stu-id="b09ec-113">For examples of how to secure ASP.NET Core apps, see [Authentication samples](xref:security/authentication/samples).</span></span>

<span data-ttu-id="b09ec-114">Kimlik doğrulaması kurulduktan sonra, kullanıcıya `ServerCallContext`aracılığıyla bir gRPC hizmeti yöntemleriyle erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="b09ec-114">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="b09ec-115">Taşıyıcı belirteç kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="b09ec-115">Bearer token authentication</span></span>

<span data-ttu-id="b09ec-116">İstemci, kimlik doğrulaması için bir erişim belirteci sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="b09ec-116">The client can provide an access token for authentication.</span></span> <span data-ttu-id="b09ec-117">Sunucu belirteci doğrular ve kullanıcıyı tanımlamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="b09ec-117">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="b09ec-118">Sunucusunda, taşıyıcı belirteç kimlik doğrulaması [JWT taşıyıcı ara yazılımı](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="b09ec-118">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="b09ec-119">.NET gRPC istemcisinde, belirteç aramalar ile üst bilgi olarak gönderilebilir:</span><span class="sxs-lookup"><span data-stu-id="b09ec-119">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

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

<span data-ttu-id="b09ec-120">Bir kanalda `ChannelCredentials` yapılandırmak, belirteci gRPC çağrılarıyla hizmete göndermenin alternatif bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="b09ec-120">Configuring `ChannelCredentials` on a channel is an alternative way to send the token to the service with gRPC calls.</span></span> <span data-ttu-id="b09ec-121">Kimlik bilgisi her bir gRPC çağrısının yapılışında çalıştırılır. Bu, belirteci kendi kendinize geçirmek için birden çok yere kod yazma gereksinimini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="b09ec-121">The credential is run each time a gRPC call is made, which avoids the need to write code in multiple places to pass the token yourself.</span></span>

<span data-ttu-id="b09ec-122">Aşağıdaki örnekteki kimlik bilgileri, kanalı her gRPC çağrısıyla birlikte gönderecek şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="b09ec-122">The credential in the following example configures the channel to send the token with every gRPC call:</span></span>

```csharp
private static GrpcChannel CreateAuthenticatedChannel(string address)
{
    var credentials = CallCredentials.FromInterceptor((context, metadata) =>
    {
        if (!string.IsNullOrEmpty(_token))
        {
            metadata.Add("Authorization", $"Bearer {_token}");
        }
        return Task.CompletedTask;
    });

    // SslCredentials is used here because this channel is using TLS.
    // CallCredentials can't be used with ChannelCredentials.Insecure on non-TLS channels.
    var channel = GrpcChannel.ForAddress(address, new GrpcChannelOptions
    {
        Credentials = ChannelCredentials.Create(new SslCredentials(), credentials)
    });
    return channel;
}
```

### <a name="client-certificate-authentication"></a><span data-ttu-id="b09ec-123">İstemci sertifikası kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="b09ec-123">Client certificate authentication</span></span>

<span data-ttu-id="b09ec-124">İstemci alternatif olarak kimlik doğrulaması için bir istemci sertifikası sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="b09ec-124">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="b09ec-125">[Sertifika kimlik doğrulaması](https://tools.ietf.org/html/rfc5246#section-7.4.4) TLS düzeyinde gerçekleşir ve bu süre ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b09ec-125">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="b09ec-126">İstek ASP.NET Core girdiğinde, [istemci sertifikası kimlik doğrulama paketi](xref:security/authentication/certauth) , sertifikayı bir `ClaimsPrincipal`çözmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b09ec-126">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="b09ec-127">Konağın istemci sertifikalarını kabul edecek şekilde yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b09ec-127">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="b09ec-128">Bkz. Kestrel, IIS ve Azure 'da istemci sertifikalarını kabul etme hakkında bilgi için bkz. [ana bilgisayarınızı yapılandırma](xref:security/authentication/certauth#configure-your-host-to-require-certificates) .</span><span class="sxs-lookup"><span data-stu-id="b09ec-128">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="b09ec-129">.NET gRPC istemcisinde istemci sertifikası, daha sonra gRPC istemcisini oluşturmak için kullanılan `HttpClientHandler` eklenir:</span><span class="sxs-lookup"><span data-stu-id="b09ec-129">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

```csharp
public Ticketer.TicketerClient CreateClientWithCert(
    string baseAddress,
    X509Certificate2 certificate)
{
    // Add client cert to the handler
    var handler = new HttpClientHandler();
    handler.ClientCertificates.Add(certificate);

    // Create the gRPC channel
    var channel = GrpcChannel.ForAddress(baseAddress, new GrpcChannelOptions
    {
        HttpClient = new HttpClient(handler)
    });

    return new Ticketer.TicketerClient(channel);
}
```

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="b09ec-130">Diğer kimlik doğrulama mekanizmaları</span><span class="sxs-lookup"><span data-stu-id="b09ec-130">Other authentication mechanisms</span></span>

<span data-ttu-id="b09ec-131">Desteklenen birçok ASP.NET Core kimlik doğrulama mekanizması gRPC ile çalışır:</span><span class="sxs-lookup"><span data-stu-id="b09ec-131">Many ASP.NET Core supported authentication mechanisms work with gRPC:</span></span>

* <span data-ttu-id="b09ec-132">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b09ec-132">Azure Active Directory</span></span>
* <span data-ttu-id="b09ec-133">İstemci sertifikası</span><span class="sxs-lookup"><span data-stu-id="b09ec-133">Client Certificate</span></span>
* <span data-ttu-id="b09ec-134">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="b09ec-134">IdentityServer</span></span>
* <span data-ttu-id="b09ec-135">JWT belirteci</span><span class="sxs-lookup"><span data-stu-id="b09ec-135">JWT Token</span></span>
* <span data-ttu-id="b09ec-136">OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="b09ec-136">OAuth 2.0</span></span>
* <span data-ttu-id="b09ec-137">OpenID Connect</span><span class="sxs-lookup"><span data-stu-id="b09ec-137">OpenID Connect</span></span>
* <span data-ttu-id="b09ec-138">WS-Federation</span><span class="sxs-lookup"><span data-stu-id="b09ec-138">WS-Federation</span></span>

<span data-ttu-id="b09ec-139">Sunucuda kimlik doğrulamasını yapılandırma hakkında daha fazla bilgi için, [ASP.NET Core kimlik doğrulaması](xref:security/authentication/identity)' na bakın.</span><span class="sxs-lookup"><span data-stu-id="b09ec-139">For more information on configuring authentication on the server, see [ASP.NET Core authentication](xref:security/authentication/identity).</span></span>

<span data-ttu-id="b09ec-140">GRPC istemcisini kimlik doğrulaması kullanacak şekilde yapılandırmak, kullanmakta olduğunuz kimlik doğrulama mekanizmasına bağlı olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="b09ec-140">Configuring the gRPC client to use authentication will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="b09ec-141">Önceki taşıyıcı belirteci ve istemci sertifikası örnekleri, GRPC istemcisinin, gRPC çağrılarına yönelik kimlik doğrulama meta verilerini gönderecek şekilde yapılandırılabilmesinin birkaç yolunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="b09ec-141">The previous bearer token and client certificate examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="b09ec-142">Türü kesin belirlenmiş gRPC istemcileri dahili olarak `HttpClient` kullanır.</span><span class="sxs-lookup"><span data-stu-id="b09ec-142">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="b09ec-143">Kimlik doğrulaması, [HttpClientHandler](/dotnet/api/system.net.http.httpclienthandler)üzerinde veya `HttpClient`özel [HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler) örnekleri eklenerek yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b09ec-143">Authentication can be configured on [HttpClientHandler](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="b09ec-144">Her gRPC çağrısının isteğe bağlı bir `CallOptions` bağımsız değişkeni vardır.</span><span class="sxs-lookup"><span data-stu-id="b09ec-144">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="b09ec-145">Özel üstbilgiler, seçeneğin üstbilgiler koleksiyonu kullanılarak gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="b09ec-145">Custom headers can be sent using the option's headers collection.</span></span>

> [!NOTE]
> <span data-ttu-id="b09ec-146">Windows kimlik doğrulaması (NTLM/Kerberos/Negotiate), gRPC ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="b09ec-146">Windows Authentication (NTLM/Kerberos/Negotiate) can't be used with gRPC.</span></span> <span data-ttu-id="b09ec-147">gRPC için HTTP/2 ve HTTP/2 Windows kimlik doğrulamasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="b09ec-147">gRPC requires HTTP/2, and HTTP/2 doesn't support Windows Authentication.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="b09ec-148">Kullanıcılara hizmetlere ve hizmet yöntemlerine erişim yetkisi verme</span><span class="sxs-lookup"><span data-stu-id="b09ec-148">Authorize users to access services and service methods</span></span>

<span data-ttu-id="b09ec-149">Varsayılan olarak, bir hizmette tüm yöntemler kimliği doğrulanmamış kullanıcılar tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b09ec-149">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="b09ec-150">Kimlik doğrulaması gerektirmek için [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) özniteliğini hizmete uygulayın:</span><span class="sxs-lookup"><span data-stu-id="b09ec-150">To require authentication, apply the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="b09ec-151">Yalnızca belirli [Yetkilendirme ilkeleriyle](xref:security/authorization/policies)eşleşen kullanıcılara erişimi kısıtlamak için `[Authorize]` özniteliğinin Oluşturucu bağımsız değişkenlerini ve özelliklerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b09ec-151">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="b09ec-152">Örneğin, `MyAuthorizationPolicy`adlı bir özel yetkilendirme ilkeniz varsa, aşağıdaki kodu kullanarak yalnızca bu ilkeyle eşleşen kullanıcıların hizmete erişebildiğinden emin olun:</span><span class="sxs-lookup"><span data-stu-id="b09ec-152">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="b09ec-153">Tek tek hizmet yöntemlerinde `[Authorize]` özniteliği de uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="b09ec-153">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="b09ec-154">Geçerli Kullanıcı hem yönteme hem **de** sınıfa uygulanan ilkelerle eşleşmezse, çağırana bir hata döndürülür:</span><span class="sxs-lookup"><span data-stu-id="b09ec-154">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="b09ec-155">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b09ec-155">Additional resources</span></span>

* [<span data-ttu-id="b09ec-156">ASP.NET Core 'de taşıyıcı belirteç kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="b09ec-156">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="b09ec-157">ASP.NET Core 'de Istemci sertifikası kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b09ec-157">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
