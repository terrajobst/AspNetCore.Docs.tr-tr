---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: Veri değişikliği arabirimi (C#) özelleştirme | Microsoft Docs
author: rick-anderson
description: Bu öğreticide standart TextBox değiştirerek düzenlenebilir bir GridView arabirimini özelleştirmek nasıl inceleyeceğiz ve onay kutusu denetimleri ile alternati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: f25b265c50870d59721a94c01d78f589a5d5f1c3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="customizing-the-data-modification-interface-c"></a>Veri değişikliği arabirimi (C#) özelleştirme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe) veya [PDF indirin](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> Bu öğreticide standart TextBox değiştirerek düzenlenebilir bir GridView arabirimini özelleştirmek nasıl inceleyeceğiz ve onay kutusu alternatif giriş Web denetimleriyle denetler.


## <a name="introduction"></a>Giriş

GridView ve DetailsView denetimleri tarafından kullanılan CheckBoxFields ve BoundFields salt okunur, düzenlenebilir ve belirten gösterge arabirimleri işlemek için kendi yeteneği nedeniyle veri değiştirme işlemini basitleştirebilir. Bu arabirimleri, herhangi bir ek bildirim temelli işaretleme veya kod eklemeye gerek kalmadan oluşturulabilir. Ancak, BoundField ve CheckBoxField'ın arabirimleri genellikle gerçek dünya senaryolarında gerekli özelleştirilebilirliğini yoksundur. GridView veya DetailsView düzenlenebilir veya belirten gösterge arabirimini özelleştirmek için bunun yerine bir TemplateField kullanmamız gerekiyor.

İçinde [önceki öğretici](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) doğrulama Web denetimler ekleyerek veri değişikliği arabirimleri özelleştirmeyi gördük. Bu öğreticide BoundField ve CheckBoxField'ın standart TextBox değiştirerek, gerçek veri toplama Web denetimleri ve onay kutusu denetimleri alternatif giriş Web denetimleri ile nasıl özelleştirileceği inceleyeceğiz. Özellikle, biz bir ürün adı, kategori, tedarikçi ve devam etmeyen durumu güncelleştirilmesi izin veren bir düzenlenebilir GridView yapı. Belirli bir satır düzenlerken, kategori ve tedarikçi alanlar kullanılabilir kategoriler ve aralarından seçim yapabileceğiniz Üreticiler kümesini içeren DropDownLists kabul eder. Ayrıca, size iki seçenek sunar RadioButtonList denetimi ile CheckBoxField'ın varsayılan onay kutusunu değiştirirsiniz: "Etkin" ve "Dönüştürülmesine".


[![GridView'ın düzenleme arabirimi DropDownLists ve RadioButtons içerir](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**Şekil 1**: GridView'ın arabirimi içerir DropDownLists düzenleme ve RadioButtons ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>1. adım: uygun oluşturma`UpdateProduct`aşırı yükleme

Bu öğreticide biz düzenleme bir ürün adı, kategori, tedarikçi verir ve durum dönüştürülmesine düzenlenebilir bir GridView oluşturacaksınız. Bu nedenle, ihtiyacımız bir `UpdateProduct` bu dört ürün değerleri beş giriş parametreleri kabul aşırı artı `ProductID`. Bizim önceki içinde aşırı, bu bir işlem gibi:

1. Veritabanı için belirtilen ürün bilgisini almak `ProductID`,
2. Güncelleştirme `ProductName`, `CategoryID`, `SupplierID`, ve `Discontinued` alanları ve
3. TableAdapter'ın aracılığıyla DAL güncelleştirme isteği gönderin `Update()` yöntemi.

Bu belirli aşırı yüklemesi için ı dönüştürülmesine olarak işaretlenen bir ürün, sağlayıcı tarafından sunulan yalnızca ürün değil sağlar iş kuralı onay verilmemiştir. Kullanımında tercih ederseniz içinde eklemek boş ya da ideal olarak, ayrı bir yöntem için mantığı çıkışı düzenleme.

Aşağıdaki kod yeni gösterir `UpdateProduct` içinde aşırı `ProductsBLL` sınıfı:


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>2. adım: düzenlenebilir GridView hazırlayın

İle `UpdateProduct` aşırı eklenen, biz bizim düzenlenebilir GridView oluşturmak için hazır. Açık `CustomizedUI.aspx` sayfasındaki `EditInsertDelete` klasörü ve GridView denetimini Designer'a ekleyin. Ardından, yeni ObjectDataSource GridView'ın akıllı etiketten oluşturun. Ürün bilgisini almak için ObjectDataSource yapılandırma `ProductBLL` sınıfının `GetProducts()` yöntemi ve ürün verileri kullanarak güncelleştirmek için `UpdateProduct` yeni oluşturduğumuz aşırı. INSERT ve DELETE sekmelerinden (hiçbiri) aşağı açılan listelerden seçin.


[![ObjectDataSource yeni oluşturduğunuz UpdateProduct aşırı kullanmak için yapılandırma](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**Şekil 2**: ObjectDataSource kullanılacak yapılandırma `UpdateProduct` oluşturduğunuz aşırı yükleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image6.png))


Veri değişikliği öğreticileri anlatıldığı gibi Visual Studio tarafından oluşturulan ObjectDataSource bildirim temelli söz dizimi atar `OldValuesParameterFormatString` özelliğine `original_{0}`. Bizim yöntemler özgün beklemeyen beri bu, bizim iş mantığı katmanı ile doğal olarak, çalışmaz `ProductID` geçirilmesi değeri. Biz önceki eğitimlerine yaptığınız gibi bu nedenle, bu özellik ataması Tanımlayıcı Sözdizimi kaldırın veya bunun yerine, bu özelliğin değeri için bir dakikanızı ayırın `{0}`.

Bu değişiklikten sonra ObjectDataSource'nın bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

Unutmayın `OldValuesParameterFormatString` özelliği kaldırıldı ve var olan bir `Parameter` içinde `UpdateParameters` koleksiyonu her tarafından beklenen giriş parametreleri için bizim `UpdateProduct` aşırı yükleme.

ObjectDataSource yalnızca bir alt kümesini ürün değerleri güncelleştirmek için yapılandırılmış olsa da, GridView şu anda gösterir *tüm* ürün alanlarının. GridView düzenlemek için bir dakikanızı ayırın böylece:

- Yalnızca içeren `ProductName`, `SupplierName`, `CategoryName` BoundFields ve `Discontinued` CheckBoxField
- `CategoryName` Ve `SupplierName` (solunda) önce görünmesi alanları `Discontinued` CheckBoxField
- `CategoryName` Ve `SupplierName` BoundFields' `HeaderText` özelliği ayarlanmış "Kategorisi" ve "Sağlayıcı" sırasıyla
- Düzenleme desteği etkin (GridView'ın akıllı etiket düzenlemeyi etkinleştir onay kutusu denetleyin)

Bu değişikliklerden sonra Tasarımcı aşağıda gösterilen GridView'ın bildirim temelli söz dizimi ile Şekil 3'e benzer görünecektir.


[![Gereksiz alanları GridView kaldırın](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**Şekil 3**: gereksiz alanları GridView kaldırın ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

Bu noktada GridView'ın salt okunur davranışı tamamlanır. Verileri görüntülerken, her ürünün gösteren ürün adı, kategori, tedarikçi, GridView, bir satır olarak oluşturulur ve durumu devam etmez.


[![GridView ait salt okunur arabirimidir tamamlandı](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**Şekil 4**: GridView ait salt okunur arabirimidir tamamlandı ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> ' Da anlatıldığı gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri öğretici](an-overview-of-inserting-updating-and-deleting-data-cs.md), s görünüm durumu olması GridView (varsayılan davranış) etkin oldukça önemlidir. GridView s ayarlarsanız `EnableViewState` özelliğine `false`, yanlışlıkla silme veya düzenleme kayıtları eşzamanlı kullanıcı sahip riski. Bkz: [Uyarı: eşzamanlılık sorunu ile ASP.NET 2.0 GridViews/DetailsView/FormViews Bu destek düzenleme ve/veya silme ve Whose görünüm durumu devre dışı](http://scottonwriting.net/sowblog/posts/10054.aspx) daha fazla bilgi için.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>3. adım: bir DropDownList kategori ve arabirimleri düzenleme tedarikçi için kullanma

Sözcüğünün `ProductsRow` nesnesini içeren `CategoryID`, `CategoryName`, `SupplierID`, ve `SupplierName` gerçek yabancı anahtar kimliği değerlerinin sağlayan özellikler `Products` tablo ve karşılık gelen veritabanı `Name` değerler `Categories` ve `Suppliers` tabloları. `ProductRow`'S `CategoryID` ve `SupplierID` her ikisi de gelen okunabilir ve while yazılan `CategoryName` ve `SupplierName` özellikleri salt okunur işaretlenir.

Salt okunur durumu nedeniyle `CategoryName` ve `SupplierName` özellikleri, karşılık gelen BoundFields olan kendi `ReadOnly` özelliğini `true`, bu değerleri bir satır düzenlendiğinde değiştiren engelliyor. Biz ayarlayabilirsiniz sırada `ReadOnly` özelliğine `false`, işleme `CategoryName` ve `SupplierName` BoundFields düzenleme sırasında kutularındaki olarak, bu tür bir yaklaşım neden olacak bir özel durum kullanıcı olduğundan ürün güncelleştirmeye çalıştığında yok `UpdateProduct` alır, aşırı `CategoryName` ve `SupplierName` girişleri. Aslında, biz bu tür bir aşırı iki nedenden dolayı oluşturmak istemiyorsanız:

- `Products` Tablo yok `SupplierName` veya `CategoryName` alanları, ancak `SupplierID` ve `CategoryID`. Bu nedenle, bu belirli kimliği değerler değil kendi arama tabloları geçirilecek bizim yöntemi istiyoruz.
- Kategorileri ve üreticiler ve bunların doğru yazım öğrenmek kullanıcının gerektiren tedarikçi veya kategorisi adını yazın kullanıcının gerektiren değerinden ideal aynıdır.

Üretici ve kategori alanları kategori ve salt okunur moddayken Üreticiler adlarını görüntülemek (şimdi yaptığı gibi) ve düzenlenen geçerli seçeneklerini aşağı açılan listesi. Açılan listeyi kullanarak, son kullanıcı, hızlı bir şekilde ne kategorileri ve tedarikçimiz arasından seçim kullanılabilir görebilir ve daha kolay kendi seçim yapabilirsiniz.

Bu davranışı sağlamak için dönüştürmek ihtiyacımız `SupplierName` ve `CategoryName` TemplateFields içine BoundFields, `ItemTemplate` yayar `SupplierName` ve `CategoryName` değerleri ve özelliği `EditItemTemplate` listesine bir DropDownList denetimi kullanır Kullanılabilir kategoriler ve tedarikçimiz.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Ekleme`Categories`ve`Suppliers`DropDownLists

Başlangıç dönüştürerek `SupplierName` ve `CategoryName` BoundFields TemplateFields tarafından içine: GridView'ın akıllı etiket Düzenle sütunları bağlantısından tıklayarak; BoundField sol alt; listeden seçerek ve tıklatarak "Bu alana Dönüştür bir TemplateField"bağlantısı. Dönüştürme işlemi bir TemplateField ikisi ile oluşturacak bir `ItemTemplate` ve bir `EditItemTemplate`bildirim temelli aşağıdaki sözdiziminde gösterildiği gibi:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

BoundField olarak salt okunur olarak işaretlendikten sonra hem `ItemTemplate` ve `EditItemTemplate` etiket Web içeren özelliği kontrol `Text` özelliği geçerli veri alanına bağlı (`CategoryName`, yukarıdaki söz diziminde). Değiştirmek ihtiyacımız `EditItemTemplate`, etiket Web denetimi DropDownList denetimi ile değiştirme.

Önceki eğitimlerine anlatıldığı gibi şablon Tasarımcı üzerinden veya doğrudan Tanımlayıcı Sözdizimi düzenlenebilir. Designer üzerinden düzenlemek için GridView'ın akıllı etiket Şablonları Düzenle bağlantısından tıklayın ve kategori alanın ile çalışmayı tercih `EditItemTemplate`. Etiket Web denetimi kaldırın ve DropDownList'ın kimliği özelliğini ayarlamak bir DropDownList denetimi yerine `Categories`.


[![TexBox kaldırın ve bir DropDownList EditItemTemplate ekleyin](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**Şekil 5**: TexBox kaldırın ve bir DropDownList ekleyin `EditItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image15.png))


Sonraki kullanılabilir kategoriler ile DropDownList doldurmak ihtiyacımız. DropDownList'ın Akıllı Etiket Veri Kaynağı Seç bağlantısından tıklayın ve adlı yeni bir ObjectDataSource oluşturmak için kabul `CategoriesDataSource`.


[![CategoriesDataSource adlı yeni bir ObjectDataSource denetimi oluşturma](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**Şekil 6**: yeni bir ObjectDataSource Denetimi adlı oluşturma `CategoriesDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image18.png))


Kategorilerin tümünü Döndür bu ObjectDataSource sağlamak için onu bağladıktan `CategoriesBLL` sınıfının `GetCategories()` yöntemi.


[![ObjectDataSource CategoriesBLL'ın GetCategories() yönteme bağlama](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**Şekil 7**: ObjectDataSource bağlamak `CategoriesBLL`'s `GetCategories()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image21.png))


Son olarak, DropDownList'ın ayarlarını yapılandırma gibi `CategoryName` alanı her DropDownList görüntülenir `ListItem` ile `CategoryID` alan değeri olarak kullanılır.


[![CategoryName alanı görüntülenir ve adlı kullanıcı, Categoryıd'si değeri olarak kullanılır](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**Şekil 8**: sahip `CategoryName` alanı görüntülenir ve `CategoryID` değeri olarak kullanılan ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image24.png))


Bunlar yapmak için bildirim temelli biçimlendirme değişiklikler sonra `EditItemTemplate` içinde `CategoryName` TemplateField bir DropDownList ve bir ObjectDataSource içerir:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> DropDownList `EditItemTemplate` etkin görünüm durumunu olması gerekir. Veri bağlama komutları gibi ve yakında databinding sözdizimi DropDownList'ın Tanımlayıcı Sözdizimi ekleyeceğiz `Eval()` ve `Bind()` yalnızca, Görünüm durumu etkin denetimlerinde yer alabilir.


Adlı bir DropDownList eklemek için bu adımları yineleyin `Suppliers` için `SupplierName` TemplateField'ın `EditItemTemplate`. Bu bir DropDownList ekleme gerektireceğini `EditItemTemplate` ve başka bir ObjectDataSource oluşturma. `Suppliers` DropDownList'ın ObjectDataSource ancak yapılandırılmalıdır çağrılacak `SuppliersBLL` sınıfının `GetSuppliers()` yöntemi. Ayrıca, yapılandırma `Suppliers` görüntülenecek DropDownList `CompanyName` kullanın ve alan `SupplierID` alan değeri olarak kendi `ListItem` s.

İki DropDownLists ekledikten sonra `EditItemTemplate` s, sayfasını bir tarayıcıda yüklemek ve Chef Anton'ın Cajun Seasoning ürün için Düzenle düğmesini tıklatın. Şekil 9 gösterildiği gibi ürün kategorisi ve tedarikçi sütunları kullanılabilir kategoriler ve aralarından seçim yapabileceğiniz Üreticiler içeren aşağı açılır listeler olarak işlenir. Ancak, dikkat edin *ilk* hem aşağı açılır listelerindeki seçili öğeleri (Meşrubat kategori) ve Exotic Liquids varsayılan sağlayıcı Cennet Cajun Seasoning New Orleans Cajun tarafından sağlanan bir Condiment olsa bile Delights.


[![İlk aşağı açılan listeler öğesi varsayılan olarak seçilidir.](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**Şekil 9**: ilk öğe aşağı açılan listeler, varsayılan olarak belirlenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image27.png))


Güncelleştir'i tıklatın, ayrıca, bu ürünün bulabilirsiniz `CategoryID` ve `SupplierID` değerleri ayarlamak `NULL`. Bunların her ikisi de davranışları neden olduğundan istenmeden DropDownLists `EditItemTemplate` s tüm veri alanları temel alınan ürün verilerini bağlı değil.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>DropDownLists bağlama`CategoryID`ve`SupplierID`veri alanları

Düzenlenen ürün kategorisi ve tedarikçi olması için aşağı açılır listeler uygun değerlere geri BLL için 's gönderilen bu değerlere sahip ayarlayabilir ve `UpdateProduct` Güncelleştir'i tıklatarak bağlı yöntemi ihtiyacımız DropDownLists bağlamak `SelectedValue` özellikleri `CategoryID` ve `SupplierID` veri alanları iki yönlü veri bağlamasını kullanma. Bunu gerçekleştirmek için `Categories` ekleyebileceğiniz DropDownList, `SelectedValue='<%# Bind("CategoryID") %>'` doğrudan tanımlayıcı sözdizimi için.

Alternatif olarak, şablonu Tasarımcısı aracılığıyla düzenleme ve DropDownList'ın akıllı etiket veri bağlamaları Düzenle bağlantıyı tıklatarak DropDownList'ın veri bağlamaları ayarlayabilirsiniz. Ardından, belirtmek `SelectedValue` özelliği bağlanan `CategoryID` iki yönlü veri bağlamasını kullanma alan (bkz. Şekil 10). Bağlamak için iki bildirim temelli veya tasarımcı işlemi yineleyin `SupplierID` veri alanı için `Suppliers` DropDownList.


[![Adlı kullanıcı, Categoryıd'si DropDownList's SelectedValue iki yönlü veri bağlamasını kullanma özelliği Bağla](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**Şekil 10**: bağlama `CategoryID` DropDownList's için `SelectedValue` özellik kullanarak iki yönlü veri bağlama ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image30.png))


Bağlamaları için uygulandıktan sonra `SelectedValue` iki DropDownLists özellikleri, düzenlenen ürün kategorisi ve tedarikçi sütunları varsayılan geçerli ürünün değerleri. Güncelleştirme, tıklatarak bağlı `CategoryID` ve `SupplierID` Seçili aşağı açılan liste öğesinin değerleri geçirilir `UpdateProduct` yöntemi. Veri bağlama deyimleri eklendikten sonra Şekil 11 öğretici gösterir; nasıl Cennet Cajun Seasoning için seçilen aşağı açılan liste öğeleri doğru Condiment ve New Orleans Cajun Delights dikkat edin.


[![Varsayılan olarak seçilen düzenlenebilir ürünün geçerli kategoriye ve tedarikçi değerleri](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**Şekil 11**: düzenlenen ürünün geçerli kategori ve tedarikçi değerler varsayılan olarak seçilidir ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>İşleme`NULL`değerleri

`CategoryID` Ve `SupplierID` sütunlarında `Products` tablo olabilir `NULL`, henüz DropDownLists `EditItemTemplate` s temsil etmek için bir liste öğesi dahil olmayan bir `NULL` değeri. Bu iki sonuçları vardır:

- Kullanıcı, bir ürün kategorisi veya tedarikçi olmayan bir değiştirmek için arabirimimizi kullanamazsınız`NULL` değeri bir `NULL` bir
- Bir ürün varsa bir `NULL` `CategoryID` veya `SupplierID`, Düzenle düğmesini tıklatarak bir özel durum oluşur. Bunun nedeni, `NULL` tarafından döndürülen değer `CategoryID` (veya `SupplierID`) içinde `Bind()` DropDownList bir değere deyimi eşlenmiyor (DropDownList aykırı zaman kendi `SelectedValue` özelliği değerineayarlayın*değil* liste öğelerinin kendi koleksiyonundaki).

Desteklemek için `NULL` `CategoryID` ve `SupplierID` değerleri ihtiyacımız başka bir tane eklemek `ListItem` temsil etmek için her DropDownList için `NULL` değeri. İçinde [ana/ayrıntı filtreleme ile bir DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Öğreticisi, ek bir ekleme gördüğümüz `ListItem` DropDownList's ayarlama dahil DropDownList sınırlama için bir `AppendDataBoundItems` özelliğine `true` ve el ile ek ekleme `ListItem`. Önceki Bu öğreticide, ancak, eklediğimiz bir `ListItem` ile bir `Value` , `-1`. Databinding mantık ASP.NET, ancak, otomatik olarak boş bir dizeye dönüştürür bir `NULL` değeri ve tersi bir. Bu nedenle, Bu öğretici için istiyoruz `ListItem`'s `Value` boş bir dize olmalıdır.

Her iki DropDownLists ayarlayarak başlangıç `AppendDataBoundItems` özelliğine `true`. Ardından, eklemek `NULL` `ListItem` aşağıdakileri ekleyerek `<asp:ListItem>` her DropDownList öğesine bildirim temelli biçimlendirme tanıyarak ister:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

I "(hiçbiri)" kullanmak metin değeri olarak bu amaçla seçmiş olduğunuz `ListItem`, ancak isterseniz de boş bir dize olacak şekilde değiştirebilirsiniz.

> [!NOTE]
> İçinde gördüğümüz gibi *ana/ayrıntı filtreleme ile bir DropDownList* öğretici, `ListItem` s eklenebilir DropDownList Tasarımcısı aracılığıyla DropDownList üzerinde 's tıklayarak `Items` Özellikleri penceresinde özelliği (hangi görüntüleyecek `ListItem` Koleksiyonu Düzenleyicisi). Ancak, eklediğinizden emin olun `NULL` `ListItem` bildirim temelli söz dizimi aracılığıyla Bu öğretici için. Kullanırsanız `ListItem` oluşturulan Tanımlayıcı Sözdizimi Koleksiyonu Düzenleyicisi atlayın `Value` tamamen ayarıyla atanan tanımlayıcı biçimlendirme gibi oluşturma, boş bir dize: `<asp:ListItem>(None)</asp:ListItem>`. Bu zararsız görünebilir, ancak eksik değeri kullanmak DropDownList neden `Text` onun yerine özellik değeri. Olması durumunda bu anlamına `NULL` `ListItem` olan seçili değer "(hiçbiri)" atanması denenecek `CategoryID`, bir özel durum oluşur. Açıkça ayarlayarak `Value=""`, `NULL` değeri için atanacak `CategoryID` zaman `NULL` `ListItem` seçilir.


Üreticiler DropDownList için bu adımları yineleyin.

Bu ek `ListItem`, düzenleme arabirimi şimdi atayabilirsiniz `NULL` ürünün değerlerini `CategoryID` ve `SupplierID` alanlar, Şekil 12'de gösterildiği gibi.


[![(Hiçbiri) bir ürün kategorisi veya tedarikçi için NULL değer atamak için seçin](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**Şekil 12**: atanacak seçin (hiçbiri) bir `NULL` bir ürün kategorisi veya tedarikçi değerini ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>4. adım: RadioButtons devam etmeyen durumunu kullanma

Şu anda ürünlerin `Discontinued` veri alanı salt okunur satırlar için devre dışı bırakılmış bir onay kutusu ve düzenlenmekte olan satır için etkinleştirilmiş bir onay kutusu işleyen bir CheckBoxField kullanılarak ifade edilir. Bu kullanıcı arabirimi genellikle uygun olsa da, biz bir TemplateField kullanarak gerekirse özelleştirebilirsiniz. Bu öğretici için CheckBoxField alınacağı kullanıcı ürünün belirtebileceğiniz iki seçenek "Etkin" ve "Dönüştürülmesine" RadioButtonList denetimi kullanan TemplateField'a değiştirelim `Discontinued` değeri.

Başlangıç dönüştürerek `Discontinued` CheckBoxField TemplateField ile oluşturacak bir TemplateField içine bir `ItemTemplate` ve `EditItemTemplate`. Bir onay kutusu ile hem şablonları içerir, `Checked` özelliği bağlı `Discontinued` veri alanı, olan iki arasındaki tek fark `ItemTemplate`'s CheckBox'ın `Enabled` özelliği ayarlanmış `false`.

Her iki onay kutusunun Değiştir `ItemTemplate` ve `EditItemTemplate` RadioButtonList denetimi ile hem RadioButtonLists ayarı `ID` özelliklerine `DiscontinuedChoice`. Ardından, RadioButtonLists her iki radyo düğmeleri, tek etiketli "Active" içermesi gerekir "False" ve "Dönüştürülmesine" etiketli bir değeri olan'true bir değer belirtin. Bunu ya da girebilirsiniz gerçekleştirmek için `<asp:ListItem>` öğelerinde kullanmak ve bildirim temelli söz dizimi aracılığıyla doğrudan `ListItem` Koleksiyonu Düzenleyicisi Tasarımcısından. Şekil 13 göstermektedir `ListItem` koleksiyon Düzenleyicisi'ni iki radyo düğmesi seçeneklerini sonra belirtilen.


[![Add](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**Şekil 13**: "Active" ve "Üretimi Durdurulmuş" seçenekleri RadioButtonList ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image39.png))


RadioButtonList itibaren `ItemTemplate` düzenlenebilir olmamalıdır ayarlamak kendi `Enabled` özelliğine `false`, bırakma `Enabled` özelliğine `true` (varsayılan) RadioButtonList için `EditItemTemplate`. Bu radyo düğmeleri salt okunur olarak düzenlenmiş sıradaki hale getirir, ancak düzenlenen satır RadioButton değerlerini değiştirmek kullanıcının izin verir.

Hala RadioButtonList denetimleri atamak ihtiyacımız `SelectedValue` özellikleri radyo düğmesi seçilir böylece ürünün dayalı `Discontinued` veri alanı. Bu öğreticide daha önce incelenmesi DropDownLists gibi ile bu veri bağlama söz dizimi ya da doğrudan bildirim temelli biçimlendirme veya RadioButtonLists akıllı etiketler veri bağlamaları Düzenle bağlantıyı aracılığıyla eklenebilir.

İki RadioButtonLists ekleme ve bunları yapılandırma sonra `Discontinued` TemplateField'ın bildirim temelli biçimlendirme gibi görünmelidir:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

Bu değişiklikler ile `Discontinued` sütun dönüştürülmüş onay kutularını listesinden (bkz. Şekil 14) radyo düğmesi çiftlerinin listesini için. Bir ürün düzenlerken, radyo düğmesi seçilir ve ürünün devam etmeyen durumu diğer radyo düğmesini seçerek ve Güncelleştir'i tıklatarak güncelleştirilebilir.


[![Devam etmeyen onay kutularını radyo düğmesi çiftleri tarafından değiştirildi](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**Şekil 14**: dönüştürülmesine onay kutularını değiştirilir radyo düğmesi çiftleri tarafından ([tam boyutlu görüntüyü görüntülemek için tıklatın](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> Bu yana `Discontinued` sütununda `Products` veritabanı olamaz `NULL` değerleri, olmayan ihtiyacımız yakalama hakkında endişelenmeye gerek `NULL` arabirimi bilgileri. Eğer, ancak `Discontinued` sütun içerebilir `NULL` biz üçüncü eklemek için isteyeceğiniz değerler radyo düğmesinin ile listeye kendi `Value` boş bir dize olarak ayarlayın (`Value=""`), kategori ve tedarikçi DropDownLists gibi yalnızca.


## <a name="summary"></a>Özet

Salt okunur, düzenleme ve ekleme arabirimleri BoundField ve CheckBoxField otomatik olarak oluşturmak olsa da, bunlar özelleştirme yeteneği yoksundur. Genellikle, ancak biz arabirimi, ekleme veya düzenleme özelleştirmek (önceki öğreticide gördüğümüz gibi) belki de doğrulama denetimleri ekleme gerekir ya da (biz Bu öğreticide gördüğünüz gibi) veri toplama kullanıcı arabirimi özelleştirerek. Bir TemplateField arabirimiyle özelleştirme aşağıdaki adımlarda toplanabilir:

1. Bir TemplateField eklemek veya mevcut BoundField veya CheckBoxField TemplateField'a Dönüştür
2. Arabirimi gerektiği şekilde büyütmek
3. Uygun veri alanları iki yönlü veri bağlamasını kullanma yeni eklenen Web denetimleri bağlama

Yerleşik ASP.NET Web denetimleri kullanarak ek olarak, bir TemplateField derlenmiş, özel sunucu denetimleri ve kullanıcı denetimleri ile şablonları de özelleştirebilirsiniz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [sonraki](implementing-optimistic-concurrency-cs.md)
