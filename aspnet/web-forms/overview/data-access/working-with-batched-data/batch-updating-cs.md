---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
title: "Toplu güncelleştirme (C#) | Microsoft Docs"
author: rick-anderson
description: "Tek bir işlemde birden çok veritabanı kayıtlarını güncelleştirmek üzere öğrenin. Kullanıcı arabirimi katmanda her satır düzenlenebilir olduğu GridView oluşturun. Veri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4e849bcc-c557-4bc3-937e-f7453ee87265
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
msc.type: authoredcontent
ms.openlocfilehash: 506ecc9fad47cc39a0323e9ed18814c26e28ee47
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="batch-updating-c"></a>Toplu güncelleştirme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_CS.zip) veya [PDF indirin](batch-updating-cs/_static/datatutorial64cs1.pdf)

> Tek bir işlemde birden çok veritabanı kayıtlarını güncelleştirmek üzere öğrenin. Kullanıcı arabirimi katmanda her satır düzenlenebilir olduğu GridView oluşturun. Veri erişim katmanı'ndaki tüm güncelleştirmeleri başarılı veya tüm güncelleştirmeler geri emin olmak için bir işlem içinde birden çok güncelleştirme işlemleri alın.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](wrapping-database-modifications-within-a-transaction-cs.md) veritabanı işlemleri için destek eklemek için veri erişim katmanı genişletmek nasıl gördük. Veritabanı işlemleri güvence altına veri değişikliği deyimleri bir dizi değişikliklerden başarısız olur veya tüm başarılı olur sağlayan bir atomik işlem olarak kabul edilir. Bu alt düzey DAL işlevleri ile göz önünden, biz re toplu veri değişikliği arabirimler oluşturma için uygulamamızla etkinleştirmek için hazır.

Bu öğreticide biz her satır düzenlenebilir (bkz: Şekil 1) olduğu GridView yapı. Her satır bir sütunu Düzenle gerek yoktur düzenleme arabirimi içinde orada s işlenmeden olduğundan, Güncelleştir ve İptal düğmeleri. Bunun yerine, iki güncelleştirme ürünleri düğme vardır sayfasında, tıklatıldığında GridView satırları numaralandırmak ve veritabanını güncelleyin.


[![Her satırda bir GridView düzenlenebilir değil](batch-updating-cs/_static/image1.gif)](batch-updating-cs/_static/image1.png)

**Şekil 1**: GridView her satır olacak düzenlenebilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image2.png))


Let s başlayın!

> [!NOTE]
> İçinde [gerçekleştirme toplu güncelleştirmeler](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) oluşturduğumuz toplu düzenleme öğretici arabirim DataList denetimini kullanma. Bu öğretici, birinde kullanımdır GridView farklıdır ve toplu güncelleştirme bir işlem kapsamı içinde gerçekleştirilir. Bu öğreticiyi tamamladıktan sonra ı önceki öğreticiye geri dönün ve önceki öğreticide eklenen veritabanı işlem ilgili işlevselliğini kullanacak şekilde güncelleştirmek için öneririz.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Tüm GridView satırları düzenlenebilir yapma adımları inceleniyor

' Da anlatıldığı gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) öğretici, GridView, satır başına temelinde temel alınan verileri düzenleme için yerleşik destek sunar. Dahili olarak, hangi satır aracılığıyla düzenlenebilir GridView Notlar kendi [ `EditIndex` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). GridView kendi veri kaynağına bağlı olarak, her satır satırın dizini değerini eşitse görmek için denetler `EditIndex`. Bu durumda, o satırdaki alanları kendi düzenleme kullanılarak işlenir s arabirimleri. BoundFields için düzenleme TextBox arabirimidir olan `Text` özelliği BoundField s tarafından belirtilen veri alanının değeri atanan `DataField` özelliği. TemplateFields için `EditItemTemplate` yerine kullanılan `ItemTemplate`.

Bir kullanıcı bir satır s Düzenle düğmesine tıkladığında düzenleme iş akışı başlayacağını geri çağırma. Bu geri gönderimin neden olur, GridView s ayarlar `EditIndex` özelliğini tıklatılan satır s dizini ve rebinds kılavuza veri. Ne zaman bir satır s iptal düğmesine tıklandığında, geri göndermede `EditIndex` değerine ayarlanmış `-1` kılavuza veri kira önce. GridView s satırları sıfırda Dizinlemeyi Başlat olduğundan, ayar `EditIndex` için `-1` GridView salt okunur modda görüntüleme etkisi vardır.

`EditIndex` Özelliği satır içi düzenleme için iyi çalışır, ancak toplu düzenleme için tasarlanmamıştır. Tüm GridView düzenlenebilir yapmak için kimliğinizi düzenleme arabirimini kullanarak işlemek her satır olması gerekiyor. Bunu yapmanın en kolay yolu TemplateField düzenleme arabirimi ile tanımlandığı şekilde her düzenlenebilir bir alanı burada uygulanan oluşturmaktır `ItemTemplate`.

Sonraki birkaç adım tamamen düzenlenebilir GridView oluşturacağız. 1. adımda biz GridView ve onun ObjectDataSource oluşturarak başlayın ve kendi BoundFields ve CheckBoxField TemplateFields dönüştürme. Adım 2 ve 3'te biz düzenleme arabirimleri TemplateFields taşırsınız `EditItemTemplate` s kendi `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>1. adım: Ürün bilgilerini görüntüleme

GridView oluşturma hakkında endişelenmeniz önce satırları nerede düzenlenebilir, ürün bilgilerini görüntüleyerek Başlat s olanak tanır. Açık `BatchUpdate.aspx` sayfasındaki `BatchData` klasörü ve araç tasarımcıya GridView sürükleyin. GridView s ayarlamak `ID` için `ProductsGrid` ve adlı yeni bir ObjectDataSource bağlamak akıllı etiketten seçin `ProductsDataSource`. Kendi verilerin alınacağı ObjectDataSource yapılandırma `ProductsBLL` s sınıfı `GetProducts` yöntemi.


[![ObjectDataSource ProductsBLL sınıfını kullanmak için yapılandırma](batch-updating-cs/_static/image2.gif)](batch-updating-cs/_static/image3.png)

**Şekil 2**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image4.png))


[![GetProducts yöntemini kullanarak ürün verilerini alma](batch-updating-cs/_static/image3.gif)](batch-updating-cs/_static/image5.png)

**Şekil 3**: Ürün kullanarak verileri almak `GetProducts` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image6.png))


GridView gibi ObjectDataSource s değişiklik özelliklerini bir satır başına temelinde çalışmak üzere tasarlanmıştır. Bir kayıt kümesini güncelleştirmek için şu verileri toplu işlemleri ve BLL aktaran ASP.NET sayfası s arka plandaki kod sınıfı biraz kod yazmaya gerekir. Bu nedenle, açılan listeleri ObjectDataSource s güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın. Sihirbazı tamamlamak için Son'u tıklatın.


[![Güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) silme](batch-updating-cs/_static/image4.gif)](batch-updating-cs/_static/image7.png)

**Şekil 4**: aşağı açılan listeler güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image8.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra ObjectDataSource s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](batch-updating-cs/samples/sample1.aspx)]

Veri Kaynağı Yapılandırma Sihirbazı Tamamlanıyor ayrıca BoundFields ve ürün veri alanları için CheckBoxField GridView oluşturmak Visual Studio neden olur. Bu öğreticide, yalnızca ürün adı, kategori, fiyat ve devam etmeyen durumunu görüntüleyin ve düzenleyin kullanıcıya izin ver s olanak tanır. Kaldırma dışındaki tüm `ProductName`, `CategoryName`, `UnitPrice`, ve `Discontinued` alanları ve yeniden adlandırma `HeaderText` ilk üç özelliklerini ürün, kategori ve fiyat, sırasıyla alanları. Son olarak, GridView s akıllı etiket etkinleştirmek disk belleği ve etkinleştirme sıralama onay kutularını kontrol edin.

Bu noktada GridView üç BoundFields sahip (`ProductName`, `CategoryName`, ve `UnitPrice`) ve bir CheckBoxField (`Discontinued`). Bu dört alanları TemplateFields dönüştürün ve ardından düzenleme arabirimi TemplateField s'den taşımak ihtiyacımız `EditItemTemplate` için kendi `ItemTemplate`.

> [!NOTE]
> Biz oluşturma ve TemplateFields içinde özelleştirme incelediniz [veri değişikliği arabirimi özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) Öğreticisi. Biz BoundFields ve CheckBoxField TemplateFields dönüştürme adımlarda size yol ve bunların düzenleme tanımlama arabirimleri kendi `ItemTemplate` s, ancak takılmış veya Yenileyici, tan t önceki Bu öğreticinin geri başvurmak için istemeyebilir.


GridView s akıllı etiketten alanları iletişim kutusunu açmak için sütunları Düzenle bağlantısına tıklayın. Ardından, her bir alan seçin ve bu alan dönüştürme TemplateField bağlantıya tıklayın.


![Varolan BoundFields ve CheckBoxField TemplateField dönüştürme](batch-updating-cs/_static/image5.gif)

**Şekil 5**: Varolan BoundFields ve CheckBoxField TemplateField dönüştürme


Her bir alan bir TemplateField eder, gelen biz düzenleme taşımak için hazır re arabirim `EditItemTemplate` s `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>2. adım: Oluşturma`ProductName`,`UnitPrice`, ve`Discontinued`arabirimleri düzenleme

Oluşturma `ProductName`, `UnitPrice`, ve `Discontinued` arabirimleri düzenleme bu adımın konu ve her bir arabirime zaten TemplateField s içinde tanımlanan oldukça basit `EditItemTemplate`. Oluşturma `CategoryName` arabirimini düzenleme biraz daha karmaşık DropDownList geçerli kategorilerin oluşturmak gerektiği. Bu `CategoryName` arabirimini düzenleme adım 3'te tackled.

S başlamalı ve izin `ProductName` TemplateField. GridView s akıllı etiket Şablonları Düzenle bağlantısından tıklayın ve detayına gitmek `ProductName` TemplateField s `EditItemTemplate`. TextBox seçin, panoya kopyalayın ve yapıştırın kendisine `ProductName` TemplateField s `ItemTemplate`. TextBox s değiştirme `ID` özelliğine `ProductName`.

Ardından, bir RequiredFieldValidator eklemek `ItemTemplate` kullanıcı her ürün s adı için bir değer sağlar emin olmak için. Ayarlama `ControlToValidate` ProductName, özelliğine `ErrorMessage` size özelliği ürün adı sağlamanız gerekir. ve `Text` özelliğine \*. Bu eklemeler yaptıktan sonra `ItemTemplate`, ekranınızın Şekil 6 benzer görünmelidir.


[![Metin kutusu ve bir RequiredFieldValidator ProductName TemplateField şimdi içerir](batch-updating-cs/_static/image6.gif)](batch-updating-cs/_static/image9.png)

**Şekil 6**: `ProductName` TemplateField şimdi içeren bir metin kutusu ve RequiredFieldValidator bir ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image10.png))


İçin `UnitPrice` arabirimini düzenleme, metin kopyalayarak Başlat `EditItemTemplate` için `ItemTemplate`. Ardından, $ TextBox ve kümesi önüne yerleştirin, `ID` UnitPrice özelliğine ve kendi `Columns` 8 özelliği.

Ayrıca bir CompareValidator ekleyin `UnitPrice` s `ItemTemplate` kullanıcı tarafından girilen değer geçerli para birimi değeri $0,00 eşit veya daha büyük olduğundan emin olmak için. Doğrulayıcı s ayarlamak `ControlToValidate` UnitPrice, özelliğine kendi `ErrorMessage` , özelliğine geçerli para birimi değeri girmelisiniz. Lütfen atlayın. herhangi bir para birimi sembolleri, kendi `Text` özelliğine \*, kendi `Type` özelliğine `Currency`, kendi `Operator` özelliğine `GreaterThanEqual`ve kendi `ValueToCompare` özelliğinin 0.


[![Negatif olmayan para birimi değeri fiyat girilen emin olmak için bir CompareValidator ekleyin](batch-updating-cs/_static/image7.gif)](batch-updating-cs/_static/image11.png)

**Şekil 7**: girilen fiyat emin olmak için bir CompareValidator negatif olmayan para birimi değeri ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image12.png))


İçin `Discontinued` TemplateField kullanabileceğiniz zaten tanımlanmış onay kutusunu `ItemTemplate`. Basitçe, `ID` Üretimi Durdurulmuş için ve kendi `Enabled` özelliğine `true`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>3. adım: Oluşturma`CategoryName`arabirimini düzenleme

Düzenleme arabiriminde `CategoryName` TemplateField s `EditItemTemplate` değerini gösteren bir metin kutusu içeren `CategoryName` veri alanı. Bu olası kategorilerini listeler bir DropDownList ile değiştirmeniz gerekir.

> [!NOTE]
> [Veri değişikliği arabirimi özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) öğretici içeren metin kutusu aksine DropDownList dahil etmek için bir şablonu özelleştirme hakkında daha kapsamlı ve eksiksiz bir tartışma. Adımlar burada tam olsa da, bunlar tersely sunulur. Oluşturma ve kategorilere DropDownList yapılandırma bir daha derinlemesine bakış için geri başvurmak [veri değişikliği arabirimi özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) Öğreticisi.


Araç kutusu üzerine bir DropDownList sürükleyin `CategoryName` TemplateField s `ItemTemplate`, ayar kendi `ID` için `Categories`. Bu noktada biz genellikle kendi akıllı etiket üzerinden DropDownLists s veri kaynağına yeni ObjectDataSource oluşturma tanımlayın. Ancak, bu içinde ObjectDataSource ekler `ItemTemplate`, her GridView satır için oluşturulan ObjectDataSource örneğindeki neden. Bunun yerine, GridView s TemplateFields dışında ObjectDataSource oluşturmak s olanak tanır. Şablon düzenleme sonlandırmak ve bir ObjectDataSource tasarımcıya araç çubuğuna sürükleyin `ProductsDataSource` ObjectDataSource. Yeni ObjectDataSource ad `CategoriesDataSource` ve kullanacak şekilde yapılandırın `CategoriesBLL` s sınıfı `GetCategories` yöntemi.


[![ObjectDataSource CategoriesBLL Clas kullanacak şekilde yapılandırma](batch-updating-cs/_static/image8.gif)](batch-updating-cs/_static/image13.png)

**Şekil 8**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` Clas ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image14.png))


[![GetCategories yöntemini kullanarak kategori verilerini alma](batch-updating-cs/_static/image9.gif)](batch-updating-cs/_static/image15.png)

**Şekil 9**: Kategori kullanarak verileri almak `GetCategories` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image16.png))


Bu ObjectDataSource yalnızca verileri almak için kullanıldığından, aşağı açılır listeler UPDATE ve DELETE sekmeler (hiçbiri) olarak ayarlayın. Sihirbazı tamamlamak için Son'u tıklatın.


[![Ayarlama güncelleştirme ve silme sekmelerde (hiçbiri) aşağı açılır listeler](batch-updating-cs/_static/image10.gif)](batch-updating-cs/_static/image17.png)

**Şekil 10**: aşağı açılan listeler güncelleştirme ve silme sekmeler (yok) olarak ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image18.png))


Sihirbazı tamamladıktan sonra `CategoriesDataSource` s bildirim temelli biçimlendirme, aşağıdaki gibi görünmelidir:


[!code-aspx[Main](batch-updating-cs/samples/sample2.aspx)]

İle `CategoriesDataSource` oluşturduktan ve yapılandırdıktan, geri dönüp `CategoryName` TemplateField s `ItemTemplate` ve DropDownList s akıllı etiketten veri kaynağı Seç bağlantıyı tıklatın. Veri Kaynağı Yapılandırma Sihirbazı'nda seçin `CategoriesDataSource` seçeneğini ilk açılan listeden ve tercih `CategoryName` görüntülemek için kullanılan ve `CategoryID` değeri olarak.


[![DropDownList CategoriesDataSource bağlama](batch-updating-cs/_static/image11.gif)](batch-updating-cs/_static/image19.png)

**Şekil 11**: DropDownList bağlamak `CategoriesDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image20.png))


Bu noktada `Categories` DropDownList tüm kategorilerini listeler, ancak değil henüz otomatik olarak GridView satıra bağlı ürün için uygun kategoriyi seçin. Bunu ayarlamak için ihtiyacımız gerçekleştirmek için `Categories` DropDownList s `SelectedValue` s ürününe `CategoryID` değeri. DropDownList s akıllı etiket veri bağlamaları Düzenle bağlantısından tıklayın ve ilişkilendirmek `SelectedValue` özelliğiyle `CategoryID` veri alan Şekil 12'de gösterildiği gibi.


![Ürün s adlı kullanıcı, Categoryıd'si değeri DropDownList s SelectedValue özelliği Bağla](batch-updating-cs/_static/image12.gif)

**Şekil 12**: s ürün bağlamak `CategoryID` DropDownList s değerine `SelectedValue` özelliği


Bir son sorun kalır: Ürün içermiyor t varsa bir `CategoryID` değeri belirtilen sonra Veri bağlamada bildirimi `SelectedValue` bir özel durum neden olur. DropDownList yalnızca kategorileri öğelerini içerir ve bir seçenek olan bu ürünler için sunmaz Bunun nedeni bir `NULL` veritabanı için değer `CategoryID`. Bu sorunu gidermek için DropDownList s ayarlamak `AppendDataBoundItems` özelliğine `true` ve DropDownList için yeni bir öğe eklemek atlama `Value` Tanımlayıcı Sözdizimi özelliğinden. Diğer bir deyişle, olduğundan emin olun `Categories` DropDownList s tanımlayıcı sözdizimi aşağıdaki gibi görünür:


[!code-aspx[Main](batch-updating-cs/samples/sample3.aspx)]

Not nasıl `<asp:ListItem Value="">` --seçin bir--sahip kendi `Value` özniteliğini açıkça boş bir dize olarak ayarlayın. Geri başvurmak [veri değişikliği arabirimi özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) işlemek için bu ek DropDownList madde neden gerekli üzerinde daha kapsamlı bir tartışma için öğretici `NULL` çalışması ve neden atamasının `Value` boş bir dize özelliği gereklidir.

> [!NOTE]
> Tümleştirilmediği olan bir olası performans ve ölçeklenebilirlik sorunu burada yoktur. Her satır kullanan bir DropDownList olduğundan `CategoriesDataSource` kendi veri kaynağı olarak `CategoriesBLL` s sınıfı `GetCategories` yöntemi çağrılabilir  *n*  sayfa başına kez ziyaret edin, burada  *n*  GridView satır sayısı. Bunlar  *n*  çağrılar `GetCategories` neden  *n*  veritabanına sorgular. İstek başına önbelleğinde veya bağımlılık veya çok kısa bir süre tabanlı bir sona erme önbelleğe alma SQL kullanarak önbelleğe alma katmanı ile döndürülen kategorilerde önbelleğe alarak Bu etkiyi veritabanında kısılmış. İstek başına hakkında daha fazla bilgi için bkz, önbelleğe [ `HttpContext.Items` başına istek önbelleği deposu](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>4. adım: düzenleme arabirimi Tamamlanıyor

Biz ullanıcı yapılan değişiklik sayısı GridView s şablonların bizim ilerleme durumunu görüntülemek için duraklatmadan. Bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Şekil 13 gösterildiği gibi her satır kullanılarak işlenir kendi `ItemTemplate`, arabirimini düzenleme hücre s içerir.


[![Düzenlenebilir her GridView satırıdır](batch-updating-cs/_static/image13.gif)](batch-updating-cs/_static/image21.png)

**Şekil 13**: düzenlenebilir her GridView satırıdır ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image22.png))


Bu noktada ilgilenebilmek birkaç Önemsiz biçimlendirme sorunları vardır. İlk olarak, unutmayın `UnitPrice` değeri dört ondalık basamak içerir. Bu sorunu gidermek için dönmek `UnitPrice` TemplateField s `ItemTemplate` ve TextBox s akıllı etiketten veri bağlamaları Düzenle bağlantısına tıklayın. Ardından, belirten `Text` özelliği bir sayı olarak biçimlendirilmelidir.


![Bir sayı olarak format metin özelliği](batch-updating-cs/_static/image14.gif)

**Şekil 14**: biçimi `Text` bir sayı olarak özelliği


İkinci olarak, onay kutusuna merkezi let s `Discontinued` sütun (yerine onu sola hizalı). GridView s akıllı etiketten Düzenle sütunlarda tıklayın ve `Discontinued` TemplateField sol alt köşedeki alanlarında listesinden. İçinde ayrıntıya `ItemStyle` ve `HorizontalAlign` Şekil 15'te gösterildiği gibi Merkezi özelliğine.


![Merkezi devam etmeyen onay kutusu](batch-updating-cs/_static/image15.gif)

**Şekil 15**: Merkezi `Discontinued` onay kutusu


Ardından, bir ValidationSummary denetimi sayfasına ekleyin ve ayarlayın, `ShowMessageBox` özelliğine `true` ve kendi `ShowSummary` özelliğine `false`. Ayrıca tıklatıldığında düğmesi Web, denetimleri ekleme, kullanıcı s değişiklikleri güncelleştirir. Özellikle, iki düğmesi Web denetimleri, GridView yukarıdaki ve her iki denetimleri ayarlama bir altındaki eklemeniz `Text` güncelleştirme ürünleri özellikleri.

Bu yana GridView s arabirimini düzenleme kendi TemplateFields tanımlanan `ItemTemplate` s, `EditItemTemplate` s gereksiz ve silinmiş.

Yukarıdaki yapmadan biçimlendirme değişiklikleri bahsedilen sonra düğmesi denetimleri ekleme ve gereksiz kaldırma `EditItemTemplate` s, sayfa s bildirim temelli söz dizimini görünmelidir aşağıdakine benzer:


[!code-aspx[Main](batch-updating-cs/samples/sample4.aspx)]

Şekil 16 düğmesi Web denetimleri ekledikten sonra bir tarayıcıdan görüntülendiğinde bu sayfayı ve biçimlendirme değişikliklerinin gösterir.


[![Sayfa şimdi iki güncelleştirme ürünleri düğmelerini içerir](batch-updating-cs/_static/image16.gif)](batch-updating-cs/_static/image23.png)

**Şekil 16**: sayfa şimdi içeren iki güncelleştirme ürünleri düğmelerini ([tam boyutlu görüntüyü görüntülemek için tıklatın](batch-updating-cs/_static/image24.png))


## <a name="step-5-updating-the-products"></a>5. adım: ürünleri güncelleştiriliyor

Bir kullanıcı bu sayfayı ziyaret ettiğinde bunlar kendi değişiklikleri yapın ve sonra iki güncelleştirme ürünleri düğmeleri birini tıklatın. Bu noktada şekilde her bir satır için kullanıcı tarafından girilen değerlerini kaydetmek ihtiyacımız bir `ProductsDataTable` örneği ve daha sonra, ardından geçer BLL yönteme geçirin `ProductsDataTable` DAL s örneğine `UpdateWithTransaction` yöntemi. `UpdateWithTransaction` İçinde oluşturduğumuz yöntemi [önceki öğretici](wrapping-database-modifications-within-a-transaction-cs.md), toplu değişiklikler atomik bir işlem olarak güncelleştirilecek sağlar.

Adlı bir yöntem oluşturma `BatchUpdate` içinde `BatchUpdate.aspx.cs` ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](batch-updating-cs/samples/sample5.cs)]

Bu yöntem ürünlerin tamamı alarak başlar geri bir `ProductsDataTable` BLL s çağrısıyla `GetProducts` yöntemi. Ardından numaralandırır `ProductGrid` GridView s [ `Rows` koleksiyonu](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). `Rows` Koleksiyonu içeren bir [ `GridViewRow` örneği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridviewrow.aspx) GridView görüntülenen her satır için. Biz sayfasında, GridView s başına en fazla on satır gösteren beri `Rows` koleksiyonu, en fazla on öğe olacaktır.

Her satır için `ProductID` gelen gerçekleşti `DataKeys` toplama ve uygun `ProductsRow` seçilir `ProductsDataTable`. Dört TemplateField giriş denetimlerini programlı olarak başvurulan ve değerlerine atandığı `ProductsRow` örnek s özellikleri. Sonra her GridView satır s değerleri güncelleştirmek için kullanılan `ProductsDataTable`, onu s geçirilen BLL s `UpdateWithTransaction` önceki öğreticide gördüğümüz gibi sadece DAL s çağırır yöntemi `UpdateWithTransaction` yöntemi.

Bu öğretici için toplu güncelleştirme algoritmadan her satır güncelleştirmeleri `ProductsDataTable` karşılık gelen ürün s bilgileri olup değişti bağımsız olarak GridView, bir satır. Bu tür blind geçekleştirilen t genellikle bir performans sorunu güncelleştirirken, Denetim yapıldığı veritabanı tablosuna değişirse bunlar gereksiz kayıtları yol açabilir. Geri [gerçekleştirme toplu güncelleştirmeler](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) arabirimi ile DataList güncelleştiriliyor bir batch incelediniz ve kullanıcı tarafından gerçekten değiştirilen kayıtları yalnızca güncelleştirecektir kod eklenen öğretici. Teknikleri çekinmeyin [gerçekleştirme toplu güncelleştirmeler](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) isterseniz bu öğreticide, kod güncelleştirilecek.

> [!NOTE]
> GridView, akıllı etiket aracılığıyla veri kaynağına bağlanırken, Visual Studio veri kaynağı s birincil anahtar değerlerinin GridView s otomatik olarak atar. `DataKeyNames` özelliği. ObjectDataSource GridView s akıllı etiket aracılığıyla GridView için 1. adımda özetlenen bağlanamadı sonra GridView s el ile ayarlamanız gerekir `DataKeyNames` erişmek için ProductID özelliğine `ProductID` her satır için değer `DataKeys` koleksiyonu.


İçinde kullanılan kod `BatchUpdate` BLL s kullanılan benzer `UpdateProduct` yöntemleri, giriş olan temel fark `UpdateProduct` yöntemleri yalnızca tek bir `ProductRow` örneği mimarisinden alınır. Özelliklerini atar kod `ProductRow` arasında aynı `UpdateProducts` yöntemler ve kod içinde `foreach` içinde döngü `BatchUpdate`genel deseni gibi.

Bu öğreticiyi tamamlamak için ihtiyacımız `BatchUpdate` yöntemi çağrıldıktan güncelleştirme ürünleri düğmelerin ya da tıklandığında. Olay işleyicileri oluşturma `Click` bu iki olayları düğmesi denetimleri ve olay işleyicileri aşağıdaki kodu ekleyin:


[!code-csharp[Main](batch-updating-cs/samples/sample6.cs)]

İlk için bir çağrı yapılır `BatchUpdate`. Ardından, `ClientScript property` ürünleri güncelleştirilmiş okuyan messagebox görüntüler JavaScript eklemesine kullanılır.

Bu kodu test etmek için bir dakikanızı ayırın. Ziyaret `BatchUpdate.aspx` bir tarayıcı aracılığıyla satır sayısını düzenleyin ve güncelleştirme ürünleri düğmelerden birini tıklatın. Giriş doğrulaması hatalar yoktur varsayıldığında, güncelleştirilen ürünler okuyan messagebox görmeniz gerekir. Güncelleştirme kararlılık doğrulamak için bir rastgele eklemeyi düşünün `CHECK` gibi izin vermez bir kısıtlama `UnitPrice` 1234.56 değerleri. Öğesinden sonra `BatchUpdate.aspx`, birkaç s ürün ayarlayın emin kayıtların Düzenle `UnitPrice` değerini yasaklanmış değerle (1234.56). Bu toplu işlem sırasında diğer değişikliklerle güncelleştirme ürünleri tıklatarak özgün değerlerine döndürülmesine olduğunda bu hataya neden olur.

## <a name="an-alternativebatchupdatemethod"></a>Alternatif`BatchUpdate`yöntemi

`BatchUpdate` Biz yalnızca yöntemi incelenmesi alır *tüm* BLL s ürünlerin `GetProducts` yöntemi ve GridView görünür yalnızca bu kayıtları güncelleştirir. Bu yaklaşım GridView disk belleği kullanmaz, ancak aşması durumunda olabilir yüzlerce, binlerce ya da binlerce ürünleri, ancak yalnızca on satırlarda GridView idealdir. Böyle bir durumda, tüm ürünleri yalnızca 10 bunları değiştirmek için veritabanından alma değerinden idealdir.

Bu durumlarda türleri için aşağıdaki kullanmayı `BatchUpdateAlternate` yöntemi bunun yerine:


[!code-csharp[Main](batch-updating-cs/samples/sample7.cs)]

`BatchMethodAlternate`Yeni ve boş bir oluşturarak başlar `ProductsDataTable` adlı `products`. Ardından GridView s adımlara `Rows` koleksiyonu ve her satırın BLL s kullanarak belirli bir ürün bilgilerini alır için `GetProductByProductID(productID)` yöntemi. Alınan `ProductsRow` örneği sahip aynı şekilde güncelleştirilmiş özelliklerini `BatchUpdate`, ancak alınan içine satır güncelleştirdikten sonra `products``ProductsDataTable` DataTable s aracılığıyla [ `ImportRow(DataRow)` yöntemi](https://msdn.microsoft.com/en-us/library/system.data.datatable.importrow(VS.80).aspx).

Sonra `foreach` döngüsü tamamlandıktan, `products` içeriyor `ProductsRow` GridView her satır için örneği. Her biri bu yana `ProductsRow` örnekleri eklenmiştir `products` (yerine güncelleştirilmiş), sizi doğrudan kendisine başarılı olursa `UpdateWithTransaction` yöntemi `ProductsTableAdatper` her kayıtlar veritabanına eklemek çalışacaktır. Bunun yerine, biz bu satırların her biri (eklenmedi) değiştirilmiş olduğunu belirtmeniz gerekir.

Bu yeni bir yöntem adlı BLL ekleyerek gerçekleştirilebilir `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, aşağıda gösterilen kümeleri `RowState` her birinin `ProductsRow` içinde örnekleri `ProductsDataTable` için `Modified` ve daha sonra geçirir `ProductsDataTable` DAL s `UpdateWithTransaction` yöntemi.


[!code-csharp[Main](batch-updating-cs/samples/sample8.cs)]

## <a name="summary"></a>Özet

GridView, satır başına yerleşik düzenleme yetenekleri sağlar ancak tam olarak düzenlenebilir arabirimler oluşturma desteği eksik. Biz bu öğreticide gördüğünüz gibi bu tür arabirimleri mümkündür, ancak iş bir bit gerektirir. Her satır olduğu düzenlenebilir GridView oluşturmak için GridView s alanları TemplateFields dönüştürmek ve düzenleme arabirimde tanımlamak ihtiyacımız `ItemTemplate` s. Ayrıca, güncelleştirme tüm - düğmesi Web denetimleri türü sayfasında, GridView ayrı eklenmelidir. Bu düğme `Click` olay işleyicileri gereksinim GridView s Numaralandırılacak `Rows` koleksiyonu, değişiklikleri saklamak bir `ProductsDataTable`ve güncelleştirilmiş bilgileri uygun BLL yönteme geçirin.

Sonraki öğreticide toplu silme için bir arabirim oluşturma göreceğiz. Her GridView satır özellikle ve bir onay kutusu eklemek yerine Tümünü Güncelleştir-yazın düğmeleri, seçili satırları silme düğmeleri sahip olacaksınız.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Teresa Murphy ve David Suru yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](wrapping-database-modifications-within-a-transaction-cs.md)
[sonraki](batch-deleting-cs.md)
