---
title: "Denetleyicileri içine bağımlılık ekleme"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 21cffcd285879fdca81cb7d92d0f079d4bf7756c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-into-controllers"></a>Denetleyicileri içine bağımlılık ekleme

<a name="dependency-injection-controllers"></a>

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC denetleyicileri bağımlılıklarını açıkça kendi oluşturucular aracılığıyla istemeniz gerekir. Bazı durumlarda, tek tek denetleyici eylemleri hizmet gerektirebilir ve bu denetleyici düzeyinde istemek için anlamı olmayabilir. Bu durumda, bir eylem yönteminin bir parametresi olarak bir hizmet eklemesine de seçebilirsiniz.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>Bağımlılık ekleme

Bağımlılık ekleme olduğunu izleyen bir teknik [bağımlılık tersine çevirme ilkesi](http://deviq.com/dependency-inversion-principle/), geniş eşleşmiş modüllerini gibi uygulamalar için izin verme. ASP.NET Core sahip için yerleşik destek [bağımlılık ekleme](../../fundamentals/dependency-injection.md), hangi yapar uygulamaları sınama ve sürdürmek daha kolay.

## <a name="constructor-injection"></a>Oluşturucu ekleme

MVC denetleyicileri için ASP.NET Core'nın yerleşik Oluşturucusu tabanlı bağımlılık ekleme desteği genişletir. Yalnızca bir hizmet türünün denetleyicinizi Oluşturucusu parametre olarak ekleyerek, ASP.NET Core yerleşik hizmet kapsayıcısında kullanarak bu türde çözümlemeye çalışır. Hizmetleri genellikle, ancak her zaman, arabirimleri kullanılarak tanımlanır. Örneğin, geçerli zamanı bağımlı bir iş mantığı uygulamanız varsa, testlerinizi bir süre kullanmak uygulamalarında geçmesine izin saati (yerine sabit kodlama), alan bir hizmeti ekleyemezsiniz.

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


Çalışma zamanında sistem saatini kullanır, böylece arabirimi bunun gibi uygulama kısmı oldukça kolaydır:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


Bu yerinde biz hizmeti bizim denetleyicisi kullanabilir. Bu durumda, bazı mantığı ekledik `HomeController` `Index` Tebrik Kartı kullanıcıya görüntülenecek yöntemine temel günün saati.

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Biz uygulamayı şimdi çalıştırırsanız, biz büyük olasılıkla bir hatayla karşılaşırsınız:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Bir hizmet olarak yapılandırmadıysanız bu hata oluşur `ConfigureServices` yönteminde bizim `Startup` sınıfı. Yönelik isteklere belirtmek için `IDateTime` bir örneği kullanılarak çözülmelidir `SystemDateTime`, için aşağıda listesinde vurgulanan satırı ekleyin, `ConfigureServices` yöntemi:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Bu belirli bir hizmet çeşitli farklı ömrü seçeneklerden biri kullanılarak uygulanan (`Transient`, `Scoped`, veya `Singleton`). Bkz: [bağımlılık ekleme](../../fundamentals/dependency-injection.md) bu kapsam seçeneklerin her biri hizmetinizi davranışını nasıl etkileyeceğini anlamak için.

Hizmet yapılandırıldıktan sonra uygulamayı çalıştıran ve giriş sayfasına giderek zamana dayalı iletiyi beklendiği gibi görüntülenmelidir:

![Sunucu selamlama](dependency-injection/_static/server-greeting.png)

>[!TIP]
> Bkz: [test denetleyicisi mantığı](testing.md) bağımlılıkları istemenin öğrenmek için [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) denetleyicileri kodu test etmek kolaylaştırır.

ASP.NET Core'nın yerleşik bağımlılık ekleme sınıfları Hizmetleri istemek için yalnızca tek bir oluşturucuya sahip destekler. Birden fazla Oluşturucusu varsa, belirten bir özel durum alabilirsiniz:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Hata iletisi durumları gibi yalnızca tek bir oluşturucuya sahip bu sorunu çözebilirsiniz. Ayrıca [varsayılan bağımlılık ekleme desteği üçüncü taraf bir uygulama ile değiştirmek](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), birden çok oluşturucular destekleyen birçoğu.

## <a name="action-injection-with-fromservices"></a>FromServices ile eylem ekleme

Bazen denetleyicinizi içinde bir hizmet için birden fazla eylem gerekmez. Bu durumda, bu eylem yönteminin bir parametresi olarak hizmet Ekle mantıklı olabilir. Bu öznitelik parametresiyle işaretleyerek yapılır `[FromServices]` aşağıda gösterildiği gibi:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Bir denetleyicisinden ayarlarına erişme

Uygulama veya yapılandırma içinden ayarlarını bir denetleyici erişim genel bir desen olur. Bu erişim açıklanan seçenekler düzeni kullanması gereken [yapılandırma](xref:fundamentals/configuration/index). Genellikle ayarlarını doğrudan bağımlılık ekleme kullanılarak, denetleyicisinden isteyemedi. Daha iyi bir yaklaşım isteği bir `IOptions<T>` örneği burada `T` ihtiyacınız yapılandırma sınıftır.

Seçenekleri deseni ile çalışmak için bunun gibi seçenekleri temsil eden bir sınıf oluşturmanız gerekir:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

Seçenekleri modeli kullanır ve Hizmetleri koleksiyonunda yapılandırma sınıf eklemek için uygulamayı yapılandırmak gereken sonra `ConfigureServices`:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> Yukarıdaki listede biz ayarları JSON biçimli bir dosyadan okunan uygulamaya yapılandırmış olursunuz. Yukarıdaki açıklamalı kodda gösterildiği gibi tamamen kodda ayarları da yapılandırabilirsiniz. Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla yapılandırma seçenekleri için.

Kesin türü belirtilmiş yapılandırma nesnesi belirlediğiniz sonra (Bu durumda, `SampleWebSettings`) ve ekli Hizmetleri koleksiyonuna, onu herhangi denetleyici veya eylem yönteminden bir örneğini isteyerek talep edebilir `IOptions<T>` (Bu durumda, `IOptions<SampleWebSettings>`) . Aşağıdaki kod bir denetleyicisinden ayarları nasıl istemek gösterir:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Seçenekleri Desen aşağıdaki ayarları ve yapılandırmayı birbirinden ayrılmış sağlar ve denetleyici izlemektir sağlar [sorunları ayrılması](http://deviq.com/separation-of-concerns/), nasıl ve nerede bilmek gerekli olmayan beri ayarları bulunamıyor bilgi. Ayrıca denetleyicisi birim testi kolaylaştırır [test denetleyicisi mantığı](testing.md), olduğundan hiçbir [statik cling](http://deviq.com/static-cling/) veya denetleyici sınıfı içinde ayarları sınıfların doğrudan örnek oluşturma.
