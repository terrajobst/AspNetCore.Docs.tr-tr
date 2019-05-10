---
title: Önbellek etiketi Yardımcısı, ASP.NET Core MVC
author: pkellner
description: Önbellek etiketi Yardımcısı'nı kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: fb69584f6e9d4756e175bbd6f3deb1f413b80fc5
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902630"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Önbellek etiketi Yardımcısı, ASP.NET Core MVC

Tarafından [Peter Kellner](http://peterkellner.net) ve [Luke Latham](https://github.com/guardrex) 

Önbellek etiketi Yardımcısı iç ASP.NET Core önbelleği sağlayıcısı için içeriği önbelleğe alarak ASP.NET Core uygulamanızı performansını olanağı sağlar.

Etiket Yardımcıları genel bakış için bkz. <xref:mvc/views/tag-helpers/intro>.

Aşağıdaki Razor biçimlendirme, geçerli tarihi önbelleğe alır:

```cshtml
<cache>@DateTime.Now</cache>
```

Etiket Yardımcısını içeren sayfasında ilk isteği, geçerli tarihi görüntüler. (Varsayılan 20 dakika) önbelleğe süresi dolana kadar veya önbellekten önbelleğe alınan tarih veriler çıkarıldığında kadar ek isteklerin önbelleğe alınan değeri gösterilir.

## <a name="cache-tag-helper-attributes"></a>Önbellek etiketi Yardımcısı öznitelikleri

### <a name="enabled"></a>Etkin

| Öznitelik türü  | Örnekler        | Varsayılan |
| --------------- | --------------- | ------- |
| Boole değeri         | `true`, `false` | `true`  |

`enabled` Önbellek etiketi Yardımcısı tarafından alınmış içeriği önbelleğe alınmış belirler. Varsayılan, `true` değeridir. Varsa kümesine `false`, işlenmiş çıktı **değil** önbelleğe alınmış.

Örnek:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-on"></a>süresi dolmadan açma

| Öznitelik türü   | Örnek                            |
| ---------------- | ---------------------------------- |
| `DateTimeOffset` | `@new DateTime(2025,1,29,17,02,0)` |

`expires-on` önbelleğe alınan öğe için bir mutlak sona erme tarihi ayarlar.

Aşağıdaki örnek, 17:02:00 29 Ocak 2025 üzerinde kadar önbellek etiketi Yardımcısı içeriğini önbelleğe alır:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a>süresi dolduktan sonra

| Öznitelik türü | Örnek                      | Varsayılan    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | 20 dakika |

`expires-after` İçeriği önbelleğe almak için ilk isteği zamanından sürenin uzunluğunu ayarlar.

Örnek:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Razor görüntüleme motorunu varsayılan ayarlar `expires-after` yirmi dakika değeri.

### <a name="expires-sliding"></a>süresi dolmadan kayan

| Öznitelik türü | Örnek                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

Değerini erişilmeyen, önbellek girişi çıkarılacak süreyi ayarlar.

Örnek:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a>Vary-tarafından-üstbilgisi

| Öznitelik türü | Örnekler                                    |
| -------------- | ------------------------------------------- |
| Dize         | `User-Agent`, `User-Agent,content-encoding` |

`vary-by-header` Bunlar değiştirdiğinizde, önbellek yenileme tetiklemek üstbilgi değerlerini virgülle ayrılmış listesini kabul eder.

Aşağıdaki örnekte üst bilgi değeri izler `User-Agent`. Bu örnek için içerikleri önbelleğe alan her farklı `User-Agent` web sunucusuna sunulur:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a>farklı-tarafından-sorgu

| Öznitelik türü | Örnekler             |
| -------------- | -------------------- |
| Dize         | `Make`, `Make,Model` |

`vary-by-query` bir virgülle ayrılmış listesini kabul eder <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> bir sorgu dizesinde (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>), tetikleme önbellek yenileme herhangi bir değerini listelenen anahtar değişiklikler.

Aşağıdaki örnek değerleri izler `Make` ve `Model`. Bu örnek için içerikleri önbelleğe alan her farklı `Make` ve `Model` web sunucusuna sunulur:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a>farklı-tarafından-route

| Öznitelik türü | Örnekler             |
| -------------- | -------------------- |
| Dize         | `Make`, `Make,Model` |

`vary-by-route` Rota veri parametre değeri değiştiğinde bir önbellek yenileme tetikleyen rota parametre adlarının virgülle ayrılmış listesini kabul eder.

Örnek:

*Startup.cs*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

*Index.cshtml*:

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-cookie"></a>farklı-tarafından-tanımlama bilgisi

| Öznitelik türü | Örnekler                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| Dize         | `.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor` |

`vary-by-cookie` tanımlama bilgisi değerleri değiştiğinde önbelleği yenileme tetiklemek tanımlama bilgisi adlarının virgülle ayrılmış listesini kabul eder.

Aşağıdaki örnek, ASP.NET Core kimliği ile ilişkili tanımlama izler. Bir kullanıcının kimliği doğrulandığında, kimlik tanımlama değişikliği bir önbellek yenileme tetikleyen:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a>farklı kullanıcı tarafından

| Öznitelik türü  | Örnekler        | Varsayılan |
| --------------- | --------------- | ------- |
| Boole değeri         | `true`, `false` | `true`  |

`vary-by-user` oturum açmış kullanıcı (veya bağlam sorumlusu) değiştiğinde önbelleği sıfırlar olup olmadığını belirtir. Geçerli kullanıcı olarak da bilinen istek bağlamı sorumlusu ve bir Razor Görünümü'nde başvurarak görüntülenebilir `@User.Identity.Name`.

Aşağıdaki örnek, geçerli bir önbellek yenileme tetiklemek için kullanıcı oturum izler:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Bu özniteliği kullanarak içerikleri önbellekte aracılığıyla bir oturum açma ve oturum kapatma döngüsü tutar. Değer ayarlandığında `true`, önbellek kimliği doğrulanmış kullanıcı için bir kimlik doğrulama döngüsü geçersiz kılar. Yeni bir benzersiz tanımlama bilgisi değeri, bir kullanıcının kimliği doğrulandığında oluşturulmuş olduğu için önbellek geçersiz kılınır. Önbellek anonim durumu için tanımlama bilgisi mevcut olduğunda veya tanımlama bilgisinin doldu korunur. Kullanıcı, **değil** kimliği doğrulanmış ve önbellek korunur.

### <a name="vary-by"></a>değişiklik tarafından

| Öznitelik türü | Örnek  |
| -------------- | -------- |
| Dize         | `@Model` |

`vary-by` hangi verilerin önbelleğe alınmış bir özelleştirme için sağlar. Önbellek etiketi Yardımcısı içeriğini özniteliğin dize değeri değiştiğinde tarafından başvurulan nesne güncelleştirildiğinde. Genellikle, model değerlerinin dize birleştirme bu özniteliğe atanır. Etkili bir şekilde, burada önbellek nesnelerindeki değerleri herhangi bir güncelleştirme geçersiz kılar bir senaryoda sonuçlanır.

Aşağıdaki örnek iki yol parametreleri, tamsayı değeri görünümü SUM'ları oluşturma denetleyici yöntemi varsayar `myParam1` ve `myParam2`ve tek bir model özelliği olarak toplamını döndürür. Bu toplam değiştiğinde, önbellek etiketi Yardımcısı içeriğini oluşturulur ve tekrar önbelleğe alınmış.  

Eylem:

```csharp
public IActionResult Index(string myParam1, string myParam2, string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*:

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a>öncelik

| Öznitelik türü      | Örnekler                               | Varsayılan  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | `High`, `Low`, `NeverRemove`, `Normal` | `Normal` |

`priority` Yerleşik önbelleği sağlayıcısı için önbellek çıkarma rehberlik sağlar. Web sunucusu çıkarır `Low` bellek baskısı altında olduğunda girişleri ilk önbellek.

Örnek:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` Özniteliği, belirli bir önbellek bekletme düzeyini garanti etmez. `CacheItemPriority` yalnızca bir öneridir. Bu öznitelik ayarını `NeverRemove` önbelleğe alınmış öğeleri her zaman korunur garanti etmez. Konular, bkz: [ek kaynaklar](#additional-resources) bölümünde daha fazla bilgi için.

Önbellek etiketi Yardımcısı bağlıdır [bellek önbellek hizmeti](xref:performance/caching/memory). Önbellek etiketi Yardımcısı eklenmemiş olan hizmet ekler.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
