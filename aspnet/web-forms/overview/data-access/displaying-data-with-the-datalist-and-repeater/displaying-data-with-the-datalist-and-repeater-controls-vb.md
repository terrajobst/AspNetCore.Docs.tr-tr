---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
title: DataList ve yineleyici denetimleri (VB) ile verileri görüntüleme | Microsoft Docs
author: rick-anderson
description: Önceki eğitimlerine GridView denetiminin verileri görüntülemek için kullandık. Bu öğretici ile başlayarak, biz ortak raporlama desenlerle derlemeye Ara...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 58618954-a9ed-4ca0-8c2d-95a5ffd9c03e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 6aa7cb76295d18711d88dd9855b43b259b558060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-vb"></a>DataList ve yineleyici denetimleri (VB) ile verileri görüntüleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_VB.exe) veya [PDF indirin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/datatutorial29vb1.pdf)

> Önceki eğitimlerine GridView denetiminin verileri görüntülemek için kullandık. Bu öğretici ile başlayarak, biz DataList ve yineleyici denetimleri ile ortak raporlama desenler derlemeye bu denetimleri ile veri görüntüleme temelleri başlayarak arayın.


## <a name="introduction"></a>Giriş

Tüm son boyunca örneklerin 28 öğreticileri, biz açık GridView denetimine veri kaynağındaki birden fazla kayıt görüntülemek gerekiyorsa. GridView, kayıt s veri alanları sütunlarda görüntüleme veri kaynağındaki her kayıt için bir satır işler. GridView, bir ek olmasını sağlarken görüntüleme, sıralama, düzenleme ve silme verilerini aracılığıyla sayfası, görünümünü biraz boxy. Ayrıca, GridView s yapısı sabit için bir HTML biçimlendirme sorumlu içerir `<table>` bir tablo satır (`<tr>`) her kayıt ve tablo hücresi (`<td>`) her bir alan için.

Birden çok kayıt görüntülerken, daha yüksek düzeyde özelleştirme Görünüm ve oluşturulan biçimlendirmenin sağlamak için ASP.NET 2.0 DataList ve yineleyici denetimleri sunar (her ikisi de aynı zamanda ASP.NET sürümde kullanılabilir 1.x). DataList ve yineleyici denetimleri içeriklerini BoundFields, CheckBoxFields, ButtonFields, yerine şablonları oluşturmak ve benzeri. GridView gibi bir HTML olarak DataList işler `<table>`, ancak tablo satırının görüntülenecek kaynak kayıtları için birden çok veri verir. Yineleyici diğer taraftan, neleri açıkça belirtmek ve denetleyebilmeniz yayılan biçimlendirme gerektiğinde ideal bir aday olduğunu daha hiçbir ek biçimlendirme oluşturur.

Sonraki düzine veya bunu öğreticileri biz DataList ve yineleyici denetimleri ile ortak raporlama desenler derlemeye bu denetimleri şablonları ile verileri görüntüleme temelleri başlayarak göreceğiz. Bu denetimler biçimlendirme göreceğiz DataList, ana/Ayrıntılar senaryoları, yolları düzenlemek ve verileri silmek için veri kaynağı kayıtlarını düzenini değiştirmek nasıl nasıl kayıtlarda sayfasında ve benzeri.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>1. adım: DataList ve yineleyici öğretici Web sayfaları ekleme

Biz Bu öğretici başlamadan önce öncelikle Bu öğretici ve DataList ve yineleyici kullanarak verileri görüntüleme ile ilgili sonraki birkaç öğreticileri için yapmamız gereken ASP.NET sayfaları eklemek için bir dakikanızı ayırın s olanak tanır. Başlangıç adlı projeye yeni bir klasör oluşturarak `DataListRepeaterBasics`. Ardından, aşağıdaki beş ASP.NET sayfaları tümünün ana sayfa kullanacak şekilde yapılandırılmış olan bu klasöre eklemek `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![DataListRepeaterBasics bir klasör oluşturun ve öğretici ASP.NET sayfaları ekleme](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image1.png)

**Şekil 1**: oluşturmak bir `DataListRepeaterBasics` klasörü ve öğretici ASP.NET sayfaları ekleme


Açık `Default.aspx` sayfasında ve sürükleyin `SectionLevelTutorialListing.ascx` kullanıcı denetiminden `UserControls` tasarım yüzeyine klasör. Bu kullanıcı, içinde oluşturduğumuz denetimi [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-vb.md) öğretici, site haritası numaralandırır ve madde işaretli listede geçerli bölümünden öğreticileri görüntüler.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image2.png)

**Şekil 2**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image4.png))


Madde işaretli liste görünümünü olması için biz oluşturma, DataList ve yineleyici öğreticileri biz bunları site haritası eklemeniz gerekir. Açık `Web.sitemap` dosya ve ekleme özel düğmeler site haritası düğümü biçimlendirme sonra aşağıdaki biçimlendirme ekleyin:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample1.xml)]


![Yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image5.png)

**Şekil 3**: yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir


## <a name="step-2-displaying-product-information-with-the-datalist"></a>2. adım: DataList ile ürün bilgilerini görüntüleme

FormView benzeyen, şablonları yerine BoundFields, CheckBoxFields ve benzeri DataList denetimi çıkış çizilir s bağlıdır. FormView DataList insanı yalnızlaştırıcı bir yerine kayıt kümesini görüntülemek için tasarlanmıştır. Bu öğretici bir DataList bağlama ürün bilgileri bir bakış ile başlayan s olanak tanır. Başlangıç açarak `Basics.aspx` sayfasındaki `DataListRepeaterBasics` klasör. Ardından, bir DataList tasarımcıya araç çubuğuna sürükleyin. DataList s şablonları belirtmeden önce Şekil 4'te gösterildiği gibi tasarımcı gri bir kutu olarak görüntüler.


[![DataList tasarımcıya araç çubuğuna sürükleyin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image6.png)

**Şekil 4**: DataList gelen araç kutusu üzerine Tasarımcı sürükleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image8.png))


DataList s akıllı etiketi, yeni ObjectDataSource ekleyin ve kullanacak şekilde yapılandırın `ProductsBLL` s sınıfı `GetProducts` yöntemi. Sihirbaz s Ekle (hiçbiri) açılan listesinden Bu öğreticide, bir salt okunur DataList oluşturma re ayarlarız olduğundan, güncelleştirme ve sekmeleri SİLİN.


[![Yeni bir ObjectDataSource oluşturmak için İptal Et](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image9.png)

**Şekil 5**: yeni ObjectDataSource Create Opt ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image11.png))


[![ObjectDataSource ProductsBLL sınıfını kullanmak için yapılandırma](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image12.png)

**Şekil 6**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image14.png))


[![GetProducts yöntemi kullanarak ürünlerin tamamı hakkında bilgi alın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image15.png)

**Şekil 7**: almak bilgi hakkında tüm ürünleri kullanımının `GetProducts` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image17.png))


ObjectDataSource yapılandırma ve akıllı etiket aracılığıyla DataList ile ilişkilendirme sonra Visual Studio otomatik olarak oluşturacak bir `ItemTemplate` adını ve veri kaynağı tarafından döndürülen her veri alanı değerini görüntüler DataList içinde (bkz. Biçimlendirme aşağıdaki). Bu varsayılan `ItemTemplate` s görünümü, bir veri kaynağı Tasarımcısı aracılığıyla FormView bağlama sırasında otomatik olarak oluşturulan şablonlar için aynıdır.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample2.aspx)]

> [!NOTE]
> FormView s akıllı etiket aracılığıyla FormView denetlemek için bir veri kaynağına bağlanırken, Visual Studio'nun oluşturduğu geri çağırma bir `ItemTemplate`, `InsertItemTemplate`, ve `EditItemTemplate`. DataList, ancak ile yalnızca bir `ItemTemplate` oluşturulur. DataList düzenleme ve FormView tarafından sunulan desteğin ekleme aynı yerleşik olmadığından budur. DataList düzenleme ve silme ilgili olayları içerir ve hiçbir basit Giden kutusu desteğiyle olarak FormView biraz kod ancak orada s ile düzenleme ve silme destek eklenebilir. Düzenleme ve Destek ile DataList gelecekteki bir öğreticide silme içerecek şekilde nasıl göreceğiz.


Bu şablon görünümünü iyileştirmek için bir dakikanızı ayırın s olanak tanır. Tüm veri alanlarını görüntülemek yerine, yalnızca görüntüle ürün adı, tedarikçi, kategori, birim ve birim fiyat başına miktar s olanak tanır. Ayrıca, let s görünen adı içinde bir `<h4>` başlık ve yerleşim kalan alanları kullanarak bir `<table>` başlığı altında.

Yapabilecekleriniz bu değişiklikleri yapmak için ya da s etiketi Düzenle şablonları bağlantıyı tıklatın veya şablonu sayfa s bildirim temelli söz dizimi aracılığıyla el ile değiştirebilirsiniz akıllı DataList Tasarımcısından özelliklerinde düzenleme şablonunu kullanın. Tasarımcıda Şablonları Düzenle seçeneğini kullanırsanız, sonuç biçimlendirme aşağıdaki biçimlendirme tam olarak eşleşmiyor olabilir, ancak aracılığıyla görüntülendiğinde bir tarayıcı Şekil 8'de gösterilen ekran çok benzer görünmelidir.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample3.aspx)]

> [!NOTE]
> Etiket Web özelliği kullanan Yukarıdaki örnek denetimleri `Text` özelliği, veri bağlamasını sözdizimi değeri atanır. Alternatif olarak, biz etiketleri tamamen yalnızca databinding sözdiziminde yazarak atlamış. Diğer bir deyişle, yerine `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` biz yerine Tanımlayıcı Sözdizimi kullanabilirdik `<%# Eval("CategoryName") %>`.


Etiket Web denetimlerinde bırakır, ancak iki avantajları sunar. İlk olarak, sonraki öğreticide anlatıldığı gibi verileri temel alan veri biçimlendirme için daha kolay bir yol sağlar. İkinci olarak, Şablonları Düzenle seçeneğini Tasarımcısı içermiyor t görüntü bildirimsel veri bağlamayı sözdiziminde bazı Web denetimi dışında görüntülenir. Bunun yerine, Şablonları Düzenle arabirimi static işaretleme çalışmak kolaylaştırmak için tasarlanmıştır ve Web denetler ve tüm veri bağlama Web denetimleri akıllı etiketleri erişilebilen veri bağlamaları Düzenle iletişim kutusu üzerinden yapılır varsayar.

Bu nedenle, Tasarımcı şablonlarını düzenleme seçeneği sağlayan DataList ile çalışırken, İçerik Şablonları Düzenle arabirimi üzerinden erişilebilir olmasını sağlamak etiket Web denetimleri kullanmak istemiyorum. Kısa süre içinde anlatıldığı gibi yineleyici şablonu s içeriği kaynağı görünümünden düzenlenmesi gerekir. Sonuç olarak, etiket Web atlayın genellikle yineleyici s şablonları hazırlayın biçimlendirmeniz gerekir bilmiyorsanız denetimleri, verilerin görünümünü programlı mantığına göre metin bağlı.


[![Her ürünün s çıkış çizilir s ItemTemplate DataList kullanmaktır](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image18.png)

**Şekil 8**: her ürün çıktı s'dir çizilir kullanarak s DataList `ItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>3. adım: DataList görünümünü iyileştirme

GridView gibi DataList stili ilgili özellikler gibi sunar `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`ve benzeri. GridView ve DetailsView denetimleri ile çalışırken, dış görünüm dosyalarında oluşturduğumuz `DataWebControls` önceden tanımlanmış tema `CssClass` bu iki denetimlerin özelliklerini ve `CssClass` birkaç kendi alt özelliği (`RowStyle` `HeaderStyle`, vb.). Aynı işlemi gerçekleştirmek için DataList s olanak tanır.

' Da anlatıldığı gibi [veri ObjectDataSource ile görüntüleme](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) öğretici, bir Web denetimi için varsayılan görünüm ilgili özelliklerini bir dış görünüm dosyası belirtir; bir tema tanımlamak kaplama, CSS, görüntü ve JavaScript dosyaları koleksiyonudur bir Web sitesi için belirli bir görünüm ve kullanımında. İçinde *veri ObjectDataSource ile görüntüleme* oluşturduğumuz öğretici, bir `DataWebControls` tema (bir klasör içinde olarak uygulanan `App_Themes` klasör), şu anda iki kaplama dosya - olan `GridView.skin` ve `DetailsView.skin`. Üçüncü bir DataList önceden tanımlanmış stil ayarlarını belirtmek için görünüm dosyası ekleme s olanak tanır.

Bir görünüm dosyası eklemek için sağ `App_Themes/DataWebControls` klasörü, yeni öğe Ekle'yi seçin ve listeden görünüm dosyası seçeneğini seçin. Dosya adı `DataList.skin`.


[![DataList.skin adlı yeni bir görünüm dosyası oluşturma](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image21.png)

**Şekil 9**: yeni bir dış dosya adlı oluşturma `DataList.skin` ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image23.png))


Kullanmak için aşağıdaki biçimlendirme `DataList.skin` dosyası:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample4.aspx)]

Bu ayarları aynı CSS sınıfları GridView ve DetailsView denetimleriyle kullanılmış gibi uygun DataList özelliklerini atayın. Burada kullanılan CSS sınıfları `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`ve benzeri tanımlanan `Styles.css` dosya ve önceki eğitimlerine eklenmiştir.

Bu görünüm dosyası eklenmesiyle DataList s görünüm (yeni görünüm dosyası; görüntüleme menüsünde etkilerini görmek için Yenile'yi seçin Tasarımcısı görünümü yenilemeniz gerekebilir) Tasarımcısı'nda güncelleştirilir. Şekil 10 gösterildiği gibi değişen her ürünün açık pembe arka plan rengi vardır.


[![DataList.skin adlı yeni bir görünüm dosyası oluşturma](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image24.png)

**Şekil 10**: yeni bir dış dosya adlı oluşturma `DataList.skin` ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>4. adım: DataList s diğer şablonlar keşfetme

Ek olarak `ItemTemplate`, DataList altı isteğe bağlı şablonları destekler:

- `HeaderTemplate` sağlanırsa, çıkış için bir başlık satırı ekler ve bu satır işlemek için kullanılır
- `AlternatingItemTemplate` değişen öğeleri işlemek için kullanılan
- `SelectedItemTemplate` Seçili öğeyi işlemek için kullanılan; Seçili öğenin dizinini karşılık gelen s DataList öğesi olan [ `SelectedIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` düzenlenen öğesi oluşturmak için kullanılan
- `SeparatorTemplate` sağlanırsa, her bir öğe arasındaki ayırıcı ekler ve bu ayırıcı işlemek için kullanılır
- `FooterTemplate` -verdiyse, çıkışı bir altbilgi satır ekler ve bu satırı işlemek için kullanılır

Belirtirken `HeaderTemplate` veya `FooterTemplate`, DataList işlenmiş çıkış ek bir üstbilgisi veya altbilgisi satır ekler. Gibi GridView s üstbilgi ve altbilgi satırları, üstbilgi ve altbilgi DataList içinde veriye bağlı değil. Bu nedenle, hiçbir veri bağlamasını sözdiziminde `HeaderTemplate` veya `FooterTemplate` bağlı veri erişmeye çalışır boş bir dize döndürür.

> [!NOTE]
> İçinde gördüğümüz gibi [özet bilgilerinde görüntüleme GridView s altbilgi](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) t destek databinding sözdizimi, veri özgü bilgileri üstbilgi ve altbilgi satırları güncelleştireceğinizi sırada öğretici eklenen bu satırları doğrudan GridView s `RowDataBound` olay işleyicisi. Bu teknik için kullanılabilir hem de çalışan toplamları hesaplamak veya diğer bilgileri verilerden denetime bağlı yanı sıra bu bilgileri altbilgi atayın. Aynı kavram DataList ve yineleyici denetimleri uygulanabilir; Yineleyici ve DataList için bir olay işleyicisi oluşturmak için olan tek fark `ItemDataBound` olayı (yerine için `RowDataBound` olay).


Bizim örneğimizde, let s ürün DataList s sonuçlarında üstündeki görüntülenen bilgiler başlığı bulunan bir `<h3>` başlığı. Bunu başarmak için add bir `HeaderTemplate` uygun biçimlendirme ile. Tasarımcısı'ndan bu DataList s akıllı etiket Şablonları Düzenle bağlantıya tıklayarak, aşağı açılan listeden üstbilgi şablonu seçin ve aşağı açılan stil Başlık 3 seçeneğinden çekme sonra metin yazarak gerçekleştirilebilir (bkz. Şekil 11) listesi.


[![Metin ürün bilgilerle HeaderTemplate ekleme](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image27.png)

**Şekil 11**: ekleme bir `HeaderTemplate` metin ürün bilgilerle ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image29.png))


Alternatif olarak, bu bildirimli olarak aşağıdaki biçimlendirmesi içinde girerek eklenebilir `<asp:DataList>` etiketler:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample5.html)]

Biraz her ürün listesini arasında boşluk eklemek için Ekle s sağlayan bir `SeparatorTemplate` her bölüm arasında bir satır içerir. Yatay çizgi etiketi (`<hr>`), bu tür bir ayırıcı ekler. Oluşturma `SeparatorTemplate` aşağıdaki biçimlendirme sahip olması:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample6.html)]

> [!NOTE]
> Gibi `HeaderTemplate` ve `FooterTemplates`, `SeparatorTemplate` kayıtlara veri kaynağından bağlı değil ve bu nedenle veri kaynağı için DataList bağlı kayıtları erişim doğrudan yükleyemezsiniz.


Bu ayrıca, yaptıktan sonra sayfası bir tarayıcı aracılığıyla görüntülerken Şekil 12'ye benzer görünmelidir. Üstbilgi satırındaki ve her ürün listesini arasındaki hat unutmayın.


[![Sütun başlığı ve her ürün listesini arasında yatay bir kural DataList içerir](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image30.png)

**Şekil 12**: sütun başlığı ve bir yatay kural arasında her ürün listesi DataList içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>5. adım: Belirli biçimlendirme yineleyici denetimi ile işleme

Görünüm/kaynak tarayıcınızdan Şekil 12 DataList örnekten ziyaret eden bunu yaparsanız, DataList HTML yayar görürsünüz `<table>` içeren bir tablo satırının (`<tr>`) sahip tek tablo hücresi (`<td>`) bağlı her öğe için DataList. Bu çıktı, aslında, ne GridView tek TemplateField ile gelen yayınlaması için aynıdır. Sonraki öğreticide anlatıldığı gibi DataList bize tablo satırının başına birden çok veri kaynağı kayıt görüntülemek etkinleştirme çıktının daha fazla özelleştirme izin vermiyor.

Bir HTML yaymak üzere t istediğiniz güncelleştireceğinizi ne `<table>`ancak? Bir veri Web denetimi tarafından oluşturulan biçimlendirme üzerinde toplam ve tam denetim için biz yineleyici denetim kullanmanız gerekir. DataList gibi yineleyici şablonları alarak oluşturulur. Yineleyici ancak, yalnızca aşağıdaki beş şablonları sunar:

- `HeaderTemplate` sağlanan, öğelerden önce belirtilen biçimlendirme ekler.
- `ItemTemplate` öğeleri işlemek için kullanılan
- `AlternatingItemTemplate` sağlanırsa, değişen öğeleri işlemek için kullanılan
- `SeparatorTemplate` sağlanan, her bir öğe arasındaki belirtilen biçimlendirme ekler.
- `FooterTemplate` -verdiyse, belirtilen biçimlendirme öğelerinden sonra ekler.

ASP.NET 1.x, denetim verisini bazı veri kaynağından gelen madde işaretli bir listesini görüntülemek için kullanılan yaygın olarak yineleyici. Böyle bir durumda `HeaderTemplate` ve `FooterTemplates` açma ve kapatma içerecektir `<ul>` etiketleri, sırasıyla while `ItemTemplate` içerecektir `<li>` öğeleri databinding sözdizimine sahip. İki örnekte de gördüğümüz bu yaklaşım hala ASP.NET 2.0 ile kullanılabilir [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-vb.md) Öğreticisi:

- İçinde `Site.master` ana sayfa madde işaretli en üst düzey site eşlemesi içeriği (temel raporlama, raporları filtreleme, özelleştirilmiş biçimlendirme ve benzeri) listesini görüntülemek için kullanılan bir yineleyici; başka bir, iç içe geçmiş yineleyici alt bölümlerini görüntülemek için kullanıldı üst düzey bölümleri
- İçinde `SectionLevelTutorialListing.ascx`, geçerli site eşlemesi bölümü alt bölümlerini madde işaretli bir listesini görüntülemek için kullanılan bir yineleyici

> [!NOTE]
> ASP.NET 2.0 tanıtır yeni [Bulletedlıst denetim](https://msdn.microsoft.com/library/ms228101.aspx), hangi bağlanabilir bir veri kaynağı denetimi basit madde işaretli listesini görüntülemek için. Bulletedlıst denetimiyle biz herhangi bir listeyle ilgili HTML belirtmek gerekmez; Bunun yerine, biz yalnızca her liste öğesi için metin olarak görüntülemek için veri alanı belirtin.


Yineleyici tüm verileri Web denetimi catch görev yapar. Yineleyici denetimi gerekli biçimlendirmeleri oluşturur, varolan bir denetim değilse kullanılabilir. Yineleyici kullanarak göstermek için ürün bilgi 2. adımda oluşturulan DataList yukarıda görüntülenen kategoriler listesine sahip s olanak tanır. Özellikle, let s sahip tek satır HTML biçiminde görüntülenen kategorileri `<table>` her kategoride tablodaki bir sütun olarak görüntülenir.

Bunu gerçekleştirmek için ürün bilgi DataList yukarıda tasarımcıya araç yineleyici denetim sürükleyerek başlatın. Kendi şablonlarını tanımlanan kadar bir DataList olduğu gibi yineleyici başlangıçta gri bir kutu olarak görüntüler.


[![Bir yineleyici Designer'a ekleyin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image33.png)

**Şekil 13**: bir yineleyici Designer'a ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image35.png))


Burada s yalnızca bir yineleyici s akıllı etiket seçeneğinde: veri kaynağı seçin. Yeni bir ObjectDataSource oluşturmak ve kullanmak üzere yapılandırmak için kabul `CategoriesBLL` s sınıfı `GetCategories` yöntemi.


[![Yeni bir ObjectDataSource oluşturma](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image36.png)

**Şekil 14**: yeni ObjectDataSource oluşturun ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image38.png))


[![ObjectDataSource CategoriesBLL sınıfını kullanmak için yapılandırma](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image39.png)

**Şekil 15**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image41.png))


[![Tüm GetCategories yöntemini kullanarak kategorilerini hakkında bilgi alın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image42.png)

**Şekil 16**: almak bilgi hakkında tüm kategorileri kullanma `GetCategories` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image44.png))


DataList, Visual Studio otomatik olarak bir ItemTemplate yineleyici için bir veri kaynağına bağlama sonra oluşturmaz. Ayrıca, yineleyici s şablonları Tasarımcısı yapılandırılamaz ve bildirimli olarak belirtilmelidir.

Tek satır olarak kategorileri görüntülemek üzere `<table>` her kategori için bir sütun ile işaretleme aşağıdakine benzer yaymak üzere yineleyici ihtiyacımız var:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample7.html)]

Bu yana `<td>Category X</td>` metindir yineler, bu yineleyici s ItemTemplate görünür bölümü. Önce - göründüğü biçimlendirme `<table><tr>` -yerleştirilecek `HeaderTemplate` bitiş biçimlendirme sırasında- `</tr></table>` -yerleştirilen `FooterTemplate`. Bu şablon ayarları girmek için sol alt köşesinde kaynak düğmesini ve aşağıdaki sözdiziminde türü tıklayarak ASP.NET sayfası bildirim temelli bölümüne bakın:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample8.aspx)]

Yineleyici belirtildiği gibi şablonları, başka bir şey, hiçbir şey daha az kesin biçimlendirme yayar. Şekil 17 bir tarayıcıdan görüntülendiğinde yineleyici s çıktısını gösterir.


[![Bir tek satır HTML &lt;tablo&gt; ayrı bir sütunun her kategoride listeler](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image45.png)

**Şekil 17**: A tek satır HTML `<table>` ayrı bir sütunun her kategoride listeler ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>6. adım: yineleyici görünümünü iyileştirme

Yineleyici tam olarak kendi şablonları tarafından belirtilen biçimlendirme yayar bu yana hiç şaşırtıcı yineleyici için stil ile ilgili özellikler yok gelmelidir. Yineleyici tarafından oluşturulan içeriğin görünümünü değiştirmek için biz elle gereken HTML veya CSS içeriği doğrudan yineleyici s şablonları eklemeniz gerekir.

Bizim örneğimizde, DataList değişen satırları ile gibi arka plan renklerini, alternatif kategori sütuna sahip s olanak tanır. Bunu gerçekleştirmek için atamak ihtiyacımız `RowStyle` her yineleyici öğesine CSS sınıfı ve `AlternatingRowStyle` CSS sınıfı değişen her yineleyici öğesine `ItemTemplate` ve `AlternatingItemTemplate` şablonları, şu şekilde:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample9.aspx)]

Ürün kategorileri metinle çıkış ayrıca başlık satırı eklemek s olanak tanır. Biz güncelleştireceğinizi beri t bilmeniz kaç sütun bizim kaynaklanan `<table>` oluşan, tüm sütunlar span garanti bir başlık satırı oluşturmak için en basit yolu kullanmaktır *iki* `<table>` s. İlk `<table>` iki satır üstbilgisi satır ve ikinci olarak, tek satır içeren bir satır içerecek `<table>` sistemdeki her kategori için bir sütun sahip. Diğer bir deyişle, aşağıdaki biçimlendirme yayma istiyoruz:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample10.html)]

Aşağıdaki `HeaderTemplate` ve `FooterTemplate` istenen biçimlendirmede neden:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample11.aspx)]

Bu değişiklikler yapıldıktan sonra Şekil 18 yineleyici gösterir.


[![Kategori sütunları arka plan rengini de alternatif ve bir başlık satırı içerir](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image48.png)

**Şekil 18**: arka plan rengi ve içeren sütun başlığı Kategori sütunları alternatif ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image50.png))


## <a name="summary"></a>Özet

GridView denetimini görüntülemek kolaylaştırır, ancak düzenleme, silme, sıralama ve verilerde, sayfa görünümü çok boxy ve kılavuz benzeri. Görünüm üzerinde daha fazla denetim için biz DataList veya yineleyici denetimlerine etkinleştirmeniz gerekir. Bu denetimlerin her ikisi BoundFields, CheckBoxFields vb. yerine şablonları kullanarak kayıt kümesini görüntüler.

Bir HTML olarak DataList işler `<table>` , varsayılan olarak, her veri kaynağı kaydı tek bir tablo satırı, tek bir TemplateField ile GridView gibi görüntüler. Ancak, bir sonraki öğreticide göreceğiz gibi DataList tablo satırının görüntülenecek birden çok kayıt izin vermez. Yineleyici kesinlikle Öte yandan, şablon içinde belirtilen biçimlendirme yayar; tüm ek biçimlendirme eklemez ve bu nedenle genellikle veri HTML öğeleri dışındaki görüntülemek için kullanılan bir `<table>` (örn. bir listeyi).

DataList ve yineleyici işlenmiş çıktılarını daha fazla esneklik sunar, ancak bunlar GridView bulunan yerleşik özelliklerinin çoğunu yoksundur. Biz yaklaşan eğitimlerine inceleyeceğiz gibi çok fazla çaba bu özelliklerden bazıları takılabilen geri ancak göz önünde, DataList kullanmaya devam yapın veya yineleyici GridView yerine bu özellikleri uygulamak zorunda kalmadan kullanabilirsiniz özelliklerini sınırlayın kendiniz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Yaakov Ellis, Liz Shulok, Randy Etikan ve Stacy Park yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](nested-data-web-controls-cs.md)
> [sonraki](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
