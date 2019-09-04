---
title: ASP.NET Core düzen
author: ardalis
description: Bir ASP.NET Core uygulamasında görünümler işlemeden önce ortak düzenleri kullanmayı, yönergeleri paylaşmayı ve ortak kodu çalıştırmayı öğrenin.
ms.author: riande
ms.date: 07/30/2019
uid: mvc/views/layout
ms.openlocfilehash: 9a7b8003b24329f6e9cbd349ee47f6844b7c5f6d
ms.sourcegitcommit: e6bd2bbe5683e9a7dbbc2f2eab644986e6dc8a87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70238037"
---
# <a name="layout-in-aspnet-core"></a>ASP.NET Core düzen

[Steve Smith](https://ardalis.com/) ve [bave Brock](https://twitter.com/daveabrock) tarafından

Sayfalar ve görünümler genellikle görsel ve programlı öğeleri paylaşır. Bu makalede nasıl yapılacağı gösterilmektedir:

* Ortak düzenleri kullanın.
* Komutları paylaşma.
* Sayfaları veya görünümleri işlemeden önce ortak kodu çalıştırın.

Bu belgede, ASP.NET Core MVC 'nin iki farklı yaklaşımının düzenleri ele alınmaktadır: Görünümler içeren Razor Pages ve denetleyiciler. Bu konu için, farklar en az:

* Razor Pages, *Sayfalar* klasöründedir.
* Görünümleri olan denetleyiciler görünümler için bir *Görünümler* klasörü kullanır.

## <a name="what-is-a-layout"></a>Düzen nedir?

Çoğu Web uygulaması, bir sayfadan sayfaya gezindikleri sürece kullanıcıya tutarlı bir deneyim sağlayan ortak bir düzene sahiptir. Düzen genellikle uygulama üstbilgisi, gezinti veya menü öğeleri ve alt bilgi gibi ortak kullanıcı arabirimi öğelerini içerir.

![Sayfa düzeni örneği](layout/_static/page-layout.png)

Betikler ve stil sayfaları gibi ortak HTML yapıları de bir uygulama içindeki birçok sayfa tarafından sık kullanılır. Bu paylaşılan öğelerin tümü, bir *Düzen* dosyasında tanımlanabilir ve bu daha sonra uygulama içinde kullanılan herhangi bir görünüm tarafından başvurulabilirler. Düzenler görünümlerde yinelenen kodu azaltır.

Kurala göre, bir ASP.NET Core uygulamasının varsayılan düzeni *_Layout. cshtml*olarak adlandırılmıştır. Şablonlarla oluşturulan yeni ASP.NET Core projelerine yönelik düzen dosyaları şunlardır:

* Razor Pages: *Pages/Shared/_Layout. cshtml*

  ![Çözüm Gezgini sayfa klasörü](layout/_static/rp-web-project-views.png)

* Görünümler içeren denetleyici: *Görünümler/paylaşılan/_Layout. cshtml*

  ![Çözüm Gezgini içindeki görünümler klasörü](layout/_static/mvc-web-project-views.png)

Düzen, uygulamadaki görünümler için üst düzey bir şablon tanımlar. Uygulamalar bir düzen gerektirmez. Uygulamalar, farklı düzenleri belirleyen farklı görünümlerle birden fazla düzen tanımlayabilir.

Aşağıdaki kod, bir şablon tarafından oluşturulan ve bir denetleyici ve görünümleri olan bir proje için Düzen dosyasını gösterir:

[!code-cshtml[](~/common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=44,72)]

## <a name="specifying-a-layout"></a>Düzen belirtme

Razor görünümlerinin bir `Layout` özelliği vardır. Bireysel görünümler bu özelliği ayarlayarak bir düzen belirtir:

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Belirtilen Düzen tam yol kullanabilir (örneğin, */Pages/Shared/_Layout.exe* veya */views/Shared/_Layout.exe*) veya kısmi bir ad (örnek: `_Layout`). Kısmi bir ad sağlandığında, Razor görüntüleme altyapısı, kendi standart bulma işlemini kullanarak düzen dosyasını arar. Önce işleyici yönteminin (veya denetleyicinin) bulunduğu klasör, sonra *paylaşılan* klasör tarafından aranır. Bu bulma işlemi, [kısmi görünümleri](xref:mvc/views/partial#partial-view-discovery)bulmak için kullanılan işlemle aynıdır.

Varsayılan olarak, tüm mizanpajın çağırması `RenderBody`gerekir. Çağrısının `RenderBody` yerleştirildiği her yerde, görünümün içerikleri işlenir.

<a name="layout-sections-label"></a>
<!-- https://stackoverflow.com/questions/23327578 -->
### <a name="sections"></a>Bölümler

Bir düzen, çağırarak `RenderSection`, isteğe bağlı olarak bir veya daha fazla *bölüme*başvurabilir. Bölümler, belirli sayfa öğelerinin yerleştirilmesi gereken yerleri düzenlemek için bir yol sağlar. Her çağrısı `RenderSection` , bu bölümün gerekli veya isteğe bağlı olup olmadığını belirtebilir:

```html
@section Scripts {
    @RenderSection("Scripts", required: false)
}
```

Gerekli bir bölüm bulunamazsa, bir özel durum oluşturulur. Tek görünümler, `@section` Razor söz dizimi kullanarak bir bölüm içinde işlenecek içeriği belirtir. Bir sayfa veya görünüm bir bölümü tanımlıyorsa, oluşturulması gerekir (veya bir hata oluşur).

Razor Pages görünümünde `@section` örnek tanım:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
}
```

Yukarıdaki kodda *betikler/Main. js* , bir sayfa veya görünümdeki `scripts` bölümüne eklenir. Aynı uygulamadaki diğer sayfalar veya görünümler bu betiği gerektirmeyebilir ve betikler bölümü tanımlamaz.

Aşağıdaki biçimlendirme, *_Validationscriptspartial. cshtml*Işlemek Için [kısmi etiket yardımcısını](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) kullanır:

```html
@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

Önceki biçimlendirme, [Yapı Iskelesi kimliği](xref:security/authentication/scaffold-identity)tarafından oluşturulmuştur.

Bir sayfada veya görünümde tanımlanan bölümler yalnızca kendi düzen sayfasında kullanılabilir. Parçalardan başvurulamaz, bileşenleri veya görünüm sisteminin diğer kısımlarını bunlara başvuramaz.

### <a name="ignoring-sections"></a>Bölümler yoksayılıyor

Varsayılan olarak, içerik sayfasındaki gövde ve tüm bölümler Düzen sayfası tarafından işlenmelidir. Razor görünümü altyapısı, gövdenin ve her bölümün işlenip işlenmeyeceğini izleyerek bunu zorlar.

Görünüm altyapısına gövde veya bölümleri yok saymasını bildirmek için `IgnoreBody` ve `IgnoreSection` yöntemlerini çağırın.

Bir Razor sayfasındaki gövde ve her bölüm işlenen ya da yoksayıldı olmalıdır.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Paylaşılan yönergeler içeri aktarılıyor

Görünümler ve sayfalar, ad alanlarını içeri aktarmak ve [bağımlılık ekleme](dependency-injection.md)'yi kullanmak için Razor yönergeleri kullanabilir. Birçok görünüm tarafından paylaşılan yönergeler, ortak bir *_Viewwimports. cshtml* dosyasında belirtilebilir. `_ViewImports` Dosya aşağıdaki yönergeleri destekler:

* `@addTagHelper`
* `@removeTagHelper`
* `@tagHelperPrefix`
* `@using`
* `@model`
* `@inherits`
* `@inject`

Dosya, işlevler ve bölüm tanımları gibi diğer Razor özelliklerini desteklemez.

Örnek `_ViewImports.cshtml` dosya:

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

ASP.NET Core MVC uygulaması için *_Viewwimports. cshtml* dosyası genellikle *Sayfalar* (veya *Görünümler*) klasörüne yerleştirilir. Bir *_Viewwimports. cshtml* dosyası herhangi bir klasöre yerleştirilebilir, bu durumda yalnızca bu klasör ve alt klasörleri içindeki sayfalara veya görünümlere uygulanır. `_ViewImports`dosyalar, kök düzeyinden başlayarak işlenir ve sonra her bir klasör için sayfanın konumu veya görünümü görüntülenir. `_ViewImports`kök düzeyinde belirtilen ayarlar klasör düzeyinde geçersiz kılınabilir.

Örneğin, şunu varsayın:

* Kök düzeyi *_viewwimports. cshtml* dosyası ve `@addTagHelper *, MyTagHelper1`içerir `@model MyModel1` .
* Alt klasör *_viewwimports. cshtml* dosyası ve `@model MyModel2` `@addTagHelper *, MyTagHelper2`içerir.

Alt klasördeki sayfaların ve görünümlerin her ikisi de etiket yardımcılarını ve `MyModel2` modeline erişimi olur.

Dosya hiyerarşisinde birden çok *_Viewimports. cshtml* dosyası bulunursa, yönergelerin birleştirilmiş davranışı şunlardır:

* `@addTagHelper`, `@removeTagHelper`: tüm çalıştırma, sırasıyla
* `@tagHelperPrefix`: görünümün en yakın olanı, diğerlerini geçersiz kılar
* `@model`: görünümün en yakın olanı, diğerlerini geçersiz kılar
* `@inherits`: görünümün en yakın olanı, diğerlerini geçersiz kılar
* `@using`: tümü dahildir; yinelemeler yoksayıldı
* `@inject`: her bir özellik için, görünümün en yakın olanı aynı özellik adına sahip diğer diğerlerini geçersiz kılar

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Her görünümden önce kod çalıştırma

Her görünüm veya sayfadan önce çalıştırılması gereken kodun *_Viewstart. cshtml* dosyasına yerleştirilmesi gerekir. Kural gereği, *_Viewstart. cshtml* dosyası *Sayfalar* (veya *Görünümler*) klasöründe bulunur. *_Viewstart. cshtml* dosyasında listelenen deyimler her tam görünüm (düzen değil ve kısmi görünümler değil) öncesinde çalıştırılır. [Viewwimports. cshtml](xref:mvc/views/layout#viewimports)gibi, *_viewstart. cshtml* ise hiyerarşik bir görünüm olur. Görünüm veya sayfalar klasöründe bir *_Viewstart. cshtml* dosyası tanımlanmışsa, *Sayfalar* (veya *Görünümler*) klasörünün kökünde (varsa) tanımlandıktan sonra çalışacaktır.

Örnek bir *_Viewstart. cshtml* dosyası:

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Yukarıdaki dosya tüm görünümlerin *_Layout. cshtml* mizanpajını kullanacağı belirtir.

*_Viewstart. cshtml* ve *_Viewwimports. cshtml* genellikle */Pages/Shared* (veya */views/Shared*) klasörüne **yerleştirilmez.** Bu dosyaların uygulama düzeyi sürümleri doğrudan */Pages* (veya */views*) klasörüne yerleştirilmelidir.
