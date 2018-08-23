---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
title: Düzenleme doğrulama denetimleri ekleme ve arabirimler (C#) ekleme | Microsoft Docs
author: rick-anderson
description: Bu öğreticide daha sağlamak EditItemTemplate ve InsertItemTemplate Web denetimi veri doğrulama denetimleri ekleme ne kadar kolay olduğunu görüyoruz...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2086cb1a-ab78-49ae-9c0b-03891c69776a
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
msc.type: authoredcontent
ms.openlocfilehash: 4990ce2c15465873ef08aeb697780d98a186b3c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754906"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-c"></a>Arabirimler (C#) ekleme ve düzenleme doğrulama denetimleri ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_CS.exe) veya [PDF olarak indirin](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/datatutorial19cs1.pdf)

> Bu öğreticide daha iyi bir kullanıcı arabirim sağlamak üzere EditItemTemplate ve InsertItemTemplate Web denetimi veri doğrulama denetimleri ekleme ne kadar kolay olduğunu göreceğiz.


## <a name="introduction"></a>Giriş

GridView ve DetailsView denetimlerini örneklerde şu üç öğreticiler tüm BoundFields CheckBoxFields (GridView veya DetailsView bir veri kaynağına bağlanırken Visual Studio tarafından otomatik olarak eklenen alan türleri ve oluşan geçmiş üzerinden incelediniz Akıllı etiket denetimi). Bir satır GridView veya DetailsView düzenlerken, salt okunur olmayan bu BoundFields metin kutuları, son kullanıcı var olan verilere değiştirebilirsiniz dönüştürülür. Benzer şekilde, ne zaman yeni bir ekleme kayıt DetailsView denetimine bu BoundFields ayarlanmış `InsertVisible` özelliği `true` (varsayılan), kullanıcı yeni kayıttaki alan değerlerini sağlayabilir boş metin kutuları işlenir. Benzer şekilde, standart, salt okunur arabiriminde devre dışı olan, CheckBoxFields, düzenleme ve arabirimleri ekleme Etkin onay kutularını içine dönüştürülür.

Varsayılan düzenleme ve arabirimleri BoundField ve CheckBoxField ekleme yararlı olabilir, ancak herhangi bir tür doğrulama arabirimi eksik. Bir kullanıcı veri girişini atlama gibi hata - aktarılmayacağını `ProductName` alan veya için geçersiz bir değer girip `UnitsInStock` (-50 gibi) bir özel durum gelen uygulama mimarisini derinliklerine ulaşmak gerçekleştirilecektir. Bu durum, düzgün bir şekilde gösterildiği şekilde işlenebilir [önceki öğreticide](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md), ideal olarak kullanıcı arabirimi ekleme veya düzenleme kullanıcı geçersiz tür veri girmesini önlemek için doğrulama denetimleri içerir ilk yerleştirin.

Özelleştirilmiş düzenleme veya ekleme arabirimi sağlamak için BoundField ya da CheckBoxField bir TemplateField ile değiştirmek gerekiyor. Tartışma konu olan TemplateField içinde [GridView denetiminde TemplateField kullanma](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) ve [DetailsView denetiminde TemplateField kullanma](../custom-formatting/using-templatefields-in-the-detailsview-control-cs.md) öğreticiler, katı oluşabilir şablonları tanımlamak için farklı satır durumları arabirimleri ayırın. TemplateField'ın `ItemTemplate` ise salt okunur alanları veya satırları DetailsView veya GridView denetimlerinde işlenirken alışkın olduğu `EditItemTemplate` ve `InsertItemTemplate` modları, sırasıyla ekleme ve düzenleme için kullanılacak arabirimleri belirtin.

Bu öğreticide TemplateField için 's doğrulama denetimleri eklemek için ne kadar kolay olduğunu göreceğiz `EditItemTemplate` ve `InsertItemTemplate` daha iyi bir kullanıcı arabirimi sağlamak için. Özellikle, bu öğreticide oluşturulan örnek alan [ekleme, güncelleştirme ve silme ile ilişkili olayları İnceleme](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) uygun doğrulama içerecek şekilde arabirimleri öğretici ve düzenleme ve ekleme artırmaktadır.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-csmd"></a>1. adım: örnek çoğaltma[ekleme, güncelleştirme ve silme ile ilişkili olayları İnceleme](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)

İçinde [ekleme, güncelleştirme ve silme ile ilişkili olayları İnceleme](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) öğretici adları ve düzenlenebilir bir GridView ürünleri fiyatları listelenen bir sayfa oluşturduk. Ayrıca, sayfanın bir DetailsView dahil olan `DefaultMode` özelliğinin ayarlandığı `Insert`, böylece her zaman işleme ekleme modunda. Bu DetailsView kullanıcı yeni bir ürün için fiyat ve adını girin, Ekle'yi tıklatın ve sistemine eklenmiş olması (bkz. Şekil 1).


[![Önceki örnekte, yeni ürün eklemek ve varolanları düzenlemek kullanıcılara](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image1.png)

**Şekil 1**: önceki örnek verir kullanıcılara yeni ürün eklemek ve varolanları düzenlemek ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image3.png))


Bu öğretici için Hedefimiz GridView ve DetailsView doğrulama denetimleri sağlamayı destekleyebilen sağlamaktır. Özellikle, bizim Doğrulama mantığı olur:

- Adı ekleme veya bir ürün düzenlerken sağlanmasını gerektirir
- Fiyat bir kayıt eklendiğinde sağlanması gerekir. bir kaydın düzenlenmesi, biz yine de bir fiyat gerektirir, ancak GridView kişinin programlama mantığını kullanır `RowUpdating` olay işleyicisi önceki öğreticide zaten var
- Fiyat için girilen değer geçerli bir para birimi biçiminde olduğundan emin olun

Biz sırasında doğrulama eklemek için önceki örnekte deneyimlerinizi göz atmadan önce ilk örnekte çoğaltmak ihtiyacımız `DataModificationEvents.aspx` Bu öğretici için sayfanın sayfasına `UIValidation.aspx`. İhtiyacımız hem de kopyalamak için bunu sağlamak için `DataModificationEvents.aspx` sayfanın bildirim temelli biçimlendirme ve kaynak kodu. İlk bildirim temelli biçimlendirme, aşağıdaki adımları uygulayarak kopyalayın:

1. Açık `DataModificationEvents.aspx` Visual Studio'daki sayfası
2. Bildirim temelli işaretleme sayfanın (sayfanın alt kısmındaki kaynağı düğmesini tıklatın) gidin
3. Metni kopyalayın `<asp:Content>` ve `</asp:Content>` gösterildiği Şekil 2'olarak etiketleri (satırlar 44 3).


[![Metin içindeki kopyalama &lt;asp: Content&gt; denetimi](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image4.png)

**Şekil 2**: metin içinde kopyalama `<asp:Content>` denetimi ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image6.png))


1. Açık `UIValidation.aspx` sayfası
2. Bildirim temelli sayfanın biçimlendirmesine gidin
3. İçindeki metni yapıştırın `<asp:Content>` denetimi.

Kaynak kodu kopyalamak açın `DataModificationEvents.aspx.cs` sayfasında ve metni kopyalayın *içinde* `EditInsertDelete_DataModificationEvents` sınıfı. Üç olay işleyicileri kopyalayın (`Page_Load`, `GridView1_RowUpdating`, ve `ObjectDataSource1_Inserting`), ancak bunu **değil** sınıf bildiriminin kopyalayın veya `using` deyimleri. Kopyalanan metni yapıştırmayı *içinde* `EditInsertDelete_UIValidation` sınıfını `UIValidation.aspx.cs`.

İçerik ve kod üzerinde taşıdıktan `DataModificationEvents.aspx` için `UIValidation.aspx`, bir tarayıcıda ilerleme durumunuzu kullanıma test etmek için bir dakikanızı ayırın. Aynı çıktı ve her iki bu sayfaların aynı işlevselliği deneyimi görmeniz gerekir (Şekil 1 için bir ekran görüntüsü geri başvuru `DataModificationEvents.aspx` iş başında).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>2. adım: BoundFields TemplateField dönüştürme

Düzenleme ve ekleme arabirimlerine doğrulama denetimleri eklemek için DetailsView ve GridView denetimleri tarafından kullanılan BoundFields TemplateField dönüştürülmesi gerekir. Bunu başarmak için akıllı etiketler GridView ve DetailsView'ın sütunları düzenlemek ve alanları düzenleme bağlantıları sırasıyla tıklayın. Burada, her BoundFields seçin ve "Bu alanı bir TemplateField dönüştürün" bağlantısına tıklayın.


[![Her GridView'ın ve DetailsView'ın BoundFields TemplateField dönüştürün](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image7.png)

**Şekil 3**: her içine GridView'ın ve DetailsView'ın BoundFields TemplateField dönüştürme ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image9.png))


Bir TemplateField alanları iletişim kutusu aracılığıyla bir BoundField dönüştürme BoundField olarak aynı salt okunur, düzenleme ve ekleme arabirimlerine sergileyen bir TemplateField oluşturur. Aşağıdaki biçimlendirme için bildirim temelli söz dizimi görülmektedir `ProductName` bir TemplateField dönüştürüldükten sonra DetailsView alanındaki:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample1.aspx)]

Bu TemplateField otomatik olarak oluşturulan üç şablonları olduğunu Not `ItemTemplate`, `EditItemTemplate`, ve `InsertItemTemplate`. `ItemTemplate` Bir tek veri alan değeri görüntüler (`ProductName`) etiketi Web denetimi kullanarak, while `EditItemTemplate` ve `InsertItemTemplate` veri alan değeri veri alanı metin ile kutusunun ilişkilendiren bir TextBox Web denetimi sunmak `Text` iki yönlü veri bağlamasını kullanma özelliği. Yalnızca DetailsView bu sayfa, ekleme için kullandığımızdan, kaldırabilir `ItemTemplate` ve `EditItemTemplate` iki TemplateField gelen olsa da bunları bırakarak içinde bir zararı.

GridView DetailsView özelliklerini ekleyerek, dönüştürme GridView'ın yerleşik desteklemediğinden `ProductName` bir TemplateField alanına sonuçlanıyor yalnızca bir `ItemTemplate` ve `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample2.aspx)]

"Dönüştür bu alana bir TemplateField" tıklayarak Visual Studio, şablonları dönüştürülmüş BoundField kullanıcı arabiriminin taklit eden bir TemplateField oluşturdu. Bu, bir tarayıcı aracılığıyla bu sayfasını ziyaret ederek doğrulayabilirsiniz. Görünümünü ve davranışını TemplateField olduğunu deneyimine benzer BoundFields yerine karşılaştıklarını bulabilirsiniz.

> [!NOTE]
> Özelleştirme gerektiğinde şablonları düzenleme arabirimlerde çekinmeyin. Örneğin, biz TextBox olmasını isteyebilirsiniz `UnitPrice` daha küçük bir metin kutusu olarak işlenen TemplateField `ProductName` metin. Bunu gerçekleştirmek için metin kutusunun ayarlayabilirsiniz `Columns` özelliğini uygun bir değer ya mutlak bir genişlik aracılığıyla sağlayın `Width` özelliği. Sonraki öğreticide tamamen metin kutusunun bir alternatif veri girişi Web denetimi ile değiştirerek düzenleme arabirimini özelleştirme göreceğiz.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>3. adım: GridView'ın doğrulama denetimleri ekleme`EditItemTemplate` s

Veri girişi formları oluştururken, kullanıcıların tüm gerekli alanları girin ve tüm sağlanan girişler geçerli, doğru biçimlendirilmiş değerlerinin olduğunu önemlidir. Bir kullanıcının giriş geçerli olduğundan emin olun yardımcı olmak için tek bir giriş denetiminin değeri doğrulamak için kullanılmak üzere tasarlanmış beş yerleşik doğrulama denetimleri ASP.NET sağlar:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) bir değer sağlandı sağlar
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) bir değeri başka bir Web denetim değerini veya bir sabit değer karşı doğrular ve değerinin biçimi belirtilen veri türü için geçerli olmasını sağlar
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) değerleri aralığı içinde bir değer olmasını sağlar
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) karşı bir değer doğrulayan bir [normal ifade](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) bir değer, kullanıcı tarafından tanımlanan özel bir yöntem karşı doğrular

Bu beş denetimleri hakkında daha fazla bilgi için kullanıma [doğrulama denetimleri bölümüne](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) , [ASP.NET hızlı başlangıç öğreticileri](https://asp.net/QuickStart/aspnet/).

DetailsView ve GridView'ın bir RequiredFieldValidator kullanılacak gerekir müşterilerimize öğreticide `ProductName` TemplateField ve DetailsView'ın içinde bir RequiredFieldValidator `UnitPrice` TemplateField. Ayrıca, her iki denetim için bir CompareValidator eklemeniz gerekir `UnitPrice` TemplateField girilen fiyat 0'a eşit veya daha büyük bir değere sahip ve geçerli para birimi biçiminde sunulan güvence altına alınır.

> [!NOTE]
> While ASP.NET 1.x sahip aynı bu beş doğrulama denetimleri, ASP.NET 2.0 birkaç geliştirme eklendi, ana iki istemci tarafı komut dosyası olan Internet Explorer dışındaki tarayıcıları ve bölüm doğrulama denetimleri içeren bir sayfa ile özelliği desteği doğrulama gruplar. Yeni doğrulama denetimi 2.0 hakkında daha fazla bilgi için başvurmak [doğrulama denetimleri ASP.NET 2.0 ayrıntıları](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Gerekli doğrulama denetimleri ekleyerek başlayalım `EditItemTemplate` GridView'ın TemplateField s. Bunu gerçekleştirmek için şablon düzenleme arabirimi oluşturan getirmek için akıllı etiketinde GridView'ın Şablonları Düzenle bağlantısına tıklayın. Buradan, hangi şablonun düzenlemek için aşağı açılan listeden seçebilirsiniz. Düzenleme arabirimi genişletmek istediğimiz olduğundan doğrulama denetimleri eklemek ihtiyacımız `ProductName` ve `UnitPrice`'s `EditItemTemplate` s.


[![ProductName ve UnitPrice'nın EditItemTemplates genişletmek ihtiyacımız](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image10.png)

**Şekil 4**: genişletme için ihtiyacımız `ProductName` ve `UnitPrice`'s `EditItemTemplate` s ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image12.png))


İçinde `ProductName` `EditItemTemplate`, sonra metin kutusu yerleştirme şablon düzenleme arabirimine Toolbox'tan sürükleyerek bir RequiredFieldValidator ekleyin.


[![Bir RequiredFieldValidator ProductName EditItemTemplate için ekleyin](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image13.png)

**Şekil 5**: eklemek için bir RequiredFieldValidator `ProductName` `EditItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image15.png))


Tüm doğrulama denetimleri, tek bir ASP.NET Web denetim girişi doğrulayarak çalışır. Bu nedenle, eklediğimiz yöntemlerin RequiredFieldValidator metin kutusunda karşı doğrulamalıdır belirtmek ihtiyacımız `EditItemTemplate`; bu doğrulama denetiminin ayarlayarak gerçekleştirilir [ControlToValidate özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) için `ID` uygun Web denetimi. TextBox şu anda yerine nondescript yok `ID` , `TextBox1`, ancak şimdi daha uygun bir şeyle değiştirmek. Metin şablonunda tıklayın ve ardından, Özellikler penceresinden değiştirme `ID` gelen `TextBox1` için `EditProductName`.


[![EditProductName için metin kutusunun Kimliğini değiştirme](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image16.png)

**Şekil 6**: metin kutusunun değiştirme `ID` için `EditProductName` ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image18.png))


Ardından, RequiredFieldValidator'ın ayarlamak `ControlToValidate` özelliğini `EditProductName`. Son olarak, ayarlama [ErrorMessage özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) "ürün adı sağlamanız gerekir" için ve [metin özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) için "\*". `Text` Özellik değeri, sağlanan varsa, doğrulama başarısız olursa doğrulama denetimi tarafından görüntülenen metin. `ErrorMessage` Gerekli olan özellik değeri varsa ValidationSummary denetimi tarafından kullanılan `Text` özellik değeri atlanırsa, `ErrorMessage` özellik değeri, aynı zamanda geçersiz giriş doğrulama denetimi tarafından görüntülenen metni.

Bu üç özelliklerini RequiredFieldValidator ayarladıktan sonra ekranınızın Şekil 7'ye benzer görünmelidir.


[![RequiredFieldValidator'ın ControlToValidate, ErrorMessage ve metin özellikleri ayarlama](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image19.png)

**Şekil 7**: RequiredFieldValidator'ın ayarlamak `ControlToValidate`, `ErrorMessage`, ve `Text` özellikleri ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image21.png))


İle eklenen RequiredFieldValidator `ProductName` `EditItemTemplate`, tüm gerekli doğrulama eklemek için kalan olduğundan `UnitPrice` `EditItemTemplate`. Biz, bu sayfa için karar beri `UnitPrice` olan isteğe bağlı bir kayıt düzenleme, bir RequiredFieldValidator eklemeniz gerekmez. Ancak, emin olmak için bir CompareValidator eklemek ihtiyacımız `UnitPrice`, belirtilirse, bir para birimi olarak düzgün biçimlendirildiğinden ve büyük veya 0'a eşit.

Biz CompareValidator'la eklemeden önce `UnitPrice` `EditItemTemplate`, ilk metin kutusuna Web denetimin Kimliğinden değiştirelim `TextBox2` için `EditUnitPrice`. Bu değişikliği yaptıktan sonra CompareValidator ekleme ayarı kendi `ControlToValidate` özelliğini `EditUnitPrice`, kendi `ErrorMessage` "Fiyat değerinden büyük veya sıfıra eşit olmalıdır ve para birimi sembolünü içeremez", özellik ve kendi `Text` özelliğini "\*".

Göstermek için `UnitPrice` değeri 0'a eşit veya sıfırdan büyük, CompareValidator'ın ayarlamak [işleci özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) için `GreaterThanEqual`, kendi [ValueToCompare özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) "0" ve kendi [ Tür özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) için `Currency`. Aşağıdaki bildirim temelli söz dizimi gösterildiği `UnitPrice` TemplateField'ın `EditItemTemplate` bu değişiklikleri yaptıktan sonra:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample3.aspx)]

Bu değişiklikleri yaptıktan sonra sayfasını bir tarayıcıda açın. Bir ürün düzenlerken geçersiz Fiyat değeri girin veya adını Atla çalışırsanız, metin kutusunun yanındaki bir yıldız işareti görünür. Şekil 8 gösterildiği gibi $19.95 para birimi sembolü içeren bir fiyat değerini geçersiz olarak kabul edilir. CompareValidator'ın `Currency` `Type` rakam ayırıcıları (örneğin, kültür ayarlarına bağlı olarak, nokta veya virgül) ve önüne bir artı veya eksi işareti için izin verir, ancak mu *değil* bir para birimi simgesi izin verir. Şu anda düzenleme arabirimi işler gibi bu davranış kullanıcılar perplex `UnitPrice` para birimi biçimi kullanarak.

> [!NOTE]
> Geri çağırma *olayları ilişkili ekleme, güncelleştirme ve silme ile* BoundField'ın ayarladığımız öğretici `DataFormatString` özelliğini `{0:c}` bir para birimi olarak biçimlendirmek için. Ayrıca, ayarladığımız `ApplyFormatInEditMode` özelliği true, biçimlendirmek için arabirimi düzenleme kullanıcının GridView neden `UnitPrice` bir para birimi olarak. Visual Studio BoundField bir TemplateField dönüştürülürken, bu ayarları not ve biçimlendirilmiş metin kutusunun `Text` özelliği veri bağlama söz dizimini kullanarak bir para birimi olarak `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Geçersiz giriş içeren metin kutuları yanında bir yıldız işareti görünür](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image22.png)

**Şekil 8**: bir yıldız işareti görünür bir sonraki geçersiz giriş içeren metin kutuları ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image24.png))


While olarak doğrulama çalışır-olduğundan, kullanıcının sahip olduğu kabul edilebilir değil bir kaydı düzenleme yaparken para birimi simgesi el ile kaldırmak. Bu sorunu gidermek için üç seçenek sunuyoruz:

1. Yapılandırma `EditItemTemplate` böylece `UnitPrice` değeri, para birimi olarak biçimlendirilmemiş.
2. Kullanıcının bir para birimi simgesi CompareValidator kaldırarak ve düzgün şekilde biçimlendirilmiş bir para birimi değeri için düzgün şekilde denetleyen bir RegularExpressionValidator yerine girin izin verin. Buradaki sorun, bir para birimi değeri doğrulamak için normal ifade kolay değildir ve kültür ayarları eklemek isterseniz, kod yazma gerekir.
3. GridView'ın sunucu tarafı doğrulama mantığı kullanır ve doğrulama denetimi tamamen kaldırmak `RowUpdating` olay işleyicisi.

Bu alıştırma için #1 seçeneği ile Bahsedelim. Şu anda `UnitPrice` metin kutusunda veri bağlama ifadesi nedeniyle bir para birimi olarak biçimlendirilmiş `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Bağlama deyime değiştirme `Bind("UnitPrice", "{0:n2}")`, duyarlık iki basamaklı sayı olarak sonucu biçimlendirir. Bu bildirim temelli söz dizimi aracılığıyla doğrudan veya veri bağlamaları Düzenle bağlantısına tıklayarak yapılabilir `EditUnitPrice` metin kutusunda `UnitPrice` TemplateField'ın `EditItemTemplate` (Şekil 9 ve 10 bakın).


[![Metin kutusunun veri bağlamaları Düzenle bağlantısına tıklayın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image25.png)

**Şekil 9**: metin kutusunun veri bağlamaları Düzenle bağlantısına tıklayın ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image27.png))


[![Biçim belirticisinin bağlama deyiminde belirtin](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image28.png)

**Şekil 10**: görünümü içinde biçim belirticisini belirtin `Bind` deyimi ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image30.png))


Bu değişikliğe düzenleme arabirimi biçimlendirilmiş fiyatına Grup ayırıcı olarak virgül ve ondalık ayırıcısı olarak nokta içerir, ancak para birimi simgesi devre dışı bırakır.

> [!NOTE]
> `UnitPrice` `EditItemTemplate` Oluşması için geri gönderme ve başlamak için güncelleştirme mantığı sağlayan, bir RequiredFieldValidator içermez. Ancak, `RowUpdating` olay işleyicisi üzerinden kopyalanmış *ekleme, güncelleştirme ve silme ile ilişkili olayları İnceleme* öğretici içerir sağlar programlı bir onay `UnitPrice` sağlanır. Bu mantık kaldırmak için projeyi olarak bırakın çekinmeyin- ya da eklemek için bir RequiredFieldValidator `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>4. adım: Veri girişi sorunlarını özetleme

Ek olarak beş doğrulama denetimleri, ASP.NET içerir [ValidationSummary denetimi](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), görüntüleyen `ErrorMessage` geçersiz veri algılandı. Bu doğrulama denetimleri, s. Bu Özet veriler, kalıcı, istemci tarafı messagebox aracılığıyla ya da web sayfasında metin olarak görüntülenebilir. Şimdi bu öğreticide herhangi bir doğrulama sorunu özetleyen bir istemci-tarafı messagebox içerecek şekilde artırın.

Bunu yapmak için ValidationSummary denetimi Tasarımcısı araç kutusundan sürükleyin. Yalnızca Özet bir messagebox görüntülemek için yapılandırmak için yapacağız olduğundan doğrulama denetimi konumunu, önemli değildir. Denetimi ekledikten sonra ayarlama, [ShowSummary özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) için `false` ve kendi [ShowMessageBox özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) için `true`. Bu ekleme ile bir istemci-tarafı messagebox tüm doğrulama hatalarını özetlenmiştir.


[![Doğrulama hataları bir istemci-tarafı Messagebox özetlenmiştir](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image31.png)

**Şekil 11**: doğrulama hataları bir istemci-tarafı Messagebox özetlenir ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>5. adım: DetailsView için 's doğrulama denetimleri ekleme`InsertItemTemplate`

Bu öğretici için kalan tüm olan DetailsView'ın ekleme arabirimine doğrulama denetimleri ekleme. DetailsView'ın şablonlarını doğrulama denetimleri ekleme işlemi, adım 3'te incelenirken aynıdır; Bu nedenle, ki bu adımı görev aracılığıyla breeze. GridView'ile ın yaptığımız gibi `EditItemTemplate` s, ı etmenizden memnuniyet duyarız, yeniden adlandırmak için `ID` kutularındaki metinleri nondescript sn `TextBox1` ve `TextBox2` için `InsertProductName` ve `InsertUnitPrice`.

Eklemek için bir RequiredFieldValidator `ProductName` `InsertItemTemplate`. Ayarlama `ControlToValidate` için `ID` şablonda, TextBox'ın kendi `Text` özelliğini "\*" ve kendi `ErrorMessage` "ürün adı sağlamanız gerekir" özelliği.

Bu yana `UnitPrice` olduğu bir RequiredFieldValidator için eklemek için bu sayfayı yeni kayıt eklerken gereken `UnitPrice` `InsertItemTemplate`, ayar, `ControlToValidate`, `Text`, ve `ErrorMessage` özellikleri uygun şekilde. Son olarak, bir CompareValidator'la ekleme `UnitPrice` `InsertItemTemplate` de yapılandırma, `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, ve `ValueToCompare` ileyaptığımızgibiözellikleri`UnitPrice`'s GridView'ın içinde CompareValidator `EditItemTemplate`.

Bu doğrulama denetimleri ekledikten sonra yeni ürün sisteme adı sağlanmazsa veya bunun ücreti negatif bir sayı ise, eklenemez veya yasa dışı biçimlendirilmiş.


[![Doğrulama mantığını DetailsView'ın ekleyerek arabirime eklenmiştir.](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image34.png)

**Şekil 12**: Doğrulama mantığı DetailsView'ın ekleyerek arabirime eklendi ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>6. adım: doğrulama denetimleri doğrulama gruplar halinde bölümleme.

İki mantıksal olarak birbirinden farklı doğrulama denetimleri kümesini sayfamızı oluşur: GridView'a karşılık gelen bu arabirimi düzenleme ve DetailsView için karşılık gelen bu arabirimi ekleme. Varsayılan olarak bir geri gönderme gerçekleştiğinde *tüm* sayfasında doğrulama denetimleri denetlenir. Ancak, bir kaydı düzenleme yaparken size doğrulamak için doğrulama denetimleri DetailsView'ın ekleme arabiriminin istemezsiniz. Bir kullanıcı bir ürün mükemmel yasal değerlerle düzenlerken Şekil 13 bizim geçerli ikilemle gösterir, ad ve fiyat ekleme arabirimi boş değerler olduğundan tıklatarak güncelleştirme bir doğrulama hatasına neden olur.


[![Doğrulama denetimleri ekleme arabirimin yangın için neden olan bir ürün güncelleştiriliyor](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image37.png)

**Şekil 13**: yangın için neden doğrulama denetimleri ekleme arabirimin bir ürün güncelleştiriliyor ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image39.png))


ASP.NET 2.0 doğrulama denetimleri doğrulama gruplara bölümlenebilir kendi `ValidationGroup` özelliği. Bir Grup doğrulama denetimleri kümesini ilişkilendirmek için ayarlamanız yeterlidir, `ValidationGroup` özelliği aynı değere. Müşterilerimize öğreticide ayarlamak `ValidationGroup` özellikleri için GridView'ın TemplateField doğrulama denetimleri `EditValidationControls` ve `ValidationGroup` DetailsView'ın TemplateField için özelliklerini `InsertValidationControls`. Bu değişiklikler, doğrudan bildirim temelli işaretlemede yapılabilir veya şablon arabirimi tasarımcısı kullanılırken Özellikler penceresinden düzenleyin.

Ek doğrulama denetimleri, düğme ve ASP.NET 2.0 düğmesi ile ilgili denetimleri de dahil bir `ValidationGroup` özelliği. Doğrulama grubun doğrulayıcılar için geçerlilik denetlenir yalnızca ne zaman bir geri gönderme aynı olan bir düğme tarafından başlattığı `ValidationGroup` özellik ayarı. Örneğin sırada tetiklemek Ekle düğmesini DetailsView'ın `InsertValidationControls` ihtiyacımız CommandField'ın ayarlamak için doğrulama grubu `ValidationGroup` özelliğini `InsertValidationControls` (bkz. Şekil 14). GridView'ın ayrıca, olarak CommandField'ın `ValidationGroup` özelliğini `EditValidationControls`.


[![Kümesi DetailsView InsertValidationControls CommandField'ın ValidationGroup özelliğini kullanıcının](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image40.png)

**Şekil 14**: DetailsView'ın ayarlamak CommandField'ın `ValidationGroup` özelliğini `InsertValidationControls` ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image42.png))


GridView'ın ve DetailsView TemplateField ve CommandFields bu değişikliklerden sonra aşağıdakine benzer görünmelidir:

DetailsView'ın TemplateField ve CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample4.aspx)]

GridView'ın CommandField ve TemplateField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample5.aspx)]

Bu noktada yalnızca DetailsView'ın Ekle düğmesi, Şekil 13 tarafından vurgulanan sorunu çözme tıklandığında Düzenle-özel doğrulama denetimleri yalnızca GridView'ın güncelleştir düğmesine tıklandığında ve Ekle özel doğrulama denetimleri yangın kov. Ancak, bu değişiklikle birlikte, ValidationSummary denetimi artık geçersiz veri girerken görüntüler. Ayrıca, ValidationSummary denetimi içeren bir `ValidationGroup` özelliği ve yalnızca gösterir özet bilgilerini doğrulama grubu bu doğrulama denetimleri. Bu nedenle, bu sayfada, birincisi iki doğrulama denetimleri sağlamak ihtiyacımız `InsertValidationControls` doğrulama grubu, diğeri de `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample6.aspx)]

Bu eklenmesiyle öğreticimize tamamlandı!

## <a name="summary"></a>Özet

Hem bir ekleme ve düzenleme arabirimi BoundFields sağlarken, arabirimi özelleştirilebilir değildir. Genellikle, düzenleme ve kullanıcının geçerli bir biçimde gerekli girişleri girdiğinden emin olmak için arabirim ekleme doğrulama denetimleri ekleme istiyoruz. Bunu gerçekleştirmek için biz BoundFields TemplateField dönüştürün ve doğrulama denetimleri için uygun şablonu ekleyin. Bu öğreticide size örnekten Genişletilmiş *ekleme, güncelleştirme ve silme ile ilişkili olayları İnceleme* DetailsView için doğrulama denetimleri ekleme Öğreticisi, arabirimi ve GridView'ın ekleme düzenleme arabirimi. Üstelik ValidationSummary denetimini kullanarak doğrulama özet bilgileri görüntülemek nasıl ve sayfadaki doğrulama denetimleri farklı doğrulama gruplar halinde bölümlere nasıl gördük.

Bu öğreticide gördüğümüz gibi TemplateField doğrulama denetimleri içerecek şekilde genişletilmesi düzenleme ve ekleme arabirimleri sağlar. TemplateField ayrıca ek giriş Web denetimleri, dahil etmek için daha uygun bir Web denetimi tarafından değiştirilecek metin kutusu etkinleştirme genişletilebilir. TextBox denetimi yabancı anahtar düzenlerken idealdir bir verilere bağlı DropDownList denetimi ile nasıl değiştirileceğini görüyoruz sonraki müşterilerimize öğreticide (gibi `CategoryID` veya `SupplierID` içinde `Products` tablo).

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Liz Shulok ve Zack Jones yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
> [İleri](customizing-the-data-modification-interface-cs.md)
