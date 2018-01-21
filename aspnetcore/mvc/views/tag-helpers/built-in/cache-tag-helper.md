---
title: "ASP.NET Core MVC etiketi yardımcı önbelleğe alma"
author: pkellner
description: "Önbellek etiket Yardımcısı ile çalışmaya nasıl gösterir"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: dfd9c3c0c4e50a99e4f8703b01bd9b384930b87a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>ASP.NET Core MVC etiketi yardımcı önbelleğe alma

Tarafından [Peter Kellner](http://peterkellner.net) 

Önbellek etiket Yardımcısı iç ASP.NET Core önbelleği sağlayıcısı için içeriği önbelleğe alarak, ASP.NET Core uygulamanızın performansını önemli ölçüde artırmak için yeteneği sağlar.

Razor görüntüleme altyapısı varsayılan ayarlar `expires-after` yirmi dakika.

Aşağıdaki Razor biçimlendirme tarih/saat önbelleğe alır:

```cshtml
<cache>@DateTime.Now</cache>
```

İçeren sayfaya yapılan ilk istek `CacheTagHelper` geçerli tarih görüntülenir. Önbellek (varsayılan 20 dakika) süresi dolana veya bellek baskısı tarafından çıkarılacak kadar ek istekleri önbelleğe alınan değeri gösterir.

Aşağıdaki özniteliklerle önbellek süresini ayarlayabilirsiniz:

## <a name="cache-tag-helper-attributes"></a>Etiket Yardımcısı özniteliklerini önbelleğe alma

- - -

### <a name="enabled"></a>Etkin    


| Öznitelik türü    | Geçerli Değerler      |
|----------------   |----------------   |
| Boole değeri           | "true" (varsayılan)  |
|                   | "false"   |


Önbellek etiket Yardımcısı tarafından içine içeriğin önbellekte olup olmadığını belirler. Varsayılan, `true` değeridir.  Varsa kümesine `false` bu önbelleği etiket Yardımcısı işlenmiş çıktıyı önbelleğe alma hiçbir etkisi olmaz.

Örnek:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| Öznitelik türü    | Örnek değer     |
|----------------   |----------------   |
| DateTimeOffset    | "@new DateTime(2025,1,29,17,02,0)"    |


Bir mutlak sona erme tarihi ayarlar. Aşağıdaki örnek, 17:02:00 saatleri 29 Ocak 2025 üzerinde kadar önbellek etiket Yardımcısı içeriğini önbelleğe alır.

Örnek:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| Öznitelik türü    | Örnek değer     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(120)"    |


Süre içeriği önbelleğe almak için ilk istek saati ayarlar. 

Örnek:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>expires-sliding

| Öznitelik türü    | Örnek değer     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(60)"     |


Değil erişilen, önbellek girişi çıkarılacak süreyi ayarlar.

Örnek:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>farklılık tarafından-üstbilgisi

| Öznitelik türü    | Örnek değerler                |
|----------------   |----------------               |
| Dize            | "User-Agent"                  |
|                   | "User-Agent, içerik kodlaması" |

Bir tek üstbilgi değeri veya bir önbellek yenileme bunlar değiştirdiğinizde tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder. Aşağıdaki örnek üstbilgi değeri izler `User-Agent`. Bu örnek için içeriği önbelleğe alır her farklı `User-Agent` web sunucusuna sunulur.

Örnek:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>farklılık-by-sorgusu

| Öznitelik türü    | Örnek değerler                |
|----------------   |----------------               |
| Dize            | "Oluştur"                |
|                   | "Marka, Model" |

Bir tek üstbilgi değeri veya bir önbellek yenileme üstbilgi değeri değiştiğinde tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder. Aşağıdaki örnek değerlere görünmesi `Make` ve `Model`.

Örnek:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>farklılık tarafından-yönlendirme

| Öznitelik türü    | Örnek değerler                |
|----------------   |----------------               |
| Dize            | "Oluştur"                |
|                   | "Marka, Model" |

Bir tek üstbilgi değeri veya değişiklik rota veri parametresinin Değer yükleyen bir önbellek yenileme tetiklemek üstbilgi değerlerini virgülle ayrılmış listesini kabul eder. Örnek:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>farklılık-tarafından-tanımlama bilgisi

| Öznitelik türü    | Örnek değerler                |
|----------------   |----------------               |
| Dize            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

Bir tek üstbilgi değeri veya bir önbellek yenileme (s) üstbilgi değerleri değiştiğinde tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder. Aşağıdaki örnek, ASP.NET kimliği ile ilişkili tanımlama bakar. Bir kullanıcının kimliği doğrulanır zaman, bir önbellek yenileme tetikler ayarlanacak istek tanımlama bilgisi.

Örnek:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>farklı kullanıcı tarafından

| Öznitelik türü    | Örnek değerler                |
|----------------   |----------------               |
| Boole değeri             | "true"                  |
|                     | "false" (varsayılan) |

Oturum açma kullanıcı (veya içerik asıl) değiştiğinde önbelleği sıfırlamalıdır olup olmadığını belirtir. Geçerli kullanıcı olarak da bilinen istek bağlamı asıl ve Razor görünümünde başvurarak görüntülenebilir `@User.Identity.Name`.

Aşağıdaki örnek, geçerli kullanıcının oturum bakar.  

Örnek:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Bu öznitelik kullanarak oturum açması ve günlük genişletme döngüsü boyunca Önbellekteki içeriği tutar.  Kullanırken `vary-by-user="true"`, bir oturum açma ve oturum genişletme eylemi önbellek kimliği doğrulanmış kullanıcı için geçersiz kılar.  Yeni bir benzersiz tanımlama bilgisi değerini oturum açma üretildiği için önbellek geçersiz kılınır. Tanımlama bilgisi mevcut olduğunda veya süresi dolmuş önbelleği için anonim durumu korunur. Bu, hiçbir kullanıcı oturum açtıysa, önbellek sürdürüleceği anlamına gelir.

- - -

### <a name="vary-by"></a>farklılık tarafından

| Öznitelik türü    | Örnek değerler                |
|----------------   |----------------               |
| Dize             | "@Model"                 |


Hangi verileri önbelleğe, özelleştirme için sağlar. Önbellek etiket Yardımcısı içeriğini özniteliğin dize değeri değişiklikleri tarafından başvurulan nesne güncelleştirildiğinde. Genellikle dize birleştirme modeli değerlerin bu özniteliğe atanır.  Etkili bir şekilde birleştirilmiş değerleri için bir güncelleştirme önbelleği geçersiz kılar anlamına gelir.

Aşağıdaki örnekte iki rota parametrelerinin tamsayı değerini görünüm SUM'ları oluşturma denetleyici yöntemi varsayar `myParam1` ve `myParam2`ve tek model özelliği döndürür. Bu toplamın değiştiğinde önbelleği etiket Yardımcısı içeriğini çizilir ve tekrar önbelleğe alınır.  

Örnek:

Eylem:

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>önceliği

| Öznitelik türü    | Örnek değerler                |
|----------------   |----------------               |
| CacheItemPriority  | "Yüksek"                   |
|                    | "Düşük" |
|                    | "NeverRemove" |
|                    | "Normal" |

Önbellek çıkarma kılavuzu için yerleşik önbelleği sağlayıcısı sağlar. Web sunucusu çıkarırsınız `Low` bellek baskısı altında olduğunda girişleri ilk önbelleğe.

Örnek:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` Özniteliği önbellek bekletme belirli bir düzeyde garanti etmez. `CacheItemPriority`yalnızca bir öneridir. Bu öznitelik ayarını `NeverRemove` önbelleği her zaman korunması garanti etmez. Bkz: [ek kaynaklar](#additional-resources) daha fazla bilgi için.

Önbellek etiket Yardımcısı bağlı [bellek önbellek hizmeti](xref:performance/caching/memory). Değil eklenmişse önbellek etiket Yardımcısı hizmet ekler.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
