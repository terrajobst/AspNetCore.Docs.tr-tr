---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: ASP.NET Web sayfaları sunarak - tutarlı bir düzen oluşturma | Microsoft Docs
author: tfitzmac
description: Bu öğretici düzenleri ASP.NET Web sayfaları kullanan bir site sayfalar için tutarlı bir görünüm oluşturmak için nasıl kullanılacağını gösterir. Tamamladığınızdan varsayar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>ASP.NET Web sayfaları sunarak - tutarlı bir düzen oluşturma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici nasıl kullanılacağını gösterir *düzenleri* ASP.NET Web sayfaları kullanan bir site sayfalar için tutarlı bir görünüm oluşturmak için. Seri aracılığıyla tamamladığınızdan varsayar [veritabanı veri ASP.NET Web sayfalarını silme](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Öğrenecekleriniz:
> 
> - Hangi bir düzen sayfasıdır.
> - Düzen sayfaları ile dinamik içerik birleştirmek nasıl.
> - Bir düzen sayfası değerleri geçirmek nasıl.


## <a name="about-layouts"></a>Düzenler hakkında

Şu ana kadar oluşturdum sayfaları tüm tam tek başına sayfa. Bunların tümü aynı siteye ait, ancak tüm ortak öğeler veya standart bir görünüm yok.

Sitelerinin çoğu bir tutarlı bir görünüm ve düzeni sahip. Örneğin, giderseniz [Microsoft.com/web](https://www.microsoft.com/web/) site ve araştırın, tüm sayfaları genel bir düzen ve görsel tema için uyması bakın:

![Üstbilgi, gezinti alanını, içerik alanının ve altbilgi düzenini gösteren Microsoft.com/web site sayfası](layouts/_static/image1.png)

Bir *verimsiz* bu düzeni oluşturmak için bir yol, bir başlığı, gezinti çubuğu ve altbilgi sayfalarınızın her biri üzerinde ayrı olarak tanımlamak için olacaktır. Her zaman aynı biçimlendirme çoğaltma. Bir şey değiştirmek istiyorsanız (örneğin, alt bilgi güncelleştirmesi), her sayfanın ayrı olarak değiştirmeniz gerekir.

Where olan *Düzen sayfaları* geldikçe. ASP.NET Web Pages'da sitenizdeki sayfaları için genel bir kapsayıcı sağlayan düzen sayfası tanımlayabilirsiniz. Örneğin, düzen sayfası üstbilgi, gezinti alanını ve altbilgi içerebilir. Düzen sayfası ana içeriğin nereye gideceğini yer tutucu içerir.

Ardından, biçimlendirme ve yalnızca bu sayfa için kod içeren her bir içerik sayfayı tanımlayabilirsiniz. İçerik sayfaları tam HTML sayfaları olmak zorunda değildir; için bile sahip olmayan bir `<body>` öğesi. Aynı zamanda bir içeriği görüntülemek istediğiniz hangi düzen sayfası ASP kod satırı sahiptirler. Kabaca bu ilişkiyi nasıl çalıştığını gösteren resim şöyledir:

![İki içerik sayfaları ve içine sığması düzen sayfası gösteren kavramsal diyagram](layouts/_static/image2.png)

Bu etkileşimi eylemde gördüğünüzde anlamak kolaydır. Bu öğreticide, bir düzeni kullanmak için filmler sayfalarınızı değiştireceksiniz.

## <a name="adding-a-layout-page"></a>Düzen sayfası ekleme

Üstbilgi ve altbilgi ana içerik için bir alanı ile bir tipik sayfa düzeni tanımlayan düzen sayfası oluşturarak başlayacağız. Adlı CSHTML sayfa WebPagesMovies sitede ekleme  *\_Layout.cshtml*.

Önde gelen alt çizgi ( `_` ) karakterdir önemli. Bir sayfanın adı bir alt çizgiyle başlıyorsa, ASP.NET doğrudan bu sayfayı tarayıcıya göndermek olmaz. Bu kural, kullanıcıları doğrudan isteği mümkün olmaması gerekir ancak bu, siteniz için gerekli olan sayfaları tanımlamanıza olanak sağlar.

Sayfa içeriği aşağıdakiyle değiştirin:

[!code-html[Main](layouts/samples/sample1.html)]

Gördüğünüz gibi bu biçimlendirme kullanan yalnızca HTML'dir `<div>` üç bölüm sayfa artı bir daha tanımlamak için öğeleri `<div>` üç bölüm tutacak öğesi. Altbilgi biraz Razor kod içerir: `@DateTime.Now.Year`, hangi sokacak geçerli yıl sayfasında bu konumda.

Adlı bir stil sayfası bağlantısını fark *Movies.css*. Öğeleri fiziksel düzenini ayrıntılarını tanımlandığı stil sayfasıdır. Bir dakika içinde oluşturacaksınız.

Bu yalnızca olağan dışı özelliği  *\_Layout.cshtml* sayfası `@Render.Body()` satır. Bu düzen başka bir sayfaya ile birleştirildiğinde içeriği nereye yer tutucu olmasıdır.

## <a name="adding-a-css-file"></a>.Css dosyası ekleme

Sayfada öğeleri gerçek düzenleme (diğer bir deyişle, Görünüm) tanımlamak için tercih edilen yol geçişli stil sayfası (CSS) kurallarını kullanmaktır. Oluşturacağınız şekilde bir *.css* yeni düzeninizi kurallarını sahip dosya.

Webmatrix'te, sitenizin kök seçin. Sonra **dosyaları** sekmesi altında Şeritinde oku **yeni** düğmesine tıklayın ve ardından **yeni klasör**.

![Şeritte 'Yeni Klasör' seçeneği yeni altında.](layouts/_static/image3.png)

Yeni bir klasör adı *stilleri*.

![Yeni Klasör 'Stiller' adlandırma](layouts/_static/image4.png)

Yeni içinde *stilleri* klasör adında bir dosya oluşturun *Movies.css*.

![Yeni bir Movies.css dosyası oluşturma](layouts/_static/image5.png)

Yeni Değiştir *.css* aşağıdaki dosyasıyla:

[!code-css[Main](layouts/samples/sample2.css)]

Çoğu iki şey not üzere dışında bu CSS kuralları hakkında dediğimiz olmaz. Yazı tipleri ve boyutları ayarlamaya ek olarak, kuralları mutlak konumlandırma üstbilgi, altbilgi ve ana içerik alanı konumunu oluşturmak için kullandığınız paroladır. CSS konumlandırma yenisiniz varsa, okuyabilirsiniz [CSS konumlandırma](http://www.w3schools.com/css/css_positioning.asp) W3Schools sitesindeki öğretici.

Alt kısmında, biz özgün olarak olan stil kurallarını kopyaladığınızdan emin tanımlanmış ayrı ayrı olarak Not başka bir şey *Movies.cshtml* dosya. Bu kurallar de kullanılan [görüntüleme veri tarafından ASP.NET Web sayfalarını kullanarak giriş](https://go.microsoft.com/fwlink/?LinkId=251580) yapmak için öğretici `WebGrid` yardımcı işleme şeritler tabloya eklenen biçimlendirme. (Kullanmak için kullanacaksanız bir *.css* dosya Stil tanımları için de stil kurallarını tüm sitenin içinde yerleştirdiğiniz.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Düzen kullanılacak filmler dosyasını güncelleştirme

Şimdi yeni düzeni kullanmak için sitenizdeki varolan dosyaların güncelleştirebilirsiniz. Açık *Movies.cshtml* dosya. En üstte, kod, ilk satır aşağıdakileri ekleyin:

[!code-csharp[Main](layouts/samples/sample3.cs)]

Şimdi şu şekilde başlatılır:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Bu bir kod satırı ASP olduğunda *filmler* sayfa çalıştırır, ile birleştirilmesini  *\_Layout.cshtml* dosya.

Bu yana *Movies.cshtml* dosya artık düzen sayfası kullanır, biçimlendirmeden kaldırabilirsiniz *Movies.cshtml* tarafından hallolduğuna sayfa  *\_Layout.cshtml*dosya. Çıkın `<!DOCTYPE>`, `<html>`, ve `<body>` açma ve kapatma etiketleri. Tüm alın `<head>` öğesi ve bu kuralları şimdi var ki bu yana hangi kılavuz için stil kurallarını içerir içeriği, bir *.css* dosya. Varolan adresinden iken değiştirme `<h1>` öğesine bir `<h2>` öğesi; sahip bir `<h1>` düzen sayfası zaten öğesinde. Değişiklik `<h2>` "Listesi filmler" için metin.

Normal bir içerik sayfasında bu tür değişiklikler yapmak zorunda olmayacaktır. Olan düzen sayfası başlattığınız siteniz olduğunda, tüm bu öğeler olmadan içerik sayfalarını başından itibaren oluşturun. Bu durumda, ancak, bir tek başına sayfa; böylece bir bit temizleme bir düzeni kullanan bir dönüştürdüğünüz.

İşiniz bittiğinde *Movies.cshtml* sayfasında, aşağıdaki gibi görünür:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Düzen test etme

Şimdi düzenini nasıl göründüğünü görebilirsiniz. Webmatrix'te sağ *Movies.cshtml* sayfasından seçim yapıp **başlatma tarayıcıda**. Tarayıcı sayfası görüntülendiğinde bu sayfayı gibi görünüyor:

![Bir düzen kullanılarak oluşturulması filmler sayfası](layouts/_static/image6.png)

ASP.NET Movies.cshtml sayfasının içeriği birleştirildi  *\_Layout.cshtml* sayfasında sağ nereye `RenderBody` yöntemidir. Ve tabi ki  *\_Layout.cshtml* sayfasında başvurular bir *.css* sayfa görünümünü tanımlayan dosyası.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>AddMovie sayfa düzeni kullanmak için güncelleştiriliyor

Düzenleri gerçek faydası, onları tüm sayfalar için sitenizdeki sağlamasıdır. Açık *AddMovie.cshtml* sayfası.

Unutmamanız *AddMovie.cshtml* sayfa başlangıçta olan bazı CSS kuralları doğrulama hatası iletilerinin görünümünü tanımlamak için bunu. Sahip olduğu bir *.css* dosya siteniz şimdi için bu kuralların taşıyabilirsiniz *.css* dosya. Bunları kaldırmak *AddMovie.cshtml* dosya ve alt kısmına ekleyin *Movies.css* dosya. Aşağıdaki kuralları taşıdığınız:

[!code-css[Main](layouts/samples/sample6.css)]

Şimdi değişiklikleri aynı tür olun *AddMovie.cshtml* yaptığınız *Movies.cshtml* — eklemek `Layout="~/_Layout.cshtml;` ve şimdi yabancı HTML biçimlendirme kaldırın. Değişiklik `<h1>` öğesine `<h2>`. İşiniz bittiğinde, sayfa bu örnekteki gibi görünür:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Sayfayı çalıştırın. Şimdi bu çizim gibi görünür:

![Bir düzen kullanılarak oluşturulması 'Filmler Ekle' sayfası](layouts/_static/image7.png)

Site sayfalarına benzer değişiklikler yapmak istediğiniz — *EditMovie.cshtml* ve *DeleteMovie.cshtml*. Ancak, bunu yapmadan önce biraz daha esnek hale getirir düzenini başka bir değişiklik yapabilirsiniz.

## <a name="passing-title-information-to-the-layout-page"></a>Düzen sayfası geçirme başlık bilgileri

 *\_Layout.cshtml* oluşturduğunuz sayfasına sahip bir `<title>` "Film Sitem" ayarlamak öğesi. Çoğu tarayıcılar bu öğenin içeriğini bir sekmede metin olarak görüntüle:

![Sayfanın &lt;başlık&gt; bir tarayıcı sekmesinde görüntülenen öğesi](layouts/_static/image8.png)

Bu başlık bilgileri geneldir. Geçerli sayfa için daha belirgin olması için başlık metni istediğinizi varsayalım. (Başlık metni arama motorları tarafından da sayfanızı hakkındadır belirlemek için kullanılır.) Bir içerik sayfasındaki gibi bilgileri geçirebilirsiniz *Movies.cshtml* veya *AddMovie.cshtml* Düzen sayfasını ve sonra kullanmak için Düzen sayfası özelleştirmek için bu bilgileri işler.

Açık *Movies.cshtml* yeniden sayfa. Üst kod içinde aşağıdaki satırı ekleyin:

[!code-csharp[Main](layouts/samples/sample8.cs)]

`Page` Nesnenin kullanılabilir tüm *.cshtml* sayfaları ve bu amaçla, yani olan bir sayfanın düzenini arasında bilgi paylaşmak için.

Açık<em>\_Layout.cshtml</em> sayfası. Değişiklik `<title>` öğesi, BT'nin bu biçimlendirme gibi görünür:

[!code-html[Main](layouts/samples/sample9.html)]

Bu kod içinde ne olursa olsun işler `Page.Title` özellik sayfası bu konumda sağ.

Çalıştırma *Movies.cshtml* sayfası. Bu zaman tarayıcı sekmesinde gösterir değeri olarak geçirilen `Page.Title`:

![Dinamik olarak oluşturulan başlık gösteren bir tarayıcı sekmesi](layouts/_static/image9.png)

İsterseniz, sayfa kaynağı tarayıcıda görüntüleyin. Görebilirsiniz `<title>` öğesi olarak oluşturulur `<title>List Movies</title>`.

> [!TIP] 
> 
> **The Page Object**
> 
> Kullanışlı bir özelliği `Page` bir dinamik Nesne olmasıdır — `Title` özelliği bir sabit ya da ayrılmış adı değil. Kullanabileceğiniz *herhangi* değerini adı `Page` nesnesi. Örneğin, kolayca başlığı adlı bir özellik kullanarak geçtiğiniz `Page.CurrentName` veya `Page.MyPage`. Yalnızca kısıtlama adı hangi özelliklerin adlı normal kuralları izlemek sahip olur. (Örneğin, ad boşluk içeremez.)
> 
> Herhangi bir sayıda değerleri kullanarak geçirebilirsiniz `Page` nesnesi. Düzen sayfası film bilgileri geçirmek istiyorsanız, aşağıdakine benzer kullanarak değerleri geçirebilirdiniz `Page.MovieTitle` ve `Page.Genre` ve `Page.MovieYear`. (Veya bilgileri depolamak için geliştirmiştir herhangi bir ad.) Tek gereksinim — büyük olasılıkla belirgin olduğu — içerik sayfasını ve düzen sayfası aynı adı kullanmak zorunda değil.
> 
> Geçirdiğiniz kullanarak bilgi `Page` nesne düzeni sayfasında görüntülenecek yalnızca metin ile sınırlı değildir. Düzen sayfası için bir değer geçirebilirsiniz ve düzen sayfası kodda sayfanın bölümünü görüntülemek karar vermek için değeri daha sonra kullanabilirsiniz ne *.css* kullanmak için dosya ve benzeri. Geçirdiğiniz değerleri `Page` nesne olan diğer değerleri gibi kullandığınız kodu. Yalnızca değerleri içerik sayfasındaki kaynaklanan ve düzen sayfası geçirilen değil.


Açık *AddMovie.cshtml* sayfasında ve bir satır için bir başlık sağlar kod üstüne ekleyin *AddMovie.cshtml* sayfa:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Çalıştırma *AddMovie.cshtml* sayfası. Yeni başlık bakın:

![Dinamik olarak oluşturulan 'Filmler Ekle' başlık gösteren bir tarayıcı sekmesi](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Düzeni kullanmak için kalan sayfalarını güncelleştirme

Şimdi yeni düzeni kullanmasını sağlayarak sitenizdeki kalan sayfalarını bitirebilirsiniz. Açık *EditMovie.cshtml* ve *DeleteMovie.cshtml* içinde açın ve her birini aynı değişiklikleri yapın.

Düzen sayfası bağlantıları kod satırını ekleyin:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Sayfanın başlığı ayarlamak için bir satır ekleyin:

[!code-csharp[Main](layouts/samples/sample12.cs)]

veya:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Tüm gereksiz HTML biçimlendirmesi Kaldır — temel olarak, yalnızca içinde olduğunda BITS bırakın `<body>` öğesi (artı üst kod bloğu).

Değişiklik `<h1>` olmasını öğesi bir `<h2>` öğesi.

Bu değişiklikler yaptığınızda, her test edin ve düzgün bir şekilde görüntüleme ve başlık doğru olduğundan emin olun.

## <a name="parting-thoughts-about-layout-pages"></a>Herhangi düşüncelerinizi Düzen sayfaları hakkında

Bu öğreticide oluşturduğunuz bir  *\_Layout.cshtml* sayfasında ve kullanılan `RenderBody` içeriği başka bir sayfadan birleştirmek için yöntem. Web sayfalarında düzenleri kullanmak için temel düzeni olmasıdır.

Düzen sayfaları biz burada kapak kaydetmedi ek özellikler vardır. Örneğin, Düzen sayfaları geçirebilmenize — bir düzen sayfası sırayla başvuru başka. İç içe geçmiş düzenleri farklı düzenleri gerektiren alt site ile çalışıyorsanız yararlı olabilir. Ek yöntemleri de kullanabilirsiniz (örneğin, `RenderSection`) ayarlamak için Düzen sayfası bölümlerde adlı.

Birleşimi, Düzen sayfaları ve *.css* dosyaları güçlü. Webmatrix'te sonraki öğretici serisinde anlatıldığı gibi temel bir site oluşturabilirsiniz bir *şablonu*, hangi size sahip bir site önceden oluşturulmuş işlevindeki. Şablonlar, Düzen sayfaları ve CSS harika arayın ve menüleri gibi özellikleri olan siteler oluşturmak için iyi kullanılmasını sağlamak. Aşağıda, Düzen sayfaları ve CSS kullanan özellikler gösteren bir şablonu temel alan bir siteden giriş sayfasının ekran görüntüsü verilmiştir:

![Üstbilgi, gezinti alanını, içerik alanının, isteğe bağlı bir bölüm ve oturum açma bağlantıları gösteren WebMatrix site şablonu tarafından oluşturulan düzeni](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Tam listesi için (bir düzen sayfasını kullanmak üzere güncelleştirilir) film sayfası

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Tam sayfa için listeleme (düzeni için güncelleştirilmiş) film sayfasına ekleyin

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Delete film sayfa (düzeni için güncelleştirilmiş) için tam sayfa listeleme

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Düzen film sayfa (düzeni için güncelleştirilmiş) için tam sayfa listeleme

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğreticide herkes görebilmeniz için Internet'e sitenizi yayımlayın öğreneceksiniz.

## <a name="additional-resources"></a>Ek Kaynaklar

- [Tutarlı görünecek oluşturma](https://go.microsoft.com/fwlink/?LinkID=202891) — bir makale düzenleri ile çalışma hakkında bazı daha fazla ayrıntı sağlar. Ayrıca, gösterir veya gizler içeriğin bir kısmı bir düzen sayfası için bir değer geçirmek nasıl açıklanır.
- [İç içe, Düzen sayfaları Razor ile](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — CAN Brind bloglar Düzen sayfaları yerleştirmek nasıl bir örneği. (Sayfaları yüklenmesini kapsar.)

> [!div class="step-by-step"]
> [Önceki](deleting-data.md)
> [sonraki](publishing.md)
