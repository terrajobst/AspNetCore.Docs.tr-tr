---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: "Ana sayfaları ve kısmi kullanarak kullanıcı Arabirimi yeniden kullanma | Microsoft Docs"
author: microsoft
description: "Adım 7 'KURU ilkesini' uygulayabilmeniz için yöntemler kısmi görünüm şablonları ve ana sayfalar kullanarak kod yinelemesinden ortadan kaldırmak için Görünüm şablonlarımız içinde arar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: c42cd6aca40b08a9f8461532fbfd0589901b64ad
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Ana sayfaları ve kısmi kullanarak kullanıcı Arabirimi yeniden kullanma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Bu adım 7 bir ücretsiz olan ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> Adım 7 biz "KURU ilkesini" uygulayabilirsiniz yollardan kısmi görünüm şablonları ve ana sayfalar kullanarak kod yinelemesinden ortadan kaldırmak için Görünüm şablonlarımız içinde arar.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner adım 7: Kısmi ve ana sayfalar

ASP.NET MVC kapsayan tasarım felsefeleri (genellikle "KURU" adlandırılır) "Yapmak değil yineleyin kendiniz" ilkesini biridir. KURU tasarım kod ve sonuçta uygulamaları oluşturmak için hızlı ve sürdürmek daha kolay hale getirir mantığı yinelenmesini ortadan kaldırmanıza yardımcı olur.

Bizim NerdDinner senaryoları çeşitli uygulanan KURU ilkesini zaten gördük. Bazı örnekler: doğrulama mantığımızı arasında her iki düzenleme zorlanacak ve bizim denetleyicisi; senaryoları oluşturmanıza sağlar bizim modeli katman içinde uygulanır "Bulunamadı" görünümü şablon düzenleme, Ayrıntılar ve silme eylem yöntemleri arasında yeniden kullanıyoruz; Biz View() yardımcı yöntemini çağırdığınızda adı açıkça belirtme ihtiyacını ortadan kaldırır, görünümü şablonlarıyla bir kuralı - adlandırma deseni kullanıyoruz; ve DinnerFormViewModel sınıfı hem düzenleme için yeniden kullanıyorsanız ve eylem senaryoları oluşturun.

Şimdi biz "KURU ilkesini" uygulayabilirsiniz yollardan kod yinelemesinden da ortadan kaldırmak için Görünüm şablonlarımız içinde bakalım.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Bizim düzenleme yeniden ziyaret edip görünüm şablonları oluşturma

Şu anda Yemeği formumuzun UI görüntülemek için iki farklı görünüm şablonları – "Edit.aspx" ve "Create.aspx" – kullanıyoruz. Bunları hızlı visual karşılaştırması oldukları nasıl benzer vurgular. Create formun nasıl göründüğünü aşağıdadır:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Ve burada "Düzenle" formumuzun benzer:

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Büyük bir fark var mı? Başlık ve üstbilgi metni dışında form düzenini ve giriş denetimlerini aynıdır.

Biz "Edit.aspx" ve "Create.aspx" Yedekleme biz bulacaksınız görünüm şablonları açarsanız aynı form düzenini ve giriş denetim kodu içerir. Bu çoğaltma, iki kez biz tanıtmak veya iyi olmayan bir yeni Yemeği özelliği - değiştirmek zaman değişiklik yapmak zorunda bitiş anlamına gelir.

### <a name="using-partial-view-templates"></a>Kısmi görünüm şablonları kullanma

ASP.NET MVC görünümü işleme mantığı için bir sayfasının alt kısmında kapsüllemek için kullanılan "kısmi görünümü" şablonlarını tanımlama yeteneği destekler. "Kısmi" görünümü işleme mantığı kez tanımlamak için kullanışlı bir yöntem sunar ve sonra birden fazla yerde bir uygulama arasında yeniden kullanabilirsiniz.

"KURU yukarı" bizim Edit.aspx ve Create.aspx görünüm şablonu çoğaltma yardımcı olmak için "form düzenini ve ortak giriş öğelerini Kapsüller DinnerForm.ascx" adlı bir kısmi görünüm şablonu oluşturabilirsiniz. Bizim/görünümler/azalma dizinde sağ tıklayıp seçerek bunu "Ekle -&gt;görünümü" menü komutu:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Bu, "Görünüm Ekle" iletişim kutusu görüntülenir. Biz, biz "DinnerForm" oluşturmak istediğiniz iletişim içinde "kısmi Görünüm Oluştur" onay kutusunu seçin ve biz bunu DinnerFormViewModel sınıfı geçer belirtmek yeni görünüm adı:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Biz "Ekle" düğmesini tıklatın, Visual Studio yeni bir "DinnerForm.ascx" Görünüm şablonu bize "\Views\Dinners" dizini içinde oluşturur.

Biz sonra kopyalayıp yinelenen form düzenini yapıştırın / bizim yeni "DinnerForm.ascx" kısmi görünüm şablonuna bizim Edit.aspx/ Create.aspx görünüm şablonlardan denetim kodu girin:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Biz DinnerForm kısmi şablon çağırın ve form çoğaltma ortadan kaldırmak için düzenleme ve oluşturma görünümü şablonlarımız sonra güncelleştirebilirsiniz. Arama Html.RenderPartial("DinnerForm") tarafından görünüm şablonlarımız içinde bunu yapabilirsiniz:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Açıkça Html.RenderPartial çağrılırken istediğiniz kısmi şablonun yolunu uygun (örneğin: ~ Views/Dinners/DinnerForm.ascx "). Bizim kodda yine de size ASP.NET MVC içindeki kurala dayalı adlandırma deseni yararlanarak ve yalnızca "DinnerForm" işlenecek kısmi adını belirtme. Biz bunu yaparken ASP.NET MVC (için DinnersController bu/görünümler/azalma olur) kurala dayalı görünümleri dizinde ilk arar. Kısmi şablonu bulamazsa vardır, ardından için /Views/Shared dizinini arar.

Html.RenderPartial() yalnızca kısmi görünüm adıyla çağrıldığında, ASP.NET MVC kısmi görünüme arama görünümü şablonu tarafından kullanılan aynı modeli ve ViewData sözlük nesneleri geçirin. Alternatif olarak, bir alternatif Model nesnesi ve/veya kısmi görünümü kullanmak için ViewData sözlüğü geçirmenize olanak aşırı yüklenmiş Html.RenderPartial() sürümü vardır. Bu yalnızca bir alt kümesini tam modeli/ViewModel geçirmek istediğiniz senaryolar için kullanışlıdır.

| **Yan konu: Neden &lt;%%&gt; yerine &lt;% = %&gt;?** |
| --- |
| Fark yukarıdaki kodu ile birlikte Zarif şeyler biri biz kullanarak bir &lt;%%&gt; yerine engellemek bir &lt;% = %&gt; Html.RenderPartial() çağrılırken engelleyin. &lt;% = %&gt; ASP.NET bloklarında gösteren bir geliştirici belirtilen değere işlemek istediği (örneğin: &lt;% = "Hello" %&gt; "Hello" hale getiren). &lt;%%&gt; blokları yerine belirtmek Geliştirici kod yürütmek istiyor ve herhangi bir çıktı bunların içindeki işlenen açıkça yapılmalıdır (örneğin: &lt;Response.Write("Hello") %&gt;. Biz kullandığınız neden bir &lt;%%&gt; yukarıdaki bizim Html.RenderPartial kod bloğu olduğundan Html.RenderPartial() yöntemi bir dize döndürmez ve bunun yerine içeriği doğrudan çağıran şablonu görüntüleme animasyonun çıktı akışı çıkarır. Bunu performansı verimliliğini artırmak için ve (büyük olasılıkla çok büyük) geçici dize nesnesi oluşturma ihtiyacını ortadan kaldırır Böyle yaparak yapar. Bu bellek kullanımını azaltır ve genel uygulama verimliliği artırır. Html.RenderPartial() kullanarak içinde olduğunda noktalı çağrı sonuna ekle unuttunuz zaman bir ortak hata bir &lt;%%&gt; bloğu. Örneğin, bu kodun bir derleyici hatası neden olur: &lt;Html.RenderPartial("DinnerForm") %&gt; yerine yazmanız gerekir: &lt;% Html.RenderPartial("DinnerForm"); %&gt; çünkü &lt;%%&gt; taşlarıdır müstakil kod deyimleri ve noktalı virgülle sonlandırılması gerekir deyimleri C# kod kullanırken. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Kod açıklamak için kısmi görünüm şablonları kullanma

Görünüm işleme mantığı birden çok yerde çoğaltılmasını önlemek için "DinnerForm" kısmi görünüm şablonu oluşturduk. Kısmi görünüm şablonları oluşturmak için en yaygın nedeni budur.

Bazen hala bile bunlar yalnızca tek bir yerde çağrıldığında, kısmi görünümleri oluşturmak için mantıklıdır. Çok karmaşık görünüm şablonları genellikle zaman kendi Görünüm işleme mantığı ayıklanan ve birine bölümlenmiş ya da daha iyi kısmi şablonları adlı okumak çok daha kolay hale gelebilir.

Örneğin, göz önünde bulundurun (hangi biz en kısa süre içinde arayacaktır) Projemizin Site.master dosyasından kod parçacığı aşağıda. Görece düz İleri kodudur kısmen oturum açma/oturum kapatma görüntülemek için mantığı bağlamak için en üstünde – okumak için ekranın sağ "LogOnUserControl" kısmi içinde kapsüllenir:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Kendiniz kafası bulduğunuz her çalışılırken bir görünüm şablonu içindeki html/kod işaretleme anlamak, oluştuysa, bunlardan bazıları ayıklanan ve iyi adlandırılmış kısmi görünümlere bulunanad daha anlaşılır olması olmayacaktır olup olmadığını göz önünde bulundurun.

### <a name="master-pages"></a>Ana sayfalar

Kısmi görünümler'ı desteklemenin yanında ASP.NET MVC ortak yerleşim ve bir sitenin üst düzey html tanımlamak için kullanılan "ana sayfa" şablonları oluşturma olanağı da destekler. İçerik denetimleri sonra geçersiz ya da "görünümler tarafından doldurulmuş" değiştirebilen bölgeleri tanımlamak için ana sayfa eklenebilir yer tutucusu. Bu bir uygulama arasında ortak bir düzen uygulamak için çok etkili (ve KURU) bir yol sağlar.

Varsayılan olarak, yeni ASP.NET MVC projeleri otomatik olarak kendisine eklenmiş bir ana sayfa şablonu vardır. Bu ana sayfa "Site.master" ve \Views\Shared\ klasördeki yaşamlarını adlandırılır:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Varsayılan Site.master dosyanın aşağıdaki gibi görünüyor. Üst gezinti menüsü ile birlikte sitenin dış html tanımlar. – Bir başlık ve birincil içerik sayfasının burada değiştirilmesi gereken diğer iki değiştirilebilir içerik yer tutucusu denetimleri aşağıdakileri içerir:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Tüm NerdDinner uygulamamız için ("Listesinde", "Ayrıntılar", "Düzenle", "Oluşturma", "Bulunamadı", vb.) oluşturduk görünüm şablonları bu Site.master şablona dayalı. Bu varsayılan olarak en çok eklenen "MasterPageFile" özniteliği aracılığıyla belirtilir &lt;% @ sayfa %&gt; biz "Görünümü Ekle" iletişim kutusunu kullanarak bizim görünümler oluşturduğunuzda yönergesi:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Ne bu biz Site.master içeriği değiştirebilirsiniz, ve sahip değişiklikleri otomatik olarak uygulanan ve biz bizim görünüm şablonlardan işleme sırasında kullanılan anlamına gelir.

Şimdi uygulamamız üstbilgisinin "MVC Uygulamam" yerine "NerdDinner" olmayacak şekilde bizim Site.master's üstbilgi bölümü güncelleştirin. Şimdi; böylece ilk sekme "Bir (HomeController'ın İNDİS() eylem yöntemi tarafından işlenen) Yemeği Bul" olduğundan ve "Ana bilgisayar bir (DinnersController'ın Create() eylem yöntemi tarafından işlenen) Yemeği" adlı yeni bir sekme ekleyelim ayrıca bizim Gezinti Menüsü güncelleştirin:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Biz Site.master dosyasını ve yenileme kaydettiğinizde bizim üstbilgi göreceğiz bizim tarayıcı Göster uygulamamız içindeki tüm görünümler arasında değişir. Örneğin:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

İle */Dinners/düzenleme / [kimlik]* URL'si:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Sonraki adım

Kısmi ve ana sayfa görünümleri düzgün bir şekilde düzenlemenizi sağlayan çok esnek seçenekler sağlar. Bunlar görünümü çoğaltma önlemenize yardımcı olması içerik / kod ve görünüm şablonlarınızı okuyun ve sürdürmek daha kolay hale bulacaksınız.

Şimdi şimdi daha önce oluşturduğumuz listeleme senaryo yeniden ziyaret ve ölçeklenebilir sayfalama desteğini etkinleştirin.

>[!div class="step-by-step"]
[Önceki](use-viewdata-and-implement-viewmodel-classes.md)
[sonraki](implement-efficient-data-paging.md)
