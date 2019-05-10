---
title: gRPC ASP.NET Core yapılandırma
author: jamesnk
description: GRPC ASP.NET Core uygulamaları için yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087337"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="24a28-103">gRPC ASP.NET Core yapılandırma</span><span class="sxs-lookup"><span data-stu-id="24a28-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="24a28-104">Hizmetleri seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="24a28-104">Configure services options</span></span>

<span data-ttu-id="24a28-105">Aşağıdaki tabloda, gRPC hizmetleri yapılandırmak için seçenekleri tanımlar:</span><span class="sxs-lookup"><span data-stu-id="24a28-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="24a28-106">Seçenek</span><span class="sxs-lookup"><span data-stu-id="24a28-106">Option</span></span> | <span data-ttu-id="24a28-107">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="24a28-107">Default Value</span></span> | <span data-ttu-id="24a28-108">Açıklama</span><span class="sxs-lookup"><span data-stu-id="24a28-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="24a28-109">Sunucudan gönderilen bayt cinsinden en büyük ileti boyutu.</span><span class="sxs-lookup"><span data-stu-id="24a28-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="24a28-110">Bir özel durum yapılandırılan en büyük ileti boyutu sonuçlarında aşan bir ileti gönderilmeye çalışılıyor.</span><span class="sxs-lookup"><span data-stu-id="24a28-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="24a28-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="24a28-111">4 MB</span></span> | <span data-ttu-id="24a28-112">Sunucu tarafından alınan bayt cinsinden en büyük ileti boyutu.</span><span class="sxs-lookup"><span data-stu-id="24a28-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="24a28-113">Sunucu bu sınırı aşan bir ileti alır bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="24a28-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="24a28-114">Bu değeri artırmak, daha büyük iletileri almasına olanak sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="24a28-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="24a28-115">Varsa `true`, ayrıntılı bir hizmet yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür.</span><span class="sxs-lookup"><span data-stu-id="24a28-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="24a28-116">Varsayılan, `false` değeridir.</span><span class="sxs-lookup"><span data-stu-id="24a28-116">The default is `false`.</span></span> <span data-ttu-id="24a28-117">Bu ayar `true` hassas bilgileri sızabilir.</span><span class="sxs-lookup"><span data-stu-id="24a28-117">Setting this to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="24a28-118">Gzip</span><span class="sxs-lookup"><span data-stu-id="24a28-118">gzip</span></span> | <span data-ttu-id="24a28-119">Sıkıştırmak ve iletileri genişletmek için kullanılan sıkıştırmasını sağlayıcıları koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="24a28-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="24a28-120">Özel sıkıştırmasını sağlayıcıları, oluşturulur ve koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="24a28-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="24a28-121">Varsayılan sağlayıcı destekler yapılandırılmış **gzip** sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="24a28-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="24a28-122">Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma algoritması.</span><span class="sxs-lookup"><span data-stu-id="24a28-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="24a28-123">Algoritma sıkıştırma sağlayıcı eşleşmelidir `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="24a28-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="24a28-124">Algorthm yanıt sıkıştırmak istemciye göndererek algoritmanın desteklediği belirtmeniz gerekir **grpc-kabul-encoding** başlığı.</span><span class="sxs-lookup"><span data-stu-id="24a28-124">For the algorthm to compress a response the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="24a28-125">Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma düzeyi.</span><span class="sxs-lookup"><span data-stu-id="24a28-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="24a28-126">Seçenekler yapılandırılabilir tüm hizmetler için bir seçenekler temsilciye sağlayarak `AddGrpc` Çağır `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24a28-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

<span data-ttu-id="24a28-127">Tek bir hizmet seçeneklerini geçersiz kılmak, sağlanan genel seçenekleri `AddGrpc` ve kullanılarak yapılandırılabilir `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="24a28-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a><span data-ttu-id="24a28-128">Kestrel'i seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="24a28-128">Configure Kestrel options</span></span>

<span data-ttu-id="24a28-129">Kestrel'i sunucu için ASP.NET gRPC davranışını etkileyen yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="24a28-129">Kestrel server has configuration options that affect the behavior of gRPC for ASP.NET.</span></span>

### <a name="request-body-data-rate-limit"></a><span data-ttu-id="24a28-130">İstek gövdesi veri hızı sınırı</span><span class="sxs-lookup"><span data-stu-id="24a28-130">Request body data rate limit</span></span>

<span data-ttu-id="24a28-131">Varsayılan olarak, Kestrel sunucunun uygular bir [en az bir istek gövdesi veri hızı](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span><span class="sxs-lookup"><span data-stu-id="24a28-131">By default, the Kestrel server imposes a [minimum request body data rate](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span></span> <span data-ttu-id="24a28-132">Akış istemci ve çift yönlü çağrıları akış için bu fiyat karşılanmadı ve bağlantının zaman aşımına. En az istek gövdesi gRPC hizmet istemcisi akış ve çift yönlü çağrıları akış içerdiğinde veri hızı sınırı devre dışı bırakılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="24a28-132">For client streaming and duplex streaming calls, this rate may not be satisfied and the connection may be timed out. The minimum request body data rate limit must be disabled when the gRPC service includes client streaming and duplex streaming calls:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a><span data-ttu-id="24a28-133">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="24a28-133">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
