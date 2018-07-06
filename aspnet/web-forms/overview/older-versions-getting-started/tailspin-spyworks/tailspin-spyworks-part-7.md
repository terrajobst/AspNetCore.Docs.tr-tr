---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: '7. Bölüm: Özellik ekleme | Microsoft Docs'
author: JoeStagner
description: Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 7. Bölüm hesabı geçirme gibi ek özellikler ekler...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 47402ccdfb702dc1bb1bdb4e634a7cd6f5ebc235
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831654"
---
<a name="part-7-adding-features"></a>7. Bölüm: Özellik ekleme
====================
tarafından [ALi Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklenebilir uygulamalar oluşturmak için nasıl çok basit olduğunu gösterir. Bu, alışveriş, kullanıma alma ve yönetim gibi bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 7. Bölüm hesabı gözden geçirme, ürün incelemeleri ve "popüler öğeler" ve "Ayrıca satın alınan" kullanıcı denetimleri gibi ek özellikler ekler.


## <a id="_Toc260221673"></a>  Özellik ekleme

Kullanıcılar kataloğumuzu göz atabilirsiniz ancak öğe, alışveriş sepetine içinde yerleştirin ve kullanıma alma işlemini tamamlamak, biz sitemizi geliştirmek için içerecektir destekleme birkaç özellikleri vardır.

1. Hesap gözden geçirme (listesi. siparişler yerleştirilir ve ayrıntılarına bakın)
2. Bazı bağlam belirli bir içeriğe ön sayfaya ekleyin.
3. Kullanıcıların gözden geçirme sağlamak için bir özellik ürün kataloğunda ekleyin.
4. Popüler öğeler ve yerde denetleyen ön sayfada görüntülemek için bir kullanıcı denetimi oluşturun.
5. "Ayrıca satın alınan" bir kullanıcı denetimi oluşturun ve ürün ayrıntıları sayfasına ekleyin.
6. Bir kişi Ekle sayfası.
7. Ekleme bir sayfa hakkında.
8. Genel hata

## <a id="_Toc260221674"></a>  Hesap gözden geçirme

"Hesap" klasöründe bir adlandırılmış OrderList.aspx ve adlandırılmış bir OrderDetails.aspx iki .aspx sayfası oluşturma

Daha önce sahip olduğumuz kadar OrderList.aspx GridView ve EntityDataSoure denetimleri özelliğinden yararlanır.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

Kullanıcı adı özniteliklerinde filtrelenmiş Siparişler tablosundan kayıtları EntityDataSoure seçer (WhereParameter bakın) kullanıcı oturumunun kullanıcının, bir oturum değişkeni içinde ayarlarız.

Ayrıca bu parametreleri GridView'ın HyperlinkField dikkat edin:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Bu bağlantı ayrıntılarını görüntüleyebilmek OrderDetails.aspx sayfasına bir sorgu dizesi parametresi olarak OrderID alan belirtilmesi her ürün için belirtin.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Siparişler ve tüm sipariş satır öğeleri görüntülemek için bir FormView'da sipariş verilerini ve GridView ile başka bir EntityDataSource görüntülenecek erişmek için bir EntityDataSource denetimini kullanacağız.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

Kod dosyasında (OrderDetails.aspx.cs) iki biraz temizlik bitlerini sahibiz.

İlk olarak biz OrderDetails her zaman bir OrderID alır emin olmanız gerekir.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Ayrıca hesaplamak ve satır öğeleri toplam sırayı görüntülemek ihtiyacımız var.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  Giriş sayfası

Bazı statik içeriklerin Default.aspx sayfasına ekleyelim.

İlk "İçerik" klasörünü oluşturacaksınız ve içerdiği bir Resimler klasörü (ve giriş sayfasında kullanılacak bir görüntü yer alacak.)

Default.aspx sayfasında alt tutucusu aşağıdaki işaretlemeyi ekleyin.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Ürün incelemeleri

Ürün gözden geçirmesi girmek için kullanabileceğiniz bir forma ilk bağlantısını içeren bir düğme ekleyeceğiz.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

ProductID sorgu dizesinde geçiriyoruz olduğunu unutmayın.

Sonraki sayfa ReviewAdd.aspx adlı ekleyelim

Bu sayfa, ASP.NET AJAX Denetim Araç Seti kullanır. Buradan indirebileceğiniz şekilde, zaten yapmadıysanız, [DevExpress](http://devexpress.com/act) ve araç seti Visual Studio ile burada kullanmak için ayarlama hakkında yönergeler ise [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

Tasarım modunda denetimler ve doğrulayıcılar araç kutusundan sürükleyin ve aşağıdaki gibi bir form oluşturun.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Biçimlendirme, aşağıdaki gibi görünmelidir.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Biz incelemeleri girebilirsiniz, ürün sayfasında bu incelemeleri görüntülemek olanak tanır.

Bu işaretleme ProductDetails.aspx sayfasına ekleyin.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Şimdi uygulamamız çalıştıran ve bir ürün için gezinme müşteri incelemeleri de dahil olmak üzere ürün bilgileri gösterir.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Sık kullanılan öğe denetimi (kullanıcı denetimleri oluşturma)

Web sitenizde satışları artırmak için birkaç özellik "müstehcen sell" popüler veya ilgili ürünler ekleyeceğiz.

Bu özelliklerin ilk ürün kataloğu daha popüler ürün listesi olacaktır.

"Kullanıcı satış uygulamamız giriş sayfasındaki öğelerin üst görüntülenecek denetimi" oluşturacağız. Bu denetim olacağından, bunu herhangi bir sayfa üzerinde yalnızca sürükleyip istiyoruz herhangi bir sayfaya Visual Studio'nun Tasarımcısı'nda denetim bırakarak kullanabiliriz.

Visual Studio Çözüm Gezgini'nde, çözüm adına sağ tıklayın ve "Controls" adlı yeni bir dizin oluşturun. Bunu yapmak gerekli olmamasına karşın, biz "Controls" dizininde bizim kullanıcı denetimleri oluşturarak Projemizin kalmasına yardımcı olur.

Denetimleri klasörü sağ tıklatın ve "Yeni öğe" seçin:

![](tailspin-spyworks-part-7/_static/image4.jpg)

"PopularItems" bizim denetimi için bir ad belirtin. Kullanıcı denetimleri için dosya uzantısı .ascx değil .aspx olduğuna dikkat edin.

Popüler öğeleri kullanıcı denetimimiz şu şekilde tanımlanır.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Burada henüz bu uygulamada kullandık olmayan bir yöntem kullanıyoruz. Repeater denetimiyle kullanıyoruz ve bir veri kaynağı denetimi kullanmak yerine Repeater denetiminde bir LINQ to Entities sorgusunda sonuçlarını bağlıyoruz.

Arka plan kod bizim denetiminin içinde şu şekilde desteklemiyoruz.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Ayrıca, bu önemli satır üst bizim denetimin biçimlendirme unutmayın.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

En popüler öğeleri, bir dakika için dakika başına değiştirme olmaz beri uygulamamız performansını artırmak için bir ağrıları getirir yönergesi ekleyebiliriz. Bu yönerge, önbelleğe alınan çıkış denetimi süresi dolduğunda, yalnızca yürütülecek denetimleri koda neden olur. Aksi takdirde, denetimin çıkışı önbelleğe alınmış sürümünü kullanılır.

Biz yapmanız gereken tek şey artık yeni denetimimiz Default.aspc sayfamızı içerir.

Kullanım sürükleyip varsayılan formumuzun Aç sütununda bir denetim örneği yerleştirmek için.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Şimdi uygulamamız giriş sayfasında ne zaman çalıştırıyoruz, en popüler öğeleri görüntüler.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "Ayrıca satın alınan" Denetim (parametrelerle kullanıcı denetimleri)

İkinci kullanıcı denetimi oluşturacağız müstehcen içerik belirginliğe ekleyerek bir sonraki düzeye satış olacaktır.

"Ayrıca satın alınan" üst öğeleri hesaplamak için mantık önemsiz değil.

"Ayrıca satın alınan" denetimimiz (önceden satın alınan) OrderDetails kayıtları için şu anda seçili ProductID seçin ve bulunan her bir benzersiz siparişi için OrderIDs alın.

Ardından al ürünleri tüm bu siparişleri ve miktarları satın alınan toplam seçiyoruz. Biz ürünleri miktar toplamı sıralamak ve ilk beş öğeleri görüntüleyebilirsiniz.

Bu mantık karmaşıklığını göz önünde bulundurulduğunda, biz bir saklı yordam bu algoritma uygular.

Saklı yordam için T-SQL aşağıdaki gibidir.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Biz bunu uygulamamız ve biz size, tablolar ve görünümler varlık veri modeli, ihtiyacımız, ek olarak belirtilen varlık veri modeli oluşturulan eklendiğinde bu saklı yordamı (SelectPurchasedWithProducts) veritabanında var olduğunu unutmayın. Bu saklı yordamı eklemeniz gerekir.

Varlık veri modeli saklı yordamı erişmek için biz işlevi aktarmanız gerekir.

Varlık veri modeli Tasarımcısı'nda açın ve Model tarayıcı açmak için Çözüm Gezgini'nde çift tıklayın, sonra tasarımcıda sağ tıklayın ve "İşlevi Al Ekle"'ı seçin.

![](tailspin-spyworks-part-7/_static/image1.png)

Bunun yapılması, bu iletişim kutusunu açar.

![](tailspin-spyworks-part-7/_static/image2.png)

Yukarıdaki "SelectPurchasedWithProducts" seçerek gördüğünüz gibi alanları doldurun ve yordam adı için sunduğumuz içeri aktarılan bir işlevin adını kullanın.

"Tamam" düğmesini tıklatın.

Bu size herhangi bir öğeyi model, olabileceği gibi biz karşı saklı yordamı yalnızca programlayabileceğiniz yapılır.

Bu nedenle bizim "Controls" klasöründe AlsoPurchased.ascx adlı yeni bir kullanıcı denetimi oluşturun.

Bu denetim için biçimlendirme PopularItems denetime çok tanıdık gelecektir.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

İşlenecek öğenin ürüne göre değişir olduğundan, çıktıyı önbelleğe değil önemli fark vardır.

ProductID denetlemek için "özelliği" olacaktır.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

Denetimin PreRender olayını işleyicisi şu üç şeyi yapmak için eed.

1. ProductID ayarlandığından emin olun.
2. Satın alınan geçerli ürünleriyle olup olmadığına bakın.
3. #2'de belirlendiği bazı öğeler çıktı.

Modeli aracılığıyla saklı yordam çağrısı ne kadar kolay olduğunu unutmayın.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Belirledikten sonra burada "Ayrıca satın alınan" biz yalnızca yineleyici sorgu tarafından döndürülen sonuçlar bağlayabilirsiniz.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

"Ayrıca satın alınan" tüm öğeler ise yalnızca diğer popüler öğeler bizim kataloğundan görüntüleyeceğiz.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

"Ayrıca satın alınan" öğeleri görüntülemek için ProductDetails.aspx sayfasını açın ve biçimlendirme içinde bu konumda görünmesi Çözüm Gezgini'nden AlsoPurchased denetimi sürükleyin.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Bunun yapılması tazelemek sayfanın en üstündeki denetimin bir başvuru oluşturur.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

ProductID birkaç AlsoPurchased kullanıcı denetimini gerektirdiğinden denetimimiz ProductID özelliğini bir sayfanın geçerli veri modeli öğesi Eval deyimini kullanarak ayarlayacağız.

![](tailspin-spyworks-part-7/_static/image3.png)

Oluşturmamızı ve şimdi çalıştırmak ve ürüne göz atın, "Ayrıca satın alınan" öğeleri görüyoruz.

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Önceki](tailspin-spyworks-part-6.md)
> [İleri](tailspin-spyworks-part-8.md)
