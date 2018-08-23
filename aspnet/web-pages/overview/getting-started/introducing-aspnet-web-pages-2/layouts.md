---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Tutarlı bir düzen oluşturma - ASP.NET Web sayfaları ile tanışın | Microsoft Docs
author: tfitzmac
description: Bu öğreticide, ASP.NET Web sayfaları kullanan bir site sayfalar için tutarlı bir görünüm oluşturmak için düzenlerini kullanmayı gösterir. Tamamladığınızdan varsayar...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 8f3d9e8a6f6a0179224e18faf11db3dc1510a095
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755112"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>ASP.NET Web sayfalarına giriş - tutarlı bir düzen oluşturma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğreticide nasıl kullanılacağını gösterir *düzenleri* kullanan ASP.NET Web sayfaları sitesinde sayfalar için tutarlı bir görünüm oluşturmak için. Bu seriyi aracılığıyla bitirdiğinizi [veritabanı verilerini silme ASP.NET Web Pages'de](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Öğrenecekleriniz:
> 
> - Bir düzen sayfası ' dir.
> - Yerleşim sayfaları dinamik içerik ile bir araya getirilebileceğini öğrenin.
> - Bir düzen sayfasına değerleri geçirmek nasıl.


## <a name="about-layouts"></a>Düzenler hakkında

Şu ana kadar oluşturduğunuz sayfaları tüm tam tek başına sayfa. Bunların tümü aynı siteye ait, ancak ortak öğeleri veya standart bir görünüm yok.

Çoğu siteleri bir tutarlı bir görünüm ve Düzen sahip. Örneğin, Git, [Microsoft.com/web](https://www.microsoft.com/web/) site ve araştırın, tüm sayfaların genel bir düzeni ve görsel temasını uygun olacağını göreceksiniz:

![Üst bilgi, gezinti alanına, içerik alanı ve alt bilgi düzenini gösteren Microsoft.com/web site sayfası](layouts/_static/image1.png)

Bir *verimsiz* üst bilgi, gezinti çubuğunda ve altbilgi her sayfalarınızın ayrı olarak tanımlamak için bu düzeni oluşturmak için bir yol olur. Her zaman aynı biçimlendirme çoğaltma. Bir şeyle değiştirmek istiyorsanız (örneğin, alt bilgi güncelleştirme) her sayfaya ayrı olarak değiştirmeniz gerekecektir.

Olduğu yer *Düzen sayfaları* vardır. ASP.NET Web sayfaları'nda, sitenizdeki sayfaları için genel bir kapsayıcı sağlayan bir düzen sayfası tanımlayabilirsiniz. Örneğin, düzen sayfası, üst bilgi, gezinti alanı ve alt bilgi içerebilir. Düzen sayfası ana içeriğin nereye gideceğini yer tutucu içerir.

Ardından, biçimlendirme ve yalnızca o sayfanın kodu içeren her bir içerik sayfayı tanımlayabilirsiniz. İçerik sayfaları tam HTML sayfalarını olmak zorunda değildir; Bunlar bile sahip olmak zorunda değildir bir `<body>` öğesi. Ayrıca bir ASP hangi düzen sayfası içeriği görüntülemek istediğiniz kod satırının sahiptirler. Kabaca bu ilişkiyi nasıl çalıştığını gösteren resim şu şekildedir:

![İki içerik sayfaları ve içine sığması bir düzen sayfası gösteren kavramsal diyagram](layouts/_static/image2.png)

Bu etkileşimi nasıl çalıştığını görün anlamak kolaydır. Bu öğreticide, bir düzeni kullanmak için filmler sayfalarınızı değiştireceksiniz.

## <a name="adding-a-layout-page"></a>Bir düzen sayfası ekleme

Bir üstbilgi, altbilgi ve ana içerik için bir alan tipik sayfa düzeniyle tanımlayan bir düzen sayfası oluşturarak başlayacağız. WebPagesMovies sitede adlı CSHTML sayfa ekleme  *\_Layout.cshtml*.

Başındaki altçizgiyi ( `_` ) karakteri önemlidir. Bir sayfanın adı bir alt çizgiyle başlıyorsa ASP.NET bu sayfayı doğrudan tarayıcıya göndermez. Bu kural, ancak bu kullanıcıları doğrudan istek işleyememelidir siteniz için gerekli olan sayfa tanımlamanıza olanak sağlar.

Sayfa içeriğini aşağıdakiyle değiştirin:

[!code-html[Main](layouts/samples/sample1.html)]

Gördüğünüz gibi bu işaretleme kullanan yalnızca HTML'dir `<div>` sayfa ayrıca bir üç bölüm daha tanımlamak için öğeleri `<div>` üç bölüm tutmak için öğesi. Altbilgi Razor kodunun bir bit içerir: `@DateTime.Now.Year`, hangi işleme geçerli yıl sayfasında bu konumdaki.

Adlı bir stil sayfası bağlantısı olduğunu fark *Movies.css*. Öğeleri fiziksel düzenini ayrıntılarını tanımlanacağı stil sayfası alınır. Bir dakika içinde oluşturacaksınız.

Bu yalnızca olağan dışı özellik  *\_Layout.cshtml* sayfasıdır `@Render.Body()` satır. Bu düzen, başka bir sayfa ile birleştirildiğinde içeriği gidecekleri yer tutucu olmasıdır.

## <a name="adding-a-css-file"></a>.Css dosyasını ekleme

Sayfada öğeleri gerçek yerleşimini (diğer bir deyişle, Görünüm) tanımlamak için tercih edilen yoludur, geçişli stil sayfası (CSS) kurallarını kullanmaktır. Oluşturacağınız şekilde bir *.css* kuralları, yeni düzene sahip bir dosya.

Webmatrix'te, sitenizin kök seçin. Ardından **dosyaları** sekmesi altında Şerit, oku **yeni** düğmesine ve ardından **yeni klasör**.

![Şeritteki yeni 'Yeni Klasör' seçeneği.](layouts/_static/image3.png)

Yeni klasör adı *stilleri*.

![Yeni Klasör 'Stilleri' adlandırma](layouts/_static/image4.png)

İçinde yeni *stilleri* klasöründe adlı bir dosya oluşturun *Movies.css*.

![Yeni bir Movies.css dosyası oluşturma](layouts/_static/image5.png)

Yeni içeriklerini *.css* aşağıdaki dosya:

[!code-css[Main](layouts/samples/sample2.css)]

İki şey not üzere dışında bu CSS kurallarını hakkında pek fazla dediğimiz olmaz. Yazı tiplerine ve boyutlarına ayarlamanın yanı sıra kuralları mutlak konumlandırma üstbilgi, altbilgi ve ana içerik alanı konumu'kurmak için kullandığınız paroladır. Edinebilirsiniz, CSS konumlandırma yeni tanışıyorsanız, [CSS konumlandırma](http://www.w3schools.com/css/css_positioning.asp) W3Schools sitesindeki öğretici.

En altında ki özgün olarak olan Stil kurallarının kopyalamış olduğunuz tanımlanmış tek tek de dikkat edilecek diğer şey *Movies.cshtml* dosya. Bu kurallar de kullanılan [veri görüntüleme ile ASP.NET Web sayfaları kullanarak giriş](https://go.microsoft.com/fwlink/?LinkId=251580) yapmak için öğreticiyi `WebGrid` yardımcı işleme şeritler tabloya eklenen biçimlendirme. (Kullanmak için kullanacaksanız bir *.css* dosya Stil tanımları için de Stil kurallarının tüm sitenin içinde yerleştirdiğiniz.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Bu düzeni kullanmak için filmler dosyası güncelleştiriliyor

Artık sitenizin yeni düzeni kullanmak için mevcut dosyaları güncelleştirebilirsiniz. Açık *Movies.cshtml* dosya. En üstünde, kodun ilk satırı aşağıdakileri ekleyin:

[!code-csharp[Main](layouts/samples/sample3.cs)]

Sayfa artık şu şekilde başlar:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Bu tek satırlık bir kod ASP olduğunda *filmler* sayfa çalıştırılırsa ile birleştirilmesini  *\_Layout.cshtml* dosya.

Bu yana *Movies.cshtml* dosyası, bir yerleşim sayfası artık kullanır, biçimlendirmeden kaldırabilirsiniz *Movies.cshtml* tarafından dikkate sayfa  *\_Layout.cshtml*dosya. Çıkın `<!DOCTYPE>`, `<html>`, ve `<body>` açılış ve kapanış etiketlerinin. Tüm Al `<head>` öğesi ve artık bu kurallar kendinizi ComUnregisterFunction kılavuz stili kurallarını içerir, içeriğinin bir *.css* dosya. Mevcut olduğundan iken değiştirmek `<h1>` öğesine bir `<h2>` öğesi; olan bir `<h1>` düzen sayfası zaten öğesinde. Değişiklik `<h2>` "Listesi filmler" metni.

Normalde bir içerik sayfasında bu tür değişiklikler yapmak zorunda mıydı. Olan düzen sayfası başlattığınız siteniz olduğunda, bu öğeler olmadan içerik sayfalarını başlangıç olarak oluşturun. Bu durumda, yine de, bir tek başına sayfa biraz temizlik, bu nedenle, bir düzeni kullanan bir dönüştürüyoruz.

İşiniz bittiğinde, *Movies.cshtml* sayfasında, aşağıdaki gibi görünür:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Test düzeni

Artık düzenini nasıl göründüğünü görebilirsiniz. Webmatrix'te, sağ *Movies.cshtml* sayfasından seçim yapıp **tarayıcıda Başlat**. Tarayıcı sayfası görüntülendiğinde, bu sayfada gibi görünür:

![Bir düzen kullanılarak oluşturulması filmler sayfası](layouts/_static/image6.png)

ASP.NET Movies.cshtml sayfanın içeriğini birleştirildi  *\_Layout.cshtml* sayfasında doğru nerede `RenderBody` yöntemidir. Ve Elbette  *\_Layout.cshtml* sayfasında başvurular bir *.css* sayfa görünümünü tanımlayan dosya.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>AddMovie sayfa düzeni kullanmak için güncelleştiriliyor

Düzenleri gerçek avantajı, bunları tüm sayfalar için sitenizde sağlamasıdır. Açık *AddMovie.cshtml* sayfası.

Unutmayın *AddMovie.cshtml* sayfası ilk olan bazı CSS kurallarını doğrulama hatası iletilerinin görünümünü tanımlamak için bunu. Elinizde bu yana bir *.css* dosya sitenizin artık bu kuralları taşıyabilirsiniz *.css* dosya. Kaldırabilir *AddMovie.cshtml* dosya ve alt kısmına ekleyin *Movies.css* dosya. Aşağıdaki kurallar taşıdığınız:

[!code-css[Main](layouts/samples/sample6.css)]

Şimdi değişiklikleri aynı tür hale *AddMovie.cshtml* yaptığınız *Movies.cshtml* — ekleme `Layout="~/_Layout.cshtml;` ve artık fazlalık HTML biçimlendirmeyi kaldırın. Değişiklik `<h1>` öğesine `<h2>`. İşiniz bittiğinde, sayfa şu örnekteki gibi görünür:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Sayfayı çalıştırın. Artık bu çizim gibi görünür:

![Bir düzen kullanılarak oluşturulması Sayfası 'Filmler Ekle'](layouts/_static/image7.png)

Site sayfaları benzer değişiklikler yapmak istediğiniz — *EditMovie.cshtml* ve *DeleteMovie.cshtml*. Ancak, bunu yapmadan önce biraz daha esnek yapan düzene başka bir değişiklik yapabilirsiniz.

## <a name="passing-title-information-to-the-layout-page"></a>Düzen Sayfası başlık bilgilerini geçirme

*\_Layout.cshtml* oluşturduğunuz sayfasına sahip bir `<title>` "My film sitesine" öğesi. Çoğu tarayıcısı bu öğenin içeriğini bir sekme üzerindeki metin olarak görüntüle:

![Sayfanın &lt;başlık&gt; bir tarayıcı sekmesinde görüntülenen öğe](layouts/_static/image8.png)

Bu başlık bilgilerini geneldir. Başlık metni geçerli sayfa için ayrıntılı olmasını istediğinizi varsayalım. (Başlık metni arama motorları tarafından da sayfanızı ne hakkında olduğunu belirlemek için kullanılır.) İçerik sayfasından gibi bilgileri geçirebilirsiniz *Movies.cshtml* veya *AddMovie.cshtml* Düzen sayfasını ve ardından Düzen sayfası özelleştirmek için bu bilgileri işler.

Açık *Movies.cshtml* yeniden sayfa. Üst kod içinde aşağıdaki satırı ekleyin:

[!code-csharp[Main](layouts/samples/sample8.cs)]

`Page` Nesnedir tüm kullanılabilir *.cshtml* sayfaları ve bu amaçla, yani ise bir sayfa ve powerapps'in düzen arasında bilgi paylaşımı için.

Açık<em>\_Layout.cshtml</em> sayfası. Değişiklik `<title>` öğesi olan bu biçimlendirme gibi görünür:

[!code-html[Main](layouts/samples/sample9.html)]

Bu kod içinde ne olursa olsun işler `Page.Title` özelliği sayfasında bu konumdaki sağ.

Çalıştırma *Movies.cshtml* sayfası. Bu süre tarayıcı sekmesini gösteren değeri olarak geçirilen `Page.Title`:

![Dinamik olarak oluşturulan bir başlık gösteren bir tarayıcı sekmesi](layouts/_static/image9.png)

İsterseniz, sayfa kaynağı tarayıcıda görüntüleme. Gördüğünüz gibi `<title>` öğesi olarak işlenen `<title>List Movies</title>`.

> [!TIP] 
> 
> **Sayfa nesnesi**
> 
> Kullanışlı bir özelliği `Page` dinamik Nesne olmasıdır; `Title` özellik sabit veya ayrılmış bir ad değil. Kullanabileceğiniz *herhangi* değerini adı `Page` nesne. Örneğin, kolayca başlık adlı bir özellik kullanarak başarılı `Page.CurrentName` veya `Page.MyPage`. Tek kısıtlama adı için hangi özelliklerin adlı normal kurallara uymak sahip olur. (Örneğin, ad boşluk içeremez.)
> 
> Herhangi bir sayıda değerleri kullanarak geçirebilirsiniz `Page` nesne. Düzen sayfasına film bilgi geçirmek istiyorsanız, aşağıdaki gibi kullanarak değerleri geçirebiliriz `Page.MovieTitle` ve `Page.Genre` ve `Page.MovieYear`. (Veya bilgileri depolamak için geliştirilen diğer adlar.) Tek gereksinim — büyük olasılıkla açık olduğu — içerik sayfası ve düzen sayfası aynı adları kullanılacak olmasıdır.
> 
> Geçirdiğiniz kullanarak bilgi `Page` nesne düzeni sayfasında görüntülenecek yalnızca metin sınırlı değildir. Düzen sayfası için bir değer geçirebilirsiniz ve düzen sayfası kod sayfası bir bölümünü görüntülemek karar vermek için değeri ardından kullanabilirsiniz ne *.css* kullanılacak dosya ve benzeri. Geçirdiğiniz değerleri `Page` nesne olan diğer değerleri gibi kullandığınız kod. Yalnızca değerleri içerik sayfasındaki kaynaklanan ve Düzen sayfasına geçirilir olduğu.


Açık *AddMovie.cshtml* sayfa ve bir satır için bir başlık sağladığı kodunu en üstüne ekleyin *AddMovie.cshtml* sayfası:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Çalıştırma *AddMovie.cshtml* sayfası. Yeni başlık görürsünüz:

![Dinamik olarak oluşturulan Ekle'filmler ' başlık gösteren bir tarayıcı sekmesi](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Bu düzeni kullanmak için geri kalan sayfalarını güncelleştirme

Şimdi yeni düzene kullanmasını sağlayarak sitenizdeki kalan sayfalarını tamamlayabilir. Açık *EditMovie.cshtml* ve *DeleteMovie.cshtml* içinde açın ve her aynı değişiklikleri yapın.

Düzen sayfasına bağlayan bir kod satırı ekleyin:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Sayfanın başlığını ayarlamak için bir satır ekleyin:

[!code-csharp[Main](layouts/samples/sample12.cs)]

veya:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Tüm gereksiz HTML biçimlendirmeyi kaldırmak — aslında içinde olan BITS bırakın `<body>` öğesi (Ayrıca üst kod bloğu).

Değişiklik `<h1>` olarak öğeyi bir `<h2>` öğesi.

Bu değişiklikler yaptınız, her test edin ve düzgün şekilde görüntülenmesini ve başlık doğru olduğundan emin olun.

## <a name="parting-thoughts-about-layout-pages"></a>Yerleşim sayfaları hakkında fikirlerinizi herhangi

Bu öğreticide oluşturduğunuz bir  *\_Layout.cshtml* sayfasında ve kullanılan `RenderBody` içeriği başka bir sayfadan birleştirmek için yöntemi. Düzenleri kullanarak Web sayfaları için temel düzeni olmasıdır.

Yerleşim sayfaları, burada ele yaramadı ek özellikleri vardır. Örneğin, Düzen sayfaları yuvalayabilirsiniz; bir düzen sayfası sırayla başvurabilir başka. İç içe geçmiş düzenleri alan farklı düzenler gerektiren bir site bölümlerini ile çalışıyorsanız yararlı olabilir. Ek yöntemleri de kullanabilirsiniz (örneğin, `RenderSection`) adlı düzen sayfası olarak bölümlerde ayarlamak için.

Birleşimi, Düzen sayfaları ve *.css* dosyaları güçlü. Webmatrix'te sonraki öğretici serisinde anlatıldığı gibi temel bir site oluşturabilirsiniz bir *şablon*, size sağlayan olan bir siteyi önceden oluşturulmuş işlevindeki. Şablonlar, Düzen sayfaları ve CSS, harika görünen ve menüler gibi özellikleri olan siteleri oluşturmak için iyi kullanılmasını sağlamak. Düzen sayfaları ve CSS özelliklerini gösteren bir şablonu temel alan bir siteden giriş sayfasının ekran görüntüsü aşağıda verilmiştir:

![Üst bilgi, gezinti alanına, içerik alanının, isteğe bağlı bir bölüm ve oturum açma bağlantılar gösteren WebMatrix site şablonu tarafından oluşturulan düzeni](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Tam listesi için (bir düzen sayfası kullanmak için güncelleştirilmiş) film sayfası

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Tam sayfa için listeleme (düzeni için güncelleştirilmiş) film sayfaya ekleyin

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Tam silme film sayfası (düzeni için güncelleştirilmiş) için sayfa listesi

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Tam sayfa listesi için düzenleme film sayfası (düzeni için güncelleştirilmiş)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğreticide, herkesin görebileceği şekilde Internet'e sitenizi yayımlama öğreneceksiniz.

## <a name="additional-resources"></a>Ek Kaynaklar

- [Tutarlı bir ara oluşturma](https://go.microsoft.com/fwlink/?LinkID=202891) — düzeni ile çalışma hakkında daha fazla ayrıntı sağlayan bir makale. Ayrıca, görüntüleyen veya gizleyen İçeriklerinden bazılarını bir düzen sayfası için bir değer geçirmek nasıl açıklar.
- [Razor sayfalarıyla Düzen iç içe geçmiş](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogları Düzen sayfaları iç içe ilişkin bir örnek. (Bir indirme sayfaları içerir.)

> [!div class="step-by-step"]
> [Önceki](deleting-data.md)
> [İleri](publishing.md)
