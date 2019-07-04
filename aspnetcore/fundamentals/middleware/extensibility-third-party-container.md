---
title: Bir üçüncü taraf kapsayıcısında ASP.NET Core ile Ara yazılım etkinleştirme
author: guardrex
description: Kesin türü belirtilmiş bir ara yazılım ile Fabrika tabanlı etkinleştirme ve üçüncü taraf kapsayıcı içinde ASP.NET Core kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/03/2019
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: 4bc99b4c336aba611287c9fbe03d4252f8abee5b
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561643"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>Bir üçüncü taraf kapsayıcısında ASP.NET Core ile Ara yazılım etkinleştirme

Tarafından [Luke Latham](https://github.com/guardrex)

Bu makalede nasıl yapılacağı açıklanır <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> ve <xref:Microsoft.AspNetCore.Http.IMiddleware> için genişletilebilirlik noktası olarak [ara yazılım](xref:fundamentals/middleware/index) bir üçüncü taraf kapsayıcı ile etkinleştirme. Hakkında tanıtıcı bilgi `IMiddlewareFactory` ve `IMiddleware`, bkz: <xref:fundamentals/middleware/extensibility>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulama tarafından ara yazılım etkinleştirme gösterir bir `IMiddlewareFactory` uygulaması `SimpleInjectorMiddlewareFactory`. Örnek kullanır [basit Enjektörü](https://simpleinjector.org) bağımlılık ekleme (dı) kapsayıcı.

Bir sorgu dizesi parametresi tarafından sağlanan değer örnek'ın Ara yazılım uygulama kayıtları (`key`). Ara yazılım, sorgu dizesi değerini bir bellek içi veritabanına kaydetmek için bir eklenen veritabanı bağlamı (kapsamlı bir hizmet) kullanır.

> [!NOTE]
> Örnek uygulama kullandığı [basit Enjektörü](https://github.com/simpleinjector/SimpleInjector) yalnızca tanıtım amacıyla. Kullanımı basit Enjektörü bir onay yok. Ara yazılım etkinleştirme yaklaşım basit Enjektörü belgelerinde açıklanan ve GitHub sorunları, basit Enjektörü maintainers tarafından önerilir. Daha fazla bilgi için [basit Enjektörü belgeleri](https://simpleinjector.readthedocs.io/en/latest/index.html) ve [basit Enjektörü GitHub deposu](https://github.com/simpleinjector/SimpleInjector).

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> Ara yazılım oluşturmak için yöntemleri sağlar.

Örnek uygulamada, bir ara yazılım fabrikası oluşturmak için uygulanan bir `SimpleInjectorActivatedMiddleware` örneği. Ara yazılım Fabrika, ara yazılım çözümlemek için basit Enjektörü kapsayıcı kullanır:

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>IMiddleware

<xref:Microsoft.AspNetCore.Http.IMiddleware> uygulamanın isteği ardışık düzeni için ara yazılımı tanımlar.

Ara yazılım etkinleştirilmiş olarak bir `IMiddlewareFactory` uygulaması (*Middleware/SimpleInjectorActivatedMiddleware.cs*):

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

Uzantı ara yazılımı için oluşturulan (*Middleware/MiddlewareExtensions.cs*):

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` çeşitli görevleri gerçekleştirmeniz gerekir:

* Basit Enjektörü kapsayıcısı ayarlamak ayarlayın.
* Ara yazılım ve üretici kaydedin.
* Uygulamanın veritabanı bağlamı basit Enjektörü kapsayıcısından kullanılabilir duruma getirin.

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Ara yazılım istek işleme ardışık düzeninde kayıtlı `Startup.Configure`:

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Fabrika tabanlı ara yazılım etkinleştirme](xref:fundamentals/middleware/extensibility)
* [Basit Enjektörü GitHub deposu](https://github.com/simpleinjector/SimpleInjector)
* [Basit Enjektörü belgeleri](https://simpleinjector.readthedocs.io/en/latest/index.html)
