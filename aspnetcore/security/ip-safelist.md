---
title: İstemci IP Güvenli ASP.NET Core için liste
author: damienbod
description: Uzak IP adresleri, onaylanan IP adreslerinden oluşan bir liste karşı doğrulamak için bir ara yazılım ya da eylem filtreleri yazmayı öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040125"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>İstemci IP Güvenli ASP.NET Core için liste

Tarafından [Damien Bowden](https://twitter.com/damien_bod) ve [Tom Dykstra](https://github.com/tdykstra)
 
Bu makalede, bir IP güvenli liste (beyaz liste olarak da bilinir) uygulamak için iki yolunu gösterir:

* Uzak IP adresi her isteğin denetlemek için ASP.NET Core ara yazılımı kullanarak.
* Belirli bir eylem yöntemleri için isteklerin uzak IP adresi denetlemek için ASP.NET Core eylem filtrelerini kullanarak.

Örnek uygulamayı her iki yaklaşımı da gösterir. Her durumda, bir uygulama ayarı onaylı istemci IP adreslerini içeren bir dize olarak depolanır. Ara yazılım veya filtre bir listeye dizeyi ayrıştırır ve uzak IP listesinde olup olmadığını denetler. Aksi durumda, bir HTTP 403 Yasak durum kodu döndürülür.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-safelist"></a>Güvenli liste

Listenin yapılandırılan *appsettings.json* dosya. Bu, noktalı virgülle ayrılmış listesidir ve IPv4 ve IPv6 adreslerini içermelidir.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>Ara yazılım

`Configure` Yöntemi ara yazılımı ekler ve güvenli liste dize bir oluşturucu parametresi geçirilir.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

Ara yazılım bir diziye dizeyi ayrıştırır ve dizideki uzak IP adresi arar. Uzak IP adresi bulunmazsa, ara yazılım 401 HTTP Yasak döndürür. Bu doğrulama işlemi, HTTP Get isteklerini atlanır.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Eylem filtresi

Yalnızca belirli denetleyicileri veya eylem yöntemleri için bir güvenli liste istiyorsanız, bir eyleme eylem filtresi kullanın. Örnek buradadır: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

Eylem filtresi Hizmetleri kapsayıcıya eklenir.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Filtre, bir denetleyici veya eylem yöntemi kullanılabilir.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

Filtrenin uygulandığı örnek uygulamada, `Get` yöntemi. Bunu göndererek uygulamayı test ettiğinizde bir `Get` API istek, öznitelik istemci IP adresini doğrulama. Ara yazılım, herhangi bir HTTP yöntemi ile API çağrısı yaparak test ettiğinizde, istemci IP'sini doğruluyor.

## <a name="razor-pages-filter"></a>Razor sayfaları Filtrele 

Bir Razor sayfaları uygulaması için bir güvenli liste istiyorsanız, bir Razor sayfaları filtresini kullanın. Örnek buradadır: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

MVC filtreleri koleksiyon ekleyerek bu filtre etkindir.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

Uygulamayı çalıştırın ve bir Razor sayfası istek istemci IP'sini Razor sayfaları filtre doğruluyor.

## <a name="next-steps"></a>Sonraki adımlar

[ASP.NET Core ara yazılım hakkında daha fazla bilgi](xref:fundamentals/middleware/index).
