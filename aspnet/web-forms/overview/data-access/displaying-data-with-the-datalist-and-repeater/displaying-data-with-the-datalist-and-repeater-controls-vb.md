---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
title: DataList ve Repeater denetimleri (VB) ile verileri görüntüleme | Microsoft Docs
author: rick-anderson
description: Önceki öğreticilerde biz GridView denetiminde verileri görüntülemek için kullandınız. Bu öğretici ile başlayarak, ortak raporlama desenler oluşturmayı hazırız...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 58618954-a9ed-4ca0-8c2d-95a5ffd9c03e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c6e4f8588fb8da2e9703f5de0032e6a21bb5a28
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397671"
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-vb"></a>DataList ve Repeater denetimleri (VB) ile verileri görüntüleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_VB.exe) veya [PDF olarak indirin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/datatutorial29vb1.pdf)

> Önceki öğreticilerde biz GridView denetiminde verileri görüntülemek için kullandınız. Bu öğretici ile başlayarak, ortak raporlama desenleri DataList ve Repeater denetimleri ile geliştirilmesine yönelik bu denetimleri ile verileri görüntüleme temellerini başlayarak bakacağız.


## <a name="introduction"></a>Giriş

Tüm geçmiş örneklerinde biz GridView denetimi için etkinleştirilmiş bir veri kaynağındaki birden çok kayıtları görüntülemek gerekirse 28 öğreticiler. GridView Sütun kayıt s veri alanlarını görüntülemek veri kaynağındaki her kayıt için bir satır oluşturur. Bir ek kolaylaştırır GridView olmakla görüntüleme, sayfası aracılığıyla, sıralama, düzenleme ve silme veri görünümünü biraz boxy arasındadır. Ayrıca, GridView s yapısı sabit için bir HTML biçimlendirmeyi sorumlu içerir `<table>` tablo satırı ile (`<tr>`) her bir kayıt ve tablo hücresi (`<td>`) her bir alan için.

Birden çok kayıt görüntülenirken büyük ölçüde özelleştirme ve biçimlendirmenin sağlamak için ASP.NET 2.0 DataList ve Repeater denetimleri sunar (ikisi de aynı zamanda ASP.NET sürümde 1.x). DataList ve Repeater denetimleri içeriklerini BoundFields, CheckBoxFields, ButtonFields, yerine şablonları oluşturma ve benzeri. GridView'gibi bir HTML olarak DataList işler `<table>`, ancak için birden çok veri kaynağı kayıtların tabloda satır görüntülenmesini sağlar. Yineleyici, diğer taraftan, ne, açıkça belirtin ve ideal bir aday yayılan biçimlendirme kesin denetime ihtiyacınız olduğunda ek biçimlendirme yok işler.

Sonraki düzine veya bunu öğreticiler DataList ve Repeater denetimleri ile sık kullanılan raporlama desenlerini geliştirilmesine yönelik bu denetimler şablonları ile verileri görüntüleme temellerini başlayarak inceleyeceğiz. Bu denetimler biçimine görüyoruz DataList, ana/Ayrıntılar senaryoları, şekilde düzenlemek ve verileri silmek için verileri kaynak kayıtları düzenini değiştirmek nasıl kayıtlarda sayfasında ve benzeri.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>1. adım: DataList ve Repeater Eğitmen Web sayfaları ekleme

Biz bu öğreticiye başlamadan önce ilk yapmamız Bu öğretici ve DataList ve Repeater'ı kullanarak veri görüntüleme ile ilgili sonraki birkaç öğreticiler için gereken ASP.NET sayfaları eklemek için bir dakikanızı ayırarak s olanak tanır. Adlı projede yeni bir klasör oluşturarak başlayın `DataListRepeaterBasics`. Ardından, tüm bunları ana sayfaya kullanacak şekilde yapılandırılmış olması ve bu klasörü için aşağıdaki beş ASP.NET sayfaları ekleyin `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![DataListRepeaterBasics bir klasör oluşturun ve öğretici ASP.NET sayfaları ekleme](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image1.png)

**Şekil 1**: oluşturma bir `DataListRepeaterBasics` klasörü ve öğretici ASP.NET sayfaları ekleyin


Açık `Default.aspx` sürükleyin ve sayfa `SectionLevelTutorialListing.ascx` kullanıcı denetimi `UserControls` tasarım yüzeyine klasör. Bu kullanıcı, oluşturduğumuz denetimini [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-vb.md) öğretici, site haritası numaralandırır ve madde işaretli listede geçerli bölümdeki öğreticiler görüntüler.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image2.png)

**Şekil 2**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image4.png))


Madde işaretli liste görünümünü sahip olmak için biz oluşturursunuz, DataList ve Repeater öğreticiler site eşlemesinin ekleneceği ihtiyacımız var. Açık `Web.sitemap` dosya ve sonra özel düğmeler ekleme site haritası düğüm biçimlendirme aşağıdaki işaretlemeyi ekleyin:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample1.xml)]


![Yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image5.png)

**Şekil 3**: yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir


## <a name="step-2-displaying-product-information-with-the-datalist"></a>2. adım: DataList'i ile ürün bilgileri görüntüleme

FormView benzer şablonları yerine BoundFields, CheckBoxFields ve benzeri DataList denetimi işlenmiş çıktı s bağlıdır. FormView DataList insanı yalnızlaştırıcı bir tane yerine kayıt kümesini görüntülemek için tasarlanmıştır. Ürün bilgileri için bir DataList bağlama göz ile bu öğreticiye başlamadan s olanak tanır. Başlangıç açarak `Basics.aspx` sayfasını `DataListRepeaterBasics` klasör. Ardından, bir DataList tasarımcı araç kutusundan sürükleyin. DataList s şablonları belirtmeden önce Şekil 4'te gösterildiği gibi tasarımcı gri bir kutu olarak gösterir.


[![DataList tasarımcı araç kutusundan sürükleyin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image6.png)

**Şekil 4**: DataList gelen araç kutusu üzerine tasarımcıya sürükleyin ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image8.png))


S DataList akıllı etiketi, yeni ObjectDataSource ekleyin ve kullanmak üzere yapılandırma `ProductsBLL` s sınıfı `GetProducts` yöntemi. Sihirbazı s Ekle (hiçbiri) açılan listeye Bu öğreticide, bir salt okunur DataList oluşturma re ayarladığımız olduğundan, güncelleştirme ve sekmeleri SİLİN.


[![Yeni bir ObjectDataSource oluşturmak için iyileştirilmiş](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image9.png)

**Şekil 5**: Create yeni ObjectDataSource Opt ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image11.png))


[![ObjectDataSource ProductsBLL sınıfını kullanmak için yapılandırma](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image12.png)

**Şekil 6**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image14.png))


[![Tüm ürünleri GetProducts yöntemi kullanma hakkında bilgi alın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image15.png)

**Şekil 7**: alma bilgileri hakkında tüm ürünleri kullanarak `GetProducts` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image17.png))


ObjectDataSource yapılandırma ve DataList aracılığıyla akıllı etiketinde ile ilişkilendirmeden sonra Visual Studio otomatik olarak oluşturma bir `ItemTemplate` adını ve veri kaynağı tarafından döndürülen her veri alanının değerini görüntüler DataList'te (bkz: Biçimlendirme aşağıdaki). Bu varsayılan `ItemTemplate` görünümünü, bir veri kaynağı Tasarımcısı aracılığıyla FormView bağlanırken otomatik olarak oluşturulan şablonlarını aynıdır.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample2.aspx)]

> [!NOTE]
> FormView denetimi aracılığıyla FormView s akıllı etiket için bir veri kaynağına bağlanırken, Visual Studio'nun oluşturduğu Hatırlayacağınız bir `ItemTemplate`, `InsertItemTemplate`, ve `EditItemTemplate`. DataList, ancak ile yalnızca bir `ItemTemplate` oluşturulur. DataList düzenleme ve FormView tarafından sunulan destek ekleme yerleşik olmadığından budur. DataList düzenleme ve silme ilgili olayları içermiyor ve düzenleme ve silme desteği basit kullanıma hazır desteği yok olarak FormView ile biraz kod ancak orada s ile eklenebilir. Düzenleme ve Destek DataList'i ile bir sonraki öğreticide silme ekleme göreceğiz.


Bu şablon görünüşünü iyileştirmek için birkaç dakikanızı s olanak tanır. Tüm veri alanlarını görüntülemek yerine, yalnızca ürün adı, tedarikçi, kategori, birim ve birim fiyatı başına miktarını Göster s olanak tanır. Ayrıca, let s görünen adı içinde bir `<h4>` başlık ve yerleşim kalan alanları kullanarak bir `<table>` altındaki başlığı.

Yapabilecekleriniz bu değişiklikleri yapmak için etiket Şablonları Düzenle bağlantısına tıklayın veya şablon sayfasına s bildirim temelli söz dizimi aracılığıyla el ile değiştirebilirsiniz akıllı s DataList Tasarımcısından özelliklerini düzenleme şablonu kullanın. Tasarımcıda Şablonları Düzenle seçeneğini kullanırsanız, sonuçta elde edilen biçimlendirme aşağıdaki biçimlendirme tam olarak eşleşmiyor olabilir, ancak bir tarayıcı aracılığıyla görüntülendiğinde Şekil 8'de gösterilen ekran çok benzer görünmelidir.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample3.aspx)]

> [!NOTE]
> Etiket Web özelliği kullanan yukarıdaki örnekte denetimleri `Text` özelliği, veri bağlama söz dizimi değeri atanır. Alternatif olarak, biz etiketleri tamamen yeni bağlama söz diziminde yazarak atlamış. Diğer bir deyişle, yerine `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` biz bunun yerine bildirim temelli söz dizimi kullanabilirdik `<%# Eval("CategoryName") %>`.


Etiket Web denetimlerinde bırakır, ancak iki avantaj sunar. İlk olarak, sonraki öğreticide anlatıldığı gibi verileri temel alan veri biçimlendirme için kolay bir yol sağlar. İkinci olarak, Şablonları Düzenle seçeneğini Tasarımcısı eklenmemişse t görünen bildirim temelli veri bağlama söz diziminde bazı Web denetimi dışında görünür. Bunun yerine, Düzen şablonları arabirimi static işaretleme ile çalışmayı kolaylaştırmak için tasarlanmıştır ve Web denetler ve tüm veri bağlama Web denetimleri akıllı etiketleri erişilebilir olan veri bağlamaları Düzenle iletişim kutusu üzerinden yapılır varsayar.

Bu nedenle, Tasarımcı şablonlarını düzenleme seçeneği sağlayan DataList'i ile çalışırken içeriği Düzen şablonları arabirimle erişilebilen, böylece etiket Web denetimlerini kullanmaya tercih ediyorum. Kısa bir süre içinde anlatıldığı gibi yineleyici şablonu s içeriği kaynağı görünümünden düzenlenmesi gerekir. Sonuç olarak, etiket Web atlamak genellikle Repeater s şablonları kaynaklı biçimlendirmeniz gerekir bilmiyorsanız denetimleri, veri görünümünü programlama mantığı temelinde metin bağlı.


[![Her ürün s çıkış işlenen s ItemTemplate DataList kullanmaktır](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image18.png)

**Şekil 8**: her ürün çıkış s'dir işlenen kullanarak s DataList `ItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>3. adım: DataList görünümünü iyileştirme

GridView gibi DataList stil özellikleri gibi sunar `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`ve benzeri. Dış Görünüm dosyaları GridView ve DetailsView denetimlerini ile çalışırken, oluşturduğumuz `DataWebControls` önceden tanımlanmış tema `CssClass` bu iki denetimin özelliklerini ve `CssClass` birkaç kendi alt özelliği (`RowStyle` `HeaderStyle`, vb.). DataList için bunu s olanak tanır.

Bölümünde açıklandığı gibi [görüntüleyen veri ile ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) öğretici, bir dış görünüm dosyası Web denetimi için varsayılan görünüm ile ilgili özellikleri belirtir; bir temayı tanımlayan görünümü, CSS, görüntü ve JavaScript dosyaları koleksiyonudur bir Web sitesi için belirli bir görünümü ve deneyimini. İçinde *görüntüleyen veri ile ObjectDataSource* öğreticide oluşturduğumuz bir `DataWebControls` tema (bir klasör içinde uygulanan `App_Themes` klasör), şu anda iki dış görünüm dosyaları - olan `GridView.skin` ve `DetailsView.skin`. Üçüncü bir DataList önceden tanımlanmış stil ayarlarını belirtmek için dış görünüm dosyası ekleme s olanak tanır.

Dış görünüm dosyası eklemek için sağ `App_Themes/DataWebControls` klasöründe yeni bir öğe Ekle'yi seçin ve listeden soubor Skinu seçeneğini belirleyin. Dosya adı `DataList.skin`.


[![DataList.skin adlı yeni bir dış görünüm dosyası oluşturma](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image21.png)

**Şekil 9**: yeni bir dış dosya adlı oluşturma `DataList.skin` ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image23.png))


Kullanmak için aşağıdaki biçimlendirme `DataList.skin` dosyası:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample4.aspx)]

Bu ayarları aynı CSS sınıfları ile GridView ve DetailsView denetimlerini kullanılmış gibi uygun DataList özelliklerine atamanız yeterlidir. Burada kullanılan CSS sınıfları `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`ve benzeri tanımlanan `Styles.css` dosyasını ve önceki öğreticilerdeki eklendi.

Bu dış görünüm dosyası eklenmesiyle DataList görünümünü (yeni dış görünüm dosyası; Görünüm menüsünden etkileri görmek için Yenile'yi seçin Tasarımcı görünümü yenilemeniz gerekebilir) tasarımcıda güncelleştirilir. Şekil 10 gösterildiği gibi değişen her ürünün bir açık pembe arka plan rengi vardır.


[![DataList.skin adlı yeni bir dış görünüm dosyası oluşturma](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image24.png)

**Şekil 10**: yeni bir dış dosya adlı oluşturma `DataList.skin` ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>4. adım: DataList s diğer şablonlarını keşfetme

Ek olarak `ItemTemplate`, altı isteğe bağlı şablonları DataList destekler:

- `HeaderTemplate` sağlanırsa, çıktıyı bir üst bilgi satırı ekler ve bu satırı işlemek için kullanılır
- `AlternatingItemTemplate` değişen öğeleri işlemek için kullanılan
- `SelectedItemTemplate` Seçili öğenin işlemek için kullanılır. Seçili öğe dizini karşılık gelen bir DataList s öğedir [ `SelectedIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` düzenlenmekte olan öğenin işlemek için kullanılan
- `SeparatorTemplate` sağlanırsa, her bir öğe arasındaki ayırıcı ekler ve bu ayırıcı oluşturmak için kullanılır.
- `FooterTemplate` -sağlanırsa, çıktıyı bir alt bilgi satır ekler ve bu satırı işlemek için kullanılır

Belirtirken `HeaderTemplate` veya `FooterTemplate`, DataList işlenen çıkışı için ek bir üstbilgi ve altbilgisindeki satır ekler. Gibi GridView s üstbilgi ve altbilgi ile satır, başlık ve DataList altbilgisinde veri bağlı değildir. Bu nedenle, tüm veri bağlama sözdiziminde `HeaderTemplate` veya `FooterTemplate` bağlı veri erişim girişimlerini boş bir dize döndürür.

> [!NOTE]
> İçinde gördüğümüz gibi [GridView s altbilgi özeti bilgilerini görüntüleme](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) üstbilgi ve altbilgi satırları t destek veri bağlama söz dizimi, verilere özgü bilgileri ki sırada Öğreticisi eklenen bu satır, doğrudan GridView s `RowDataBound` olay işleyicisi. Bu teknik yapmak için kullanılabilir hem de çalışan toplamları hesaplamak veya verileri diğer bilgiler denetime bağlı hem de alt bilgi için bu bilgileri atayın. Aynı kavram DataList ve Repeater denetimleri için uygulanabilir; DataList ve Repeater için bir olay işleyicisi oluşturmak için tek fark, `ItemDataBound` olay (yerine için `RowDataBound` olay).


Bizim örneğimizde, let s, ürün üst kısmında DataList s sonuçlarında görüntülenen bilgiler başlığına sahip bir `<h3>` başlığı. Bunu gerçekleştirmek için ekleme bir `HeaderTemplate` uygun biçimlendirmeye sahip. Tasarımcısı'ndan bu DataList s akıllı etiket Şablonları Düzenle bağlantısına tıklayarak, aşağı açılan listeden üstbilgi şablonu seçme ve stili açılan 3 başlığı seçeneğinden seçtikten sonra metin yazarak gerçekleştirilebilir (bkz. Şekil 11) listesi.


[![Metin ürün bilgileri içeren bir HeaderTemplate Ekle](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image27.png)

**Şekil 11**: ekleme bir `HeaderTemplate` metin ürün bilgilerle ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image29.png))


Alternatif olarak, bu bildirimli olarak aşağıdaki biçimlendirme içinde girerek eklenebilir `<asp:DataList>` etiketler:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample5.html)]

Bir bit alanı arasındaki her ürün listesi eklemek için Ekle s izin bir `SeparatorTemplate` , her bölüm arasında bir satır içerir. Yatay cetvel etiketi (`<hr>`), böyle bir ayırıcı ekler. Oluşturma `SeparatorTemplate` aşağıdaki biçimlendirme sahip olacak şekilde:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample6.html)]

> [!NOTE]
> Gibi `HeaderTemplate` ve `FooterTemplates`, `SeparatorTemplate` herhangi bir kayda veri kaynağından bağlı değil ve bu nedenle doğrudan erişim veri kaynağının kayıtları için DataList bağlı olamaz.


Bu ayrıca, yaptıktan sonra bir tarayıcı aracılığıyla sayfayı görüntülerken Şekil 12'ye benzer görünmelidir. Üst bilgi satırı ve her ürün listesi arasındaki unutmayın.


[![DataList üst bilgi satırı ve her ürün listesi arasındaki yatay bir kural içerir](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image30.png)

**Şekil 12**: üst bilgi satırı ve bir yatay kuralı arasındaki her ürün listesi DataList içerir ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>5. adım: Repeater denetimiyle belirli biçimlendirme işleme

Şekil 12 DataList örnekten ziyaret edildiğinde görüntüle/kaynak tarayıcınızdan eklemeyin; eklerseniz DataList HTML yayan görürsünüz `<table>` içeren bir tablo satırının (`<tr>`) sahip tek tablo hücresi (`<td>`) bağlı her öğe için DataList. Bu çıkış, aslında ne GridView tek TemplateField ile gelen yayılan için aynıdır. Bir sonraki öğreticide anlatıldığı gibi DataList bize tablo satır başına birden çok veri kaynağı kayıt görüntülemek etkinleştirme çıktının daha fazla özelleştirme izin vermiyor.

T, bir HTML yayma istiyorsanız ne istemiyorsunuz `<table>`, ancak? Veri Web denetimi tarafından oluşturulan işaretleme üzerinde toplam ve tam denetimi için biz Repeater denetiminde kullanmanız gerekir. DataList gibi şablonları alan bir yineleyici oluşturulur. Yineleyici ancak yalnızca şu beş şablonlarını sunar:

- `HeaderTemplate` sağlanan, öğelerin belirtilen biçimlendirme ekler.
- `ItemTemplate` öğeleri işlemek için kullanılan
- `AlternatingItemTemplate` sağlanırsa, değişen öğeleri işlemek için kullanılan
- `SeparatorTemplate` sağlanan, her bir öğe arasındaki belirtilen biçimlendirme ekler.
- `FooterTemplate` -sağlanırsa, belirtilen biçimlendirme öğelerden sonra ekler.

ASP.NET'te 1.x, denetim verisini bazı veri kaynağından gelen bir madde işaretli liste görüntülemek için yaygın olarak kullanılan yineleyici. Böyle bir durumda `HeaderTemplate` ve `FooterTemplates` açılış ve kapanış içerecektir `<ul>` etiketleri, sırasıyla while `ItemTemplate` içerecektir `<li>` veri bağlama söz dizimi ile öğeleri. İki örneklerde de gördüğümüz gibi bu yaklaşım hala ASP.NET 2.0 sürümünde kullanılabilir [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-vb.md) Öğreticisi:

- İçinde `Site.master` ana sayfa üst düzey site haritası içeriği (temel raporlama, filtreleme raporları, özelleştirilmiş biçimlendirme ve benzeri) madde işaretli bir listesini görüntülemek için bir yineleyici kullanıldı; başka bir, iç içe geçmiş Repeater alt bölümlerini görüntülemek için kullanılan üst düzey bölümleri
- İçinde `SectionLevelTutorialListing.ascx`, geçerli site eşlemesi bölümü alt bölümlerini madde işaretli bir listesini görüntülemek için bir yineleyici kullanıldı

> [!NOTE]
> ASP.NET 2.0 tanıtır yeni [Bulletedlıst denetimi](https://msdn.microsoft.com/library/ms228101.aspx), hangi bağlanabilir veri kaynak denetimi için basit bir madde işaretli liste görüntülemek için. Bulletedlıst denetimiyle biz listeyle ilgili HTML belirtmek gerekmez; Bunun yerine, biz yalnızca her liste öğesine ilişkin metin olarak görüntülenecek veri alanı belirtin.


Yineleyici bir catch tüm verileri Web denetimi işlevi görür. Repeater denetimiyle gerekli biçimlendirmeleri oluşturur, varolan bir denetimi değilse kullanılabilir. Yineleyici kullanarak göstermek için ürün bilgi 2. adımda oluşturulan DataList yukarıda görüntülenen kategorileri listesine sahip s olanak tanır. Özellikle, let s sahip tek satır HTML olarak görüntülenen kategorileri `<table>` tablodaki bir sütun olarak görüntülenen her kategorisi.

Bunu yapmak için ürün bilgilerini DataList yukarıda Tasarımcısı araç kutusundan Repeater denetimiyle sürükleyerek başlatın. Kendi şablonları tanımlanan kadar bir DataList olduğu gibi yineleyici başlangıçta gri bir kutu olarak görüntüler.


[![Repeater'da tasarımcıya ekleyin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image33.png)

**Şekil 13**: Repeater'da tasarımcıya eklemek ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image35.png))


Burada s yalnızca bir yineleyici s akıllı etiket seçeneğinde: veri kaynağı seçin. Yeni bir ObjectDataSource oluşturmak ve kullanmak üzere yapılandırmak için iyileştirilmiş `CategoriesBLL` s sınıfı `GetCategories` yöntemi.


[![Yeni bir ObjectDataSource oluşturma](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image36.png)

**Şekil 14**: yeni ObjectDataSource oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image38.png))


[![ObjectDataSource CategoriesBLL sınıfını kullanmak için yapılandırma](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image39.png)

**Şekil 15**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image41.png))


[![Tüm GetCategories yöntemi kullanarak kategorileri hakkında bilgi alın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image42.png)

**Şekil 16**: alma bilgileri hakkında tüm kategorileri kullanarak `GetCategories` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image44.png))


DataList farklı olarak, Visual Studio otomatik olarak bir ItemTemplate yineleyici için bir veri kaynağına bağlandıktan sonra oluşturmaz. Ayrıca, yineleyici s şablonları Tasarımcısı ile yapılandırılamaz ve bildirimli olarak belirtilmelidir.

Tek satır olarak kategorileri görüntülemek üzere `<table>` biçimlendirme aşağıdakine benzer yaymak için bir yineleyici ihtiyacımız her kategori için bir sütun ile:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample7.html)]

Bu yana `<td>Category X</td>` yinelenir, bu s ItemTemplate Yineleyicideki görünür bölümünün metindir. -Daha önce görünen işaretleme `<table><tr>` -yerleştirileceği `HeaderTemplate` - bitiş biçimlendirme sırasında `</tr></table>` -oluşturacak yerleştirilmiş `FooterTemplate`. Bu şablon ayarlarını girmek için sol alt köşesine kaynak düğmesini ve türü aşağıdaki söz tıklayarak ASP.NET sayfasını bildirim temelli bölümüne gidin:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample8.aspx)]

Kesin biçimlendirme, şablonlar, başka bir şey, hiçbir şey daha az tarafından belirtilen yineleyici yayar. Şekil 17 bir tarayıcıdan görüntülendiğinde Repeater s çıktı gösterir.


[![Tek satır HTML &lt;tablo&gt; her kategoriyi ayrı bir sütunda listeler](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image45.png)

**Şekil 17**: A tek satır HTML `<table>` listeler her kategoriyi ayrı bir sütunda ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>6. adım: görünüm yineleyicinin geliştirme

Tam olarak işaretleme, şablonları tarafından belirtilen yineleyici yayan olduğundan, yineleyici için stil özellikleri yoktur süpriz gelmelidir. Yineleyici tarafından oluşturulan içeriğin görünümünü değiştirmek için biz elle gereken HTML ya da CSS içeriği doğrudan Repeater s şablonların eklemeniz gerekir.

Bizim örneğimizde, değişen satırları DataList'te ile gibi arka plan renkleri, diğer kategori sütunlarına sahip s olanak tanır. Bunu gerçekleştirmek için atamak ihtiyacımız `RowStyle` her yineleyici öğesine bir CSS sınıfı ve `AlternatingRowStyle` değişen her yineleyici öğesine bir CSS sınıfı `ItemTemplate` ve `AlternatingItemTemplate` şablonları, şu şekilde:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample9.aspx)]

Ürün kategorileri metinle çıktı da üst bilgi satırı eklemek s olanak tanır. Biz ki bu yana t bilmeniz kaç sütun bizim kaynaklanan `<table>` oluşur, tüm sütunları span kesin bir üst bilgi satırı oluşturmak için en basit yolu kullanmaktır *iki* `<table>` s. İlk `<table>` iki satır üst bilgi satırı ve ikinci olarak, tek satır içeren bir satır içeren `<table>` sistemde her kategori için bir sütuna sahip. Diğer bir deyişle, aşağıdaki biçimlendirme yayma istiyoruz:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample10.html)]

Aşağıdaki `HeaderTemplate` ve `FooterTemplate` istenen işaretlemede neden:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample11.aspx)]

Bu değişiklikleri yaptıktan sonra Şekil 18 Repeater gösterir.


[![Kategori sütunları içinde arka plan rengini değiştirmek ve bir üst bilgi satırı içerir](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image48.png)

**Şekil 18**: Kategori sütunları alternatif arka plan rengi ve üst bilgi satırı içeren ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image50.png))


## <a name="summary"></a>Özet

GridView denetiminde görüntülenecek kolaylaştırır, ancak düzenleme, silme, sıralama ve verilerde, sayfa görünümü çok boxy ve benzer. DataList veya Repeater denetimleri etkinleştirmek için görünümü hakkında daha fazla denetime için oluşturmamız gerekir. Bu denetimlerin her ikisi de BoundFields, CheckBoxFields vb. yerine şablonları kullanarak kayıt kümesini görüntüler.

Bir HTML olarak DataList işler `<table>` , varsayılan olarak, her veri kaynağı kaydı GridView tek TemplateField ile olduğu gibi bir tek bir tablo satır görüntüler. Ancak, bir sonraki öğreticide göreceğiz gibi DataList tablo satırının görüntülenecek birden çok kayıt izin vermez. Yineleyici kesinlikle Öte yandan, kendi şablon içinde belirtilen biçimlendirme yayar; herhangi bir ek biçimlendirme eklemez ve bu nedenle yaygın olarak HTML öğeleri dışındaki verileri görüntülemek için kullanılan bir `<table>` (örn. bir madde işaretli liste).

DataList ve Repeater işlenmiş çıkışı daha fazla esneklik sunar, ancak bunlar GridView içinde bulunan yerleşik özelliklerin çoğunu yoksundur. Sonraki öğreticilerde inceleyeceğiz gibi çok fazla çaba, bu özelliklerin bazıları takılabilen geri ancak göz önünde DataList kullanmaya devam yapın veya Repeater GridView yerine bu özellikleri uygulamasına gerek kalmadan kullanabileceğiniz özellikleri sınırla kendiniz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Yaakov Ellis, Liz Shulok Randy Etikan ve Stacy Park yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](nested-data-web-controls-cs.md)
> [İleri](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
