---
title: Web API kuralları kullanma
author: pranavkm
description: ASP.NET Core web API kuralları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710081"
---
# <a name="use-web-api-conventions"></a>Web API kuralları kullanma

ASP.NET Core 2.2 ortak ayıklamak için bir yol sunar [API belgeleri](xref:tutorials/web-api-help-pages-using-swagger) ve birden fazla eylem, denetleyicileri veya derlemedeki tüm denetleyicileri uygulanır. Web API kurallardır bireysel eylemleri dekorasyon yerine [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute). En yaygın "Geleneksel" dönüş türleri ve, eylem için eylem uygulayan kuralı yöntemi seçmek için bir yol ile iade durum kodları tanımlamanızı sağlar.

Varsayılan olarak, ASP.NET Core MVC 2.2 varsayılan kuralları, bir dizi ile birlikte gelen `Microsoft.AspNetCore.Mvc.DefaultApiConventions`. ASP.NET Core iskele oluşturulduğunu denetleyicisinde kurallarını temel alır. Eylemlerinizi desenini izler, yapı iskelesi üretir, varsayılan kuralları kullanılarak başarılı olmalıdır.

Çalışma zamanında, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> kurallarını anlar. `ApiExplorer` Açık API belge oluşturucular ile iletişim kurmak için MVC'nin soyutlamadır. Uygulanan kuralı öznitelikleri, bir eylem ile ilişkili alın ve eylemin Swagger belgelerinde yer alır. API Çözümleyicileri de kuralları anlayın. Eyleminizi sıra dışı ise (örneğin, belgelenen uygulanan kural gereği olmayan bir durum kodu döndürür), bu belge için teşvik, bir uyarı üretir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Web API kuralları uygula

Bir kuralı uygulamak için üç yolunuz vardır. Kuralları oluşturma yok. Her eylem, tam olarak bir kuralı ile ilişkilendirilmiş olabilir. (Aşağıda açıklanmıştır) daha belirli kuralları, daha özgül daha önceliklidir. İki veya daha fazla kurallar aynı öncelik için bir eylem uyguladığınızda seçim belirleyici değildir. Bir eylem için geçerli bir kural için aşağıdaki seçenekler mevcuttur. en az belirli belirli:

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Bireysel eylemleri için geçerlidir ve kuralı türü ve geçerli kuralı yöntemini belirtir. Aşağıdaki örnekte, kuralı yöntemi `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` uygulanan `Update` eylem:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` bir denetleyiciye uygulanan &mdash; kuralı türü denetleyicinin tüm eylemleri için geçerlidir. Kuralı yöntemleri (Ayrıntılar) kuralları yazma işleminin parçası olarak uygulanacak eylemleri belirlemek ipuçları ile donatılmış. Örneğin:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` bir derlemeye uygulanan &mdash; kuralı türü geçerli derlemedeki tüm denetleyicileri için geçerlidir. Örneğin:

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a>Web API kuralları oluşturma

Yöntemleri statik bir türle bir kuraldır. Bu yöntemleri ile açıklamalı olan `[ProducesResponseType]` veya `[ProducesDefaultResponseType]` öznitelikleri.

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

Bu kural bir derlemeye uygulama sonuçları herhangi bir işlem adıyla uygulama kuralı yönteminde `Find` ve adlı tam olarak bir parametreye sahip `id`, bunlar diğer daha özel meta veri öznitelikleri yok sürece.

Ek olarak `[ProducesResponseType]` ve `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` ve `[ApiConventionTypeMatch]` uygulamak için yöntemleri belirler kuralı yöntemi uygulanabilir. Örneğin:

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` Yönteme uygulanan seçeneği kuralı "İle Bul" ön ekli olduğu sürece herhangi bir eylem eşleşebilir gösterir. Bu gibi yöntemleri içerir `Find`, `FindPet`, veya `FindById`.
* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` Uygulanan kural yöntemleri soneki tanımlayıcıda bitiş tam olarak bir parametre ile eşleşebilir parametresi gösterir. Bu gibi parametreler içeren `id` veya `petId`. `ApiConventionTypeMatch` benzer şekilde türleriyle sınırlamak parametresinin türü için uygulanabilir. A `params[]` bağımsız değişkeni, açıkça eşlenmesi gerekmez kalan parametreleri belirtmek için kullanılabilir.
