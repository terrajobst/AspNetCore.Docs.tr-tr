---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: DetailsView denetiminde TemplateField (C#) kullanma | Microsoft Docs
author: rick-anderson
description: GridView ile sunulan aynı TemplateField özellikleri ile DetailsView denetiminde de mevcuttur. Bu öğreticide bir ürün görüntüleyeceğiz...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a6467f6532ff72a82f4f416de2286377766ea264
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823968"
---
<a name="using-templatefields-in-the-detailsview-control-c"></a>DetailsView denetiminde TemplateField (C#) kullanma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe) veya [PDF olarak indirin](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> GridView ile sunulan aynı TemplateField özellikleri ile DetailsView denetiminde de mevcuttur. Bu öğreticide TemplateField içeren bir DetailsView kullanarak aynı anda bir ürün görüntüleyeceğiz.


## <a name="introduction"></a>Giriş

TemplateField daha yüksek bir işleme veri esneklik derecesini BoundField, CheckBoxField, HyperLinkField ve diğer veri alan denetimleri daha sunar. İçinde [önceki öğreticide](using-templatefields-in-the-gridview-control-cs.md) GridView'a TemplateField kullanma Aranan:

- Bir sütunda birden çok veri alan değerlerini görüntüler. Özellikle, her ikisi de `FirstName` ve `LastName` alanları bir GridView Sütun birleştirilmiş.
- Veri alanı değeri express için alternatif bir Web denetimi kullanın. Nasıl gösterileceğini gördüğümüz `HiredDate` takvim denetimini kullanarak değer.
- Temel alınan verileri temel alan durum bilgilerini gösterir. Sırada `Employees` tablo, bir çalışan işin oluştu gün sayısını döndüren bir sütun içermediğinden, ki TemplateField ve biçimlendirme yöntemi kullanarak önceki öğreticide GridView örnekte gibi bilgileri görüntüleyebilirsiniz.

GridView ile sunulan aynı TemplateField özellikleri ile DetailsView denetiminde de mevcuttur. Bu öğreticide iki TemplateField içeren bir DetailsView kullanarak aynı anda bir ürün görüntüleyeceğiz. İlk TemplateField birleştirecek `UnitPrice`, `UnitsInStock`, ve `UnitsOnOrder` bir DetailsView satır içine veri alanları. İkinci TemplateField değerini görüntüler `Discontinued` alan, ancak "Evet" durumunda görüntülemek için bir biçimlendirme yöntemi kullanacak `Discontinued` olduğu `true`ve "Hayır" Aksi takdirde.


[![İki TemplateField görüntüsünü özelleştirmek için kullanılır](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**Şekil 1**: iki TemplateField görüntüsünü özelleştirmek için kullanılır ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))


Haydi başlayalım!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>1. adım: DetailsView için veri bağlama

Genellikle yalnızca BoundFields içeren DetailsView denetiminde oluşturarak başlayın ve ardından yeni TemplateField eklemek veya mevcut BoundFields TemplateField dönüştürmek kolay TemplateField ile çalışırken önceki öğreticide açıklandığı gibi gerekli . Bu nedenle, bu sayfaya tasarımcı aracılığıyla bir DetailsView ekleyerek ve ürünlerin listesini döndüren bir ObjectDataSource bağlama başlangıç Öğreticisi. Bu adımları her ürünün Boole olmayan değer alanları ve bir Boole değeri alan (artık Üretilmiyor) için bir CheckBoxField BoundFields ile bir DetailsView oluşturur.

Açık `DetailsViewTemplateField.aspx` sayfa ve bir DetailsView tasarımcı araç kutusundan sürükleyin. Akıllı etiketi DetailsView'ın çağıran yeni bir ObjectDataSource denetimi eklemek seçin `ProductsBLL` sınıfın `GetProducts()` yöntemi.


[![GetProducts() yöntemini çağıran yeni ObjectDataSource denetim ekleme](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**Şekil 2**: yeni bir ObjectDataSource Denetimi, Invoke'lar Ekle `GetProducts()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))


Bu rapor için kaldırma `ProductID`, `SupplierID`, `CategoryID`, ve `ReorderLevel` BoundFields. Ardından, BoundFields yeniden sıralamak için `CategoryName` ve `SupplierName` BoundFields görünür hemen sonra `ProductName` BoundField. Ayarlama çekinmeyin `HeaderText` özellikleri ve biçimlendirme özellikleri BoundFields sizin için uygun gördüğünüz. Gibi GridView ile bu BoundField düzeyi düzenlemeler alanları iletişim kutusu (DetailsView'ın akıllı etiket alanları Düzenle bağlantısına tıklayarak erişilebilir) ya da bildirim temelli söz dizimi aracılığıyla gerçekleştirilebilir. Son olarak, DetailsView ait Temizle `Height` ve `Width` DetailsView izin vermek üzere özellik değerlerini görüntülenen veriler temel alınarak genişletmek için Denetim ve akıllı etiketinde sayfalama etkinleştir onay kutusunu işaretleyin.

Bu değişiklikleri yaptıktan sonra bildirim temelli biçimlendirme DetailsView denetiminizin aşağıdakine benzer görünmelidir:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

Bir tarayıcı aracılığıyla sayfasını görüntülemek için bir dakikanızı ayırın. Bu noktada listelenen tek bir ürün (Chai) ile ürün adı, kategori, tedarikçi, fiyat, stoktaki birimleri, sipariş birimlerde ve artık sağlanmayan durumunu gösteren satırları görmeniz gerekir.


[![Bir dizi BoundFields kullanarak ürün ayrıntıları gösterilir](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**Şekil 3**: Ürün uygulamasının Ayrıntılar, bir seri BoundFields kullanarak gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>2. adım: bir satıra fiyat, stoktaki birimleri ve sipariş birimlerde birleştirme

Bir satır için DetailsView sahip `UnitPrice`, `UnitsInStock`, ve `UnitsOnOrder` alanları. Biz bu veri alanları tek bir satıra bir TemplateField yeni TemplateField ekleyerek veya var olan birini dönüştürerek birleştirebilirsiniz `UnitPrice`, `UnitsInStock`, ve `UnitsOnOrder` içine bir TemplateField BoundFields. Kişisel mevcut BoundFields dönüştürme istiyorum, ancak yeni TemplateField ekleyerek uygulama yapalım.

Akıllı Etiket alanları iletişim kutusu çağrılırken DetailsView'ın alanları Düzenle bağlantısına tıklayarak başlayın. Ardından, yeni TemplateField ekleyip ayarlayın, `HeaderText` "Fiyat ve stok" ve onun yukarıda konumlandırılmış şekilde yeni TemplateField taşıma özelliğini `UnitPrice` BoundField.


[![Yeni bir TemplateField DetailsView denetimi ekleme](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**Şekil 4**: yeni TemplateField DetailsView denetimi ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))


Bu yeni TemplateField şu anda görüntülenen değerleri içerecek beri `UnitPrice`, `UnitsInStock`, ve `UnitsOnOrder` BoundFields, kaldıralım.

Tanımlamak için bu adım için son görevdir; `ItemTemplate` biçimlendirme olabilir Envanter TemplateField ve fiyat için denetimin bildirim temelli söz dizimi aracılığıyla arabirimi Tasarımcısı'nda veya el ile düzenleme DetailsView'ın şablon aracılığıyla gerçekleştirilir. Bir GridView olduğu gibi ile DetailsView'ın şablon düzenleme arabirimi akıllı etiketinde Şablonları Düzenle bağlantısına tıklayarak erişilebilir. Buradan aşağı açılan listeden düzenleyin ve ardından araç kutusundan herhangi bir Web denetim eklemek için şablonu seçebilirsiniz.

Bu öğretici için fiyat ve envanter TemplateField'ın bir etiket denetimi ekleyerek başlayın `ItemTemplate`. Ardından, akıllı etiket Web denetimin etiketinde veri bağlamaları Düzenle bağlantısına tıklayın ve bağlama `Text` özelliğini `UnitPrice` alan.


[![Etiketin metin özelliği UnitPrice veri alanına bağlama](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**Şekil 5**: etiketin bağlama `Text` özelliğini `UnitPrice` veri alanı ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Fiyat para birimi olarak biçimlendirme

Bu eklenmesiyle, etiket Web denetimi fiyat ve envanter TemplateField artık yalnızca seçili ürün için fiyat görüntülenir. Şekil 6 ilerlememizin ekran görüntüsü şimdiye kadarki bir tarayıcıdan görüntülendiğinde gösterir.


[![Fiyat ve envanter TemplateField fiyatı gösterir](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**Şekil 6**: Fiyat ve envanter TemplateField fiyatı gösterir ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))


Ürünün fiyatı bir para birimi olarak biçimlendirilmemiş unutmayın. Bir BoundField ile biçimlendirme ayarlayarak mümkündür `HtmlEncode` özelliğini `false` ve `DataFormatString` özelliğini `{0:formatSpecifier}`. Bir TemplateField için veri bağlama söz dizimi veya uygulamanın kodu (olduğu gibi ASP.NET sayfa arka plan kod sınıfı gibi) bir yerde tanımlanmış bir biçimlendirme yöntemi kullanarak tüm biçimlendirme yönergeleri ancak belirtilmelidir.

Kullanılan Web etiket denetiminde veri bağlama söz dizimi biçimlendirme belirtmek için etiketin akıllı etiketinde veri bağlamaları Düzenle bağlantısına tıklayarak DataBindings iletişim kutusuna dönmek. Biçimlendirme yönergeleri biçimi aşağı açılan listeden doğrudan girebilir veya tanımlı biçim dizeleri birini seçin. BoundField ile ın gibi `DataFormatString` özelliği, biçimlendirmeyi kullanarak belirtilen `{0:formatSpecifier}`.

İçin `UnitPrice` alan açılır listede uygun değer seçerek ya da yazarak belirtilen para birimi biçimlendirme kullanın `{0:C}` el ile.


[![Fiyat para birimi olarak Biçimlendir](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**Şekil 7**: fiyatı bir para birimi olarak Biçimlendir ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))


Bildirimli olarak, biçimlendirme belirtimi ikinci parametre olarak belirtilen `Bind` veya `Eval` yöntemleri. Bildirim temelli biçimlendirmede aşağıdaki veri bağlama ifadesinde Tasarımcısı sonuçlarından yaptığınız ayarları:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>TemplateField için kalan veri alanları ekleme

Bu noktada biz görüntülenen biçimlendirilmiş ve `UnitPrice` veri alanı Fiyat ve envanter TemplateField, ancak yine görüntüleme `UnitsInStock` ve `UnitsOnOrder` alanları. Şimdi bu bir satırda aşağıdaki fiyat ve parantez içindeki görüntü. Şablon içinde imleci konumlandırma ve yalnızca görüntülenecek metni yazarak gibi biçimlendirme Tasarımcısı'nda şablon düzenleme arabiriminden eklenebilir. Alternatif olarak, bu işaretleme, bildirim temelli söz diziminde doğrudan girilebilir.

Fiyat ve envanter bilgileri fiyat ve envanter TemplateField görüntüler. böylece static işaretleme, etiket Web denetimleri ve veri bağlama söz dizimi ekleme şu şekilde:

*UnitPrice*  
(**Stoktaki / sipariş:** *unitsInStock* / *SiparişBirimleri*)

Bu görev gerçekleştirdikten sonra DetailsView'ın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

Bu değişikliklerle fiyat ve envanter bilgileri tek bir DetailsView satır içine birleştirilmiştir.


[![Fiyat ve envanter bilgileri tek bir satır görüntülenir](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**Şekil 8**: Fiyat ve envanter bilgileri tek bir satır görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>3. adım: kullanımdan kaldırılan alan bilgilerini özelleştirme

`Products` Tablonun `Discontinued` ürün durdurmuştur olup olmadığını belirten bir bit değeri bir sütundur. Bir DetailsView (veya GridView) bir veri kaynak denetimine bağlanırken, Boole değeri alanlar ister `Discontinued`, Boole olmayan değer alanları, oysa CheckBoxFields uygulanan `ProductID`, `ProductName`ve benzeri olarak uygulanır BoundFields. CheckBoxField veri alanın değeri True ve unchecked yanlışsa, işaretli bir devre dışı onay kutusu işler.

CheckBoxField görüntülemek yerine biz ürün kesilir olup olmadığını yerine belirten metin görüntülemek isteyebilirsiniz. Bunu gerçekleştirmek için biz CheckBoxField DetailsView kaldırın ve ardından bir BoundField ekleyin, `DataField` özelliğinin ayarlandığı `Discontinued`. Bunu yapmak için bir dakikanızı ayırın. Bu değişiklikten sonra DetailsView metni "True" Artık üretilmeyen ürünler ve "False" için hala etkin olan ürünleri gösterir.


[![Dizeleri artık sağlanmayan durumunu görüntülemek için kullanılır True ve False](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**Şekil 9**: dizeleri True ve False kullanılan artık Üretilmiyor durumunu görüntülemek için ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))


"Evet" ve "Hayır", bunun yerine kullanılacak dizeleri "True" veya "False" istemedik düşünün. Bu tür özelleştirme ürettiği bir TemplateField ve bir biçimlendirme yöntemi ile gerçekleştirilebilir. Bir biçimlendirme yöntemi, herhangi bir sayıda giriş parametreleri alabilir, ancak şablonuna eklenecek HTML (bir dize olarak) döndürmesi gerekir.

Bir biçimlendirme yöntemi ekleyin `DetailsViewTemplateField.aspx` sayfa arka plan kod sınıf adlı `DisplayDiscontinuedAsYESorNO` Boole giriş parametresi olarak kabul eder ve bir dize döndürür. Bu yöntemin önceki öğreticide açıklandığı gibi *gerekir* olarak işaretlenmiş `protected` veya `public` şablondan erişilebilir olması için.


[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

Bu yöntem giriş parametresi denetler (`discontinued`) ve "YES" ise döndürür `true`, aksi halde "Hayır".

> [!NOTE]
> Biz içerebilecek bir veri alanına geçirme önceki öğretici geri çağırma içinde incelenirken biçimlendirme yönteminin `NULL` s ve bu nedenle olmadığını denetlemek gereken çalışanın `HiredDate` özellik değeri olan bir veritabanı `NULL` sonraki değer erişim `EmployeesRow`'s `HiredDate` özelliği. Bu tür bir denetimi bu yana burada gerekmiyor `Discontinued` sütunu hiçbir zaman veritabanı sahip `NULL` atanan değerler. Ayrıca, bu nedenle, yöntem bir Boole parametresi yerine kabul etmek giriş kabul edebilir, bir `ProductsRow` örneği veya türünde bir parametre `object`.


Bu biçimlendirme yöntemi ile tam kalan tek şey TemplateField 's çağırmaya `ItemTemplate`. TemplateField kaldırabilir ya da oluşturmak için `Discontinued` BoundField yeni TemplateField eklemek veya dönüştürmek `Discontinued` içine bir TemplateField BoundField. Ardından, bildirim temelli biçimlendirme görünümünden TemplateField Düzenle çağıran bir ItemTemplate içeren `DisplayDiscontinuedAsYESorNO` yöntemi geçerli değerini geçirir, `ProductRow` örneğinin `Discontinued` özelliği. Bu, aracılığıyla erişilebilir `Eval` yöntemi. Özellikle, TemplateField'ın biçimlendirme gibi görünmelidir:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

Bu neden `DisplayDiscontinuedAsYESorNO` DetailsView işlenirken çağrılacak yöntem tümleştirilmesidir `ProductRow` örneğinin `Discontinued` değeri. Bu yana `Eval` türünde bir değer döndürür `object`, ancak `DisplayDiscontinuedAsYESorNO` yöntemi türünde bir giriş parametresi bekliyor `bool`, biz cast `Eval` yöntemleri dönüş değeri için `bool`. `DisplayDiscontinuedAsYESorNO` Yöntemi ardından döndürür "Evet" veya "Hayır" değere göre alır. Döndürülen değerdir bu DetailsView görüntülenen satır (bkz. Şekil 10).


[![Evet veya Hayır değerler artık Üretilmiyor sıradaki artık gösterilmektedir:](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**Şekil 10**: Evet veya Hayır değerler artık gösterilen artık Üretilmiyor satırında ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))


## <a name="summary"></a>Özet

DetailsView denetiminde TemplateField daha ileri düzeyde bir değerinden başka bir alan denetimlerle birlikte kullanılabilir ve durumlar için ideal olan verileri görüntüleme esneklik sağlar burada:

- Birden çok veri alanları bir GridView sütunu görüntülenmesi gerekir
- Veri en iyi şekilde düz metin yerine bir Web denetimi kullanılarak ifade edilir
- Temel alınan verileri, meta veri görüntüleme gibi veya verileri yeniden biçimlendirme çıkış bağlıdır

Büyük ölçüde esneklik DetailsView'ın temel alınan verilerin işlenmesi için TemplateField olanak tanırken, her bir alan bir HTML bir satır olarak işlenen DetailsView çıktı hala bir bit boxy hissettirir `<table>`.

FormView denetimi, yüksek düzeyde işlenen çıkışı yapılandırma esneklik sunar. FormView alanları ancak bunun yerine yalnızca bir dizi şablon içermiyor (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`, vb.). Sonraki müşterilerimize öğreticide işlenmiş düzenini daha fazla denetim elde etmenizi FormView kullanmayı göreceğiz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Dan Jagers oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](using-templatefields-in-the-gridview-control-cs.md)
> [İleri](using-the-formview-s-templates-cs.md)
