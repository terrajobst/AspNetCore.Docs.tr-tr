---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Veri bağlama denetimleri | Microsoft Docs
author: microsoft
description: Çoğu ASP.NET uygulama bir arka uç veri kaynağından veri sunumu bir ölçüde kullanır. Verilere bağlı denetimler etkileşim kuran w pivotal bir parçası olan...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: bfee5ed9c786cc8799da108b99781ff6eb508477
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833225"
---
<a name="data-bound-controls"></a>Veri sınırı denetimleri
====================
tarafından [Microsoft](https://github.com/microsoft)

> Çoğu ASP.NET uygulama bir arka uç veri kaynağından veri sunumu bir ölçüde kullanır. Verilere bağlı denetimler, dinamik Web uygulamalarında verilerle etkileşim pivotal bir parçası olmuştur. ASP.NET 2.0 verilere bağlı denetimler, yeni BaseDataBoundControl sınıfı ve bildirim temelli söz dizimi de dahil olmak üzere bazı önemli geliştirmeler sunuyor.


Çoğu ASP.NET uygulama bir arka uç veri kaynağından veri sunumu bir ölçüde kullanır. Verilere bağlı denetimler, dinamik Web uygulamalarında verilerle etkileşim pivotal bir parçası olmuştur. ASP.NET 2.0 verilere bağlı denetimler, yeni BaseDataBoundControl sınıfı ve bildirim temelli söz dizimi de dahil olmak üzere bazı önemli geliştirmeler sunuyor.

BaseDataBoundControl DataBoundControl sınıfı ve HierarchicalDataBoundControl sınıfı için temel sınıf olarak görev yapar. DataBoundControl türetilen aşağıdaki sınıflar Bu modülde ele alınacaktır:

- AdRotator
- Liste denetimleri
- GridView
- FormView
- DetailsView

Ayrıca HierarchicalDataBoundControl sınıfından türetilen aşağıdaki sınıflar ele alınacaktır:

- TreeView
- Menü
- SiteMapPath

## <a name="databoundcontrol-class"></a>DataBoundControl sınıfı

Tablo ile etkileşim kurmak için kullanılan (vb'de MustInherit işaretlenmiştir) soyut bir sınıf DataBoundControl sınıfı olan veya liste stilini veri. Aşağıdaki denetimler DataBoundControl türetilen denetimler bazılarıdır.

## <a name="adrotator"></a>AdRotator

AdRotator denetimi, belirli bir URL'ye yapılır bağlı bir Web sayfasındaki bir grafik başlığı görüntülemenizi sağlar. Görüntülenen grafiğin özelliklerini kullanarak denetim için döndürülür. Belirli ad görüntülemenin bir sayfada sıklığını kullanılarak yapılandırılabilir **izlenimler** özelliği ve reklam anahtar sözcük filtresi kullanarak filtrelenebilir.

AdRotator denetimleri bir XML dosyası veya tablo için verileri bir veritabanında kullanın. Aşağıdaki öznitelikler XML dosyalarında AdRotator denetimini yapılandırmak için kullanılır.

### <a name="imageurl"></a>ImageUrl
Ad için görüntülenecek bir görüntü URL'si.

### <a name="navigateurl"></a>NavigateUrl
Ad tıklandığında kullanıcı için yapılması gereken URL. Bu URL olarak kodlanmış olmalıdır.

### <a name="alternatetext"></a>AlternateText
Bir araç ipucunda görüntülenen ve ekran okuyucular tarafından okunan alternatif metin. ImageUrl tarafından belirtilen görüntünün bulunmadığında da görüntüler.

### <a name="keyword"></a>Anahtar sözcüğü
Anahtar sözcük filtreleme kullanırken kullanılabilecek bir anahtar sözcük tanımlar. Bu seçenek belirtilmişse, yalnızca bu reklam anahtar sözcük Filtresi ile eşleşen bir anahtar sözcüğü ile görüntülenir.

### <a name="impressions"></a>İzlenimler
Belirli bir ad ne sıklıkta görünür olasılığı belirleyen ağırlığı sayı. Aynı dosyada diğer reklam izlenimi göredir. En yüksek değeri bir XML dosyasındaki tüm reklamlar için toplu izlenimler 2,048,000,000 1 ' dir.

### <a name="height"></a>Yükseklik
Ad piksel cinsinden yüksekliği.

### <a name="width"></a>Genişlik
Ad piksel cinsinden genişliği.


> [!NOTE]
> Yükseklik ve genişlik öznitelikleri yüksekliğini ve genişliğini AdRotator denetimi için geçersiz kılın.


Tipik bir XML dosyası aşağıdakine benzeyebilir:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Yukarıdaki örnekte, Contoso için ad iki kez izlenimler özniteliğinin değeri nedeniyle, ASP.NET Web sitesi için bir ad olarak görünür olarak olasıdır.

Yukarıdaki XML dosyasından reklam görüntülemek için sayfaya AdRotator denetim ekleme ve ayarlama **AdvertisementFile** aşağıda gösterildiği gibi XML dosyasına işaret edecek şekilde özelliği:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

AdRotator denetiminiz için bir veritabanı tablosu veri kaynağı olarak kullanmayı seçerseniz, aşağıdaki şemayı kullanarak bir veritabanı ayarlamak öncelikle gerekir:

| **Sütun adı** | **Veri türü** | **Açıklama** |
| --- | --- | --- |
| Kimlik | int | Birincil anahtar. Bu sütun, herhangi bir ad olabilir. |
| ImageUrl | nvarchar (*uzunluğu*) | Ad için görüntülemek için göreli veya mutlak URL'si görüntüsü. |
| NavigateUrl | nvarchar (*uzunluğu*) | Ad hedef URL'si. Bir değer belirtmezseniz, ad köprü değil. |
| AlternateText | nvarchar (*uzunluğu*) | Görüntü bulunamazsa görüntülenen metin. Bazı tarayıcılarda metin araç ipucu olarak görüntülenir. Grafik göremez kullanıcılar yüksek sesle okumak açıklamasını duyabileceğiniz alternatif metin erişilebilirlik için de kullanılır. |
| Anahtar sözcüğü | nvarchar (*uzunluğu*) | Bir kategori ad sayfasında filtre uygulayabilirsiniz. |
| İzlenimler | int(4) | Ad ne sıklıkta görüntülenen olasılığını gösteren bir sayı. Büyük sayı, daha sık ad görüntülenir. XML dosyasındaki tüm izlenimler değerlerin toplamını 2,048,000,000 1 aşamaz. |
| Genişlik | int(4) | Resmin piksel cinsinden genişliği. |
| Yükseklik | int(4) | Piksel cinsinden görüntü yüksekliği. |

Zaten sahip olduğunuz farklı bir şeması olan bir veritabanı durumlarda kullanabilirsiniz **AlternateTextField**, **ImageUrlField**, ve **NavigateUrlField** eşlemek için özellikleri Mevcut veritabanını AdRotator öznitelikleri. AdRotator denetimindeki veritabanından veri görüntüleme için sayfaya veri kaynak denetimi ekleyin, veritabanınıza işaret edecek şekilde veri kaynak denetimi için bağlantı dizesini yapılandırmak ve AdRotator denetimin ayarlayın **DataSourceID** özelliği veri kaynağı denetiminin kimliği. Bir ihtiyaç AdRotator sahip olduğu durumlarda reklam AdCreated olay programlı olarak kullanın. AdCreated olay iki parametre alır; bir nesne, diğeri AdCreatedEventArgs örneği. AdCreatedEventArgs oluşturulan ad bir başvurudur.

Aşağıdaki kod parçacığı için bir ad program aracılığıyla ImageUrl NavigateUrl ve AlternateText ayarlar:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Liste denetimleri

Liste denetimleri ListBox, DropDownList, CheckBoxList, RadioButtonList ve Bulletedlıst içerir. Bu denetimlerin her birinden veri bir veri kaynağına bağlı olabilir. Bunlar, bir alan veri kaynağında görünen metni olarak kullanın ve isteğe bağlı olarak, ikinci bir alan bir öğenin değerini kullanabilirsiniz. Öğeleri tasarım zamanında de bir statik olarak eklenebilir ve statik öğeleri ve dinamik öğeleri veri kaynağından eklenen karıştırabilirsiniz.

Verileri bir liste denetimini bağlama, bir veri kaynağı denetimi sayfasına ekleyin. Veri kaynak denetimi için bir SELECT komutu belirtin ve ardından DataSourceID özelliği liste denetimi veri kaynağı denetimini Kimliğine ayarlayın. Kullanım **DataTextField** ve **DataValueField** görüntülenecek metni ve denetimi için değer tanımlamak için özellikleri. Ayrıca, kullanabileceğiniz **DataTextFormatString** özelliği şu şekilde görünen metin görünümünü kontrol etmek için:

| **İfade** | **Açıklama** |
| --- | --- |
| Fiyat: {0:C} | Sayısal/ondalık verileri. Değişmez değer görüntüler "Fiyat:" sayılar para biçiminde ardından. Culture özniteliği içinde belirtilen kültür ayarı para birimi biçimi bağlıdır **sayfa** yönerge veya Web.config dosyasında. |
| {0:D4} | Tamsayı verileri. Ondalık sayı ile kullanılamaz. Tamsayıların dört karakter geniş sıfır doldurulan bir alanda görüntülenir. |
| {0:N2}% | Sayısal verileri. İle 2 ondalık basamak sayısını görüntüler duyarlık "%" değişmez değer tarafından izlenen. |
| {0:000.0} | Sayısal/ondalık verileri. Sayı bir ondalık basamağa yuvarlanır. Üç basamak, sıfır edilirken daha az numaralandırır. |
| {0:D} | Tarih/Saat verileri. Uzun tarih biçimi ("Perşembe 06 Ağustos 1996") görüntüler. Tarih biçimi sayfa veya Web.config dosyasında kültür ayarına bağlıdır. |
| {0:d} | Tarih/Saat verileri. Görüntüler kısa tarih ("12/31/99") biçim. |
| {0:yy-MM-dd} | Tarih/Saat verileri. Sayısal yıl ay gün biçiminde (96-08-06) tarihi görüntüler |

## <a name="gridview"></a>GridView

GridView denetiminde tablo verilerini görüntüleme ve bildirim temelli bir yaklaşım kullanarak düzenleme olanak sağlar ve DataGrid denetimi geçmiştir. GridView denetiminde aşağıdaki özellikler kullanılabilir.

- SqlDataSource gibi kaynak denetimlerini verilere bağlama.
- Yerleşik sıralama özellikleri.
- Yerleşik güncelleştirme ve silme özellikleri.
- Yerleşik disk belleği özellikleri.
- Yerleşik satır seçimi özellikleri.
- GridView nesne modeli, özellikler, dinamik olarak ayarlamak için programlı erişim olaylarını işlemek ve benzeri.
- Birden çok anahtar alanları.
- Köprü sütunlar için birden fazla veri alanları.
- Özelleştirilebilir görünümü temalar ve stilleri.

**Sütun alanları**

GridView denetiminde her sütun bir DataControlField nesnesi tarafından temsil edilir. Varsayılan olarak, GenerateColumns ayarlandığı **true**, veri kaynağındaki her alan için bir AutoGeneratedField nesnesi oluşturur. Her alanı daha sonra veri kaynağındaki her bir alanın görüntülenen sırayla GridView denetimindeki bir sütun olarak işlenir. Alanları görünür sütunu el ile de denetleyebilirsiniz **GridView** ayarlayarak denetim **GenerateColumns** özelliğini **false** ve ardından, kendi tanımlama sütun alanı koleksiyonu. Farklı sütun alanı türleri denetiminde sütunları davranışını belirleyin.

Aşağıdaki tablo kullanılabilecek farklı sütun alan türlerini listeler.

| **Sütun alanı türü** | **Açıklama** |
| --- | --- |
| BoundField | Bir veri kaynağında bir alanın değerini görüntüler. GridView denetiminde varsayılan sütun türü budur. |
| ButtonField | Her öğe için bir komut düğmesi GridView denetiminde görüntüler. Bu, bir sütun Ekle veya Kaldır düğmesi gibi özel düğme denetimleri oluşturmak sağlar. |
| CheckBoxField | GridView denetiminde her öğe için bir onay kutusu görüntüler. Bu sütun bir alan türü, bir Boole değeri ile alanlarını görüntülemek için yaygın olarak kullanılır. |
| CommandField | Görüntüler, seçme, düzenleme veya silme işlemleri gerçekleştirmek için komut düğmeleri önceden tanımlanmış. |
| HyperLinkField | Bir alanın değerini bir veri kaynağında bir köprü olarak görüntüler. Bu sütun bir alan türü, ikinci bir alanı köprünün URL'sini bağlamanıza olanak sağlar. |
| ImageField | Her öğe için bir görüntü GridView denetiminde görüntüler. |
| TemplateField | Görüntüler kullanıcı tanımlı içerik belirtilen bir şablona göre GridView denetimindeki her bir öğe için. Bu sütun bir alan türü, özel sütun alanı oluşturmanızı sağlar. |

Bir sütun alanı koleksiyonu bildirimli olarak tanımlamak için önce açılış ve kapanış ekleyin **&lt;sütunları&gt;** etiketleri arasında açılış ve kapanış etiketlerinin GridView denetimi. Ardından, açılış ve kapanış arasında dahil etmek istediğiniz sütunu alanlar listesinde **&lt;sütunları&gt;** etiketler. Belirtilen sütun, sütunlar koleksiyonuna listelendikleri sırada eklenir. **Sütunları** koleksiyon depolar tüm sütun alanları denetimi ve GridView denetimindeki sütun alanlarının programlı bir şekilde yönetmenizi sağlar.

Açıkça bildirilirse sütun alanları otomatik olarak oluşturulan sütun alanları ile birlikte görüntülenebilir. Her ikisi de kullanıldığında, açıkça bildirilirse sütun alanları otomatik olarak oluşturulan sütun alanlara göre ve ardından ilk olarak işlenir.

## <a name="binding-to-data"></a>Verilere Bağlama

GridView denetiminde veri kaynak denetimine bağlı olabilir (gibi **SqlDataSource**, **ObjectDataSource**, vb.), yanı sıra System.Collections.IEnumerable uygulayan herhangi bir veri kaynağı Arabirim (örneğin, System.Data.DataView, System.Collections.ArrayList veya System.Collections.Hashtable). GridView denetimi için uygun bir veri kaynağı türü bağlamak için aşağıdaki yöntemlerden birini kullanın:

- Bir veri kaynak denetimine bağlamak için DataSourceID özelliği GridView denetimi veri kaynağı denetimini kimliği değerini ayarlayın. GridView denetiminde otomatik olarak belirtilen veri kaynak denetimine bağlar ve veri kaynağının sıralama, güncelleştirme, silme ve disk belleği işlevleri gerçekleştirmek için denetimin özelliklerinden yararlanabilirsiniz. Bu verilere bağlamak için tercih edilen yöntemdir.
- System.Collections.IEnumerable arabirimi uygulayan bir veri kaynağına bağlamak için program aracılığıyla GridView denetiminde veri kaynağı özelliğini veri kaynağına ayarlayın ve ardından DataBind yöntemi çağırın. GridView denetiminde bu yöntemi kullanırken, yerleşik sıralama, güncelleştirme, silme ve işlevsellik sayfalama sağlamaz. Bu işlev kendiniz sağlamanız gerekir.

## <a name="data-operations"></a>Veri İşlemleri

GridView denetiminde, kullanıcının sıralama, güncelleştirme, silme, seçin ve denetimindeki öğeler arasında sayfasında olanak tanıyan birçok yerleşik özellikleri sağlar. GridView denetiminde veri kaynak denetimine bağlı olduğunda GridView denetiminde denetimin özellikleri veri kaynağı yararlanabilir ve Otomatik sıralama, güncelleştirme ve silme işlevleri sağlar.

> [!NOTE]
> GridView denetiminde, sıralama, güncelleştirme ve diğer türde veri kaynaklarını silmek için destek sağlayabilirsiniz; Ancak, bu işlemler için uygulama ile ilgili olay işleyicisi sağlamak gerekir.


Sıralama sütunun başlığına tıklayarak belirli bir sütuna göre GridView denetimindeki öğeleri sıralama izin verir. Sıralamayı etkinleştirmek için AllowSorting özelliğini ayarlamak **true**.

Otomatik güncelleştirme, silme ve seçim işlevlerini düğme etkin bir **ButtonField** veya **TemplateField** "Düzenle", "Delete" ve "Seçin" komut adı ile sütun alanı Sırasıyla tıklandığında. GridView denetiminde otomatik olarak ekleyebilirsiniz bir **CommandField** sütun alanı bir düzenleme, silme veya içinAutoGenerateEditButton,AutoGenerateDeleteButtonveyaAutoGenerateSelectButtonözelliğiayarlanmışsaseçmedüğmesiile**true**sırasıyla.

> [!NOTE]
> Veri kaynağına kayıt ekleme, GridView denetimi tarafından doğrudan desteklenmiyor. Ancak, GridView denetimi DetailsView birlikte veya FormView denetimi kullanarak kayıtları eklemek mümkündür.


Aynı anda veri kaynağındaki tüm kayıtları görüntülemek yerine GridView denetiminde otomatik olarak kayıtları sayfalara bölmek. Disk belleğini etkinleştirmek için AllowPagingözelliðini ayarlamak **true**.

## <a name="customizing-the-user-interface"></a>Kullanıcı Arabirimini Özelleştirme

Denetimin farklı bölümlerine stil özelliklerini ayarlayarak GridView denetiminin görünümünü özelleştirebilirsiniz. Aşağıdaki tabloda, farklı bir stil özellikleri listeler.

| **Stil özelliği** | **Açıklama** |
| --- | --- |
| AlternatingRowStyle | GridView denetiminde değişen veri satırları için stilini ayarlar. Bu özelliği ayarlandığında, veri satırları RowStyle ayarları arasında değişen görüntülenir ve **AlternatingRowStyle** ayarları. |
| EditRowStyle | GridView denetiminde düzenlenmekte olan satır için stilini ayarlar. |
| EmptyDataRowStyle | GridView denetiminde veri kaynağı herhangi bir kayıt olmadığında görüntülenen boş veri satırı için stilini ayarlar. |
| FooterStyle | GridView denetiminde alt bilgi satırının stilini ayarlar. |
| HeaderStyle | GridView denetiminde üst bilgi satırının stilini ayarlar. |
| PagerStyle | GridView denetiminde çağrı satırının stilini ayarlar. |
| RowStyle | GridView denetiminde veri satırları için stilini ayarlar. Zaman **AlternatingRowStyle** özelliği ayarlanmışsa, veri satırları arasında değişen görüntülenen **RowStyle** ayarları ve **AlternatingRowStyle** ayarları. |
| SelectedRowStyle | GridView denetiminde seçili satır için stilini ayarlar. |

Ayrıca, göstermek veya denetim farklı kısımlarını gizleyebilirsiniz. Aşağıdaki tablo, hangi parçalarının gösterilen veya gizli denetleyen özellikleri listeler.

| **Özelliği** | **Açıklama** |
| --- | --- |
| ShowFooter | GridView denetiminde alt kısmında gizler veya gösterir. |
| ShowHeader | GridView denetiminde üst bilgi bölümünü gizler veya gösterir. |

### <a name="events"></a>Olaylar

GridView denetiminde programlayabileceğiniz çeşitli olayları sağlar. Bu, her bir olay gerçekleştiğinde, özel bir yordamı çalıştırmak sağlar. GridView denetimi tarafından desteklenen olaylar aşağıdaki tabloda listelenmektedir.

| **Olay** | **Açıklama** |
| --- | --- |
| PageIndexChanged | Çağrı düğmelerden birine tıklandığında, ancak disk belleği işlemi GridView denetiminde işleme sonra gerçekleşir. Bu olay, bir kullanıcı denetiminde farklı bir sayfasına gider sonra bir görev gerçekleştirmeniz gerektiğinde, yaygın olarak kullanılır. |
| PageIndexChanging | Çağrı düğmelerden birine tıklandığında, ancak önce GridView denetimi disk belleği işlemi işleme gerçekleşir. Bu olay, genellikle disk belleği işlemi iptal etmek için kullanılır. |
| RowCancelingEdit | Bir sıranın iptal düğmesine tıklandığında, ancak önce GridView denetiminde yinelenir düzenleme modunda olduğunda oluşur. Bu olay, genellikle iptal etme işlemi durdurmak için kullanılır. |
| RowCommand | GridView denetiminde bir düğme tıklandığında gerçekleşir. Bu olay, genellikle bir düğme denetimi tıklandığında bir görevi gerçekleştirmek için kullanılır. |
| RowCreated | GridView denetiminde yeni bir satır oluşturulduğunda gerçekleşir. Bu olay, genellikle satır oluşturulduğunda bir satırın içeriğini değiştirmek için kullanılır. |
| RowDataBound | GridView denetiminde verileri bir veri satırı bağlandığında gerçekleşir. Bu olay, genellikle satır verilere bağlı olup bir satırın içeriğini değiştirmek için kullanılır. |
| RowDeleted | Bir sıranın Sil düğmesine tıklandığında, ancak GridView denetiminde kayıt veri kaynağından sildikten sonra gerçekleşir. Bu olay, genellikle sonuçlarını silme işlemini denetlemek için kullanılır. |
| RowDeleting | Bir sıranın Sil düğmesine tıklandığında, ancak önce GridView denetimi kaydı veri kaynağından siler gerçekleşir. Bu olay, genellikle silme işlemi iptal etmek için kullanılır. |
| RowEditing | Bir sıranın Düzenle düğmesine tıklandığında, ancak önce GridView denetimi düzenleme moduna girer gerçekleşir. Bu olay, genellikle düzenleme işlemi iptal etmek için kullanılır. |
| RowUpdated | Bir sıranın güncelleştir düğmesine tıklandığında, ancak satır GridView denetiminde güncelleştirildikten sonra gerçekleşir. Bu olay sık sık güncelleştirme işlemi sonuçlarını denetlemek için kullanılır. |
| RowUpdating | Bir sıranın güncelleştir düğmesine tıklandığında, ancak önce GridView satır denetimi güncelleştirmeler olduğunda gerçekleşir. Bu olay sık sık güncelleştirme işlemini iptal etmek için kullanılır. |
| SelectedIndexChanged | Bir sıranın Seç düğmesine tıklandığında, ancak select işlemi GridView denetiminde işleme sonra gerçekleşir. Bu olay, genellikle bir satır denetimi seçtikten sonra bir görevi gerçekleştirmek için kullanılır. |
| SelectedIndexChanging | Bir sıranın Seç düğmesine tıklandığında, ancak önce GridView denetimi select işlemi işleme gerçekleşir. Bu olay, genellikle seçme işlemini iptal etmek için kullanılır. |
| Sıralı | Bir sütunu sırala köprü tıklandığında, ancak sıralama işlemi GridView denetiminde işleme sonra gerçekleşir. Bu olay, genellikle kullanıcı bir sütunu sıralamak için bir köprü tıklattıktan sonra bir görevi gerçekleştirmek için kullanılır. |
| Sıralama | Bir sütunu sırala köprü tıklandığında, ancak önce GridView denetimi sıralama işlemi işleme gerçekleşir. Bu olay, genellikle sıralama işlemi iptal veya özel bir sıralama yordamı gerçekleştirmek için kullanılır. |

## <a name="formview"></a>FormView

FormView denetimi, bir veri kaynağından tek bir kayıt görüntülemek için kullanılır. Satır alanları yerine kullanıcı tanımlı şablonları görüntüler dışında DetailsView denetimi için benzerdir. Kendi şablonlarınızı oluşturmak, verilerin nasıl görüntüleneceğini denetleme daha fazla esneklik sağlar. FormView denetimi aşağıdaki özellikleri destekler:

- SqlDataSource ve ObjectDataSource gibi kaynak denetimlerini verilere bağlama.
- Yerleşik ekleme özellikleri.
- Yerleşik güncelleştirme ve silme özellikleri.
- Yerleşik disk belleği özellikleri.
- FormView nesne modeli, özellikler, dinamik olarak ayarlamak için programlı erişim olaylarını işlemek ve benzeri.
- Özelleştirilebilir görünümü kullanıcı tanımlı şablonları, temalar ve stilleri.

## <a name="templates"></a>Şablonlar

FormView denetim içeriği görüntülemek farklı bölümlerini denetim şablonları oluşturmanız gerekir. Çoğu şablonları isteğe bağlıdır; Ancak, Denetim yapılandırıldığı modu için bir şablon oluşturmanız gerekir. Örneğin, ekleme kayıtları destekleyen bir FormView'da denetimi tanımlanan INSERT öğe şablonu olmalıdır. Aşağıdaki tabloda, oluşturabileceğiniz farklı şablonları listeler.

| **Şablon türü** | **Açıklama** |
| --- | --- |
| EditItemTemplate | FormView denetimini düzenleme modundayken veri satırı için içeriği tanımlar. Bu şablon, genellikle giriş denetimleri ve kullanıcı ile varolan bir kaydı düzenleyebilir komut düğmeleri içerir. |
| EmptyDataTemplate'i | FormView denetimini herhangi bir kayıt içermeyen bir veri kaynağına bağlı olduğunda görüntülenen boş veri satırı için içeriği tanımlar. Bu şablon, genellikle veri kaynağı herhangi bir kayıt içermez kullanıcıyı uyarmak için içerik içerir. |
| FooterTemplate | Alt satır için içeriği tanımlar. Bu şablon, genellikle altbilgi satırında görüntülemek istediğiniz tüm ek içeriklere de içerir. Alternatif olarak, yalnızca FooterText özelliğini ayarlayarak altbilgi satırında görüntülenecek metin belirtebilirsiniz. |
| HeaderTemplate | Üst bilgi satırı içeriğini tanımlar. Bu şablon, genellikle başlık satırındaki görüntülemek istediğiniz tüm ek içeriklere de içerir. Alternatif olarak, yalnızca başlık satırındaki HeaderText özelliğini ayarlayarak görüntülenecek metin belirtebilirsiniz. |
| ItemTemplate | FormView denetimini salt okunur modda olduğunda, veri satırı için içeriği tanımlar. Bu şablon, genellikle varolan bir kaydı değerlerini görüntülemek için içerik içerir. |
| InsertItemTemplate | FormView denetim ekleme modunda olduğunda veri satırı için içeriği tanımlar. Bu şablon, genellikle giriş denetimleri ve kullanıcının yeni bir kayıt eklemek komut düğmeleri içerir. |
| PagerTemplate | Disk belleği özelliği etkinleştirilmişse görüntülenen çağrı satır içeriğini tanımlar (AllowPagingözelliðini ayarlandığında **true**). Bu şablon, genellikle kullanıcı başka bir kayıtla gidebilirsiniz denetimleri içerir. |

Giriş denetimlerinde öğe şablonu Ekle ve öğe şablonu düzen bir veri kaynağı alanları bir iki yönlü bir bağlama ifadesi kullanarak bağlı olabilir. Bu, otomatik olarak bir güncelleştirme için bir giriş denetiminin değerlerini ayıklamak veya işlem eklemek FormView denetim sağlar. İki yönlü bir bağlama ifadeleri otomatik olarak özgün alan değerlerini görüntülemek için bir düzen öğe şablonunda giriş denetimleri de izin verin.

### <a name="binding-to-data"></a>Verilere Bağlama

FormView denetim bir veri kaynak denetimine bağlı olabilir (gibi **SqlDataSource**, AccessDataSource, **ObjectDataSource** vb.), veya uygulayan herhangi bir veri kaynağı System.Collections.IEnumerable arabirimi (örneğin, System.Data.DataView System.Collections.ArrayList ve System.Collections.Hashtable). FormView denetimi uygun bir veri kaynağı türüne bağlamak için aşağıdaki yöntemlerden birini kullanın:

- Bir veri kaynak denetimine bağlamak için veri kaynağı denetimini kimliği değerini FormView denetimin DataSourceID özelliği ayarlayın. FormView denetimi otomatik olarak belirtilen veri kaynak denetimine bağlar ve veri kaynağı ekleme, güncelleştirme, silme ve disk belleği işlevleri gerçekleştirmek için denetimin özelliklerinden yararlanabilirsiniz. Bu verilere bağlamak için tercih edilen yöntemdir.
- Uygulayan bir veri kaynağına bağlamak için **System.Collections.IEnumerable** arabirim program aracılığıyla veri kaynağına FormView Denetimin DataSource özelliği ayarlayabilir ve sonra veri bağlama yöntemini çağırın. Bu yöntemi kullanırken FormView denetimini yerleşik ekleme, güncelleştirme, silme ve disk belleği işlevselliğini sağlamaz. Uygun olay'ı kullanarak bu işlevselliği sağlayacak şekilde gerekir.

## <a name="data-operations"></a>Veri İşlemleri

FormView denetim, güncelleştirme, silme, ekleme ve denetimindeki öğeler arasında sayfasında kullanıcı çok sayıda yerleşik özellikleri sağlar. FormView denetim bir veri kaynak denetimine bağlı olduğunda FormView denetimini denetimin özellikleri veri kaynağı yararlanabilir ve otomatik güncelleştirme, silme, ekleme ve işlevsellik sayfalama sağlayın. FormView denetim, güncelleştirme, silme, ekleme ve sayfalandırma işlemleri ile diğer veri kaynağı türleri için destek sağlayabilirsiniz; Ancak, bu işlemler için uygulama ile ilgili olay işleyicisi sağlamanız gerekir.

FormView denetim şablonları kullandığından, güncelleştirme, silme veya ekleme işlemleri gerçekleştirmek için komut düğmeleri otomatik olarak oluşturabileceği bir yol sağlamaz. Bu komut düğmeleri uygun şablon el ile eklemeniz gerekir. FormView denetim olan belirli düğmeler kendi **CommandName** özellikleri belirli değerlere ayarlayın. FormView denetimini tanır komut düğmeleri aşağıdaki tabloda listelenmektedir.

| **Düğme** | **CommandName değeri** | **Açıklama** |
| --- | --- | --- |
| İptal | "İptal" | İşlemleri ekleme veya güncelleştirme işlemi iptal etmek ve kullanıcı tarafından girilen değerler atmak için kullanılır. FormView denetimini ardından DefaultMode özelliği tarafından belirtilen modunu döndürür. |
| Sil | "Sil" | Silme işlemleri veri kaynağından görüntülenen kaydı silmek için kullanılır. ItemDeleting ve ItemDeleted olayları başlatır. |
| Düzenle | "Edit" | Güncelleştirme işlemleri FormView denetim düzenleme moduna yerleştirmek için kullanılır. Belirtilen içerik **EditItemTemplate** özelliği, veri satırı için görüntülenir. |
| Ekleme | "Ekle" | Ekleme işlemleri veri kaynağında kullanıcı tarafından sağlanan değerleri kullanarak yeni bir kayıt eklemeye çalışırsanız için kullanılır. ItemInserting ve ItemInserted olayları başlatır. |
| Yeni | "Yeni" | İşlem ekleme FormView denetim ekleme modunda yerleştirmek için kullanılır. Belirtilen içerik **InsertItemTemplate** özelliği, veri satırı için görüntülenir. |
| Sayfa | "Page" | Disk belleği gerçekleştirir çağrı cihazı sırada bir düğmeyi temsil etmesi için disk belleği işlemlerinde kullanılır. Disk belleği işlemi belirtmek için ayarlayın **CommandArgument** özelliğini "İleri", "Önceki", "First", "Last" veya hangi gitmek sayfanın dizini. PageIndexChanging ve PageIndexChanged olayları başlatır. |
| Güncelleştirme | "Güncelleştir" | Güncelleştirme işlemleri görüntülenen veri kaynağındaki kaydı kullanıcı tarafından sağlanan değerlerle güncelleştirin denemek için kullanılır. ItemUpdating ve Itemupdated olayları başlatır. |

Silme aksine düğmesine (görüntülenen kaydı hemen silen), düzenleme veya yeni düğmesine tıklandığında, düzenleme denetimi bunlara FormView veya modu sırasıyla ekleyin. Düzenleme modunda yer alan içeriği **EditItemTemplate** özelliği için geçerli veri öğesi görüntülenir. Genellikle, Düzenle düğmesini iptal düğmesi ve bir güncelleştirme ile değiştirilir, öğe şablonu Düzen tanımlanır. Alanın veri türünü (örneğin, TextBox veya bir CheckBox denetimi) için uygun olan giriş denetimleri de genellikle kullanıcının değiştirmek bir alanın değeri görüntülenir. İptal düğmesine tıklayarak tüm değişiklikleri siler ancak güncelleştir düğmesine tıklayarak veri kaynağındaki kaydı güncelleştirir.

Benzer şekilde, yer alan içeriği **InsertItemTemplate** denetim ekleme modunda olduğunda veri öğesi için özelliği görüntülenir. INSERT öğe şablonu, genellikle yeni düğme ekleme ve bir iptal düğmesi ile değiştirilir ve yeni kayıt için değerleri girmesini boş giriş denetimlerinin görüntülenir tanımlanır. İptal düğmesine tıklayarak tüm değişiklikleri siler ancak Ekle düğmesine tıklayarak veri kaynağında kayıt ekler.

FormView denetimi, kullanıcının diğer veri kaynağındaki kayıtları gitmek bir disk belleği özelliği sağlar. Etkin olduğunda, çağrı cihazı satır sayfa gezinti denetimlerini içeren FormView denetimi görüntülenir. Disk belleğini etkinleştirmek için ayarlanmış **AllowPaging** özelliğini **true**. PagerStyle içinde bulunan nesnelerin özelliklerini ve PagerSettings özelliğini ayarlayarak, çağrı cihazı satır özelleştirebilirsiniz. Yerleşik çağrı satır UI kullanmak yerine, kendi kullanıcı arabirimini kullanarak oluşturabileceğiniz **PagerTemplate** özelliği.

## <a name="customizing-the-user-interface"></a>Kullanıcı Arabirimini Özelleştirme

Denetimin farklı bölümlerine stil özelliklerini ayarlayarak FormView denetiminin görünümünü özelleştirebilirsiniz. Aşağıdaki tabloda, farklı bir stil özellikleri listeler.

| **Stil özelliği** | **Açıklama** |
| --- | --- |
| EditRowStyle | FormView denetimini olduğunda veri satırı için stil ayarlarını düzenleme modu. |
| EmptyDataRowStyle | Veri kaynağı herhangi bir kayıt olmadığında FormView denetimde görüntülenen boş veri satırı için stilini ayarlar. |
| FooterStyle | FormView denetimini altbilgi satırının stilini ayarlar. |
| HeaderStyle | FormView denetimi üstbilgi satırının stilini ayarlar. |
| InsertRowStyle | FormView denetimini olduğunda veri satırı için stil ayarları modu ekleyin. |
| PagerStyle | Disk belleği özelliği etkinleştirilmişse FormView denetimde görüntülenen çağrı satır stili ayarları. |
| RowStyle | FormView denetimini salt okunur modunda olduğunda veri satırı için stilini ayarlar. |

## <a name="events"></a>Olaylar

FormView denetimini programlayabileceğiniz çeşitli olayları sağlar. Bu, her bir olay gerçekleştiğinde, özel bir yordamı çalıştırmak sağlar. FormView denetimi tarafından desteklenen olaylar aşağıdaki tabloda listelenmektedir.

| **Olay** | **Açıklama** |
| --- | --- |
| ItemCommand | FormView denetimindeki bir düğme tıklandığında gerçekleşir. Bu olay, genellikle bir düğme denetimi tıklandığında bir görevi gerçekleştirmek için kullanılır. |
| ItemCreated | FormView denetiminde tüm FormViewRow nesneler oluşturulduktan sonra gerçekleşir. Bu olay, genellikle gösterilmeden önce bir kayıt değerlerini değiştirmek için kullanılır. |
| ItemDeleted | Sil düğmesini oluşur (bir düğme kendi **CommandName** özelliği "Sil" olarak ayarlanmış) tıklandığında, ancak FormView denetim kaydı veri kaynağından siler. Bu olay, genellikle sonuçlarını silme işlemini denetlemek için kullanılır. |
| ItemDeleting | Sil düğmesine tıklandığında, ancak önce FormView denetim kaydı veri kaynağından siler gerçekleşir. Bu olay, genellikle silme işlemini iptal etmek için kullanılır. |
| ItemInserted | Ekle düğmesi oluşur (bir düğme ile kendi **CommandName** özelliği "Ekleme" olarak ayarlanmış) tıklandığında, ancak kayıt FormView denetimi ekledikten sonra. Bu olay sık sık ekleme işleminin sonuçlarını denetlemek için kullanılır. |
| ItemInserting | Ekle düğmesine tıklandığında, ancak önce FormView kaydı denetimi ekler gerçekleşir. Bu olay sık sık ekleme işlemi iptal etmek için kullanılır. |
| Itemupdated | Güncelleştir düğmesine oluşur (bir düğme ile kendi **CommandName** özelliği "Güncelleştir" için ayarlanmış) tıklandığında, ancak FormView denetimini satırı güncelleştirir. Bu olay sık sık güncelleştirme işlemi sonuçlarını denetlemek için kullanılır. |
| ItemUpdating | Güncelleştir düğmesine tıklandığında, ancak önce FormView denetim kaydı güncelleştirir gerçekleşir. Bu olay sık sık güncelleştirme işlemi iptal etmek için kullanılır. |
| ModeChanged | FormView denetimini modları değiştirdikten sonra gerçekleşir (için düzenleme, ekleme veya salt okunur modda). Bu olay, genellikle FormView Denetim modu değiştiğinde bir görevi gerçekleştirmek için kullanılır. |
| ModeChanging | FormView denetimini modları değiştirmeden önce ortaya çıkar (için düzenleme, ekleme veya salt okunur modda). Bu olay, genellikle bir modu değişikliği iptal etmek için kullanılır. |
| PageIndexChanged | Çağrı düğmelerden birine tıklandığında, ancak disk belleği işlemi FormView denetim işleme sonra gerçekleşir. Bu olay, kullanıcı denetiminde farklı bir kayda gider sonra bir görev gerçekleştirmeniz gerektiğinde, yaygın olarak kullanılır. |
| PageIndexChanging | Çağrı düğmelerden birine tıklandığında, ancak önce FormView disk belleği işlemi denetim işleme gerçekleşir. Bu olay, genellikle disk belleği işlemi iptal etmek için kullanılır. |

## <a name="detailsview"></a>DetailsView

DetailsView denetiminde, her alan kaydın bir tablonun satırında görüntülendiği bir tablodaki veri kaynağından tek bir kayıt görüntülemek için kullanılır. Bunu birlikte bir GridView denetimi ile ana öğe-ayrıntı senaryoları için kullanılabilir. DetailsView denetiminde aşağıdaki özellikleri destekler:

- SqlDataSource gibi kaynak denetimlerini verilere bağlama.
- Yerleşik ekleme özellikleri.
- Yerleşik güncelleştirme ve silme özellikleri.
- Yerleşik disk belleği özellikleri.
- Özelliği dinamik olarak ayarlamak için DetailsView nesne modeli programlı erişim olaylarını işlemek ve benzeri.
- Özelleştirilebilir görünümü temalar ve stilleri.

## <a name="row-fields"></a>Satır Alanları

DetailsView denetiminde her veri satırı bir alan denetimi bildirerek oluşturulur. Farklı satır alan türlerini denetimindeki satırları davranışını belirleyin. Alan denetimleri DataControlField türetilir. Aşağıdaki tablo kullanılabilecek farklı satır alan türlerini listeler.

| **Sütun alanı türü** | **Açıklama** |
| --- | --- |
| BoundField | Bir veri kaynağı, bir alanın değerini metin olarak görüntüler. |
| ButtonField | Bir komut düğmesi DetailsView denetiminde TemplateField görüntüler. Bu bir Ekle veya Kaldır düğmesi gibi özel bir düğme denetimini sahip bir satır görüntülemenize olanak sağlar. |
| CheckBoxField | DetailsView denetiminde TemplateField bir onay kutusu görüntüler. Bu satır alan türü, bir Boole değeri ile alanlarını görüntülemek için yaygın olarak kullanılır. |
| CommandField | Yerleşik komut görüntüler düğmeleri, düzenleme, gerçekleştirmek için ekleme veya silme işlemleri DetailsView denetiminde. |
| HyperLinkField | Bir alanın değerini bir veri kaynağında bir köprü olarak görüntüler. Bu satır alan türü, ikinci bir alanı köprünün URL'sini bağlamanıza olanak sağlar. |
| ImageField | DetailsView denetiminde TemplateField bir resim görüntüler. |
| TemplateField | Görüntüler kullanıcı tanımlı içerik DetailsView denetiminde belirtilen bir şablona göre bir satır için. Bu satır alan türü, bir özel satır alan oluşturmanızı sağlar. |

Varsayılan olarak, AutoGenerateRows ayarlandığı **true**, otomatik olarak oluşturduğu bir ilişkili satır alan nesne bağlanabilir türünün her alanı için veri kaynağı. Geçerli bağlanabilir dize, DateTime, ondalık, GUID ve ilkel türleri türleridir. Her bir alan metin olarak her alan veri kaynağında görüntülendiği sırada bir satır sonra görüntülenir.

Otomatik olarak satırları oluşturma, kayıttaki her alan görüntülemek için hızlı ve kolay bir yol sağlar. Ancak DetailsView yararlanması için denetimin özelliklerini açıkça DetailsView denetiminde TemplateField eklenecek satır alanlar bildirmeniz gerekir Gelişmiş. Satır alanları bildirmek için ilk olarak **AutoGenerateRows** özelliğini **false**. Ardından, açılış ve kapanış ekleyin **&lt;alanları&gt;** açılış ve kapanış etiketlerinin DetailsView denetiminde arasında etiketler. Son olarak, açılış ve kapanış arasında dahil etmek istediğiniz satır alanları listesinde **&lt;alanları&gt;** etiketler. Belirtilen satır alanları listelendikleri sırada alanlar koleksiyona eklenir. **Alanları** koleksiyon DetailsView denetiminde bulunan satır alanlarının programlı bir şekilde yönetmenizi sağlar.

> [!NOTE]
> Satır alanları alanlar koleksiyona eklenmez otomatik olarak oluşturulur.


## <a name="binding-to-data"></a>Verilere Bağlama

DetailsView denetiminde bir veri kaynağı denetimi gibi bağlanabilir **SqlDataSource** veya AccessDataSource, veya gibi System.Data.DataView, System.Collections.IEnumerable arabirimi uygulayan herhangi bir veri kaynağı System.Collections.ArrayList ve System.Collections.Hashtable.

DetailsView denetiminde uygun bir veri kaynağı türüne bağlamak için aşağıdaki yöntemlerden birini kullanın:

- Bir veri kaynak denetimine bağlamak için DataSourceID özelliği DetailsView denetimi veri kaynağı denetimini kimliği değerini ayarlayın. DetailsView denetiminde otomatik olarak belirtilen veri kaynak denetimine bağlar. Bu verilere bağlamak için tercih edilen yöntemdir.
- Uygulayan bir veri kaynağına bağlamak için **System.Collections.IEnumerable** arabirim program aracılığıyla veri kaynağına DetailsView denetiminde veri kaynağı özelliği ayarlayabilir ve ardından DataBind yöntemi çağırın.

## <a name="security"></a>Güvenlik

Bu denetim, kullanıcı girişi, kötü amaçlı istemci komut dosyası içerebilir görüntülemek için kullanılabilir. Bir istemciden yürütülebilir komut dosyası, SQL deyimleri ya da başka bir kod için uygulamanızda görüntülemeden önce gönderilen tüm bilgiler kontrol edin. ASP.NET, bir giriş isteği doğrulama özelliği blok betiği ve HTML kullanıcı girişi sağlar.

## <a name="data-operations"></a>Veri İşlemleri

DetailsView denetiminde, kullanıcının güncelleştirme, silme, ekleme ve denetimindeki öğeler arasında sayfasında olanak tanıyan yerleşik özellikleri sağlar. DetailsView denetiminde veri kaynak denetimine bağlı olduğunda DetailsView denetiminde denetimin özellikleri veri kaynağı yararlanabilir ve otomatik güncelleştirme, silme, ekleme ve işlevsellik sayfalama sağlayın.

DetailsView denetiminde, güncelleştirme, silme, ekleme ve sayfalandırma işlemleri ile diğer veri kaynağı türleri için destek sağlayabilirsiniz; Ancak, bu işlemlerin uygun Olay işleyicisindeki uygulamasını sağlamanız gerekir.

DetailsView denetiminde otomatik olarak ekleyebilirsiniz bir **CommandField** düğmesiyle AutoGenerateEditButton,AutoGenerateDeleteButtonveyaAutoGenerateInsertButtonözellikleriniayarlayarakbirdüzenleme,silmeveyayenisatıralan**true**sırasıyla. Silme aksine düğmesine (seçili kaydın hemen silen), düzenleme veya yeni düğmesine tıklandığında, düzenleme denetimi bunlara DetailsView veya modu, sırasıyla ekleyin. Düzenleme modunda Düzenle düğmesini iptal düğmesi ve bir güncelleştirme ile değiştirilir. Alanın veri türünü (örneğin, TextBox veya bir CheckBox denetimi) için uygun olan giriş denetimleri ile değiştirmek kullanıcı için bir alanın değeri görüntülenir. İptal düğmesine tıklayarak tüm değişiklikleri siler ancak güncelleştir düğmesine tıklayarak veri kaynağındaki kaydı güncelleştirir. Benzer şekilde, ekleme modunda yeni düğme ekleme ve bir iptal düğmesi ile değiştirilir ve boş giriş denetimleri için yeni kayıt için değerleri girmesini görüntülenir.

DetailsView denetiminde, kullanıcının diğer veri kaynağındaki kayıtları gitmek bir disk belleği özelliği sağlar. Etkin olduğunda, sayfa gezinti denetimlerinin bir çağrı satır görüntülenir. Disk belleğini etkinleştirmek için AllowPagingözelliðini ayarlamak **true**. Çağrı satır PagerStyle ve PagerSettings özellikleri kullanılarak özelleştirilebilir.

## <a name="customizing-the-user-interface"></a>Kullanıcı Arabirimini Özelleştirme

Denetimin farklı bölümlerine stil özelliklerini ayarlayarak DetailsView denetiminde görünümünü özelleştirebilirsiniz. Aşağıdaki tabloda, farklı bir stil özellikleri listeler.

| **Stil özelliği** | **Açıklama** |
| --- | --- |
| AlternatingRowStyle | DetailsView denetiminde değişen veri satırları için stilini ayarlar. Bu özelliği ayarlandığında, veri satırları RowStyle ayarları arasında değişen görüntülenir ve **AlternatingRowStyle** ayarları. |
| CommandRowStyle | DetailsView denetiminde yerleşik komut düğmeleri içeren satır için stilini ayarlar. |
| EditRowStyle | DetailsView denetiminde olduğunda veri satırları için stil ayarlarını düzenleme modu. |
| EmptyDataRowStyle | DetailsView denetiminde TemplateField veri kaynağı herhangi bir kayıt olmadığında görüntülenen boş veri satırı için stilini ayarlar. |
| FooterStyle | DetailsView denetiminde alt bilgi satırının stilini ayarlar. |
| HeaderStyle | DetailsView denetiminde üst bilgi satırının stilini ayarlar. |
| InsertRowStyle | DetailsView denetiminde olduğunda veri satırları için stil ayarlarını modu ekleyin. |
| PagerStyle | DetailsView denetiminde çağrı satırının stilini ayarlar. |
| RowStyle | DetailsView denetiminde veri satırları için stilini ayarlar. Zaman **AlternatingRowStyle** özelliği ayarlanmışsa, veri satırları arasında değişen görüntülenen **RowStyle** ayarları ve **AlternatingRowStyle** ayarları. |
| FieldHeaderStyle | DetailsView denetiminde üst sütun stilini ayarlar. |

## <a name="events"></a>Olaylar

DetailsView denetiminde programlayabileceğiniz çeşitli olayları sağlar. Bu, her bir olay gerçekleştiğinde, özel bir yordamı çalıştırmak sağlar. Aşağıdaki tabloda DetailsView denetimi tarafından desteklenen olayları listeler. DetailsView denetiminde kendi temel sınıftan de bu olayları devralır: veri bağlama, veri bağlama, Disposed, başlatma, yük, PreRender ve işleme.

| **Olay** | **Açıklama** |
| --- | --- |
| ItemCommand | DetailsView denetiminde TemplateField bir düğme tıklandığında gerçekleşir. |
| ItemCreated | DetailsView denetiminde TemplateField tüm DetailsViewRow nesneler oluşturulduktan sonra gerçekleşir. Bu olay, genellikle gösterilmeden önce bir kayıt değerlerini değiştirmek için kullanılır. |
| ItemDeleted | Sil düğmesine tıklandığında, ancak DetailsView denetiminde kayıt veri kaynağından sildikten sonra gerçekleşir. Bu olay, genellikle sonuçlarını silme işlemini denetlemek için kullanılır. |
| ItemDeleting | Sil düğmesine tıklandığında, ancak önce DetailsView denetimi kaydı veri kaynağından siler gerçekleşir. Bu olay, genellikle silme işlemini iptal etmek için kullanılır. |
| ItemInserted | Ekle düğmesine tıklandığında, ancak kayıt DetailsView denetimi ekledikten sonra gerçekleşir. Bu olay sık sık ekleme işleminin sonuçlarını denetlemek için kullanılır. |
| ItemInserting | Ekle düğmesine tıklandığında, ancak önce DetailsView denetimi kaydı ekler gerçekleşir. Bu olay sık sık ekleme işlemi iptal etmek için kullanılır. |
| Itemupdated | Güncelleştir düğmesine tıklandığında, ancak satır DetailsView denetiminde güncelleştirmeler sonra oluşur. Bu olay sık sık güncelleştirme işlemi sonuçlarını denetlemek için kullanılır. |
| ItemUpdating | Güncelleştir düğmesine tıklandığında, ancak önce DetailsView denetimi kaydı güncelleştirir gerçekleşir. Bu olay sık sık güncelleştirme işlemi iptal etmek için kullanılır. |
| ModeChanged | DetailsView denetiminde modları (düzenleme, ekleme veya salt okunur modda) olarak değiştirdikten sonra gerçekleşir. Bu olay, genellikle DetailsView denetiminde modu değiştiğinde bir görevi gerçekleştirmek için kullanılır. |
| ModeChanging | DetailsView denetiminde modları (düzenleme, ekleme veya salt okunur modda) değiştirmeden önce gerçekleşir. Bu olay, genellikle bir modu değişikliği iptal etmek için kullanılır. |
| PageIndexChanged | Çağrı düğmelerden birine tıklandığında, ancak disk belleği işlemi DetailsView denetiminde işleme sonra gerçekleşir. Bu olay, kullanıcı denetiminde farklı bir kayda gider sonra bir görev gerçekleştirmeniz gerektiğinde, yaygın olarak kullanılır. |
| PageIndexChanging | Çağrı düğmelerden birine tıklandığında, ancak önce DetailsView denetimi disk belleği işlemi işleme gerçekleşir. Bu olay, genellikle disk belleği işlemi iptal etmek için kullanılır. |

## <a name="the-menu-control"></a>Menü denetimi

Menü denetimi ASP.NET 2.0, tam özellikli Gezinti sistem olacak şekilde tasarlanmıştır. Bu sınırlama SiteMapDataSource gibi hiyerarşik veri kaynaklarına kolayca olabilir.

Bir menü denetimleri yapısı bildirimli olarak veya dinamik olarak tanımlanabilir ve tek bir kök düğümde ve tüm alt düğümleri sayısı oluşur. Aşağıdaki kod, bir menü denetimi menüsünü bildirimli olarak tanımlar.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Yukarıdaki örnekte, kök düğümü Home.aspx düğümüdür. Çeşitli düzeylerde kök düğümü içindeki tüm diğer düğümlere yerleştirilir.

Menü denetimi işleyebilen menüleri iki tür vardır; statik menüleri ve dinamik menü. Statik menüler, her zaman görünür olduğu menü öğelerinin oluşur. Yalnızca kullanıcı bunlar üzerinde fareyle geldiğinde görünür olan menü öğelerinin dinamik menüler oluşur. Müşteriler genellikle bildirimli olarak tanımlanan menülerle statik menüleri ve menülerle çalışma zamanında databound olan dinamik menüler karışıklığa neden olabilir. Aslında, dinamik ve statik menüleri popülasyon yönteme ilgili değildir. Koşulları *statik* ve *dinamik* yalnızca olup olmadığını menü statik varsayılan olarak görüntülenen veya yalnızca için kullanıcı bazı eylemde bulunduğunda görüntülenme bakın.

**StaticDisplayLevels** özelliği, statik ve bu nedenle, varsayılan olarak görüntülenen menüyü kaç düzeyde yapılandırmak için kullanılır. Yukarıdaki örnekte ayarlama **StaticDisplayLevels** 2 değerini özelliği ana düğümü, müzik düğüm ve filmler düğümünü statik olarak görüntülenecek menü neden. Kullanıcı üst düğümün üzerine geldiğinde tüm diğer düğümlere dinamik olarak görüntülenir.

**MaximumDynamicDisplayLevels** özelliği yapılandırır en fazla dinamik düzeylerini menü görüntüleyebilen. Dinamik menüler tarafından belirtilen değerden daha yüksek bir düzeyde **MaximumDynamicDisplayLevels** özelliği atılır.

> [!NOTE]
> Bu durumlarda menülerin nedeniyle MaximumDynamicDisplayLevels özelliği işlemek için burada görünmez karşılaşabileceğiniz neredeyse kesindir. Bu gibi durumlarda, özellik müşteriler menülerin görüntülenmesini için izin vermek için yeterince ayarlandığından emin olun.


## <a name="data-binding-the-menu-control"></a>Veri bağlama menü denetimi

Menü denetimi SiteMapDataSource veya XmlDataSource'ta gibi bir hiyerarşik veri kaynağına bağlanabilir. SiteMapDataSource en yaygın olarak yöntemi Menü denetimine veri bağlama için dışına birtakım dosya akışları ve bilinen bir menü denetimi API'sine şemasına sağlar çünkü kullanılır. Listenin altındaki basit birtakım dosyası gösterir.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Yalnızca bir kök siteMapNode öğesi, bu durumda, giriş öğesi olduğuna dikkat edin. Bazı öznitelikler için her siteMapNode yapılandırılabilir. En yaygın olarak kullanılan öznitelikler şunlardır:

- **URL** kullanıcı menü öğesi tıkladığında görüntülenecek URL'sini belirtir. Bu öznitelik mevcut değilse düğüm yalnızca geri tıklandığında deftere.
- **Başlık** menü öğesini görüntülenen metni belirtir.
- **Açıklama** Belgeler düğümü için kullanılır. Ayrıca, fare, düğümün üzerine gelindiğinde Araç İpucu olarak görüntüler.
- **siteMapFile** iç içe geçmiş Site Haritaları için izin verir. Bu öznitelik, doğru biçimlendirilmiş bir ASP.NET site haritası dosyasına işaret etmelidir.
- **rolleri** ASP.NET güvenlik kırpma tarafından denetlenmesi için bir düğüm için görünümünü sağlar.

Bu öznitelikler tüm isteğe bağlı olsa da, menü davranışını yok belirtilirse beklenen olmayabileceğini unutmayın. Örneğin, varsa *url* özniteliği ancak *açıklama* özniteliği değil, düğüm görünür durumda değildir ve belirtilen URL'ye mümkün olacaktır.

## <a name="controlling-a-menus-operation"></a>Menüler işlemi denetleme

Bir ASP.NET menü denetimi işleyişini etkileyen çeşitli özellikler vardır. **yönlendirme** özelliği **DisappearAfter** özelliği **StaticItemFormatString** özelliği ve **StaticPopoutImageUrl**özelliği olan bunlardan yalnızca birkaçıdır.

- **Yönlendirme** olarak ayarlanabilir *yatay* veya *dikey* ve statik menü öğeleri yatay veya dikey satırda düzenlendiği ve bağlı Yığılmış denetler başka bir. Bu özellik, dinamik menüler etkilemez.
- **DisappearAfter** özelliği yapılandırır fare sonra ne kadar bir Dinamik menü görünür kalmalıdır uzağa taşınması. Değeri milisaniye ve 500 varsayılan olarak belirtilir. Bu özelliğin değerini -1 olarak ayarlanması, menüsünün hiçbir zaman otomatik olarak kaybolmasına neden olur. Bu durumda, kullanıcı menü dışında tıkladığında menü yalnızca kaybolur.
- **StaticItemFormatString** özelliği tutarlı duyuruları menü sisteminizdeki bakımını yapmayı kolaylaştırır. Bu özelliği belirtirken *{0}* veri kaynağında açıklamasını yerine girilmesi gerekir. Örneğin, menü öğesi alıştırma 1 say Mız ürün sayfasını ziyaret edin, vb. sahip olmak için şu adresi ziyaret edin belirtebilirdiniz bizim {0} StaticItemFormatString sayfası. Çalışma zamanında tüm oluşumlarıyla ASP.NET değiştirecek {0} menü öğesi için doğru açıklaması.
- **StaticPopoutImageUrl** özelliği, belirli menü düğümü üzerine gelerek erişilebilir alt düğümleri olduğunu göstermek için kullanılan görüntüyü belirtir. Dinamik menüler, varsayılan görüntü kullanmaya devam eder.

## <a name="templated-menu-controls"></a>Şablonlu menü denetimleri

Menü denetimi, şablonlu denetim ve için iki farklı öğe şablonları sağlar; StaticItemTemplate'i ve DynamicItemTemplate'i. Bu şablonları kullanarak kolayca sunucu veya kullanıcı denetimleri, menülere ekleyebilirsiniz.

Visual Studio .NET şablonlarında düzenlemek için akıllı etiket menüsünde düğmesini ve düzenleme şablonlarını seçin. Ardından StaticItemTemplate'i veya DynamicItemTemplate'i düzenleme arasında seçim yapabilirsiniz.

Sayfa yüklendiğinde StaticItemTemplate'e eklenen herhangi bir denetim statik menüsünde görünür. DynamicItemTemplate'e eklenen herhangi bir denetim tüm açılan menülerde görünür.

## <a name="menu-events"></a>Menü olayları

Menü denetimi için benzersiz olan iki olay; yine de sahip istiyor musunuz? **MenuItemClicked** ve **MenuItemDatabound** olay.

Bir menü öğesi tıklatıldığında MenuItemClicked olay tetiklenir. Bir menü öğesi veriye bağlı olduğunda MenuItemDatabound olay tetiklenir. **MenuEventArgs** yapan için olay işleyicisi Item özelliği aracılığıyla menü öğesine erişim sağlar.

## <a name="controlling-a-menus-appearance"></a>Menüler görünümü denetleme

Ayrıca, bir veya daha fazla biçim menülere kullanılabilir birçok stilleri kullanarak bir menü denetimi görünümünü etkileyebilir. Bunlar arasında **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**ve **DynamicHoverStyle**. Bu özellikler, standart bir HTML stil dizesi kullanılarak yapılandırılır. Örneğin, aşağıdaki dinamik menüler stilini etkiler.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Vurgulu stillerin kullanıyorsanız eklemeniz gerekecektir bir &lt;baş&gt; öğesinde sayfasına *runat* öğesi kümesine *sunucu*.


Menü denetimleri de ASP.NET 2.0 Temalar kullanımını destekler.

## <a name="the-treeview-control"></a>TreeView denetimi

TreeView denetimi verileri gibi ağaç yapısında görüntüler. Menü denetimi gibi ile kolayca veri SiteMapDataSource gibi bir hiyerarşik veri kaynağına bağımlı olabilir.

Müşteriler, ASP.NET 2.0 TreeView denetimi ile ilgili sorun büyük olasılıkla sorunun ilk olup olmadığı için ASP.NET kullanılabilir TreeView IE WebControl ilişkili olduğu 1.x. Bu değil. ASP.NET 2.0 TreeView denetimi sıfırdan yazılmıştır ve önceden kullanılabilen IE TreeView WebControl üzerinde önemli bir iyileştirme sunar.

Ayrıntıya menü denetimi ile aynı şekilde gerçekleştirildiği TreeView denetimi için bir site haritası bağlama konusunda gitmiyor. Ancak, TreeView denetimi çalışır bir şekilde ayrı bazı farklar vardır.

Varsayılan olarak, bir TreeView denetimi tamamen genişletilmiş görünür. İlk yükleme sırasında genişletme düzeyini değiştirmek için değiştirme **ExpandDepth** denetiminin özelliği. Bu, özellikle TreeView databound belirli düğümlerini genişleterek bağlı olduğu durumlarda önemlidir.

## <a name="databinding-the-treeview-control"></a>TreeView denetiminde veri bağlama

Menü denetimi, ağaç görünümünde kendisi de büyük miktarlarda veri işleme için uygundur. Bu nedenle SiteMapDataSource veya XMLDataSource veri bağlama ek olarak, ağaç görünümünde genellikle bir veri kümesi veya diğer ilişkisel verileri bağlı veri'gereklidir. TreeView denetimi için büyük miktarlarda verinin nerede bağlı durumda, aslında denetimde görünen veri bağlamak en iyisidir. TreeView düğümleri genişletilmiş gibi ek veriler veri bağlama sonra kullanabilirsiniz.

Bu gibi durumlarda, **PopulateOnDemand** özelliği TreeView ayarlanmalıdır *true*. Ardından bir uygulama için sağlamanız gerekir **TreeNodePopulate** yöntemi.

## <a name="data-binding-without-postback"></a>Veri bağlama geri gönderme

Önceki örnekte bir düğümü ilk kez genişlettiğinizde, sayfanın geri gönderir ve yeniler olduğuna dikkat edin. Thats Bu örnek, ancak sorun değil, bir üretim ortamında ile büyük miktarda veri olabilir hayal edebilirsiniz. Daha iyi bir senaryo biri, ağaç görünümünde yine de dinamik olarak düğümlerini doldurmak, ancak olmadan bir post, sunucuya geri göndermek olacaktır.

Ayarlayarak **PopulateNodesFromClient** ve **PopulateOnDemand** özelliklerini true olarak ASP.NET TreeView denetimi dinamik olarak tekrar olmadan düğümleri doldurmak. Üst düğüm genişletildiğinde istemci tarafından bir XMLHttp istekte ve OnTreeNodePopulate olay tetiklenir. Ardından verileri için kullanılan bir XML veri adası ile sunucu yanıt alt düğümleri bağlayın.

ASP.NET dinamik olarak bu işlevselliğini uygular istemci kodu oluşturur. &lt;Betik&gt; betiği içeren etiketler, bir AXD dosyasına işaret eden oluşturulur. Örneğin, listenin altındaki XMLHttp isteği oluşturan kodu için kod bağlantıları gösterir.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Tarayıcınızda AXD dosyanın üstüne gidin ve açmak XMLHttp istek uygulayan kod görürsünüz. Bu yöntem, müşterilerin betiğin değiştirmelerini engeller.

## <a name="controlling-the-operation-of-the-treeview-control"></a>TreeView denetimi işlemini denetleme

TreeView denetimi, denetimin çalışmasını etkileyen birçok özelliğe sahiptir. En belirgin özellikleri **ShowCheckBoxes**, **ShowExpandCollapse**, ve **ShowLines**.

**ShowCheckBoxes** özellik işlendiğinde bir onay kutusu düğüm Göster olup olmadığını etkiler. Bu özellik için geçerli değerler **hiçbiri**, **kök**, **üst**, **yaprak**, ve **tüm**. Bunlar gibi TreeView denetimini etkiler:

| **Özellik değeri** | **Etkin** |
| --- | --- |
| Yok. | Tüm düğümlerde onay kutuları görüntülenmez. Varsayılan ayar budur. |
| Kök | Bir onay kutusu yalnızca kök düğümde görüntülenir. |
| Üst | Bir onay kutusu yalnızca alt düğüm varsa bu düğümler üzerinde görüntülenir. Bu alt düğümlerin, üst düğümleri veya yaprak düğümleri olabilir. |
| Yaprak | Alt düğümler olmadan olan bu düğümlerde bir onay kutusu görüntülenir. |
| Tümü | Tüm düğümlerde bir onay kutusu görüntülenir. |

Onay kutularını kullanıldığında **CheckedNodes** özelliği geri gönderme sırasında denetlenir TreeView düğümlerin koleksiyonunu döndürür.

**ShowExpandCollapse** özelliği, kök ve üst düğümleri yanındaki genişletme/daraltma görüntünün görünümünü denetler. Bu özellik ayarlanırsa **false**, ağaç düğümleri köprü olarak işlenir ve bağlantıya tıklayarak Genişlet/Daralt.

**ShowLines** özellik denetler üst düğümleri alt düğümlerine bağlanma satırları olup olmadığını görüntülenir. Zaman **false** (varsayılan), satır görüntülenir. Zaman **true**, TreeView denetimi tarafından belirtilen klasörde satırları görüntülerini kullanacak **LineImagesFolder** özelliği.

TreeView satırları görünümünü özelleştirmek için Visual Studio .NET 2005 bir satır Tasarımcısı araç içerir. TreeView denetimi akıllı etiket düğmeyi kullanarak bu aracına erişebilirsiniz.


![](data-bound-controls/_static/image1.jpg)

**Şekil 1**


Seçtiğinizde **çizgi görüntülerini özelleştirme** menü seçeneğini satırı Tasarımcısı araç TreeView satırları görünümünü yapılandırmanıza olanak sağlayan başlatılır.

## <a name="treeview-events"></a>TreeView olayları

TreeView denetimi aşağıdaki benzersiz olaylar vardır:

- Bir düğümü seçildiğinde SelectedNodeChanged gerçekleşir dayalı **SelectAction** özelliği.
- TreeNodeCheckChanged düğümler checkboxs durumu değiştiğinde gerçekleşir.
- Bir düğüm genişletildiğinde TreeNodeExpanded gerçekleşir dayalı **SelectAction** özelliği.
- Bir düğüm daraltıldığında TreeNodeCollapsed gerçekleşir.
- Bağlı veri bir düğümü olduğu TreeNodeDataBound gerçekleşir.
- Bir düğüm doldurulur TreeNodePopulate gerçekleşir.

**SelectAction** özelliği bir düğümü seçildiğinde hangi olay tetiklenir yapılandırmanıza olanak tanır. Aşağıdaki eylemleri SelectAction özelliği sağlar:

- TreeNodeSelectAction.Expand başlatır düğümü seçildiğinde TreeNodeExpanded.
- TreeNodeSelectAction.None düğümü seçildiğinde bir olay başlatır.
- TreeNodeSelectAction.Select düğümü seçildiğinde SelectedNodeChanged olayını başlatır.
- TreeNodeSelectAction.SelectExpand SelectedNodeChanged olayı hem düğümü seçildiğinde TreeNodeExpanded olayını başlatır.

## <a name="controlling-appearance-with-styles"></a>Stiller görünümü denetleme

Ağaç denetimi stilleri bir denetimin görünümünü kontrol etmek için birçok özellik sağlar. Aşağıdaki özellikler kullanılabilir.

| **Özellik adı** | **Denetimler** |
| --- | --- |
| HoverNodeStyle | Fareyi üzerine gelindiğinde düğümlerini stilini denetler. |
| LeafNodeStyle | Yaprak düğümlerini stilini denetler. |
| NodeStyle | Tüm düğümleri stili denetler. Belirli bir düğümün stilleri (örneğin, LeafNodeStyle) bu stil geçersiz kılar. |
| ParentNodeStyle | Tüm üst düğümleri stili denetler. |
| RootNodeStyle | Kök düğüm stili denetler. |
| SelectedNodeStyle | Seçili düğüm stili denetler. |

Bu özelliklerin her biri, salt okunur. Ancak, bunlar her iade olacak bir **TreeNodeStyle** nesneyi ve nesnenin özelliklerini kullanarak değiştirilebilir *özelliği alt özellik* biçimi. Örneğin, ayarlanacak **ForeColor** özelliği **SelectedNodeStyle**, şu sözdizimini kullanmanız gerekir:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Yukarıdaki etiketi kapatılmamış dikkat edin. Burada gösterilen bildirim temelli söz dizimi kullanırken TreeViews düğümleri de HTML kodunda verilebilir olmasıdır.

Stil özellikleri de kullanarak kod içinde belirtilebilir *property.subproperty* biçimi. Örneğin, ayarlanacak **ForeColor** özelliği **RootNodeStyle** kodda, aşağıdaki sözdizimini kullanın:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Farklı bir stil özellikleri kapsamlı bir listesi için TreeNodeStyle nesnede MSDN belgelerine bakın.


## <a name="the-sitemappath-control"></a>SiteMapPath denetimi

SiteMapPath denetimi, ASP.NET geliştiricilerine yönelik ekmek yakınında gezinme kontrolü sağlar. Başka gezinme denetimleri gibi veri kaynakları SiteMapDataSource veya XmlDataSource gibi hiyerarşik verilere bağlı kolayca olabilir.

Bir SiteMapPath denetimi SiteMapNodeItem nesnelerin oluşur. Düğümler üç tür vardır; Kök düğümü, üst düğümleri ve geçerli düğüm. Kök hiyerarşik yapının üst düğümü düğümüdür. Geçerli düğüm geçerli sayfayı temsil eder. Diğer tüm düğümlere üst düğümlerdir.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>SiteMapPath denetimi işlemini denetleme

SiteMapPath denetimi işleyişini denetleyen özellikler aşağıdaki gibidir:

| **Özelliği** | **Özellik açıklaması** |
| --- | --- |
| ParentLevelsDisplayed | Kaç üst düğümleri görüntülenme şeklini denetler. Varsayılan değer üst düğümleri görüntülenen sayısına kısıtlama getirir -1 ' dir. |
| PathDirection | Bir SiteMapPath yönünü denetler. Geçerli değerler şunlardır: (varsayılan) RootToCurrent ve CurrentToRoot. |
| PathSeparator | Bir SiteMapPath denetimi düğümlerini ayıran karakter denetleyen bir dize. Varsayılan değer:. |
| RenderCurrentNodeAsLink | Geçerli düğüm bir bağlantı olarak işlenen olup olmadığını denetleyen bir Boole değeri. Varsayılan değer false'tur. |
| SkipLinkText | Ekran okuyucular tarafından sayfası görüntülendiğinde erişilebilirlik yardımcı olur. Bu özellik ekran okuyucular SiteMapPath denetimi atlamayı sağlar. Bu özelliği devre dışı bırakmak için String.Empty özelliğini ayarlayın. |

## <a name="templated-sitemappath-controls"></a>Şablonlu SiteMapPath denetimleri

SiteMapControl şablonlu bir denetimdir ve denetim görüntülenmesinde farklı şablonu kullanmak için bu nedenle, tanımlayabilirsiniz. Bir SiteMapPath denetimi şablonlarında düzenlemek için Denetim akıllı etiket düğmesine tıklayın ve menüden şablonları düzenleyin. Kullanılabilir farklı şablonlar seçebileceğiniz aşağıda gösterildiği gibi bu SiteMapTasks menü görüntüler.


![](data-bound-controls/_static/image2.jpg)

**Şekil 2**


**NodeTemplate** şablon SiteMapPath herhangi bir düğüm gösterir. Düğüm bir kök düğümü geçerli düğüm olup olmadığını ve **RootNodeTemplate** veya **CurrentNodeTemplate** olan yapılandırılmış, NodeTemplate geçersiz kılınır.

## <a name="sitemappath-events"></a>SiteMapPath olayları

Denetim sınıfından türetilmemiş iki olay SiteMapPath denetimi vardır. **ItemCreated** olay ve **ItemDataBound** olay. Bir SiteMapPath öğe oluşturulduğunda ItemCreated olay tetiklenir. Bir SiteMapPath düğüm veri bağlama sırasında DataBind yöntemi çağrıldığında ItemDataBound ortaya çıkar. A **SiteMapNodeItemEventArgs** nesne öğesi özelliği yoluyla belirli SiteMapNodeItem erişim sağlar.

## <a name="controlling-appearance-with-styles"></a>Stiller görünümü denetleme

Aşağıdaki stilleri bir SiteMapPath denetimi biçimlendirme için kullanılabilir.

| **Özellik adı** | **Denetimler** |
| --- | --- |
| CurrentNodeStyle | Geçerli düğüm için metin stili denetler. |
| RootNodeStyle | Kök düğümü için metin stili denetler. |
| NodeStyle | Tüm düğümleri CurrentNodeStyle veya RootNodeStyle uygulanmaz varsayılarak metni stilini denetler. |

NodeStyle özelliği CurrentNodeStyle veya RootNodeStyle tarafından geçersiz kılınır. Bu özelliklerin her birini salt okunur ve döndüren bir **stili** nesne. Bu özelliklerden birini kullanarak bir düğümü görünümünü etkilemek için döndürülen stil nesnesini özelliklerini ayarlamanız gerekir. Örneğin, aşağıdaki kod, geçerli düğümünün forecolor özelliği değiştirir.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

Özelliği de gösterildiği gibi program aracılığıyla uygulanabilir:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Bir şablonu uygulanmışsa, stili uygulanmaz.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Laboratuvar 1: bir ASP.NET menü denetimi yapılandırma

1. Yeni bir Web sitesi oluşturun.
2. Dosya, yeni dosya ve Site Haritası dosya şablonları listesinden seçerek bir Site haritası dosyası ekleyin.
3. Site haritası (varsayılan olarak birtakım) açın ve liste gibi görünüyor şekilde değiştirin. Site haritası bağlama sayfaları gerçekten var, ancak bu alıştırma için bir sorun olmayacaktır.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Varsayılan Web formunun Tasarım Görünümü'nde açın.
5. Araç Kutusu Gezinti bölümünden sayfasına yeni bir menü denetimi ekleyin.
6. Araç kutusu veri bölümünden yeni SiteMapDataSource ekleyin. SiteMapDataSource, sitenizdeki birtakım dosya otomatik olarak kullanır. (Birtakım dosya *gerekir* sitenin kök klasörü içinde.)
7. Menü denetimine tıklayın ve ardından menü görevleri iletişim kutusu görüntülemek için akıllı etiket düğmesine tıklayın.
8. Veri Kaynağı Seç açılan listede SiteMapDataSource1 seçin.
9. AutoFormat bağlantısına tıklayın ve bir biçim menüsünü seçin.
10. Özellikler bölmesinde **StaticDisplayLevels** özelliği 2. Menü denetimi, artık tasarımcıda giriş, ürünler ve hizmetler düğüm görüntülemelidir.
11. Menü kullanabilmek için tarayıcınızı sayfasında göz atın. (Önceden site haritaya eklenen sayfaları gerçekten mevcut olduğundan, deneyin ve onlara göz atın, bir hata görürsünüz.)

StaticDisplayLevels ve MaximumDynamicDisplayLevels özelliklerini değiştirme ile denemeler yapın ve bunlar menüsünün nasıl oluşturulacağını nasıl etkilediğini görün.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Laboratuvar 2: Bir TreeView denetimi dinamik olarak bağlama

Bu alıştırmada, yerel olarak çalışan SQL Server sahip olduğunuzu ve Northwind veritabanının SQL Server örneğinde mevcut olduğunu varsayar. Lütfen bu koşullar karşılanmazsa, örnek bağlantı dizesini değiştirin. Aynı zamanda güvenilir bir bağlantı yerine SQL Server kimlik doğrulaması belirtmek gerekebileceğini unutmayın.

1. Yeni bir Web sitesi oluşturun.
2. Default.aspx kod görünümüne geçin ve tüm kod aşağıda kod ile değiştirin. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Sayfayı treeview.aspx kaydedin.
4. Sayfaya göz atın.
5. Sayfanın ilk kez görüntülendiğinde, tarayıcınızda sayfa kaynağı görüntüleyin. Yalnızca görünür düğümleri istemciye gönderilen unutmayın.
6. Herhangi bir düğümün yanındaki artı işaretine tıklayın.
7. Sayfasında yeniden kaynağı görüntüle. Yeni düğüm mevcut olduğuna dikkat edin.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Laboratuvar 3: GridView ve DetailsView kullanarak veri düzenleme ve görünümü ayrıntıları

1. Yeni bir Web sitesi oluşturun.
2. Yeni bir web.config Web sitesine ekleyin.
3. Bir bağlantı dizesi aşağıda gösterildiği gibi web.config dosyasına ekleyin: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Ortamınıza temel bağlantı dizesini değiştirmeniz gerekebilir.
4. Web.config dosyasını kaydedip kapatın.
5. Default.aspx açın ve yeni bir SqlDataSource denetimi ekleyin.
6. SqlDataSource denetimi Kimliğini değiştirme **ürünleri**.
7. İçinde **SqlDataSource görevleri** menüsünde tıklatın **veri kaynağı yapılandırma**.
8. Seçin **Northwind** bağlantı açılır ve İleri'ye tıklayın.
9. Seçin **ürünleri** gelen **adı** açılır ve **ProductID**, **ProductName**, **UnitPrice**, ve **unitsInStock** aşağıda gösterildiği gibi onay kutularını. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. **İleri**'ye tıklayın.
11. **Son**'a tıklayın.
12. Kaynak görünümüne geçin ve oluşturulan kodu inceleyin. Bildirim **SelectCommand**, **DeleteCommand**, **InsertCommand**, ve **UpdateCommand** SqlDataSource için eklendi denetimi. Ayrıca, eklenen parametreleri dikkat edin.
13. Tasarım görünümüne geçin ve yeni bir GridView denetimi sayfaya ekleyin.
14. Seçin **ürünleri** gelen **veri kaynağı Seç** açılır.
15. Denetleme **etkinleştirme sayfalama** ve **Seçimi Etkinleştir** aşağıda gösterildiği gibi. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Tıklayın **sütunları Düzenle** emin olun ve bağlama **alanları otomatik olarak oluşturmak** denetlenir.
17. **Tamam**'ı tıklatın.
18. GridView denetimi seçili durumdayken yanındaki düğmeye tıklayın **DataKeyNames** özelliği Özellikler bölmesinde.
19. Seçin **ProductID** gelen **uygun veri alanlarını** listelemek ve tıklayın **&gt;** düğmesi ekleyin.
20. Tamam'a tıklayın.
21. Yeni bir SqlDataSource denetimi sayfasına ekleyin.
22. SqlDataSource denetimi Kimliğini değiştirme **ayrıntıları**.
23. SqlDataSource Görevler menüsünden **veri kaynağı yapılandırma**.
24. Seçin **Northwind** açılır ve **sonraki**.
25. Seçin <strong>ürünleri</strong> gelen <strong>adı</strong> açılır ve <strong> \</ strong > * onay kutusu <strong>sütunları</strong> listbox.
26. Tıklayın **burada** düğmesi.
27. Seçin **ProductID** gelen **sütun** açılır.
28. Seçin **=** işleci açılır.
29. Seçin **denetimi** gelen **kaynak** açılır.
30. Seçin **GridView1'i** gelen **denetim kimliği** açılır.
31. Tıklayın **Ekle** düğmesini WHERE yan tümcesi ekleyin.
32. **Tamam**'ı tıklatın.
33. Tıklayın **Gelişmiş** denetleyin ve düğme **Generate INSERT, UPDATE ve DELETE deyimleri** onay kutusu.
34. **Tamam**'ı tıklatın.
35. Tıklayın **sonraki** tıklatıp **son**.
36. Bir DetailsView denetimi sayfasına ekleyin.
37. İçinde **veri kaynağı Seç** açılır listesinde, seçin **ayrıntıları**.
38. Denetleme **düzenlemeyi etkinleştir** aşağıda gösterildiği gibi onay kutusu. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Sayfayı kaydedin ve Default.aspx göz atın.
40. Tıklayın **seçin** DetailsView güncelleştirmeyi otomatik olarak görmek için farklı kayıtlara yanındaki bağlantı.
41. Tıklayın **Düzenle** DetailsView denetiminde bağlantı.
42. Kayıt için bir değişiklik yapın ve tıklayın **güncelleştirme**.
