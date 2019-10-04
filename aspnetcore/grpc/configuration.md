---
title: .NET için gRPC yapılandırması
author: jamesnk
description: GRPC 'yi .NET uygulamaları için nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 09/05/2019
uid: grpc/configuration
ms.openlocfilehash: 3ef92f10d914ef9fa3e13a7bdd5c863bab297f57
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925215"
---
# <a name="grpc-for-net-configuration"></a>.NET için gRPC yapılandırması

## <a name="configure-services-options"></a>Hizmet seçeneklerini yapılandırma

GRPC Hizmetleri, *Startup.cs*içinde `AddGrpc` ile yapılandırılır. Aşağıdaki tabloda, gRPC hizmetlerini yapılandırma seçenekleri açıklanmaktadır:

| Seçenek | Default Value | Açıklama |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | Sunucudan gönderilebilecek en büyük ileti boyutu (bayt). Yapılandırılan en büyük ileti boyutunu aşan bir ileti gönderilmeye çalışılıyor, bir özel durumla sonuçlanır. |
| `MaxReceiveMessageSize` | 4 MB | Sunucu tarafından alınabilecek, bayt olarak en büyük ileti boyutu. Sunucu bu sınırı aşan bir ileti alırsa bir özel durum oluşturur. Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir. |
| `EnableDetailedErrors` | `false` | İse `true`, bir hizmet yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür. Varsayılan, `false` değeridir. `EnableDetailedErrors` İçin`true` ayarı, hassas bilgileri sızdırabilir. |
| `CompressionProviders` | Gzip | İletileri sıkıştırmak ve açmak için kullanılan bir sıkıştırma sağlayıcıları koleksiyonu. Özel sıkıştırma sağlayıcıları oluşturulup koleksiyona eklenebilir. Varsayılan yapılandırılmış sağlayıcılar **gzip** sıkıştırmasını destekler. |
| `ResponseCompressionAlgorithm` | `null` | Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma algoritması. Algoritmanın içindeki `CompressionProviders`bir sıkıştırma sağlayıcısıyla eşleşmesi gerekir. Bir yanıtı sıkıştırmaya yönelik algoritma için, istemci, **GRPC-Accept-Encoding** üstbilgisine göndererek algoritmayı desteklediğini göstermelidir. |
| `ResponseCompressionLevel` | `null` | Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma düzeyi. |

Seçenekler, `AddGrpc` içindeki `Startup.ConfigureServices`çağrıya bir seçenek temsilcisi sağlayarak tüm hizmetler için yapılandırılabilir:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Tek bir hizmetin seçenekleri ' de `AddGrpc` belirtilen genel seçenekleri geçersiz kılar ve kullanılarak `AddServiceOptions<TService>`yapılandırılabilir:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>İstemci seçeneklerini yapılandırma

gRPC istemci yapılandırması üzerinde `GrpcChannelOptions`ayarlanır. Aşağıdaki tabloda, gRPC kanallarını yapılandırma seçenekleri açıklanmaktadır:

| Seçenek | Default Value | Açıklama |
| ------ | ------------- | ----------- |
| `HttpClient` | Yeni örnek | GRPC çağrısı yapmak için kullanılır.`HttpClient` İstemci, özel `HttpClientHandler`yapılandırmak üzere ayarlanabilir veya GRPC çağrılarına yönelik http işlem hattına ek işleyiciler ekleyebilir. Hayır `HttpClient` belirtilmişse kanal için yeni `HttpClient` bir örnek oluşturulur. Otomatik olarak elden kaldırılacaktır. |
| `DisposeHttpClient` | `false` | , Ve bir `HttpClient` belirtilmişse, `HttpClient` ,, bırakıldığında `GrpcChannel` örnek de silinir. `true` |
| `LoggerFactory` | `null` | İstemci tarafından GRPC çağrıları hakkındaki bilgileri günlüğe kaydetmek için kullanılır.`LoggerFactory` Örnek, kullanılarak `LoggerFactory.Create`bağımlılık ekleme veya oluşturma öğesinden çözülebilir. `LoggerFactory` Günlüğe kaydetmeyi yapılandırma örnekleri için bkz <xref:grpc/diagnostics#grpc-client-logging>. |
| `MaxSendMessageSize` | `null` | İstemciden gönderilebilecek en büyük ileti boyutu (bayt). Yapılandırılan en büyük ileti boyutunu aşan bir ileti gönderilmeye çalışılıyor, bir özel durumla sonuçlanır. |
| `MaxReceiveMessageSize` | 4 MB | İstemci tarafından alınabilecek, bayt olarak en büyük ileti boyutu. İstemci bu sınırı aşan bir ileti alırsa bir özel durum oluşturur. Bu değeri artırmak, istemcinin daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir. |
| `Credentials` | `null` | Bir `ChannelCredentials` örnek. Kimlik bilgileri, gRPC çağrılarına kimlik doğrulama meta verileri eklemek için kullanılır. |
| `CompressionProviders` | Gzip | İletileri sıkıştırmak ve açmak için kullanılan bir sıkıştırma sağlayıcıları koleksiyonu. Özel sıkıştırma sağlayıcıları oluşturulup koleksiyona eklenebilir. Varsayılan yapılandırılmış sağlayıcılar **gzip** sıkıştırmasını destekler. |

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
