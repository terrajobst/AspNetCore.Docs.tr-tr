---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: Radyo düğmelerini (VB) GridView sütun ekleme | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, tek bir satır seçme daha sezgisel bir yol ile kullanıcı sağlamak üzere bir GridView denetimi radyo düğmelerini bir sütun eklemek nasıl bakar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: 39f6102e6b56e4bf258ca582633624d752bdc81f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-gridview-column-of-radio-buttons-vb"></a>Radyo düğmelerini (VB) GridView sütun ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe) veya [PDF indirin](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> Bu öğretici kullanıcı GridView, tek bir satır seçme daha sezgisel bir yol sağlamak için bir GridView denetimi radyo düğmelerini bir sütun eklemek nasıl bakar.


## <a name="introduction"></a>Giriş

GridView denetiminin büyük bir bölümünü yerleşik işlevselliği sunar. Metin, görüntüler, bağlar ve düğmeleri görüntüleme için farklı alan sayısını içerir. Bu, daha fazla özelleştirme için şablonları destekler. Fare birkaç tıklama ile burada her satır seçilebilir bir düğme GridView yapmak veya düzenleme veya silme özellikleri etkinleştirmek için olası s. Sağlanan özellikler sayısız rağmen genellikle olacaktır durumlarda, ek, desteklenmeyen özellikleri eklenmesi gerekir. Bu öğretici ve sonraki iki biz ek özellikleri içerecek şekilde GridView s işlevselliğini geliştirmek nasıl inceleyeceğiz.

Bu öğretici ve bir sonraki satır seçimi işlem arttırma odaklanın. İçinde incelenmesi gibi [ana/Ayrıntılar DetailView ile seçilebilir ana GridView kullanarak ayrıntı](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md), bir CommandField seçme düğmesi içerir GridView ekleyebilirsiniz. Tıklatıldığında geri gönderimin ensues ve GridView s `SelectedIndex` özelliği, Select düğmesine tıklanana satırın dizini güncelleştirildi. İçinde [ana/Ayrıntılar DetailView ile seçilebilir ana GridView kullanarak ayrıntı](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) Öğreticisi, seçili GridView satır ayrıntılarını görüntülemek için bu özelliği kullanmak nasıl gördük.

Birçok durumda Seç düğmesini yapmaya çalışırken, bu da başkaları için çalışmayabilir. Düğme kullanarak yerine iki diğer kullanıcı arabirimi öğeleri seçimi için kullanılanlardır: radyo düğmesini seçin ve onay kutusu. Onay kutusunu veya radyo düğmesi seçme düğmesi yerine, her bir satır içeren biz GridView genişletebilirsiniz. Burada kullanıcı yalnızca GridView kayıtları birini seçebilir senaryolarda, radyo düğmesinin Seç düğmesini tercih edilen olabilir. Burada kullanıcının olası seçebilirsiniz durumlarda gibi birden çok kayıt olduğu bir kullanıcı onay kutusunu silmek için birden çok ileti seçmek isteyebilirsiniz, bir web tabanlı e-posta uygulamasında sunar Seç düğmesini veya radyo düğmesi kullanılamıyor işlevi kullanıcı arabirimleri.

Bu öğretici için GridView radyo düğmelerini bir sütun ekleme bakar. Devam etmeden öğretici onay kutularını kullanarak araştırır.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>1. adım: geliştirme GridView Web sayfaları oluşturma

Radyo düğmelerini sütununu dahil etmek için GridView geliştirme başlamadan önce öncelikle Bu öğretici ve sonraki iki yapmamız gereken Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırın s olanak tanır. Adlı yeni bir klasör eklemeye başlayın `EnhancedGridView`. Ardından, o klasördeki her sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleme `Site.master` ana sayfa:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![ASP.NET sayfaları için SqlDataSource ile ilgili öğreticiler ekleme](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**Şekil 1**: SqlDataSource ilgili öğreticileri için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `EnhancedGridView` klasörü öğreticileri kendi bölümünde listeler. Sözcüğünün `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimi Ekle `Default.aspx` s Tasarım görünümü sayfaya Çözüm Gezgini'nden sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**Şekil 2**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))


Son olarak, bu dört sayfaları girişlere ekleyin `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme sonra kullanma eklemeniz SqlDataSource denetimi `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticileri Web sitesini görüntülemek için bir dakikanızı ayırın. Soldaki menüde artık düzenleme, ekleme ve öğreticiler silmek için öğeler içerir.


![Artık Site Haritası GridView öğreticileri iyileştirmeyi girişleri içerir](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**Şekil 3**: artık Site Haritası GridView öğreticileri iyileştirmeyi girişleri içerir


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>2. adım: bir GridView tedarikçileri görüntüleme

Bu öğretici GridView yapı s denetlemesine izin vermek için ABD sağlayıcıdan her GridView satır sağlayarak radyo düğmesi listeler. Radyo düğmesi aracılığıyla tedarikçi seçtikten sonra kullanıcı bir düğmeye tıklayarak tedarikçi s ürünleri görüntüleyebilirsiniz. Bu görev Önemsiz ses olsa da, özellikle zor hale subtleties dizi vardır. Biz bu subtleties inceleyin önce ilk tedarikçileri listeleme GridView alma s olanak tanır.

Başlangıç açarak `RadioButtonField.aspx` sayfasındaki `EnhancedGridView` tasarımcıya araç GridView sürükleyerek klasör. GridView s ayarlamak `ID` için `Suppliers` ve yeni bir veri kaynağı oluşturmak için akıllı etiketten seçin. Özellikle, adlandırılmış bir ObjectDataSource oluşturma `SuppliersDataSource` kendi verisinden çeker `SuppliersBLL` nesnesi.


[![SuppliersDataSource adlı yeni bir ObjectDataSource oluşturma](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**Şekil 4**: yeni ObjectDataSource adlandırılmış oluşturma `SuppliersDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))


[![ObjectDataSource SuppliersBLL sınıfını kullanmak için yapılandırma](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**Şekil 5**: ObjectDataSource kullanılacak yapılandırma `SuppliersBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))


Biz yalnızca bu sağlayıcılardan ABD listelemek istediğiniz beri seçin `GetSuppliersByCountry(country)` yöntemi seçme sekmesinde aşağı açılan listeden.


[![ObjectDataSource SuppliersBLL sınıfını kullanmak için yapılandırma](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**Şekil 6**: ObjectDataSource kullanılacak yapılandırma `SuppliersBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))


(Hiçbiri) seçeneğini ve İleri'yi güncelleştirme sekmesinden seçin.


[![ObjectDataSource SuppliersBLL sınıfını kullanmak için yapılandırma](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**Şekil 7**: ObjectDataSource kullanılacak yapılandırma `SuppliersBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))


Bu yana `GetSuppliersByCountry(country)` yöntemi bir parametre kabul eder, bize bu parametrenin kaynağı için veri kaynağı Yapılandırma Sihirbazı'nı ister. (Bu örnekte, ABD), sabit kodlanmış bir değer belirtmek için kaynak aşağı açılan liste hiçbiri olarak ayarlayın ve varsayılan değeri metin kutusuna girin parametrenin bırakın. Sihirbazı tamamlamak için Son'u tıklatın.


[![ABD ülke parametresi için varsayılan değer olarak kullanın](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**Şekil 8**: kullanım ABD için varsayılan değer olarak `country` parametre ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))


Sihirbazı tamamladıktan sonra GridView bir BoundField tedarikçi veri alanların her biri için içerir. Kaldırma dışındaki tüm `CompanyName`, `City`, ve `Country` BoundFields ve yeniden adlandırma `CompanyName` BoundFields `HeaderText` tedarikçiye özelliği. Bunu yaptıktan sonra GridView ve ObjectDataSource Tanımlayıcı Sözdizimi aşağıdakine benzer görünmelidir.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

Bu öğretici için seçilen tedarikçi görüntülemek için kullanıcı s ürünleri sağlayıcı listesi aynı sayfada ya da başka bir sayfaya izin s olanak tanır. Bunu yapabilmek için iki düğmesi Web denetimleri sayfasına ekleyin. I ullanıcı kümesi `ID` s için bu iki düğmelerinin `ListProducts` ve `SendToProducts`, fikir ile olduğunda `ListProducts` tıklandığında geri gönderimin ortaya çıkar ve seçili tedarikçi s ürünleri ne zaman ancak aynı sayfa üzerinde listelenen `SendToProducts` tıklandığında, Kullanıcı ürünleri listeler başka bir sayfaya whisked.

Şekil 9 gösterir `Suppliers` GridView ve iki düğmesi Web tarayıcı üzerinden görüntülendiğinde denetler.


[![ABD Bu sağlayıcıdan kendi adına, şehir ve listelenen ülke bilgileri sahip](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**Şekil 9**: Bu sağlayıcıdan ABD sahip Their adını, şehir ve ülke listelenen bilgileri ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>3. adım: radyo düğmelerini bir sütun ekleme

Bu noktada `Suppliers` GridView şirket adını, şehir ve her üretici ülke ABD'de görüntüleme üç BoundFields sahiptir. Radyo düğmeleri, sütunun ancak hala bulunmaması. Ne yazık ki, GridView içermiyor t, kılavuza ekleyebilirsiniz ve yapılması bir yerleşik RadioButtonField, aksi takdirde içerir. Bunun yerine, bir TemplateField ekleyebilir ve yapılandırma kendi `ItemTemplate` her GridView satır için bir radyo düğmesi sonuçta bir radyo düğmesi oluşturulacak.

Başlangıçta, istenen kullanıcı arabirimi RadioButton Web denetimi ekleyerek uygulanabilir varsayıyoruz `ItemTemplate` bir TemplateField. Bu gerçekten GridView her satır için bir tek radyo düğmesi ekler, ancak radyo düğmeleri gruplanamıyor ve bu nedenle birbirini dışlamaz. Diğer bir deyişle, son kullanıcının GridView aynı anda birden çok radyo düğmeleri seçebilirsiniz.

Let s TemplateField RadioButton Web denetimleri kullanarak ihtiyacımız işlevselliği sunmaz olsa bile, şekliyle bu yaklaşımı uygulamak elde edilen radyo düğmeleri değil gruplandırılır neden incelemek için faydalı s. Hangi tedarikçilerin en soldaki alan kolaylaştırarak GridView için bir TemplateField ekleyerek başlayın. Ardından, GridView s akıllı etiket Şablonları Düzenle bağlantısına tıklayın ve bir RadioButton Web Denetimi Araç Kutusu'ndan TemplateField s sürükleyin `ItemTemplate` (bkz. Şekil 10). RadioButton s ayarlamak `ID` özelliğine `RowSelector` ve `GroupName` özelliğine `SuppliersGroup`.


[![ItemTemplate RadioButton Web denetim ekleme](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**Şekil 10**: RadioButton Web denetimine ekleme `ItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))


Bu eklemeleri Tasarımcısı aracılığıyla yaptıktan sonra GridView s biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

RadioButton s [ `GroupName` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) ne radyo düğmeleri, bir dizi gruplandırmak için kullanılır. Aynı tüm RadioButton denetimlerini `GroupName` değeri olarak kabul edilir gruplandırılmış; yalnızca bir radyo düğmesi seçilebilir bir gruptan aynı anda. `GroupName` Özellik belirtir s işlenmiş radyo düğmesi için değer `name` özniteliği. Radyo düğmeleri tarayıcı inceler `name` radyo belirlemek için öznitelikler düğmesini gruplandırmaları.

Eklenen RadioButton Web denetimi ile `ItemTemplate`, bir tarayıcı aracılığıyla bu sayfasını ziyaret edin ve kılavuz s satırları radyo düğmeleri tıklayın. Radyo düğmeleri değil nasıl gruplandırılacağını dikkat edin, tüm satırları Şekil 11 seçmek olası hale getirerek gösterir.


[![GridView s radyo düğmeleri değil gruplandırılmış olan](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**Şekil 11**: GridView s radyo düğmeleri gruplanır değil ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))


Radyo düğmeleri değil gruplandırılmış neden çünkü bunların işlenmiş `name` öznitelikleri aynı önceliklere sahip olmasına rağmen farklı `GroupName` özellik ayarı. Bu farklılıklar görmek için görünümü/kaynak tarayıcıdan yapın ve radyo düğmesi biçimlendirme inceleyin:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

Bildirim nasıl hem `name` ve `id` öznitelikleri için değerleri tam belirtildiği gibi özellikleri penceresinde değildir, ancak bu, diğer bir sayı $a `ID` değerleri. Ek `ID` işlenen önüne eklenen değerleri `id` ve `name` öznitelikleri `ID` s radyo düğmeleri üst denetimleri `GridViewRow` s `ID` s, GridView s `ID`, Denetim s içerik `ID`ve Web formu s `ID`. Bunlar `ID` s eklenir, böylece her çizilir GridView Web denetiminde olan benzersiz bir `id` ve `name` değerleri.

Her denetim gereksinimlerini farklı bir işlenen `name` ve `id` bu nasıl tarayıcı istemci tarafı ve nasıl web sunucusuna hangi eylemini tanımlar her denetim benzersiz olarak tanımlayan veya değişikliğinin geri göndermede olduğundan. Örneğin, biz durumu değişti RadioButton s işaretli olduğunda bazı sunucu tarafı kodu çalıştırmak istediğinizi düşünelim. Biz RadioButton s ayarlayarak bunu `AutoPostBack` özelliğine `True` ve bir olay işleyicisi oluşturma `CheckChanged` olay. Ancak, işlenen `name` ve `id` radyo düğmeleri tümünün aynı olan üzerinde hangi özel belirleyemedik geri gönderme değerleri RadioButton tıklattınız.

Bunu kısa biz RadioButton Web denetimi kullanarak bir GridView radyo düğmelerini bir sütunu oluşturulamıyor ' dir. Bunun yerine, bunun yerine tasarlanmayan teknikleri uygun biçimlendirme her GridView satırına eklenmiş emin olmak için kullanırız gerekir.

> [!NOTE]
> RadioButton Web denetimi, bir şablon için eklendiğinde HTML denetimi, radyo düğmesi benzersiz içerecektir gibi `name` özniteliği, radyo düğmeleri gruplanmamış kılavuzunda yapma. HTML denetimleri ile bilmiyorsanız, HTML denetimlerini nadiren, özellikle ASP.NET 2.0 ile kullanılan gibi bu not göz ardı çekinmeyin. Ancak, daha fazla bilgi edinmek istiyorsanız, bkz: [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s blog girdisi [Web denetimleri ve HTML denetimlerini](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Radyo düğmesi biçimlendirmesi eklemesine değişmez değer bir denetimi kullanma

Doğru radyo düğmelerini GridView içindeki tüm grup için el ile içine radyo düğmeleri biçimlendirmesi eklemesine ihtiyacımız `ItemTemplate`. Her radyo düğmesi aynı gereken `name` özniteliği, ancak bir benzersiz olmalıdır `id` (istemci tarafı komut dosyası aracılığıyla radyo düğmesi erişim istiyoruz durumda) özniteliği. Bir kullanıcı radyo düğmesini seçer ve gönderileri geri sayfa sonra tarayıcıyı geri s seçili radyo düğmesinin değeri göndermek `value` özniteliği. Bu nedenle, her radyo düğmesi benzersiz bir gerekir `value` özniteliği. Son olarak, geri göndermede eklediğinizden emin olmak ihtiyacımız `checked` öznitelik seçimi ve gönderileri geri kullanıcı yaptıktan sonra Aksi takdirde seçili bir radyo düğmesi için radyo düğmeleri döndürecektir varsayılan durumlarına (tüm seçilmemiş).

Alt düzey biçimlendirme bir şablona ekle için gerçekleştirilecek iki yaklaşım vardır. Biçimlendirme ve arka plan kodu sınıfında tanımlanan yöntemler biçimlendirme çağrıları bir karışımını yapmak için biridir. Bu teknik ilk içinde açıklanan [kullanarak TemplateFields GridView denetiminde](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) Öğreticisi. Örneğimizde, şöyle görünebilir:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

Burada, `GetUniqueRadioButton` ve `GetRadioButtonValue` uygun döndürdü arka plan kodu sınıfında tanımlanan yöntemler olacaktır `id` ve `value` öznitelik değerlerini her radyo düğmesi için. İyi atamak için bu yaklaşım çalışır `id` ve `value` öznitelikleri, ancak belirtmek ihtiyacı olduğunda kısa döner `checked` veri GridView ilk bağlandığında databinding sözdizimi yalnızca çalıştırıldığı için öznitelik değeri. GridView Görünüm durumunun etkin varsa, bu nedenle, biçimlendirme yöntemleri yalnızca zaman ateşlenir sayfa ilk kez yüklenen (veya ne zaman GridView açıkça DataSet'e veri kaynağına) ve bu nedenle ayarlar işlevi `checked` t won özniteliği çağrılabilir geri gönderme. Bu s yerine zarif bir sorun ve biraz t, şu anda bırakacağız şekilde bu makalenin kapsamı dışında. I yapmak için ancak, yukarıdaki yaklaşım kullanarak denemek için öneririz ve burada, takılı noktası üzerinden çalışır. Bu tür bir won alıştırma t alma olsa da, tüm daha yakın çalışan bir sürüm, daha derin bir anlayış GridView ve Veri bağlamada yaşam döngüsü verildiğine yardımcı olur.

Diğer bir yaklaşım injecting özel bir şablon ve biz Bu öğreticide kullanacaksınız yaklaşımı alt düzey biçimlendirme eklemektir bir [değişmez değer denetim](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) şablon. Ardından, GridView s `RowCreated` veya `RowDataBound` olay işleyicisi değişmez değer denetim programlı olarak erişilebilir ve kendi `Text` özelliği biçimlendirme yaymak üzere ayarlanmış.

Başlat TemplateField s'den RadioButton kaldırarak `ItemTemplate`, sabit bir denetimle değiştirme. S değişmez değer denetimini ayarlama `ID` için `RadioButtonMarkup`.


[![ItemTemplate değişmez değer denetim ekleme](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**Şekil 12**: değişmez değer denetimine ekleme `ItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))


Ardından, GridView s için bir olay işleyicisi oluşturun `RowCreated` olay. `RowCreated` Olay eklenen her satır için olsun veya olmasın veri GridView DataSet'e bağlanır sonra ateşlenir. Anlamına gelir, hatta geri göndermede veri görünümü durumundan yeniden yüklendiğinde `RowCreated` Olay hala gönderir ve bu biz bunu kullanıyor yerine neden `RowDataBound` (hangi harekete yalnızca ne zaman veri açıkça Web denetimi verilere bağlıdır).

Bu olay işleyicisi yalnızca taktirde istiyoruz biz veri satırı postalarla re. Her veri satırı için programlı olarak başvurmak istiyoruz `RadioButtonMarkup` değişmez değer denetim ve kümesi kendi `Text` yayma için biçimlendirme özelliğine. Aşağıdaki kodda gösterildiği gibi bir radyo yayılan biçimlendirme oluşturur, düğme `name` özniteliği olarak ayarlanmış `SuppliersGroup`, `id` özniteliği olarak ayarlanmış `RowSelectorX`, burada *X* GridView satır dizini ve, `value` özniteliği GridView satır dizinine ayarlanır.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

GridView satır seçilir ve geri gönderimin gerçekleştiğinde, biz ilginizi `SupplierID` seçili tedarikçi. Bu nedenle, bir her radyo düğmesinin değeri gerçek olması gerektiğini düşünebilirsiniz `SupplierID` (yerine GridView satır dizini). Bu bazı durumlarda çalışabilir olsa da, doğrudan kabul etmek ve işlemek için bir güvenlik riski doğurur bir `SupplierID`. Bizim GridView, örneğin, yalnızca ABD Üreticiler listeler. Ancak, varsa `SupplierID` radyo düğmesi, düzenleme yaramaz kullanıcının durdurmak için hangi s doğrudan geçirilen `SupplierID` geri göndermede geri gönderilen değer? Satır dizini olarak kullanarak `value`ve ardından alma `SupplierID` geri gönderme üzerinde `DataKeys` koleksiyonu biz olun kullanıcı yalnızca birini kullandığını `SupplierID` GridView satırları biriyle ilişkili değerler.

Bu olay işleyici kodu ekledikten sonra sayfasını bir tarayıcıda çıkışı test etmek için bir dakikanızı ayırın. İlk olarak, bu yalnızca bir radyo Not aynı anda düğmesi kılavuzundaki seçilebilir. Ancak, ne zaman radyo düğmesini seçerek ve düğmelerden birini tıklatarak, geri gönderimin oluştuğu ve tüm radyo düğmeleri ilk durumlarına geri döndürmek (diğer bir deyişle, geri göndermede, seçili radyo düğmesinin artık seçilidir). Bu sorunu gidermek için büyütmek için ihtiyacımız `RowCreated` olan geri gönderme gönderilen seçili radyo düğmesi dizin inceler ve ekler için olay işleyicisini `checked="checked"` özniteliği satır dizini eşleşmeleri verilmiş biçimlendirme.

Geri gönderimin ortaya çıktığında, tarayıcının geri gönderdiği `name` ve `value` seçili radyo düğmesinin. Değer kullanılarak programlı olarak alınabilir `Request.Form("name")`. [ `Request.Form` Özelliği](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) sağlayan bir [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) form değişkeni temsil eden. Form değişkeni adları ve web sayfasında form alanların değerlerini ve geri gönderimin ensues olduğunda web tarayıcısı tarafından gönderilir. Çünkü işlenen `name` GridView radyo düğmeleri özniteliğidir `SuppliersGroup`, web sayfasını tarayıcıda gönderecek geri gönderildiğinde `SuppliersGroup=valueOfSelectedRadioButton` (yanı sıra diğer form alanlarını) web sunucusu dön. Bu bilgiler daha sonra erişilebilen `Request.Form` özelliği kullanılarak: `Request.Form("SuppliersGroup")`.

Biz bu yana belirlemek için seçilen radyo düğmesinin yalnızca de dizin `RowCreated` ancak olay işleyicisi `Click` düğmesi Web denetimleri için olay işleyicileri, let s ekleyin bir `SuppliersSelectedIndex` döndürürarkaplandakikodsınıfıözelliğine`-1`hiçbir radyo düğmesi seçilirse ve bir radyo düğmesi seçilirse, seçili dizin.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

Eklenecek biliyoruz eklenen bu özellik ile `checked="checked"` biçimlendirmede `RowCreated` olay işleyicisi zaman `SuppliersSelectedIndex` eşittir `e.Row.RowIndex`. Bu mantığı eklemek için olay işleyicisini güncelleştirin:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

Bu değişiklikle, seçili radyo düğmesinin sonra geri gönderimin seçili kalır. Hangi radyo düğmesi seçilir belirtme olanağı sunuyoruz, biz davranışı sayfa ilk sitesini ziyaret ettiğinizde birinci GridView satır s radyo düğmesi seçili (yerine, varsayılan olarak seçili Hayır radyo düğmeleri sahip, geçerli olduğu değişebilir davranış). Varsayılan olarak seçili ilk radyo düğmesi için basitçe değiştirme `If SuppliersSelectedIndex = e.Row.RowIndex Then` şu deyimi: `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`.

Bu noktada seçili ve geri göndermelere arasında hatırlanan tek bir GridView satır izin veren GridView için bir sütun gruplandırılmış radyo düğmelerini ekledik. Bizim sonraki adımlar seçili sağlayıcı tarafından sağlanan ürünleri görüntülemek üzeresiniz. Adım 4'te kullanıcı başka bir sayfasına yeniden yönlendirmek nasıl seçili gönderme göreceğiz `SupplierID`. Adım 5'te aynı sayfada GridView içinde seçilen tedarikçi s ürünleri görüntülemek nasıl göreceğiz.

> [!NOTE]
> TemplateField (Bu uzun adım 3 odak) kullanmak yerine özel bir oluşturuyoruz `DataControlField` uygun kullanıcı arabirimi ve işlevselliği işler sınıfı. [ `DataControlField` Sınıfı](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) BoundField, CheckBoxField, TemplateField ve diğer yerleşik GridView ve DetailsView alanlar kendisinden türetilen temel sınıftır. Özel oluşturma `DataControlField` sınıf anlamına radyo düğmelerini sütunu yalnızca bildirim temelli söz dizimini kullanarak eklenmiş ve diğer web sayfaları ve diğer web uygulamalarını önemli ölçüde daha kolay işlevselliği çoğaltma hale getirir.


Şimdiye kadar özel, oluşturulan ve derlenmiş ise ASP.NET denetimleri, ancak bunun nedenle legwork yeterince miktarda gerektirir ve ile bir ana bilgisayar subtleties ve dikkatle ele alınması kenar durumlarda taşır bildiğiniz. Bu nedenle, biz radyo düğmelerini özel olarak bir sütun uygulama atlayabilirsiniz `DataControlField` şu an için sınıf ve takılıyor TemplateField seçeneğiyle. Biz oluşturma, kullanma ve özel dağıtma keşfetmek için Fırsat belki de sahip olacaksınız `DataControlField` gelecekteki bir öğretici sınıflarda!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>4. adım: ayrı bir sayfasında seçilen tedarikçi s ürünleri görüntüleme

GridView satır kullanıcının seçtiği sonra seçili tedarikçi s ürünleri göster gerekir. Bazı durumlarda, biz biz aynı sayfada yapmak için tercih edebilirsiniz bazılarında ayrı bir sayfada, bu ürünler görüntülemek isteyebilirsiniz. İlk ürünleri ayrı bir sayfada görüntülemek nasıl inceleyin s sağlar; Adım 5'te biz bir GridView ekleme adresindeki göreceğiz `RadioButtonField.aspx` seçili tedarikçi s ürünleri görüntülemek için.

Şu anda sayfasında iki düğmesi Web denetimleri vardır `ListProducts` ve `SendToProducts`. Zaman `SendToProducts` düğmesine tıklandığında, kullanıcıya gönderilecek istiyoruz `~/Filtering/ProductsForSupplierDetails.aspx`. Bu sayfayı oluşturulan [ana/ayrıntı filtreleme arasında iki sayfa](../masterdetail/master-detail-filtering-across-two-pages-vb.md) öğretici ve görüntüler tedarikçi ürünleri, `SupplierID` adlı sorgu dizesi alanı geçirilen `SupplierID`.

Bu işlevselliği sağlayacak şekilde için bir olay işleyicisi oluşturun `SendToProducts` düğmesi s `Click` olay. Adım 3'te eklediğimiz `SuppliersSelectedIndex` , radyo düğmesinin satırın dizinini döndürür özelliği seçilidir. Karşılık gelen `SupplierID` GridView s'den alınabilir `DataKeys` koleksiyonu ve kullanıcı için sonra gönderilebilir `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` kullanarak `Response.Redirect("url")`.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

Radyo düğmeleri biri GridView seçilidir sürece bu kodu son derece çalışır. Başlangıçta, GridView seçili radyo düğmeleri yok ve kullanıcı, `SendToProducts` düğmesini `SuppliersSelectedIndex` olacaktır `-1`, bu yana durum için bir özel duruma neden olacak `-1` diziniaralıkdışında`DataKeys`koleksiyonu. Güncelleştirilecek karar verirseniz bu değil, ancak etkinleştirildiğinde `RowCreated` ilk radyo düğmesi başlangıçta seçilen GridView olması amacıyla adım 3'te açıklandığı gibi olay işleyicisi.

Uyum sağlamak için bir `SuppliersSelectedIndex` değerini `-1`, etiket Web denetimi GridView yukarıda sayfasına ekleyin. Ayarlama, `ID` özelliğine `ChooseSupplierMsg`, kendi `CssClass` özelliğine `Warning`, kendi `EnableViewState` ve `Visible` özelliklerine `False`ve kendi `Text` Lütfen özelliğine kılavuzdan bir üretici seçin. CSS sınıfı `Warning` kırmızı, italik, kalın, büyük yazı tipiyle metni görüntüleyen ve içinde tanımlanan `Styles.css`. Ayarlayarak `EnableViewState` ve `Visible` özelliklerine `False`, etiket dışında işlenmez yalnızca postbacks için nereye s denetim `Visible` özelliği ayarlanmış program aracılığıyla `True`.


[![GridView yukarıda etiket Web denetim ekleme](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**Şekil 13**: Etiket Web denetimi yukarıda GridView ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))


Ardından, büyütmek `Click` görüntülemek için olay işleyicisini `ChooseSupplierMsg` , etiket `SuppliersSelectedIndex` olan değerinden sıfır ve kullanıcı için yeniden yönlendirme `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` Aksi takdirde.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

Tarayıcı ve tıklatın sayfasını ziyaret edin `SendToProducts` GridView tedarikçi seçmeden önce düğmesi. Şekil 14 gösterildiği gibi bu görüntüler `ChooseSupplierMsg` etiketi. Ardından, bir üretici seçin ve tıklatın `SendToProducts` düğmesi. Bu, seçilen sağlayıcı tarafından sağlanan ürünleri listeler bir sayfaya whisk. Şekil 15 gösterir `ProductsForSupplierDetails.aspx` sayfasında Bigfoot Breweries tedarikçi seçildiğinde.


[![Hayır tedarikçi seçtiyseniz ChooseSupplierMsg etiketi görüntülenir](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**Şekil 14**: `ChooseSupplierMsg` etiket Hayır tedarikçi seçiliyse görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))


[![Seçili sağlayıcı s ürünleri ProductsForSupplierDetails.aspx içinde görüntülenir](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**Şekil 15**: Seçili tedarikçi s ürünleri görüntülenir `ProductsForSupplierDetails.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>5. adım: Seçili tedarikçi s ürünleri aynı sayfa üzerinde görüntüleme

Adım 4'te kullanıcı başka bir seçili tedarikçi görüntülemek için web sayfasına s ürünleri göndermek nasıl gördük. Alternatif olarak, seçili sağlayıcı s ürünleri aynı sayfa üzerinde görüntülenebilir. Bunu göstermek için başka bir GridView ekleyeceğiz `RadioButtonField.aspx` seçili tedarikçi s ürünleri görüntülemek için.

Yalnızca bir sağlayıcı seçildikten sonra görüntülemek için bu GridView ürünlerin istiyoruz beri Masası Web denetim altındaki ekleme `Suppliers` ayarı GridView, kendi `ID` için `ProductsBySupplierPanel` ve kendi `Visible` özelliğine `False`. Paneli seçili sağlayıcı için ürünleri metni ekleyin ve ardından adlı GridView tarafından `ProductsBySupplier`. Adlı yeni bir ObjectDataSource bağlamak GridView s akıllı etiketten seçin `ProductsBySupplierDataSource`.


[![İçin yeni bir ObjectDataSource ProductsBySupplier GridView bağlama](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**Şekil 16**: bağlama `ProductsBySupplier` yeni ObjectDataSource GridView ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))


Ardından, kullanılacak ObjectDataSource yapılandırın `ProductsBLL` sınıfı. ObjectDataSource çağıracağı biz yalnızca seçili sağlayıcı tarafından sağlanan bu ürünleri almak istediğiniz, bu yana geçen belirtin `GetProductsBySupplierID(supplierID)` verilerini alma yöntemi. (Hiçbiri) güncelleştirme, ekleme, aşağı açılan listelerden seçin ve sekmeleri SİLİN.


[![ObjectDataSource GetProductsBySupplierID(supplierID) yöntemi kullanmak üzere yapılandırma](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**Şekil 17**: ObjectDataSource kullanılacak yapılandırma `GetProductsBySupplierID(supplierID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))


[![Güncelleştirme, ekleme, açılan listeleri (hiçbiri) ayarlayın ve sekmeleri silme](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**Şekil 18**: aşağı açılan listeler (hiçbiri), güncelleştirme, ekleme ve silme sekmeleri ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))


Yapılandırma seçin sonra güncelleştirme, ekleme ve sekmeler Sil, İleri'yi tıklatın. Bu yana `GetProductsBySupplierID(supplierID)` yönteminin beklediği bir giriş parametresi, veri kaynağı Oluştur Sihirbazı'nı bize kaynağı parametre s değeri belirtmek üzere ister.

Biz bir birkaç burada buna bir parametre s kaynak belirtme seçeneğiniz vardır. Biz varsayılan parametre nesnesini kullanın ve program aracılığıyla değerini atayın `SuppliersSelectedIndex` parametresi s özelliğine `DefaultValue` ObjectDataSource s özelliğinde `Selecting` olay işleyicisi. Geri başvurmak [program aracılığıyla ObjectDataSource ait parametre değerleri ayarlanıyor](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md) program aracılığıyla ObjectDataSource s parametreleri için değerler atama Yenileyici Öğreticisi.

Alternatif olarak, size bir ControlParameter'da ve kullanabileceğiniz başvurmak `Suppliers` GridView s [ `SelectedValue` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (bkz. Şekil 19). GridView s `SelectedValue` özelliği döndürür `DataKey` değeri ile eşleşen [ `SelectedIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). GridView s programlı olarak ayarlamak ihtiyacımız bu seçeneğin çalışması sırayla `SelectedIndex` seçili özelliğine ne zaman satır `ListProducts` düğmesine tıklandığında. Ayarlayarak ek bir avantaj olarak `SelectedIndex`, seçili kaydı sürer `SelectedRowStyle` tanımlanan `DataWebControls` tema (sarı bir arka plan).


[![GridView s SelectedValue parametre kaynağı olarak belirtmek için bir ControlParameter'da kullanın](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**Şekil 19**: bir ControlParameter'da GridView s SelectedValue parametre kaynağı olarak belirtmek için kullanın ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))


Sihirbazı tamamladığınızda, Visual Studio ürün s veri alanları için alanları otomatik olarak eklenir. Kaldırma dışındaki tüm `ProductName`, `CategoryName`, ve `UnitPrice` BoundFields, değiştirip `HeaderText` ürün, kategori ve fiyat özellikleri. Yapılandırma `UnitPrice` BoundField böylece değeri para birimi olarak biçimlendirilir. Bu değişiklikleri yaptıktan sonra paneli, GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

Bu alıştırmada tamamlamak için GridView s ayarlamak ihtiyacımız `SelectedIndex` özelliğine `SelectedSuppliersIndex` ve `ProductsBySupplierPanel` Masası s `Visible` özelliğine `True` zaman `ListProducts` düğmesine tıklandığında. Bunu gerçekleştirmek için bir olay işleyicisi oluşturun `ListProducts` düğmesi Web denetimi s `Click` olay ve aşağıdaki kodu ekleyin:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

Bir sağlayıcının GridView, seçili olmadığında, `ChooseSupplierMsg` etiketi görüntülenir ve `ProductsBySupplierPanel` Masası gizli. Aksi durumda, bir tedarikçi seçtiyseniz `ProductsBySupplierPanel` görüntülenir ve GridView s `SelectedIndex` özelliği güncelleştirilir.

Şekil 20 Bigfoot Breweries tedarikçi seçili sonra Göster ürünleri sayfa düğmesine tıklandığında sonuçlarını gösterir.


[![Bigfoot Breweries tarafından sağlanan ürünleri aynı sayfada listelenen](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**Şekil 20**: ürünleri verdiği Bigfoot Breweries aynı sayfada listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))


## <a name="summary"></a>Özet

' Da anlatıldığı gibi [ana/Ayrıntılar DetailView ile seçilebilir ana GridView kullanarak ayrıntı](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) Öğreticisi, kayıtları bir CommandField kullanarak GridView seçilebilir, `ShowSelectButton` özelliği ayarlanmış `True`. Ancak CommandField kendi düğmeleri normal düğmeler, bağlantılar veya görüntüleri olarak görüntüler. Bir alternatif satır seçimi kullanıcı arabirimini radyo düğmesini veya onay kutusu her GridView satırda sağlamaktır. Bu öğreticide radyo düğmelerini bir sütun ekleme incelendi.

Ne yazık ki, bir beklediğiniz gibi bir sütun radyo düğmeleri olmayan t basit veya basit olarak ekleniyor. Bir düğmeyi tıklatarak eklenebilir hiçbir yerleşik RadioButtonField yoktur ve bir TemplateField içinde RadioButton Web denetimini kullanarak sorunları kendi kümesi sunar. Sonunda, böyle bir arabirim sağlamak için ya da özel bir oluşturmak sahibiz `DataControlField` sınıf veya TemplateField sırasında içine uygun HTML injecting için çare `RowCreated` olay.

Radyo düğmelerini bir sütun ekleme incelediniz izin bize onay kutuları, bir sütun eklemeyi için uygulamamızla açın. Onay kutularını sütunla bir kullanıcı bir veya daha fazla GridView satırları seçin ve ardından tüm (örneğin, e-postaların bir dizi web tabanlı e-posta istemcisinden seçilerek ve sonra tüm seçilen e-postaları silmek) seçili satırları başka bir işlem gerçekleştirin. Sonraki öğreticide böyle bir sütun ekleme göreceğiz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme David Suru oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
> [sonraki](adding-a-gridview-column-of-checkboxes-vb.md)
