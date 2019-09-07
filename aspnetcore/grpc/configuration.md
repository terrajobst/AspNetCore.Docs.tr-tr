---
title: ASP.NET Core yapılandırma için gRPC
author: jamesnk
description: GRPC 'yi ASP.NET Core uygulamalar için nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 09/05/2019
uid: grpc/configuration
ms.openlocfilehash: d6f095820271a3bb07e05e29299fbb82b042983b
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773680"
---
# <a name="grpc-for-aspnet-core-configuration"></a>ASP.NET Core yapılandırma için gRPC

## <a name="configure-services-options"></a>Hizmet seçeneklerini yapılandırma

Aşağıdaki tabloda, gRPC hizmetlerini yapılandırma seçenekleri açıklanmaktadır:

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | Sunucudan gönderilebilecek en büyük ileti boyutu (bayt). Yapılandırılan en büyük ileti boyutunu aşan bir ileti gönderilmeye çalışılıyor, bir özel durumla sonuçlanır. |
| `MaxReceiveMessageSize` | 4 MB | Sunucu tarafından alınabilecek, bayt olarak en büyük ileti boyutu. Sunucu bu sınırı aşan bir ileti alırsa bir özel durum oluşturur. Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir. |
| `EnableDetailedErrors` | `false` | İse `true`, bir hizmet yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür. Varsayılan, `false` değeridir. `EnableDetailedErrors` İçin`true` ayarı, hassas bilgileri sızdırabilir. |
| `CompressionProviders` | gzip | İletileri sıkıştırmak ve açmak için kullanılan bir sıkıştırma sağlayıcıları koleksiyonu. Özel sıkıştırma sağlayıcıları oluşturulup koleksiyona eklenebilir. Varsayılan yapılandırılmış sağlayıcılar **gzip** sıkıştırmasını destekler. |
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
| `HttpClient` | Yeni örnek | GRPC çağrısı yapmak için kullanılır.`HttpClient` İstemci, özel `HttpClientHandler`yapılandırmak üzere ayarlanabilir veya GRPC çağrılarına yönelik http işlem hattına ek işleyiciler ekleyebilir. Hayır `HttpClient` belirtilmişse kanal için yeni `HttpClient` bir örnek oluşturulur. Otomatik olarak elden kaldırılacaktır. |
| `DisposeHttpClient` | `false` | , Ve bir `HttpClient` belirtilmişse, `HttpClient` ,, bırakıldığında `GrpcChannel` örnek de silinir. `true` |
| `LoggerFactory` | `null` | İstemci tarafından GRPC çağrıları hakkındaki bilgileri günlüğe kaydetmek için kullanılır.`LoggerFactory` Örnek, kullanılarak `LoggerFactory.Create`bağımlılık ekleme veya oluşturma öğesinden çözülebilir. `LoggerFactory` Günlüğe kaydetmeyi yapılandırma örnekleri için bkz <xref:fundamentals/logging/index>. |
| `MaxSendMessageSize` | `null` | İstemciden gönderilebilecek en büyük ileti boyutu (bayt). Yapılandırılan en büyük ileti boyutunu aşan bir ileti gönderilmeye çalışılıyor, bir özel durumla sonuçlanır. |
| `MaxReceiveMessageSize` | 4 MB | İstemci tarafından alınabilecek, bayt olarak en büyük ileti boyutu. İstemci bu sınırı aşan bir ileti alırsa bir özel durum oluşturur. Bu değeri artırmak, istemcinin daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir. |
| `Credentials` | `null` | Bir `ChannelCredentials` örnek. Kimlik bilgileri, gRPC çağrılarına kimlik doğrulama meta verileri eklemek için kullanılır. |
| `CompressionProviders` | gzip | İletileri sıkıştırmak ve açmak için kullanılan bir sıkıştırma sağlayıcıları koleksiyonu. Özel sıkıştırma sağlayıcıları oluşturulup koleksiyona eklenebilir. Varsayılan yapılandırılmış sağlayıcılar **gzip** sıkıştırmasını destekler. |

Aşağıdaki kod:

* Kanalda en büyük gönderme ve alma iletisi boyutunu ayarlar.
* İstemci oluşturur.

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:tutorials/grpc/grpc-start>
