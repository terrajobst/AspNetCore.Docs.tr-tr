---
title: "ASP.NET Core üçüncü taraf kapsayıcısında ile Ara yazılım etkinleştirme"
author: guardrex
description: "Kesin türü belirtilmiş ara yazılımı Fabrika tabanlı etkinleştirme ve bir üçüncü taraf kapsayıcı ASP.NET çekirdek kullanmayı öğrenin."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 02/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: b6fd02d1863efe571bafe1344fb34939883e6cb5
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>ASP.NET Core üçüncü taraf kapsayıcısında ile Ara yazılım etkinleştirme

Tarafından [Luke Latham](https://github.com/guardrex)

Bu makalede nasıl kullanılacağı gösterilmektedir [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) ve [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) için genişletilebilirlik noktası olarak [ara yazılımı](xref:fundamentals/middleware/index) bir üçüncü taraf kapsayıcısı ile etkinleştirme. Hakkında tanıtıcı bilgi `IMiddlewareFactory` ve `IMiddleware`, bkz: [ara yazılımı Fabrika tabanlı etkinleştirme](xref:fundamentals/middleware/extensibility) konu.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek uygulama tarafından ara yazılım etkinleştirme gösterir bir `IMiddlewareFactory` uygulaması, `SimpleInjectorMiddlewareFactory`. Örnek kullanır [basit Injector](https://simpleinjector.org) bağımlılık ekleme (dı) kapsayıcı.

Bir sorgu dizesi parametresi tarafından sağlanan değer örnek 's Ara uygulama kaydeder (`key`). Ara yazılım, sorgu dizesi değerini bir bellek içi veritabanına kaydetmek için eklenen veritabanı bağlamı (kapsamlı bir hizmeti) kullanır.

> [!NOTE]
> Örnek uygulama kullandığı [basit Injector](https://github.com/simpleinjector/SimpleInjector) yalnızca tanıtım amacıyla. Basit Injector kullanımını bir onay değil. Ara yazılım etkinleştirme yaklaşım basit Injector belgelerinde açıklanan ve GitHub sorunları basit Injector maintainers tarafından önerilir. Daha fazla bilgi için bkz: [basit Injector belgelerine](https://simpleinjector.readthedocs.io/en/latest/index.html) ve [basit Injector GitHub deposunu](https://github.com/simpleinjector/SimpleInjector).

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) ara oluşturmak için yöntemleri sağlar.

Örnek uygulaması oluşturmak için bir ara yazılım fabrikası uygulanan bir `SimpleInjectorActivatedMiddleware` örneği. Ara yazılım Fabrika ara yazılım çözümlemek için basit Injector kapsayıcı kullanır:

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulamanın isteği ardışık düzeni için ara yazılımı tanımlar.

Ara yazılım tarafından etkinleştirilen bir `IMiddlewareFactory` uygulaması (*Middleware/SimpleInjectorActivatedMiddleware.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

Uzantı ara yazılımı için oluşturulan (*Middleware/MiddlewareExtensions.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` çeşitli görevleri gerçekleştirmeniz gerekir:

* Basit Injector kapsayıcısı ayarlama ayarlayın.
* Fabrika ve ara yazılımın kaydedin.
* Uygulamanın veritabanı bağlamı basit Injector kapsayıcı için bir Razor sayfası kullanımına.

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

Ara yazılım istek işleme ardışık düzeninde kayıtlı `Startup.Configure`:

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=12)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Ara yazılımı Fabrika tabanlı etkinleştirme](xref:fundamentals/middleware/extensibility)
* [Basit Injector GitHub deposunu](https://github.com/simpleinjector/SimpleInjector)
* [Basit Injector belgeleri](https://simpleinjector.readthedocs.io/en/latest/index.html)
