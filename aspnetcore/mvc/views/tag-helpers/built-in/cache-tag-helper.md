---
title: ASP.NET Core MVC 'de önbellek etiketi Yardımcısı
author: pkellner
description: Önbellek etiketi yardımcısını kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: db9e1a968588410f11e5f137dfdd4542df505ebc
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662738"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>ASP.NET Core MVC 'de önbellek etiketi Yardımcısı

By [Peter Kellner](https://peterkellner.net)

Önbellek etiketi Yardımcısı, içeriğini dahili ASP.NET Core önbellek sağlayıcısına önbelleğe alarak ASP.NET Core uygulamanızın performansını iyileştirebilme olanağı sağlar.

Etiket Yardımcıları hakkında genel bilgi için bkz. <xref:mvc/views/tag-helpers/intro>.

Şu Razor işaretlemesi geçerli tarihi önbelleğe alır:

```cshtml
<cache>@DateTime.Now</cache>
```

Etiket Yardımcısını içeren sayfanın ilk isteği geçerli tarihi görüntüler. Önbellek süresi dolana kadar veya önbelleğe alınmış tarih önbellekten çıkarılana kadar ek istekler önbelleğe alınmış değeri gösterir.

## <a name="cache-tag-helper-attributes"></a>Önbellek etiketi yardımcı öznitelikleri

### <a name="enabled"></a>enabled

| Öznitelik türü  | Örnekler        | Varsayılan |
| --------------- | --------------- | ------- |
| Boole         | `true`, `false` | `true`  |

`enabled`, önbellek etiketi Yardımcısı tarafından eklenen içeriğin önbelleğe alınıp alınmayacağını belirler. Varsayılan değer: `true`. `false`olarak ayarlanırsa, işlenen çıktı önbelleğe **alınmaz** .

Örnek:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-on"></a>süre sonu-açık

| Öznitelik türü   | Örnek                            |
| ---------------- | ---------------------------------- |
| `DateTimeOffset` | `@new DateTime(2025,1,29,17,02,0)` |

`expires-on`, önbelleğe alınmış öğe için mutlak bir sona erme tarihi ayarlar.

Aşağıdaki örnek, 29 Ocak 2025 ' de 5:02 PM 'e kadar önbellek etiketi Yardımcısı 'nın içeriğini önbelleğe alır:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a>süre sonu-sonra

| Öznitelik türü | Örnek                      | Varsayılan    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | 20 dakika |

`expires-after`, içeriği önbelleğe almak için ilk istek zamanından itibaren geçen süreyi belirler.

Örnek:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Razor Görünüm altyapısı varsayılan `expires-after` değerini yirmi dakika olarak ayarlar.

### <a name="expires-sliding"></a>süre sonu-kayan

| Öznitelik türü | Örnek                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

Değerine erişilmediyse önbellek girişinin çıkarılme süresini ayarlar.

Örnek:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a>üst bilgiye göre değişiklik

| Öznitelik türü | Örnekler                                    |
| -------------- | ------------------------------------------- |
| String         | `User-Agent`, `User-Agent,content-encoding` |

`vary-by-header`, değiştiğinde önbellek yenilemeyi tetikleyen üst bilgi değerlerinin virgülle ayrılmış bir listesini kabul eder.

Aşağıdaki örnek `User-Agent`üst bilgi değerini izler. Örnek, Web sunucusuna sunulan her farklı `User-Agent` için içeriği önbelleğe alır:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a>sorguya göre değişiklik

| Öznitelik türü | Örnekler             |
| -------------- | -------------------- |
| String         | `Make`, `Make,Model` |

`vary-by-query`, listelenen herhangi bir anahtarın değeri değiştiğinde önbellek yenilemeyi tetikleyen bir sorgu dizesindeki (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> virgülle ayrılmış bir listesini kabul eder.

Aşağıdaki örnek, `Make` ve `Model`değerlerini izler. Örnek, Web sunucusuna sunulan her farklı `Make` ve `Model` için içeriği önbelleğe alır:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a>Yönlendirme ölçütü

| Öznitelik türü | Örnekler             |
| -------------- | -------------------- |
| String         | `Make`, `Make,Model` |

`vary-by-route`, yol verileri parametre değeri değiştiğinde önbellek yenilemeyi tetikleyen yol parametresi adlarının virgülle ayrılmış listesini kabul eder.

Örnek:

*Startup.cs*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

*Index. cshtml*:

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-cookie"></a>tanımlama bilgisine göre farklılık

| Öznitelik türü | Örnekler                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| String         | `.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor` |

`vary-by-cookie`, tanımlama bilgisi değerleri değiştiğinde önbellek yenilemeyi tetikleyen tanımlama bilgisi adlarının virgülle ayrılmış listesini kabul eder.

Aşağıdaki örnek ASP.NET Core kimlikle ilişkili tanımlama bilgisini izler. Bir kullanıcının kimliği doğrulandığında, kimlik tanımlama bilgisindeki bir değişiklik önbellek yenilemeyi tetikler:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a>kullanıcıya göre değişiklik

| Öznitelik türü  | Örnekler        | Varsayılan |
| --------------- | --------------- | ------- |
| Boole         | `true`, `false` | `true`  |

`vary-by-user`, oturum açan kullanıcının (veya bağlamı asıl) değiştiği zaman önbelleğin sıfırlamayacağını belirtir. Geçerli Kullanıcı, Istek bağlamı sorumlusu olarak da bilinir ve `@User.Identity.Name`başvurarak Razor görünümünde görüntülenebilir.

Aşağıdaki örnek, bir önbellek yenilemeyi tetiklemek için geçerli oturum açan kullanıcıyı izler:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Bu özniteliğin kullanılması, oturum açma ve oturum kapatma döngüsüyle önbellekteki içerikleri saklar. Değer `true`olarak ayarlandığında, bir kimlik doğrulama çevrimi kimliği doğrulanmış kullanıcı için önbelleği geçersiz kılar. Bir kullanıcının kimliği doğrulandığında yeni bir benzersiz tanımlama bilgisi değeri oluşturulduğundan önbellek geçersiz kılındı. Bir tanımlama bilgisi yoksa veya tanımlama bilgisinin süresi dolduğunda önbellek, anonim durum için korunur. Kullanıcının kimliği **doğrulanmıyorsa** , önbellek korunur.

### <a name="vary-by"></a>değişiklik ölçütü-

| Öznitelik türü | Örnek  |
| -------------- | -------- |
| String         | `@Model` |

`vary-by` hangi verilerin önbelleğe alınacağını özelleştirmek için izin verir. Özniteliğin dize değeri tarafından başvurulan nesne değiştiğinde, önbellek etiketi Yardımcısı 'nın içeriği güncelleştirilir. Genellikle, model değerlerinin dize birleştirmesi bu özniteliğe atanır. Etkin olarak, bu, herhangi bir birleştirilmiş değerden bir güncelleştirmenin önbelleği geçersiz hale getirildiği bir senaryoya neden olur.

Aşağıdaki örnek, görünümü işleyen denetleyici yönteminin, `myParam1` ve `myParam2`iki yol parametresi tamsayı değerini toplamasını ve toplamı tek model özelliği olarak geri döndürdüğünü varsayar. Bu toplam değiştiğinde, önbellek etiketi Yardımcısı 'nın içeriği işlenir ve yeniden önbelleğe alınır.  

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

*Index. cshtml*:

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a>öncelik

| Öznitelik türü      | Örnekler                               | Varsayılan  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | `High`, `Low`, `NeverRemove`, `Normal` | `Normal` |

`priority` yerleşik önbellek sağlayıcısına önbellek çıkarma kılavuzu sağlar. Web sunucusu, bellek baskısı altında olduğunda önce önbellek girdilerini `Low` çıkarşır.

Örnek:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` özniteliği belirli bir önbellek saklama düzeyini garanti etmez. `CacheItemPriority` yalnızca bir öneridir. Bu özniteliği `NeverRemove` olarak ayarlamak, önbelleğe alınmış öğelerin her zaman korunduğunu garanti etmez. Daha fazla bilgi için [ek kaynaklar](#additional-resources) bölümündeki konulara bakın.

Önbellek etiketi Yardımcısı, [bellek önbelleği hizmetine](xref:performance/caching/memory)bağımlıdır. Önbellek etiketi Yardımcısı eklenmemişse hizmeti ekler.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
