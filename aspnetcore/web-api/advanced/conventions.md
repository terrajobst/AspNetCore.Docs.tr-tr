---
title: Web API kuralları kullanma
author: pranavkm
description: ASP.NET Core web API kuralları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 25e8d5209f02683c533ef7c316b91d447f1b20ba
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705464"
---
# <a name="use-web-api-conventions"></a>Web API kuralları kullanma

Tarafından [Pranav Krishnamoorthy](https://github.com/pranavkm) ve [Scott Addie](https://github.com/scottaddie)

ASP.NET Core 2.2 ve sonraki sürümleri içeren ortak ayıklamak için bir yol [API belgeleri](xref:tutorials/web-api-help-pages-using-swagger) ve birden fazla eylem, denetleyicileri veya derlemedeki tüm denetleyicileri uygulanır. Web API kurallardır bireysel eylemleri dekorasyon yerine [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).

Bir kuralı sağlar:

* En yaygın dönüş türleri ve durum kodları belirli bir eylem türünden döndürülen tanımlayın.
* Tanımlanmış standart sapma eylemleri tanımlayın.

ASP.NET Core MVC 2.2 ve sonraki sürümleri içeren bir dizi varsayılan kuralları <xref:Microsoft.AspNetCore.Mvc.DefaultApiConventions?displayProperty=fullName>. Denetleyicide kurallarını temel alır (*ValuesController.cs*) içinde ASP.NET Core sağlanan **API** proje şablonu. Eylemlerinizi şablondaki desenleri izlerseniz, varsayılan kuralları kullanılarak başarılı olmalıdır. Varsayılan kuralları gereksinimlerinizi karşılamıyorsa bkz [web API'si kuralları oluşturma](#create-web-api-conventions).

Çalışma zamanında, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> kurallarını anlar. `ApiExplorer` ile iletişim kurmak için MVC'nin soyutlama [Openapı](https://www.openapis.org/) (diğer adıyla Swagger) belgesi üreteçleri. Uygulanan kuralı öznitelikleri bir eylemle ilişkili ve eylemin Openapı belgelerinde yer alır. [API Çözümleyicileri](xref:web-api/advanced/analyzers) ayrıca kurallarını anlama. Eyleminizi sıra dışı ise (örneğin, belgelenen uygulanan kural gereği olmayan bir durum kodu döndürür), bir uyarı, durum kodu belgeye önerir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Web API kuralları uygula

Kuralları oluşturma yok; Her eylem, tam olarak bir kuralı ile ilişkilendirilmiş olabilir. Daha özel kuralları üzerinde daha az belirgin kurallar önceliklidir. İki veya daha fazla kurallar aynı öncelik için bir eylem uyguladığınızda seçim belirleyici değildir. Bir eylem için geçerli bir kural için aşağıdaki seçenekler mevcuttur. en az belirli belirli:

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Bireysel eylemleri için geçerlidir ve kuralı türü ve geçerli kuralı yöntemini belirtir.

    Aşağıdaki örnekte, varsayılan kuralı türü 's `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` kuralı yöntemi uygulanan `Update` eylem:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` Kuralı yöntemi aşağıdaki öznitelikleri eylemini uygular:

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(StatusCodes.Status204NoContent)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    ```

Daha fazla bilgi için `[ProducesDefaultResponseType]`, bkz: [varsayılan yanıt](https://swagger.io/docs/specification/describing-responses/#default).

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` bir denetleyiciye uygulanan &mdash; belirtilen kural türü denetleyicinin tüm eylemleri için geçerlidir. Bir kural yöntem kuralı yöntemi uygulandığı eylemleri belirlemek ve ipuçları ile sunulur. İpuçları hakkında daha fazla bilgi için bkz. [web API'si kuralları oluşturma](#create-web-api-conventions)).

    Aşağıdaki örnekte, tüm eylemler için varsayılan kurallar kümesini uygulanan *ContactsConventionController*:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` bir derlemeye uygulanan &mdash; belirtilen kural türü geçerli derlemedeki tüm denetleyicileri için geçerlidir. Bir öneri, derleme düzeyinde öznitelikler uygulamak *Startup.cs* dosya.

    Aşağıdaki örnekte, varsayılan kuralları kümesi derlemedeki tüm denetleyicileri uygulanır:

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a>Web API kuralları oluşturma

Varsayılan API kurallarının gereksinimlerinizi karşılamıyorsa, kendi kuralları oluşturun. Bir kural aşağıdaki gibidir:

* Yöntemleri statik türü.
* Tanımlama yeteneği [yanıt türleri](#response-types) ve [adlandırma gereksinimlerini](#naming-requirements) eylemleri.

### <a name="response-types"></a>Yanıt türleri

Bu yöntemleri ile açıklamalı olan `[ProducesResponseType]` veya `[ProducesDefaultResponseType]` öznitelikleri. Örneğin:

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public static void Find(int id)
    {
    }
}
```

Daha özel meta veri öznitelikleri yoksa, bu kuralı bir derlemeye uygulamak, uygular:

* Kuralı yöntemi adlı herhangi bir eylem için geçerli `Find`.
* Adlı bir parametre `id` üzerinde mevcut olduğundan `Find` eylem.

### <a name="naming-requirements"></a>Adlandırma gereksinimleri

`[ApiConventionNameMatch]` Ve `[ApiConventionTypeMatch]` öznitelikler uygulandıkları eylemleri belirler kuralı yöntemi uygulanabilir. Örneğin:

```csharp
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status404NotFound)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

Önceki örnekte:

* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` Yönteminizden seçeneği, kuralı, "Bul" ile önek herhangi bir eylem eşleştiğini gösterir. Eşleşen eylemleri örnekler `Find`, `FindPet`, ve `FindById`.
* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` Uygulanan parametresi kuralı soneki tanımlayıcıda bitiş tam olarak bir parametreye sahip yöntemleri eşleştiğini gösterir. Örnekler parametreleri gibi `id` veya `petId`. `ApiConventionTypeMatch` parametre türü sınırlamak için türlerine benzer şekilde uygulanabilir. A `params[]` bağımsız değişkenini açıkça eşlenmesi gerekmez kalan parametrelerle gösterir.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
