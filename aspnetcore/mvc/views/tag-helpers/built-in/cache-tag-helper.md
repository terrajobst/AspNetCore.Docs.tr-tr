---
title: Önbellek etiketi Yardımcısı, ASP.NET Core MVC
author: pkellner
description: Önbellek etiketi Yardımcısı ile çalışma hakkında bilgi verilmektedir
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 425d8c2235f0070665bc0c967d2498f2cff2a4a6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754045"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Önbellek etiketi Yardımcısı, ASP.NET Core MVC

Tarafından [Peter Kellner](http://peterkellner.net) 

Önbellek etiketi Yardımcısı iç ASP.NET Core önbelleği sağlayıcısı için içeriği önbelleğe alarak, ASP.NET Core uygulamanızı performansını önemli ölçüde artırmak olanağı sağlar.

Razor görüntüleme motorunu varsayılan ayarlar `expires-after` yirmi dakika.

Aşağıdaki Razor işaretlemesi tarih/saat önbelleğe alır:

```cshtml
<cache>@DateTime.Now</cache>
```

İlk istek içeren sayfasına `CacheTagHelper` geçerli tarih/saat görüntülenir. Önbellek süresi (varsayılan 20 dakika) ya da veriler çıkarıldığında tarafından bellek baskısı kadar ek istekler önbelleğe alınan değeri gösterilir.

Aşağıdaki özniteliklerle önbellek süresi ayarlayabilirsiniz:

## <a name="cache-tag-helper-attributes"></a>Önbellek etiketi Yardımcısı öznitelikleri

- - -

### <a name="enabled"></a>Etkin    


| Öznitelik türü    | Geçerli Değerler      |
|----------------   |----------------   |
| Boole değeri           | "true" (varsayılan)  |
|                   | "false"   |


Önbellek etiketi Yardımcısı tarafından alınmış içeriği önbelleğe alınmış olup olmadığını belirler. Varsayılan, `true` değeridir.  Varsa kümesine `false` Bu önbellek etiketi Yardımcısı üzerinde işlenen çıkışı önbelleğe alma hiçbir etkisi olmaz.

Örnek:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>süresi dolmadan açma 

| Öznitelik türü |           Örnek değer            |
|----------------|------------------------------------|
| DateTimeOffset | "@new DateTime(2025,1,29,17,02,0)" |

Mutlak sona erme tarihini ayarlar. Aşağıdaki örnek, 17:02:00 29 Ocak 2025 üzerinde kadar önbellek etiketi Yardımcısı içeriğini önbelleğe alır.

Örnek:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>süresi dolduktan sonra

| Öznitelik türü |        Örnek değer         |
|----------------|------------------------------|
|    Zaman aralığı    | "@TimeSpan.FromSeconds(120)" |

İçeriği önbelleğe almak için ilk isteği zamanından sürenin uzunluğunu ayarlar. 

Örnek:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>süresi dolmadan kayan

| Öznitelik türü |        Örnek değer        |
|----------------|-----------------------------|
|    Zaman aralığı    | "@TimeSpan.FromSeconds(60)" |

Değil erişilen, önbellek girişi çıkarılacak süreyi ayarlar.

Örnek:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>Vary-tarafından-üstbilgisi

| Öznitelik türü    | Örnek değerler                |
|----------------   |----------------               |
| Dize            | "User-Agent"                  |
|                   | "User-Agent, content-encoding" |

Tek bir üst bilgi değeri ya da bunlar değiştirdiğinizde, önbellek yenileme tetiklemek üstbilgi değerlerini virgülle ayrılmış bir listesini kabul eder. Aşağıdaki örnekte üst bilgi değeri izler `User-Agent`. Örnek içeriğini önbelleğe alır her farklı `User-Agent` web sunucusuna sunulur.

Örnek:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>farklı-tarafından-sorgu

| Öznitelik türü    | Örnek değerler                |
|----------------   |----------------               |
| Dize            | "Oluştur"                |
|                   | "Marka, Model" |

Tek bir üst bilgi değeri veya üst bilgi değeri değiştiğinde bir önbellek yenileme tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder. Aşağıdaki örnekte değerlerinde görünür `Make` ve `Model`.

Örnek:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>farklı-tarafından-route

| Öznitelik türü    | Örnek değerler                |
|----------------   |----------------               |
| Dize            | "Oluştur"                |
|                   | "Marka, Model" |

Rota veri parametresinin Değer değişiklik olduğunda, önbellek yenileme tetiklemek üstbilgi değerlerini virgülle ayrılmış bir listesi veya bir tek bir üstbilgi değerini kabul eder. Örnek:

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

### <a name="vary-by-cookie"></a>farklı-tarafından-tanımlama bilgisi

| Öznitelik türü    | Örnek değerler                |
|----------------   |----------------               |
| Dize            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

Bir tek bir üstbilgi değerini veya bir önbellek yenileme (s) başlık değerleri değiştiğinde tetikleyen üstbilgi değerlerini virgülle ayrılmış listesini kabul eder. Aşağıdaki örnek, ASP.NET Core kimliği ile ilişkili tanımlama bakar. Ne zaman bir kullanıcının kimliği doğrulanır ve bu önbellek yenileme tetikler ayarlamak için istek tanımlama bilgisi.

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

Oturum açmış kullanıcı (veya bağlam sorumlusu) değiştiğinde önbelleği sıfırlamalısınız olup olmadığını belirtir. Geçerli kullanıcı olarak da bilinen istek bağlamı sorumlusu ve bir Razor Görünümü'nde başvurarak görüntülenebilir `@User.Identity.Name`.

Aşağıdaki örnek, geçerli kullanıcı oturum bakar.  

Örnek:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Bu özniteliği kullanarak içerikleri bir oturum açma ve günlük genişletme döngüsü boyunca önbellekte tutar.  Kullanırken `vary-by-user="true"`, önbellek kimliği doğrulanmış kullanıcı için bir oturum açma ve günlük genişletme eylemi geçersiz kılar.  Oturum açma için yeni bir benzersiz tanımlama bilgisi değeri güvenle önbellek geçersiz kılınır. Tanımlama bilgisi varsa ya da süresi dolmuş önbellek için anonim durumu korunur. Bu, hiçbir kullanıcı oturum açtıysa, önbellekte tutulan anlamına gelir.

- - -

### <a name="vary-by"></a>değişiklik tarafından

| Öznitelik türü | Örnek değerler |
|----------------|----------------|
|     Dize     |    "@Model"    |

Hangi veriler önbelleğe, özelleştirme için sağlar. Önbellek etiketi Yardımcısı içeriğini özniteliğin dize değeri değiştiğinde tarafından başvurulan nesne güncelleştirildiğinde. Genellikle model değerlerinin dize birleştirme bu özniteliğe atanır.  Etkili bir şekilde önbelleğe nesnelerindeki değerleri herhangi bir güncelleştirme geçersiz kılar anlamına gelir.

Aşağıdaki örnek iki yol parametreleri, tamsayı değeri görünümü SUM'ları oluşturma denetleyici yöntemi varsayar `myParam1` ve `myParam2`ve tek bir model özelliği döndürür. Bu toplam değiştiğinde, önbellek etiketi Yardımcısı içeriğini oluşturulur ve tekrar önbelleğe alınmış.  

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
| CacheltemPriority  | "Yüksek"                   |
|                    | "Düşük" |
|                    | "NeverRemove" |
|                    | "Normal" |

Yerleşik önbelleği sağlayıcısı için önbellek çıkarma rehberlik sağlar. Web sunucusu çıkarırsınız `Low` bellek baskısı altında olduğunda girişleri ilk önbellek.

Örnek:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` Özniteliği, belirli bir önbellek bekletme düzeyini garanti etmez. `CacheItemPriority` yalnızca bir öneridir. Bu öznitelik ayarını `NeverRemove` önbelleği her zaman korunması garanti etmez. Bkz: [ek kaynaklar](#additional-resources) daha fazla bilgi için.

Önbellek etiketi Yardımcısı bağlıdır [bellek önbellek hizmeti](xref:performance/caching/memory). Önbellek etiketi Yardımcısı henüz eklenmemiş bir hizmet ekler.

## <a name="additional-resources"></a>Ek kaynaklar

* [Belleğe yüklenmiş önbellek](xref:performance/caching/memory)
* [Kimliğe giriş](xref:security/authentication/identity)
