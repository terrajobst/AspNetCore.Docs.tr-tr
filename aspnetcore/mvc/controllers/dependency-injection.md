---
title: ASP.NET Core denetleyicilere bağımlılık ekleme
author: ardalis
description: ASP.NET Core MVC denetleyicilerinin bağımlılıklarını açıkça nasıl isteyeceğini, ASP.NET Core bağımlılık ekleme ile oluşturucuları aracılığıyla nasıl isteyeceğini öğrenin.
ms.author: riande
ms.date: 02/24/2019
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 202b62d4b30c5c61c407abdc8509a2a75e181cb6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660085"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a>ASP.NET Core denetleyicilere bağımlılık ekleme

<a name="dependency-injection-controllers"></a>

By [shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT)ve [Steve Smith](https://github.com/ardalis)

ASP.NET Core MVC denetleyicileri, oluşturucular aracılığıyla bağımlılıkları doğrudan ister. ASP.NET Core, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)için yerleşik desteğe sahiptir. Dı, uygulamaların test ve bakımını daha kolay hale getirir.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="constructor-injection"></a>Oluşturucu Ekleme

Hizmetler bir oluşturucu parametresi olarak eklenir ve çalışma zamanı hizmeti hizmet kapsayıcısından çözer. Hizmetler genellikle arabirimler kullanılarak tanımlanır. Örneğin, geçerli zamanı gerektiren bir uygulamayı düşünün. Aşağıdaki arabirim `IDateTime` hizmetini kullanıma sunar:

[!code-csharp[](dependency-injection/sample/ControllerDI/Interfaces/IDateTime.cs?name=snippet)]

Aşağıdaki kod `IDateTime` arabirimini uygular:

[!code-csharp[](dependency-injection/sample/ControllerDI/Services/SystemDateTime.cs?name=snippet)]

Hizmeti hizmet kapsayıcısına ekleyin:

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup1.cs?name=snippet&highlight=3)]

<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>hakkında daha fazla bilgi için bkz. [dı hizmeti yaşam süreleri](xref:fundamentals/dependency-injection#service-lifetimes).

Aşağıdaki kod, günün saatine göre kullanıcıya bir karşılama görüntüler:

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet)]

Uygulamayı çalıştırın ve zamana göre bir ileti görüntülenir.

## <a name="action-injection-with-fromservices"></a>FromServices ile eylem ekleme

<xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute>, bir hizmetin Oluşturucu Ekleme kullanmadan doğrudan bir eylem yöntemine ekleme izin vermez:

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet2)]

## <a name="access-settings-from-a-controller"></a>Denetleyiciden erişim ayarları

Bir denetleyici içinden uygulama veya yapılandırma ayarlarına erişmek ortak bir modeldir. <xref:fundamentals/configuration/options> açıklanan *Seçenekler deseninin* ayarları yönetmek için tercih edilen yaklaşım vardır. Genellikle, bir denetleyiciye doğrudan <xref:Microsoft.Extensions.Configuration.IConfiguration> eklemeyin.

Seçenekleri temsil eden bir sınıf oluşturun. Örnek:

[!code-csharp[](dependency-injection/sample/ControllerDI/Models/SampleWebSettings.cs?name=snippet)]

Yapılandırma sınıfını hizmetler koleksiyonuna ekleyin:

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup.cs?highlight=4&name=snippet1)]

Bir JSON biçimli dosyadan ayarları okumak için uygulamayı yapılandırın:

[!code-csharp[](dependency-injection/sample/ControllerDI/Program.cs?name=snippet&range=10-15)]

Aşağıdaki kod, hizmet kapsayıcısından `IOptions<SampleWebSettings>` ayarlarını ister ve `Index` yönteminde kullanır:

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/SettingsController.cs?name=snippet)]

## <a name="additional-resources"></a>Ek kaynaklar

* Denetleyicilerde açıkça bağımlılıklar isteyerek kodu daha kolay test etme hakkında bilgi edinmek için <xref:mvc/controllers/testing> bakın.

* [Varsayılan bağımlılık ekleme kapsayıcısını üçüncü taraf bir uygulamayla değiştirin](xref:fundamentals/dependency-injection#default-service-container-replacement).
