---
title: ASP.NET Core için gRPC 'de güvenlik konuları
author: jamesnk
description: ASP.NET Core için gRPC güvenlik konuları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 07/07/2019
uid: grpc/security
ms.openlocfilehash: 4a70cb16d8397dbc69a626435fb72a0512788b14
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308811"
---
# <a name="security-considerations-in-grpc-for-aspnet-core"></a>ASP.NET Core için gRPC 'de güvenlik konuları

, [James bAyKiNg](https://twitter.com/jamesnk)

Bu makalede, gRPC 'yi .NET Core ile güvenli hale getirme hakkında bilgi verilmektedir.

## <a name="transport-security"></a>Taşıma güvenliği

gRPC iletileri HTTP/2 kullanılarak gönderilir ve alınır. Şunları öneririz:

* [Aktarım Katmanı Güvenliği (TLS)](https://tools.ietf.org/html/rfc5246) , üretim GRPC uygulamalarında iletileri güvenli hale getirmek için kullanılır.
* gRPC Hizmetleri yalnızca güvenli bağlantı noktalarını dinler ve bunlara yanıt vermelidir.

TLS, Kestrel içinde yapılandırılır. Kestrel uç noktalarını yapılandırma hakkında daha fazla bilgi için bkz. [Kestrel Endpoint Configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).

## <a name="exceptions"></a>Özel Durumlar

Özel durum iletileri genellikle bir istemciye görüntülenmemelidir gizli veriler olarak değerlendirilir. Varsayılan olarak, gRPC, bir gRPC hizmeti tarafından oluşturulan özel durumun ayrıntılarını istemciye göndermez. Bunun yerine, istemci bir hata oluştuğunu belirten genel bir ileti alır. İstemciye özel durum iletisi teslimi, [Enabledetailederrors](xref:grpc/configuration#configure-services-options)ile geçersiz kılınabilir (örneğin, geliştirme veya test). Özel durum iletileri, üretim uygulamalarındaki istemciye gösterilmemelidir.

## <a name="message-size-limits"></a>İleti boyutu sınırları

GRPC istemcilerine ve hizmetlerine gelen iletiler belleğe yüklenir. İleti boyutu sınırları, gRPC 'nin aşırı kaynak tüketmesini önlemeye yardımcı olmak için bir mekanizmadır.

gRPC gelen ve giden iletileri yönetmek için ileti başına boyut sınırlarını kullanır. Varsayılan olarak, gRPC gelen iletileri 4 MB ile sınırlandırır. Giden iletilerde sınır yoktur.

Sunucusunda, gRPC ileti limitleri bir uygulamadaki tüm hizmetler için ile `AddGrpc`yapılandırılabilir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 1 * 1024 * 1024;  // 1 megabyte
        options.SendMaxMessageSize = 1 * 1024 * 1024;     // 1 megabyte
    });
}
```

Ayrıca, kullanılarak `AddServiceOptions<TService>`tek bir hizmet için sınırlamalar da yapılandırılabilir. İleti boyutu sınırlarını yapılandırma hakkında daha fazla bilgi için bkz. [GRPC yapılandırması](xref:grpc/configuration).

## <a name="client-certificate-validation"></a>İstemci sertifikası doğrulaması

[İstemci sertifikaları](https://tools.ietf.org/html/rfc5246#section-7.4.4) , bağlantı oluşturulduğunda başlangıçta onaylanır. Varsayılan olarak, Kestrel bir bağlantının istemci sertifikası için ek doğrulama gerçekleştirmez.

İstemci sertifikaları tarafından güvenliği sağlanan gRPC hizmetlerinin [Microsoft. AspNetCore. Authentication. Certificate](xref:security/authentication/certauth) paketini kullanması önerilir. ASP.NET Core sertifika kimlik doğrulaması, bir istemci sertifikasında aşağıdakiler de dahil olmak üzere ek doğrulama gerçekleştirir:

* Sertifikada geçerli bir genişletilmiş anahtar kullanımı (EKU) vardır
* Geçerlilik süresi içinde
* Sertifika iptalini denetle
