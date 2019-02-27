---
title: ASP.NET core'da alanları
author: rick-anderson
description: Alanlar (yönlendirme için) ayrı bir ad ve klasör yapısını (için görünümler) bir gruba ilgili işlevleri düzenlemek için kullanılan bir ASP.NET MVC özelliği nasıl olduğunu öğrenin.
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833533"
---
# <a name="areas-in-aspnet-core"></a>ASP.NET core'da alanları

Tarafından [Dhananjay Kumar](https://twitter.com/debug_mode) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Alanlar (yönlendirme için) ayrı bir ad ve klasör yapısını (için görünümler) bir gruba ilgili işlevleri düzenlemek için kullanılan, ASP.NET bir özelliğidir. Alanlara kullanarak başka bir rota parametresini ekleyerek yönlendirme amacıyla hiyerarşi oluşturur `area`, `controller` ve `action` veya bir Razor sayfası `page`.

Alanları bir ASP.NET Core Web uygulaması daha küçük işlevsel gruplar halinde bölümlere ayırmak için bir yol her biri kendi Razor sayfaları, denetleyicileri, görünümler ve modeller kümesi sağlar. Bir alan etkin bir uygulama içinde bir yapıdır. Bir ASP.NET Core web projesinde farklı klasörlerde bulunan sayfaları, Model, denetleyici ve görünüm gibi mantıksal bileşenler tutulur. ASP.NET Core çalışma zamanı, bu bileşenler arasındaki ilişki oluşturmak için adlandırma kuralları kullanır. Büyük bir uygulama için ayrı yüksek düzey alanlarına işlev uygulamasını bölümleme için yararlı olabilir. Örneğin, bir e-ticaret uygulamayla kullanıma alma, fatura ve arama gibi birden çok iş birimleri. Bu birimleri her görünümleri, denetleyicileri, Razor sayfaları ve modelleri içerecek şekilde kendi alanı vardır.

Alanları projesinde kullanmayı olduğunda:

* Uygulama, mantıksal olarak ayrılabilen birden çok üst düzey işlevsel bileşenden.
* Böylece her işlevsel alan üzerinde bağımsız olarak çalışılabilecek uygulamasını bölümleme istiyorsunuz.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)). İndirme örnek alanları test etmek için temel bir uygulama sağlar.

## <a name="areas-for-controllers-with-views"></a>Görünüm denetleyicileri için alanları

Alanları denetleyicileri ve görünümleri tipik bir ASP.NET Core web uygulaması şunları içerir:

* Bir [alan klasör yapısını](#area-folder-structure).
* Denetleyicileri düzenlenmiş ile [ &lbrack;alan&rbrack; ](#attribute) denetleyici alanı ile ilişkilendirilecek özniteliği: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]
* [Başlangıç olarak eklenen alan yolu](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

## <a name="area-folder-structure"></a>Alan klasör yapısı
İki mantıksal gruplar olan bir uygulama düşünün *ürünleri* ve *Hizmetleri*. Alanlara kullanarak klasör yapısı şuna benzer olacaktır:

* Proje adı
  * Alanlar
    * Ürünler
      * Denetleyiciler
        * HomeController.cs
        * ManageController.cs
      * Görünümler
        * Ana Sayfası
          * Index.cshtml
        * yönetme
          * Index.cshtml
          * About.cshtml
    * Hizmetler
      * Denetleyiciler
        * HomeController.cs
      * Görünümler
        * Ana Sayfası
          * Index.cshtml

Önceki Düzen alanları kullanılırken tipik olsa da, yalnızca görünüm dosyaları bu klasör yapısı kullanmak için gereklidir. Görünüm bulma eşleşen bir alan görünüm dosyası şu sırayla arar:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

Klasörleri görüntüle konumunu ister *denetleyicileri* ve *modelleri* mu **değil** önemi. Örneğin, *denetleyicileri* ve *modelleri* klasörü gerekli değildir. İçeriği *denetleyicileri* ve *modelleri* bir .dll derlenmiş kodudur. İçeriği *görünümleri* , görünüm için bir istek yayınlanana kadar derlenmiş değil.

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a>Denetleyici ile bir alanı ilişkilendirin

Alanı denetleyicileri ile atanır [ &lbrack;alan&rbrack; ](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) özniteliği:

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a>Alan yolu ekleme

Alan yolları, genellikle geleneksel özniteliği yerine yönlendirmesi kullanır. Geleneksel yönlendirme sipariş bağlıdır. Genel olarak, bunlar olmadan bir alan yolları daha ayrıntılı olarak alanları rotalarla önceki rota tablosunda yerleştirilmelidir.

`{area:...}` URL alanı tüm alanlar arasında Tekdüzen ise, rota şablonlarındaki bir belirteci olarak kullanılabilir:

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

Önceki kodda, `exists` yol alanı eşleşmelidir kısıtlama uygular. Kullanarak `{area:...}` alanlarına yönlendirme eklemeye az karmaşık mekanizmadır.

Aşağıdaki kod <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> alan yolları adlı iki oluşturmak için:

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

Kullanırken `MapAreaRoute` bkz: ASP.NET Core 2.2 ile [bu GitHub sorunu](https://github.com/aspnet/AspNetCore/issues/7772).

Daha fazla bilgi için [alan yönlendirme](xref:mvc/controllers/routing#areas).

### <a name="link-generation-with-areas"></a>Alanları ile bağlantı oluşturma

Aşağıdaki kodu [örnek indirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) gösterir, belirtilen alanla nesil bağlantı:

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

Yukarıdaki kod ile oluşturulan herhangi bir uygulamada geçerli bağlantılardır.

Örnek indirme içeren bir [kısmi Görünüm](xref:mvc/views/partial) içeren önceki bağlantıları ve aynı bağlantı alanı belirtmeden. Kısmi görünüm başvurulduğundan [Düzen dosyası](), uygulamayı her sayfa oluşturulan bağlantıları görüntüler. Alanı belirtmeden oluşturulan bağlantıları, yalnızca bir sayfa aynı alan ve denetleyici başvurulduğunda geçerlidir.

Alan veya denetleyici belirtilmediğinde yönlendirme bağlıdır *ortam* değerleri. Geçerli isteğin geçerli rota değerleri için bağlantı oluşturma ortam değerleri olarak kabul edilir. Örnek uygulama için çoğu durumda, ortam değerleri kullanarak yanlış bağlantılar oluşturur.

Daha fazla bilgi için [denetleyici eylemlerine yönlendirme](xref:mvc/controllers/routing).

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a>Düzen _ViewStart.cshtml dosyasını kullanarak alanları için paylaşılan

Uygulamanın tamamında için yaygın bir düzen paylaşmak için taşıma *_ViewStart.cshtml* uygulama kök klasörüne.

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a>Görünümleri depolandığı varsayılan alanı klasörü Değiştir

Varsayılan alan klasöründen aşağıdaki kod değişiklikleri `"Areas"` için `"MyAreas"`:

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a>Yayımlama alanları

Tüm `*.cshtml` ve `wwwroot/**` dosyaları ne zaman çıkış yayımlanır `<Project Sdk="Microsoft.NET.Sdk.Web">` dahil *.csproj* dosya.
