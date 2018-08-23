---
title: ASP.NET core'da düzeni
author: ardalis
description: Yaygın düzenlerini kullanmayı, yönergeleri paylaşın ve işleme görünümleri önce ortak kod içinde ASP.NET Core uygulaması çalıştırma hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: ad0b339572f387be8a636204015ffc361947acb8
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754046"
---
# <a name="layout-in-aspnet-core"></a>ASP.NET core'da düzeni

Tarafından [Steve Smith](https://ardalis.com/)

Görünümler, görsel ve programlama öğeleri sık paylaşın. Bu makalede, yaygın düzenlerini kullanmayı, yönergeleri paylaşın ve ASP.NET Core uygulamanızı oluşturma görünümleri önce ortak kod çalıştırmak öğreneceksiniz.

## <a name="what-is-a-layout"></a>Bir düzen nedir

Çoğu web uygulaması, sayfalar arasında gezinirken, kullanıcı ile tutarlı bir deneyim sağlayan ortak bir düzeni vardır. Düzen, genellikle Uygulama Başlığı, gezinti veya menü öğeleri ve alt bilgi gibi ortak kullanıcı arabirimi öğeleri içerir.

![Sayfa düzeni örneği](layout/_static/page-layout.png)

Betikleri ve stil sayfalarını gibi ortak HTML yapıları, bir uygulama içinde birçok sayfaları da sık sık kullanılır. Tüm bu paylaşılan öğeleri içinde tanımlanabilir bir *Düzen* dosya, uygulama içinde kullanılan herhangi bir görünüm tarafından başvurulabilir. Düzenleri azaltmaya yardımcı görünümlerde yinelenen kod izleyin [yoksa yineleyin kendiniz (KURU) İlkesi](http://deviq.com/don-t-repeat-yourself/).

Kural gereği, ASP.NET Core uygulaması için varsayılan düzen adlı `_Layout.cshtml`. Visual Studio ASP.NET Core MVC proje şablonu Düzen bu dosyada içerir `Views/Shared` klasörü:

![Çözüm Gezgini klasöründe görünümleri](layout/_static/web-project-views.png)

Bu düzen, bir üst düzey şablonu görünümler için uygulamada tanımlar. Uygulamaları bir düzen gerektirmeyen ve uygulamaları alan farklı düzenler belirterek farklı görünümleri ile birden fazla Düzen tanımlayabilirsiniz.

Bir örnek `_Layout.cshtml`:

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Bir düzen belirtme

Razor görünümleri olan bir `Layout` özelliği. Tek bir görünüm bu özelliğini ayarlayarak bir düzen belirtin:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Belirtilen düzen bir tam yol kullanabilirsiniz (örnek: `/Views/Shared/_Layout.cshtml`) ya da kısmi bir ad (örnek: `_Layout`). Kısmi bir adı sağlandığında, Razor görünüm altyapısını kullanarak kendi standart bulma işlemi için yerleşim dosyası arar. Denetleyici ilişkili klasörü, ilk olarak, arkasından aranır `Shared` klasör. Bu bulma işlemi için keşfetmek için kullanılan bir aynıdır [kısmi görünümler](partial.md).

Varsayılan olarak, her Düzen çağırmalıdır `RenderBody`. Her yerde çağrısı `RenderBody` olan konumdaki görünüm içeriğinin işlenir.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Bölümler

Bir düzen, isteğe bağlı olarak bir veya daha fazla başvurabilirsiniz *bölümleri*, çağırarak `RenderSection`. Bölümler, belirli sayfa öğeleri nereye yerleştirileceğini düzenlemek için bir yol sağlar. Her çağrı `RenderSection` bu bölümün gerekli veya isteğe bağlı olup olmadığını belirtebilirsiniz. Gerekli bölüm bulunamazsa, bir özel durum oluşturulur. Tek bir görünüm içinde bir bölümde kullanılarak oluşturulması için içeriği belirtin `@section` Razor söz dizimi. Görünüm bir bölüm tanımlar, işlenen gerekir (veya bir hata meydana gelir).

Bir örnek `@section` görünüm tanımında:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

Yukarıdaki kod doğrulama betikleri için eklenen `scripts` bölümü bir form içeren bir görünümü. Aynı uygulamayı diğer görünümlerde herhangi ek komut dosyası gerekli değil ve bu nedenle betikleri bölümü tanımlaması gerekmez.

Bir görünümde tanımlı bölüm, yalnızca kendi anlık düzen sayfası içinde kullanılabilir. Bunlar, kısmi görünüm bileşenleri veya Görünüm sistemin diğer bölümlerini başvurulamaz.

### <a name="ignoring-sections"></a>Bölümler yoksayılıyor

Varsayılan olarak, gövdesini ve içerik sayfasındaki tüm bölümlerin tümünü düzen sayfası tarafından oluşturulması gerekir. Razor görüntüleme motorunu gövdesi ve her bölümde oluşturulmasını isteyip izleyerek zorlar.

Gövde veya bölüm yok saymak için Görünüm altyapısı açmasını sağlamak için çağrı `IgnoreBody` ve `IgnoreSection` yöntemleri.

Gövde ve her bölümde bir Razor sayfası işlenen yoksayıldı veya gerekir.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Paylaşılan yönergeleri alma

Görünümler, ad alanlarını alma veya gerçekleştirme gibi pek çok şeyi yapmak için Razor yönergeleri kullanabilir [bağımlılık ekleme](dependency-injection.md). Yönergeleri çoğu görünümler tarafından paylaşılan ortak belirtilen `_ViewImports.cshtml` dosya. `_ViewImports` Dosyasını aşağıdaki yönergeleri destekler:

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

`_ViewImports.cshtml` Bir ASP.NET Core MVC uygulaması genellikle yerleştirilir için dosya `Views` klasör. A `_ViewImports.cshtml` içinde çalışması, yalnızca uygulanacak görünümleri, klasör ve alt klasörleri içinde herhangi bir klasör içinde dosya yerleştirilebilir. `_ViewImports` dosyaları kök düzeyinde başlangıç işlenir ve ardından kadar önde gelen her bir klasör için görünümünün kendisinde konumu ayarları kök düzeyinde belirtilen şekilde geçersiz klasör düzeyinde.

Örneğin, bir kök düzeyinde `_ViewImports.cshtml` dosyasını belirtir `@model` ve `@addTagHelper`ve başka bir `_ViewImports.cshtml` farklı bir görünüm denetleyicisi ilişkili bir klasörde dosya belirtir `@model` ve başka ekler `@addTagHelper`, görünümü Her iki etiket Yardımcıları erişebilir ve ikincisi kullanacağı `@model`.

Birden çok `_ViewImports.cshtml` dosyaları için bir görünüm çalıştırın, birlikte bulunan yönergeleri davranışını `ViewImports.cshtml` dosyaları şu şekilde olacaktır:

* `@addTagHelper`, `@removeTagHelper`: sırayla tüm çalışma

* `@tagHelperPrefix`: en yakındakine görünümüne başka geçersiz kılar.

* `@model`: en yakındakine görünümüne başka geçersiz kılar.

* `@inherits`: en yakındakine görünümüne başka geçersiz kılar.

* `@using`: tüm; dahildir yinelenenler yoksayıldı

* `@inject`: her bir özellik için en yakın bir görünüm için aynı adla başkalarıyla geçersiz kılar

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Her görünüm önce kod çalıştırma

Sahip kod önce her bir görünüm çalıştırmanız gerekir, bu yerleştirilmelidir `_ViewStart.cshtml` dosya. Kural olarak, `_ViewStart.cshtml` dosyası `Views` klasör. Listelenen deyimleri `_ViewStart.cshtml` önce her tam görünüm (değil düzenleri ve kısmi görünümler) çalıştırın. Gibi [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` hiyerarşik olduğu anlamına gelir. Varsa bir `_ViewStart.cshtml` dosya görünümü denetleyicisi ilişkili klasöründe tanımlanır, kök dizininde tanımlananla sonra çalıştırılacak `Views` klasörü (varsa).

Bir örnek `_ViewStart.cshtml` dosyası:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Yukarıdaki dosyanın tüm görünümlere kullanacağını belirtir `_Layout.cshtml` düzeni.

> [!NOTE]
> Ne `_ViewStart.cshtml` ya da `_ViewImports.cshtml` genelde yerleştirilir `/Views/Shared` klasör. Uygulama düzeyinde bu dosyaların sürümleri doğrudan yerleştirilmelidir `/Views` klasör.
