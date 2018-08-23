---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: (VB) radyo düğmelerinden oluşan GridView sütunu ekleme | Microsoft Docs
author: rick-anderson
description: Bu öğretici, kullanıcı, tek bir satır seçilmesi, daha sezgisel bir yol sağlamak üzere bir GridView denetimi radyo düğmelerinden oluşan bir sütun eklemek nasıl bakan...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: 76b3dbd502eff7c97f57fdacd120ac2312aaceae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754755"
---
<a name="adding-a-gridview-column-of-radio-buttons-vb"></a>(VB) radyo düğmelerinden oluşan GridView sütunu ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe) veya [PDF olarak indirin](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> Bu öğretici, kullanıcı GridView'ın tek bir satır seçilmesi, daha sezgisel bir yol sağlamak üzere bir GridView denetimi radyo düğmelerinden oluşan bir sütun eklemek nasıl bakar.


## <a name="introduction"></a>Giriş

GridView denetiminde yerleşik işlevsellik harika bir fırsat sunar. Bu metin, resimler, köprüler ve düğmeleri görüntülemek için farklı alanların sayısını içerir. Bu, daha fazla özelleştirme için şablonları destekler. Fare, birkaç tıklamayla, burada her bir satır seçilebilir bir düğmeyle GridView yapmak veya düzenleme veya silme özelliklerini etkinleştirmek için olası s. Sağlanan özellikler deseninizi oluşturmayı rağmen sıklıkla olacaktır durumlarda, ek, desteklenmeyen özellikler eklenmesi gerekir. Bu öğretici ve sonraki iki ek özellikleri içerecek şekilde GridView s işlevselliğini geliştirmek nasıl inceleyeceğiz.

Bu öğretici ve bir sonraki satır seçimi işlem geliştirme üzerinde odaklanın. Denetlenen gibi [ana/Ayrıntılar Detailview'u ile seçilebilir bir ana GridView kullanan Detail](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md), bir CommandField seçme düğmesi içeren GridView ekleyebilirsiniz. Tıklandığında, bir geri gönderme ensues ve GridView s `SelectedIndex` özelliği seçin, düğmeye tıkladı satırın dizini güncelleştirildi. İçinde [ana/Ayrıntılar Detailview'u ile seçilebilir bir ana GridView kullanan Detail](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) Öğreticisi, seçili GridView satır ayrıntılarını görüntülemek için bu özelliği kullanmak nasıl gördük.

Çoğu durumda Seç düğmesini yapmaya çalışırken, de başkaları için çalışmayabilir. Bir düğme kullanmak yerine, diğer iki kullanıcı arabirimi öğeleri genellikle seçimi için kullanılır: radyo düğmesi ve onay kutusu. Radyo düğmesinin veya onay kutusu seçme düğmesi yerine, her satır içerir, böylece biz GridView genişletebilirsiniz. Burada kullanıcının yalnızca birini GridView kayıtları seçebilirsiniz senaryolarda, radyo düğmesini seçin düğmenin üzerine tercih edilen olabilir. Kullanıcı büyük olasılıkla seçebildiğiniz durumlarda burada kullanıcı onay silmek için birden çok ileti seçmek isteyebilirsiniz, bir web tabanlı e-posta uygulaması gibi birden fazla kayda Seç düğmesini veya radyo düğmesi kullanılamıyor işlevselliği sunar kullanıcı arabirimleri.

Bu öğretici için GridView radyo düğmelerinden oluşan bir sütun ekleme bakar. Devam etmeden öğretici, onay kutularını kullanarak açıklar.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>1. adım: geliştirme GridView Web sayfaları oluşturma

Radyo düğmelerinden oluşan bir sütun içerecek şekilde Gridview'u geliştirme başlamadan önce ilk yapmamız gereken Bu öğretici ve sonraki iki Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırarak s olanak tanır. Başlangıç adlı yeni bir klasör ekleyerek `EnhancedGridView`. Ardından, o klasördeki her bir sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleyin `Site.master` ana sayfa:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![SqlDataSource ile ilgili öğreticiler için ASP.NET sayfaları ekleme](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**Şekil 1**: SqlDataSource ile ilgili öğreticiler için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `EnhancedGridView` klasörü kendi bölümünde öğreticileri listeler. Bu geri çağırma `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimine ekleme `Default.aspx` sayfaya s Tasarım görünümü Çözüm Gezgini'nde sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**Şekil 2**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))


Son olarak, girişleri olarak bu dört sayfalar ekleme `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme kullanma sonra eklemeniz SqlDataSource denetimi `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticiler Web sitesini görüntülemek için bir dakikanızı ayırın. Sol taraftaki menüden, artık düzenleme, ekleme ve silme öğreticiler için öğeleri içerir.


![Site Haritası artık iyileştirmeyi GridView öğreticiler içerir](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**Şekil 3**: artık Site Haritası GridView öğreticiler iyileştirmeyi girişleri içerir


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>2. adım: tedarikçileri GridView içinde görüntüleme

Bu öğreticide, GridView yapı s denetlemesine izin vermek için ABD sağlayıcıdan bir radyo düğmesi sağlayarak her GridView satır ile listeler. Radyo düğmesi aracılığıyla bir tedarikçi seçtikten sonra kullanıcı bir düğmeye tıklayarak tedarikçi s ürünleri görüntüleyebilirsiniz. Bu görev Önemsiz Kulağa özellikle zor hale ıot'nin sayısı vardır. Biz bu ıot'nin araştırmadan önce ilk tedarikçileri listeleme GridView alın s olanak tanır.

Başlangıç açarak `RadioButtonField.aspx` sayfasını `EnhancedGridView` GridView tasarımcıya Toolbox'tan sürükleyerek klasörü. GridView s ayarlamak `ID` için `Suppliers` ve akıllı etiketinde yeni bir veri kaynağı oluşturmayı seçin. Özellikle, adlandırılmış bir ObjectDataSource oluşturma `SuppliersDataSource` kendi verileri çeker `SuppliersBLL` nesne.


[![SuppliersDataSource adlı yeni bir ObjectDataSource oluşturma](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**Şekil 4**: adlı yeni bir ObjectDataSource oluşturma `SuppliersDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))


[![ObjectDataSource SuppliersBLL sınıfını kullanmak için yapılandırma](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**Şekil 5**: ObjectDataSource kullanılacak yapılandırma `SuppliersBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))


Biz yalnızca bu tedarikçileri ABD listelemek istediğiniz beri seçin `GetSuppliersByCountry(country)` seçme sekmesinde açılır listeden yöntemi.


[![ObjectDataSource SuppliersBLL sınıfını kullanmak için yapılandırma](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**Şekil 6**: ObjectDataSource kullanılacak yapılandırma `SuppliersBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))


(Hiçbiri) seçeneği ve İleri'yi güncelleştirme sekmesinden seçin.


[![ObjectDataSource SuppliersBLL sınıfını kullanmak için yapılandırma](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**Şekil 7**: ObjectDataSource kullanılacak yapılandırma `SuppliersBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))


Bu yana `GetSuppliersByCountry(country)` yöntemi, bir parametre kabul eder, bize bu parametrenin kaynağı için veri kaynağı Yapılandırma Sihirbazı'nı ister. Sabit kodlanmış bir değer (Bu örnekte, ABD), belirtmek için açılır listede kaynak hiçbiri olarak ayarlayın ve varsayılan değeri metin kutusuna girin parametrenin bırakın. Sihirbazı tamamlamak için Son'u tıklatın.


[![ABD ülke parametresi için varsayılan değer kullanın.](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**Şekil 8**: kullanım ABD için varsayılan değer olarak `country` parametre ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))


Sihirbazı tamamladıktan sonra GridView bir BoundField tedarikçi veri alanların her biri için dahil edilir. Kaldırma dışındaki tüm `CompanyName`, `City`, ve `Country` BoundFields ve yeniden adlandırma `CompanyName` BoundFields `HeaderText` tedarikçiye özelliği. Bunu yaptıktan sonra GridView ve ObjectDataSource bildirim temelli söz dizimi aşağıdaki gibi görünmelidir.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

Bu öğretici için seçilen tedarikçi görüntülemesini s ürünleri sağlayıcı listesi olarak aynı sayfadaki veya başka bir sayfaya izin s olanak tanır. Bunu yapabilmek için iki düğme Web sayfasına denetim ekleme. Miyim ve kümesi `ID` s için bu iki düğme `ListProducts` ve `SendToProducts`, fikir ile kullanırken `ListProducts` tıklandığında bir geri gönderme ortaya çıkar ve seçili sağlayıcı s ürünleri ne zaman ancak aynı sayfada listelenen `SendToProducts` tıklandığında Kullanıcı olduğu ürünleri listeler bir başka bir sayfaya whisked.

Şekil 9 gösterir `Suppliers` GridView ve iki düğme Web tarayıcısı üzerinden görüntülendiğinde denetler.


[![Bu sağlayıcılardan ABD kendi adı, şehir ve ülke bilgileri listelenen sahip](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**Şekil 9**: Bu sağlayıcılardan ABD sahip Their adını, şehir ve ülke listelenen bilgileri ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>3. adım: radyo düğmelerini içeren bir sütun ekleme

Bu noktada `Suppliers` GridView sahip üç BoundFields şirket adı, şehir ve ülke her tedarikçi ABD'de görüntüleme. Radyo düğmelerinden oluşan bir sütun ancak yine de bulunmaması. Ne yazık ki GridView eklenmemişse t, bu kılavuza ekleyebilirsiniz ve yapılması bir yerleşik RadioButtonField, aksi takdirde içerir. Bunun yerine, bir TemplateField ekleyebilir ve yapılandırma, `ItemTemplate` her GridView satır için bir radyo düğmesi sonuçta bir radyo düğmesi işlemek için.

Başlangıçta istenen kullanıcı arabirimi için bir RadioButton Web denetimi ekleyerek uygulanabilir varsayıyoruz `ItemTemplate` bir TemplateField biri. Bu gerçekten de her GridView satır için tek bir radyo düğmesi ekleyeceksiniz ancak radyo düğmeleri birleştirilemez ve birbirini dışlamaz. Diğer bir deyişle, bir son kullanıcının birden fazla radyo düğmeleri aynı anda GridView ' seçebilirsiniz.

Web RadioButton denetimlerinin bir TemplateField kullanma ihtiyacımız işlevselliği sunmaz olsa da, let s uygulamak, bu yaklaşım, sonuçta elde edilen radyo düğmeleri gruplandırılmadığını neden incelemek için faydalı s. Bir TemplateField tedarikçilerin en soldaki alan yapmadan GridView'a ekleyerek başlayın. Ardından, GridView s akıllı etiket Şablonları Düzenle bağlantısına tıklayın ve bir RadioButton Web denetimi TemplateField s ile araç kutusundan sürükleyin `ItemTemplate` (bkz. Şekil 10). RadioButton s ayarlamak `ID` özelliğini `RowSelector` ve `GroupName` özelliğini `SuppliersGroup`.


[![ItemTemplate için RadioButton Web denetim ekleme](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**Şekil 10**: bir RadioButton Web denetimine ekleme `ItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))


Bu eklemeler Tasarımcısı aracılığıyla yaptıktan sonra GridView s biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

RadioButton s [ `GroupName` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) ne radyo düğmelerinden oluşan bir serinin gruplandırmak için kullanılır. Aynı tüm RadioButton denetimlerinin `GroupName` değer olarak kabul edilir gruplandırılmış; yalnızca bir radyo düğmesini seçilebilir bir gruptan aynı anda. `GroupName` s işlenmiş radyo düğmesinin değeri alan özelliği belirtir `name` özniteliği. Radyo düğmeleri tarayıcı inceler `name` radyo belirlemek için öznitelikleri gruplandırmaları düğmesi.

Eklenen RadioButton Web denetimi ile `ItemTemplate`, bir tarayıcı aracılığıyla bu sayfasını ziyaret edin ve kılavuz s satırları radyo düğmeleri tıklayın. Tüm satırları Şekil 11 seçilecek çözmelerine nasıl radyo düğmeleri gruplandırılmadığını olduğuna dikkat edin gösterir.


[![GridView s radyo düğmeleri değil gruplandırılmış olan](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**Şekil 11**: GridView s radyo düğmeleri gruplanır değil ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))


Radyo düğmeleri değil gruplandırılmış neden olduğundan, işlenmiş `name` öznitelikleri aynı sahip olmasına rağmen farklı `GroupName` özellik ayarı. Bu farklılıkları görmek için bir görünüm/kaynak tarayıcıdan yapın ve radyo düğmesi biçimlendirme inceleyin:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

Bildirim nasıl hem `name` ve `id` öznitelikleri, Özellikler penceresinde belirtilen tam değerlerini değildir, ancak diğer sayıyla tanımlandıkları `ID` değerleri. Ek `ID` işlenen önüne eklenen değerleri `id` ve `name` öznitelikler `ID` s radyo düğmeleri üst denetimlerin `GridViewRow` s `ID` s, GridView s `ID`, İçerik denetimi s `ID`ve Web formu s `ID`. Bunlar `ID` s eklenir, böylece her işlenen Web GridView denetiminde sahip benzersiz bir `id` ve `name` değerleri.

Her denetim gereksinimlerini farklı bir işlenen `name` ve `id` bu nasıl tarayıcının istemci tarafı ve web sunucusuna hangi eylemin nasıl tanımladığı her denetimin benzersiz olarak tanımlayan veya değişikliğinin geri göndermede olduğundan. Örneğin, bir RadioButton s durumu değiştirildi işaretli olduğunda bazı sunucu tarafı kodu çalıştırmak istedik düşünün. Biz bunu RadioButton s ayarlayarak gerçekleştirmek `AutoPostBack` özelliğini `True` ve oluşturmak için bir olay işleyicisi `CheckChanged` olay. Ancak, işlenen `name` ve `id` tüm radyo düğmeleri aynı olan üzerinde hangi özel belirleyemedik geri gönderme değerleri RadioButton tıkladı.

Bunu kısa, biz bir sütunu radyo düğmelerinden oluşan GridView RadioButton Web denetimi kullanarak oluşturulamıyor ' dir. Bunun yerine, uygun biçimlendirme her GridView satır eklenmiş emin olmak için biz yerine tasarlanmayan eski teknikleri kullanmalısınız.

> [!NOTE]
> RadioButton Web denetimi, HTML denetimi, bir şablon eklediğinizde radyo düğmesini benzersiz içerecektir gibi `name` özniteliği, radyo düğmeleri Gruplandırılmamış kılavuz yapma. HTML denetimleri ile ilgili bilgi sahibi değilseniz, HTML denetimlerini nadiren, özellikle ASP.NET 2.0 olarak kullanıldığından bu not dikkate çekinmeyin. Ancak, daha fazla bilgi edinmek istiyorsanız bkz [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s blog girişine [Web denetimleri ve HTML denetimleri](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Radyo düğmesi biçimlendirmesi eklemesine bir değişmez değer denetimi kullanma

Doğru radyo düğmeleri GridView içindeki tüm grup için el ile radyo düğmeleri işaretleme içine eklenmek üzere gerekiyor `ItemTemplate`. Her radyo düğmesini aynı gereken `name` özniteliği, ancak bir benzersiz olmalıdır `id` (biz bir radyo düğmesi istemci tarafı komut dosyası aracılığıyla erişmesini isteyebileceğiniz) özniteliği. Bir kullanıcı bir radyo düğmesini seçer ve gönderileri sayfanın geri sonra tarayıcıyı yeniden s seçili radyo düğmesinin değeri göndermek `value` özniteliği. Bu nedenle, her bir radyo düğmesi benzersiz bir gerekir `value` özniteliği. Son geri göndermede eklediğinizden emin olmak ihtiyacımız `checked` özniteliği kullanıcı seçimi ve gönderileri geri yaptıktan sonra Aksi takdirde işaretli bir radyo düğmesi için radyo düğmeleri döndürecektir varsayılan durumlarına (tüm seçili).

Alt düzey biçimlendirme bir şablona ekleme için gerçekleştirilen iki yaklaşım vardır. Biçimlendirme ve biçimlendirme yöntemleri arka plan kod sınıfı içinde tanımlanan çağrıları yapmak için biridir. Bu teknik ilk de bahsedilen [GridView denetiminde TemplateField kullanma](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) öğretici. Örneğimizde, şöyle görünebilir:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

Burada, `GetUniqueRadioButton` ve `GetRadioButtonValue` uygun döndürülen arka plan kod sınıfı içinde tanımlanan yöntemleri olacak `id` ve `value` öznitelik değerlerini her radyo düğmesi için. Bu yaklaşım da atamak için çalışır `id` ve `value` öznitelikleri, ancak belirtmek ihtiyaç duyulduğunda kısa denk `checked` veri GridView'a ilk kez bağlandığında veri bağlama söz dizimi yalnızca çalıştırıldığı için öznitelik değeri. GridView Görünüm durumunun etkin varsa, bu nedenle, biçimlendirme yöntemlerini yalnızca zaman ateşlenir sayfa ilk yüklenen (veya zaman GridView açıkça DataSet'e veri kaynağına) ve bu nedenle ayarlar işlevi `checked` t kazanılan özniteliği çağrılabilir geri gönderme. Bunu yerine ince bir sorun s ve biraz ben bunu şu anda bırakacağız için bu makalenin kapsamı dışında. Ben, ancak yukarıdaki yaklaşımı kullanmayı deneyin geçirmenizi ve üzerinden Burada, takılabilir noktasına çalışır. Böyle bir alıştırma kazanılan t alma karşın, herhangi bir daha yakın çalışan başka bir sürüm GridView ve veri bağlama yaşam döngüsü daha derin bir anlayış teşvik yardımcı olur.

Diğer bir yaklaşım ekleme özel bir şablon ve Bu öğretici için kullanacağız yaklaşımı alt düzey işaretlemede eklemektir bir [değişmez değer denetim](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) şablon. Ardından GridView s `RowCreated` veya `RowDataBound` olay işleyicisi, değişmez değer denetim programlı olarak erişilebilir ve kendi `Text` özellik yaymak için işaretlemede ayarlayın.

Başlangıç RadioButton TemplateField s kaldırarak `ItemTemplate`, değişmez değer denetimiyle değiştirin. S değişmez değer denetim kümesi `ID` için `RadioButtonMarkup`.


[![ItemTemplate için değişmez değer denetim ekleme](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**Şekil 12**: bir sabit değer denetimine ekleme `ItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))


Ardından, GridView s için bir olay işleyicisi oluşturun `RowCreated` olay. `RowCreated` Eklenen her satır için olup olmadığını veri GridView'a DataSet'e bağlanır sonra olayı tetikler. Diğer bir deyişle, hatta geri göndermede veri görünümü durumundan yeniden yüklendiğinde `RowCreated` hala olayı tetikler ve bu kullanıyoruz yerine bunu nedeni `RowDataBound` (hangi harekete yalnızca zaman veri açıkça Web denetimi verilere bağlıdır).

Bu olay işleyicisinde yalnızca, devam etmek istediğimiz veri satırı uğraşmanızı re ediyoruz. Programlı olarak başvurmak istediğimiz için her veri satırı `RadioButtonMarkup` değişmez değer denetim ve kümesi kendi `Text` özelliğine yayması için işaretleme. Aşağıdaki kodda gösterildiği gibi bir radyo yayılan biçimlendirme oluşturur ayarlanmış düğmesini `name` özniteliği `SuppliersGroup`, olan `id` özniteliği `RowSelectorX`burada *X* GridView satır dizinidir ve `value` özniteliği GridView satır dizine ayarlayın.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

Bir GridView satır seçilidir ve bu bir geri gönderme gerçekleştiğinde ilgileniriz `SupplierID` seçili sağlayıcısının. Bu nedenle, bir değer her radyo düğmesinin fiili olmalıdır düşünebilirsiniz `SupplierID` (yerine GridView satır dizinini). Bu bazı durumlarda çalışabilir ancak körüne kabul edin ve işlemek için bir güvenlik riski olur bir `SupplierID`. Bizim GridView, örneğin, yalnızca ABD bu sağlayıcıları listeler. Ancak, varsa `SupplierID` yaramaz kullanıcının düzenleme durdurmak için hangi s radyo düğmesini doğrudan geçirilen `SupplierID` geri göndermede geri gönderilen değeri? Satır dizini olarak kullanarak `value`ve ardından alma `SupplierID` geri gönderme üzerinde `DataKeys` koleksiyonu emin oluruz kullanıcı yalnızca birini kullandığından `SupplierID` GridView satır biriyle ilişkili değerleri.

Bu olay işleyici kodu ekledikten sonra bir tarayıcıda sayfası test etmek için bir dakikanızı ayırın. İlk olarak, bu yalnızca bir radyo Not aynı anda kılavuzunda düğmesini seçilebilir. Ancak, ne zaman radyo düğmesini seçerek ve düğmelerden birine tıklayarak, bir geri gönderme oluşur ve tüm radyo düğmeleri kendi ilk durumuna geri döndürme (diğer bir deyişle, geri gönderme üzerinde seçili radyo düğmesinin artık seçili). Bu sorunu gidermek için genişletmek ihtiyacımız `RowCreated` BT'nin geri gönderme ' gönderilen seçilen radyo düğmesi dizin inceler ve ekler için olay işleyicisi `checked="checked"` satır dizini eşleşme yayılan işaretlemede özniteliği.

Bir geri gönderme ortaya çıktığında, tarayıcının geri gönderir `name` ve `value` seçili radyo düğmesinin. Değeri kullanılarak programlı olarak alınabilir `Request.Form("name")`. [ `Request.Form` Özelliği](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) sağlayan bir [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) temsil eden form değişkeni. Form değişkeni adları ve değerleri web sayfasında form alanlarının ve bir geri gönderme ensues her web tarayıcısı tarafından geri gönderilir. Çünkü işlenen `name` GridView radyo düğmeleri özniteliğidir `SuppliersGroup`, web sayfasının tarayıcı gönderecek geri gönderildiğinde `SuppliersGroup=valueOfSelectedRadioButton` web sunucusu (yanı sıra diğer form alanlarını) dön. Bu bilgiler daha sonra yanından erişilebilen `Request.Form` özelliğini kullanma: `Request.Form("SuppliersGroup")`.

Biz bu yana belirlemek için seçilen radyo düğmesinin de yalnızca dizin `RowCreated` ancak olay işleyicisi `Click` let s düğmesi Web denetimleri için olay işleyicileri, ekleme bir `SuppliersSelectedIndex` döndürürarkaplankodsınıfıözelliğini`-1`hiçbir radyo düğmesi seçilirse ve radyo düğmeleri biri seçiliyse seçili dizin.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

Eklenecek biliyoruz bu özelliği eklendi `checked="checked"` işaretlemede `RowCreated` olay işleyicisi, `SuppliersSelectedIndex` eşittir `e.Row.RowIndex`. Olay işleyicisi bu mantığı içerecek şekilde güncelleştirin:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

Bu değişiklik, bir geri gönderme sonra seçili radyo düğmesinin seçili kalır. Hangi radyo düğmesi seçilir belirtme olanağı sunuyoruz, biz davranışı sayfa ilk kez ziyaret edildi, ilk GridView satır s radyo düğmesi seçildi (yerine varsayılan olarak seçili hiçbir radyo düğmeleri sahip, geçerli olduğu değiştirin, böylece davranış). Varsayılan olarak seçilen ilk radyo düğmesi için yalnızca değiştirme `If SuppliersSelectedIndex = e.Row.RowIndex Then` aşağıdaki deyime: `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`.

Bu noktada gruplandırılmış radyo düğmelerinden oluşan bir sütunu seçili ve Geri göndermeler arasında anımsanacak tek bir GridView satır izin veren GridView ekledik. Bizim sonraki adımlar seçili sağlayıcı tarafından sağlanan ürünleri görüntülemek üzeresiniz. Adım 4'te nasıl kullanıcı başka bir sayfaya yönlendirmek seçilen gönderme görüyoruz `SupplierID`. Adım 5'te seçili sağlayıcı s ürünleri GridView aynı sayfada görüntülemek nasıl göreceğiz.

> [!NOTE]
> Bir TemplateField (uzun Bu adım 3 odağı) kullanmak yerine özel bir oluşturabilir `DataControlField` işlevselliği ve uygun kullanıcı arabirimi işleyen sınıfı. [ `DataControlField` Sınıfı](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) BoundField, CheckBoxField, TemplateField ve diğer yerleşik GridView ve DetailsView alanları türettiğiniz taban sınıfı. Özel bir oluşturma `DataControlField` sınıfı anlamına radyo düğmelerinden oluşan sütunu yalnızca bildirim temelli söz dizimi kullanılarak eklenebilir ve diğer web sayfaları ve diğer web uygulamaları önemli ölçüde daha kolay işlevselliği çoğaltma hale getirir.


Ancak, ASP.NET denetimleri bugüne kadar oluşturulmuş özel, önceden derlenmiş varsa, bunu yapmanız bu nedenle ciddi miktarda bir çalıştırabilirsiniz gerektirir ve onunla ıot'nin ve dikkatle işlenmelidir istisnai durumlara bir konağa taşır bilirsiniz. Bu nedenle, size özel olarak radyo düğmelerinden oluşan bir sütunu uygulama bırakmayı `DataControlField` sınıfı şimdilik ve TemplateField seçeneğiyle devam edin. Belki de biz oluşturma, kullanma ve özel dağıtma keşfetme olanağı gerekir `DataControlField` sınıfları bir sonraki öğreticide!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>4. adım: seçili sağlayıcı s ürünleri ayrı bir sayfa görüntüleme

Bir GridView satır kullanıcının seçtiği sonra seçili sağlayıcı s ürünleri göster gerekir. Bazı durumlarda, biz size aynı sayfada yapmak için tercih edebilirsiniz bazılarında ayrı bir sayfa bu ürünleri görüntülemek isteyebilirsiniz. İlk ürünleri ayrı bir sayfada görüntülemek nasıl inceleyin s sağlar; Adım 5'te GridView'a ekleme sırasında göz atacağız `RadioButtonField.aspx` seçili sağlayıcı s ürünleri görüntülemek için.

Şu anda sayfadaki iki düğme Web denetimleri vardır `ListProducts` ve `SendToProducts`. Zaman `SendToProducts` düğmesine tıklandığında, kullanıcıya göndermek istiyoruz `~/Filtering/ProductsForSupplierDetails.aspx`. Bu sayfa oluşturulduğu [ana/ayrıntı filtreleme arasında iki sayfa](../masterdetail/master-detail-filtering-across-two-pages-vb.md) öğretici ve tedarikçi ürünleri görüntüler, `SupplierID` adlı sorgu dizesi alanı geçirilen `SupplierID`.

Bu işlevi sağlamak için bir olay işleyicisi oluşturma `SendToProducts` s düğmesi `Click` olay. 3. adımda eklediğimiz `SuppliersSelectedIndex` seçili özelliği, radyo düğmesinin satır dizinini döndürür. Buna karşılık gelen `SupplierID` GridView s alınabilir `DataKeys` koleksiyonunu ve kullanıcı için ardından gönderilebilir `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` kullanarak `Response.Redirect("url")`.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

Bu kod, bir radyo düğmelerinden oluşan GridView seçili sürece son derece çalışır. Başlangıçta, seçtiğiniz radyo düğmeleri GridView yok ve kullanıcı, `SendToProducts` düğmesi `SuppliersSelectedIndex` olacaktır `-1`, bir özel bu yana durum neden olacak `-1` dizinaralığınındışında`DataKeys`koleksiyonu. Güncelleştirilecek verdiyseniz önemli değildir, ancak budur `RowCreated` başlangıçta seçili GridView ilk radyo düğmesinin olması için adım 3'te açıklandığı gibi olay işleyicisi.

Uyum sağlamak için bir `SuppliersSelectedIndex` değerini `-1`, etiket Web denetimi GridView yukarıda sayfasına ekleyin. Ayarlama, `ID` özelliğini `ChooseSupplierMsg`, kendi `CssClass` özelliğini `Warning`, kendi `EnableViewState` ve `Visible` özelliklerine `False`ve onun `Text` Lütfen özellik kılavuzundan bir sağlayıcı seçin. CSS sınıfının `Warning` kırmızı, italik, kalın, büyük yazı tipiyle metni görüntüler ve içinde tanımlanan `Styles.css`. Ayarlayarak `EnableViewState` ve `Visible` özelliklerine `False`, etiket dışında işlenmez yalnızca postbacks için yeri s denetim `Visible` programlı olarak ayarlanırsa `True`.


[![GridView yukarıda etiket Web denetim ekleme](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**Şekil 13**: Etiket Web denetimi yukarıda GridView ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))


Ardından, büyütmek `Click` görüntülemek için olay işleyicisi `ChooseSupplierMsg` kullanırsanız `SuppliersSelectedIndex` olan değerinden sıfır ve kullanıcı için yeniden yönlendirme `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` Aksi takdirde.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

Bir tarayıcı tıklayıp sayfasını ziyaret edin `SendToProducts` GridView bir tedarikçi seçmeden önce düğmesi. Şekil 14 gösterildiği gibi bu görüntüler `ChooseSupplierMsg` etiketi. Ardından, bir sağlayıcı seçin ve tıklayın `SendToProducts` düğmesi. Bu, seçilen sağlayıcı tarafından sağlanan olduğu ürünleri listeler bir sayfaya whisk. Şekil 15 gösterir `ProductsForSupplierDetails.aspx` Bigfoot Breweries tedarikçi seçildiğinde sayfa.


[![Hayır tedarikçi seçtiyseniz ChooseSupplierMsg etiketi gösterilir](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**Şekil 14**: `ChooseSupplierMsg` Hayır tedarikçi seçtiyseniz etiketi gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))


[![Seçili sağlayıcı s ürünleri ProductsForSupplierDetails.aspx içinde görüntülenir.](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**Şekil 15**: seçili sağlayıcı s ürünleri görüntülenir `ProductsForSupplierDetails.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>5. adım: seçili sağlayıcı s ürünleri aynı sayfa üzerinde görüntüleme

Adım 4'te kullanıcı başka bir seçili sağlayıcı görüntülemek için web sayfasına s ürünleri göndermek nasıl gördük. Alternatif olarak, aynı sayfada seçilen tedarikçi s ürünler görüntülenebilir. Bunu açıklamak üzere; başka bir GridView'a ekleyeceğiz `RadioButtonField.aspx` seçili sağlayıcı s ürünleri görüntülemek için.

Yalnızca bir sağlayıcı seçildikten sonra görüntülemek için bu GridView ürünlerin istiyoruz olduğundan, altında paneli Web denetim ekleme `Suppliers` GridView ayarlama, kendi `ID` için `ProductsBySupplierPanel` ve kendi `Visible` özelliğini `False`. Panel içinde seçili sağlayıcı için ürünleri metin ekleyin ve ardından adlı GridView tarafından `ProductsBySupplier`. GridView s akıllı etiketten adlı yeni bir ObjectDataSource bağlamak seçin `ProductsBySupplierDataSource`.


[![İçin yeni bir ObjectDataSource ProductsBySupplier GridView bağlama](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**Şekil 16**: bağlama `ProductsBySupplier` yeni ObjectDataSource GridView'a ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))


Ardından, kullanılacak ObjectDataSource yapılandırın `ProductsBLL` sınıfı. Yalnızca seçili sağlayıcı tarafından sağlanan bu ürünlerin almak istiyoruz beri ObjectDataSource çağırması gereken belirtin `GetProductsBySupplierID(supplierID)` verilerini almak için yöntemi. INSERT, UPDATE, aşağı açılan listelerden (hiçbiri) seçin ve sekmeleri SİLİN.


[![ObjectDataSource GetProductsBySupplierID(supplierID) yöntemi kullanmak üzere yapılandırma](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**Şekil 17**: ObjectDataSource kullanılacak yapılandırma `GetProductsBySupplierID(supplierID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))


[![(Hiçbiri) açılan listeler, ekleme, güncelleştirme ayarlayın ve sekme Sil](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**Şekil 18**: açılan listeler (hiçbiri), güncelleştirme, ekleme ve silme sekmeleri ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))


SELECT yapılandırdıktan güncelleştirme, ekleme ve sekmeleri SİLİN, İleri'ye tıklayın. Bu yana `GetProductsBySupplierID(supplierID)` yöntemi giriş parametresi bekliyor, bize Pro hodnotu parametru s kaynağını belirtmek için veri kaynağı Oluştur Sihirbazı'nı ister.

Birkaç burada kaynak parametre s değerinin de belirten bir seçenek sunuyoruz. Biz varsayılan parametre nesnesini kullanın ve değerini programlı olarak atama `SuppliersSelectedIndex` parametresi s özelliğini `DefaultValue` ObjectDataSource s özelliğinde `Selecting` olay işleyicisi. Kiracıurl [ObjectDataSource parametre değerlerini programlı olarak ayarlama](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md) program aracılığıyla ObjectDataSource s parametreleri için değerler atama Yenileyici Öğreticisi.

Alternatif olarak, biz bir ControlParameter'da olarak kullanıp başvurmak `Suppliers` GridView s [ `SelectedValue` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (bkz. Şekil 19). GridView s `SelectedValue` özelliği döndürür `DataKey` değeri ile eşleşen [ `SelectedIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). GridView s program üzerinden ayarlamak ihtiyacımız bu seçeneğin çalışması sırayla `SelectedIndex` özelliğini seçili olduğunda satır `ListProducts` düğmesine tıklandığında. Ayarlayarak ek bir avantaj olarak `SelectedIndex`, seçili kayıt sürer `SelectedRowStyle` tanımlanan `DataWebControls` tema (sarı bir arka plan).


[![GridView s SelectedValue parametre kaynağı olarak belirtmek için bir ControlParameter'da kullanın](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**Şekil 19**: SelectedValue GridView s parametre kaynağını belirtmek için bir ControlParameter'da kullanın ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))


Sihirbazı tamamladığınızda, Visual Studio ürün s veri alanları için alanları otomatik olarak eklenir. Kaldırma dışındaki tüm `ProductName`, `CategoryName`, ve `UnitPrice` BoundFields, değiştirip `HeaderText` ürün, kategori ve fiyat özellikler. Yapılandırma `UnitPrice` BoundField böylece değerini para birimi olarak biçimlendirilir. Bu değişiklikleri yaptıktan sonra paneli, GridView ve ObjectDataSource s bildirim temelli biçimlendirmeyi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

Bu alıştırmada tamamlanması GridView s ayarlamak ihtiyacımız `SelectedIndex` özelliğini `SelectedSuppliersIndex` ve `ProductsBySupplierPanel` paneli s `Visible` özelliğini `True` olduğunda `ListProducts` düğmesine tıklandığında. Bunu gerçekleştirmek için bir olay işleyicisi oluşturma `ListProducts` düğmesi Web denetimi s `Click` olay ve aşağıdaki kodu ekleyin:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

GridView ' bir tedarikçi seçilmedi, `ChooseSupplierMsg` etiketi gösterilir ve `ProductsBySupplierPanel` gizli paneli. Aksi takdirde bir tedarikçi seçtiyseniz `ProductsBySupplierPanel` görüntülenir ve GridView s `SelectedIndex` özellik güncelleştirilir.

Şekil 20 Bigfoot Breweries tedarikçi seçildi sonra Göster ürün sayfası düğmesine tıklandığında sonuçları gösterilmektedir.


[![Bigfoot Breweries göre sağlanan ürünlerin aynı sayfada listelenir](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**Şekil 20**: Bigfoot Breweries göre sağlanan ürünlerin aynı sayfada listelenir ([tam boyutlu görüntüyü görmek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))


## <a name="summary"></a>Özet

Bölümünde açıklandığı gibi [ana/Ayrıntılar Detailview'u ile seçilebilir bir ana GridView kullanan Detail](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) Öğreticisi, kayıtları bir CommandField kullanarak GridView seçilebilir olan `ShowSelectButton` özelliği `True`. Ancak CommandField düğmeleri normal düğmeler, bağlantılar veya görüntü olarak görüntüler. Radyo düğmesinin veya onay kutusu her GridView satırında bir alternatif satır seçimi kullanıcı arabirimini sağlamaktır. Bu öğreticide, biz nasıl radyo düğmelerinden oluşan bir sütun eklemek incelenir.

Ne yazık ki, radyo düğmeleri birincile t sütunu bir bekleyebileceğiniz gibi basit veya basit olarak ekleniyor. Bir düğmeye tıklayarak eklenebilir hiçbir yerleşik RadioButtonField yoktur ve bir TemplateField içinde RadioButton Web denetimi kullanarak sorunları kendi kümesi sunar. Sonunda, böyle bir arabirim sağlamak ya da özel bir oluşturmak sahibiz `DataControlField` sınıfı veya oturum sırasında bir TemplateField uygun HTML ekleme için çare `RowCreated` olay.

Radyo düğmelerinden oluşan bir sütun eklemek nasıl incelediniz izin bize onay kutularından oluşan bir sütun eklemek için uygulamamızla açın. Onay kutularından oluşan bir sütunla bir kullanıcı bir veya daha fazla GridView satır seçin ve ardından (örneğin, e-postalar bir dizi web tabanlı e-posta istemcisinden seçip ardından tüm seçilen e-postaları silmek) seçili tüm satırları bazı işlemi gerçekleştirin. Sonraki öğreticide böyle bir sütun eklemek nasıl göreceğiz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme David Suru oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
> [İleri](adding-a-gridview-column-of-checkboxes-vb.md)
