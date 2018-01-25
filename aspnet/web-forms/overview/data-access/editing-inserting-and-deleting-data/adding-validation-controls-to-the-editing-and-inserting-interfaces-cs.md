---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
title: "Düzenleme için doğrulama denetimleri ekleme ve arabirimler (C#) ekleme | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide daha sağlamak için EditItemTemplate ve InsertItemTemplate Web denetimi, veri doğrulama denetimleri eklemek için ne kadar kolay olduğunu göreceksiniz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2086cb1a-ab78-49ae-9c0b-03891c69776a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
msc.type: authoredcontent
ms.openlocfilehash: b8b05705629b5e8a9acfc5d23517ef1b3cfa7cd6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-c"></a>Düzenleme için doğrulama denetimleri ekleme ve arabirimler (C#) ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_CS.exe) veya [PDF indirin](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/datatutorial19cs1.pdf)

> Bu öğreticide daha iyi bir kullanıcı arabirimi için EditItemTemplate ve InsertItemTemplate Web denetimi, veri doğrulama denetimleri eklemek için ne kadar kolay olduğunu göreceksiniz.


## <a name="introduction"></a>Giriş

Örnekler GridView ve DetailsView denetimlerinde şu üç öğreticileri tüm BoundFields ve CheckBoxFields (Visual Studio tarafından GridView veya DetailsView bir veri kaynağına bağlama sırasında otomatik olarak eklenen alan türleri oluşan geçmiş üzerinden incelediniz Akıllı etiket kontrol). GridView veya DetailsView satırda düzenlerken, salt okunur olmayan bu BoundFields son kullanıcı var olan verileri değiştirebilirsiniz kutularındaki dönüştürülür. Benzer şekilde, ne zaman yeni bir ekleme kayıt DetailsView denetime, bu BoundFields özelliği `InsertVisible` özelliği ayarlanmış `true` (varsayılan), içine kullanıcı yeni kayıttaki alan değerlerini sağlayabilir boş metin kutuları çizilir. Benzer şekilde, standart, salt okunur arabiriminde devre dışıdır, CheckBoxFields, düzenleme ve arabirimleri ekleme Etkin onay kutularını uygulamasına dönüştürülür.

Varsayılan düzenleme ve arabirimleri BoundField ve CheckBoxField ekleme yardımcı olabilirler, ancak herhangi bir tür doğrulaması arabirimi eksik. Bir kullanıcının atlanması gibi bir veri girişi hata - yaptığı varsa `ProductName` alan veya için geçersiz bir değer girme `UnitsInStock` (-50 gibi) bir özel durum gelen uygulama mimarisi derinliklerine içinde gerçekleştirilecektir. Bu özel durumun düzgün biçimde şekilde işlenebilir sırada [önceki öğretici](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md), ideal olarak düzenleme veya kullanıcı arabirimini ekleyerek bir kullanıcı bu tür geçersiz veri girmesini engellemek için doğrulama denetimleri içerir ilk yerleştirin.

Özelleştirilmiş düzenleme veya ekleme arabirimi sağlamak için biz BoundField veya CheckBoxField TemplateField ile değiştirmeniz gerekir. Tartışma konu olan TemplateFields içinde [kullanarak TemplateFields GridView denetiminde](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) ve [kullanarak TemplateFields DetailsView denetiminde](../custom-formatting/using-templatefields-in-the-detailsview-control-cs.md) öğreticileri, katı oluşabilir tanımlama şablonları farklı satır durumları için arabirimleri ayırın. TemplateField's `ItemTemplate` DetailsView veya GridView denetimleri, satırların veya salt okunur alanları işlenirken ancak için kullanılır `EditItemTemplate` ve `InsertItemTemplate` modları, sırasıyla ekleme ve düzenleme için kullanmak üzere arabirimleri gösterir.

Bu öğreticide TemplateField için 's doğrulama denetimleri ekleme ne kadar kolay olduğunu göreceğiz `EditItemTemplate` ve `InsertItemTemplate` daha iyi bir kullanıcı arabirimi için. Özellikle, bu öğreticide oluşturduğunuz örneğini alır [ekleme, güncelleştirme ve silme ile ilişkili olay incelenerek](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) öğretici ve güçlendirir düzenleme ve ekleme arabirimleri uygun doğrulama eklenecek.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-csmd"></a>1. adım: örnekten çoğaltma[ekleme, güncelleştirme ve silme ile ilişkili olayları inceleniyor](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)

İçinde [ekleme, güncelleştirme ve silme ile ilişkili olay incelenerek](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) adları ve düzenlenebilir GridView ürünlerinde fiyatları listelenen bir sayfa oluşturduğumuz öğretici. Ayrıca, sayfa bir DetailsView dahil olan `DefaultMode` özelliği ayarlandığı `Insert`, böylece her zaman işleme ekleme modunda. Bu DetailsView kullanıcı için yeni bir ürün adı ve fiyat girin, Ekle'yi tıklatın ve sistemine eklenmiş olması (bkz: Şekil 1).


[![Önceki örneğe yeni ürünler ekleyin ve varolanları düzenlemek kullanıcılara](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image1.png)

**Şekil 1**: önceki örnek sağlar kullanıcılara yeni ürün ekleme ve düzenleme var olanları ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image3.png))


Amacımız Bu öğretici için doğrulama denetimlerinin sağlamak için DetailsView ve GridView büyütmek olmaktır. Özellikle, doğrulama mantığımızı olur:

- Ekleme veya bir ürün düzenlerken adının sağlanması gerekir
- Fiyat kayıt eklerken sağlanmasını gerektirir; bir kayıt düzenlerken, biz yine bir fiyat gerektirir, ancak programlı mantığı GridView, kullanıcının kullanacağı `RowUpdating` olay işleyicisi önceki öğretici zaten mevcut
- Fiyat için girilen değer geçerli para birimi biçiminde olduğundan emin olun

Biz doğrulama dahil etmek için önceki örnekte program.cs'ye adresindeki bakabilirsiniz önce ilk örnekten çoğaltmak ihtiyacımız `DataModificationEvents.aspx` Bu öğretici için sayfa sayfasına `UIValidation.aspx`. Bu hem kopyalamak için ihtiyacımız gerçekleştirmek için `DataModificationEvents.aspx` sayfanın bildirim temelli biçimlendirme ve kaynak kodu. İlk bildirim temelli biçimlendirme, aşağıdaki adımları gerçekleştirerek kopyalayın:

1. Açık `DataModificationEvents.aspx` Visual Studio'da sayfası
2. Sayfanın bildirim temelli biçimlendirme (sayfanın sonundaki kaynağı düğmesini tıklatın) gidin
3. Metni kopyalayın `<asp:Content>` ve `</asp:Content>` etiketleri (satırları 44 3), Şekil 2'te gösterilen farklı.


[![Metin içinde kopyalama &lt;asp: içerik&gt; denetimi](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image4.png)

**Şekil 2**: metin içinde kopyalama `<asp:Content>` denetimi ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image6.png))


1. Açık `UIValidation.aspx` sayfası
2. Sayfanın bildirim temelli biçimlendirme gidin
3. İçindeki metni yapıştırın `<asp:Content>` denetim.

Kaynak kodu kopyalamak için açık `DataModificationEvents.aspx.cs` sayfasında ve yalnızca metni kopyalayın *içinde* `EditInsertDelete_DataModificationEvents` sınıfı. Üç olay işleyicileri kopyalayın (`Page_Load`, `GridView1_RowUpdating`, ve `ObjectDataSource1_Inserting`), ancak bunu **değil** sınıf bildiriminin kopyalayın veya `using` deyimleri. Kopyalanan metni yapıştırmayı *içinde* `EditInsertDelete_UIValidation` sınıfını `UIValidation.aspx.cs`.

İçerik ve kod üzerinden taşıdıktan `DataModificationEvents.aspx` için `UIValidation.aspx`, bir tarayıcıda ilerleme durumunuzu çıkışı test etmek için bir dakikanızı ayırın. Aynı çıktı ve her iki bu sayfaları aynı işlevselliği deneyimi görmeniz gerekir (geri için bir ekran görüntüsü Şekil 1'e başvurun `DataModificationEvents.aspx` uygulamada).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>2. adım: BoundFields TemplateFields dönüştürme

Doğrulama denetimleri düzenleme ve ekleme arabirimleri eklemek için DetailsView ve GridView denetimleri tarafından kullanılan BoundFields TemplateFields dönüştürülmesi gerekir. Bunu başarmak için GridView ve DetailsView'un akıllı etiketler Edit Columns ve alanları Düzenle bağlantıları sırasıyla tıklayın. Burada, her BoundFields birini seçin ve "Bu alan TemplateField'a dönüştürme" bağlantısını tıklatın.


[![Her DetailsView'un ve GridView'ın BoundFields TemplateFields Dönüştür](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image7.png)

**Şekil 3**: her içine DetailsView'un ve GridView'ın BoundFields TemplateFields dönüştürme ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image9.png))


TemplateField alanları iletişim kutusu üzerinden bir BoundField dönüştürme aynı salt okunur, düzenleme ve ekleme arabirimleri BoundField olarak sergiler TemplateField oluşturur. Bildirim temelli söz dizimi aşağıdaki biçimlendirmeyi gösterir `ProductName` TemplateField'a dönüştürüldükten sonra DetailsView'da alan:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample1.aspx)]

Bu TemplateField otomatik olarak oluşturulmuş üç şablonlar vardı Not `ItemTemplate`, `EditItemTemplate`, ve `InsertItemTemplate`. `ItemTemplate` Bir tek veri alan değeri görüntüler (`ProductName`) bir etiket Web denetimi, kullanırken `EditItemTemplate` ve `InsertItemTemplate` veri alan değeri veri alanı kutusuyla ait ilişkilendiren bir metin kutusu Web denetimi sunmak `Text` iki yönlü veri bağlamasını kullanma özelliği. Biz yalnızca DetailsView bu sayfada eklemek için kullandığından, kaldırabilir `ItemTemplate` ve `EditItemTemplate` iki TemplateFields gelen bunları bırakırlar içinde hiçbir zarar olsa.

GridView DetailsView'un Özellikler ekleme, GridView's dönüştürme yerleşik desteklemediğinden `ProductName` bir TemplateField alanına sonuçlanıyor yalnızca bir `ItemTemplate` ve `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample2.aspx)]

"Dönüştürme bir TemplateField bu alana" düğmesine tıklayarak Visual Studio, şablonları dönüştürülen BoundField kullanıcı arabiriminin taklit TemplateField oluşturdu. Bu, bir tarayıcı aracılığıyla bu sayfasını ziyaret ederek doğrulayabilirsiniz. TemplateFields davranışı ve görünümü olduğunu deneyimine aynı BoundFields yerine kullanıldığında bulabilirsiniz.

> [!NOTE]
> Gerektiğinde şablonlarında düzenleme arabirimleri özelleştirmek çekinmeyin. Örneğin, biz TextBox zorunda isteyebilir `UnitPrice` daha küçük bir metin olarak işlenen TemplateFields `ProductName` metin kutusu. Bunu gerçekleştirmek için metin kutusu 's ayarlayabilirsiniz `Columns` uygun değeri özelliğine veya mutlak bir genişliği aracılığıyla sağlayın `Width` özelliği. Sonraki öğreticide bir alternatif veri girişi Web denetimi TextBox değiştirerek düzenleme arabirimi tamamen özelleştirmeyi göreceğiz.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>3. adım: GridView'ın doğrulama denetimleri ekleme`EditItemTemplate` s

Veri girişi formları oluşturulurken, kullanıcılar gerekli alanları girin ve tüm sağlanan girişleri yasal, düzgün biçimlendirilmiş değerlerinin olduğundan önemlidir. Bir kullanıcının giriş geçerli olduğundan emin olun yardımcı olmak için ASP.NET, tek bir giriş denetiminin değerini doğrulamak için kullanılmak üzere tasarlanmış beş yerleşik doğrulama denetimleri sağlar:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) değeri sağlanmış sağlar
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) bir değeri başka bir Web denetimi değer veya sabit bir değer karşı doğrular veya değerinin biçimi, belirtilen veri türü için geçerli olmasını sağlar
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) değerleri aralığı içinde bir değer olmasını sağlar
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) bir değer karşı doğrular bir [normal ifade](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) bir değer özel, kullanıcı tanımlı bir yöntem karşı doğrular

Bu beş denetimleri hakkında daha fazla bilgi için kullanıma [doğrulama denetimleri bölüm](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) , [ASP.NET Quickstart öğreticileri](https://asp.net/QuickStart/aspnet/).

Bizim öğretici için size bir RequiredFieldValidator DetailsView ve GridView'ın kullanmanız gerekir `ProductName` TemplateFields ve RequiredFieldValidator DetailsView'un içinde `UnitPrice` TemplateField. Ayrıca, şu iki denetimlere bir CompareValidator eklemeniz gerekir `UnitPrice` TemplateFields girilen fiyat 0 değerine eşit veya daha büyük bir değere sahip ve geçerli para birimi biçiminde sunulan sağlar.

> [!NOTE]
> While ASP.NET 1.x sahip aynı bu beş doğrulama denetimleri, ASP.NET 2.0 bazı geliştirmeler eklendi, ana iki istemci tarafı komut dosyası olan Internet Explorer dışında tarayıcılar ve bir sayfaya bölüm doğrulama denetimleri yeteneği desteği doğrulama grupları. 2. 0 doğrulama denetimi yenilikleri hakkında daha fazla bilgi için başvurmak [ASP.NET 2. 0 doğrulama denetimleri Dissecting](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Gerekli doğrulama denetimlerini ekleyerek başlayalım `EditItemTemplate` GridView'ın TemplateFields s. Bunu başarmak için şablon düzenleme arabirimi getirmek için GridView'ın akıllı etiket Şablonları Düzenle bağlantısından tıklayın. Buradan, aşağı açılan listeden düzenlemek için hangi şablonu seçebilirsiniz. Düzenleme arabirimi büyütmek istiyoruz olduğundan, doğrulama denetimleri eklemek ihtiyacımız `ProductName` ve `UnitPrice`'s `EditItemTemplate` s.


[![ProductName ve UnitPrice'nın EditItemTemplates genişletmek ihtiyacımız](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image10.png)

**Şekil 4**: genişletme için ihtiyacımız `ProductName` ve `UnitPrice`'s `EditItemTemplate` s ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image12.png))


İçinde `ProductName` `EditItemTemplate`, yerleştirme sonra metin kutusuna bir RequiredFieldValidator şablon düzenleme arabirimine Araç Kutusu'ndan sürükleyerek ekleyin.


[![Bir RequiredFieldValidator ProductName EditItemTemplate Ekle](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image13.png)

**Şekil 5**: bir RequiredFieldValidator eklemek `ProductName` `EditItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image15.png))


Tüm doğrulama denetimleri, tek bir ASP.NET Web denetim girişi doğrulayarak çalışır. Bu nedenle, az önce eklediğimiz RequiredFieldValidator metin kutusuna karşı doğrulamalıdır belirtmek ihtiyacımız `EditItemTemplate`; bu doğrulama denetiminin ayarlayarak gerçekleştirilir [ControlToValidate özelliğini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) için `ID` uygun Web denetimi. Metin kutusu şu anda yerine nondescript sahip `ID` , `TextBox1`, ancak bunu daha uygun bir şeye değiştirelim. Metin şablonu kutusuna tıklayın ve sonra Özellikler penceresinde değiştirmek `ID` gelen `TextBox1` için `EditProductName`.


[![TextBox'ın kimliği için EditProductName değiştirme](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image16.png)

**Şekil 6**: TextBox's değiştirme `ID` için `EditProductName` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image18.png))


Ardından, RequiredFieldValidator's ayarlayın `ControlToValidate` özelliğine `EditProductName`. Son olarak, ayarlamak [ErrorMessage özelliğini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) "ürün adı sağlamanız gerekir" için ve [metin özelliğini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) için "\*". `Text` Özellik değeri, sağlanan varsa, doğrulama başarısız olursa doğrulama denetimi tarafından görüntülenen metin. `ErrorMessage` , Gerekli özellik değeri ValidationSummary denetimi tarafından; kullanılır `Text` özellik değeri atlanırsa, `ErrorMessage` özellik değeri olduğundan ayrıca geçersiz giriş doğrulama denetimi tarafından görüntülenen metni.

Bu üç RequiredFieldValidator özelliklerini ayarladıktan sonra ekranınızın Şekil 7'ye benzer görünmelidir.


[![RequiredFieldValidator'ın ControlToValidate, ErrorMessage ve metin özelliklerini ayarlama](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image19.png)

**Şekil 7**: RequiredFieldValidator's ayarlamak `ControlToValidate`, `ErrorMessage`, ve `Text` özellikleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image21.png))


Eklenen RequiredFieldValidator ile `ProductName` `EditItemTemplate`, tüm gerekli doğrulama eklemek için kalır olan `UnitPrice` `EditItemTemplate`. Biz, bu sayfa için karar beri `UnitPrice` olan isteğe bağlı olduğunda bir kaydı düzenleme, biz bir RequiredFieldValidator eklemeniz gerekmez. Ancak, emin olmak için bir CompareValidator eklemek ihtiyacımız `UnitPrice`, sağlandıysa, para birimi olarak düzgün biçimlendirildiğinden ve büyük veya 0 değerine eşit.

Biz CompareValidator eklemeden önce `UnitPrice` `EditItemTemplate`, ilk metin kutusuna Web denetimin Kimliğinden değiştirelim `TextBox2` için `EditUnitPrice`. Bu değişikliği yaptıktan sonra CompareValidator eklemek ayarı kendi `ControlToValidate` özelliğine `EditUnitPrice`, kendi `ErrorMessage` "Fiyat değerinden büyük veya sıfıra eşit olmalı ve para birimi simgesini içeremez", özellik ve kendi `Text` özelliğine "\*".

Belirtmek için `UnitPrice` değeri sıfırdan büyük veya 0 değerine eşit, CompareValidator's ayarlamak [işleci özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) için `GreaterThanEqual`, kendi [ValueToCompare özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) "0" ve onun [ Türü özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) için `Currency`. Aşağıdaki bildirim temelli söz dizimini gösterir `UnitPrice` TemplateField'ın `EditItemTemplate` bu değişiklikleri yaptıktan sonra:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample3.aspx)]

Bu değişiklikleri yaptıktan sonra sayfasını bir tarayıcıda açın. Adını Atla veya bir ürün düzenlerken geçersiz Fiyat değerini girin denerseniz, textbox yanındaki bir yıldız işareti görünür. Şekil 8'de görüldüğü gibi $19.95 gibi para birimi simgesini içeren bir fiyat değer geçersiz olarak kabul edilir. CompareValidator's `Currency` `Type` basamak ayırıcıları (örneğin, virgül veya nokta kültür ayarlarına bağlı olarak) ve bir başında artı veya eksi sağlar, ancak mu *değil* para birimi simgesini izin verir. Şu anda düzenleme arabirimi işler gibi bu davranış kullanıcılar perplex `UnitPrice` para birimi biçimi kullanarak.

> [!NOTE]
> Uygulamasında geri çağırma *ekleme, güncelleştirme ve silme ile ilişkili olayları* BoundField's ayarlarız öğretici `DataFormatString` özelliğine `{0:c}` para birimi olarak biçimlendirmek için. Ayrıca, ayarlarız `ApplyFormatInEditMode` özelliği true, biçimlendirmek için arabirimi düzenleme kullanıcının GridView neden `UnitPrice` bir para birimi olarak. Visual Studio BoundField TemplateField'a dönüştürülürken bu ayarları not ettiğiniz ve TextBox's biçimlendirilmiş `Text` özelliği veri bağlama sözdizimini kullanarak bir para birimi olarak `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Geçersiz giriş ile kutularındaki yanında bir yıldız işareti görünür](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image22.png)

**Şekil 8**: bir yıldız işareti görünür sonraki geçersiz giriş içeren metin kutuları için ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image24.png))


While olarak doğrulama çalışır-olduğundan, kullanıcının sahip olduğu kabul edilebilir değil bir kayıt düzenlerken para birimi simgesini el ile kaldırmak. Bu sorunu gidermek için şu üç seçeneğiniz vardır:

1. Yapılandırma `EditItemTemplate` böylece `UnitPrice` değeri para birimi olarak biçimlendirilmemiş.
2. Kullanıcının CompareValidator kaldırarak ve düzgün şekilde düzgün biçimlendirilmiş para birimi değeri için denetler RegularExpressionValidator ile değiştirerek bir para birimi simgesini girin izin verin. Buradaki sorun para birimi değeri doğrulamak için normal ifade kolay değildir ve kültür ayarları içerecek şekilde istediyseniz kod yazma gerektirir.
3. Doğrulama denetimi tamamen kaldırın ve GridView'ın sunucu tarafı doğrulama mantığı Bel `RowUpdating` olay işleyicisi.

Bu alıştırma için #1 seçeneğiyle edelim. Şu anda `UnitPrice` metin kutusunda veri bağlama deyimi nedeniyle bir para birimi olarak biçimlendirilmiş `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Bağ ifadesini değiştirin `Bind("UnitPrice", "{0:n2}")`, bir dizi duyarlık iki basamaklı sonucu biçimlendirir. Bu bildirim temelli söz dizimi aracılığıyla doğrudan veya veri bağlamaları Düzenle bağlantıyı tıklatarak yapılabilir `EditUnitPrice` metin kutusuna `UnitPrice` TemplateField'ın `EditItemTemplate` (bkz: Şekil 9 ve 10).


[![TextBox'ın veri bağlamaları Düzenle bağlantısına tıklayın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image25.png)

**Şekil 9**: metin kutusu'nın veri bağlamaları Düzenle bağlantısına tıklayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image27.png))


[![Bağ deyiminde biçim belirteci belirtin](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image28.png)

**Şekil 10**: biçim belirteci belirtin `Bind` deyimi ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image30.png))


Bu değişikliği düzenleme arabiriminde biçimlendirilmiş Fiyat Grup ayırıcı olarak virgül ve ondalık ayırıcı olarak nokta içerir, ancak para birimi simgesini devre dışı bırakır.

> [!NOTE]
> `UnitPrice` `EditItemTemplate` Oluşması için geri gönderme ve başlamak için güncelleştirme mantık sağlayan bir RequiredFieldValidator içermez. Ancak, `RowUpdating` üzerinden kopyalanan olay işleyicisi *ekleme, güncelleştirme ve silme ile ilişkili olay incelenerek* öğretici içerir sağlar programlı bir onay `UnitPrice` sağlanır. Bu mantık kaldırmak için bunu olarak bırakın çekinmeyin-, veya bir RequiredFieldValidator eklemeyi `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>4. adım: Veri girişi sorunlarını özetleme

Beş doğrulama denetimleri ek olarak, ASP.NET içerir [ValidationSummary denetimi](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), görüntüleyen `ErrorMessage` geçersiz veriler algılandı bu doğrulama denetimleri s. Bu özet verilerini kalıcı, istemci-tarafı messagebox aracılığıyla ya da web sayfasında metin olarak görüntülenebilir. Şimdi tüm doğrulama sorunlarını özetlemeye bir istemci-tarafı messagebox dahil etmek için bu öğreticiyi geliştirin.

Bunu gerçekleştirmek için araç kutusu tasarımcıya ValidationSummary denetimi sürükleyin. Yalnızca Özet messagebox görüntülemek için yapılandırma oluşturacağız beri doğrulama denetimi konumunu gerçekten, önemli değildir. Denetim ekledikten sonra ayarlamak kendi [ShowSummary özelliğinin](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) için `false` ve kendi [ShowMessageBox özelliğini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) için `true`. Bu ayrıca ile tüm doğrulama hatalarını bir istemci-tarafı messagebox özetlenmiştir.


[![Doğrulama hatalarını bir istemci-tarafı Messagebox özetlenir](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image31.png)

**Şekil 11**: doğrulama hataları, bir istemci-tarafı Messagebox özetlenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>5. adım: DetailsView'un doğrulama denetimleri ekleme`InsertItemTemplate`

Kalan Bu öğretici için tek şey DetailsView'un ekleme arabirim doğrulama denetimleri ekleme. DetailsView'un şablonlarını doğrulama denetimleri ekleme işlemi, adım 3'te incelenmesi aynıdır; Bu nedenle, biz Bu adımda görev aracılığıyla breeze. GridView ile 's yaptığımız gibi `EditItemTemplate` s, ı teşvik edin, yeniden adlandırmak için `ID` kutularındaki metinleri nondescript s `TextBox1` ve `TextBox2` için `InsertProductName` ve `InsertUnitPrice`.

Bir RequiredFieldValidator eklemek `ProductName` `InsertItemTemplate`. Ayarlama `ControlToValidate` için `ID` TextBox şablonunda, kendi `Text` özelliğine "\*" ve kendi `ErrorMessage` "ürün adı sağlamanız gerekir" özelliği.

Bu yana `UnitPrice` olan yeni bir kayıt eklerken bu sayfa için gerekli bir RequiredFieldValidator eklemek `UnitPrice` `InsertItemTemplate`, ayar kendi `ControlToValidate`, `Text`, ve `ErrorMessage` özellikleri uygun şekilde. Son olarak, bir CompareValidator eklemek `UnitPrice` `InsertItemTemplate` de yapılandırma, `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, ve `ValueToCompare` ile yaptığımızgibiözellikleri`UnitPrice`'s GridView'ın CompareValidator `EditItemTemplate`.

Bu doğrulama denetimleri eklendikten sonra yeni bir ürün için sistem adı belirtilmedi veya kendi fiyat negatif bir sayı ise, eklenemez veya yasadışı olarak biçimlendirilmiş.


[![Doğrulama mantığını DetailsView'un ekleme arabirimine eklendi](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image34.png)

**Şekil 12**: doğrulama mantığını DetailsView'un ekleme arabirimine eklendi ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>6. adım: doğrulama denetimleri doğrulama gruplar halinde bölümlendirme

İki mantıksal olarak farklı doğrulama denetimleri kümesi sayfamızı oluşur: GridView karşılık gelen olanlar arabirimi düzenleme ve DetailsView'a karşılık gelen olanlar arabirimi ekleme. Varsayılan olarak geri gönderimin ortaya çıktığında, *tüm* sayfasında doğrulama denetimleri denetlenir. Ancak, bir kayıt düzenlerken biz doğrulamak için doğrulama denetimlerinin DetailsView'un ekleme arabiriminin istemezsiniz. Şekil 13 kullanıcı mükemmel geçerli değerleri olan bir ürün düzenlerken bizim geçerli ikilemini gösterilmektedir, adı ve fiyat değerlerini ekleme arabiriminde boş olduğundan tıklatmak güncelleştirme bir doğrulama hatası neden olur.


[![Bir ürün güncelleştirme ekleme arabiriminin doğrulama denetimleri için yangın neden olur.](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image37.png)

**Şekil 13**: ürün güncelleştirme neden olan ekleme arabiriminin doğrulama denetimleri için yangın ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image39.png))


ASP.NET 2. 0 doğrulama denetimleri doğrulama gruplara bölümlenebilir kendi `ValidationGroup` özelliği. Bir gruptaki doğrulama denetimleri kümesini ilişkilendirmek için yalnızca ayarlamak kullanıcıların `ValidationGroup` özelliği aynı değere. Bizim öğretici için ayarlanmış `ValidationGroup` GridView'ın TemplateFields doğrulama denetimleri özelliklerini `EditValidationControls` ve `ValidationGroup` DetailsView'un TemplateFields özelliklerini `InsertValidationControls`. Bu değişiklikler doğrudan bildirim temelli biçimlendirmede yapılabilir veya şablon arabirimi tasarımcının kullanırken Özellikler penceresi düzenleyin.

Ek olarak doğrulama denetimleri, düğmesi ve ASP.NET 2.0 düğmesi ilgili denetimlerinde de dahil bir `ValidationGroup` özelliği. Bir doğrulama grubun doğrulayıcıları geçerliliğini denetlenir yalnızca zaman geri gönderimin aynı olan bir düğme tarafından kopyaladığınızda `ValidationGroup` özellik ayarı. Örneğin, tetiklemek DetailsView'un Ekle düğmesi için sırayla `InsertValidationControls` ihtiyacımız CommandField's ayarlamak için doğrulama grubu `ValidationGroup` özelliğine `InsertValidationControls` (Şekil 14 bakın). Ayrıca, GridView's ayarlamak CommandField'ın `ValidationGroup` özelliğine `EditValidationControls`.


[![CommandField'ın ValidationGroup InsertValidationControls özelliğine kümesi DetailsView kullanıcının](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image40.png)

**Şekil 14**: DetailsView'un ayarlamak CommandField'ın `ValidationGroup` özelliğine `InsertValidationControls` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image42.png))


Bu değişikliklerden sonra DetailsView GridView'ın TemplateFields ve CommandFields aşağıdakine benzer görünmelidir:

DetailsView'un TemplateFields ve CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample4.aspx)]

GridView'ın CommandField ve TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample5.aspx)]

Bu noktada yalnızca DetailsView'un Ekle düğmesi, Şekil 13 tarafından vurgulanmış sorunu çözme tıklandığında Düzenle-özel doğrulama denetimleri yalnızca GridView'ın güncelleştirme düğmesine tıklandığında ve INSERT özgü doğrulama denetimleri yangın kov. Ancak, bu değişikliği bizim ValidationSummary denetimi artık geçersiz veri girerken görüntüler. ValidationSummary denetimi de içeren bir `ValidationGroup` özelliği ve yalnızca Özet bilgiler görüntüler bu doğrulama denetimleri, doğrulama grubu için. Bu nedenle, bu sayfada bir iki doğrulama denetimleri ihtiyacımız `InsertValidationControls` doğrulama grubu, diğeri de `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample6.aspx)]

Bu eklenmesiyle öğreticimizi tamamlandı!

## <a name="summary"></a>Özet

BoundFields hem ekleme ve düzenleme arabirim sağlayabilir, ancak arabirimi özelleştirilebilir değil. Genellikle, düzenleme ve kullanıcının geçerli bir biçimde gerekli girişleri girdiğinden emin olmak için arabirimi ekleme doğrulama denetimleri eklemek istiyoruz. Bunu gerçekleştirmek için biz BoundFields TemplateFields dönüştürmek ve uygun şablonların doğrulama denetimleri eklemeniz gerekir. Bu öğreticide biz örnekten Genişletilmiş *ekleme, güncelleştirme ve silme ile ilişkili olay incelenerek* öğretici DetailsView için doğrulama denetimleri ekleme, ekleme arabirimi ve GridView'ın düzenleme arabirimi. Ayrıca, ValidationSummary denetimi kullanarak Özet doğrulama bilgilerini görüntülemek nasıl ve nasıl ayrı doğrulama gruplar halinde doğrulama denetimleri sayfada bölümlenir gördük.

Bu öğreticide gördüğümüz gibi TemplateFields doğrulama denetimleri içerecek şekilde engagement'ta düzenleme ve ekleme arabirimi izin verir. TemplateFields ayrıca ek giriş Web denetimleri, dahil etmek için daha uygun bir Web denetimi tarafından değiştirilecek TextBox etkinleştirme genişletilebilir. TextBox denetimi yabancı anahtar düzenlerken idealdir bir veriye bağlı DropDownList denetimi ile değiştirme göreceğiz bizim sonraki öğreticide (gibi `CategoryID` veya `SupplierID` içinde `Products` tablo).

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Liz Shulok ve Zack CAN yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
[sonraki](customizing-the-data-modification-interface-cs.md)
