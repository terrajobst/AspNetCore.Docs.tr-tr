---
title: ASP.NET core'da denetleyicilere bağımlılık ekleme
author: ardalis
description: ASP.NET Core MVC denetleyicileri bağımlılıklarını oluşturucuları bağımlılık ekleme ASP.NET Core ile açıkça aracılığıyla nasıl istek keşfedin.
ms.author: riande
ms.date: 02/24/2019
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 6b08c321f4cae1f4efd8ea40300eaf4dfc2f63a1
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903071"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a>ASP.NET core'da denetleyicilere bağımlılık ekleme

<a name="dependency-injection-controllers"></a>

Tarafından [Shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Steve Smith](https://github.com/ardalis)

ASP.NET Core MVC denetleyicileri oluşturucular açıkça aracılığıyla bağımlılıkları isteyin. ASP.NET Core için yerleşik desteği vardır [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection). DI uygulamaları test edin ve bakımını kolaylaştırır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="constructor-injection"></a>Oluşturucu ekleme

Hizmet bir oluşturucu parametresi eklenir ve çalışma zamanı hizmet kapsayıcı hizmetinden giderir. Hizmetleri, genellikle arabirimleri kullanılarak tanımlanır. Örneğin, geçerli zamanı gerektiren bir uygulama düşünün. Aşağıdaki kullanıma sunan arabirim `IDateTime` hizmeti:

[!code-csharp[](dependency-injection/sample/ControllerDI/Interfaces/IDateTime.cs?name=snippet)]

Aşağıdaki kod uygulayan `IDateTime` arabirimi:

[!code-csharp[](dependency-injection/sample/ControllerDI/Services/SystemDateTime.cs?name=snippet)]

Hizmet, hizmet kapsayıcıya ekleyin:

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup1.cs?name=snippet&highlight=3)]

Daha fazla bilgi için <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>, bkz: [DI hizmet yaşam süreleri](xref:fundamentals/dependency-injection#service-lifetimes).

Aşağıdaki kod bir karşılama günün saatini temel alan kullanıcı için görüntüler:

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet)]

Uygulamayı çalıştırın ve saatini temel alan bir ileti görüntülenir.

## <a name="action-injection-with-fromservices"></a>FromServices ile eylemi ekleme

<xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute> Oluşturucu ekleme kullanmadan, doğrudan bir eylem yöntemi bir hizmet ekleme sağlar:

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet2)]

## <a name="access-settings-from-a-controller"></a>Bir denetleyiciden erişim ayarları

Bir denetleyici içinde gelen uygulama veya yapılandırma ayarlarına erişme ortak bir desendir. *Seçenekleri deseni* açıklanan <xref:fundamentals/configuration/options> ayarlarını yönetmek için tercih edilen yaklaşım. Genellikle doğrudan ekleme yoksa <xref:Microsoft.Extensions.Configuration.IConfiguration> içine bir denetleyici.

Seçenekleri temsil eden bir sınıf oluşturun. Örneğin:

[!code-csharp[](dependency-injection/sample/ControllerDI/Models/SampleWebSettings.cs?name=snippet)]

Yapılandırma sınıfı Hizmetleri koleksiyona ekleyin:

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup.cs?highlight=4&name=snippet1)]

Okumasına JSON biçimli bir dosyadan ayarları yapılandırın:

[!code-csharp[](dependency-injection/sample/ControllerDI/Program.cs?name=snippet&range=10-15)]

Aşağıdaki kod istekleri `IOptions<SampleWebSettings>` hizmet kapsayıcı ayarları ve bunları kullanan `Index` yöntemi:

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/SettingsController.cs?name=snippet)]

## <a name="additional-resources"></a>Ek kaynaklar

* Bkz: <xref:mvc/controllers/testing> kod test denetleyicileri bağımlılıkları açıkça isteyerek daha kolay hale getirmek öğrenin.

* [Varsayılan bağımlılık ekleme kapsayıcısını üçüncü taraf bir uygulama ile değiştirin](xref:fundamentals/dependency-injection#default-service-container-replacement).
