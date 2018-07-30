---
title: ASP.NET Core ile depo deseni
author: ardalis
description: ASP.NET Core uygulamanızı depo uygulama tasarım deseni uygulamayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342770"
---
# <a name="repository-pattern-with-aspnet-core"></a>ASP.NET Core ile depo deseni

Tarafından [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), ve [Luke Latham](https://github.com/guardrex)

*Depo deseni* veri erişimi soyutlama arabirimi arkasında yalıtan bir tasarım örüntüsüdür. Veritabanına bağlanma ve veri depolama nesnelerini düzenleme, arabirim uygulama tarafından sağlanan yöntemleri aracılığıyla gerçekleştirilir. Sonuç olarak, bağlantılar, komutları ve okuyucular gibi veritabanı konuları uğraşmanız kodu çağırmak için gerek yoktur.

ASP.NET Core ile depo düzeninin uygulaması aşağıdaki faydaları sağlar:

* Uygulamanın daha az karmaşık iş ve veri erişim katmanları arasında doğrudan hiçbir karşılıklı bağımlılığı olan kuruluştur.
* Kod merkezi olarak bir veya daha fazla depo tarafından yönetildiğinden veritabanı erişim kodu yeniden kullanmak daha kolaydır.
* İş etki alanı bağımsız olarak veritabanı katmanından test birimi olabilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) başlatıp film karakter adlarının bir listesini görüntülemek için havuz deseni kullanır. Uygulamanın kullandığı [Entity Framework Core](/ef/core/) ve `ApplicationDbContext` verilere nasıl erişildiği burada sınıfı, veri kalıcılığı, ancak veritabanı altyapısı için görünür değildir. Veri erişimi ve veritabanı nesneleri soyutlanır arkasındaki bir [depo](https://martinfowler.com/eaaCatalog/repository.html).

## <a name="repository-interface"></a>Depo Arabirimi

Özellikleri ve yöntemleri uygulaması için bir depo arabirimi tanımlar. Örnek uygulamada, film karakter veri deposu arabirimidir `ICharacterRepository`. `ICharacterRepository` tanımlar `ListAll` ve `Add` ile çalışması için gereken yöntemleri `Character` uygulama örnekleri:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

`Character` olarak tanımlanır:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a>Depo somut türü

Arabirim somut bir tür tarafından uygulanır. Örnek uygulamada `CharacterRepository` veritabanı bağlamı yönetir ve uygulayan `ListAll` ve `Add` yöntemleri:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a>Depo Hizmeti kaydedin

Havuz ve veritabanı bağlamı service kapsayıcısında ile kayıtlı `Startup.ConfigureServices`. Örnek uygulamada `ApplicationDbContext` uzantı yöntemine yapılan çağrı ile yapılandırılmış [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext). `ICharacterRepository` kapsamlı bir hizmet olarak kayıtlı:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a>Depo bir örneğini ekleme

Veritabanı erişimi gerekli olduğu bir sınıfta bir örnek depo yapıcı istenen ve sınıfı yöntemleri tarafından kullanmak için özel bir alan atandı. Örnek uygulamada `ICharacterRepository` için kullanılır:

* Hiçbir karakteri yoksa veritabanını doldurmak.
* Görüntü karakterlerin listesini edinin.

Çağıran kod arabirimin uygulamasıyla yalnızca nasıl etkileştiğini fark `CharacterRepository`. Kodu çağırma kullanmaz `ApplicationDbContext` doğrudan:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a>Genel depo arabirimi

Bu konu ve örnek uygulama, bir depo için her bir iş nesnesi oluşturulduğu, depo düzeninin basit uygulaması gösterilmektedir. Uygulamayı birkaç nesneleri artarsa bir *genel depo arabirimi* depo deseni uygulamak için gereken kod miktarını düşürebilir. Daha fazla bilgi için [DevIQ: depo deseni: Genel depo arabirimi](http://deviq.com/repository-pattern/).

## <a name="additional-resources"></a>Ek kaynaklar

* [DevIQ: Depo deseni](https://deviq.com/repository-pattern/)
* [Bağımlılık ekleme](xref:fundamentals/dependency-injection)
* [Görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection)
* [Denetleyicilere bağımlılık ekleme](xref:mvc/controllers/dependency-injection)
* [Gereksinim işleyicilerine bağımlılık ekleme](xref:security/authorization/dependencyinjection)
* [Tersine çevirme denetimi kapsayıcıları ve bağımlılık ekleme deseni](https://www.martinfowler.com/articles/injection.html)
