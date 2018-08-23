---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
title: DataList denetimi (C#) ile satır başına birden çok kayıt gösterme | Microsoft Docs
author: rick-anderson
description: Bu kısa öğreticide DataList'in düzeni RepeatColumns ve RepeatDirection özelliklerini kullanarak özelleştirmek nasıl şunları keşfedeceğiz.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: cf5acaf5-d4f6-4957-badc-b89956b285f3
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f79c446a0c9407309ab65cd993df544e883afb22
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754428"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-c"></a>DataList denetimi (C#) ile satır başına birden çok kayıt gösterme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_CS.exe) veya [PDF olarak indirin](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/datatutorial31cs1.pdf)

> Bu kısa öğreticide DataList'in düzeni RepeatColumns ve RepeatDirection özelliklerini kullanarak özelleştirmek nasıl şunları keşfedeceğiz.


## <a name="introduction"></a>Giriş

DataList örnekler ediyoruz ve son iki öğreticilerde görülen işlenen her kaydın veri kaynağından bir satır tek sütunlu HTML olarak `<table>`. Bu davranışı DataList olsa da, veri kaynağı öğesi çok sütunlu, çok satırlı bir tablo arasında yayılır şekilde DataList görüntüsünü özelleştirmek çok daha kolaydır. Ayrıca, bu kaynak tek satır, çok sütunlu bir DataList içinde görüntülenen öğelerin s tüm verileri bulunabilir.

Biz aracılığıyla DataList s düzenini özelleştirebilirsiniz, `RepeatColumns` ve `RepeatDirection` sırasıyla olup olmadığını öğeleri yatay veya dikey olarak yerleştirilir ve kaç sütun işlendiğini belirtmek, özellikleri. Şekil 1'de, örneğin, üç sütun içeren bir tablo ürün bilgileri görüntüleyen bir DataList gösterir.


[![DataList satır başına üç ürün gösterecek](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image1.png)

**Şekil 1**: DataList gösteren üç ürün her satır ([tam boyutlu görüntüyü görmek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image3.png))


DataList, satır başına birden çok veri kaynağı öğesi göstererek daha etkili bir şekilde yatay ekran alanını kullanabilir. Bu kısa öğreticide Biz bu iki DataList özellikleri hakkında bilgi edineceksiniz.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>1. adım: Bir DataList ürün bilgilerini görüntüleme

İnceleyeceğiz önce `RepeatColumns` ve `RepeatDirection` özellikler, let s ilk oluşturmak bir DataList sayfamızı üzerinde standart tek sütunlu, çok satırlı Tablo düzeni kullanarak ürün bilgilerini listeler. Bu örnekte, ürün adı, kategori ve aşağıdaki biçimlendirme kullanarak fiyatını görüntüle s sağlar:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample1.html)]

Biz ve bu adımlara hızlı bir şekilde taşırsınız şekilde önceki örneklerde, bir DataList veri bağlamak nasıl görülen. Başlangıç açarak `RepeatColumnAndDirection.aspx` sayfasını `DataListRepeaterBasics` klasörü ve DataList tasarımcı araç kutusundan sürükleyin. DataList s akıllı etiketten yeni ObjectDataSource oluşturmak ve bunu kendi verileri çekmek için yapılandırmak için iyileştirilmiş `ProductsBLL` s sınıfı `GetProducts` (hiçbiri) seçme yöntemi seçeneği s INSERT, UPDATE, sihirbazdan ve sekmeleri SİLİN.

Visual Studio oluşturma ve DataList için yeni ObjectDataSource bağlama sonra otomatik olarak oluşturur bir `ItemTemplate` ürün veri alanların her biri için ad ve değer görüntüleyen. Ayarlama `ItemTemplate` değiştirerek, yukarıda gösterilen biçimlendirme kullanmasını sağlayacak şekilde DataList s akıllı etiketinde bildirim temelli biçimlendirme aracılığıyla doğrudan veya Şablonları Düzenle seçeneğini *ürün adı*, *kategori adı* , ve *fiyat* metin değerlerine atama için uygun bağlama söz dizimini kullanın etiket denetimleri ile kendi `Text` özellikleri. Güncelleştirdikten sonra `ItemTemplate`, sayfa s, bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample2.aspx)]

Bildirim girdiğim ve bir biçim belirtici olarak dahil `Eval` için veri bağlama söz dizimi `UnitPrice`, döndürülen değer - para birimi olarak biçimlendirme `Eval("UnitPrice", "{0:C}").`

Bir tarayıcı sayfanızı ziyaret etmek için bir dakikamızı ayıralım. Şekil 2 gösterildiği gibi DataList ürünlerin tek sütunlu, çok satırlı tablo olarak işler.


[![Varsayılan olarak, tek sütunlu, çok satırlı bir tablo olarak DataList oluşturur](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image4.png)

**Şekil 2**: varsayılan olarak, tek sütunlu, çok satırlı bir tablo olarak işler DataList ([tam boyutlu görüntüyü görmek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>2. adım: DataList s Düzen yönünü değiştirme

DataList öğelerinden dikey tek sütunlu, çok satırlı bir tablo olarak düzenlemek için varsayılan davranışı sırasında bu davranışı kolayca s DataList değiştirilebilir [ `RepeatDirection` özelliği](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). `RepeatDirection` Özelliği olası iki değerden birini kabul edebilir: `Horizontal` veya `Vertical` (varsayılan).

Değiştirerek `RepeatDirection` özelliğinden `Vertical` için `Horizontal`, veri kaynağı başına bir sütun oluşturma DataList kayıtlarını tek bir satır içinde işler. Bu etkiyi göstermek için tasarımcıda DataList tıklayın ve ardından, Özellikler penceresinden değiştirme `RepeatDirection` özelliğinden `Vertical` için `Horiztonal`. Hemen Bunu yaptıktan sonra Tasarımcı DataList s düzeni tek satır, çok sütunlu bir arabirim oluşturma ayarlar (bkz: Şekil 3).


[![RepeatDirection özelliği belirleyen nasıl yönü DataList s öğeleri açıklanmıştır kullanıma sunulduğunda](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image7.png)

**Şekil 3**: `RepeatDirection` özelliği yönü DataList s öğeleri açıklanmıştır kullanıma şeklini belirler ([tam boyutlu görüntüyü görmek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image9.png))


Küçük miktarlarda veri, bir tek satır görüntülerken, çok sütunlu tabloyu ekran gerçek boyutunuzu maksimuma çıkarmak için ideal bir yöntem olabilir. Daha büyük veri hacimleri için ancak tek bir satır hangi bildirim, bu öğeler ekranda kapalı sağa uyacak bu can t, çok sayıda sütun gerektirir. Şekil 4 tek satır DataList işlendiğinde ürünleri gösterir. Pek çok ürünlerin (üzerinde 80) olduğundan, kullanıcının her ürün hakkındaki bilgileri görüntülemek için sağda kaydırarak gerekecektir.


[![Yeterince büyük veri kaynakları için bir tek sütun DataList yatay kaydırma gerektirir](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image10.png)

**Şekil 4**: için yeterince büyük veri kaynakları, bir tek sütun DataList olacak gerektiren yatay kaydırma ([tam boyutlu görüntüyü görmek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>3. adım: Verileri birden çok sütun, çok satırlı bir tablo görüntüleme

Çok sütunlu, çok satırlı bir DataList oluşturmak için ayarlanacak ihtiyacımız [ `RepeatColumns` özelliği](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) görüntülenecek sütun sayısı. Varsayılan olarak, `RepeatColumns` özelliği, tüm öğeleri tek bir satır veya sütun görüntülenecek DataList neden 0 olarak ayarlanır (değerine göre `RepeatDirection` özelliği).

Bizim örneğimizde, tablo satırının başına üç ürünleri görüntülemek s olanak tanır. Bu nedenle `RepeatColumns` özelliğini 3. Bu değişikliği yaptıktan sonra sonuçları bir tarayıcıda görüntülemek için bir dakikanızı ayırın. Şekil 5 gösterildiği gibi ürünler artık üç sütunlu, çok satırlı bir tablo listelenir.


[![Satır üç ürünleri görüntülenir](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image13.png)

**Şekil 5**: üç ürünler, her satırı görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image15.png))


`RepeatDirection` Özellik DataList'te öğeleri nasıl düzenlendiği etkiler. Şekil 5 gösterir sonuçlarıyla `RepeatDirection` özelliğini `Horizontal`. İlk üç ürünleri Chai Chang ve Aniseed Syrup soldan sağa doğru yukarıdan düzenlenmiştir unutmayın. İlk üç altında bir satır (Chef Acı s ile Cajun Seasoning başlayarak) sonraki üç ürünleri görüntülenir. Değiştirme `RepeatDirection` özelliğini yeniden `Vertical`, ancak bu ürünleri üstten alta doğru kullanıma yerleştirir, soldan sağa, Şekil 6 gösterildiği gibi.


[![Burada, ürünleri kullanıma dikey olarak düzenlenir olan](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image16.png)

**Şekil 6**: Burada, ürünleri kullanıma dikey olarak düzenlenir olan ([tam boyutlu görüntüyü görmek için tıklatın](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image18.png))


DataList için bağlı toplam kayıt sayısı sonuçta elde edilen tabloda görüntülenen satırların sayısını bağlıdır. Tam olarak, bu veri kaynağı öğelerini toplam sayısının tavan bölünmüş tarafından s `RepeatColumns` özellik değeri. Bu yana `Products` tablo şu anda 3 ile bölünebilir olduğu 84 ürünleri yok, 28 satır vardır. Veri kaynağındaki öğelerin sayısı ve `RepeatColumns` özellik değeri, katı olmayan ve son satır veya sütun boş hücreler sahip olur. Varsa `RepeatDirection` ayarlanır `Vertical`, son sütun, boş hücreler; olacaktır `RepeatDirection` olduğu `Horizontal`, son satır boş hücrelerin sahip olacaktır.

## <a name="summary"></a>Özet

DataList varsayılan olarak, öğeleri tek bir TemplateField ile GridView düzenini taklit eden bir tek sütunlu, çok satırlı tabloda listeler. Bu varsayılan düzen kabul edilebilir olmakla birlikte, biz ekran gerçek boyutunuzu satır başına birden çok veri kaynağı öğesi görüntüleyerek en üst düzeye çıkarabilirsiniz. Bunu gerçekleştirmenin olan s DataList ayarlamanın tek gereken bunların `RepeatColumns` satır görüntülenecek sütun sayısını özelliği. Ayrıca, s DataList `RepeatDirection` özelliği, çok sütunlu, çok satırlı tablonun içeriğini yatay olarak soldan sağa, yukarıdan aşağıya veya soldan sağa, yukarıdan dikey düzenleneceğini olup olmadığını belirtmek için kullanılabilir.

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme John Suru oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
> [İleri](nested-data-web-controls-cs.md)
