---
title: ASP.NET core'da uygulama bölümleri
author: ardalis
description: Uygulama kaynaklarını özetlerdir, uygulama bölümleri bulmak veya bir derlemeye ait özelliklerin yüklenmesini önlemek için kullanmayı öğrenin.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 9d0b4b5fadcc287172f23fa36c421f04ca2ade4a
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815475"
---
# <a name="application-parts-in-aspnet-core"></a>ASP.NET core'da uygulama bölümleri

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Bir *uygulama bölümü* etiket Yardımcıları bulunabileceğini veya MVC denetleyicileri, görünüm bileşenleri gibi özellikleri, bir uygulamanın kaynaklar üzerinde bir soyutlamadır. Bir uygulama bölümü bir bütünleştirilmiş kod başvurusu ve düzenlemenizi sağlayan türler ve derleme başvurularını kapsülleyen bir AssemblyPart örneğidir. *Özellik sağlayıcıları* bir ASP.NET Core MVC uygulaması özelliklerini doldurmak için uygulama bölümleri ile çalışır. Bul (veya yüklenmesini önlemek için) yapılandırmanız, izin vermek için uygulama bölümleri için ana kullanım örneği olan bir derlemeden MVC özellikleri.

## <a name="introducing-application-parts"></a>Uygulama Bölümleri ile tanışın

MVC uygulamaları yüklemek, özelliklerinden [uygulama bölümleri](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). Özellikle, [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart) sınıfı, bir derleme tarafından desteklenen bir uygulama bölümü temsil eder. Bu sınıfların bulmak ve MVC denetleyicileri, görünüm bileşenleri, etiket yardımcıları ve razor derleme kaynakları gibi özellikleri yüklemek için kullanabilirsiniz. [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) uygulama bölümleri ve özellik sağlayıcıları kullanılabilir MVC uygulaması için izleme sorumludur. Etkileşim kurabileceğiniz `ApplicationPartManager` içinde `Startup` MVC yapılandırırken:

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

Varsayılan olarak MVC Bağımlılık ağacı aramak ve denetleyicileri (hatta diğer derlemelerde) bulun. (Örneğin, derleme zamanında başvuru değil bir eklenti) gelen rasgele bir derlemeyi yüklemek için bir uygulama bölümünü kullanabilirsiniz.

Uygulama bölümleri için kullanabileceğiniz *önlemek* belirli bir derleme veya konum denetleyicileri aranıyor. Hangi bölümlerini (veya derlemeleri) değiştirerek uygulaması tarafından denetleyebilirsiniz `ApplicationParts` koleksiyonunu `ApplicationPartManager`. Giriş sırası `ApplicationParts` koleksiyon önemli değildir. Tam olarak yapılandırılması önemlidir `ApplicationPartManager` kapsayıcıda Hizmetleri'ni yapılandırmak için kullanmadan önce. Örneğin, tam olarak yapılandırmanız gereken `ApplicationPartManager` çağırmadan önce `AddControllersAsServices`. Bunu yapmak başarısız olan anlamına gelir uygulama bölümleri denetleyicileri sonra yöntem çağrısının etkilenmez eklediğiniz (hizmet olarak kayıtlı gerekmez), uygulamanızın içinde yanlış davranışlara neden olabilir.

Kullanılacak istemediğiniz denetleyicileri içeren bir bütünleştirilmiş kod varsa, oradan kaldırın `ApplicationPartManager`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           apm.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

Proje derleme ve bunların bağımlı derlemeleri yanı sıra `ApplicationPartManager` bölümlerini içerecek `Microsoft.AspNetCore.Mvc.TagHelpers` ve `Microsoft.AspNetCore.Mvc.Razor` varsayılan olarak.

## <a name="application-feature-providers"></a>Uygulama özellik sağlayıcıları

Uygulama özellik sağlayıcıları, uygulama bölümleri inceleyin ve bu parçaları için özellikler sağlar. Aşağıdaki MVC özellikler için yerleşik özellik sağlayıcıları vardır:

* [Denetleyiciler](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Meta veri başvurusu](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Etiket Yardımcıları](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Görünüm bileşenleri](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Özellik sağlayıcıları devralmanız `IApplicationFeatureProvider<T>`burada `T` özelliği türüdür. Kendi özellik MVC'nin özellik türlerinden herhangi birini sağlayıcıları, yukarıda listelenen uygulayabilirsiniz. Özellik sağlayıcıları sırasını `ApplicationPartManager.FeatureProviders` sonraki sağlayıcıları önceki sağlayıcıları tarafından gerçekleştirilen eylemler için tepki verebilir olduğundan koleksiyon önemli olabilir.

### <a name="sample-generic-controller-feature"></a>Örnek: Genel denetleyicisi özelliği

Varsayılan olarak, ASP.NET Core MVC denetleyicileri genel göz ardı eder (örneğin, `SomeController<T>`). Bu örnek sonra varsayılan Sağlayıcısı'nı çalıştıran ve belirtilen bir liste türleri genel denetleyicisi örnekleri ekleyen bir denetleyici özellik sağlayıcısı kullanıyor (tanımlanan `EntityTypes.Types`):

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Varlık türleri:

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Özellik sağlayıcısı eklenir `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Varsayılan olarak, yönlendirme için kullanılan genel denetleyicisi adları biçiminde olacaktır *GenericController'1 [pencere öğesi]* yerine *pencere öğesi*. Aşağıdaki öznitelik adı denetleyici tarafından kullanılan genel tür karşılık gelecek şekilde değiştirmek için kullanılır:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Sınıfı:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Eşleşen bir rota istendiğinde sonucu:

![Örnek uygulamadan çıktı örneği okur, 'Hello genel Sprocket denetleyicisinden.'](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Örnek: Kullanılabilir özellikleri görüntüle

İsteyerek doldurulan özellikleri uygulamanıza yineleyebilirsiniz bir `ApplicationPartManager` aracılığıyla [bağımlılık ekleme](../../fundamentals/dependency-injection.md) ve uygun özellikler örneğini doldurmak için kullanarak:

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Örnek çıktı:

![Örnek uygulamayı örnek çıktısı](app-parts/_static/available-features.png)
