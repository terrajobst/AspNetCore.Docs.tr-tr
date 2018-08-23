---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
title: Toplu güncelleştirme (C#) | Microsoft Docs
author: rick-anderson
description: Tek bir işlemde birden çok veritabanı kayıtlarını güncelleştirmek hakkında bilgi edinin. Kullanıcı arabirimi katmanda her satırı düzenlenebilir olduğu GridView ekleriz. Veri...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 4e849bcc-c557-4bc3-937e-f7453ee87265
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
msc.type: authoredcontent
ms.openlocfilehash: c878056273ea821e4dd4481fa1b6f7690f22b285
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753141"
---
<a name="batch-updating-c"></a>Toplu güncelleştirme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_CS.zip) veya [PDF olarak indirin](batch-updating-cs/_static/datatutorial64cs1.pdf)

> Tek bir işlemde birden çok veritabanı kayıtlarını güncelleştirmek hakkında bilgi edinin. Kullanıcı arabirimi katmanda her satırı düzenlenebilir olduğu GridView ekleriz. Veri erişim katmanındaki tüm güncelleştirmeleri başarılı olması veya tüm güncelleştirmeleri geri alınacak emin olmak için bir işlem içinde birden çok güncelleştirme işlemi biz kaydır.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](wrapping-database-modifications-within-a-transaction-cs.md) veritabanı işlemleri için destek eklemek için veri erişim katmanı genişletme gördük. Veritabanı işlemleri, bir dizi veri değişikliği deyim tüm değişiklikler yapamaz veya tüm başarılı olur sağlayan bir atomik işlem olarak kabul edilir olduğunu garanti. Bu alt düzey DAL işlevleri ile ortada, biz re toplu veri değişikliği arabirimleri oluşturmaya uygulamamızla etkinleştirmek için hazır.

Bu öğreticide her satırı düzenlenebilir (bkz. Şekil 1) olduğu GridView oluşturacağız. Her satır bir düzenleme sütununun gerek düzenleme arabirimi içinde orada s işlenen olduğundan, Güncelleştir ve İptal düğmeleri. Bunun yerine, iki Update ürünleri düğme sayfada olduğunu, tıklandığında GridView satırları listeleme ve veritabanını güncelleştir.


[![Her GridView satırında düzenlenebilir olduğunu](batch-updating-cs/_static/image1.gif)](batch-updating-cs/_static/image1.png)

**Şekil 1**: her GridView satırında düzenlenebilir olduğunu ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image2.png))


Let s başlayın!

> [!NOTE]
> İçinde [toplu güncelleştirmeler gerçekleştirme](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) öğreticide oluşturduğumuz bir toplu düzenleme arabirim DataList denetimi kullanarak. Bu öğreticide, bir kullanımdır GridView farklıdır ve toplu güncelleştirme bir işlem kapsamında gerçekleştirilir. Bu öğreticiyi tamamladıktan sonra önceki öğreticiye geri dönün ve önceki öğreticide eklenen veritabanı işlem ile ilgili işlevselliğini kullanacak şekilde güncelleştirmek geçmenizi öneriyoruz.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Tüm GridView satır düzenlenebilir yapma adımları İnceleme

Bölümünde açıklandığı gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) öğreticide GridView satır başına temelinde, temel alınan verileri düzenleme için yerleşik destek sunar. Dahili olarak, hangi satır yoluyla düzenlenebilir GridView notları kendi [ `EditIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). GridView kendi veri kaynağına bağlı olarak, her satır, satır dizinini değeri eşitse görmek için denetler `EditIndex`. Bu durumda, bu satır s alanları düzenleme kendi kullanılarak işlenir arabirimleri. BoundFields için düzenleme TextBox arabirimidir olan `Text` özelliği BoundField s tarafından belirtilen veri alanının değeri atanır `DataField` özelliği. TemplateField için `EditItemTemplate` yerine kullanılan `ItemTemplate`.

Bir kullanıcı bir satır s Düzenle düğmesine tıkladığında düzenleme iş akışı başlatan geri çağırma. Bu geri göndermeye neden olur, GridView s ayarlar `EditIndex` tıklanan satır s dizini ve rebinds kılavuza veri özelliği. Ne zaman bir satır s iptal düğmesine tıklandığında, geri göndermede `EditIndex` değerine ayarlanmış `-1` kılavuza veriler yeniden bağlama önce. Sıfırdan dizin GridView s satırları başlangıcından itibaren ayarlama `EditIndex` için `-1` GridView salt okunur modunda görüntüleme etkisi vardır.

`EditIndex` Özelliği de satır içi düzenleme için çalışır, ancak toplu düzenleme için tasarlanmamıştır. Tüm GridView düzenlenebilir hale getirmek için düzenleme, arabirim kullanılarak her satır ihtiyacımız var. Bunu yapmanın en kolay yolu, düzenleme arabirimiyle bir TemplateField tanımlandığı gibi her düzenlenebilir bir alanı burada uygulanan oluşturmaktır `ItemTemplate`.

Sonraki birkaç adım tamamen düzenlenebilir GridView oluşturacağız. 1. adımda biz GridView ve kendi ObjectDataSource oluşturarak başlayın ve CheckBoxField ve BoundFields TemplateField dönüştürün. Adım 2 ve 3 düzenleme arabirimleri TemplateField geçeceğiz `EditItemTemplate` s kendi `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>1. adım: Ürün bilgilerini görüntüleme

GridView oluşturma hakkında endişe önce satır nerede düzenlenebilir, ürün bilgilerini görüntüleyerek Başlat s olanak tanır. Açık `BatchUpdate.aspx` sayfasını `BatchData` klasörü ve GridView tasarımcı araç kutusundan sürükleyin. GridView s ayarlamak `ID` için `ProductsGrid` ve adlı yeni bir ObjectDataSource bağlamak, akıllı etiketten seçin `ProductsDataSource`. ObjectDataSource, verileri almak için yapılandırma `ProductsBLL` s sınıfı `GetProducts` yöntemi.


[![ObjectDataSource ProductsBLL sınıfını kullanmak için yapılandırma](batch-updating-cs/_static/image2.gif)](batch-updating-cs/_static/image3.png)

**Şekil 2**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image4.png))


[![GetProducts yöntemi kullanarak ürün verileri alma](batch-updating-cs/_static/image3.gif)](batch-updating-cs/_static/image5.png)

**Şekil 3**: Ürün kullanarak verileri almak `GetProducts` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image6.png))


GridView gibi ObjectDataSource s değişiklik özelliklerini satır içi olarak çalışacak şekilde tasarlanmıştır. Kayıt kümesini güncelleştirmek için biz verilerin toplu işlemleri ve BLL için aktaran ASP.NET sayfası s arka plan kod sınıfı bir bit kod yazma gerekir. Bu nedenle, açılan listeler, UPDATE, INSERT ve DELETE sekmeler (hiçbiri) ObjectDataSource s'te ayarlayın. Sihirbazı tamamlamak için Son'u tıklatın.


[![Güncelleştirme, ekleme, açılan listeler ayarlayın ve sekmeleri (hiçbiri) silme](batch-updating-cs/_static/image4.gif)](batch-updating-cs/_static/image7.png)

**Şekil 4**: aşağı açılan listeler güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image8.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra ObjectDataSource s bildirim temelli biçimlendirmeyi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](batch-updating-cs/samples/sample1.aspx)]

Veri Kaynağı Yapılandırma Sihirbazı Tamamlanıyor ayrıca BoundFields ve ürün veri alanları için bir CheckBoxField GridView içinde oluşturmak Visual Studio neden olur. Bu öğreticide, yalnızca ürün adı, kategori, fiyat ve artık sağlanmayan durumunu görüntüleyin ve düzenleyin kullanıcıya izin s olanak tanır. Kaldırma dışındaki tüm `ProductName`, `CategoryName`, `UnitPrice`, ve `Discontinued` alanları ve yeniden adlandırma `HeaderText` ilk üç özelliklerini alanları ürün, kategori ve fiyat, sırasıyla. Son olarak, GridView s akıllı etiket etkinleştirme sayfalama ve sıralamayı etkinleştir onay kutularını işaretleyin.

Bu noktada GridView üç BoundFields sahiptir (`ProductName`, `CategoryName`, ve `UnitPrice`) ve bir CheckBoxField (`Discontinued`). Bu dört alan TemplateField dönüştürmek ve ardından düzenleme arabirimi TemplateField s taşımak ihtiyacımız `EditItemTemplate` için kendi `ItemTemplate`.

> [!NOTE]
> Biz oluşturma ve özelleştirme içinde TemplateField incelediniz [veri değişikliği arabirimini özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) öğretici. TemplateField BoundFields ve CheckBoxField dönüştürme adımları gösterilecektir ve bunların düzenleme tanımlama arabirimleri kendi `ItemTemplate` s, ancak takılı kalarak veya bilgilerinizi tazelemeniz rsquo; bu önceki öğreticiye geri başvurmak için istemeyebilir.


GridView s akıllı etiketten alanları iletişim kutusunu açmak için sütunları Düzenle bağlantısına tıklayın. Ardından, her bir alan seçin ve bu alan dönüştürme TemplateField bağlantıya tıklayın.


![Mevcut BoundFields ve CheckBoxField TemplateField dönüştürün](batch-updating-cs/_static/image5.gif)

**Şekil 5**: CheckBoxField ve mevcut BoundFields TemplateField dönüştürün


Her alanın bir TemplateField olduğuna göre gelen biz yeniden düzenleme taşımaya hazır arabirim `EditItemTemplate` s `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>2. adım: Oluşturma`ProductName`,`UnitPrice`, ve`Discontinued`arabirimleri düzenleme

Oluşturma `ProductName`, `UnitPrice`, ve `Discontinued` arabirimleri düzenleme bu adımın konu ve her arabirim zaten TemplateField s'te tanımlanan oldukça basittir `EditItemTemplate`. Oluşturma `CategoryName` arabirimini düzenleme biraz daha karmaşık bir DropDownList ilgili kategorilerin oluşturmak gerektiğinden. Bu `CategoryName` arabirimini düzenleme 3. adımda tackled.

İle başlayan s izin `ProductName` TemplateField. GridView s akıllı etiketinde Şablonları Düzenle bağlantısına tıklayın ve için detaya gidin `ProductName` TemplateField s `EditItemTemplate`. Metin kutusunu seçin, panoya kopyalayın ve ardından ona yapıştırın `ProductName` TemplateField s `ItemTemplate`. TextBox s değiştirme `ID` özelliğini `ProductName`.

Ardından, bir RequiredFieldValidator için ekleme `ItemTemplate` kullanıcı her ürün s adı için bir değer sağladığından emin olmak için. Ayarlama `ControlToValidate` ProductName, özelliğini `ErrorMessage` , özelliğe ürün adı sağlamalısınız. ve `Text` özelliğini \*. Bu eklemeler yaptıktan sonra `ItemTemplate`, Şekil 6'ekranınızın benzemelidir.


[![Bir metin kutusu ve bir RequiredFieldValidator ProductName TemplateField şimdi içerir](batch-updating-cs/_static/image6.gif)](batch-updating-cs/_static/image9.png)

**Şekil 6**: `ProductName` TemplateField artık içeren bir metin kutusu ve RequiredFieldValidator ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image10.png))


İçin `UnitPrice` TextBox'dan kopyalayarak arabirimi düzenleme, başlangıç `EditItemTemplate` için `ItemTemplate`. Ardından, metin kutusu ve kümesi önünde bir $'nı koyun, `ID` UnitPrice özelliğini ve kendi `Columns` 8 özelliği.

Ayrıca bir CompareValidator'la ekleme `UnitPrice` s `ItemTemplate` kullanıcı tarafından girilen değer geçerli bir para birimi değeri büyüktür veya eşittir 0,00 ABD Doları olduğundan emin olmak için. Doğrulayıcı s ayarlamak `ControlToValidate` özelliğini UnitPrice, kendi `ErrorMessage` özelliği size geçerli para birimi değerini girmeniz gerekmektedir. Lütfen bir para birimi sembolleri, numarayı atlayın. kendi `Text` özelliğini \*, kendi `Type` özelliğini `Currency`, kendi `Operator` özelliğini `GreaterThanEqual`ve onun `ValueToCompare` özelliğinin 0.


[![Para birimi değeri negatif olmayan bir CompareValidator fiyat girilen emin olmak için ekleyin](batch-updating-cs/_static/image7.gif)](batch-updating-cs/_static/image11.png)

**Şekil 7**: girilen fiyat emin olmak için bir CompareValidator negatif olmayan bir para birimi değeri eklemek ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image12.png))


İçin `Discontinued` TemplateField kullanabileceğiniz önceden tanımlanmış onay `ItemTemplate`. Ayarlamanız yeterlidir, `ID` artık Üretilmiyor için ve kendi `Enabled` özelliğini `true`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>3. adım: Oluşturma`CategoryName`arabirimini düzenleme

Düzenleme arabiriminde `CategoryName` TemplateField s `EditItemTemplate` değerini gösteren bir metin kutusu içeren `CategoryName` veri alanı. Bu olası kategorileri listeleyen bir DropDownList ile değiştirilecek ihtiyacımız var.

> [!NOTE]
> [Veri değişikliği arabirimini özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) öğretici TextBox'ın aksine bir DropDownList dahil etmek için bir şablonu özelleştirme hakkında daha kapsamlı ve eksiksiz bir tartışma içerir. Buradaki adımları tam olsa da, bunlar tersely sunulur. Oluşturma ve yapılandırma DropDownList kategoriler bir daha derinlemesine bakış için kiracıurl [veri değişikliği arabirimini özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) öğretici.


Bir DropDownList araç kutusundan sürükleyin `CategoryName` TemplateField s `ItemTemplate`, ayar, `ID` için `Categories`. Bu noktada biz genellikle akıllı etiketinde aracılığıyla DropDownList s veri kaynağına yeni ObjectDataSource oluşturma tanımlarsınız. Ancak, bu içinde ObjectDataSource ekler `ItemTemplate`, her GridView satır için oluşturulan bir ObjectDataSource örneğinde neden olur. Bunun yerine, GridView s TemplateField dışında ObjectDataSource oluşturma s olanak tanır. Şablon düzenleme sonlandırmak ve bir ObjectDataSource tasarımcı araç kutusundan sürükleyin `ProductsDataSource` ObjectDataSource. Yeni ObjectDataSource ad `CategoriesDataSource` ve kullanacak şekilde yapılandırma `CategoriesBLL` s sınıfı `GetCategories` yöntemi.


[![ObjectDataSource CategoriesBLL Clas kullanmak için yapılandırma](batch-updating-cs/_static/image8.gif)](batch-updating-cs/_static/image13.png)

**Şekil 8**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` Clas ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image14.png))


[![Kategori veri GetCategories yöntemi](batch-updating-cs/_static/image9.gif)](batch-updating-cs/_static/image15.png)

**Şekil 9**: Kategori kullanarak verileri almak `GetCategories` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image16.png))


Bu ObjectDataSource yalnızca verileri almak için kullanıldığından, açılan listeler UPDATE ve DELETE sekmeler (yok) olarak ayarlayın. Sihirbazı tamamlamak için Son'u tıklatın.


[![Kümesi açılan listeler güncelleştirme ve silme sekmelerde (yok)](batch-updating-cs/_static/image10.gif)](batch-updating-cs/_static/image17.png)

**Şekil 10**: aşağı açılan listeler güncelleştirme ve silme sekmeler (yok) olarak ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image18.png))


Sihirbazı tamamladıktan sonra `CategoriesDataSource` s bildirim temelli biçimlendirme, aşağıdaki gibi görünmelidir:


[!code-aspx[Main](batch-updating-cs/samples/sample2.aspx)]

İle `CategoriesDataSource` oluşturulan ve yapılandırılan dönmek `CategoryName` TemplateField s `ItemTemplate` ve DropDownList s akıllı etiketten veri kaynağı Seç bağlantıya tıklayın. Veri Kaynağı Yapılandırma Sihirbazı'nda seçin `CategoriesDataSource` seçeneği ilk açılan listeden ve seçtiğiniz `CategoryName` görüntülemek için kullanılan ve `CategoryID` değeri.


[![Bir DropDownList CategoriesDataSource için bağlama](batch-updating-cs/_static/image11.gif)](batch-updating-cs/_static/image19.png)

**Şekil 11**: DropDownList'e bağlama `CategoriesDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image20.png))


Bu noktada `Categories` DropDownList kategorilerin tümünü listeler, ancak değil ancak otomatik olarak GridView satır ilişkili ürün için uygun kategoriyi seçin. Bunu ayarlamak için ihtiyacımız gerçekleştirmek için `Categories` DropDownList s `SelectedValue` s ürüne `CategoryID` değeri. DropDownList s akıllı etiketinde veri bağlamaları Düzenle bağlantısına tıklayın ve ilişkilendirmek `SelectedValue` özelliğiyle `CategoryID` veri alanı Şekil 12'de gösterildiği gibi.


![Ürün s CategoryID değeri DropDownList s SelectedValue özelliği Bağla](batch-updating-cs/_static/image12.gif)

**Şekil 12**: s ürün bağlama `CategoryID` DropDownList s değerine `SelectedValue` özelliği


Son bir sorun kalır: Ürün eklenmemişse t varsa bir `CategoryID` değeri belirtilen sonra veri bağlama ifadesi `SelectedValue` bir özel durumu oluşur. DropDownList kategorileri için yalnızca belirli bir öğe içeriyor ve bir seçenek olan bu ürünlere yönelik sunmaz nedeni bir `NULL` veritabanı için değer `CategoryID`. Bu sorunu gidermek için DropDownList s ayarlamak `AppendDataBoundItems` özelliğini `true` ve DropDownList'e yeni bir öğe eklemek atlama `Value` bildirim temelli söz dizimi özelliği. Diğer bir deyişle, emin `Categories` DropDownList s bildirim temelli söz dizimi aşağıdaki gibi görünür:


[!code-aspx[Main](batch-updating-cs/samples/sample3.aspx)]

Not nasıl `<asp:ListItem Value="">` --bir seçin--sahip kendi `Value` özniteliği boş bir dize olarak açıkça ayarlayın. Kiracıurl [veri değişikliği arabirimini özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) işlemek için bu ek DropDownList öğesi neden gerekli üzerinde daha kapsamlı bir tartışma için öğretici `NULL` durum ve neden atamasını `Value` boş bir dize özelliğini büyük/küçük harf önemlidir.

> [!NOTE]
> Bu söz olası bir performans ve ölçeklenebilirlik sorun burada yoktur. Her satır kullanan bir DropDownList olduğundan `CategoriesDataSource` kendi veri kaynağı olarak `CategoriesBLL` s sınıfı `GetCategories` yöntemin çağrılacağı *n* ziyaret başına sayfa burada *n* sayısı GridView satır. Bunlar *n* çağrılar `GetCategories` neden *n* veritabanını sorgular. İstek başına önbellek veya bir SQL bağımlılık veya bir çok kısa zamana bağlı süre sonu önbellek kullanarak önbelleğe alma katmanı aracılığıyla döndürülen kategorileri önbelleğe alarak Bu etkiyi veritabanında lessened. Önbelleği seçeneğini, istek başına daha fazla bilgi için bkz: [ `HttpContext.Items` istek başına önbellek Store](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>4. adım: düzenleme arabirimi Tamamlanıyor

Biz ve yapılan bir dizi değişiklik GridView s şablonlara ilerlememizin görüntülemek için duraklatmadan. Bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Şekil 13 gösterildiği gibi her satır kullanılarak işlenir, `ItemTemplate`, arabirimini düzenleme hücre s içerir.


[![Her GridView satır düzenlenebilir olduğunu](batch-updating-cs/_static/image13.gif)](batch-updating-cs/_static/image21.png)

**Şekil 13**: her GridView satır düzenlenebilir olduğunu ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image22.png))


Biz bu noktada ilgileniriz birkaç küçük biçimlendirme sorunları vardır. İlk olarak, dikkat `UnitPrice` değerini dört ondalık noktası içerir. Bu sorunu gidermek için iade `UnitPrice` TemplateField s `ItemTemplate` ve metin kutusu s akıllı etiketten veri bağlamaları Düzenle bağlantısına tıklayın. Ardından, belirten `Text` özelliği, bir sayı olarak biçimlendirilmelidir.


![Metin özelliğini bir sayı olarak Biçimlendir](batch-updating-cs/_static/image14.gif)

**Şekil 14**: biçim `Text` bir sayı olarak özelliği


İkinci olarak, onay kutusuna merkezi let s `Discontinued` sütun (yerine bunu sola hizalanmış). GridView s akıllı etiketi Düzenle sütunlarda'ı tıklatın ve seçin `Discontinued` TemplateField sol alt köşedeki alanlar listesinden. Detaya `ItemStyle` ayarlayıp `HorizontalAlign` Şekil 15'te gösterildiği gibi Merkezi'ne özelliği.


![Kullanımdan Kaldırılan onay merkezi](batch-updating-cs/_static/image15.gif)

**Şekil 15**: Merkezi `Discontinued` onay kutusu


Ardından, ValidationSummary denetimi sayfaya ekleyip ayarlayın, `ShowMessageBox` özelliğini `true` ve kendi `ShowSummary` özelliğini `false`. Ayrıca tıklandığında düğmesi Web, denetimleri ekleme, kullanıcı s değişiklikleri güncelleştirir. Özellikle, iki düğme Web denetimleri, bir GridView yukarıda ve her iki denetim ayarı bir altındaki eklemeniz `Text` Update ürünleri özellikleri.

GridView s beri arabirimini düzenleme kendi TemplateField içinde tanımlanır `ItemTemplate` s, `EditItemTemplate` s gereksiz ve silinebilir.

Yukarıdaki yapmadan biçimlendirme değişikliklerini belirtilen sonra düğme denetimleri ekleme ve kaldırma gereksiz `EditItemTemplate` s, sayfa s bildirim temelli söz dizimi görünmelidir aşağıdakine benzer:


[!code-aspx[Main](batch-updating-cs/samples/sample4.aspx)]

Şekil 16 düğmesi Web denetimleri eklendikten sonra bir tarayıcıdan görüntülendiğinde bu sayfada ve biçimlendirme değişiklikleri gösterir.


[![Sayfa artık iki güncelleştirme ürünleri düğmeleri içerir](batch-updating-cs/_static/image16.gif)](batch-updating-cs/_static/image23.png)

**Şekil 16**: sayfa artık içeren iki güncelleştirme ürünleri düğmeler ([tam boyutlu görüntüyü görmek için tıklatın](batch-updating-cs/_static/image24.png))


## <a name="step-5-updating-the-products"></a>5. adım: ürünleri güncelleştiriliyor

Bir kullanıcı bu sayfayı ziyaret ettiğinde bunlar kendi değişiklikleri yapın ve iki Update ürünleri düğmelerden birine tıklayın. Bu noktada şekilde her bir satır için kullanıcı tarafından girilen değerler kaydetmek ihtiyacımız bir `ProductsDataTable` örnek ve ardından, daha sonra geçer BLL yönteme geçirin `ProductsDataTable` DAL s örneğine `UpdateWithTransaction` yöntemi. `UpdateWithTransaction` , Oluşturduğumuz yöntemi [önceki öğretici](wrapping-database-modifications-within-a-transaction-cs.md), toplu değişiklikler atomik işlem güncelleştirilecek sağlar.

Adlı bir yöntem oluşturma `BatchUpdate` içinde `BatchUpdate.aspx.cs` ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](batch-updating-cs/samples/sample5.cs)]

Bu yöntem tüm ürünleri alarak başlar geri bir `ProductsDataTable` BLL s çağrısıyla `GetProducts` yöntemi. Ardından numaralandırır `ProductGrid` GridView s [ `Rows` koleksiyon](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). `Rows` Koleksiyonu içeren bir [ `GridViewRow` örneği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) GridView içinde görüntülenen her satır için. Biz sayfasında GridView s başına en fazla on satır gösteren bu yana `Rows` koleksiyonu, en fazla on öğe olacaktır.

Her satır için `ProductID` gelen yakaladı `DataKeys` toplama ve uygun `ProductsRow` seçilir `ProductsDataTable`. Dört TemplateField giriş denetimlerini programlı olarak başvurulur ve değerlerine atandığı `ProductsRow` örnek s özellikleri. Sonra her GridView satır s değerleri güncelleştirmek için kullanılmış olan `ProductsDataTable`, onu s geçirilen BLL s `UpdateWithTransaction` önceki öğreticide gördüğümüz gibi yalnızca DAL s ile çağırır yöntemi `UpdateWithTransaction` yöntemi.

Bu öğreticide kullanılan toplu güncelleştirme algoritması her satırı güncelleştirir `ProductsDataTable` karşılık gelen bir satıra s ürün bilgisi değiştirildi mi bakılmaksızın GridView. Böyle blind dahilse t genellikle bir performans sorunu güncelleştirirken, Denetim yapıldığı veritabanı tablosuna değişirse, gereksiz kayıtları neden olabilir. Geri [toplu güncelleştirmeler gerçekleştirme](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) DataList'i ile arabirimini güncelleştirme toplu incelediniz ve gerçekte kullanıcı tarafından değiştirilmiş olan kayıtları yalnızca güncelleştirilecek kodunu eklenmiş Öğreticisi. Teknikleri araştırmalarında [toplu güncelleştirmeler gerçekleştirme](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) isterseniz bu öğreticide, kod güncelleştirilecek.

> [!NOTE]
> GridView akıllı etiketinde aracılığıyla veri kaynağına bağlanırken, Visual Studio GridView s için veri kaynağı s birincil anahtar değerlerini otomatik olarak atar. `DataKeyNames` özelliği. ObjectDataSource GridView aracılığıyla GridView s akıllı etiket için 1. adımda açıklandığı bağlanmadı sonra GridView s el ile ayarlamak ihtiyacınız olacak `DataKeyNames` özelliğine erişmek için ProductID `ProductID` her satırı için değer `DataKeys` koleksiyonu.


Uygulamasında kullanılan kodlar `BatchUpdate` BLL s'te kullanılan benzer `UpdateProduct` yöntemleri, temel fark, olan `UpdateProduct` yöntemleri yalnızca tek bir `ProductRow` örneği mimariden alınır. Özelliklerini atar kod `ProductRow` arasında aynı `UpdateProducts` yöntemleri ve içindeki kod `foreach` içinde döngü `BatchUpdate`genel desen gibi.

Bu öğreticiyi tamamlamak için sağlamak ihtiyacımız `BatchUpdate` yöntemini çağırmış ya da Update ürünleri düğme tıklandığında. Olay işleyicileri `Click` olayları bu iki düğme denetimleri ve olay işleyicileri aşağıdaki kodu ekleyin:


[!code-csharp[Main](batch-updating-cs/samples/sample6.cs)]

İlk için bir çağrı yapılır `BatchUpdate`. Ardından, `ClientScript property` ürünleri güncelleştirilmiş okuyan bir messagebox görüntüler JavaScript ekleme için kullanılır.

Bu kodu test etmek için bir dakikanızı ayırın. Ziyaret `BatchUpdate.aspx` tarayıcısından, satır sayısı düzenleyin ve Update ürünleri düğmelerden birine tıklayın. Hiçbir giriş doğrulama hataları var. varsayıldığında, güncelleştirilen ürünler okuyan bir messagebox görmeniz gerekir. Güncelleştirme kararlılık doğrulamak için rastgele bir sıra eklemeyi göz önünde bulundurun `CHECK` kısıtlaması, bir izin vermiyor gibi `UnitPrice` 1234,56 değerleri. Öğesinden sonra `BatchUpdate.aspx`, bir dizi kaydı, bir ürünün s emin Düzenle `UnitPrice` Yasak değere (1234,56) değeri. Update ürünleri, toplu işlem sırasında diğer değişikliklerle tıklayarak özgün değerlerine döndürülmesine olduğunda bu hataya neden olur.

## <a name="an-alternativebatchupdatemethod"></a>Alternatif`BatchUpdate`yöntemi

`BatchUpdate` Biz yalnızca yöntemi incelenirken alır *tüm* BLL s ürünlerin `GetProducts` yöntemi ve ardından GridView içinde görünen yalnızca kayıtları güncelleştirir. Bu yaklaşım, disk belleği GridView kullanmaz, ancak aşması durumunda olabilir yüzlerce, binlerce veya on binlerce ürünleri, ancak yalnızca on satır GridView idealdir. Böyle bir durumda, tüm ürünlerin ve bunların 10 tanesinin yalnızca değiştirmek için veritabanından alma küçüktür idealdir.

Bu tür durumlar için aşağıdaki kullanmayı `BatchUpdateAlternate` yöntemi bunun yerine:


[!code-csharp[Main](batch-updating-cs/samples/sample7.cs)]

`BatchMethodAlternate` Yeni bir boş oluşturarak başlar `ProductsDataTable` adlı `products`. Ardından GridView s adımlarını `Rows` koleksiyonu ve BLL s kullanarak belirli bir ürün bilgileri her bir satır alır için `GetProductByProductID(productID)` yöntemi. Alınan `ProductsRow` örneğine sahip aynı şekilde güncelleştirilmiş özelliklerini `BatchUpdate`, ancak satır içine alınır güncelleştirdikten sonra `products``ProductsDataTable` DataTable s aracılığıyla [ `ImportRow(DataRow)` yöntemi](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Sonra `foreach` döngüsü tamamlandıktan `products` içerir `ProductsRow` GridView her satır için örneği. Her biri bu yana `ProductsRow` örnekleri eklenmiştir `products` (yerine güncelleştirilmiş), biz körüne geçirin, `UpdateWithTransaction` yöntemi `ProductsTableAdatper` veritabanına kayıtların her birinde eklemeye çalışacaktır. Bunun yerine, ki bu satırların her biri (eklenmez) değiştirildiğini belirtmeniz gerekir.

Bu yeni bir yöntem adlı BLL ekleyerek gerçekleştirilebilir `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, aşağıda gösterilen kümeleri `RowState` her birinin `ProductsRow` içinde örnekler `ProductsDataTable` için `Modified` geçirir `ProductsDataTable` DAL s `UpdateWithTransaction` yöntemi.


[!code-csharp[Main](batch-updating-cs/samples/sample8.cs)]

## <a name="summary"></a>Özet

GridView satır başına yerleşik düzenleme yetenekleri sağlar, ancak tam olarak düzenlenebilir arabirimleri oluşturmak için bunlara dönülemiyor. Biz bu öğreticide gördüğünüz gibi bu tür arabirimleri mümkündür, ancak iş bir bit gerektirir. Her satır olduğu düzenlenebilir GridView oluşturmak için GridView s alanları TemplateField dönüştürmek ve düzenleme arabiriminden tanımlamak ihtiyacımız `ItemTemplate` s. Ayrıca, güncelleştirme tüm - düğmesi Web denetimleri türü GridView ayrı sayfaya eklenmesi gerekir. Bu düğmeler `Click` GridView s listeleme gereken olay işleyicileri `Rows` koleksiyonu değişiklikleri depolamak bir `ProductsDataTable`ve güncelleştirilmiş bilgileri uygun BLL metoduna geçirin.

Sonraki öğreticide batch silmek için bir arabirim oluşturma göreceğiz. Her GridView satır özellikle ve bir onay kutusu eklemek yerine tüm güncelleştirme-yazın düğmeleri, biz seçili satırları sil düğmeleri sahip olacaksınız.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Teresa Murphy ve David Suru yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](wrapping-database-modifications-within-a-transaction-cs.md)
> [İleri](batch-deleting-cs.md)
