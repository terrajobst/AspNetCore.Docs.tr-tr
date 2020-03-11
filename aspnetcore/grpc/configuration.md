---
title: .NET için gRPC yapılandırması
author: jamesnk
description: GRPC 'yi .NET uygulamaları için nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 02/26/2020
uid: grpc/configuration
ms.openlocfilehash: cabe2d86f535bf3063dd7ede9e8a3bc5de70e244
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666903"
---
# <a name="grpc-for-net-configuration"></a>.NET için gRPC yapılandırması

## <a name="configure-services-options"></a>Hizmet seçeneklerini yapılandırma

gRPC Hizmetleri, *Startup.cs*içinde `AddGrpc` ile yapılandırılır. Aşağıdaki tabloda, gRPC hizmetlerini yapılandırma seçenekleri açıklanmaktadır:

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| MaxSendMessageSize | `null` | Sunucudan gönderilebilecek en büyük ileti boyutu (bayt). Yapılandırılan en büyük ileti boyutunu aşan bir ileti gönderilmeye çalışılıyor, bir özel durumla sonuçlanır. |
| MaxReceiveMessageSize | 4 MB | Sunucu tarafından alınabilecek, bayt olarak en büyük ileti boyutu. Sunucu bu sınırı aşan bir ileti alırsa bir özel durum oluşturur. Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir. |
| EnableDetailedErrors | `false` | `true`, bir hizmet yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür. Varsayılan değer: `false`. `EnableDetailedErrors` `true` olarak ayarlamak, hassas bilgileri sızdırabilir. |
| CompressionProviders | gzip | İletileri sıkıştırmak ve açmak için kullanılan bir sıkıştırma sağlayıcıları koleksiyonu. Özel sıkıştırma sağlayıcıları oluşturulup koleksiyona eklenebilir. Varsayılan yapılandırılmış sağlayıcılar **gzip** sıkıştırmasını destekler. |
| <span style="word-break:normal;word-wrap:normal">ResponseCompressionAlgorithm</span> | `null` | Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma algoritması. Algoritmanın `CompressionProviders`bir sıkıştırma sağlayıcısıyla eşleşmesi gerekir. Bir yanıtı sıkıştırmaya yönelik algoritma için, istemci, **GRPC-Accept-Encoding** üstbilgisine göndererek algoritmayı desteklediğini göstermelidir. |
| ResponseCompressionLevel | `null` | Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma düzeyi. |
| Durdurucular | Yok | Her gRPC çağrısıyla çalıştırılan bir dinleyici koleksiyonu. Yakalayıcılar kayıtlı oldukları sırada çalıştırılır. Küresel olarak yapılandırılan yakalayıcılar, tek bir hizmet için yapılandırmadan önce çalıştırılır. GRPC yakalayıcılar hakkında daha fazla bilgi için bkz. [GRPC yakalayıcılar Ile ara yazılım karşılaştırması](xref:grpc/migration#grpc-interceptors-vs-middleware). |

Seçenekler, `Startup.ConfigureServices``AddGrpc` çağrısına bir seçenek temsilcisi sağlayarak tüm hizmetler için yapılandırılabilir:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Tek bir hizmetin seçenekleri `AddGrpc` için belirtilen genel seçenekleri geçersiz kılar ve `AddServiceOptions<TService>`kullanılarak yapılandırılabilir:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>İstemci seçeneklerini yapılandırma

gRPC istemci yapılandırması `GrpcChannelOptions`ayarlanır. Aşağıdaki tabloda, gRPC kanallarını yapılandırma seçenekleri açıklanmaktadır:

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| HttpClient | Yeni örnek | GRPC çağrıları yapmak için kullanılan `HttpClient`. İstemci, özel bir `HttpClientHandler`yapılandırmak ya da gRPC çağrıları için HTTP işlem hattına ek işleyiciler eklemek üzere ayarlanabilir. `HttpClient` belirtilmemişse, kanal için yeni bir `HttpClient` örneği oluşturulur. Otomatik olarak elden kaldırılacaktır. |
| DisposeHttpClient | `false` | `true`ve bir `HttpClient` belirtilmişse, `GrpcChannel` bırakıldığında `HttpClient` örneği de bırakılır. |
| LoggerFactory | `null` | İstemci tarafından gRPC çağrıları hakkındaki bilgileri günlüğe kaydetmek için kullanılan `LoggerFactory`. `LoggerFactory` bir örnek, `LoggerFactory.Create`kullanılarak bağımlılık ekleme veya oluşturma işleminden çözülebilir. Günlüğe kaydetmeyi yapılandırma örnekleri için bkz. <xref:grpc/diagnostics#grpc-client-logging>. |
| MaxSendMessageSize | `null` | İstemciden gönderilebilecek en büyük ileti boyutu (bayt). Yapılandırılan en büyük ileti boyutunu aşan bir ileti gönderilmeye çalışılıyor, bir özel durumla sonuçlanır. |
| <span style="word-break:normal;word-wrap:normal">MaxReceiveMessageSize</span> | 4 MB | İstemci tarafından alınabilecek, bayt olarak en büyük ileti boyutu. İstemci bu sınırı aşan bir ileti alırsa bir özel durum oluşturur. Bu değeri artırmak, istemcinin daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir. |
| Kimlik Bilgileri | `null` | Bir `ChannelCredentials` örneği. Kimlik bilgileri, gRPC çağrılarına kimlik doğrulama meta verileri eklemek için kullanılır. |
| CompressionProviders | gzip | İletileri sıkıştırmak ve açmak için kullanılan bir sıkıştırma sağlayıcıları koleksiyonu. Özel sıkıştırma sağlayıcıları oluşturulup koleksiyona eklenebilir. Varsayılan yapılandırılmış sağlayıcılar **gzip** sıkıştırmasını destekler. |

Aşağıdaki kod:

* Kanalda en büyük gönderme ve alma iletisi boyutunu ayarlar.
* İstemci oluşturur.

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/diagnostics>
* <xref:tutorials/grpc/grpc-start>
