---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: Toplu silme (C#) | Microsoft Docs
author: rick-anderson
description: Tek bir işlemde birden çok veritabanı kaydı silmeyi öğrenin. Kullanıcı arabirimi katmanda daha önceki bir tut oluşturulmuş bir geliştirilmiş GridView ekleriz...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: e81c1aca63c07597915999144695d5a6a7deafbf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834070"
---
<a name="batch-deleting-c"></a>Toplu silme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip) veya [PDF olarak indirin](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> Tek bir işlemde birden çok veritabanı kaydı silmeyi öğrenin. Kullanıcı arabirimi katmanında bir önceki öğreticide oluşturulan bir Gelişmiş GridView bağlı ekleriz. Veri erişim katmanındaki biz tüm silme işlemleri başarılı olması veya tüm silme işlemleri geri alınacak emin olmak için bir işlem içinde birden çok silme işlemleri kaydır.


## <a name="introduction"></a>Giriş

[Önceki öğretici](batch-updating-cs.md) toplu düzenleme tam olarak düzenlenebilir GridView kullanarak arabirimi oluşturmak nasıl incelediniz. Burada kullanıcıların yaygın olarak düzenlemekte olduğunuz birçok kaydı tek seferde durumlarda arabirimini düzenleme toplu çok daha az Geri göndermeler ve klavye ve fare bağlamı gerektirir anahtarları, böylece son kullanıcı s verimliliğini artırma. Bu teknik benzer şekilde tek bir seferde birçok kayıtları silmek kullanıcılar için yaygın olduğu sayfaları için faydalıdır.

Bir çevrimiçi e-posta istemcisi kullanan herkesin zaten arabirimleri silme en yaygın batch biriyle alışkın olduğu: bir onay kutusu her satırda bir karşılık gelen silmek tüm seçili öğeleri içeren bir kılavuz düğmesine (bkz. Şekil 1). Bu öğretici çok kısa olduğundan ediyoruz ve tüm önceki öğreticilerde, web tabanlı bir arabirim hem kayıtları bir dizi tek bir atomik işlem olarak silmek için bir yöntem oluşturmanın zor bir iş zaten bitti. İçinde [onay kutularından oluşan GridView sütunu ekleme](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) sütunu bulunan onay kutularını ve içinde GridView oluşturduğumuz öğretici [veritabanı değişikliklerini bir işlemin içinde sarmalama](wrapping-database-modifications-within-a-transaction-cs.md) bir yöntemde oluşturduğumuz Öğreticisi bir işlem silmek için kullanacağınız BLL bir `List<T>` , `ProductID` değerleri. Bu öğreticide, biz sınayabilmesi ve örnek siliniyor çalışma toplu oluşturmak için önceki bizim deneyimleri birleştirin.


[![Bir onay kutusu her bir satır içerir](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**Şekil 1**: bir onay kutusu her bir satır içerir ([tam boyutlu görüntüyü görmek için tıklatın](batch-deleting-cs/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>1. adım: Arabirimi siliniyor toplu işi oluşturma

Zaten arabiriminde silme batch oluşturduğumuz bu yana [onay kutularından oluşan GridView sütunu ekleme](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) Öğreticisi, biz yalnızca kopyalayabilir kendisine `BatchDelete.aspx` sıfırdan oluşturmak yerine. Başlangıç açarak `BatchDelete.aspx` sayfasını `BatchData` klasörü ve `CheckBoxField.aspx` sayfasını `EnhancedGridView` klasör. Gelen `CheckBoxField.aspx` sayfasında kaynak görünümüne gidin ve biçimlendirme arasında kopyalama `<asp:Content>` etiketleri Şekil 2'de gösterildiği gibi.


[![Bildirim temelli biçimlerini CheckBoxField.aspx Panoya Kopyala](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**Şekil 2**: bildirim temelli biçimlerini kopyalama `CheckBoxField.aspx` panoya ([tam boyutlu görüntüyü görmek için tıklatın](batch-deleting-cs/_static/image4.png))


Ardından, Kaynak Görünümü'nde Git `BatchDelete.aspx` ve içinde Pano'nun içindekileri yapıştırın `<asp:Content>` etiketler. Kodu arka plan kod sınıfının içinde de kopyalayıp `CheckBoxField.aspx.cs` için arka plan kod sınıfı içinde `BatchDelete.aspx.cs` ( `DeleteSelectedProducts` s düğmesi `Click` olay işleyicisi `ToggleCheckState` yöntemi ve `Click` olay işleyicileri için `CheckAll` ve `UncheckAll` düğmeler). Bu içerik üzerinde kopyaladıktan sonra `BatchDelete.aspx` sayfası s arka plan kod sınıfı aşağıdaki kodu içermelidir:


[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

Kaynak kodu ve bildirim temelli biçimlendirme kopyaladıktan sonra test etmek için bir dakikanızı ayırarak `BatchDelete.aspx` bir tarayıcıdan görüntüleyerek. GridView s ürün adı, kategori ve bir onay kutusu birlikte fiyat listesi her satırın ilk on ürünleri listeleme GridView görmeniz gerekir. Üç düğme olmalıdır: denetle, tüm işaretini kaldırın ve ürün seçildi silin. Tüm onay kutularının işaretini kaldırın tüm temizler sırada denetle düğmesine tıklayarak tüm onay kutusunu seçer. Seçili ürünlerin Sil tıklayarak listeleyen bir ileti görüntüler `ProductID` seçili ürünlerin değerleri gerçekten ürünleri silmez ancak.


[![CheckBoxField.aspx arabiriminden için BatchDeleting.aspx taşındı](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**Şekil 3**: arabiriminden `CheckBoxField.aspx` taşındı `BatchDeleting.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](batch-deleting-cs/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>2. adım: İşlemleri kullanarak işaretli ürün siliniyor

Başarıyla üzerinden kopyalanmasını arabirimi siliniyor batch ile `BatchDeleting.aspx`, tüm kalan olduğunu ürün seçildi Sil düğmesini kullanarak işaretli ürünleri siler. böylece kodu güncelleştirmeye `DeleteProductsWithTransaction` yönteminde `ProductsBLL` sınıfı. Eklenen bu yöntem, [veritabanı değişikliklerini bir işlemin içinde sarmalama](wrapping-database-modifications-within-a-transaction-cs.md) öğreticide, girdi olarak kabul eden bir `List<T>` , `ProductID` değerleri ve karşılık gelen her siler `ProductID` kapsamında bir işlem.

`DeleteSelectedProducts` s düğmesi `Click` olay işleyicisi aşağıdaki kullanmakta `foreach` döngü her GridView satır yineleme yapmak için:


[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

Her satır için `ProductSelector` onay kutusu Web denetimi programlı olarak başvuruluyor. İşaretlenirse, satır s `ProductID` alınır `DataKeys` koleksiyonu ve `DeleteResults` etiket s `Text` özelliği, satır silme işlemi için seçildi belirten bir ileti içerecek şekilde güncelleştirilir.

Yukarıdaki kod gerçekte herhangi bir kayıt çağrısı olarak silmez `ProductsBLL` s sınıfı `Delete` yöntemi dışında bırakılır. Olan bu silme mantığı uygulanması, kodun ürünleri siler ancak atomik bir işlem içinde değil. Diğer bir deyişle, bir özel durum dizideki ilk birkaç silme başarılı oldu, ancak daha sonraki bir (belki de bir yabancı anahtar kısıtlaması ihlali nedeniyle) başarısız oldu, ancak bu ürünlerin zaten silinmiş silinmiş olarak kalır.

Kararlılık güvence altına almak için kullanmayı gerekiyor `ProductsBLL` s sınıfı `DeleteProductsWithTransaction` yöntemi. Bu yöntem bir listesini kabul ettiğinden `ProductID` değerleri ihtiyacımız ilk kılavuz bu listeden derlemek ve ardından bunu bir parametre olarak geçiriyoruz. İlk örneğini oluşturacağız bir `List<T>` türü `int`. İçinde `foreach` ihtiyacımız seçili ürünlerin eklemek için döngü `ProductID` bu değerleri `List<T>`. Döngü sonra bu `List<T>` geçirilmelidir `ProductsBLL` s sınıfı `DeleteProductsWithTransaction` yöntemi. Güncelleştirme `DeleteSelectedProducts` s düğmesi `Click` olay işleyicisi aşağıdaki kod ile:


[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

Güncelleştirilmiş kod oluşturur bir `List<T>` türü `int` (`productIDsToDelete`) ve onunla doldurur `ProductID` silmek için değerler. Sonra `foreach` seçilmedi, en az bir ürün varsa, döngü `ProductsBLL` s sınıfı `DeleteProductsWithTransaction` yöntemi çağrılır ve bu listesi geçirildi. `DeleteResults` Veri DataSet'e GridView'a (yeni silinen kayıtlar artık ızgaradaki satırların olarak görünecek biçimde) ve etiket de görüntülenir.

Şekil 4, satır sayısı silinmek üzere seçilen sonra GridView gösterir. Şekil 5, ürün seçildi Sil düğmesini hemen tıklatıldıktan sonra ekran gösterir. Şekil 5'te unutmayın `ProductID` silinen kayıtlar değerlerini GridView altına etiketi gösterilir ve bu satırların artık içinde GridView.


[![Seçili ürünlerin silinecek](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**Şekil 4**: Seçili ürün silinecek ([tam boyutlu görüntüyü görmek için tıklatın](batch-deleting-cs/_static/image8.png))


[![Silinen ürünlerin ProductID GridView altında listelenen değerler](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**Şekil 5**: silinen ürünlerin `ProductID` GridView altında listelenen değerler ([tam boyutlu görüntüyü görmek için tıklatın](batch-deleting-cs/_static/image10.png))


> [!NOTE]
> Test etmek için `DeleteProductsWithTransaction` metodu s kararlılık, el ile bir ürün için bir giriş ekleyin `Order Details` tablo ve ardından bu ürünü (birlikte diğerleri) silmeyi deneyin. Bir yabancı anahtar kısıtlaması ihlali ürünle birlikte ilişkili bir sipariş silin, ancak diğer seçili ürünlerin silme işlemleri geri alınacak nasıl unutmayın çalışırken alırsınız.


## <a name="summary"></a>Özet

Sütunu onay kutularından oluşan GridView ekleme arabirimi silinirken bir toplu iş oluşturmayı içerir ve düğmesi Web denetimi, tıklandığında, tek bir atomik işlem olarak seçili tüm satırları siler. Bu öğreticide böyle bir arabirim tarafından iki önceki öğreticilerde, çalışmanın birlikte eklemekten oluşturduk [onay kutularından oluşan GridView sütunu ekleme](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) ve [veritabanı değişikliklerini bir işlemin içinde sarmalama](wrapping-database-modifications-within-a-transaction-cs.md). İlk öğreticide GridView onay kutularından oluşan bir sütun oluşturduk ve BLL yönteminde uyguladık ikinci, geçirildiğinde bir `List<T>` , `ProductID` değerleri, bunları tüm bir işlem kapsamında silindi.

Sonraki öğreticide batch ekler gerçekleştirmek için bir arabirim oluşturacağız.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Hilton Giesenow ve Teresa Murphy yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](batch-updating-cs.md)
> [İleri](batch-inserting-cs.md)
