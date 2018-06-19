---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: Toplu silme (C#) | Microsoft Docs
author: rick-anderson
description: Tek bir işlemde birden çok veritabanı kayıtlarını sil öğrenin. Kullanıcı arabirimi katmanda daha önceki bir tut oluşturulmuş bir geliştirilmiş GridView inşa edilmiştir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 59c90dcf373d19aad42250ee6dedba5f09f833b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891873"
---
<a name="batch-deleting-c"></a>Toplu silme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip) veya [PDF indirin](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> Tek bir işlemde birden çok veritabanı kayıtlarını sil öğrenin. Kullanıcı arabirimi katmanda sizi bir önceki öğreticide oluşturulmuş bir geliştirilmiş GridView bağlı oluşturun. Veri erişim katmanı'ndaki tüm silme işlemleri başarılı veya tüm silme işlemleri geri emin olmak için bir işlem içinde birden çok silme işlemleri alın.


## <a name="introduction"></a>Giriş

[Önceki öğretici](batch-updating-cs.md) tam olarak düzenlenebilir GridView kullanarak arabirimi düzenleme toplu oluşturmak nasıl incelediniz. Burada kullanıcıları yaygın olarak düzenleme çok sayıda kayıt aynı anda durumlarda arabirimini düzenleme bir toplu iş çok daha az Geri göndermeler ve klavye ve fare bağlamı gerektirir anahtarları, böylece son kullanıcı s verimliliği artırma. Bu teknik benzer şekilde tek bir seferde çok sayıda kayıt silmek kullanıcılar için ortak olduğu sayfaları için faydalıdır.

Bir çevrimiçi e-posta istemcisi kullanan herkesin zaten arabirimleri silme en yaygın batch biriyle alışkın olduğu: bir karşılık gelen silme işaretli öğelerin tümünü içeren bir kılavuz her satırda bir onay düğmesine (bkz: Şekil 1). Bu öğretici yerine kısa olduğundan biz ve web tabanlı arabirim ve kayıtları bir dizi olarak tek bir atomik işlemle silmek için bir yöntem oluşturma önceki öğreticileri sabit iş zaten yapıldı. İçinde [onay kutularını GridView sütunu ekleyerek](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) onay kutularını ve içinde sütunu GridView oluşturduğumuz öğretici [kaydırma veritabanı değişiklikleri bir işlem içinde](wrapping-database-modifications-within-a-transaction-cs.md) bir yöntem oluşturduğumuz Öğreticisi bir işlem silmek için kullanacağınız BLL bir `List<T>` , `ProductID` değerleri. Bu öğreticide, biz temel yapı ve örnek silme çalışma toplu oluşturmak için önceki deneyimlere birleştirin.


[![Her satır bir onay kutusu içerir](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**Şekil 1**: her satır bir onay kutusu içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-deleting-cs/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>1. adım: Arabirim silme toplu iş oluşturma

Zaten arabiriminde silme toplu oluşturduğumuz beri [onay kutularını GridView sütunu ekleyerek](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) öğretici, biz yalnızca kopyalayabilir kendisine `BatchDelete.aspx` sıfırdan oluşturmak yerine. Başlangıç açarak `BatchDelete.aspx` sayfasındaki `BatchData` klasör ve `CheckBoxField.aspx` sayfasındaki `EnhancedGridView` klasör. Gelen `CheckBoxField.aspx` sayfasında, kaynak görünümüne gidin ve biçimlendirme arasında kopyalama `<asp:Content>` etiketler Şekil 2'de gösterildiği gibi.


[![CheckBoxField.aspx bildirim temelli işaretleme Panoya Kopyala](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**Şekil 2**: bildirim temelli biçimlerini kopyalama `CheckBoxField.aspx` panoya ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-deleting-cs/_static/image4.png))


Ardından, kaynak görünümüne gidin `BatchDelete.aspx` içinde içindekileri yapıştırın `<asp:Content>` etiketler. Arka plan kodu sınıfında içinde koddan de kopyalayıp `CheckBoxField.aspx.cs` için arka plan kodu sınıfında içinde `BatchDelete.aspx.cs` ( `DeleteSelectedProducts` düğmesi s `Click` olay işleyicisi `ToggleCheckState` yöntemi ve `Click` olay işleyicileri için `CheckAll` ve `UncheckAll` düğmeleri). Bu içerik üzerinde kopyaladıktan sonra `BatchDelete.aspx` sayfa s arka plandaki kod sınıfı, aşağıdaki kodu içermelidir:


[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

Kaynak kodu ve bildirim temelli biçimlendirme kopyaladıktan sonra test etmek için bir dakikanızı ayırın `BatchDelete.aspx` bir tarayıcıdan görüntüleyerek. S ürün adı, kategori ve bir onay kutusu birlikte fiyat listesi her satırın ilk on ürünleriyle GridView içinde listeleme GridView görmeniz gerekir. Üç düğme olmalıdır: denetle, tüm seçeneğinin işaretini kaldırın ve seçili ürünler silin. Tüm onay kutularının işaretini tüm temizler sırada denetle düğmesine tıkladığınızda tüm onay kutularını seçer. Sil Seçili ürünlerin tıklatmak listeleyen bir ileti görüntüler `ProductID` değerleri seçili ürünlerin gerçekte ürünleri silmez ancak.


[![CheckBoxField.aspx arabiriminden BatchDeleting.aspx için taşındı](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**Şekil 3**: arabiriminden `CheckBoxField.aspx` taşındı `BatchDeleting.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-deleting-cs/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>2. adım: İşlemleri kullanarak Checked ürünleri silme

Başarıyla üzerinden kopyalanmasını arabirimi silme yığın `BatchDeleting.aspx`, tüm seçili ürünleri Sil düğmesini kullanarak checked ürünleri siler kodu güncelleştirmeye kalır olan `DeleteProductsWithTransaction` yönteminde `ProductsBLL` sınıfı. Eklenen bu yöntem [kaydırma veritabanı değişiklikleri bir işlem içinde](wrapping-database-modifications-within-a-transaction-cs.md) öğretici, kendi giriş kabul bir `List<T>` , `ProductID` değerleri ve her karşılık gelen siler `ProductID` kapsamında bir işlem.

`DeleteSelectedProducts` Düğmesi s `Click` olay işleyicisi şu anda kullandığı aşağıdaki `foreach` her GridView satır döngüsünü kullanabilirsiniz:


[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

Her satır için `ProductSelector` onay kutusunu Web denetimi programlı olarak başvurulur. İşaretlenirse, satır s `ProductID` alınır `DataKeys` koleksiyonu ve `DeleteResults` etiket s `Text` özelliği, satır silme işlemi için seçilmedi belirten bir ileti içerecek şekilde güncelleştirilir.

Yukarıdaki kod gerçekte herhangi bir kayıt için çağrı olarak silmez `ProductsBLL` s sınıfı `Delete` yöntemi açıklamalı çıkışı. Olan bu silme mantığı uygulanacak, kod ürünleri siler ancak atomik bir işlem içinde değil. Diğer bir deyişle, sıradaki ilk birkaç siler başarılı oldu, ancak daha sonraki bir (belki de bir yabancı anahtar kısıtlaması ihlali nedeniyle) başarısız oldu, özel durum ancak zaten silinmiş bu ürünlerden silinmiş olarak kalır.

Kararlılık güvence altına almak için kullanmayı ihtiyacımız `ProductsBLL` s sınıfı `DeleteProductsWithTransaction` yöntemi. Bu yöntem listesini kabul ettiğinden `ProductID` değerleri ihtiyacımız ilk kılavuz bu listeden derlemek ve parametre olarak geçirmek. İlk örneğini oluşturuyoruz bir `List<T>` türü `int`. İçinde `foreach` ihtiyacımız seçili ürünlerin eklemek için döngü `ProductID` bu değerleri `List<T>`. Döngü sonra bu `List<T>` için geçirilmelidir `ProductsBLL` s sınıfı `DeleteProductsWithTransaction` yöntemi. Güncelleştirme `DeleteSelectedProducts` düğmesi s `Click` aşağıdaki kod ile olay işleyicisi:


[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

Güncelleştirilmiş kod oluşturur bir `List<T>` türü `int` (`productIDsToDelete`) ve onunla doldurur `ProductID` silmek için değerleri. Sonra `foreach` seçili, en az bir ürün ise, döngü `ProductsBLL` s sınıfı `DeleteProductsWithTransaction` yöntemi çağrılır ve bu listeyi geçirildi. `DeleteResults` Etiketi de görüntülenir ve verileri DataSet'e GridView (yeni silinen kayıtlar artık ızgaradaki satırların olarak görünmesini sağlayacak şekilde).

Satır sayısı silinmek üzere seçilen sonra Şekil 4 GridView gösterir. Seçili ürünleri Sil düğmesine hemen tıklatıldıktan sonra Şekil 5 ekran gösterir. Şekil 5'te Not `ProductID` silinen kayıtların değerlerini GridView altındaki etikette görüntülenir ve bu satırların artık GridView.


[![Seçili ürünlerin silinecek](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**Şekil 4**: Seçili ürünleri silinecek ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-deleting-cs/_static/image8.png))


[![Silinen ürünleri ProductID GridView altında listelenen değerleri](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**Şekil 5**: silinen ürünleri `ProductID` GridView altında listelenen değerleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-deleting-cs/_static/image10.png))


> [!NOTE]
> Test etmek için `DeleteProductsWithTransaction` s yöntemi kararlılık el ile bir ürün için bir giriş eklemek `Order Details` tablo ve bu ürünün (yanı sıra diğerleri) silme girişimi. Bir yabancı anahtar kısıtlaması ihlali ilişkili sipariş ürünle silin, ancak, diğer seçili ürünleri silme işlemleri geri alınacak nasıl dikkat edin çalışılırken alırsınız.


## <a name="summary"></a>Özet

GridView onay kutularını sütunu ekleyerek arabirimi silme bir toplu iş oluşturmayı içerir ve bir düğme Web denetlemek,, tıklandığında, tüm seçili satırları tek bir atomik işlemle siler. Bu öğreticide böyle bir arabirim birlikte iki önceki eğitimlerine işlerini eklemekten tarafından oluşturduğumuz [onay kutularını GridView sütunu ekleyerek](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) ve [kaydırma veritabanı değişiklikleri bir işlem içinde](wrapping-database-modifications-within-a-transaction-cs.md). İlk öğreticide GridView onay kutularını sütunla oluşturduğumuz ve ikincisi biz BLL yönteminde uygulanan, geçirildiğinde bir `List<T>` , `ProductID` değerleri, bunları bir işlem kapsamı içinde tüm silindi.

Sonraki öğreticide toplu eklemeleri gerçekleştirmek için bir arabirim oluşturacağız.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Hilton Giesenow ve Teresa Murphy yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](batch-updating-cs.md)
> [sonraki](batch-inserting-cs.md)
