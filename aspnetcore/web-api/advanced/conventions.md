---
title: Web API 'SI kurallarını kullanma
author: pranavkm
description: ASP.NET Core Web API kuralları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: web-api/advanced/conventions
ms.openlocfilehash: 2c7e33da24322504fc5e1be83c0b814710186687
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881313"
---
# <a name="use-web-api-conventions"></a>Web API 'SI kurallarını kullanma

, [Pranav Krishnamoorthy](https://github.com/pranavkm) ve [Scott Ade](https://github.com/scottaddie) tarafından

ASP.NET Core 2,2 ve üzeri, ortak [API belgelerini](xref:tutorials/web-api-help-pages-using-swagger) ayıklamanızı ve bunu birden çok eyleme, denetleyiciye veya bir derleme içindeki tüm denetleyicilere uygulamanıza yönelik bir yol içerir. Web API kuralları, [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute)tek tek eylemleri dekorasyon bir yerdir.

Bir kural şunları yapmanıza olanak sağlar:

* Belirli bir eylem türünden döndürülen en yaygın dönüş türlerini ve durum kodlarını tanımlayın.
* Tanımlanan standartta saptacak eylemleri belirler.

ASP.NET Core MVC 2,2 ve üzeri, <xref:Microsoft.AspNetCore.Mvc.DefaultApiConventions?displayProperty=fullName>bir dizi varsayılan kural içerir. Kurallar ASP.NET Core **API** proje şablonunda belirtilen denetleyiciyi (*ValuesController.cs*) temel alır. Eylemleriniz şablondaki desenleri izledikten sonra varsayılan kuralları kullanarak başarılı olmanız gerekir. Varsayılan kurallar ihtiyaçlarınızı karşılamıyorsa, bkz. [Web API kuralları oluşturma](#create-web-api-conventions).

Çalışma zamanında <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> kuralları anlamıştır. `ApiExplorer`, [Openapı](https://www.openapis.org/) (Swagger olarak da bilinir) belge oluşturucuları ile iletişim kurmak için MVC 'nin soyutlamasıdır. Uygulanan kuraldaki öznitelikler bir eylemle ilişkilendirilir ve eylemin Openapı belgelerine dahil edilir. [API Çözümleyicileri](xref:web-api/advanced/analyzers) , kuralları da anlalar. Eyleminiz geleneksel değilse (örneğin, uygulanan kural tarafından belgelenmemiş bir durum kodu döndürürse), durum kodunu belgeleyerek bir uyarı görürsünüz.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Web API 'SI kurallarını Uygula

Kurallar oluşturma; Her eylem, tam olarak bir kurala göre ilişkilendirilebilir. Daha belirgin kuralların daha az belirli kurallara göre daha fazla olması. Aynı önceliğe sahip iki veya daha fazla kural bir eyleme uygulandığınızda seçim belirleyici değildir. Aşağıdaki seçenekler, en çok belirli olan en az belirli bir eyleme bir kural uygulamak için mevcuttur:

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; tek tek eylemler için geçerlidir ve kural türünü ve uygulanan kural yöntemini belirtir.

    Aşağıdaki örnekte, varsayılan kural türünün `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` kuralı yöntemi `Update` eylemine uygulanır:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` Convention yöntemi eyleme aşağıdaki öznitelikleri uygular:

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(StatusCodes.Status204NoContent)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    ```

    `[ProducesDefaultResponseType]`hakkında daha fazla bilgi için bkz. [Varsayılan Yanıt](https://swagger.io/docs/specification/describing-responses/#default).

1. bir denetleyiciye uygulanan `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute`, belirtilen kural türünü denetleyicideki tüm eylemlere uygular &mdash;. Kural yöntemi, kural yönteminin uygulandığı eylemleri belirleyen ipuçlarıyla işaretlenir. İpuçları hakkında daha fazla bilgi için bkz. [Web API kuralları oluşturma](#create-web-api-conventions)).

    Aşağıdaki örnekte, varsayılan kurallar kümesi *ContactsConventionController*içindeki tüm eylemlere uygulanır:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. bir derlemeye uygulanan `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute`, belirtilen kural türünü geçerli derlemedeki tüm denetleyicilere uygular &mdash;. Öneri olarak, *Startup.cs* dosyasına derleme düzeyi öznitelikleri uygulayın.

    Aşağıdaki örnekte, varsayılan kural kümesi derlemedeki tüm denetleyicilere uygulanır:

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a>Web API kuralları oluşturma

Varsayılan API kuralları gereksinimlerinizi karşılamıyorsa, kendi kurallarınızı oluşturun. Bir kural:

* Metotları olan statik bir tür.
* Eylemlerde [Yanıt türleri](#response-types) ve [adlandırma gereksinimleri](#naming-requirements) tanımlama özelliğine sahiptir.

### <a name="response-types"></a>Yanıt türleri

Bu yöntemlere `[ProducesResponseType]` veya `[ProducesDefaultResponseType]` öznitelikleriyle açıklama eklenir. Örneğin:

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

Daha özel meta veri öznitelikleri yoksa, bu kuralı bir derlemeye uygulamak şunları uygular:

* Kural yöntemi, `Find`adlı tüm eylemler için geçerlidir.
* `Find` eyleminde `id` adlı bir parametre var.

### <a name="naming-requirements"></a>Adlandırma gereksinimleri

`[ApiConventionNameMatch]` ve `[ApiConventionTypeMatch]` öznitelikleri, uygulandıkları eylemleri belirleyen kural yöntemine uygulanabilir. Örneğin:

```csharp
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status404NotFound)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

Yukarıdaki örnekte:

* Yöntemine uygulanan `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` seçeneği, kuralının "Find" önekini ön eki eklenmiş herhangi bir eylem ile eşleştiğini gösterir. Eşleşen eylemlere örnek olarak `Find`, `FindPet`ve `FindById`verilebilir.
* Parametreye uygulanan `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix`, yöntemin sonek tanımlayıcısında biten tek bir parametreye sahip yöntemlerle eşleştiğini gösterir. Örnekler `id` veya `petId`gibi parametreleri içerir. parametre türünü kısıtlamak için `ApiConventionTypeMatch` benzer şekilde türlere uygulanabilir. `params[]` bağımsız değişkeni, açıkça eşleştirilmesinin gerekli olmadığı kalan parametreleri gösterir.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
