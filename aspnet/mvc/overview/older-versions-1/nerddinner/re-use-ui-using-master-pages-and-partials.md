---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Ana sayfaları ve kısmi bölümleri kullanarak kullanıcı arabirimini yeniden kullanma | Microsoft Docs
author: microsoft
description: 7. adım, kısmi görünüm şablonları ve ana sayfaları kullanarak kod çoğaltma ortadan kaldırmak için Görünüm şablonlarımızı içinde biz 'KURU ilke' uygulayabilirsiniz yöntem incelenir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: f96d3f3059a442f977d4422797c872b865a55695
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391437"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Ana sayfaları ve kısmi bölümleri kullanarak kullanıcı arabirimini yeniden kullanma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 7 bir ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> 7. adım, kısmi görünüm şablonları ve ana sayfaları kullanarak kod çoğaltma ortadan kaldırmak için Görünüm şablonlarımızı içinde biz "KURU ilkesini" uygulayabilirsiniz yöntem incelenir.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner adım 7: Kısmi ve ana sayfalar

ASP.NET MVC benimsediğini tasarım felsefeleri birini (genellikle "KURU" adlandırılır) "Yapmak değil yineleyin kendiniz" ilkesidir. KURU tasarım, kod ve uygulamaları oluşturmak için daha hızlı ve kolay bir şekilde korumak sonunda getiren mantıksal çoğaltılması ortadan yardımcı olur.

Bizim NerdDinner senaryolardan birkaç uygulanan KURU ilkeye zaten gördük. Birkaç örnek: bizim Doğrulama mantığı üzerinde hem düzenleme zorunlu ve denetleyicimizin; senaryoları oluşturmanıza etkinleştirir bizim modeli katmanı içinde uygulanır "Bulunamadı" Görünüm şablonu düzen, Ayrıntılar ve Sil eylem yöntemleri arasında yeniden kullanıyoruz; Biz View() yardımcı yöntemini çağırdığınızda adı açıkça belirtmek için gereğini ortadan kaldırır bizim görünümü şablonlar sayesinde, bir kuralı - adlandırma deseni kullanıyoruz; ve DinnerFormViewModel sınıfı yeniden her iki düzenleme için kullanarak ve eylem senaryoları oluşturun.

Şimdi biz "KURU ilkesini" uygulayabilirsiniz yöntemlerine kod yinelemesinden de ortadan kaldırmak için Görünüm şablonlarımızı içinde göz atalım.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Bizim düzenleme yeniden ziyaret ve görüntüleme şablonları oluşturma

Akşam Yemeği formumuzu kullanıcı arabirimini görüntülemek için iki farklı görünümü şablonları – "Edit.aspx" ve "Create.aspx" – şu anda kullanıyoruz. Bunları hızlı visual karşılaştırması olduklarından nasıl benzer vurgular. Form oluştur benzer aşağıdadır:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Ve "Düzenle" formumuzu gibi görünür:

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Büyük bir fark var mı? Form düzeni ve giriş denetimleri başlık ve üst bilgi metni dışında aynıdır.

Biz biz bulabilirsiniz şablonları göster "Edit.aspx" ve "Create.aspx" Yedekleme açarsanız aynı form düzeni ve giriş denetim kodu içerirler. Bu çoğaltma, iki kez biz tanıtmak veya iyi değil bir yeni Dinner özelliği - değiştirmek zaman değişiklik yapmak zorunda son anlamına gelir.

### <a name="using-partial-view-templates"></a>Kısmi görünüm şablonları kullanma

ASP.NET MVC Görünüm işleme mantığı bir sayfa alt bir kısmı için kapsüllemek için kullanılabilecek "kısmi Görünüm" şablonlarını tanımlamak için özelliğini destekler. "Kısmi" kez görünümü işleme mantığı tanımlamak için kullanışlı bir yol sağlar ve ardından birden fazla yerde bir uygulamanın tamamında yeniden kullanın.

"KURU-yukarı" bizim Edit.aspx ve Create.aspx görünüm şablonu çoğaltma yardımcı olmak için "form düzeninden ve ortak giriş öğelerini Kapsüller DinnerForm.ascx" adlı bir kısmi görünüm şablonu oluşturabilirsiniz. Bizim/Views/azalma dizinde sağ tıklayıp seçerek bunu "Add -&gt;görünümü" menü komutu:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Bu "Görünüm Ekle" iletişim kutusu görüntüler. Biz, biz "DinnerForm" oluşturmak istediğiniz iletişim kutusunda "kısmi Görünüm Oluştur" onay kutusunu işaretleyin ve biz bunu DinnerFormViewModel sınıfı geçer belirtmek yeni görünüm adı:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Biz "Ekle" düğmesine tıkladığınızda, Visual Studio yeni bir "DinnerForm.ascx" Görünüm şablonu bizim için "\Views\Dinners" dizin içinde oluşturur.

Biz ardından kopyalama/yinelenen form düzenini yapıştırma / sunduğumuz yeni "DinnerForm.ascx" kısmi görünüm şablonuna Edit.aspx/ Create.aspx görünümü şablonlarımızı denetim koddan giriş:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Ardından, biz DinnerForm kısmi şablonu çağırmak ve form çoğaltma ortadan kaldırmak için düzenleme ve oluşturma görünümü şablonlarımızı güncelleştirebilirsiniz. Tarafından çağıran Html.RenderPartial("DinnerForm") görünümü şablonlarımızı içinde bunu yapabilirsiniz:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Açıkça Html.RenderPartial çağrılırken istediğiniz kısmi şablonun yolunu belirtebilme (örneğin: ~ Views/Dinners/DinnerForm.ascx "). Yukarıdaki bizim kodu ancak biz ASP.NET MVC içindeki kural tabanlı adlandırma deseni yararlanarak ve yalnızca "DinnerForm" işlenecek kısmi adını belirtme. Bunu, ASP.NET MVC (için DinnersController bu/Views/azalma olur) kural tabanlı görünümleri dizinde ilk görünecektir. Kısmi şablonu bulamazsa var. Bunun ardından için /Views/Shared dizinde görünür.

Yalnızca kısmi görünümün adı ile Html.RenderPartial() çağrıldığında, ASP.NET MVC kısmi görünüme arama görünümü şablon tarafından kullanılan aynı modeli ve ViewData sözlük nesneleri geçirin. Alternatif olarak, diğer Model nesnesi ve/veya ViewData sözlük kullanmak kısmi görünüm için geçirmenize olanak tanıyan aşırı yüklü sürümler Html.RenderPartial(), vardır. Bu, yalnızca bir alt kümesini tam Model/ViewModel geçirmek istediğiniz senaryolar için kullanışlıdır.

| **Yan konu: Neden &lt;%%&gt; yerine &lt;% = %&gt;?** |
| --- |
| Fark etmiş yukarıdaki kodla Zarif şeylerden biri biz kullanarak bir &lt;%%&gt; bloğu yerine bir &lt;% = %&gt; Html.RenderPartial() çağırırken engelleyin. &lt;% = %&gt; ASP.NET bloklarında belirtmek belirtilen değerini işlemek bir geliştirici istediğini (örneğin: &lt;% = "Hello" %&gt; "Hello" getiren). &lt;%%&gt; bloklar yerine belirtmek Geliştirici kod yürütmek ister ve tüm bunları çıktısı işlenen açıkça yapılmalıdır (örneğin: &lt;Response.Write("Hello") %&gt;. Kullandığımız nedeni bir &lt;%%&gt; Html.RenderPartial kodumuz yukarıdaki ile bloğudur Html.RenderPartial() yöntem bir dize döndürmüyor ve bunun yerine içeriği doğrudan çağıran şablonu görüntüle stream çıkış çıkarır çünkü. Bunu bir geçici (büyük olasılıkla çok büyük) dize nesnesi oluşturmaya gerek önler Böyle yaparak ve performans verimliliği nedeniyle yapar. Bu işlem, bellek kullanımını azaltır ve genel uygulama aktarım hızını artırır. Html.RenderPartial() içinde olduğunda çağrının sonunda noktalı virgül ekleyin kullanırken yaygın bir yanlış bir &lt;%%&gt; blok. Örneğin, bu kod bir derleyici hatasına neden olur: &lt;Html.RenderPartial("DinnerForm") %&gt; yerine yazmanız gereken: &lt;% Html.RenderPartial("DinnerForm"); %&gt; Bunun nedeni, &lt;%%&gt; taşlarıdır kendi içinde kod deyimlerini ve noktalı virgülle sonlandırılması gerekir deyimleri C# kodunu kullanarak. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Kod açıklamak için kısmi görünüm şablonları kullanma

Görünüm işleme mantığı birden çok yerde yinelemekten "DinnerForm" kısmi görünüm şablonu oluşturduk. Kısmi görünüm şablonları oluşturmak için en yaygın nedeni budur.

Bazen hala bile, yalnızca tek bir yerde çağrıldığında, kısmi görünümleri oluşturmak için mantıklıdır. Çok karmaşık görüntüleme şablonları genellikle zaman kendi Görünüm işleme mantığı ayıklanan ve bölümlenmiş bir veya daha iyi şablonlarının kısmi adlı çok daha kolay hale gelebilir.

Örneğin, aşağıdaki kod parçacığını (Bu, size en kısa süre içinde gönderdiğimizde) Projemizin Site.master dosyasından. Kodu görece sorunsuz bir oturum açma/kapatma görüntülenecek mantıksal kısmen bağlantı çünkü en üstünde – okumak için ekranın sağ "LogOnUserControl" kısmi içinde kapsüllenir:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Kendinizi bu yanıltıcı bulduğunuz her çalışılırken bir görünüm şablonu içindeki html/kod biçimlendirme anlamak, bu, bazıları ise ayıklanan ve iyi adlandırılmış kısmi görünümlere yeniden düzenlenen NET olmaz mıydı olup olmadığını göz önünde bulundurun.

### <a name="master-pages"></a>Ana sayfalar

ASP.NET MVC kısmi görünümler hizmetinin yanı sıra, bir sitenin üst düzey html ve yaygın bir düzen tanımlamak için kullanılan "ana sayfa" şablonları oluşturma olanağı da destekler. Yer tutucu denetimler sonra geçersiz kılınan veya "görünümler tarafından doldurulur" değiştirilebilir bölgeleri belirlemek için ana sayfasına eklenebilir içerik. Bu yaygın bir düzen uygulama uygulamak için çok etkili (ve KURU) bir yol sağlar.

Varsayılan olarak, yeni ASP.NET MVC projeleri otomatik olarak kendilerine eklenen bir ana sayfası şablonu vardır. Bu ana sayfa "Site.master" ve hayatını \Views\Shared\ klasördeki adlandırılır:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Varsayılan Site.master dosyanın aşağıdaki gibi görünür. Bu, bir üst gezinti menüsü ile birlikte sitenin dış html tanımlar. – Bir başlık ve diğeri için birincil bir sayfanın içeriğini değiştirildiği desene iki değiştirilebilir içerik yer tutucusu denetimleri aşağıdakileri içerir:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Tüm şablonları göster NerdDinner uygulamamız için ("List", "Details", "Düzenle", "Oluşturma", "Bulunamadı", vb.) oluşturduk bu Site.master şablona dayalı. Bu varsayılan olarak en üstüne eklendi "MasterPageFile" özniteliği aracılığıyla gösterilir &lt;% @ sayfa %&gt; biz "Görünüm Ekle" iletişim kutusunu kullanarak bizim görünümleri oluştururken yönergesi:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Ne bu Site.master içeriği Değiştirebiliriz ve sahip değişiklikleri otomatik olarak olması uygulanan ve biz görünümü şablonlarımızdan birini işlendiğinde kullanılan anlamına gelir.

Böylece uygulamamızın başlığını "MVC Uygulamam" yerine "NerdDinner" bizim Site.master ait üst bilgisi bölümü güncelleştirelim. Şimdi ilk sekme olan "A (HomeController'ın İNDİS() eylem yöntemi tarafından işlenen) Dinner Bul" ve "Konak a (DinnersController'ın Create() eylem yöntemi tarafından işlenen) Dinner" adlı yeni bir sekme ekleyelim de bizim Gezinti Menüsü güncelleştirin:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Biz yenileme ve Site.master dosyayı kaydettiğinizde bizim üstbilgi görüyoruz bizim tarayıcı Göster uygulamamız içindeki tüm görünümler arasında değişiklikleri. Örneğin:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

İle */Dinners/düzenleme / [ID]* URL'si:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Sonraki adım

Kısmi ve ana sayfalar düzgün bir şekilde görünümleri düzenleyin olanak tanıyan çok esnek seçenekler sağlar. Bunlar görünümü dll'sindedir Yardım içerik / kod ve görünüm şablonlarınızı okunması ve düzenlenmesi daha kolay hale getirmek bulabilirsiniz.

Şimdi artık daha önce oluşturduğumuz listeleme senaryoyu yeniden ziyaret ve ölçeklenebilir sayfalama desteğini etkinleştirin.

> [!div class="step-by-step"]
> [Önceki](use-viewdata-and-implement-viewmodel-classes.md)
> [İleri](implement-efficient-data-paging.md)
