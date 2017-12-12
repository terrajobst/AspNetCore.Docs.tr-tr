---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: TemplateFields DetailsView denetimini (VB) kullanarak | Microsoft Docs
author: rick-anderson
description: "GridView ile kullanılabilen aynı TemplateFields özellikleri de DetailsView denetimi ile kullanılabilir. Bu öğreticide biz bir ürün görüntülersiniz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b368a651253a569865bb92fa93d3462f88d8935f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>TemplateFields DetailsView denetiminde (VB) kullanma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) veya [PDF indirin](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> GridView ile kullanılabilen aynı TemplateFields özellikleri de DetailsView denetimi ile kullanılabilir. Bu öğreticide biz TemplateFields içeren bir DetailsView kullanarak aynı anda bir ürün görüntülersiniz.


## <a name="introduction"></a>Giriş

TemplateField BoundField, CheckBoxField, HyperLinkField ve diğer veri alan denetimleri daha yüksek bir dereceye verilerini işleme esneklik sunar. İçinde [önceki öğretici](using-templatefields-in-the-gridview-control-vb.md) için GridView içinde TemplateField kullanarak Aranan:

- Bir sütunda birden çok veri alan değerlerini görüntüler. Özellikle, her ikisi de `FirstName` ve `LastName` alanları tek bir GridView sütunda birleştirilmiş.
- Bir veri alanı değeri ifade etmek için alternatif bir Web denetimi kullanın. Nasıl gösterileceğini gördüğümüz `HiredDate` bir Takvim denetimi kullanarak değer.
- Temel alınan verileri temel alan durum bilgilerini gösterir. Sırada `Employees` tablo bir çalışan işi süredir gün sayısını döndürür bir sütun içermiyor, TemplateField ve biçimlendirme yöntemini kullanarak önceki öğreticideki GridView örnekte gibi bilgileri görüntülemek bulduk.

GridView ile kullanılabilen aynı TemplateFields özellikleri de DetailsView denetimi ile kullanılabilir. Bu öğreticide şu iki TemplateFields içeren bir DetailsView kullanarak aynı anda bir ürün görüntülersiniz. İlk TemplateField birleştirir `UnitPrice`, `UnitsInStock`, ve `UnitsOnOrder` bir DetailsView satıra veri alanları. İkinci TemplateField değerini görüntüler `Discontinued` alan, ancak bir biçimlendirme yöntemi "Evet" görüntülenmesi için kullanacağı `Discontinued` olan `True`ve "Hayır" Aksi takdirde.


[![İki TemplateFields görüntüsünü özelleştirmek için kullanılır](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Şekil 1**: iki TemplateFields görüntüsünü özelleştirmek için kullanılır ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))


Haydi başlayalım!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>1. adım: DetailsView'a veri bağlama

Yalnızca BoundFields içeren DetailsView denetimi oluşturarak başlayın ve ardından yeni TemplateFields eklemek veya mevcut BoundFields TemplateFields dönüştürmek genellikle kolayıdır TemplateFields ile çalışırken, önceki öğreticide anlatıldığı gibi gerekli . Bu nedenle, bu sayfanın Tasarımcısı aracılığıyla bir DetailsView ekleme ve ürünlerin listesini döndürür bir ObjectDataSource bağlayarak başlangıç Öğreticisi. Bu adımları bir DetailsView her ürünün Boole olmayan değer alanları ve bir Boolean değeri alan (Üretimi Durdurulmuş) için bir CheckBoxField BoundFields ile oluşturur.

Açık `DetailsViewTemplateField.aspx` sayfasında ve bir DetailsView tasarımcıya araç çubuğuna sürükleyin. Çağıran yeni bir ObjectDataSource denetimi eklemek DetailsView'un akıllı etiketten seçin `ProductsBLL` sınıfının `GetProducts()` yöntemi.


[![Yeni ObjectDataSource GetProducts() yöntemini çağıran bir denetim ekleme](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Şekil 2**: yeni bir ObjectDataSource Denetimi o başlatır eklemek `GetProducts()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))


Bu rapor için kaldırma `ProductID`, `SupplierID`, `CategoryID`, ve `ReorderLevel` BoundFields. Ardından, BoundFields yeniden sıralamak için `CategoryName` ve `SupplierName` BoundFields görünür hemen sonra `ProductName` BoundField. Ayarlanacak çekinmeyin `HeaderText` özellikleri ve yazarken BoundFields biçimlendirme özelliklerini uygun gördüğünüz. Gibi GridView ile bu BoundField düzeyi düzenlemeler alanları iletişim kutusu (DetailsView'un akıllı etiket alanları Düzenle bağlantıya tıklayarak erişilebilir) veya bildirim temelli söz dizimi aracılığıyla gerçekleştirilebilir. Son olarak, DetailsView'un temizleyin `Height` ve `Width` DetailsView izin vermek üzere özellik değerlerini görüntülenen verileri tabanlı genişletmek için denetlemek ve akıllı etiket içinde etkinleştirmek disk belleği onay kutusunu işaretleyin.

Bu değişiklikleri yaptıktan sonra DetailsView denetiminizin bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Bir tarayıcı aracılığıyla sayfasını görüntülemek için bir dakikanızı ayırın. Bu noktada ürün adı, kategori, tedarikçi, fiyat, stok, sipariş birimlerde ve devam etmeyen durumunu gösteren satırlarla listelenen tek bir ürün (Chai) görmeniz gerekir.


[![Ürün Ayrıntıları BoundFields bir dizi kullanarak gösterilir](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Şekil 3**: ürünün Ayrıntılar, bir seri BoundFields kullanarak gösterilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>2. adım: bir satıra fiyat, stok ve sipariş birimlerde birleştirme

DetailsView için bir satır var `UnitPrice`, `UnitsInStock`, ve `UnitsOnOrder` alanları. Biz bu veri alanları tek bir satıra TemplateField ile yeni bir TemplateField ekleyerek veya var olan bir dönüştürme birleştirebilirsiniz `UnitPrice`, `UnitsInStock`, ve `UnitsOnOrder` bir TemplateField içine BoundFields. Kişisel varolan BoundFields dönüştürme tercih ediyorum olsa da, yeni TemplateField ekleyerek uygulama yapalım.

Alanları iletişim kutusu çağrılırken DetailsView'un akıllı etiket alanları Düzenle bağlantıya tıklayarak başlatın. Ardından, yeni TemplateField ekleyin ve ayarlayın, `HeaderText` özelliğini "Fiyat ve stok" ve taşıma onun yukarıda konumlandırılmış şekilde yeni TemplateField `UnitPrice` BoundField.


[![Yeni bir TemplateField denetimini ekleme](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Şekil 4**: yeni TemplateField denetimini ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))


Bu yeni TemplateField şu anda görüntülenen değerler içerecek beri `UnitPrice`, `UnitsInStock`, ve `UnitsOnOrder` BoundFields, şimdi bunları kaldırın.

Bu adım için son görev tanımlamaktır `ItemTemplate` fiyat ve olabilir stok TemplateField için biçimlendirme DetailsView'un şablon denetimin bildirim temelli söz dizimi arabirimi Tasarımcı veya el ile düzenleme aracılığıyla gerçekleştirilir. GridView olduğu gibi ile DetailsView'un şablon düzenleme arabirimi akıllı etiket Şablonları Düzenle bağlantıya tıklayarak erişilebilir. Buradan, aşağı açılan listeden düzenlemek ve Web denetimleri Araç Kutusu'ndan eklemek için şablonu seçebilirsiniz.

Bu öğretici için fiyat ve stok TemplateField'ın bir etiket denetimi ekleyerek başlayın `ItemTemplate`. Ardından, etiket Web denetimin akıllı etiket veri bağlamaları Düzenle bağlantısından tıklayın ve bağlama `Text` özelliğine `UnitPrice` alan.


[![Etiketin metin özelliğini UnitPrice veri alanına bağlayın](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Şekil 5**: etiketin bağlamak `Text` özelliğine `UnitPrice` veri alanı ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Para birimi olarak fiyat biçimlendirme

Bu eklenmesiyle etiket Web denetimi fiyat ve stok TemplateField artık yalnızca seçilen ürün fiyatını görüntüler. Şekil 6 bizim ilerleme ekran görüntüsü bugüne kadarki bir tarayıcıdan görüntülendiğinde gösterir.


[![Fiyat ve stok TemplateField fiyat gösterir](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Şekil 6**: Fiyat ve stok TemplateField fiyat gösterir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))


Ürünün fiyat para birimi olarak biçimlendirilmemiş unutmayın. Bir BoundField ile biçimlendirme ayarlayarak mümkündür `HtmlEncode` özelliğine `False` ve `DataFormatString` özelliğine `{0:formatSpecifier}`. Bir TemplateField, ancak tüm biçimlendirme yönergeleri databinding sözdiziminde veya herhangi bir yerde (ASP.NET sayfa arka plandaki kod sınıfı olduğu gibi) uygulamanın kodunu içinde tanımlanan bir biçimlendirme yöntemi kullanılarak belirtilmesi gerekir.

Etiket Web denetiminde kullanılan databinding söz dizimi biçimlendirme belirtmek için etiketin akıllı etiket veri bağlamaları Düzenle bağlantısından tıklayarak veri bağlamaları iletişim kutusuna dönmek. Biçimlendirme yönergeleri doğrudan biçimi aşağı açılan listesinde yazın veya tanımlı biçim dizeleri birini seçin. BoundField ile 's gibi `DataFormatString` özelliği, biçimlendirmeyi kullanarak belirtilen `{0:formatSpecifier}`.

İçin `UnitPrice` alan para birimi biçimlendirme belirtilen uygun aşağı açılan liste değeri seçerek veya yazarak kullanım `{0:C}` el ile.


[![Para birimi olarak fiyat biçimlendirme](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Şekil 7**: Fiyat bir para birimi olarak biçimlendirmek ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))


İkinci bir parametresi olarak biçimlendirme belirtimi bildirimli olarak, belirtilen `Bind` veya `Eval` yöntemleri. Bildirim temelli biçimlendirmede aşağıdaki databinding ifadesinde Tasarımcısı sonucundan yaptığınız ayarları:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>TemplateField kalan veri alanları ekleme

Bu noktada biz görüntülenen biçimlendirilmiş ve yapılandırdığınız `UnitPrice` veri alan Fiyat ve stok TemplateField, ancak yine de görüntülenecek gerekir `UnitsInStock` ve `UnitsOnOrder` alanları. Şimdi bu fiyat aşağıda ve parantez içinde satırındaki görüntü. Şablon düzenleme arabirimden Tasarımcısı'nda, bu tür biçimlendirme imlecinizi şablonu içindeki konumlandırma ve yalnızca görüntülenecek metni yazarak eklenebilir. Alternatif olarak, bu biçimlendirme doğrudan bildirim temelli sözdiziminde girilebilir.

Fiyat ve stok TemplateField görüntüler için fiyat ve stok bilgileri static işaretleme, etiket Web denetimleri ve Veri bağlamada sözdizimi ekleyin sözlüğüdür:

*UnitPrice*  
(**Stoktaki / sipariş:** *unitsInStock* / *SiparişBirimleri*)

Bu görev gerçekleştirdikten sonra DetailsView'un bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

Bu değişikliklerle biz fiyat ve stok bilgileri tek bir DetailsView satır içine birleştirilmiş.


[![Envanter bilgileri ve fiyat tek satırda görüntülenir](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Şekil 8**: Fiyat ve envanter bilgileri tek satırda görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>3. adım: devam etmeyen alan bilgilerini özelleştirme

`Products` Tablonun `Discontinued` ürün ermiştir olup olmadığını gösteren bir bit değeri bir sütundur. Boole değeri alanları DetailsView (veya GridView) bir veri kaynağı denetimi ile bağlanırken, ister `Discontinued`, Boole olmayan değer alanları, ancak CheckBoxFields uygulanan `ProductID`, `ProductName`ve benzeri olarak uygulanır BoundFields. CheckBoxField veri alanın değeri True ve unchecked Aksi durumda ise, denetlenir devre dışı bırakılmış bir onay işler.

CheckBoxField görüntülemek yerine biz ürün kesilir olup olmadığına bakılmaksızın yerine metin belirten görüntülemek isteyebilirsiniz. Bunu gerçekleştirmek için şu CheckBoxField DetailsView kaldırın ve ardından bir BoundField ekleyin, `DataField` özelliği ayarlandığı `Discontinued`. Bunu yapmak için bir dakikanızı ayırın. Bu değişiklikten sonra DetailsView metni "True" devam etmeyen ürünleri ve "False" için hala etkin olan ürünler için gösterir.


[![Dizeleri True ve False devam etmeyen durumunu görüntülemek için kullanılır](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Şekil 9**: dizeleri True ve False kullanılan üretimi durdurulmuş durumunu görüntülemek için ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))


"Evet" ve "Hayır", bunun yerine kullanılacak dizeler "True" veya "False" istemediğinize düşünün. Bu tür özelleştirme bir TemplateField ve biçimlendirme yöntemi Etkinleştirici ile gerçekleştirilebilir. Biçimlendirme yöntemi herhangi bir sayıda giriş parametreleri alabilir, ancak HTML şablonuna eklemesine (bir dize olarak) döndürmesi gerekir.

Biçimlendirme bir yöntem ekleyin `DetailsViewTemplateField.aspx` adlı sayfanın arka plandaki kod sınıfı `DisplayDiscontinuedAsYESorNO` kabul eden bir `Northwind.ProductsRow` nesnesi bir giriş parametresi olarak ve bir dize döndürür. Önceki öğretici, bu yöntem anlatıldığı gibi *gerekir* olarak işaretlenmelidir `Protected` veya `Public` şablondan erişilebilir olması için.


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Bu yöntem giriş parametresi denetler (`discontinued`) ve onu ise "Evet" döndürür `True`, aksi takdirde "Hayır".

> [!NOTE]
> İçerebilecek bir veri alanını geçirme önceki öğretici geri çağırma incelenmesi biçimlendirme yönteminde `NULL` s ve bu nedenle denetlemek gereken çalışanın `HiredDate` özellik değeri olan bir veritabanı `NULL` önce değer erişme `EmployeesRow`'s `HiredDate` özelliği. Bu tür bir denetimi burada bu yana gerekli değildir `Discontinued` sütun hiç veritabanı sahip `NULL` değerler atanır. Yöntem bir Boole değeri giriş kabul etmek zorunda yerine parametre kabul Ayrıca, bu yüzden bir `ProductsRow` örneği veya türünde bir parametre `Object`.


Bu biçimlendirme yöntemi ile tam kalan tek şey TemplateField 's çağırmak için `ItemTemplate`. TemplateField kaldırın veya oluşturmak için `Discontinued` BoundField ve yeni TemplateField eklemek veya dönüştürmek `Discontinued` bir TemplateField içine BoundField. Çağıran bir ItemTemplate içeren bildirim temelli biçimlendirme görünümünden TemplateField sonra düzenleme `DisplayDiscontinuedAsYESorNO` geçerli değerinde geçirerek yöntemini `ProductRow` örneğinin `Discontinued` özelliği. Bu aracılığıyla erişilebilen `Eval` yöntemi. Özellikle, TemplateField'ın biçimlendirme gibi görünmelidir:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

Bu neden olacak `DisplayDiscontinuedAsYESorNO` DetailsView işlenirken çağrılacak yöntem tümleştirilmesidir `ProductRow` örneğinin `Discontinued` değeri. Bu yana `Eval` yöntemi türünde bir değer döndürür `Object`, ancak `DisplayDiscontinuedAsYESorNO` yöntemi bekliyor türünde bir giriş parametresi `Boolean`, biz cast `Eval` yöntemleri dönüş değeri için `Boolean`. `DisplayDiscontinuedAsYESorNO` Yöntemi sonra döndürür "Evet" veya "Hayır" değerine bağlı olarak alır. Bu DetailsView'da görüntülenenleri döndürülen değer: (bkz. Şekil 10) satır.


[![Şimdi gösterilen Üretimi Durdurulmuş satırda Evet veya Hayır değerler:](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Şekil 10**: Evet veya Hayır değerlerdir şimdi gösterilen Üretimi Durdurulmuş satırda ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))


## <a name="summary"></a>Özet

Bir diğer alan denetimleriyle kullanılabilir ve durumlarda idealdir çok verileri görüntüleme esneklik daha yüksek derecede DetailsView denetiminde TemplateField sağlar burada:

- Birden çok veri alanları bir GridView sütununda görüntülenen gerekir
- Verileri bir Web denetimi düz metin yerine en iyi ifade
- Temel alınan verileri, meta veri görüntüleme gibi veya verileri yeniden biçimlendirme çıkış bağlıdır

Büyük ölçüde DetailsView'un temel alınan veri işleme esneklik için TemplateFields izin verirken, her bir alan bir HTML bir satır olarak işlenen DetailsView çıkış hala biraz boxy hissi `<table>`.

FormView denetimini büyük oranda işlenmiş çıkış yapılandırmada esneklik sunar. FormView alanlar ancak bunun yerine yalnızca bir dizi şablonları içermiyor (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`, vb.). FormView bizim sonraki öğreticide işlenmiş düzeni daha fazla denetim elde etmek için nasıl kullanılacağını göreceğiz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Dan Jagers oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](using-templatefields-in-the-gridview-control-vb.md)
[sonraki](using-the-formview-s-templates-vb.md)
