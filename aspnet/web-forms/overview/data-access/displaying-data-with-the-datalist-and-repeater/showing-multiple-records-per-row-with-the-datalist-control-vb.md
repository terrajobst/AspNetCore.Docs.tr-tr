---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: "DataList denetimi (VB) ile satır başına birden çok kayıt gösteren | Microsoft Docs"
author: rick-anderson
description: "Bu kısa öğreticide biz DataList'ın düzeni RepeatColumns ve RepeatDirection özelliklerini aracılığıyla özelleştirmek nasıl ele alacağız."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 416178533f022f2a262799e6f042d6009bb9d999
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>DataList denetimi (VB) ile satır başına birden çok kaydı gösteriliyor
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) veya [PDF indirin](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> Bu kısa öğreticide biz DataList'ın düzeni RepeatColumns ve RepeatDirection özelliklerini aracılığıyla özelleştirmek nasıl ele alacağız.


## <a name="introduction"></a>Giriş

DataList örnekler biz ve son iki eğitimlerine görülen işlenen her kayıt veri kaynağından bir satır tek sütunlu HTML olarak `<table>`. Bu varsayılan DataList davranışı olmakla birlikte, veri kaynağı öğesi birden çok sütun, çok satırlı bir tablo arasında yayılır şekilde DataList görüntüsünü özelleştirmek çok daha kolaydır. Ayrıca, bu s tüm verileri olma olasılığı kaynak öğeler tek satır, birden çok sütun DataList içinde görüntülenir.

Biz DataList s düzeni aracılığıyla özelleştirebilirsiniz kendi `RepeatColumns` ve `RepeatDirection` sırasıyla kaç sütun işlenir ve olup öğelerden dikey veya yatay olarak düzenlenmiştir belirtmek olan özellikler. Şekil 1'de, örneğin, üç sütunları olan bir tabloda ürün bilgilerini görüntüleyen bir DataList gösterir.


[![Satır başına üç ürünlerini DataList gösterir](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Şekil 1**: DataList gösterir üç ürünleri satır başına ([tam boyutlu görüntüyü görüntülemek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


Satır başına birden çok veri kaynağı öğe göstererek DataList yatay ekran alanı daha verimli şekilde kullanabilir. Bu kısa öğreticide Biz bu iki DataList özellik ele alacağız.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>1. adım: Bir DataList ürün bilgilerini görüntüleme

İnceleyeceğiz önce `RepeatColumns` ve `RepeatDirection` özellikleri, let s ilk oluşturma DataList sayfamızı üzerinde standart tek sütunlu, çok satırlı tablo düzenini kullanarak ürün bilgilerini listeler. Bu örnekte, ürün adı, kategori ve aşağıdaki biçimlendirme kullanarak fiyat görüntülemek s sağlar:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Biz ve bu adımları uygularken size hızla taşırsınız şekilde verileri bir DataList önceki örneklerde nasıl bağlanacağını görülür. Başlangıç açarak `RepeatColumnAndDirection.aspx` sayfasındaki `DataListRepeaterBasics` klasörü ve araç tasarımcıya DataList sürükleyin. Yeni ObjectDataSource oluşturun ve bunu kendi verisinden çıkarmak için yapılandırmak üzere DataList s akıllı etiketten opt `ProductsBLL` s sınıfı `GetProducts` (hiçbiri) seçme yöntemini seçeneği s INSERT, UPDATE, sihirbazdan ve silme sekmeleri.

Visual Studio oluşturma ve yeni ObjectDataSource DataList bağlama sonra otomatik olarak oluşturacak bir `ItemTemplate` , ad ve değer ürün veri alanların her biri için görüntüler. Ayarlama `ItemTemplate` , değiştirme, yukarıda gösterilen biçimlendirme kullanmayacağından bildirim temelli biçimlendirme aracılığıyla doğrudan veya Şablonları Düzenle DataList s akıllı etiketinde seçeneği *ürün adı*, *kategori adı* , ve *fiyat* metin değerleri atamak için uygun bağlama söz dizimini kullanın etiket denetimleri ile bunların `Text` özellikleri. Güncelleştirdikten sonra `ItemTemplate`, sayfa s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Bildirim girdiğim ve bir biçim belirticisi dahil `Eval` databinding söz diziminin `UnitPrice`, döndürülen değer bir para birimi - olarak biçimlendirme`Eval("UnitPrice", "{0:C}").`

Bir tarayıcıda sayfanızı ziyaret etmek için bir dakikanızı ayırın. Şekil 2'de görüldüğü gibi DataList ürünlerin tek sütunlu, çok satırlı tablo olarak işler.


[![Varsayılan olarak, tek bir sütun, çok satırlı bir tablo olarak DataList işler](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Şekil 2**: varsayılan olarak, bir tek sütunlu, çok satırlı bir tablo olarak işleyen DataList ([tam boyutlu görüntüyü görüntülemek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>2. adım: DataList s Düzen yönünü değiştirme

DataList öğelerinden tek sütunlu, çok satırlı bir tablo dikey olarak yerleştirme için varsayılan davranışı sırasında bu davranış kolayca s DataList değiştirilebilir [ `RepeatDirection` özelliği](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). `RepeatDirection` Özelliği iki olası değerlerden biri kabul edebilir: `Horizontal` veya `Vertical` (varsayılan).

Değiştirerek `RepeatDirection` özelliğinden `Vertical` için `Horizontal`, veri kaynağı öğesi başına bir sütunu oluşturma DataList kayıtlarını tek bir satır içinde işler. Bu etkiyi göstermeye DataList Tasarımcısı'nda tıklatın ve sonra Özellikler penceresinde değiştirme `RepeatDirection` özelliğinden `Vertical` için `Horiztonal`. Hemen Bunu yaptıktan sonra Tasarımcı DataList s düzeni tek satır, birden çok sütun bir arabirim oluşturma ayarlar (bkz: Şekil 3).


[![RepeatDirection özelliği belirleyen nasıl yönü DataList s yerleştirilmiş çıkışı öğeler](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Şekil 3**: `RepeatDirection` özelliği belirleyen nasıl yönü DataList s yerleştirilmiş çıkışı öğeleridir ([tam boyutlu görüntüyü görüntülemek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


Küçük miktarda veri, bir tek satır, görüntülenirken çok sütunlu bir tablo ekran Gayrimenkul en üst düzeye çıkarmak için ideal bir yöntem olabilir. Büyük miktarda veriyi için ancak, tek bir satır hangi iter, bu öğeler sağa kapalı ekrana sığar bu can t çok sayıda sütun gerektirir. Şekil 4'te bir tek satır DataList işlendiğinde ürünlerini gösterir. Birçok ürün (üzerinde 80) olduğundan, kullanıcının şu ana kadar ürünlerinin her biri hakkında bilgileri görüntülemek için sağa kaydırma gerekecektir.


[![Yeterli büyüklükte veri kaynakları için bir tek sütun DataList yatay kaydırma gerektirir](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Şekil 4**: için yeterince büyük veri kaynakları, bir tek sütun DataList olacak gerektiren yatay kaydırma ([tam boyutlu görüntüyü görüntülemek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Adım 3: Çok sütunlu, çok satırlı bir tablo verilerini görüntüleme

Birden çok sütun, birden fazla satır DataList oluşturmak için ayarlamak ihtiyacımız [ `RepeatColumns` özelliği](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) görüntülenecek sütunlar sayısı. Varsayılan olarak, `RepeatColumns` özelliği tek bir satır veya bir sütundaki tüm öğeleri görüntülemek DataList neden olacak 0 olarak ayarlanır (değerine bağlı olarak `RepeatDirection` özelliği).

Bizim örneğimizde, tablo satır başına üç ürünleri görüntüler s olanak tanır. Bu nedenle, `RepeatColumns` 3 özelliği. Bu değişikliği yaptıktan sonra sonuçları bir tarayıcıda görüntülemek için bir dakikanızı ayırın. Şekil 5 gösterildiği gibi ürünler artık üç sütun, çok satırlı bir tablo listelenir.


[![Satır üç ürünleri görüntülenir](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Şekil 5**: üç ürünleri, satır görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


`RepeatDirection` Özelliği etkiler DataList öğeleri nasıl düzenlendiği. Şekil 5 gösterir sonuçlarıyla `RepeatDirection` özelliğini `Horizontal`. İlk üç ürünleri Chai, izleme ve Aniseed Syrup soldan sağa doğru üstten alta düzenlenmiştir unutmayın. İlk üç altında bir satır (Chef Anton s ile Cajun Seasoning başlayarak) sonraki üç ürünleri görüntülenir. Değiştirme `RepeatDirection` özelliğini yeniden `Vertical`, ancak, bu ürünler üstten alta doğru çıkışı yerleştirir, soldan sağa, Şekil 6 gösterildiği gibi.


[![Burada, Ürün çıkışı dikey olarak düzenlenir değil](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Şekil 6**: Burada, Ürün çıkışı dikey olarak düzenlenir değil ([tam boyutlu görüntüyü görüntülemek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


DataList bağlı toplam kayıt sayısı sonuçta ortaya çıkan tabloda görüntülenen satır sayısını bağlıdır. Tam olarak, bu veri kaynağı öğelerini toplam sayısı üst sınıra bölünmüş tarafından s `RepeatColumns` özellik değeri. Bu yana `Products` tablo şu anda 3 ile tam bölünebilir olduğu 84 ürünleri yok, 28 satır vardır. Varsa veri kaynağındaki öğelerin sayısı ve `RepeatColumns` özellik değeri tam bölünebilir olmayan sonra son satır veya sütun boş hücreler sahip olur. Varsa `RepeatDirection` ayarlanır `Vertical`, son sütunu boş hücrelerin; olacaktır sonra `RepeatDirection` olan `Horizontal`, son satırını boş hücrelerin sahip olacaktır.

## <a name="summary"></a>Özet

DataList varsayılan olarak, tek bir TemplateField ile GridView düzenini taklit eden bir tek sütunlu, birden fazla satır tabloda kendi öğeleri listeler. Bu varsayılan düzen kabul edilebilir olmakla birlikte, biz ekran Gayrimenkul satır başına birden çok veri kaynağı öğelerini görüntüleyerek en üst düzeye çıkarabilirsiniz. Bunu gerçekleştirmenin olan s DataList ayarlama basitçe kurabilirsiniz `RepeatColumns` satır görüntülenecek sütun sayısını özelliğine. Ayrıca, DataList s `RepeatDirection` özelliği, birden çok sütun, birden fazla satır tablosunun içeriğini yatay olarak soldan sağa üstten alta veya dikey olarak üstten alta soldan sağa düzenleneceğini olup olmadığını belirtmek için kullanılabilir.

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme John Suru oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
[sonraki](nested-data-web-controls-vb.md)
