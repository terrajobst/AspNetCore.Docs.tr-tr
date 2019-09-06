---
title: ASP.NET Core yapılandırma için gRPC
author: jamesnk
description: GRPC 'yi ASP.NET Core uygulamalar için nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/21/2019
uid: grpc/configuration
ms.openlocfilehash: 34eb598211c87fbb2c68ae5e041da50d02f543f7
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310310"
---
# <a name="grpc-for-aspnet-core-configuration"></a>ASP.NET Core yapılandırma için gRPC

## <a name="configure-services-options"></a>Hizmet seçeneklerini yapılandırma

Aşağıdaki tabloda, gRPC hizmetlerini yapılandırma seçenekleri açıklanmaktadır:

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | Sunucudan gönderilebilecek en büyük ileti boyutu (bayt). Yapılandırılan en büyük ileti boyutunu aşan bir ileti gönderilmeye çalışılıyor, bir özel durumla sonuçlanır. |
| `MaxReceiveMessageSize` | 4 MB | Sunucu tarafından alınabilecek, bayt olarak en büyük ileti boyutu. Sunucu bu sınırı aşan bir ileti alırsa bir özel durum oluşturur. Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir. |
| `EnableDetailedErrors` | `false` | İse `true`, bir hizmet yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür. Varsayılan, `false` değeridir. `EnableDetailedErrors` İçin`true` ayarı, hassas bilgileri sızdırabilir. |
| `CompressionProviders` | gzip, söndür | İletileri sıkıştırmak ve açmak için kullanılan bir sıkıştırma sağlayıcıları koleksiyonu. Özel sıkıştırma sağlayıcıları oluşturulup koleksiyona eklenebilir. Varsayılan yapılandırılmış sağlayıcılar **gzip** ve **söndür** sıkıştırmayı destekler. |
| `ResponseCompressionAlgorithm` | `null` | Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma algoritması. Algoritmanın içindeki `CompressionProviders`bir sıkıştırma sağlayıcısıyla eşleşmesi gerekir. Bir yanıtı sıkıştırmaya yönelik algoritma için, istemci, **GRPC-Accept-Encoding** üstbilgisine göndererek algoritmayı desteklediğini göstermelidir. |
| `ResponseCompressionLevel` | `null` | Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma düzeyi. |

Seçenekler, `AddGrpc` içindeki `Startup.ConfigureServices`çağrıya bir seçenek temsilcisi sağlayarak tüm hizmetler için yapılandırılabilir:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Tek bir hizmetin seçenekleri ' de `AddGrpc` belirtilen genel seçenekleri geçersiz kılar ve kullanılarak `AddServiceOptions<TService>`yapılandırılabilir:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>İstemci seçeneklerini yapılandırma

Aşağıdaki tabloda, gRPC kanallarını yapılandırma seçenekleri açıklanmaktadır:

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| `HttpClient` | Yeni örnek | GRPC çağrısı yapmak için kullanılır.`HttpClient` İstemci, özel `HttpClientHandler`yapılandırmak üzere ayarlanabilir veya GRPC çağrılarına yönelik http işlem hattına ek işleyiciler ekleyebilir. Varsayılan olarak yeni `HttpClient` bir örnek oluşturulur. |
| `MaxSendMessageSize` | `null` | İstemciden gönderilebilecek en büyük ileti boyutu (bayt). Yapılandırılan en büyük ileti boyutunu aşan bir ileti gönderilmeye çalışılıyor, bir özel durumla sonuçlanır. |
| `MaxReceiveMessageSize` | 4 MB | İstemci tarafından alınabilecek, bayt olarak en büyük ileti boyutu. Sunucu bu sınırı aşan bir ileti alırsa bir özel durum oluşturur. Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir. |
| `TransportOptions` | `null` | Taşıma seçenekleri, kanalın gRPC hizmetini nasıl çağırdığı hakkında yapılandırma. Şu anda tek uygulama `HttpClientTransport` seçenekleri, GRPC tarafından `HttpClient` kullanılan seçeneği belirlemenizi sağlar. |
| `Credentials` | `null` | Bir `ChannelCredentials` örnek. Kimlik bilgileri, gRPC çağrılarına kimlik doğrulama meta verileri eklemek için kullanılır. |
| `CompressionProviders` | gzip, söndür | İletileri sıkıştırmak ve açmak için kullanılan bir sıkıştırma sağlayıcıları koleksiyonu. Özel sıkıştırma sağlayıcıları oluşturulup koleksiyona eklenebilir. Varsayılan yapılandırılmış sağlayıcılar **gzip** ve **söndür** sıkıştırmayı destekler. |

Aşağıdaki kod:

* Kanalda en büyük gönderme ve alma iletisi boyutunu ayarlar.
* İstemci oluşturur.

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:tutorials/grpc/grpc-start>
