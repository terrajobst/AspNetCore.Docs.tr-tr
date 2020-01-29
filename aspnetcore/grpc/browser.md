---
title: tarayıcı uygulamalarında gRPC
author: jamesnk
description: ASP.NET Core GRPC hizmetlerini, gRPC-Web kullanan tarayıcı uygulamalarından çağrılabilir olacak şekilde nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/24/2020
uid: grpc/browser
ms.openlocfilehash: 6359c3b76b3cb1ba2b6d9f9a989f64cbf4c4379d
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76830662"
---
# <a name="grpc-in-browser-apps"></a>tarayıcı uygulamalarında gRPC

, [James bAyKiNg](https://twitter.com/jamesnk)

> [!IMPORTANT]
> **gRPC-.NET 'teki Web desteği deneysel**
>
> gRPC-.NET için Web, kaydedilmiş bir ürün değil, deneysel bir projem projem. Şunları yapmak istiyoruz:
>
> * GRPC-Web ' i uygulamaya yönelik yaklaşımımızı test edin.
> * Bu yaklaşım .NET geliştiricileri için, bir ara sunucu aracılığıyla gRPC-Web ayarlamanın geleneksel bir yolu ile karşılaştırıldığında yararlı olup olmadığını hakkında geri bildirim alın.
>
> Bu geliştiricilerin, ve ile üretken olduğu bir şey oluşturduğumuzun olduğundan emin olmak için [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) lütfen geri bildirimde bulunun.

Tarayıcı tabanlı bir uygulamadan HTTP/2 gRPC hizmetini çağırmak mümkün değildir. [GRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) , tarayıcı JavaScript ve Blazor uygulamalarının GRPC hizmetlerini çağırmasını sağlayan bir protokoldür. Bu makalede, .NET Core 'da gRPC-Web ' i kullanma açıklanmaktadır.

## <a name="configure-grpc-web-in-aspnet-core"></a>ASP.NET Core 'de gRPC-Web yapılandırma

ASP.NET Core 'de barındırılan gRPC Hizmetleri, gRPC-Web ' i HTTP/2 gRPC ile birlikte destekleyecek şekilde yapılandırılabilir. gRPC-Web, hizmetlerde herhangi bir değişiklik yapılmasını gerektirmez. Tek değişiklik başlangıç yapılandırması ' dır.

GRPC-Web ' i bir ASP.NET Core gRPC hizmeti ile etkinleştirmek için:

* [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) paketine bir başvuru ekleyin.
* *Startup.cs*'e `AddGrpcWeb` ve `UseGrpcWeb` ekleyerek, uygulamayı GRPC-Web kullanacak şekilde yapılandırın:

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=3,10,14)]

Yukarıdaki kod:

* Yönlendirme ve bitiş noktalarından önce `UseGrpcWeb`gRPC-Web ara yazılımını ekler.
* `endpoints.MapGrpcService<GreeterService>()` yönteminin `EnableGrpcWeb`ile gRPC-Web 'i desteklediğini belirtir. 

Alternatif olarak, tüm hizmetleri, bir `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` ConfigureServices 'e ekleyerek gRPC-Web ' i destekleyecek şekilde yapılandırın.

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=5,12,16)]

ASP.NET Core CORS 'yi destekleyecek şekilde yapılandırmak gibi, tarayıcıdan gRPC-Web ' i çağırmak için bazı ek yapılandırmalar gerekebilir. Daha fazla bilgi için bkz. [cors desteği](xref:security/cors).

## <a name="call-grpc-web-from-the-browser"></a>Tarayıcıdan gRPC-Web 'i çağırma

Tarayıcı uygulamaları GRPC hizmetlerini çağırmak için gRPC-Web kullanabilir. GRPC hizmetlerini tarayıcıda GRPC-Web ile çağırırken bazı gereksinimler ve sınırlamalar vardır:

* Sunucu, gRPC-Web ' i destekleyecek şekilde yapılandırılmış olmalıdır.
* İstemci akışı ve çift yönlü akış çağrıları desteklenmez. Sunucu akışı destekleniyor.
* Farklı bir etki alanında gRPC hizmetlerinin çağrılması için [CORS](xref:security/cors) 'nin sunucuda yapılandırılması gerekir.

### <a name="javascript-grpc-web-client"></a>JavaScript gRPC-Web istemcisi

Bir JavaScript gRPC-Web istemcisi vardır. JavaScript 'ten gRPC-Web kullanma hakkında yönergeler için bkz. [gRPC-web Ile JavaScript istemci kodu yazma](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).

### <a name="configure-grpc-web-with-the-net-grpc-client"></a>.NET gRPC istemcisiyle gRPC-Web yapılandırma

.NET gRPC istemcisi, gRPC-Web çağrıları yapmak için yapılandırılabilir. Bu, tarayıcıda barındırılan ve JavaScript kodunda aynı HTTP kısıtlamalarına sahip olan [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) uygulamaları için yararlıdır. Bir .NET istemcisiyle gRPC-Web ' i çağırmak [http/2 gRPC ile aynıdır](xref:grpc/client). Tek değişiklik, kanalın oluşturulma şekli olur.

GRPC-Web kullanmak için:

* [GRPC .net. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) paketine başvuru ekleyin.
* Kanalı `GrpcWebHandler`kullanacak şekilde yapılandırın:

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

Yukarıdaki kod:

* GRPC-Web kullanmak için bir kanal yapılandırır.
* Bir istemci oluşturur ve kanalı kullanarak bir çağrı yapar.

`GrpcWebHandler` oluşturulduğunda aşağıdaki yapılandırma seçeneklerine sahiptir:

* **InnerHandler**: http çağrısını yapan temel <xref:System.Net.Http.HttpMessageHandler>, örneğin, `HttpClientHandler`.
* **Mod**: enum `GrpcWebMode`. `GrpcWebMode.GrpcWebText`, içeriği Base64 kodlamalı olacak şekilde yapılandırır, bu da sunucu akış çağrılarını desteklemek için gereklidir.
* **HttpVersion**: HTTP protokol `Version`. gRPC-Web belirli bir protokol gerektirmez ve yapılandırılmadığı takdirde bir istek yapıldığında bir tane belirtmez.

## <a name="additional-resources"></a>Ek kaynaklar

* [Web Istemcileri için gRPC GitHub projesi](https://github.com/grpc/grpc-web)
* <xref:security/cors>
