---
title: gRPC ASP.NET Core yapılandırma
author: jamesnk
description: GRPC ASP.NET Core uygulamaları için yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 5/30/2019
uid: grpc/configuration
ms.openlocfilehash: 1f8250dc9aa8b82da384ee28287011baa19dc11f
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491238"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC ASP.NET Core yapılandırma

## <a name="configure-services-options"></a>Hizmetleri seçeneklerini yapılandırma

Aşağıdaki tabloda, gRPC hizmetleri yapılandırmak için seçenekleri tanımlar:

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | Sunucudan gönderilen bayt cinsinden en büyük ileti boyutu. Bir özel durum yapılandırılan en büyük ileti boyutu sonuçlarında aşan bir ileti gönderilmeye çalışılıyor. |
| `ReceiveMaxMessageSize` | 4 MB | Sunucu tarafından alınan bayt cinsinden en büyük ileti boyutu. Sunucu bu sınırı aşan bir ileti alır bir özel durum oluşturur. Bu değeri artırmak, daha büyük iletileri almasına olanak sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir. |
| `EnableDetailedErrors` | `false` | Varsa `true`, ayrıntılı bir hizmet yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür. Varsayılan, `false` değeridir. Ayarı `EnableDetailedErrors` için `true` hassas bilgileri sızabilir. |
| `CompressionProviders` | Gzip | Sıkıştırmak ve iletileri genişletmek için kullanılan sıkıştırmasını sağlayıcıları koleksiyonu. Özel sıkıştırmasını sağlayıcıları, oluşturulur ve koleksiyona eklenir. Varsayılan sağlayıcı destekler yapılandırılmış **gzip** sıkıştırma. |
| `ResponseCompressionAlgorithm` | `null` | Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma algoritması. Algoritma sıkıştırma sağlayıcı eşleşmelidir `CompressionProviders`. Algoritma yanıt sıkıştırmak istemci göndererek algoritmanın desteklediği belirtmelidir **grpc-kabul-encoding** başlığı. |
| `ResponseCompressionLevel` | `null` | Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma düzeyi. |

Seçenekler yapılandırılabilir tüm hizmetler için bir seçenekler temsilciye sağlayarak `AddGrpc` Çağır `Startup.ConfigureServices`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Tek bir hizmet seçeneklerini geçersiz kılmak, sağlanan genel seçenekleri `AddGrpc` ve kullanılarak yapılandırılabilir `AddServiceOptions<TService>`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>İstemci seçeneklerini yapılandırın

Aşağıdaki kod, ayarlar istemci en büyük gönderme ve alma ileti boyutu:

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
