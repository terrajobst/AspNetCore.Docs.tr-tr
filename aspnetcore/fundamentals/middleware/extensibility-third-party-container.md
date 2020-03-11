---
title: ASP.NET Core bir üçüncü taraf kapsayıcısı ile ara yazılım etkinleştirme
author: rick-anderson
description: Fabrika tabanlı etkinleştirme ve ASP.NET Core bir üçüncü taraf kapsayıcısı ile kesin olarak belirlenmiş bir ara yazılımı nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: a5c5bf6dff6ef795add075df932dd625129ef793
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663137"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>ASP.NET Core bir üçüncü taraf kapsayıcısı ile ara yazılım etkinleştirme

::: moniker range=">= aspnetcore-3.0"

Bu makalede, <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> ve <xref:Microsoft.AspNetCore.Http.IMiddleware> bir üçüncü taraf kapsayıcısı ile [Ara yazılım](xref:fundamentals/middleware/index) etkinleştirme için bir genişletilebilirlik noktası olarak nasıl kullanılacağı gösterilmektedir. `IMiddlewareFactory` ve `IMiddleware`hakkında giriş bilgileri için bkz. <xref:fundamentals/middleware/extensibility>.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Örnek uygulama, `SimpleInjectorMiddlewareFactory``IMiddlewareFactory` bir uygulama tarafından ara yazılım etkinleştirmesini gösterir. Örnek, [basit Injector](https://simpleinjector.org) bağımlılık ekleme (dı) kapsayıcısını kullanır.

Örneğin ara yazılım uygulamasının bir sorgu dizesi parametresi (`key`) tarafından belirtilen değeri kaydeder. Ara yazılım, sorgu dizesi değerini bellek içi bir veritabanına kaydetmek için eklenen bir veritabanı bağlamını (kapsamlı bir hizmet) kullanır.

> [!NOTE]
> Örnek uygulama, yalnızca tanıtım amacıyla [basit Injector](https://github.com/simpleinjector/SimpleInjector) kullanır. Basit Injector kullanımı bir onay değildir. Basit Injector belgelerinde açıklanan ara yazılım etkinleştirme yaklaşımları ve GitHub sorunları, basit Injector 'ın sürdürülebilir tarafından önerilir. Daha fazla bilgi için bkz. [basit Injector belgeleri](https://simpleinjector.readthedocs.io/en/latest/index.html) ve [basit injecgithub deposu](https://github.com/simpleinjector/SimpleInjector).

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>, ara yazılım oluşturmak için yöntemler sağlar.

Örnek uygulamada, bir `SimpleInjectorActivatedMiddleware` örneği oluşturmak için bir ara yazılım fabrikası uygulanır. Ara yazılım fabrikası, ara yazılımı çözümlemek için basit Injector kapsayıcısını kullanır:

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>Imıddliware

<xref:Microsoft.AspNetCore.Http.IMiddleware>, uygulamanın istek ardışık düzeni için ara yazılımı tanımlar.

`IMiddlewareFactory` bir uygulama tarafından etkinleştirilen ara yazılım (*Ara yazılım/Simpleınjectoractivatedara yazılım. cs*):

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

Ara yazılım (*Ara yazılım/MiddlewareExtensions. cs*) için bir uzantı oluşturulur:

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` birkaç görevi gerçekleştirmelidir:

* Basit Injector kapsayıcısını ayarlayın.
* Fabrika ve ara yazılımı kaydedin.
* Uygulamanın veritabanı bağlamını basit ınjtor kapsayıcısından kullanılabilir hale getirin.

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

Ara yazılım `Startup.Configure`içindeki istek işleme ardışık düzenine kaydedilir:

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Startup.cs?name=snippet2&highlight=12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu makalede, <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> ve <xref:Microsoft.AspNetCore.Http.IMiddleware> bir üçüncü taraf kapsayıcısı ile [Ara yazılım](xref:fundamentals/middleware/index) etkinleştirme için bir genişletilebilirlik noktası olarak nasıl kullanılacağı gösterilmektedir. `IMiddlewareFactory` ve `IMiddleware`hakkında giriş bilgileri için bkz. <xref:fundamentals/middleware/extensibility>.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Örnek uygulama, `SimpleInjectorMiddlewareFactory``IMiddlewareFactory` bir uygulama tarafından ara yazılım etkinleştirmesini gösterir. Örnek, [basit Injector](https://simpleinjector.org) bağımlılık ekleme (dı) kapsayıcısını kullanır.

Örneğin ara yazılım uygulamasının bir sorgu dizesi parametresi (`key`) tarafından belirtilen değeri kaydeder. Ara yazılım, sorgu dizesi değerini bellek içi bir veritabanına kaydetmek için eklenen bir veritabanı bağlamını (kapsamlı bir hizmet) kullanır.

> [!NOTE]
> Örnek uygulama, yalnızca tanıtım amacıyla [basit Injector](https://github.com/simpleinjector/SimpleInjector) kullanır. Basit Injector kullanımı bir onay değildir. Basit Injector belgelerinde açıklanan ara yazılım etkinleştirme yaklaşımları ve GitHub sorunları, basit Injector 'ın sürdürülebilir tarafından önerilir. Daha fazla bilgi için bkz. [basit Injector belgeleri](https://simpleinjector.readthedocs.io/en/latest/index.html) ve [basit injecgithub deposu](https://github.com/simpleinjector/SimpleInjector).

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>, ara yazılım oluşturmak için yöntemler sağlar.

Örnek uygulamada, bir `SimpleInjectorActivatedMiddleware` örneği oluşturmak için bir ara yazılım fabrikası uygulanır. Ara yazılım fabrikası, ara yazılımı çözümlemek için basit Injector kapsayıcısını kullanır:

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>Imıddliware

<xref:Microsoft.AspNetCore.Http.IMiddleware>, uygulamanın istek ardışık düzeni için ara yazılımı tanımlar.

`IMiddlewareFactory` bir uygulama tarafından etkinleştirilen ara yazılım (*Ara yazılım/Simpleınjectoractivatedara yazılım. cs*):

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

Ara yazılım (*Ara yazılım/MiddlewareExtensions. cs*) için bir uzantı oluşturulur:

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` birkaç görevi gerçekleştirmelidir:

* Basit Injector kapsayıcısını ayarlayın.
* Fabrika ve ara yazılımı kaydedin.
* Uygulamanın veritabanı bağlamını basit ınjtor kapsayıcısından kullanılabilir hale getirin.

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Ara yazılım `Startup.Configure`içindeki istek işleme ardışık düzenine kaydedilir:

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet2&highlight=12)]

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Fabrika tabanlı ara yazılım etkinleştirme](xref:fundamentals/middleware/extensibility)
* [Basit ınjecgithub deposu](https://github.com/simpleinjector/SimpleInjector)
* [Basit Injector belgeleri](https://simpleinjector.readthedocs.io/en/latest/index.html)
