---
title: ASP.NET core'da düzeni
author: ardalis
description: Yaygın düzenlerini kullanmayı, yönergeleri paylaşın ve işleme görünümleri önce ortak kod içinde ASP.NET Core uygulaması çalıştırma hakkında bilgi edinin.
ms.author: riande
ms.date: 10/18/2018
uid: mvc/views/layout
ms.openlocfilehash: 1bd225c804b333efea834a46b7d9ba46b1bb69d8
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410579"
---
# <a name="layout-in-aspnet-core"></a>ASP.NET core'da düzeni

Tarafından [Steve Smith](https://ardalis.com/) ve [Dave Brock](https://twitter.com/daveabrock)

Sayfalar ve görünümler sık görsel ve programlama öğeleri paylaşın. Bu makalede gösterilmiştir nasıl yapılır:

* Ortak düzenler kullanın.
* Yönergeleri paylaşın.
* İşleme sayfaları veya görünümleri önce ortak kodu çalıştırın.

Bu belge düzenleri için ASP.NET Core MVC iki farklı yaklaşım açıklanmaktadır: Razor sayfaları ve görünüm denetleyicileri. Bu konu için en az bir fark vardır:

* Razor sayfaları bulunduğunuz *sayfaları* klasör.
* Görünümler kullanan denetleyicileriyle bir *görünümleri* görünümleri için klasör.

## <a name="what-is-a-layout"></a>Bir düzen nedir

Çoğu web uygulaması, sayfalar arasında gezinirken, kullanıcı ile tutarlı bir deneyim sağlayan ortak bir düzeni vardır. Düzen, genellikle Uygulama Başlığı, gezinti veya menü öğeleri ve alt bilgi gibi ortak kullanıcı arabirimi öğeleri içerir.

![Sayfa düzeni örneği](layout/_static/page-layout.png)

Betikleri ve stil sayfalarını gibi ortak HTML yapıları, bir uygulama içinde birçok sayfaları da sık sık kullanılır. Tüm bu paylaşılan öğeleri içinde tanımlanabilir bir *Düzen* dosya, uygulama içinde kullanılan herhangi bir görünüm tarafından başvurulabilir. Düzenleri görünümleri yinelenen kodları azaltabilir.

Kural gereği, ASP.NET Core uygulaması için varsayılan düzen adlı *_Layout.cshtml*. Düzen dosyası şablonları ile oluşturulan yeni ASP.NET Core projeleri için:

* Razor sayfaları için: *Pages/Shared/_Layout.cshtml*

  ![Çözüm Gezgini sayfalar klasöründe](layout/_static/rp-web-project-views.png)

* Görünüm denetleyicisi: *Views/Shared/_Layout.cshtml*

 ![Çözüm Gezgini klasöründe görünümleri](layout/_static/mvc-web-project-views.png)

Düzen görünümleri için üst düzey şablon uygulamada tanımlar. Uygulamaları bir düzen gerektirmez. Uygulamaları farklı görünümler alan farklı düzenler belirten birden fazla Düzen tanımlayabilirsiniz.

Aşağıdaki kod projesi bir denetleyici ve görünümler ile oluşturulmuş bir şablonu Düzen dosyası gösterir:

[!code-html[](~/common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=44,72)]

## <a name="specifying-a-layout"></a>Bir düzen belirtme

Razor görünümleri olan bir `Layout` özelliği. Tek bir görünüm bu özelliğini ayarlayarak bir düzen belirtin:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Belirtilen düzen bir tam yol kullanabilirsiniz (örneğin, */Pages/Shared/_Layout.cshtml* veya */Views/Shared/_Layout.cshtml*) ya da kısmi bir ad (örnek: `_Layout`). Kısmi bir adı sağlandığında, Razor görünüm altyapısını kullanarak kendi standart bulma işlemi için yerleşim dosyası arar. İşleyici yöntemi (veya denetleyicisi) bulunduğu klasör, ilk olarak, arkasından aranır *paylaşılan* klasör. Bu bulma işlemi için keşfetmek için kullanılan bir aynıdır [kısmi görünümler](partial.md).

Varsayılan olarak, her Düzen çağırmalıdır `RenderBody`. Her yerde çağrısı `RenderBody` olan konumdaki görünüm içeriğinin işlenir.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Bölümler

Bir düzen, isteğe bağlı olarak bir veya daha fazla başvurabilirsiniz *bölümleri*, çağırarak `RenderSection`. Bölümler, belirli sayfa öğeleri nereye yerleştirileceğini düzenlemek için bir yol sağlar. Her çağrı `RenderSection` bu bölümün gerekli veya isteğe bağlı olup olmadığını belirtebilirsiniz:

```html
@section Scripts {
    @RenderSection("Scripts", required: false)
}
```

Gerekli bölüm bulunamazsa, bir özel durum oluşturulur. Tek bir görünüm içinde bir bölümde kullanılarak oluşturulması için içeriği belirtin `@section` Razor söz dizimi. Bir sayfa ya da görünümün bir bölüm tanımlar, işlenen gerekir (veya bir hata meydana gelir).

Bir örnek `@section` Razor sayfaları görünüm tanımında:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
}
```

Önceki kodda, *scripts/main.js* eklenir `scripts` bir sayfa ya da Görünüm bölümü. Diğer sayfaları veya görünümleri aynı uygulamada bu betik gerekli değil ve betikleri bölüm tanımlayın mıydı.

Aşağıdaki biçimlendirmede kullanan [kısmi etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) işlenecek *_ValidationScriptsPartial.cshtml*:

```html
@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

Önceki biçimlendirme tarafından oluşturulmuş [kimlik iskele kurma özelliği](xref:security/authentication/scaffold-identity).

Bir sayfa veya görünümde tanımlı bölüm, yalnızca kendi anlık düzen sayfası içinde kullanılabilir. Bunlar, kısmi görünüm bileşenleri veya Görünüm sistemin diğer bölümlerini başvurulamaz.

### <a name="ignoring-sections"></a>Bölümler yoksayılıyor

Varsayılan olarak, gövdesini ve içerik sayfasındaki tüm bölümlerin tümünü düzen sayfası tarafından oluşturulması gerekir. Razor görüntüleme motorunu gövdesi ve her bölümde oluşturulmasını isteyip izleyerek zorlar.

Gövde veya bölüm yok saymak için Görünüm altyapısı açmasını sağlamak için çağrı `IgnoreBody` ve `IgnoreSection` yöntemleri.

Gövde ve her bölümde bir Razor sayfası işlenen yoksayıldı veya gerekir.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Paylaşılan yönergeleri alma

Görünümlere ve sayfalara Razor ad alanları ve kullanım içeri aktarma yönergelerini kullanabilirsiniz [bağımlılık ekleme](dependency-injection.md). Yönergeleri çoğu görünümler tarafından paylaşılan ortak belirtilen *_viewımports.cshtml* dosya. `_ViewImports` Dosyasını aşağıdaki yönergeleri destekler:

* `@addTagHelper`
* `@removeTagHelper`
* `@tagHelperPrefix`
* `@using`
* `@model`
* `@inherits`
* `@inject`

Dosya, İşlevler ve bölüm tanımları gibi diğer Razor özellikleri desteklemez.

Bir örnek `_ViewImports.cshtml` dosyası:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

*_Viewımports.cshtml* bir ASP.NET Core MVC uygulaması genellikle yerleştirilir için dosya *sayfaları* (veya *görünümleri*) klasörü. A *_viewımports.cshtml* herhangi bir klasör içinde dosya yerleştirilebileceğini, bu durumda, yalnızca sayfa veya görünümler bu klasöre ve alt klasörleri içinde uygulanır. `_ViewImports` dosyaları kök düzeyinde ve ardından sayfanın konumunu öncesinde her klasör için başlangıç işlenir veya kendisini görüntüleyin. `_ViewImports` kök düzeyindeki ayarları klasör düzeyinde geçersiz kılınabilir.

Örneğin, varsayalım:

* Kök düzeyinde *_viewımports.cshtml* dosyasını içeren `@model MyModel1` ve `@addTagHelper *, MyTagHelper1`.
* Bir alt klasör *_viewımports.cshtml* dosyasını içeren `@model MyModel2` ve `@addTagHelper *, MyTagHelper2`.

Sayfalar ve görünümler alt iki etiket Yardımcıları erişimi olacaktır ve `MyModel2` modeli.

Birden çok *_viewımports.cshtml* yönergeleri birleşik davranışını olan dosyaları dosya hiyerarşide bulunur:

* `@addTagHelper`, `@removeTagHelper`: sırayla tüm çalışma
* `@tagHelperPrefix`: en yakındakine görünümüne başka geçersiz kılar.
* `@model`: en yakındakine görünümüne başka geçersiz kılar.
* `@inherits`: en yakındakine görünümüne başka geçersiz kılar.
* `@using`: tüm; dahildir yinelenenler yoksayıldı
* `@inject`: her bir özellik için en yakın bir görünüm için aynı adla başkalarıyla geçersiz kılar

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Her görünüm önce kod çalıştırma

Her görünüm veya sayfa önce çalıştırmak için gereken kodu yerleştirilmelidir *_ViewStart.cshtml* dosya. Kural olarak, *_ViewStart.cshtml* dosyası *sayfaları* (veya *görünümleri*) klasörü. Listelenen deyimleri *_ViewStart.cshtml* önce her tam görünüm (değil düzenleri ve kısmi görünümler) çalıştırın. Gibi [ViewImports.cshtml](xref:mvc/views/layout#viewimports), *_ViewStart.cshtml* hiyerarşik olduğu anlamına gelir. Varsa bir *_ViewStart.cshtml* dosya tanımlanır görünümü veya sayfalar klasöründe, kök dizininde tanımlananla sonra çalıştırılacak *sayfaları* (veya *görünümleri*) klasörü (varsa).

Bir örnek *_ViewStart.cshtml* dosyası:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Yukarıdaki dosyanın tüm görünümlere kullanacağını belirtir *_Layout.cshtml* düzeni.

*_ViewStart.cshtml* ve *_viewımports.cshtml* olan **değil** genellikle yerleştirilen */sayfaları/paylaşılan* (veya   */görünümler/paylaşılan*) klasör. Uygulama düzeyinde bu dosyaların sürümleri doğrudan yerleştirilmelidir */sayfaları* (veya */görünümler*) klasörü.
