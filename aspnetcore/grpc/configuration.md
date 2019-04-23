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
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59984525"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC ASP.NET Core yapılandırma

## <a name="configure-services-options"></a>Hizmetleri seçeneklerini yapılandırma

Aşağıdaki tabloda, gRPC hizmetleri yapılandırmak için seçenekleri tanımlar:

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | Sunucudan gönderilen bayt cinsinden en büyük ileti boyutu. Bir özel durum yapılandırılan en büyük ileti boyutu sonuçlarında aşan bir ileti gönderilmeye çalışılıyor. |
| `ReceiveMaxMessageSize` | 4 MB | Sunucu tarafından alınan bayt cinsinden en büyük ileti boyutu. Sunucu bu sınırı aşan bir ileti alır bir özel durum oluşturur. Bu değeri artırmak, daha büyük iletileri almasına olanak sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir. |
| `EnableDetailedErrors` | `false` | Varsa `true`, ayrıntılı bir hizmet yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür. Varsayılan, `false` değeridir. Bu ayar `true` hassas bilgileri sızabilir. |
| `CompressionProviders` | Gzip | Sıkıştırmak ve iletileri genişletmek için kullanılan sıkıştırmasını sağlayıcıları koleksiyonu. Özel sıkıştırmasını sağlayıcıları, oluşturulur ve koleksiyona eklenir. Varsayılan sağlayıcı destekler yapılandırılmış **gzip** sıkıştırma. |
| `ResponseCompressionAlgorithm` | `null` | Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma algoritması. Algoritma sıkıştırma sağlayıcı eşleşmelidir `CompressionProviders`. Algorthm yanıt sıkıştırmak istemciye göndererek algoritmanın desteklediği belirtmeniz gerekir **grpc-kabul-encoding** başlığı. |
| `ResponseCompressionLevel` | `null` | Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma düzeyi. |

Seçenekler yapılandırılabilir tüm hizmetler için bir seçenekler temsilciye sağlayarak `AddGrpc` Çağır `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

Tek bir hizmet seçeneklerini geçersiz kılmak, sağlanan genel seçenekleri `AddGrpc` ve kullanılarak yapılandırılabilir `AddServiceOptions<TService>`:

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a>Kestrel'i seçeneklerini yapılandırın

Kestrel'i sunucu için ASP.NET gRPC davranışını etkileyen yapılandırma seçenekleri vardır.

### <a name="request-body-data-rate-limit"></a>İstek gövdesi veri hızı sınırı

Varsayılan olarak, Kestrel sunucunun uygular bir [en az bir istek gövdesi veri hızı](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>). Akış istemci ve çift yönlü çağrıları akış için bu fiyat karşılanmadı ve bağlantının zaman aşımına. En az istek gövdesi gRPC hizmet istemcisi akış ve çift yönlü çağrıları akış içerdiğinde veri hızı sınırı devre dışı bırakılmalıdır:

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

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
