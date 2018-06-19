---
title: ASP.NET Core düzeni
author: ardalis
description: Ortak düzenler kullanma, yönergeleri paylaşma ve işleme görünümleri önce ortak kodun bir ASP.NET Core uygulamada çalıştırmak öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 8e89c8e6cf18c47abb6bf432cdc6bb6b97e8aeb0
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
ms.locfileid: "29904757"
---
# <a name="layout-in-aspnet-core"></a>ASP.NET Core düzeni

Tarafından [Steve Smith](https://ardalis.com/)

Görünümleri sık visual ve programlama öğeleri paylaşır. Bu makalede, ortak düzenler kullanma, yönergeleri paylaşma ve ortak kodun işleme görünümleri önce ASP.NET uygulamanızı çalıştırın öğreneceksiniz.

## <a name="what-is-a-layout"></a>Bir düzen nedir

Çoğu web uygulamaları sayfadan sayfaya gidin gibi kullanıcı ile tutarlı bir deneyim sağlayan ortak bir düzen sahip. Düzen genellikle Uygulama Başlığı, gezinti veya menü öğeleri ve altbilgi gibi ortak kullanıcı arabirimi öğeleri içerir.

![Sayfa düzeni örneği](layout/_static/page-layout.png)

Komut dosyalarını ve stil sayfalarını gibi ortak HTML yapıları da sıklıkla bir uygulama içinde birçok sayfalar tarafından kullanılır. Tüm bu paylaşılan öğeleri de tanımlanabilir bir *düzeni* sonra uygulama içinde kullanılan herhangi bir görünüm tarafından başvurulabilir dosya. Düzenleri azaltmaya yardımcı görünümlerinde yinelenen kod izleyin [yok yineleyin kendiniz (KURU) İlkesi](http://deviq.com/don-t-repeat-yourself/).

Kurala göre bir ASP.NET uygulaması için varsayılan düzeni adlı `_Layout.cshtml`. Visual Studio ASP.NET Core MVC proje şablonu bu düzen dosyasını içeren `Views/Shared` klasörü:

![Çözüm Gezgini görünümler klasöründe](layout/_static/web-project-views.png)

Bu düzen görünümleri için bir en üst düzey şablon uygulamada tanımlar. Uygulamaları bir düzen gerekmez ve uygulamaları farklı düzenler belirtme farklı görünümleri ile birden fazla yerleşim tanımlayabilirsiniz.

Örnek `_Layout.cshtml`:

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Bir düzen belirtme

Razor görünümleri olan bir `Layout` özelliği. Tek bir görünüm, bu özelliği ayarlanarak bir düzeni belirtin:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Belirtilen Düzen tam bir yol kullanabilirsiniz (örneğin: `/Views/Shared/_Layout.cshtml`) ya da kısmi bir ad (örnek: `_Layout`). Kısmi bir ad sağlandığında, Razor görüntüleme altyapısı kendi standart bulma işlemini kullanarak Düzen dosyayı arar. Denetleyici ilişkili klasörü, ilk olarak, arkasından aranır `Shared` klasör. Bu bulma işlemi keşfetmek için kullanılan bir benzerdir [kısmi görünümler](partial.md).

Varsayılan olarak, her düzeni çağırmalısınız `RenderBody`. Yerde çağrısı `RenderBody` olan yerleştirildiğinde, görünüm içeriğini işlenir.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Bölümler

Bir düzen isteğe bağlı olarak bir veya daha fazla başvurabilir *bölümleri*, çağırarak `RenderSection`. Bölümler belirli sayfa öğelerini nereye yerleştirileceğini düzenlemek için bir yol sağlar. Her çağrı `RenderSection` bu bölümün gerekli veya isteğe bağlı olup olmadığını belirtebilirsiniz. Gerekli bölüm bulunamazsa, bir özel durum. Tek bir görünüm içinde bölüm kullanılarak oluşturulması için içeriği belirtin `@section` Razor sözdizimi. Bir görünüm bir bölüm tanımlıyorsa oluşturulması gerekir (veya bir hata meydana gelir).

Örnek `@section` bir görünüm tanımında:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

Doğrulama betikleri eklenir Yukarıdaki kod `scripts` bir form içeren bir görünümde bölümü. Aynı uygulama diğer görünümlere ek betik gerektirmeyebilecek ve bu nedenle komut dosyaları bölüm tanımlamak gerekiyor olmayacaktır.

Bir görünümde tanımlandığı bölümleri, kendi hemen düzen sayfası yalnızca kullanılabilir. Bunlar, kısmi, görünüm bileşenleri ya da Görünüm sisteminin diğer bölümleriyle başvurulamaz.

### <a name="ignoring-sections"></a>Bölümler yoksayılıyor

Varsayılan olarak, gövde ve içerik sayfasındaki tüm bölümleri tüm düzen sayfası tarafından oluşturulması gerekir. Razor görüntüleme altyapısı bu gövde ve her bölüm çizilir olup olmadığını izleyerek zorlar.

Gövde veya bölüm yok saymak için Görünüm altyapısı istemek üzere çağrı `IgnoreBody` ve `IgnoreSection` yöntemleri.

Gövde ve her bir Razor sayfasını bölümünde çizilir göz ardı ya da gerekir.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Paylaşılan yönergeleri alma

Ad alanlarını alma veya gerçekleştirme gibi pek çok şeyi yapmak için Razor yönergesi görünümleri kullanabilir [bağımlılık ekleme](dependency-injection.md). Yönergeleri birçok görünümler tarafından paylaşılan ortak bir belirtilen `_ViewImports.cshtml` dosya. `_ViewImports` Dosyasını aşağıdaki yönergeleri destekler:

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

Dosya işlevleri ve bölüm tanımlarını gibi diğer Razor özellikleri desteklemiyor.

Bir örnek `_ViewImports.cshtml` dosyası:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

`_ViewImports.cshtml` ASP.NET Core MVC uygulama genellikle yerleştirilir için dosya `Views` klasör. A `_ViewImports.cshtml` , onu yalnızca uygulanacak durum görünümleri bu klasörde ve alt klasörlerinde içinde herhangi bir klasör içinde dosya yerleştirilebilir. `_ViewImports` dosyaları kök düzeyinde başlangıç işlenir, ve ardından kadar önde gelen her klasör için konumun görünümünün kendisinde ayarları kök düzeyinde belirtilen şekilde geçersiz klasör düzeyinde.

Örneğin, bir kök düzeyinde varsa `_ViewImports.cshtml` dosyayı belirtir `@model` ve `@addTagHelper`ve başka bir `_ViewImports.cshtml` görünüm denetleyicisini ilişkili klasöründeki dosyasını farklı bir belirtir `@model` ve başka ekler `@addTagHelper`, görünümü Her iki etiket Yardımcıları erişebilir ve ikincisi kullanacağı `@model`.

Birden çok `_ViewImports.cshtml` dosyaları için bir görünüm çalıştırmak, bulunan yönergeleri davranışını birleştirilmiş `ViewImports.cshtml` dosyaları şu şekilde olacaktır:

* `@addTagHelper`, `@removeTagHelper`: sırayla tüm çalışma

* `@tagHelperPrefix`: en yakın bir görünüme herhangi diğer geçersiz kılar

* `@model`: en yakın bir görünüme herhangi diğer geçersiz kılar

* `@inherits`: en yakın bir görünüme herhangi diğer geçersiz kılar

* `@using`: tüm dahil; Yinelenen değer yok sayılır

* `@inject`: her bir özellik için en yakın bir görünüm için aynı adla başkalarıyla geçersiz kılar

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Her görünüm önce kodu çalıştırma

Sahip code her görünüm önce çalıştırmanız gerekir, bu yerleştirilmelidir `_ViewStart.cshtml` dosya. Kural tarafından `_ViewStart.cshtml` dosyasının bulunduğu `Views` klasör. Listelenen deyimleri `_ViewStart.cshtml` önce her tam görünüm (değil düzenleri ve değil kısmi görünümler) çalıştırın. Gibi [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` hiyerarşik olduğu. Varsa bir `_ViewStart.cshtml` dosya denetleyici ilişkili görünüm klasöründe tanımlanmış, kök dizininde tanımlanan bir sonra çalıştırılacak `Views` klasörü (varsa).

Bir örnek `_ViewStart.cshtml` dosyası:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Yukarıdaki dosyanın tüm görünümleri kullanacağını belirtir `_Layout.cshtml` düzeni.

> [!NOTE]
> Ne `_ViewStart.cshtml` ya da `_ViewImports.cshtml` genelde yerleştirilir `/Views/Shared` klasör. Bu dosyaların uygulama düzeyinde sürümleri doğrudan yerleştirilmelidir `/Views` klasör.
