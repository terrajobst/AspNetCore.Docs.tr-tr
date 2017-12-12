---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: "Veri bağlama denetimleri | Microsoft Docs"
author: microsoft
description: "Çoğu ASP.NET uygulamaları bir arka uç veri kaynağından veri sunumu bazı derecesini kullanır. Verilere bağlı denetimler etkileşen w bileşendirler bir parçası olmuştur..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 3ebb0f9a7a2f071b7bf7aa3855920f1a5784a61f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="data-bound-controls"></a>Veri bağlama denetimleri
====================
tarafından [Microsoft](https://github.com/microsoft)

> Çoğu ASP.NET uygulamaları bir arka uç veri kaynağından veri sunumu bazı derecesini kullanır. Verilere bağlı denetimler veri dinamik Web uygulamaları ile etkileşim bileşendirler bir parçası olmuştur. ASP.NET 2.0 verilere bağlı denetimler, yeni BaseDataBoundControl sınıfı ve bildirim temelli söz dizimi dahil olmak üzere bazı önemli geliştirmeler sunar.


Çoğu ASP.NET uygulamaları bir arka uç veri kaynağından veri sunumu bazı derecesini kullanır. Verilere bağlı denetimler veri dinamik Web uygulamaları ile etkileşim bileşendirler bir parçası olmuştur. ASP.NET 2.0 verilere bağlı denetimler, yeni BaseDataBoundControl sınıfı ve bildirim temelli söz dizimi dahil olmak üzere bazı önemli geliştirmeler sunar.

BaseDataBoundControl DataBoundControl ve HierarchicalDataBoundControl sınıf için temel sınıf olarak görev yapar. Bu modülde DataBoundControl türetilen aşağıdaki sınıflar aşağıdakiler ele alınacaktır:

- AdRotator
- Liste denetimleri
- GridView
- FormView
- DetailsView

Ayrıca HierarchicalDataBoundControl sınıfından türetilen aşağıdaki sınıflar aşağıdakiler ele alınacaktır:

- TreeView
- Menü
- SiteMapPath

## <a name="databoundcontrol-class"></a>DataBoundControl sınıfı

Tablo ile etkileşim kurmak için kullanılan (VB MustInherit işaretli) bir Özet sınıf DataBoundControl sınıftır veya liste stili veri. Aşağıdaki denetimleri DataBoundControl türetilen denetimler bazılarıdır.

## <a name="adrotator"></a>AdRotator

AdRotator denetimi, belirli bir URL bağlı bir Web sayfasında bir grafik başlığı görüntülemenizi sağlar. Görüntülenen grafiği için denetim özelliklerini kullanma döndürülür. Belirli ad görüntülemenin bir sayfada sıklığı kullanılarak yapılandırılabilir **izlenim** özelliği ve reklam filtre anahtar sözcüğü filtresini kullanma.

AdRotator denetimleri bir XML dosyası veya tablo veriler için bir veritabanını kullanın. Aşağıdaki öznitelikler XML dosyalarında AdRotator denetimini yapılandırmak için kullanılır.

### <a name="imageurl"></a>ImageUrl
Ad için görüntülenecek resim URL'si.

### <a name="navigateurl"></a>NavigateUrl
Ad tıklatıldığında kullanıcı için gerçekleştirilmelidir URL. Bu URL kodlanmış olmalıdır.

### <a name="alternatetext"></a>AlternateText
Bir araç ipucunda görüntülenir ve ekran okuyucular tarafından okunur alternatif metin. ImageUrl tarafından belirtilen görüntüsü bulunmadığında da görüntüler.

### <a name="keyword"></a>Anahtar sözcüğü
Anahtar sözcük filtreleri kullanılırken kullanılabilecek bir anahtar sözcük tanımlar. Belirtilmişse, yalnızca anahtar sözcüğü Filtresi ile eşleşen bir anahtar sözcüğüyle reklamlar görüntülenir.

### <a name="impressions"></a>İzlenim
Ne sıklıkta belirli ad görünmesi olasıdır belirleyen ağırlıklı sayı. Aynı dosyada diğer reklam izlenim görelidir. Bir XML dosyasındaki tüm reklamlar için toplu izlenim en büyük değerini 2,048,000,000 1'dir.

### <a name="height"></a>Yükseklik
Ad piksel cinsinden yüksekliği.

### <a name="width"></a>Genişlik
Ad piksel cinsinden genişliği.


> [!NOTE]
> Yükseklik ve genişlik öznitelikleri yüksekliğini ve genişliğini AdRotator denetimi için geçersiz kılar.


Tipik bir XML dosyası aşağıdakine benzeyebilir:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Yukarıdaki örnekte, contoso ad iki kez izlenim özniteliğinin değeri nedeniyle, ASP.NET Web sitesi için ad olarak görünmesi olarak olasıdır.

Yukarıdaki XML dosyasından ADs görüntülemek için bir sayfaya bir AdRotator denetimi ekleyin ve ayarlayın **AdvertisementFile** aşağıda gösterildiği gibi XML dosyasına işaret edecek şekilde özelliği:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Bir veritabanı tablosu AdRotator denetiminiz için veri kaynağı olarak kullanmayı seçerseniz, önce aşağıdaki Şeması'nı kullanarak bir veritabanını ayarlamak gerekir:

| **Sütun adı** | **Veri türü** | **Açıklama** |
| --- | --- | --- |
| Kimlik | int | Birincil anahtar. Bu sütun, herhangi bir ad olabilir. |
| ImageUrl | nvarchar (*uzunluğu*) | Ad için görüntülemek için göreli veya mutlak görüntünün URL'si. |
| NavigateUrl | nvarchar (*uzunluğu*) | Ad için hedef URL. Bir değer belirtmezseniz, ad köprü değil. |
| AlternateText | nvarchar (*uzunluğu*) | Görüntü bulunamazsa görüntülenen metin. Bazı tarayıcılarda metin araç ipucu olarak görüntülenir. Grafiğin göremiyorum kullanıcılar sesli okuma açıklamasını duyabileceğiniz alternatif metin erişilebilirlik için de kullanılır. |
| Anahtar sözcüğü | nvarchar (*uzunluğu*) | Sayfa filtre uygulayabilirsiniz ad için bir kategori. |
| İzlenim | int(4) | Ad ne sıklıkta görüntülenen olasılığını belirten bir sayı. Büyük sayı daha sık ad görüntülenir. XML dosyasındaki tüm izlenim değerlerin toplamını 2,048,000,000-1 aşamaz. |
| Genişlik | int(4) | Görüntünün piksel cinsinden genişliği. |
| Yükseklik | int(4) | Görüntünün piksel cinsinden yüksekliği. |

Zaten sahip olduğu bir veritabanı farklı bir şema ile durumlarda, kullandığınız **AlternateTextField**, **ImageUrlField**, ve **NavigateUrlField** eşlemek için özellikleri Mevcut veritabanını AdRotator öznitelikleri. AdRotator denetiminde veritabanındaki verileri görüntülemek için bir veri kaynağı denetimi sayfasına ekleme, veri kaynağı denetimi, veritabanına işaret etmek için bağlantı dizesini yapılandırmak ve AdRotator denetimin ayarlayın **DataSourceID** özelliği veri kaynağı denetiminin kimliği. AdRotator ads program aracılığıyla yapılandırmasına gerek sahip olduğu durumlarda, AdCreated olayını kullanın. AdCreated olay iki parametre alır; bir nesne, diğeri AdCreatedEventArgs örneği. AdCreatedEventArgs oluşturuluyor ad başvurudur.

Aşağıdaki kod parçacığını bir ad için program aracılığıyla ImageUrl, NavigateUrl ve AlternateText ayarlar:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Liste denetimleri

Liste denetimleri ListBox, DropDownList, CheckBoxList, RadioButtonList ve Bulletedlıst içerir. Bu denetimlerin her birinin veri bir veri kaynağına bağlı olabilir. Bunlar bir alan veri kaynağında görüntü metni olarak kullanın ve ikinci bir alan bir öğe değeri olarak isteğe bağlı olarak kullanabilirsiniz. Öğeleri tasarım zamanında de bir statik olarak eklenebilir ve statik öğeleri ve bir veri kaynağından eklenen dinamik öğeler karıştırabilirsiniz.

Veri için bir liste denetimini bağlama, bir veri kaynağı denetimi sayfasına ekleyin. Veri kaynağı denetimi için SELECT komutu belirtmek ve liste denetimi DataSourceID özelliği veri kaynağı denetiminin Kimliğini ayarlayın. Kullanım **DataTextField** ve **DataValueField** görüntüleme metni ve denetim değeri tanımlamak için özellikleri. Ayrıca, kullanabileceğiniz **DataTextFormatString** özelliğini aşağıdaki gibi görünen metin görünümünü denetlemek için:

| **İfade** | **Açıklama** |
| --- | --- |
| Fiyat: {0: c} | Sayısal/ondalık verileri. Sabit görüntüler "Fiyat:" sayılar para biçiminde arkasından. Para birimi biçimi kültür öznitelikte belirtilen kültür ayarı bağlıdır **sayfa** yönergesi veya Web.config dosyasında. |
| {0:D4} | Tamsayı veri. Ondalık sayılar ile kullanılamaz. Tamsayıları dört karakter geniş sıfır doldurulan bir alanında görüntülenir. |
| {0:N2} % | Sayısal veriler için. 2 ondalık basamak numarasıyla görüntüler duyarlık ardından değişmez değeri "%". |
| {0:000.0} | Sayısal/ondalık verileri. Sayılar bir ondalık basamak yuvarlanır. Sıfır doldurulan üç basamak daha az numaralandırır. |
| {0: D} | Tarih/Saat verileri. Uzun tarih biçimi ("Perşembe 06 Ağustos 1996") görüntüler. Tarih biçimi sayfası veya Web.config dosyasının kültür ayarına bağlıdır. |
| {0: d} | Tarih/Saat verileri. Görüntüler kısa tarih biçiminde ("12/31/99"). |
| {0:yy-aa-gg} | Tarih/Saat verileri. Sayısal yıl ay gün biçimine (96-08-06) tarihi görüntüler |

## <a name="gridview"></a>GridView

GridView denetim tablo verilerini görüntüleme ve bildirim temelli bir yaklaşım kullanarak düzenleme olanağı sağlar ve DataGrid denetimini devamıdır. GridView denetiminde aşağıdaki özellikler mevcuttur.

- Veri kaynağı denetimleri SqlDataSource gibi bağlama.
- Yerleşik sıralama özellikleri.
- Yerleşik güncelleştirme ve yetenekleri siliniyor.
- Yerleşik disk belleği özellikleri.
- Yerleşik satır seçimi yetenekleri.
- Dinamik olarak özelliklerini ayarlamak için GridView nesne modeline programlı erişim olayları işlemek ve benzeri.
- Birden çok anahtar alanları.
- Köprü sütunlar için birden çok veri alanları.
- Temalar ve stiller aracılığıyla özelleştirilebilir görünümü.

**Sütun alanlarını**

GridView denetiminde her sütun DataControlField nesnesiyle temsil edilir. Varsayılan olarak, GenerateColumns özelliğini ayarlamak **doğru**, veri kaynağındaki her bir alan için bir AutoGeneratedField nesnesi oluşturur. Her bir alan sonra her bir alan veri kaynağında göründüğü sıra GridView denetiminde sütun olarak işlenir. Alanlar görünür, hangi sütunun el ile de denetleyebilirsiniz **GridView** ayarlayarak denetim **GenerateColumns** özelliğine **false** ve ardından, kendi tanımlama sütun alanı koleksiyonu. Farklı bir sütun alan türleri denetiminde sütunların davranışını belirler.

Aşağıdaki tabloda kullanılabilir farklı bir sütun alan türleri listelenmektedir.

| **Sütun alan türü** | **Açıklama** |
| --- | --- |
| BoundField | Bir veri kaynağı, bir alanın değerini görüntüler. GridView denetiminin varsayılan sütun türü budur. |
| ButtonField | Her öğe için bir komut düğmesi GridView denetiminde görüntüler. Bu, bir sütun Ekle veya Kaldır düğmesi gibi özel düğme denetimlerinin oluşturmanıza olanak sağlar. |
| CheckBoxField | Her öğe için bir onay kutusu GridView denetiminde görüntüler. Bu sütun alan türü genellikle bir Boole değeri içeren alanlar görüntülemek için kullanılır. |
| CommandField | Görüntüler seçerek, düzenleme veya silme işlemlerini gerçekleştirmek için komut düğmeleri önceden tanımlanmış. |
| HyperLinkField | Bir alanın değeri bir veri kaynağında bir köprü olarak görüntüler. Bu sütun alan türü köprü URL'sine ikinci bir alana bağlamak sağlar. |
| ImageField | Her öğe için görüntü GridView denetiminde görüntüler. |
| TemplateField | Görüntüler kullanıcı tanımlı içerik belirtilen bir şablonu göre GridView denetiminde her öğe için. Bu sütun alan türü, bir özel sütun alanı oluşturmanıza olanak sağlar. |

Bir sütun alanı koleksiyonu bildirimli olarak tanımlamak için ilk kez açma ve kapama ekleyin  **&lt;sütunları&gt;**  açma ve kapatma etiketleri GridView denetiminin arasındaki etiketler. Ardından, açma ve kapatma arasında dahil etmek istediğiniz sütun alanları listesinde  **&lt;sütunları&gt;**  etiketler. Belirtilen sütun, listelenen sırayla sütun koleksiyonuna eklenir. **Sütunları** koleksiyonu depolar tüm sütun alanları denetiminde ve GridView denetiminde sütun alanları programlamayla yönetmenizi sağlar.

Açıkça bildirilen sütun alanlarını otomatik olarak oluşturulan sütun alanları ile birlikte görüntülenebilir. Her ikisi de kullanıldığında, açıkça bildirilen sütun alanları tarafından otomatik olarak oluşturulan sütun alanlarını ve ardından ilk olarak işlenir.

## <a name="binding-to-data"></a>Verilere Bağlama

GridView denetimi için veri kaynağı denetimi bağlanabilir (gibi **SqlDataSource**, **ObjectDataSource**, vb.), System.Collections.IEnumerable uygulayan gibi herhangi bir veri kaynağını yanı sıra Arabirim (örneğin, System.Data.DataView, System.Collections.ArrayList veya System.Collections.Hashtable). GridView denetimi için uygun veri kaynağı türü bağlamak için aşağıdaki yöntemlerden birini kullanın:

- Bir veri kaynağı denetimi bağlamak için veri kaynağı denetiminin kimliği değerine GridView denetiminin DataSourceID özelliğini ayarlayın. GridView denetimini otomatik olarak belirtilen veri kaynağı denetime bağlar ve veri kaynağının sıralama, güncelleştirme, silme ve disk belleği işlevleri gerçekleştirmek için denetimin özelliklerinden yararlanabilirsiniz. Bu veri bağlamak için tercih edilen yöntemdir.
- System.Collections.IEnumerable arabirimini uygulayan bir veri kaynağına bağlamak için programlamayla GridView denetiminin DataSource özelliği veri kaynağı olarak ayarlanmış ve DataBind yöntemini çağırın. Bu yöntemi kullanırken, GridView denetiminin yerleşik sıralama, güncelleştirme, silme ve işlevsellik disk belleği sağlamaz. Bu işlev kendiniz vermeniz gerekir.

## <a name="data-operations"></a>Veri İşlemleri

GridView denetimi sıralama, güncelleştirme, silme, seçin ve denetimindeki öğeleri aracılığıyla sayfasında kullanıcı çok sayıda yerleşik özellikleri sunar. GridView denetimi veri kaynağı denetime bağlı olduğunda GridView denetim denetimin özellikleri veri kaynağı yararlanmak ve Otomatik sıralama, güncelleştirme ve silme işlevselliği sağlar.

> [!NOTE]
> GridView denetiminin, sıralama, güncelleştirme ve diğer tür veri kaynaklarını silmek için destek sağlayabilirsiniz; Ancak, bu işlemlerin uygulamasını ile ilgili olay işleyicisi sağlamanız gerekir.


Sıralama, belirli bir sütuna göre GridView denetimindeki öğeleri sütunun başlığına tıklayarak sıralayın kullanıcıya sağlar. Sıralamayı etkinleştirmek için AllowSorting özelliğini ayarlamak **doğru**.

Otomatik güncelleştirme, silme ve seçim işlevler düğme etkin bir **ButtonField** veya **TemplateField** sütun alanıyla komut adı "Düzenle", "Delete" ve "Seç" Sırasıyla tıklandığında. GridView denetimini otomatik olarak ekleyebilir bir **CommandField** sütun alanı düzenleme, silme veya AutoGenerateEditButton, AutoGenerateDeleteButton veya AutoGenerateSelectButton özelliği için ayarlarsanızSeçdüğmesini**true**sırasıyla.

> [!NOTE]
> Veri kaynağında kayıtları ekleme doğrudan GridView denetim tarafından desteklenmiyor. Ancak, GridView denetimi DetailsView ile birlikte veya FormView denetimi kullanarak kayıtları eklemek mümkündür.


Aynı anda veri kaynağındaki tüm kayıtları görüntüleme yerine GridView denetimini otomatik olarak kayıtları sayfalarına bölmek. Disk belleği etkinleştirmek için AllowPagingözelliðini ayarlamak **doğru**.

## <a name="customizing-the-user-interface"></a>Kullanıcı Arabirimini Özelleştirme

Denetimin farklı bölümleri için stil özellikleri ayarlayarak GridView denetiminin görünümünü özelleştirebilirsiniz. Aşağıdaki tabloda farklı stil özellikleri listeler.

| **Stil özelliği** | **Açıklama** |
| --- | --- |
| AlternatingRowStyle | GridView denetiminde değişen verileri satır stili ayarları. Bu özellik ayarladığınızda, veri satırına RowStyle ayarları arasında değişen görüntülenir ve **AlternatingRowStyle** ayarlar. |
| EditRowStyle | GridView denetiminde düzenlenmekte olan satır için stil ayarları. |
| EmptyDataRowStyle | Veri kaynağı herhangi bir kayıt içermediğinde GridView denetiminde gösterilen boş veri satırı için stil ayarları. |
| FooterStyle | GridView denetiminin altbilgi satırının stilini ayarlar. |
| HeaderStyle | GridView denetiminin başlık satırının stilini ayarlar. |
| PagerStyle | GridView denetiminin çağrı cihazı satırının stilini ayarlar. |
| RowStyle | GridView denetiminde veri satırına stilini ayarlar. Zaman **AlternatingRowStyle** özelliğini de ayarlayın, veri satırı arasında değişen görüntülenen **RowStyle** ayarları ve **AlternatingRowStyle** ayarlar. |
| SelectedRowStyle | GridView denetiminde seçili satır stili ayarları. |

Ayrıca, göstermek veya denetimi farklı kısımlarını gizleyebilirsiniz. Aşağıdaki tabloda, hangi bölümlerinin gösterileceğini veya gizleneceğini denetleyen özellikler listelenmektedir.

| **Özelliği** | **Açıklama** |
| --- | --- |
| ShowFooter | Gösterir veya gizler GridView denetiminin altbilgi bölümü. |
| ShowHeader | Gösterir veya gizler GridView denetiminin üstbilgi bölümü. |

### <a name="events"></a>Olaylar

GridView denetiminin karşı program çeşitli olayları sağlar. Bu, her bir olay gerçekleştiğinde bir özel yordam çalıştırmanızı sağlar. Aşağıdaki tabloda GridView denetimi tarafından desteklenen olayları listeler.

| **Olay** | **Açıklama** |
| --- | --- |
| PageIndexChanged | Çağrı cihazı düğmeleri birini tıklatıldığında, ancak disk belleği işlemi GridView denetim işleme sonra oluşur. Kullanıcı denetimi farklı bir sayfasına gider sonra bir görev gerçekleştirmeniz gerektiğinde, bu olay sık kullanılır. |
| PageIndexChanging | Çağrı cihazı düğmeleri birini tıklandığında, ancak önce GridView, disk belleği işlemi denetim işleme oluşur. Bu olay, genellikle disk belleği işlemi iptal etmek için kullanılır. |
| RowCancelingEdit | Bir sıranın iptal düğmesine tıklandığında, ancak önce GridView denetimini çıkar düzenleme modunda olduğunda oluşur. Bu olay sık sık iptal etme işlemi durdurmak için kullanılır. |
| RowCommand | GridView denetiminde bir düğme tıklatıldığında gerçekleşir. Bu olay sık sık denetimde bir düğme tıklatıldığında bir görevi gerçekleştirmek için kullanılır. |
| RowCreated | Yeni bir satır GridView denetiminde oluşturulduğunda gerçekleşir. Bu olay sık sık satır oluşturulduğunda bir satır içeriğini değiştirmek için kullanılır. |
| RowDataBound | Bir veri satırı GridView denetiminde veri noktasına bağlı olduğunda oluşur. Bu olay, genellikle veri satır bağlandığında bir satır içeriğini değiştirmek için kullanılır. |
| RowDeleted | Bir sıranın Sil düğmesine tıklandığında, ancak GridView denetim kaydı veri kaynağından sildikten sonra oluşur. Bu olay sık sık silme işlemi sonuçlarını denetlemek için kullanılır. |
| RowDeleting | Bir sıranın Sil düğmesine tıklandığında, ancak önce GridView denetim kaydı veri kaynağından siler oluşur. Bu olay sık sık silme işlemini iptal etmek için kullanılır. |
| RowEditing | Bir sıranın Düzenle düğmesine tıklandığında, ancak önce GridView denetiminin düzenleme modunu girdiği oluşur. Bu olay sık sık düzenleme işlemi iptal etmek için kullanılır. |
| RowUpdated | Bir sıranın güncelleştirme düğmesine tıklandığında, ancak satır GridView denetiminin güncelleştirildikten sonra oluşur. Bu olay sık sık güncelleştirme işlemi sonuçlarını denetlemek için kullanılır. |
| RowUpdating | Bir sıranın güncelleştirme düğmesine tıklandığında, ancak önce GridView denetimini satır güncelleştirir oluşur. Bu olay sık sık güncelleştirme işlemini iptal etmek için kullanılır. |
| SelectedIndexChanged | Bir sıranın Seç düğmesine tıklandığında, ancak select işlemi GridView denetim işleme sonra oluşur. Bu olay sık sık denetimde bir satır seçtikten sonra bir görevi gerçekleştirmek için kullanılır. |
| SelectedIndexChanging | Bir sıranın Seç düğmesine tıklandığında, ancak önce GridView select işlemi denetim işleme oluşur. Bu olay sık sık seçme işlemini iptal etmek için kullanılır. |
| Sıralanmış | Bir sütunu sıralamak için köprü tıklatıldığında, ancak sıralama işlemi GridView denetim işleme sonra oluşur. Bu olay, genellikle kullanıcı bir sütunu sıralamak için bir köprüyü tıklattıktan sonra bir görevi gerçekleştirmek için kullanılır. |
| Sıralama | Bir sütunu sıralamak için köprü tıklandığında, ancak önce GridView sıralama işlemi denetim işleme oluşur. Bu olay, genellikle sıralama işlemi iptal etmek veya özel bir sıralama yordamı gerçekleştirmek için kullanılır. |

## <a name="formview"></a>FormView

FormView denetimi, bir veri kaynağından tek bir kaydını görüntülemek için kullanılır. Kullanıcı tanımlı şablonları satır alanları yerine görüntüler dışında denetimini, benzer. Kendi şablonlarınızı oluşturulması verilerin nasıl görüntüleneceğini denetleme daha fazla esneklik sunar. FormView denetimi aşağıdaki özellikleri destekler:

- Veri kaynağı denetimleri SqlDataSource ve ObjectDataSource gibi bağlama.
- Yerleşik ekleme özellikleri.
- Yerleşik güncelleştirme ve yetenekleri siliniyor.
- Yerleşik disk belleği özellikleri.
- Dinamik olarak özelliklerini ayarlamak için FormView nesne modeline programlı erişim olayları işlemek ve benzeri.
- Kullanıcı tanımlı şablonları, temalar ve stiller özelleştirilebilir görünümü.

## <a name="templates"></a>Şablonlar

FormView denetimi içeriği görüntülemek için denetim farklı parçalarının şablonları oluşturmanız gerekir. Çoğu şablon isteğe bağlıdır; Ancak, Denetim yapılandırıldığı modu için bir şablon oluşturmanız gerekir. Örneğin, ekleme kayıtları destekleyen FormView denetim tanımlı bir INSERT öğe şablonu olması gerekir. Aşağıdaki tabloda oluşturabileceğiniz farklı şablonları listeler.

| **Şablon türü** | **Açıklama** |
| --- | --- |
| EditItemTemplate | FormView denetimini düzenleme modunda olduğunda veri satırı içeriğini tanımlar. Bu şablon, giriş denetimlerini ve komut düğmeleri ile kullanıcı varolan bir kaydı düzenleyebileceğiniz genellikle içerir. |
| EmptyDataTemplate'i | FormView denetimini herhangi bir kayıt içermeyen bir veri kaynağına bağlandığında görüntülenen boş veri satırı içeriğini tanımlar. Bu şablon, genellikle veri kaynağında herhangi bir kayıt yok kullanıcıyı uyarmak için içerik içerir. |
| FooterTemplate | Altbilgi satırı içeriğini tanımlar. Bu şablon, genellikle altbilgi satırında görüntülemek istediğiniz herhangi bir ek içerik içerir. Alternatif olarak, yalnızca FooterText özelliği ayarlanarak altbilgi satırında görüntülenecek metin belirtebilirsiniz. |
| HeaderTemplate | Üstbilgi satırındaki içeriğini tanımlar. Bu şablon, genellikle başlık satırındaki görüntülemek istediğiniz herhangi bir ek içerik içerir. Alternatif olarak, yalnızca HeaderText özelliği ayarlanarak başlık satırındaki görüntülenecek metin belirtebilirsiniz. |
| ItemTemplate | FormView denetimini salt okunur modunda olduğunda veri satırı içeriğini tanımlar. Bu şablon, varolan bir kayıt değerlerini görüntülemek için içeriği genellikle içerir. |
| InsertItemTemplate | FormView denetim ekleme modunda olduğunda veri satırı içeriğini tanımlar. Bu şablon, giriş denetimlerini ve kullanıcının yeni bir kayıt ekleyebilmeniz için komut düğmeleri genellikle içerir. |
| PagerTemplate | Disk belleği özelliği etkinleştirilmişse, görüntülenen çağrı cihazı satır içeriğini tanımlar (AllowPagingözelliðini ayarlandığında **true**). Bu şablon, genellikle başka bir kayıtla gidebilirsiniz kullanıcı ile denetimleri içerir. |

Giriş denetimlerini düzenleme öğe şablonu ve INSERT öğesi şablonunda bir iki yönlü bağlama deyimi kullanarak bir veri kaynağı alanlarına bağlanabilir. Bu otomatik olarak bir güncelleştirme için giriş denetiminin değerlerini ayıklayın veya işlemi eklemek FormView denetim sağlar. İki yönlü bağlama ifadeleri otomatik olarak özgün alan değerlerini görüntülemek için bir düzen öğesi şablonunda giriş denetimleri de olanak sağlar.

### <a name="binding-to-data"></a>Verilere Bağlama

FormView denetimini bir veri kaynağı denetimine bağlı olabilir (gibi **SqlDataSource**, AccessDataSource, **ObjectDataSource** vb.), veya uygulayan herhangi bir veri kaynağı System.Collections.IEnumerable arabirimi (örneğin, System.Data.DataView, System.Collections.ArrayList ve System.Collections.Hashtable). FormView denetimi için uygun veri kaynağı türü bağlamak için aşağıdaki yöntemlerden birini kullanın:

- Bir veri kaynağı denetimi bağlamak için veri kaynağı denetiminin kimliği değerine FormView denetiminin DataSourceID özelliğini ayarlayın. FormView denetimini otomatik olarak belirtilen veri kaynağı denetime bağlar ve veri kaynağı ekleme, güncelleştirme, silme ve disk belleği işlevleri gerçekleştirmek için denetimin özelliklerinden yararlanabilir. Bu veri bağlamak için tercih edilen yöntemdir.
- Uygulayan bir veri kaynağına bağlanmak için **System.Collections.IEnumerable** arabirimi, program aracılığıyla FormView denetiminin DataSource özelliği veri kaynağı olarak ayarlanmış ve DataBind yöntemini çağırın. Bu yöntemi kullanırken, FormView denetimini yerleşik ekleme, güncelleştirme, silme ve disk belleği işlevleri sağlamaz. Bu işlev uygun olay kullanarak sağlamanız gerekir.

## <a name="data-operations"></a>Veri İşlemleri

FormView denetimi Güncelleştir, Sil, Ekle ve denetimindeki öğeleri aracılığıyla sayfasında kullanıcı çok sayıda yerleşik özellikleri sunar. FormView denetimi veri kaynağı denetime bağlı olduğunda FormView denetimini denetimin özellikleri veri kaynağı yararlanmak ve otomatik güncelleştirme, silme, ekleme ve işlevsellik disk belleği sağlayın. FormView denetimini güncelleştirme, silme, Ekle ve diğer veri kaynağı türleri ile sayfalandırma işlemleri için destek sağlayabilirsiniz; Ancak, bu işlemlerin uygulamasını ile ilgili olay işleyicisi sağlamanız gerekir.

FormView denetim şablonları kullandığından, güncelleştirme, silme veya ekleme işlemleri gerçekleştirmek için komut düğmeleri otomatik olarak oluşturmak için bir yol sağlamaz. Bu komut düğmeleri uygun şablonunda el ile eklemeniz gerekir. FormView denetimini olan belirli düğmeler kendi **CommandName** özellikleri belirli değerlere ayarlayın. Aşağıdaki tabloda FormView denetimini tanıdığı komut düğmeleri listeler.

| **Düğmesi** | **CommandName değeri** | **Açıklama** |
| --- | --- | --- |
| İptal | "İptal" | Güncelleştirme veya işlemleri ekleme işlemi iptal etmek ve kullanıcı tarafından girilen değerler atmak için kullanılır. FormView denetim sonra DefaultMode özelliği tarafından belirtilen moduna döner. |
| Sil | "Sil" | Operations silinmesi veri kaynağından görüntülenen kaydını silmek için kullanılır. ItemDeleting ve ItemDeleted olayları başlatır. |
| Düzenle | "Düzenle" | FormView denetimini düzenleme moduna için işlemleri güncelleştirilirken kullanılır. Belirtilen içerik **EditItemTemplate** özelliği için veri satırı görüntülenir. |
| Ekleme | "Ekle" | İşlem ekleme, kullanıcı tarafından sağlanan değerleri kullanarak veri kaynağının yeni bir kayıt eklemeye çalışırsanız için kullanılır. ItemInserting ve ItemInserted olayları başlatır. |
| Yeni | "Yeni" | İşlem ekleme FormView denetim ekleme modunda yerleştirmek için kullanılır. Belirtilen içerik **InsertItemTemplate** özelliği için veri satırı görüntülenir. |
| Sayfa | "Sayfa" | Disk belleği gerçekleştirir çağrı cihazı satırda bir düğmeyi temsil etmesi için sayfalandırma işlemleri kullanılır. Disk belleği işlemi belirtmek için ayarlayın **CommandArgument** "İleri", "Önceki", "İlk", "Son" veya hangi gitmek sayfanın dizini düğmesinin özelliği. PageIndexChanging ve PageIndexChanged olayları başlatır. |
| Güncelleştirme | "Güncelleştir" | Kullanıcı tarafından sağlanan değerlerle veri kaynağındaki görüntülenen kayıt güncelleştirmeyi denemek için işlemleri güncelleştirilirken kullanılır. ItemUpdating ve ItemUpdated olayları başlatır. |

Delete aksine button (görüntülenen kaydı hemen silen), düzenleme veya yeni düğmesine tıklandığında, Düzen denetim geçer FormView veya modu sırasıyla ekleyin. Düzenleme modunda yer alan içeriği **EditItemTemplate** özelliği için geçerli veri öğesi görüntülenir. Genellikle, sağlayacak şekilde Düzenle düğmesini iptal düğmesi ve bir güncelleştirme ile değiştirilir düzenleme öğe şablonu tanımlanır. Alanın veri türünü (örneğin, bir metin kutusu veya bir onay kutusu denetimi) uygun olan giriş denetimleri de genellikle bir alanın değerini değiştirmek kullanıcı için birlikte görüntülenir. Değişiklikleri iptal düğmesine tıkladığınızda bir kenara bırakır sırada Güncelleştir düğmesini tıklatarak veri kaynağı kaydı güncelleştirir.

Benzer şekilde, içinde yer alan içeriği **InsertItemTemplate** özelliği denetim ekleme modunda olduğunda veri öğesi için görüntülenir. INSERT öğe şablonu, genellikle yeni düğmesini iptal düğmesi ekleme ile değiştirilir ve boş giriş denetimleri için yeni bir kayıt değerleri girmek kullanıcı için görüntülenen şekilde tanımlanır. Değişiklikleri iptal düğmesine tıkladığınızda bir kenara bırakır sırada için Ekle düğmesini kaydı veri kaynağında ekler.

FormView denetimi veri kaynağındaki diğer kayıtlarını gidin olanak tanır. bir disk belleği özelliği sunar. Etkinleştirildiğinde, çağrı cihazı satır sayfa gezinti denetimlerinin içeren FormView denetiminde görüntülenir. Disk belleği etkinleştirmek için ayarlanmış **AllowPaging** özelliğine **doğru**. Çağrı cihazı satır PagerStyle içerdiği nesnelerin özelliklerini ve PagerSettings özelliğini ayarlayarak özelleştirebilirsiniz. Yerleşik çağrı cihazı satır UI kullanmak yerine, kendi kullanıcı arabirimini kullanarak oluşturabileceğiniz **PagerTemplate** özelliği.

## <a name="customizing-the-user-interface"></a>Kullanıcı Arabirimini Özelleştirme

Denetimin farklı bölümleri için stil özellikleri ayarlayarak FormView denetiminin görünümünü özelleştirebilirsiniz. Aşağıdaki tabloda farklı stil özellikleri listeler.

| **Stil özelliği** | **Açıklama** |
| --- | --- |
| EditRowStyle | FormView denetimini olduğunda veri satırı için stil ayarlarını düzenleme modunda. |
| EmptyDataRowStyle | Veri kaynağı herhangi bir kayıt içermediğinde FormView denetiminde gösterilen boş veri satırı için stil ayarları. |
| FooterStyle | FormView denetimini altbilgi satırının stilini ayarlar. |
| HeaderStyle | FormView denetimini başlık satırının stilini ayarlar. |
| InsertRowStyle | FormView denetimini olduğunda veri satırı için stil ayarlarını modu ekleyin. |
| PagerStyle | Disk belleği özelliği etkinleştirilmişse, FormView denetiminde gösterilen çağrı cihazı satır stili ayarları. |
| RowStyle | FormView denetimini salt okunur modunda olduğunda veri satırı için stil ayarları. |

## <a name="events"></a>Olaylar

FormView denetimini karşı program çeşitli olayları sağlar. Bu, her bir olay gerçekleştiğinde bir özel yordam çalıştırmanızı sağlar. Aşağıdaki tabloda FormView denetimi tarafından desteklenen olayları listeler.

| **Olay** | **Açıklama** |
| --- | --- |
| ItemCommand | FormView denetimi içinde bir düğme tıklatıldığında gerçekleşir. Bu olay sık sık denetimde bir düğme tıklatıldığında bir görevi gerçekleştirmek için kullanılır. |
| ItemCreated | Tüm FormViewRow nesneleri FormView denetiminde oluşturulduktan sonra oluşur. Bu olay sık sık görüntülenmeden önce bir kayıt değerlerini değiştirmek için kullanılır. |
| ItemDeleted | Sil düğmesini oluşur (bir düğme kendi **CommandName** özelliği "Sil" olarak ayarlanmış) tıklandığında, ancak FormView denetim kaydı veri kaynağından siler. Bu olay sık sık silme işlemi sonuçlarını denetlemek için kullanılır. |
| ItemDeleting | Sil düğmesine tıklandığında, ancak önce FormView denetim kaydı veri kaynağından siler oluşur. Bu olay sık sık silme işlemini iptal etmek için kullanılır. |
| ItemInserted | Ekle düğmesi oluşur (bir düğme kendi **CommandName** özelliği "Ekle" olarak ayarlanmış) tıklandığında, ancak kayıt FormView denetimini ekler. Bu olay sık sık ekleme işleminin sonuçlarını denetlemek için kullanılır. |
| ItemInserting | Ekle düğmesine tıklandığında, ancak önce FormView kaydı denetimi ekler oluşur. Bu olay sık sık ekleme işlemini iptal etmek için kullanılır. |
| ItemUpdated | Bir güncelleştirme düğmesi oluşur (bir düğme kendi **CommandName** özelliği "Güncelleştir" olarak ayarlanmış) tıklandığında, ancak FormView denetimini satır güncelleştirildiğinde. Bu olay sık sık güncelleştirme işlemi sonuçlarını denetlemek için kullanılır. |
| ItemUpdating | Bir güncelleştirme düğmesine tıklandığında, ancak önce FormView denetim kaydı güncelleştirir oluşur. Bu olay sık sık güncelleştirme işlemi iptal etmek için kullanılır. |
| ModeChanged | FormView denetimini modları değişiklikler sonra oluşur (için düzenleme, ekleme veya salt okunur modda). Bu olay sık sık FormView denetimini modları değiştiğinde bir görevi gerçekleştirmek için kullanılır. |
| ModeChanging | FormView denetimini modları değiştirmeden önce oluşur (için düzenleme, ekleme veya salt okunur modda). Bu olay, genellikle bir modu değişikliği iptal etmek için kullanılır. |
| PageIndexChanged | Çağrı cihazı düğmeleri birini tıklatıldığında, ancak disk belleği işlemi FormView denetim işleme sonra oluşur. Kullanıcı denetimi farklı bir kayıtta gider sonra bir görev gerçekleştirmeniz gerektiğinde, bu olay sık kullanılır. |
| PageIndexChanging | Çağrı cihazı düğmeleri birini tıklandığında, ancak önce FormView disk belleği işlemi denetim işleme oluşur. Bu olay, genellikle disk belleği işlemi iptal etmek için kullanılır. |

## <a name="detailsview"></a>DetailsView

DetailsView denetimi, her bir alan kaydının tablonun satırında görüntülendiği tek kayıtlı bir veri kaynağından bir tabloda görüntülemek için kullanılır. Bu bir GridView denetimi ile birlikte ana-ayrıntı senaryoları için kullanılabilir. DetailsView denetimi aşağıdaki özellikleri destekler:

- Veri kaynağı denetimleri SqlDataSource gibi bağlama.
- Yerleşik ekleme özellikleri.
- Yerleşik güncelleştirme ve yetenekleri siliniyor.
- Yerleşik disk belleği özellikleri.
- Dinamik olarak özelliklerini ayarlamak için DetailsView nesne modeline programlı erişim olayları işlemek ve benzeri.
- Temalar ve stiller aracılığıyla özelleştirilebilir görünümü.

## <a name="row-fields"></a>Satır Alanları

Her veri satırı DetailsView denetimindeki bir alan denetimi bildirerek oluşturulur. Farklı satır alan türleri denetimindeki satırları davranışını belirler. Alan denetimleri DataControlField türetilir. Aşağıdaki tabloda kullanılabilir farklı satır alan türleri listelenmektedir.

| **Sütun alan türü** | **Açıklama** |
| --- | --- |
| BoundField | Bir veri kaynağı, bir alanın değerini metin olarak görüntüler. |
| ButtonField | Komut düğmesi DetailsView denetiminde görüntüler. Bu bir Ekle veya Kaldır düğmesi gibi bir özel düğme denetimi ile bir satır görüntülemenize olanak sağlar. |
| CheckBoxField | Bir onay kutusu DetailsView denetiminde görüntüler. Bu satır alan türü genellikle bir Boole değeri içeren alanlar görüntülemek için kullanılır. |
| CommandField | Yerleşik komutu görüntüler düzenleme, gerçekleştirmek için düğmeler ekleme veya silme işlemleri DetailsView denetimi. |
| HyperLinkField | Bir alanın değeri bir veri kaynağında bir köprü olarak görüntüler. Bu satır alan türü köprü URL'sine ikinci bir alana bağlamak sağlar. |
| ImageField | Bir görüntü DetailsView denetiminde görüntüler. |
| TemplateField | Görüntüler kullanıcı tanımlı içerik belirtilen bir şablonu göre DetailsView denetimindeki bir satır için. Bu satır alan türü, özel satır alanı oluşturmanıza olanak sağlar. |

Varsayılan olarak, AutoGenerateRows özelliğini ayarlamak **doğru**, hangi otomatik olarak oluşturur bağlanabilirse türünün her alanı için bir ilişkili satır alanı nesnesi veri kaynağında. Geçerli bağlanabilirse String, DateTime, ondalık, GUID ve ilkel türler kümesi türleridir. Her bir alan her bir alan veri kaynağında görüntülendiği sırada metin olarak bir satır sonra görüntülenir.

Satırları otomatik olarak üretmek kayıttaki her alan görüntülemek için hızlı ve kolay bir yol sağlar. Ancak DetailsView kullanmak için denetimin DetailsView denetiminde eklenecek satır alanları açıkça bildirmelidir Gelişmiş özellikleri. Satır alanları bildirmek için önce ayarlayın **AutoGenerateRows** özelliğine **false**. Ardından, açma ve kapama eklemek  **&lt;alanları&gt;**  açma ve kapatma etiketleri DetailsView denetiminin arasındaki etiketler. Son olarak, açma ve kapatma arasında dahil etmek istediğiniz satır alanları listesinde  **&lt;alanları&gt;**  etiketler. Belirtilen satır alanları alanlar koleksiyonu listelenen sırayla eklenir. **Alanları** koleksiyonu DetailsView denetimi satır alanlarında programlı olarak yönetmenizi sağlar.

> [!NOTE]
> Alanlar alanlar koleksiyonu eklenmez satır otomatik olarak oluşturulur.


## <a name="binding-to-data"></a>Verilere Bağlama

DetailsView denetimi bir veri kaynağı denetimi gibi bağlanabilir **SqlDataSource** veya AccessDataSource, veya System.Data.DataView gibi System.Collections.IEnumerable arabirimini uygulayan herhangi bir veri kaynağı System.Collections.ArrayList ve System.Collections.Hashtable.

Uygun bir veri kaynağı türüne DetailsView denetimi bağlamak için aşağıdaki yöntemlerden birini kullanın:

- Bir veri kaynağı denetimi bağlamak için veri kaynağı denetiminin kimliği değerine DetailsView denetiminin DataSourceID özelliğini ayarlayın. DetailsView denetimi için belirtilen veri kaynağı denetimi otomatik olarak bağlar. Bu veri bağlamak için tercih edilen yöntemdir.
- Uygulayan bir veri kaynağına bağlanmak için **System.Collections.IEnumerable** arabirimi, program aracılığıyla DetailsView denetiminin DataSource özelliği veri kaynağı olarak ayarlanmış ve DataBind yöntemini çağırın.

## <a name="security"></a>Güvenlik

Bu denetim, kötü amaçlı istemci komut dosyası içerebilir kullanıcı girişi görüntülemek için kullanılabilir. Bir istemciden yürütülebilir komut dosyası, SQL deyimlerini veya başka bir kod için uygulamanızda görüntülemeden önce gönderilen bilgilere bakın. ASP.NET bir giriş istek doğrulama özelliği bloğu komut dosyası ve HTML kullanıcı girişi sağlar.

## <a name="data-operations"></a>Veri İşlemleri

DetailsView denetimi Güncelleştir, Sil, Ekle ve denetimindeki öğeleri aracılığıyla sayfasında kullanıcı yerleşik özellikleri sağlar. DetailsView denetimi veri kaynağı denetime bağlı olduğunda DetailsView denetimi denetimin özellikleri veri kaynağı yararlanmak ve otomatik güncelleştirme, silme, ekleme ve işlevsellik disk belleği sağlayın.

DetailsView denetimi, güncelleştirme, silme, Ekle ve diğer veri kaynağı türleri ile sayfalandırma işlemleri için destek sağlayabilirsiniz; Ancak, bu işlemlerin uygun olay işleyici içinde uygulamasını sağlamanız gerekir.

DetailsView denetimi otomatik olarak ekleyebilir bir **CommandField** AutoGenerateEditButton,AutoGenerateDeleteButtonveyaAutoGenerateInsertButtonözelliklerineayarlayarakdüzenleme,silmeveyayenibirdüğmesatıralanıyla**true**sırasıyla. Delete aksine düğmesini (Seçili kaydı hemen silen), düzenleme veya yeni düğmesine tıklandığında, Düzen denetim geçer DetailsView veya modu, sırasıyla ekleyin. Düzenleme modunda Düzenle düğmesini iptal düğmesi ve bir güncelleştirme ile değiştirilir. Alanın veri türünü (örneğin, bir metin kutusu veya bir onay kutusu denetimi) uygun olan giriş denetimlerini, bir alanın değerini değiştirmek kullanıcı için birlikte görüntülenir. Değişiklikleri iptal düğmesine tıkladığınızda bir kenara bırakır sırada Güncelleştir düğmesini tıklatarak veri kaynağı kaydı güncelleştirir. Benzer şekilde, INSERT modunda yeni düğmesini iptal düğmesi ekleme ile değiştirilir ve boş giriş denetimlerini kullanıcının için yeni bir kayıt değerleri girmek görüntülenir.

DetailsView denetimi veri kaynağındaki diğer kayıtlarını gidin olanak tanır. bir disk belleği özelliği sunar. Etkinleştirildiğinde, sayfa gezinti denetimlerinin bir çağrı cihazı satırda görüntülenir. Disk belleği etkinleştirmek için AllowPagingözelliðini ayarlamak **doğru**. Çağrı cihazı satır PagerStyle ve PagerSettings özellikleri kullanılarak özelleştirilebilir.

## <a name="customizing-the-user-interface"></a>Kullanıcı Arabirimini Özelleştirme

Denetimin farklı bölümleri için stil özellikleri ayarlayarak DetailsView denetiminin görünümünü özelleştirebilirsiniz. Aşağıdaki tabloda farklı stil özellikleri listeler.

| **Stil özelliği** | **Açıklama** |
| --- | --- |
| AlternatingRowStyle | DetailsView denetimi değişen verileri satırları stil ayarları. Bu özellik ayarladığınızda, veri satırına RowStyle ayarları arasında değişen görüntülenir ve **AlternatingRowStyle** ayarlar. |
| CommandRowStyle | DetailsView denetiminde yerleşik komut düğmeleri içeren satırı stilini ayarlar. |
| EditRowStyle | DetailsView denetimi olduğunda veri satırlar için stil ayarlarını düzenleme modunda. |
| EmptyDataRowStyle | Veri kaynağı herhangi bir kayıt içermediğinde DetailsView denetiminde gösterilen boş veri satırı için stil ayarları. |
| FooterStyle | DetailsView denetimi altbilgi satırının stilini ayarlar. |
| HeaderStyle | DetailsView denetimi üstbilgi satırının stilini ayarlar. |
| InsertRowStyle | DetailsView denetimi olduğunda veri satırlar için stil ayarlarını modu ekleyin. |
| PagerStyle | DetailsView denetiminin çağrı cihazı satırının stilini ayarlar. |
| RowStyle | DetailsView denetimindeki veri satırları stilini ayarlar. Zaman **AlternatingRowStyle** özelliğini de ayarlayın, veri satırı arasında değişen görüntülenen **RowStyle** ayarları ve **AlternatingRowStyle** ayarlar. |
| FieldHeaderStyle | DetailsView denetiminin üstbilgi sütun için stilini ayarlar. |

## <a name="events"></a>Olaylar

DetailsView denetimi karşı program çeşitli olayları sağlar. Bu, her bir olay gerçekleştiğinde bir özel yordam çalıştırmanızı sağlar. Aşağıdaki tabloda DetailsView denetimi tarafından desteklenen olayları listeler. DetailsView denetimini, bu olaylar ayrıca temel sınıflardan devralır.: veri bağlama, veriye bağlı, Disposed, başlatma, yük, PreRender ve işleme.

| **Olay** | **Açıklama** |
| --- | --- |
| ItemCommand | DetailsView denetiminde bir düğme tıklatıldığında gerçekleşir. |
| ItemCreated | Tüm DetailsViewRow nesneleri DetailsView denetiminde oluşturulduktan sonra oluşur. Bu olay sık sık görüntülenmeden önce bir kayıt değerlerini değiştirmek için kullanılır. |
| ItemDeleted | Sil düğmesine tıklandığında, ancak DetailsView denetimi kaydı veri kaynağından sildikten sonra oluşur. Bu olay sık sık silme işlemi sonuçlarını denetlemek için kullanılır. |
| ItemDeleting | Sil düğmesine tıklandığında, ancak önce DetailsView denetim kaydı veri kaynağından siler oluşur. Bu olay sık sık silme işlemini iptal etmek için kullanılır. |
| ItemInserted | Ekle düğmesine tıklandığında, ancak DetailsView denetimi kaydı ekledikten sonra oluşur. Bu olay sık sık ekleme işleminin sonuçlarını denetlemek için kullanılır. |
| ItemInserting | Ekle düğmesine tıklandığında, ancak önce DetailsView kaydı denetimi ekler oluşur. Bu olay sık sık ekleme işlemini iptal etmek için kullanılır. |
| ItemUpdated | Bir güncelleştirme düğmesine tıklandığında, ancak satır DetailsView denetimi güncelleştirildikten sonra oluşur. Bu olay sık sık güncelleştirme işlemi sonuçlarını denetlemek için kullanılır. |
| ItemUpdating | Bir güncelleştirme düğmesine tıklandığında, ancak önce DetailsView denetim kaydı güncelleştirir oluşur. Bu olay sık sık güncelleştirme işlemi iptal etmek için kullanılır. |
| ModeChanged | DetailsView denetimi modları (düzenleme, ekleme veya salt okunur modda) değiştirdikten sonra gerçekleşir. Bu olay sık sık DetailsView denetimi modları değiştiğinde bir görevi gerçekleştirmek için kullanılır. |
| ModeChanging | DetailsView denetimi modları (düzenleme, ekleme veya salt okunur modda) değiştirmeden önce gerçekleşir. Bu olay, genellikle bir modu değişikliği iptal etmek için kullanılır. |
| PageIndexChanged | Çağrı cihazı düğmeleri birini tıklatıldığında, ancak disk belleği işlemi DetailsView denetimini işler sonra oluşur. Kullanıcı denetimi farklı bir kayıtta gider sonra bir görev gerçekleştirmeniz gerektiğinde, bu olay sık kullanılır. |
| PageIndexChanging | Çağrı cihazı düğmeleri birini tıklandığında, ancak önce DetailsView disk belleği işlemi denetim işleme oluşur. Bu olay, genellikle disk belleği işlemi iptal etmek için kullanılır. |

## <a name="the-menu-control"></a>Menü denetimi

ASP.NET 2.0 menü denetiminde tam özellikli gezinti sistemi olacak şekilde tasarlanmıştır. SiteMapDataSource gibi hiyerarşik veri kaynaklarına kolayca veriye bağlı olabilir.

Menü denetimleri yapısı bildirimli olarak veya dinamik olarak tanımlanabilir ve tek bir kök düğümde ve herhangi bir sayıda alt düğümleri oluşur. Aşağıdaki kod menü denetim için bir menü bildirimli olarak tanımlar.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Yukarıdaki örnekte, kök düğümü Home.aspx düğümdür. Farklı düzeylerde kök düğümü içindeki tüm diğer düğümlere yerleştirilir.

Menü denetimi işleyebilen menüleri iki tür vardır; statik menüleri ve dinamik menüler. Statik menüleri her zaman görünür menü öğelerinin oluşur. Yalnızca kullanıcı üzerlerine fareyle üzerine geldiğinde görünür olan menü öğelerinin dinamik menüler oluşur. Müşteriler genellikle bildirimli olarak tanımlanan menülerle statik menüleri ve menülerle çalışma zamanında veriye bağlı olan dinamik menüler karışıklığa neden olabilir. Aslında, dinamik ve statik menüleri popülasyon yönteme ilgisiz. Koşulları *statik* ve *dinamik* yalnızca desteklemediğini menü statik varsayılan olarak görüntülenen veya yalnızca için kullanıcı bazı eylemde bulunduğunda görüntülenen bakın.

**StaticDisplayLevels** özelliği menü kaç düzeyde statik ve bu nedenle görüntülenen varsayılan olarak yapılandırmak için kullanılır. Yukarıdaki örnekte ayarı **StaticDisplayLevels** özelliği için 2 değerini giriş düğümü, müzik düğümü ve filmler düğümü statik olarak görüntülemek için menüsünden neden. Kullanıcının üst düğümün üzerine geldiğinde diğer tüm düğümlere dinamik olarak görüntülenir.

**MaximumDynamicDisplayLevels** özelliği yapılandırır en fazla dinamik düzeylerini menü görüntüleyebilen. Dinamik menüler tarafından belirtilen değerden daha yüksek bir düzeyde **MaximumDynamicDisplayLevels** özelliği atılır.

> [!NOTE]
> Bu durumlarda menüleri nedeniyle MaximumDynamicDisplayLevels özelliği işlemek için burada görünmüyor karşılaşabileceğiniz neredeyse kesin olur. Bu gibi durumlarda özelliği müşterilerin menüleri görüntü için izin vermek için yeterince ayarlandığından emin olun.


## <a name="data-binding-the-menu-control"></a>Veri Menü denetimine bağlama

Menü denetimi SiteMapDataSource veya XmlDataSource'ta gibi tüm hiyerarşik veri kaynağına bağlanabilir. SiteMapDataSource en yaygın olarak yöntemi Menü denetimine veri bağlama için dışına birtakım dosya akışları ve şemasına menü denetimi için bilinen bir API sağlar çünkü kullanılır. Aşağıda kod basit birtakım dosya gösterir.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Yalnızca bir kök siteMapNode öğesi, bu durumda, giriş öğesi olduğuna dikkat edin. Çeşitli öznitelikler için her siteMapNode yapılandırılabilir. En yaygın olarak kullanılan öznitelikler şunlardır:

- **URL** kullanıcı menü öğesini tıklattığında görüntülenecek URL'yi belirtir. Bu öznitelik yoksa, düğüm yalnızca geri tıklatıldığında gönderecek.
- **Başlık** menü öğesini görüntülenen metni belirtir.
- **Açıklama** belgelemek düğüm için kullanılır. Fare düğüm üzerinde üzerine getirildiği Ayrıca araç ipucu olarak görüntüler.
- **siteMapFile** için iç içe geçmiş haritaları sağlar. Bu öznitelik, doğru biçimlendirilmiş bir ASP.NET site haritası dosyasını işaret etmelidir.
- **rolleri** ASP.NET güvenlik kırpma tarafından denetlenmesi için bir düğümün görünümünü sağlar.

Bu öznitelikler tüm isteğe bağlı olsa da, menüsünün davranışını değil belirtilirse gerekenin olabileceğini unutmayın. Örneğin, varsa *url* özniteliği belirtildi ancak *açıklama* öznitelik değil, düğüm görünür olmaz ve belirtilen URL'ye mümkün olacaktır.

## <a name="controlling-a-menus-operation"></a>Menüleri işlemi denetleme

Bir ASP.NET menü denetimi çalışmasını etkileyen çeşitli özellikler vardır; **yönlendirmesini** özelliği, **DisappearAfter** özelliği, **StaticItemFormatString** özelliği ve **StaticPopoutImageUrl**özelliği olan birkaçı bu.

- **Yönlendirmesini** olarak ayarlanabilir *yatay* veya *dikey* ve statik menü öğelerini yatay bir satır veya dikey olarak düzenlendiği ve üzerine yığılmış olup olmadığını denetler bir diğer. Bu özellik, dinamik menüler etkilemez.
- **DisappearAfter** özelliği yapılandırır fare sonra ne kadar süreyle Dinamik menü görünür kalacağı dosyayı ayrılmayın. Değer milisaniye ve 500 varsayılan değeri belirtildi. Bu özelliği -1 değeri ayarlamak menüsünün hiçbir zaman otomatik olarak kaybolmasına neden olur. Bu durumda, kullanıcı menüsünün dışında tıklattığında menüsü yalnızca kaybolur.
- **StaticItemFormatString** özellik tutarlı duyuruları menü sisteminizi korumak kolaylaştırır. Bu özellik belirtirken *{0}* veri kaynağında görüntülenen açıklama yerine girilmesi gerekir. Örneğin, menü öğesinden alıştırma 1 say Mız ürün sayfasını ziyaret edin, vb. olması için bizim {0} StaticItemFormatString sayfasını ziyaret edin belirtmeniz gerekir. Çalışma zamanında, ASP.NET, menü öğesi için doğru açıklamasıyla {0} olayı yerini alır.
- **StaticPopoutImageUrl** özelliği, belirli menüsü düğümünü üzerine gelerek erişilebilir alt düğümleri olduğunu belirtmek için kullanılan görüntü belirtir. Dinamik menüler varsayılan görüntü kullanmaya devam eder.

## <a name="templated-menu-controls"></a>Şablonlu menü denetimleri

Menü denetimi, bir şablonlu denetimdir ve için iki farklı öğe şablonları sağlar; StaticItemTemplate'i ve DynamicItemTemplate'i. Bu şablonları kullanarak, kolayca sunucu veya kullanıcı denetimleri, menülere ekleyebilirsiniz.

Visual Studio .NET şablonlarında düzenlemek için menüsünde akıllı etiket düğmesini tıklatın ve düzenleme şablonlarını seçin. Ardından, StaticItemTemplate'i veya DynamicItemTemplate'i düzenleme arasında seçebilirsiniz.

Sayfa yüklendiğinde StaticItemTemplate'e eklenen tüm denetimler statik menüsünde görüntülenir. DynamicItemTemplate'e eklenen tüm denetimler üzerinde tüm açılır menüler görünür.

## <a name="menu-events"></a>Menü olayları

Menü denetimi için benzersiz olan iki olay; yine de sahip istiyor musunuz? **MenuItemClicked** ve **MenuItemDatabound** olay.

Menü öğesi tıklatıldığında MenuItemClicked olay tetiklenir. Menü öğesi veriye bağlı MenuItemDatabound olay tetiklenir. **MenuEventArgs** , geçirilen olay işleyicisi öğesi özelliği aracılığıyla menü öğesine erişim sağlar.

## <a name="controlling-a-menus-appearance"></a>Menüleri görünümü denetleme

Ayrıca, bir veya daha fazla biçimi menülere kullanılabilir birçok stil kullanarak bir menü denetiminin görünümünü etkileyebilir. Bunlar arasında **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**ve **DynamicHoverStyle**. Bu özellikleri, standart bir HTML stil dize kullanılarak yapılandırılır. Örneğin, aşağıdaki dinamik menüler stili etkiler.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Vurgulu stilleri birini kullanıyorsanız, eklemeniz gerekir bir &lt;head&gt; öğesinde ile sayfaya *runat* öğesini ayarlamak *server*.


Menü denetimleri, ASP.NET 2.0 temaları kullanımını da destekler.

## <a name="the-treeview-control"></a>TreeView denetimi

TreeView denetimi bir ağaç benzeri yapıda verilerini görüntüler. Menü denetimiyle olduğu gibi bu veri SiteMapDataSource gibi tüm hiyerarşik veri kaynağına bağlı kolayca olabilir.

Müşteriler, ASP.NET 2.0 TreeView denetimi hakkında sorun büyük olasılıkla sorunun ilk olsun veya olmasın, ASP.NET için kullanılabilir TreeView IE WebControl ilişkili olduğu 1.x. Değil. ASP.NET 2.0 TreeView denetimi sıfırdan yazılmıştır ve IE TreeView önceden kullanılabilen WebControl önemli iyileştirme sunar.

Menü denetimi ile aynı şekilde gerçekleştirilir gibi bir TreeView denetimi için site haritası bağlamak nasıl ı ayrıntıya gitme olmaz. Ancak, TreeView denetimi çalışır yolunda farklı bazı farklılıklar vardır.

Varsayılan olarak, bir TreeView denetimi tam olarak genişletilmiş görüntülenir. İlk yükleme sırasında genişletme düzeyini değiştirmek için değiştirmek **ExpandDepth** denetiminin özelliği. Bu, özellikle TreeView belirli düğümler genişletilen bağlı veriye bağlı olduğu durumlarda önemlidir.

## <a name="databinding-the-treeview-control"></a>Veri bağlama TreeView denetimi

Menü denetimi TreeView kendisi de büyük miktarlarda veri işleme için uygundur. Bu nedenle, bir SiteMapDataSource veya XMLDataSource databinding yanı sıra TreeView genellikle bir veri kümesi veya diğer ilişkisel veri bağlı verilerdir. TreeView denetimi için büyük miktarlarda verinin nerede bağlı olduğu durumlarda, denetimi aslında görünür olan veri bağlamak en iyisidir. TreeView düğümler genişletilen gibi ek verilere veri bağlama sonra kullanabilirsiniz.

Bu durumda, **PopulateOnDemand** TreeView özelliği ayarlanmalıdır *doğru*. Ardından için bir uygulama sunmak amacıyla gerekir **TreeNodePopulate** yöntemi.

## <a name="data-binding-without-postback"></a>Veri bağlama geri gönderme

İlk olarak önceki örnek düğümünde genişlettiğinizde, sayfa geri gönderir ve yeniler dikkat edin. Thats bir sorun değildir bu örnek, ancak bunun büyük miktarda veri ile bir üretim ortamında olabilir düşünebilirsiniz. Daha iyi bir senaryo bir hangi TreeView hala dinamik olarak düğümlerinden doldurmak, ancak olmadan bir post sunucusuna geri olacaktır.

Ayarlayarak **PopulateNodesFromClient** ve **PopulateOnDemand** özelliklerini true, ASP.NET TreeView denetimi için dinamik olarak bir post geri olmadan düğümleri doldurmak. Üst düğüm genişletildiğinde istemci tarafından bir XMLHttp istekte ve OnTreeNodePopulate olay tetiklenir. Ardından verileri için kullanılan bir XML veri adası ile sunucu yanıt alt düğümler bağlayın.

ASP.NET dinamik bu işlevselliğini hayata Geçiren istemci kodu oluşturur. &lt;Betik&gt; komut dosyasını içeren etiketler, bir AXD dosyasına işaret eden oluşturulur. Örneğin, listenin altına XMLHttp istek oluşturur betik kodu için kod bağlantılarını gösterir.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Tarayıcınızda Yukarıda AXD dosyasını bulun ve açın XMLHttp isteği uygulayan kod görürsünüz. Bu yöntem, komut dosyasını değiştirerek müşteriler engeller.

## <a name="controlling-the-operation-of-the-treeview-control"></a>TreeView denetimi işlemi denetleme

TreeView denetimi denetimin çalışmasını etkileyen çeşitli özellikler vardır. En bariz Özellikler **ShowCheckBoxes**, **ShowExpandCollapse**, ve **ShowLines**.

**ShowCheckBoxes** özelliği düğümler işlendiğinde bir onay kutusu gösterir olup olmadığına bakılmaksızın etkiler. Bu özellik için geçerli değerler **hiçbiri**, **kök**, **üst**, **yaprak**, ve **tüm**. Bu TreeView denetimi şu şekilde etkiler:

| **Özellik değeri** | **Etkisi** |
| --- | --- |
| Yok. | Onay kutularını tüm düğümlerde görüntülenmez. Varsayılan ayar budur. |
| kök | Bir onay kutusu yalnızca kök düğümde görüntülenir. |
| Üst | Bir onay kutusu yalnızca alt düğümleri sahip düğümleri üzerinde görüntülenir. Alt düğümleri üst düğümleri veya yaprak düğümlerin olabilir. |
| Yaprak | Bir onay kutusu hiçbir alt düğümleri olan bu düğümlerde görüntülenir. |
| Tümü | Tüm düğümler üzerinde bir onay kutusu görüntülenir. |

Onay kutuları kullanılıyorsa **CheckedNodes** özelliği geri gönderme denetlenir TreeView düğümler topluluğunu döndürür.

**ShowExpandCollapse** özelliği, kök ve üst düğümleri yanındaki Genişlet/Daralt görüntünün görünümünü denetler. Bu özellik ayarlanmışsa **yanlış**, TreeView düğümleri köprü olarak işlenir ve bağlantısını tıklatarak genişletilmiş ve daraltılmış.

**ShowLines** özelliği denetler alt düğümler üst düğümlerini bağlayan satırları desteklemediğini görüntülenir. Zaman **false** (varsayılan), satır görüntülenir. Zaman **true**, TreeView denetimi tarafından belirtilen klasörde satırları görüntülerini kullanacak **LineImagesFolder** özelliği.

TreeView satırları görünümünü özelleştirmek için bir satır Designer araç Visual Studio .NET 2005 içerir. Bu aracı TreeView denetimi akıllı etiket düğmesini kullanarak erişebilirsiniz.


![](data-bound-controls/_static/image1.jpg)

**Şekil 1**


Seçtiğinizde **çizgi görüntülerini özelleştirme** menü seçeneği satır Tasarımcısı aracını TreeView satırları görünümünü yapılandırmanıza olanak sağlayan başlatılacak.

## <a name="treeview-events"></a>TreeView olayları

TreeView denetimi aşağıdaki benzersiz olaylar vardır:

- Bir düğümü seçildiğinde SelectedNodeChanged oluşur dayalı olarak **SelectAction** özelliği.
- TreeNodeCheckChanged düğümleri checkboxs durumu değiştiğinde oluşur.
- Bir düğüm genişletildiğinde TreeNodeExpanded oluşur dayalı olarak **SelectAction** özelliği.
- Bir düğüm daraltılmış TreeNodeCollapsed oluşur.
- Bir düğüm veri olduğunda TreeNodeDataBound gerçekleşir bağlı.
- Bir düğüm doldurulur TreeNodePopulate oluşur.

**SelectAction** özelliği, bir düğümü seçildiğinde hangi olay tetiklenir yapılandırmanıza olanak verir. SelectAction özelliği aşağıdaki eylemleri sağlar:

- TreeNodeSelectAction.Expand başlatır düğümü seçildiğinde TreeNodeExpanded.
- TreeNodeSelectAction.None düğümü seçildiğinde hiçbir olayını başlatır.
- TreeNodeSelectAction.Select düğümü seçildiğinde SelectedNodeChanged olayını başlatır.
- TreeNodeSelectAction.SelectExpand SelectedNodeChanged olayı ve düğümü seçildiğinde TreeNodeExpanded olayını başlatır.

## <a name="controlling-appearance-with-styles"></a>Stil görünümünü denetleme

TreeView denetimi stilleri denetiminin görünümünü kontrol etmek için birçok özellik sağlar. Aşağıdaki özellikler kullanılabilir.

| **Özellik adı** | **Denetimleri** |
| --- | --- |
| HoverNodeStyle | Fare üzerlerine vurgulanan zaman düğümleri stilini denetler. |
| LeafNodeStyle | Yaprak düğümlerin stilini denetler. |
| NodeStyle | Tüm düğümler için stil denetler. Belirli düğüm stilleri (örneğin, LeafNodeStyle) bu stili geçersiz kılar. |
| ParentNodeStyle | Tüm üst düğümlerin stilini kontrol eder. |
| RootNodeStyle | Kök düğüm için stil denetler. |
| SelectedNodeStyle | Seçili düğümün stilini kontrol eder. |

Bu özelliklerin her biri, salt okunur durumdadır. Ancak, bunlar her iade edecek bir **TreeNodeStyle** nesne ve bu nesnenin özellikleri kullanılarak değiştirilebilir *özelliği alt* biçimi. Örneğin, ayarlamak için **ForeColor** özelliği **SelectedNodeStyle**, aşağıdaki sözdizimini kullanın:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Yukarıdaki etiketi kapatılmadı dikkat edin. Burada gösterilen tanımlayıcı sözdizimi kullanılırken de HTML kodunda TreeViews düğümler arasında olmasıdır.

Stil özelliklerini kod kullanarak da belirtilebilir *property.subproperty* biçimi. Örneğin, ayarlamak için **ForeColor** özelliği **RootNodeStyle** kodda, aşağıdaki sözdizimini kullanın:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Farklı bir stil özellikleri kapsamlı bir listesi için TreeNodeStyle nesnede MSDN belgelerine bakın.


## <a name="the-sitemappath-control"></a>ASP

ASP, ASP.NET geliştiricilerinin ekmek Kırıntı Gezinti denetimi sağlar. Diğer Gezinti denetimleri gibi veri kaynakları SiteMapDataSource veya XmlDataSource gibi hiyerarşik veriye bağlı kolayca olabilir.

Bir ASP SiteMapNodeItem nesnelerin yapılır. Üç tür düğüm vardır; Kök düğüm, üst düğümleri ve geçerli düğüm. Kök düğüm, hiyerarşik bir yapı üstündeki olandır. Geçerli düğüm geçerli sayfasını temsil eder. Diğer tüm düğümlere üst düğümler var.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>ASP işlemi denetleme

ASP işleyişini denetleyen özellikler aşağıdaki gibidir:

| **Özelliği** | **Özellik açıklaması** |
| --- | --- |
| ParentLevelsDisplayed | Kaç tane üst düğümleri görüntülenme şeklini denetler. Varsayılan değer görüntülenen üst düğüm sayısını sınırlaması getirir -1 ' dir. |
| PathDirection | SiteMapPath yönünü denetler. Geçerli değerler RootToCurrent (varsayılan) ve CurrentToRoot ' dir. |
| PathSeparator | Bir ASP düğümler ayıran karakter denetleyen bir dize. Varsayılan değer:. |
| RenderCurrentNodeAsLink | Geçerli düğüm bir bağlantıyı destekleyip desteklemediğini işlenmeden denetimleri bir Boole değeri. Varsayılan değer False olur. |
| SkipLinkText | Sayfanın ekran okuyucular tarafından görüntülendiğinde erişilebilirlik ile yardımcı olur. Bu özellik, ASP atlamak ekran okuyucular sağlar. Bu özellik devre dışı bırakmak için String.Empty özelliğini ayarlayın. |

## <a name="templated-sitemappath-controls"></a>Şablonlu SiteMapPath denetimleri

SiteMapControl şablonlu denetimdir ve denetim görüntüleme farklı şablonları kullanmak için bu nedenle tanımlayabilirsiniz. Bir ASP şablonlarını düzenlemek için denetimi akıllı etiket düğmeyi tıklatın ve menüden Şablonları Düzenle. Kullanılabilir farklı şablonlar arasında seçebileceğiniz aşağıda gösterildiği gibi bu SiteMapTasks menüsünü görüntüler.


![](data-bound-controls/_static/image2.jpg)

**Şekil 2**


**NodeTemplate** SiteMapPath herhangi bir düğüm için şablon başvuruyor. Düğüm bir kök düğümü veya geçerli düğüm ise ve **RootNodeTemplate** veya **CurrentNodeTemplate** olan yapılandırılmış, NodeTemplate geçersiz kılındı.

## <a name="sitemappath-events"></a>SiteMapPath olayları

ASP denetim sınıfından türetilmemiş iki olay; yine de sahip istiyor musunuz? **ItemCreated** olay ve **ItemDataBound** olay. Bir SiteMapPath öğesi oluşturulduğunda ItemCreated olay tetiklenir. SiteMapPath düğümü veri bağlama sırasında DataBind yöntemi çağrıldığında ItemDataBound tetiklenir. A **SiteMapNodeItemEventArgs** nesne öğe özelliği yoluyla belirli SiteMapNodeItem erişim sağlar.

## <a name="controlling-appearance-with-styles"></a>Stil görünümünü denetleme

Aşağıdaki stiller, ASP biçimlendirme için kullanılabilir.

| **Özellik adı** | **Denetimleri** |
| --- | --- |
| CurrentNodeStyle | Geçerli düğüm için metin stilini denetler. |
| RootNodeStyle | Kök düğüm için metin stilini denetler. |
| NodeStyle | CurrentNodeStyle veya RootNodeStyle uygulanmaz varsayılarak tüm düğümler için metin stilini denetler. |

NodeStyle özelliği CurrentNodeStyle veya RootNodeStyle tarafından geçersiz kılınır. Bu özelliklerin her biri salt okunurdur ve döndüren bir **stili** nesnesi. Bu özelliklerden birini kullanarak bir düğümü görünümünü etkilemek için döndürülen stil nesnesinin özelliklerini ayarlamanız gerekir. Örneğin, aşağıdaki kodu geçerli düğümünün forecolor özelliği değiştirir.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

Özellik de bir şekilde program aracılığıyla uygulanabilir:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Bir şablon uyguladıysanız, stil uygulanmaz.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Laboratuvar 1: bir ASP.NET menü denetimi yapılandırma

1. Yeni bir Web sitesi oluşturun.
2. Dosya, yeni dosya seçerek ve Site Haritası dosya şablonları listesinden bir Site haritası dosyası ekleyin.
3. Site haritasını (varsayılan olarak birtakım) açın ve liste gibi görünüyor şekilde değiştirin. Site haritası bağlama sayfaları gerçekten var, ancak bu alıştırma için bir sorun olmayacaktır.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Varsayılan Web formu Tasarım görünümünde açın.
5. Araç Kutusu Gezinti bölümünden yeni bir menü denetimi sayfasına ekleyin.
6. Araç veri bölümünden yeni SiteMapDataSource ekleyin. SiteMapDataSource, sitenizdeki birtakım dosya otomatik olarak kullanır. (Birtakım dosya *gerekir* sitenin kök klasöründe olabilir.)
7. Menü denetimi tıklatın ve ardından menü görevler iletişim kutusunu görüntülemek için akıllı etiket düğmesine tıklayın.
8. Veri Kaynağı Seç açılır listede SiteMapDataSource1 seçin.
9. Otomatik Biçim bağlantısına tıklayın ve bir biçim menüsünü seçin.
10. Özellikler bölmesinde ayarlayın **StaticDisplayLevels** özelliğini 2. Menü denetimi, şimdi Tasarımcısı'nda giriş, ürünler ve hizmetler düğümü görüntülemelidir.
11. Sayfanın menüsünü kullanmak için tarayıcınızda göz atın. (Site haritası önceden eklenen sayfaları gerçekten mevcut değil çünkü deneyin ve bunlara göz atın, bir hata görürsünüz.)

StaticDisplayLevels ve MaximumDynamicDisplayLevels özelliklerini değiştirerek ve menü nasıl işlendiğine nasıl etkilediklerini bakın.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Laboratuvar 2: Bir TreeView denetimi dinamik olarak bağlama

Bu alıştırmada, yerel olarak çalışan SQL Server olması ve Northwind veritabanı SQL Server örneğinde mevcut olduğunu varsayar. Lütfen bu koşullar karşılanmazsa, örnekteki bağlantı dizesini değiştirin. Aynı zamanda güvenilen bir bağlantı yerine SQL Server kimlik doğrulaması belirtmek gerekebileceğini unutmayın.

1. Yeni bir Web sitesi oluşturun.
2. Default.aspx kod görünümüne geçin ve tüm kod aşağıda listelenen kod ile değiştirin. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Sayfayı treeview.aspx kaydedin.
4. Sayfa göz atın.
5. Sayfa ilk görüntülendiğinde sayfasının kaynağı tarayıcınızda görüntüleyin. Yalnızca görünür düğüm istemciye gönderilen unutmayın.
6. Herhangi bir düğümün yanındaki artı işaretini tıklatın.
7. Yeniden sayfasında kaynağı görüntüle. Yeni görüntülenen düğümleri mevcut olduğuna dikkat edin.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Laboratuvar 3: Görünümü ve bir GridView ve DetailsView kullanarak verileri düzenleme ayrıntıları

1. Yeni bir Web sitesi oluşturun.
2. Yeni bir web.config Web sitesine ekleyin.
3. Aşağıda gösterildiği gibi web.config dosyasında bir bağlantı dizesi ekleyin: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Ortamınıza temel bağlantı dizesi değiştirmeniz gerekebilir.
4. Web.config dosyasını kaydedip kapatın.
5. Default.aspx açın ve yeni bir SqlDataSource denetim ekleyin.
6. SqlDataSource denetiminin Kimliğini değiştirmek **ürünleri**.
7. İçinde **SqlDataSource görevleri** menüsünde tıklatın **Configure Data Source**.
8. Seçin **Northwind** bağlantı açılır ve İleri'yi tıklatın.
9. Seçin **ürünleri** gelen **adı** açılır ve denetleme **ProductID**, **ProductName**, **UnitPrice**, ve **unitsInStock** aşağıda gösterildiği gibi onay kutuları. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. 
              **İleri**'ye tıklayın.
11. **Son**'a tıklayın.
12. Kaynak görünümüne geçin ve oluşturulan kodu inceleyin. Bildirim **SelectCommand**, **DeleteCommand**, **InsertCommand**, ve **UpdateCommand** SqlDataSource eklendi Denetim. Ayrıca, eklenen parametreleri dikkat edin.
13. Tasarım görünümüne geçin ve yeni bir GridView denetim sayfasına ekleyin.
14. Seçin **ürünleri** gelen **veri kaynağı Seç** açılır.
15. Denetleme **etkinleştirmek disk belleği** ve **Seçimi Etkinleştir** aşağıda gösterildiği gibi. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Tıklatın **Edit Columns** emin olun ve bağlama **otomatik oluşturma alanları** denetlenir.
17. **Tamam**'ı tıklatın.
18. GridView denetimi seçili yanındaki düğmesini **DataKeyNames** özelliği Özellikler bölmesinde.
19. Seçin **ProductID** gelen **kullanılabilir veri alanları** listesinde ve tıklatın  **&gt;**  eklemek için düğmesi.
20. Tamam'ı tıklatın.
21. Yeni bir SqlDataSource denetim sayfasına ekleyin.
22. SqlDataSource denetiminin Kimliğini değiştirmek **ayrıntıları**.
23. SqlDataSource Görevler menüsünden **Configure Data Source**.
24. Seçin **Northwind** tıklayın ve açılan **sonraki**.
25. Seçin **ürünleri** gelen **adı** açılır ve denetleme  **\***  onay kutusu **sütunları** listbox.
26. Tıklatın **burada** düğmesi.
27. Seçin **ProductID** gelen **sütun** açılır.
28. Seçin  **=**  işleci açılır.
29. Seçin **denetim** gelen **kaynak** açılır.
30. Seçin **GridView1'i** gelen **denetim kimliği** açılır.
31. Tıklatın **Ekle** düğmesi WHERE yan tümcesi ekleyin.
32. **Tamam**'ı tıklatın.
33. Tıklatın **Gelişmiş** düğmesine tıklayın ve denetleme **Generate INSERT, UPDATE ve DELETE deyimleri** onay kutusu.
34. **Tamam**'ı tıklatın.
35. Tıklatın **sonraki** tıklatıp **son**.
36. DetailsView denetimini sayfasına ekleyin.
37. İçinde **veri kaynağı Seç** açılan listesinde, seçin **ayrıntıları**.
38. Denetleme **düzenlemeyi etkinleştir** aşağıda gösterildiği gibi onay kutusu. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Sayfayı kaydedin ve Default.aspx göz atın.
40. Tıklatın **seçin** DetailsView güncelleştirmeyi otomatik olarak görmek için farklı kayıtlar yanındaki bağlantı.
41. Tıklatın **Düzenle** DetailsView denetiminde bağlantı.
42. Kayda değişiklik yaparak tıklatın **güncelleştirme**.
