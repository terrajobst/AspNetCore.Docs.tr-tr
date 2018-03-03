---
title: "ASP.NET Core uygulama bölümleri"
author: ardalis
description: "Bir uygulama soyutlamalar kaynaklardır uygulama bölümleri bulmak veya bir derlemeye ait özelliklerin yüklenmesini önlemek için nasıl kullanılacağını öğrenin."
manager: wpickett
ms.author: riande
ms.date: 01/04/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/extensibility/app-parts
ms.openlocfilehash: a6391dcff2edc239f611be6bac60b40de292634e
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="application-parts-in-aspnet-core"></a>ASP.NET Core uygulama bölümleri

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Bir *uygulama bölümü* MVC denetleyicileri, görünüm bileşenleri gibi özellikleri, bir uygulama kaynakları üzerinden bir soyutlamadır veya etiket Yardımcıları saptanmalıdır. Bir uygulama bölümü bir derleme başvurusu ve düzenlemenizi sağlayan türleri ve derleme başvurularını yalıtan bir AssemblyPart örnektir. *Özellik sağlayıcıları* ASP.NET Core MVC uygulama özelliklerini doldurmak için uygulama bölümleri ile çalışır. Ana kullanım örneği uygulama bölümleri için Bul (veya yüklenmesini önlemek için) Uygulamanızı yapılandırmak izin vermektir bütünleştirilmiş MVC özelliklerinden.

## <a name="introducing-application-parts"></a>Uygulama bölümlerini Tanıtımı

MVC uygulamaları kendi özelliklerinden yük [uygulama bölümleri](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). Özellikle, [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) sınıfı, bir derlemeyi tarafından yedeklenen bir uygulama bölümü temsil eder. Bu sınıfların bulmak ve denetleyicileri, görünümü bileşenler, etiket yardımcıları ve razor derleme kaynakları gibi MVC özellikleri yüklemek için kullanabilirsiniz. [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) MVC uygulamasını uygulama bölümleri ve özellik sağlayıcıları kullanılabilir izlemek için sorumludur. Etkileşim kurabildikleri `ApplicationPartManager` içinde `Startup` MVC yapılandırırken:

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

Varsayılan olarak MVC bağımlılığı ağacı arayın ve denetleyicileri (hatta diğer derlemelerde) bulun. Bir rastgele derlemeden (örneğin, derleme zamanında başvurulan değil bir eklenti) yüklemek için bir uygulama bölümü kullanabilirsiniz.

Uygulama bölümleri için kullanabileceğiniz *kaçının* belirli derleme veya konum denetleyicileri aranıyor. Hangi bölümleri (veya derlemeler) değiştirerek uygulamaya kullanılabilir olacağını kontrol `ApplicationParts` koleksiyonu `ApplicationPartManager`. Girdileri sırasını `ApplicationParts` koleksiyonu önemli değildir. Tam olarak yapılandırılması önemlidir `ApplicationPartManager` kapsayıcısında Hizmetleri'ni yapılandırmak için kullanmadan önce. Örneğin, tam olarak yapılandırmanız gerekiyor `ApplicationPartManager` çağırmadan önce `AddControllersAsServices`. Bunu yapmak, başarısız olan anlamına gelir uygulama bölümleri denetleyicileri sonra yöntem çağrısı etkilenmeyecek eklediğiniz (hizmet olarak kayıtlı kalmaz), uygulamanızın yanlış bevavior neden olabilir.

Kullanılacak istemediğiniz denetleyicileri içeren bir derleme varsa kaldırmadan `ApplicationPartManager`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

Proje derleme ve bağımlı derlemeleri, ek olarak `ApplicationPartManager` bölümleri için içerecektir `Microsoft.AspNetCore.Mvc.TagHelpers` ve `Microsoft.AspNetCore.Mvc.Razor` varsayılan olarak.

## <a name="application-feature-providers"></a>Uygulama özellik sağlayıcıları

Uygulama özellik sağlayıcıları uygulama bölümleri inceleyin ve bu bölümleri için özellikleri sağlar. Aşağıdaki MVC özellikler için yerleşik özellik sağlayıcıları vardır:

* [Denetleyiciler](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Meta veri başvurusu](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Etiket Yardımcıları](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Görünüm bileşenleri](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Özellik sağlayıcıları devral `IApplicationFeatureProvider<T>`, burada `T` özellik türüdür. MVC'ın özellik türlerinin herhangi biriyle sağlayıcıları yukarıda listelenen kendi özellik uygulayabilirsiniz. Özellik sağlayıcıların sırası `ApplicationPartManager.FeatureProviders` sonraki sağlayıcıları önceki sağlayıcıları tarafından gerçekleştirilen eylemler için tepki gösterebilmesi beri koleksiyonu önemli olabilir.

### <a name="sample-generic-controller-feature"></a>Örnek: Genel denetleyicisi özelliği

Varsayılan olarak, ASP.NET Core MVC genel denetleyicileri göz ardı eder (örneğin, `SomeController<T>`). Bu örnek sonra varsayılan Sağlayıcısı'nı çalıştıran ve belirtilen liste için genel denetleyici örnekleri türlerinin ekleyen bir denetleyici özellik sağlayıcısı kullanır (tanımlanan `EntityTypes.Types`):

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Varlık türleri:

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Özellik sağlayıcısı eklenir `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Varsayılan olarak, yönlendirme için kullanılan genel denetleyicisi adları biçiminde olacaktır *GenericController'1 [pencere]* yerine *pencere öğesi*. Aşağıdaki öznitelik adı denetleyici tarafından kullanılan genel tür karşılık gelecek şekilde değiştirmek için kullanılır:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Sınıfı:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Eşleşen bir rota istendiğinde sonucu:

![Örnek uygulamadan çıktı örneği okuma ve 'Hello genel Sproket denetleyicisinden.'](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Örnek: Görüntü kullanılabilir özellikler

İsteyerek uygulamanıza doldurulan kullanılabilen özellikleri yineleyebilirsiniz bir `ApplicationPartManager` aracılığıyla [bağımlılık ekleme](../../fundamentals/dependency-injection.md) ve uygun özelliklerin örneklerini doldurmak için kullanma:

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Örnek çıktı:

![Örnek uygulaması örnek çıkışı](app-parts/_static/available-features.png)
