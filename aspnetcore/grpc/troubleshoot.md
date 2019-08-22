---
title: .NET Core 'da gRPC sorunlarını giderme
author: jamesnk
description: .NET Core 'da gRPC kullanırken hata giderme.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/17/2019
uid: grpc/troubleshoot
ms.openlocfilehash: 7621266dfe26b7126d1607e195dd5dcaab4efa55
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886488"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="2bef9-103">.NET Core 'da gRPC sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="2bef9-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="2bef9-104">, [James bAyKiNg](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="2bef9-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="2bef9-105">İstemci ve hizmet SSL/TLS yapılandırması arasında uyuşmazlık</span><span class="sxs-lookup"><span data-stu-id="2bef9-105">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="2bef9-106">GRPC şablonu ve örnekleri, varsayılan olarak gRPC hizmetlerini güvenli hale getirmek için [Aktarım Katmanı Güvenliği 'ni (TLS)](https://tools.ietf.org/html/rfc5246) kullanır.</span><span class="sxs-lookup"><span data-stu-id="2bef9-106">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="2bef9-107">gRPC istemcilerinin güvenli gRPC hizmetlerini başarıyla çağırması için güvenli bir bağlantı kullanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2bef9-107">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="2bef9-108">ASP.NET Core gRPC hizmetinin uygulama başlatma bölümünde yazılan günlüklerde TLS kullandığını doğrulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2bef9-108">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="2bef9-109">Hizmet bir HTTPS uç noktasını dinleyerek:</span><span class="sxs-lookup"><span data-stu-id="2bef9-109">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="2bef9-110">.NET Core istemcisinin, güvenli bir `https` bağlantıyla çağrı yapmak için sunucu adresinde kullanması gerekir:</span><span class="sxs-lookup"><span data-stu-id="2bef9-110">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
}
```

<span data-ttu-id="2bef9-111">Tüm gRPC istemci uygulamaları TLS 'yi destekler.</span><span class="sxs-lookup"><span data-stu-id="2bef9-111">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="2bef9-112">diğer dillerden gRPC istemcileri, genellikle kanalı ile `SslCredentials`yapılandırılmış olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2bef9-112">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="2bef9-113">`SslCredentials`İstemcinin kullanacağı sertifikayı belirtir ve güvenli olmayan kimlik bilgileri yerine kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2bef9-113">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="2bef9-114">Farklı gRPC istemci uygulamalarını TLS kullanmak üzere yapılandırma örnekleri için bkz. [GRPC kimlik doğrulaması](https://www.grpc.io/docs/guides/auth/).</span><span class="sxs-lookup"><span data-stu-id="2bef9-114">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="2bef9-115">.NET Core istemcisiyle güvenli olmayan gRPC hizmetlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="2bef9-115">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="2bef9-116">.NET Core istemcisiyle güvenli olmayan gRPC hizmetlerini çağırmak için ek yapılandırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="2bef9-116">Additional configuration is required to call insecure gRPC services with the .NET Core client.</span></span> <span data-ttu-id="2bef9-117">GRPC istemcisinin, sunucu adresinde `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` anahtarı olarak `true` ayarlanması ve kullanması `http` gerekir:</span><span class="sxs-lookup"><span data-stu-id="2bef9-117">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="2bef9-118">MacOS 'ta ASP.NET Core gRPC uygulaması başlatılamıyor</span><span class="sxs-lookup"><span data-stu-id="2bef9-118">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="2bef9-119">Kestrel, macOS ve Windows 7 gibi eski Windows sürümlerindeki TLS ile HTTP/2 ' yi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="2bef9-119">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="2bef9-120">ASP.NET Core gRPC şablonu ve örnekleri varsayılan olarak TLS kullanır.</span><span class="sxs-lookup"><span data-stu-id="2bef9-120">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="2bef9-121">GRPC sunucusunu başlatmayı denediğinizde aşağıdaki hata iletisini görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="2bef9-121">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="2bef9-122">https://localhost:5001 IPv4 geri döngü arabirimine bağlanılamıyor: Eksik ALPN desteği nedeniyle, macOS 'ta TLS üzerinden HTTP/2 desteklenmez. '.</span><span class="sxs-lookup"><span data-stu-id="2bef9-122">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="2bef9-123">Bu sorunu geçici olarak çözmek için Kestrel ve gRPC istemcisini TLS *olmadan* http/2 kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2bef9-123">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="2bef9-124">Bunu yalnızca geliştirme sırasında yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2bef9-124">You should only do this during development.</span></span> <span data-ttu-id="2bef9-125">TLS 'nin kullanılması, gRPC iletilerinin şifrelenmeden gönderilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="2bef9-125">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="2bef9-126">Kestrel, *program.cs*içinde TLS olmadan bir http/2 uç noktası yapılandırmalıdır:</span><span class="sxs-lookup"><span data-stu-id="2bef9-126">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="2bef9-127">Bir HTTP/2 uç noktası TLS olmadan yapılandırıldığında, uç noktanın [Listenoptions. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) olarak `HttpProtocols.Http2`ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2bef9-127">When an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="2bef9-128">`HttpProtocols.Http1AndHttp2`, HTTP/2 üzerinde anlaşmak için TLS gerektiğinden kullanılamıyor.</span><span class="sxs-lookup"><span data-stu-id="2bef9-128">`HttpProtocols.Http1AndHttp2` can't be used because TLS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="2bef9-129">TLS olmadan, uç nokta varsayılan HTTP/1.1 ve gRPC çağrıları için tüm bağlantılar başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="2bef9-129">Without TLS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="2bef9-130">GRPC istemcisinin, TLS kullanmak için de yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2bef9-130">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="2bef9-131">Daha fazla bilgi için bkz. [.NET Core istemcisiyle güvenli olmayan gRPC hizmetlerini çağırma](#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="2bef9-131">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="2bef9-132">HTTP/2, TLS olmadan yalnızca uygulama geliştirme sırasında kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2bef9-132">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="2bef9-133">Üretim uygulamaları her zaman aktarım güvenliği kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2bef9-133">Production apps should always use transport security.</span></span> <span data-ttu-id="2bef9-134">Daha fazla bilgi için bkz. [gRPC 'Deki güvenlik konuları ASP.NET Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="2bef9-134">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="2bef9-135">GRPC C# varlıkları  *\*. proto* dosyalarından oluşturulan kod değildir</span><span class="sxs-lookup"><span data-stu-id="2bef9-135">gRPC C# assets are not code generated from *\*.proto* files</span></span>

<span data-ttu-id="2bef9-136">aynı somut istemci ve hizmet temel sınıflarının gRPC kod üretimi, bir projeden başvurulmak için prototip dosyaları ve araçları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2bef9-136">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="2bef9-137">Şunları dahil etmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="2bef9-137">You must include:</span></span>

* <span data-ttu-id="2bef9-138">`<Protobuf>` öğe grubunda kullanmak istediğiniz *. proto* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="2bef9-138">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="2bef9-139">[Içeri aktarılan *. proto* dosyalarına](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) proje tarafından başvurulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2bef9-139">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="2bef9-140">GRPC araç paketi [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/)'a paket başvurusu.</span><span class="sxs-lookup"><span data-stu-id="2bef9-140">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="2bef9-141">GRPC C# varlıkları oluşturma hakkında daha fazla bilgi için bkz <xref:grpc/basics>.</span><span class="sxs-lookup"><span data-stu-id="2bef9-141">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="2bef9-142">Varsayılan olarak, `<Protobuf>` başvuru somut bir istemci ve hizmet temel sınıfı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2bef9-142">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="2bef9-143">Başvuru öğesinin `GrpcServices` özniteliği varlık oluşturmayı sınırlamak C# için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2bef9-143">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="2bef9-144">Geçerli `GrpcServices` seçenekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2bef9-144">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="2bef9-145">`Both`(mevcut olmadığında varsayılan)</span><span class="sxs-lookup"><span data-stu-id="2bef9-145">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

<span data-ttu-id="2bef9-146">GRPC hizmetlerini barındıran bir ASP.NET Core Web uygulaması yalnızca oluşturulan hizmet taban sınıfına ihtiyaç duyuyor:</span><span class="sxs-lookup"><span data-stu-id="2bef9-146">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="2bef9-147">GRPC çağrısı yapan bir gRPC istemci uygulaması yalnızca somut istemcinin oluşturulması gerekir:</span><span class="sxs-lookup"><span data-stu-id="2bef9-147">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
