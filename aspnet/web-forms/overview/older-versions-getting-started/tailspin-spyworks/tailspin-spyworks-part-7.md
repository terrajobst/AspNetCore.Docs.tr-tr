---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: '7. Kısım: Özellik ekleme | Microsoft Docs'
author: JoeStagner
description: Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 7 hesabı geçirme gibi ek özellikler ekler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-7-adding-features"></a>7. Kısım: Özellikler ekleme
====================
tarafından [CAN Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir. Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 7 hesabı gözden geçirme, ürün incelemeleri ve "popüler öğeler" ve "da satın alınan" kullanıcı denetimleri gibi ek özellikler ekler.


## <a id="_Toc260221673"></a>  Özellik ekleme

Kullanıcıların bizim katalog göz atabilirsiniz ancak kendi alışveriş sepeti içinde öğeleri yerleştirin ve kullanıma alma işlemini tamamlamak, biz sitemizi geliştirmek için içerecektir destekleme sayısı özellikleri vardır.

1. Hesap gözden geçirme (listesi. siparişleri yerleştirilir ve ayrıntılarına bakın)
2. Bazı içeriği belirli içerik ön sayfasına ekleyin.
3. Kullanıcıların gözden geçirme izin vermek için bir özellik katalogdaki ürünlerin ekleyin.
4. Denetim popüler öğeler ve Yerleştir ön sayfada görüntülemek için bir kullanıcı denetimi oluşturun.
5. Bir "Da satın" kullanıcı denetimi oluşturma ve ürün ayrıntıları sayfasına ekleyin.
6. Bir kişi Ekle sayfası.
7. Ekleme bir sayfa hakkında.
8. Genel hata

## <a id="_Toc260221674"></a>  Hesap gözden geçirme

"Hesap" klasöründe iki .aspx sayfaları bir adlandırılmış OrderList.aspx ve adlandırılmış bir OrderDetails.aspx oluşturun

Daha önce sahip olduğumuz kadar OrderList.aspx GridView ve EntityDataSoure denetimleri özelliğinden yararlanır.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

Kullanıcı adı özniteliklerinde filtre Siparişler tablosundaki kayıtları EntityDataSoure seçer (WhereParameter bakın) kullanıcı oturumunun kullanıcının bağlandığınızda oturum değişkeni içinde ayarlarız.

Ayrıca bu GridView HyperlinkField parametrelerinde unutmayın:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Bu bağlantıyı OrderDetails.aspx sayfa bir sorgu dizesi parametresi olarak OrderID alan belirtme her ürün için sipariş ayrıntılarını görüntülemek için belirtin.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Siparişler bir FormView sipariş verilerini ve başka bir EntityDataSource GridView ile görüntülenecek ve siparişin tüm satır öğeleri görüntülemek için erişmek için bir EntityDataSource denetimini kullanacağız.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

Kod arkasında dosyasında (OrderDetails.aspx.cs) temizlik iki az bitleri sahip.

İlk olarak kimliğinizi Sipariş Ayrıntıları her zaman bir OrderID aldığından emin olmak gerekiyor.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Biz de hesaplamak ve görüntüleme satır öğelerden toplam sırası gerekir.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  Giriş sayfası

Bazı statik içerik için Default.aspx sayfasında ekleyelim.

Önce "İçerik" klasörü oluşturacaksınız ve içerdiği bir görüntü klasörü (ve giriş sayfasında kullanılacak resim eklemek.)

Aşağıdaki biçimlendirmede Default.aspx sayfasında alt tutucuyu ekleyin.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Ürün incelemeleri

Ürün Değerlendirmesi girmek için kullanabileceğiniz bir forma bir bağlantı içeren bir düğme ilk ekleyeceğiz.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Biz sorgu dizesinde ProductID geçtiğiniz unutmayın

Sonraki sayfa ReviewAdd.aspx adlı ekleyelim

Bu sayfa, ASP.NET AJAX Denetim Araç Seti kullanır. Buradan indirebilirsiniz şekilde, zaten yapmadıysanız, [DevExpress](http://devexpress.com/act) ve burada Visual Studio ile kullanmak için araç seti ayarlama hakkında rehberlik ise [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

Tasarım modunda denetimleri ve doğrulayıcıları Araç Kutusu'ndan sürükleyin ve aşağıdaki gibi bir form oluşturun.

![](tailspin-spyworks-part-7/_static/image2.jpg)

İşaretleme aşağıdakine benzer görünecektir.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Biz incelemeler girebilirsiniz, ürün sayfasında bu incelemeleri görüntülemek olanak sağlar.

Bu biçimlendirme ProductDetails.aspx sayfasına ekleyin.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Şimdi uygulamamız çalıştıran ve bir ürün için gezinme müşteri incelemelerini ürün bilgilerini gösterir.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Popüler öğeler denetimi (kullanıcı denetimleri oluşturma)

Web sitenizde satışları artırmak için birkaç özellik "müstehcen satış" popüler ya da ilgili ürünler için ekleyeceğiz.

Bu özelliklerin ilk bizim ürün kataloğunda daha popüler ürün listesini olacaktır.

"Kullanıcı uygulamamız giriş sayfasında öğelerinin satış üstünde görüntülenecek denetimi" oluşturacağız. Bu denetim olacağından, biz bunu herhangi bir sayfasında yalnızca sürükleyip biz gibi herhangi bir sayfaya Visual Studio'nun Tasarımcısı'nda denetimi bırakarak kullanabilirsiniz.

Visual Studio'nun Çözüm Gezgini'nde, çözüm adına sağ tıklayın ve "Denetimleri" adlı yeni bir dizin oluşturun. Bunu yapmak gerekli olmamasına karşın, sizi Projemizin "Denetimleri" dizininde bizim kullanıcı denetimleri oluşturarak tutmaya yardımcı olur.

Denetimleri klasörü sağ tıklatın ve "Yeni öğesi" seçin:

![](tailspin-spyworks-part-7/_static/image4.jpg)

"PopularItems" bizim denetimi için bir ad belirtin. Kullanıcı denetimleri için dosya uzantısı .ascx değil .aspx olduğuna dikkat edin.

Bizim popüler öğeleri kullanıcı denetimi şu şekilde tanımlanır.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Burada size henüz bu uygulamada kullandık olmayan bir yöntem kullanıyorsunuz. Yineleyici denetim kullanmakta olduğunuz ve bir veri kaynağı denetimi kullanmak yerine biz yineleyici denetim bir LINQ to Entities sorgusunun sonuçları bağlama.

Arka plan kod bizim denetiminin içinde aşağıdaki gibi bunu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Ayrıca bizim denetimin biçimlendirme üstündeki önemli bu satırı unutmayın.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

En popüler öğeleri bir dakika dakika temelinde değiştirme olmaz bu yana uygulama performansını artırmak için ağrıları getirir yönergesi ekleyebiliriz. Bu yönerge, önbelleğe alınan çıkış denetimi sona erdiğinde, yalnızca yürütülecek denetimleri kod neden olur. Aksi takdirde, denetimin çıkış önbelleğe alınan sürümü kullanılır.

Şimdi tüm yapmanız sahibiz bizim yeni denetim Default.aspc sayfamızı içerir.

Kullanım sürükleyip varsayılan formumuzun açık sütununda denetim örneği yerleştirilecek.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Şimdi biz giriş sayfası uygulamamız çalıştırdığınızda en popüler öğelerini görüntüler.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "Ayrıca satın" (kullanıcı denetimleri parametrelerle) denetleme

Oluşturacağız ikinci kullanıcı denetimi müstehcen içerik belirginliğe ekleyerek bir sonraki düzeye satış olur.

En üstteki "Da satın" öğelerini hesaplamak için mantık Önemsiz olmayan ' dir.

Bizim "Da satın" denetimi (önceden satın) sipariş ayrıntıları kayıtları için şu anda seçili ProductID seçin ve bulunan her benzersiz sıra OrderIDs alın.

Ardından biz al ürünleri tüm bu siparişleri ve miktarları satın alınan toplam seçer. Biz ürün tarafından miktar toplamı sıralama ve ilk beş öğeleri görüntüleyebilirsiniz.

Bu mantık karmaşıklığını verildiğinde, biz bu algoritma bir saklı yordam gerçekleştireceksiniz.

Saklı yordam için T-SQL aşağıdaki gibidir.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Biz, uygulamamız ve biz biz, tabloları ve gerektiğini, varlık veri modeli görünümleri yanı sıra belirtilen varlık veri modeli oluşturulan dahil olduğunda bu saklı yordam (SelectPurchasedWithProducts) veritabanında var olduğunu unutmayın Bu saklı yordam içermelidir.

Varlık veri modelinin saklı yordam erişmek için şu işlev içeri aktarmanız gerekir.

Varlık veri modeli Tasarımcısı'nda açmak ve modeli tarayıcısı açmak için Çözüm Gezgini'nde çift tıklatın, sonra tasarımcıda sağ tıklayın ve "İşlev içeri aktarma Ekle" seçin.

![](tailspin-spyworks-part-7/_static/image1.png)

Bunun yapılması, bu iletişim kutusunu açar.

![](tailspin-spyworks-part-7/_static/image2.png)

Yukarıdaki "SelectPurchasedWithProducts" seçerek gördüğünüz gibi alanları doldurun ve içeri aktarılan bizim işlevin adını yordam adı kullanın.

"Tamam" düğmesini tıklatın.

Bu size herhangi bir model öğesinde olabileceği gibi biz yalnızca saklı yordam karşı programlama yapabilirsiniz yapılır.

Bu nedenle, bizim "Denetimleri" klasöründe AlsoPurchased.ascx adlı yeni bir kullanıcı denetimi oluşturun.

Bu denetim için biçimlendirme PopularItems denetimine çok tanıdık gelecektir.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

Bu yana işlenmek üzere öğenin ürüne göre farklılık gösterir, çıktıyı önbelleğe değil önemli farktır.

ProductID denetlemek için "özelliği" olacaktır.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

Denetimin PreRender olayını işleyicisinde biz taşmas üç şey yapar.

1. ProductID ayarlandığından emin olun.
2. Satın alınan geçerli ürünleriyle olup olmadığına bakın.
3. #2'de belirlenen bazı öğeler çıktı.

Modeli aracılığıyla saklı yordam çağrısı ne kadar kolay olduğunu unutmayın.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Var. "da satın alınan" belirlendikten sonra biz yalnızca yineleyici sorgu tarafından döndürülen sonuçlar bağlayabilirsiniz.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

"Ayrıca satın" öğeler olsaydı biz popüler diğer öğeler yalnızca bizim Kataloğu'ndan görüntülersiniz.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

"Ayrıca satın" öğeleri görüntülemek için ProductDetails.aspx sayfasını açın ve böylece bu konumda bir işaretleme görünür AlsoPurchased denetim Çözüm Gezgini'nden sürükleyin.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Bunun yapılması denetimi başvuru tazelemek sayfanın en üstünde oluşturur.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

AlsoPurchased kullanıcı denetimi ProductID birkaç gerektirdiğinden biz bizim denetiminin ProductID özellik sayfasının geçerli veri modeli öğesi karşı Eval bir deyimi kullanarak ayarlar.

![](tailspin-spyworks-part-7/_static/image3.png)

Biz yapı ve şimdi çalıştırdığınızda ve bir ürün Gözat biz "Da satın" öğeleri görürler.

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Önceki](tailspin-spyworks-part-6.md)
> [sonraki](tailspin-spyworks-part-8.md)
