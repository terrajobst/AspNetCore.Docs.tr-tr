---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Bir ASP.NET sayfası (VB) BLL ve DAL düzeyi özel durumları işleme | Microsoft Docs
author: rick-anderson
description: Bu öğreticide bir özel durum ekleme, güncelleştirme veya silme işlemi, sırasında gerçekleşmesi gereken kolay, bilgilendirici hata iletisini görüntülemek nasıl göreceğiz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: b76554b6e8c00dbe3b33de8158b925d7314afb72
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Bir ASP.NET sayfası (VB) BLL ve DAL düzeyi özel durumları işleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) veya [PDF indirin](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> Bu öğreticide bir INSERT, update veya ASP.NET veri Web denetimi silme işlemi sırasında bir özel durum gerçekleşeceğini kolay, bilgilendirici hata iletisini görüntülemek nasıl göreceğiz.


## <a name="introduction"></a>Giriş

Bir Katmanlı Uygulama mimarisi kullanarak bir ASP.NET web uygulaması verilerle çalışma aşağıdaki üç genel adımları içerir:

1. İş mantığı katmanı, hangi yöntemin çağrılması gerekir ve geçirileceğini hangi parametre değerleri belirler. Parametre değerleri sabit kodlanmış, programlı olarak atanmış olabilir veya girişleri kullanıcı tarafından girilen.
2. Invoke yöntemi.
3. Sonuçları işleyin. Veri döndüren bir BLL yöntemi çağrılırken, bu verileri bir veri Web denetimi bağlama gerektirebilir. Verileri değiştirme BLL yöntemleri için bu bir dönüş değerini temel veya düzgün biçimde 2. adımda çıkan herhangi bir özel durum işleme bazı eylemler gerçekleştirme içerebilir.

İçinde gördüğümüz gibi [önceki öğretici](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md), ObjectDataSource ve Web denetimleri veri adımları 1 ve 3 için genişletilebilirlik noktaları sağlar. GridView, örneğin, harekete kendi `RowUpdating` alan değerlerini kendi ObjectDataSource için 's atama önce olay `UpdateParameters` koleksiyonu; kendi `RowUpdated` ObjectDataSource işlemi tamamlandıktan sonra olayı oluşturulur.

Biz zaten adım 1 sırasında yangın ve nasıl giriş parametreleri özelleştirmek veya işlemi iptal etmek için kullanılabilmesi için görülen olayları incelendi. Bu öğreticide biz uygulamamızla işlemi tamamlandıktan sonra harekete olayları kapatmanız. Bunun yanı sıra, geçebiliriz bu sonrası düzeyi olay işleyicileri ile işlem sırasında bir özel durum oluştu, belirlemek ve standart ASP.NET varsayarak yerine kolay, bilgilendirici hata iletisi ekranda görüntüleme kolaylıkla, idare özel durum sayfası.

Bu sonrası düzeyi olaylar ile çalışmayı göstermek için düzenlenebilir GridView ürünleri listeler bir sayfa oluşturalım. Bir özel durum ise, bir ürün güncelleştirme başlatıldığında bizim ASP.NET sayfası bir sorun oluştu açıklayan GridView yukarıda kısa bir ileti görüntüler. Haydi başlayalım!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>1. adım: ürünlerin düzenlenebilir bir GridView oluşturma

Önceki öğreticide düzenlenebilir GridView yalnızca iki alanlarla oluşturduğumuz `ProductName` ve `UnitPrice`. Bu gerekli bir ek aşırı için oluşturma `ProductsBLL` sınıfının `UpdateProduct` yöntemi, yalnızca üç giriş parametreleri (ürün adı, birim fiyat ve kimliği) her ürün alanı için karşılıklı olarak bir parametre kabul eden. Bu öğretici için ürün adı, miktar birimi, birim fiyat ve birim başına stokta görüntüler, ancak yalnızca adını, birim fiyat ve birimleri düzenlenmesi stokta verir düzenlenebilir bir GridView oluşturma Bu teknik yeniden, uygulama yapalım.

Bu senaryoda çalışmak için başka bir aşırı yüklemesini ihtiyacımız `UpdateProduct` yöntemi, dört parametre kabul eden: Ürün adı, birim fiyat, stock ve kimliği birim Aşağıdaki yöntemi ekleyin `ProductsBLL` sınıfı:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Bu yöntem tam, biz bu dört belirli ürün alanları düzenleme için izin veren ASP.NET sayfası oluşturmak için hazır. Açık `ErrorHandling.aspx` sayfasındaki `EditInsertDelete` klasörü ve tasarımcı sayfadan GridView ekleyin. Yeni bir ObjectDataSource için GridView bağlamak eşleme `Select()` yönteme `ProductsBLL` sınıfının `GetProducts()` yöntemi ve `Update()` yönteme `UpdateProduct` yeni oluşturduğunuz aşırı.


[![Dört giriş parametreleri kabul UpdateProduct yöntemi aşırı yüklemesini kullanın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Şekil 1**: kullanım `UpdateProduct` yöntemi aşırı olduğunu kabul dört giriş parametreleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))


Bu bir ObjectDataSource ile oluşturacak bir `UpdateParameters` ürün alanların her biri için dört parametre ve bir alanla GridView koleksiyonu. ObjectDataSource'nın bildirim temelli biçimlendirme atar `OldValuesParameterFormatString` özellik değeri `original_{0}`, neden olacak bir özel durum bizim BLL sınıfının adlı bir giriş parametresi beklemeyen beri `original_productID` iletilecek parametre. Bu tamamen Tanımlayıcı Sözdizimi ayarı kaldırmayı unutmayın (veya varsayılan değerine ayarlı `{0}`).

Ardından, yalnızca dahil etmek GridView Karşılaştır `ProductName`, `QuantityPerUnit`, `UnitPrice`, ve `UnitsInStock` BoundFields. Ayrıca gerekli bulduğunuz herhangi alan düzeyindeki biçimlendirme uygulamak çekinmeyin (değiştirme gibi `HeaderText` özellikleri).

Önceki öğreticide biz biçimlendirmek ne Aranan `UnitPrice` BoundField hem salt okunur modda ve düzenleme modunda bir para birimi olarak. Şimdi aynı burada yapın. Bu gerekli BoundField's ayarını geri çağırma `DataFormatString` özelliğine `{0:c}`, kendi `HtmlEncode` özelliğine `false`ve kendi `ApplyFormatInEditMode` için `true`, Şekil 2'de gösterildiği gibi.


[![Para birimi olarak görüntülenecek UnitPrice BoundField yapılandırın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Şekil 2**: yapılandırmak `UnitPrice` bir para birimi olarak görüntülenecek BoundField ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))


Biçimlendirme `UnitPrice` düzenleme arabiriminde bir para birimi GridView için 's olay işleyicisi oluşturma gerektirdiğinden `RowUpdating` para birimi olarak biçimlendirilmiş dizeye ayrıştırır olay bir `decimal` değeri. Sözcüğünün `RowUpdating` son öğretici olay işleyiciden da kontrol kullanıcı tarafından sağlanan emin olmak için bir `UnitPrice` değeri. Ancak, Bu öğretici için şimdi fiyat atlayın izin verin.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

Bizim GridView içeren bir `QuantityPerUnit` BoundField, ancak bu BoundField yalnızca görüntüleme amacıyla olmalıdır ve kullanıcı tarafından düzenlenebilir olmamalıdır. Bu düzenlemek için yalnızca BoundFields ayarlamak `ReadOnly` özelliğine `true`.


[![QuantityPerUnit BoundField salt okunur yapma](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Şekil 3**: olun `QuantityPerUnit` BoundField salt okunur ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))


Son olarak, GridView'ın akıllı etiket gelen düzenlemeyi etkinleştir onay kutusunu işaretleyin. Bu adımları tamamladıktan sonra `ErrorHandling.aspx` sayfanın Tasarımcısı Şekil 4'e benzer görünmelidir.


[![Tüm gerekli BoundFields ve onay Kaldır onay kutusunu düzenlemeyi etkinleştir](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Şekil 4**: kaldırmak dışındaki tüm gerekli BoundFields ve denetimi etkinleştirmek düzenleme Checkbox ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))


Bu noktada, tüm ürünlerin listesini sahibiz `ProductName`, `QuantityPerUnit`, `UnitPrice`, ve `UnitsInStock` alanları; ancak, yalnızca `ProductName`, `UnitPrice`, ve `UnitsInStock` alanları düzenlenebilir.


[![Kullanıcılar artık kolayca ürünlerin adları, Fiyatlar ve stok alanları birimlerinde düzenleyebilirsiniz](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Şekil 5**: kullanıcılar yapabilirsiniz şimdi kolayca Düzenle ürünlerin adları, Fiyatlar ve birimler hisse senedi alanları ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>2. adım: Düzgün biçimde DAL düzeyi özel durumları işleme

Bizim düzenlenebilir GridView son derece kullanıcıların geçerli değerleri düzenlenen ürün adı, fiyat ve stok girdiğinizde çalışırken, geçersiz değerler girerek bir özel durum oluşur. Örneğin, atlama `ProductName` değer neden bir [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) bu yana durum oluşturulmasına `ProductName` özelliğinde `ProdcutsRow` sınıfına sahip kendi `AllowDBNull` özelliğini `false`if kapalı olduğu veritabanı bir `SqlException` TableAdapter ile veritabanına bağlanmaya çalışılırken oluşturulur. Herhangi bir eylemde bulunmadan, bu özel durumlar Kabarcık yukarı veri erişim katmanı, ASP.NET sayfası sonra iş mantığı katmanı ve son olarak ASP.NET çalışma zamanı için.

Web uygulamanızın nasıl yapılandırılacağı ve uygulamadan ziyaret ettiğiniz olup olmadığına bağlı olarak `localhost`, işlenmeyen bir özel durum genel sunucu hatası sayfası, ayrıntılı hata raporu veya kullanıcı dostu bir web sayfası neden olabilir. Bkz: [Web uygulama hata işleme ASP.NET](http://www.15seconds.com/issue/030102.htm) ve [customErrors öğesi](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) ASP.NET çalışma zamanı yakalanmayan bir özel durum olarak nasıl yanıt vereceğini hakkında daha fazla bilgi.

Şekil 6 gösteren bir ürün belirtmeden güncellerken karşılaşılan ekran `ProductName` değeri. Bu ayrıntılı hata raporu görüntülenen çıkarken varsayılandır `localhost`.


[![Ürün adı olacak görünen özel durum ayrıntıları atlama](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Şekil 6**: Ürün adı olur görünen özel durum ayrıntıları atlama ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))


Bu tür özel durum ayrıntıları bir uygulamayı test edilirken yararlı olsa da, bu tür bir ekran karşısında bir özel durum ile bir son kullanıcı sunan değerinden idealdir. Büyük olasılıkla bir son kullanıcı ne bilmiyor bir `NoNullAllowedException` olduğu veya neden neden oldu. Ürün güncelleştirilmeye çalışılırken sorunlar olduğunu açıklayan için daha kullanıcı dostu bir ileti kullanıcının sunması daha iyi bir yaklaşımdır.

Bir işlemi gerçekleştirirken bir özel durum oluşursa, ObjectDataSource ve veri Web denetimi sonrası düzeyi olayları algılaması ve ASP.NET çalışma zamanı kadar tırmanmasını gelen özel durum iptal etmek için bir yol sağlar. Bizim örneğimizde, olay işleyici GridView için 's oluşturalım `RowUpdated` bir özel durum harekete ve bu durumda, özel durum ayrıntıları bir etiket Web denetiminde görüntüler varsa, belirler olay.

ASP.NET sayfası ayarı için bir etiket eklemeye başlayın kendi `ID` özelliğine `ExceptionDetails` ve temizleme kendi `Text` özelliği. Bu ileti kullanıcının göz çizmek için ayarlamanız kendi `CssClass` özelliğine `Warning`, eklediğimiz için bir CSS sınıfı olduğu `Styles.css` önceki öğretici dosyasında. Kırmızı, italik, kalın, çok büyük yazı tipiyle görüntülenecek etiketin metin bu CSS sınıfı neden geri çağırma.


[![Bir etiket Web denetimi sayfasına ekleme](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Şekil 7**: sayfasına bir etiket Web denetim ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))


Bu etiket Web denetimi yalnızca hemen sonra görünür olmasını istiyoruz bu yana bir özel durum oluştu, Ayarla kendi `Visible` özelliği false olarak `Page_Load` olay işleyicisi:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

İlk sayfasını ziyaret edin ve sonraki Geri göndermeler Bu kod `ExceptionDetails` denetim olacaktır, `Visible` özelliğini `false`. GridView, kullanıcının tespit edebilirsiniz bir DAL veya BLL düzeyinde özel karşısında `RowUpdated` olay işleyicisi, biz ayarlayacak `ExceptionDetails` denetimin `Visible` özelliğinin true. Web denetimi olay işleyicileri sonra oluşan bu yana `Page_Load` sayfa yaşam döngüsü olay işleyicisine etiketi gösterilecek. Ancak, sonraki geri gönderme üzerinde `Page_Load` olay işleyicisi dönecek `Visible` özelliğini yeniden `false`, görünümden yeniden gizleme.

> [!NOTE]
> Alternatif olarak, biz ayarı gerekliliğini kaldıramadı `ExceptionDetails` denetimin `Visible` özelliğinde `Page_Load` atayarak kendi `Visible` özelliği `false` Tanımlayıcı Sözdizimi ve (kendi ayarlayarakgörünümdurumunudevredışıbırakma`EnableViewState` özelliğine `false`). Sonraki Öğreticide bu alternatif bir yaklaşım kullanacağız.


Etiket denetimi eklenen bizim sonraki adım olay işleyicisi GridView için 's oluşturmaktır `RowUpdated` olay. GridView Tasarımcısı'nda seçin, özellikleri penceresine gidin ve GridView'ın olayları listeleyen Şimşek Cıvata simgesine tıklayın. Ayrıca bir girişi yok GridView'ın zaten olmalıdır `RowUpdating` olay, bu öğreticide daha önce bu olay için bir olay işleyicisi oluşturduğumuz gibi. İçin bir olay işleyicisi oluşturun `RowUpdated` de olay.


![GridView's RowUpdated olayı için bir olay işleyicisi oluşturun](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Şekil 8**: olay işleyicisi GridView için 's oluşturmak `RowUpdated` olayı


> [!NOTE]
> Aşağı açılan listeleri ile olay işleyicisi arka plandaki kod sınıfı dosyanın üst kısmında da oluşturabilirsiniz. GridView soldaki aşağı açılan listeden seçin ve `RowUpdated` sağdaki bir olay.


Bu olay işleyicisi oluşturma aşağıdaki kod ASP.NET sayfa arka plan kodu sınıfına ekleyin:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

Bu olay işleyicinin ikinci giriş parametresi türünde bir nesnedir [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), özel durumları işlemek için ilgi üç özellik vardır:

- `Exception` oluşturulan özel durum referansı; hiçbir özel durum, bu özellik bir değeri olur `null`
- `ExceptionHandled` özel durum olarak yürütüldü olup olmadığını gösteren bir Boole değeri `RowUpdated` olay işleyicisi if `false` (varsayılan), özel durumu ASP.NET çalışma zamanı kadar percolating yeniden oluşturulur.
- `KeepInEditMode` varsa kümesine `true` düzenlenen GridView satır düzenleme modunda; kalır `false` (varsayılan), GridView satır, salt okunur moduna geri döner.

Bizim kodu daha sonra denetlemeli `Exception` değil `null`, işlem gerçekleştirilirken bir özel durumu gerçekleşti anlamına gelir. Bu durumda, istiyoruz:

- Kullanıcı dostu bir ileti görüntüler `ExceptionDetails` etiketi
- Özel durumun işlendiğini gösterir
- GridView satır düzenleme modunda tutun

Bu aşağıdaki kod bu amaçları gerçekleştirir:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Bu olay işleyicisi olup olmadığını denetleyerek başlar `e.Exception` olan `null`. Değilse `ExceptionDetails` etiketin `Visible` özelliği ayarlanmış `true` ve kendi `Text` "Ürün güncelleştirilirken bir sorun oluştu." özelliği Oluşturulan özel durumu ayrıntılarını bulunan `e.Exception` nesnenin `InnerException` özelliği. Bu iç özel duruma incelenir ve belirli bir tür ise, ek, yararlı iletisine eklenir `ExceptionDetails` etiketin `Text` özelliği. Son olarak, `ExceptionHandled` ve `KeepInEditMode` özellikleri her ikisi de ayarlandığında `true`.

Şekil 9 ürün adı atlama bu sayfayı ekran görüntüsü gösterir; Şekil 10 geçersiz girerken sonuçları gösterir `UnitPrice` değerini (-50).


[![ProductName BoundField bir değeri içermelidir](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Şekil 9**: `ProductName` BoundField bir değer içermesi gerekir ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))


[![Negatif UnitPrice değerler izin verilmiyor:](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Şekil 10**: negatif `UnitPrice` değerler izin ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))


Ayarlayarak `e.ExceptionHandled` özelliğine `true`, `RowUpdated` olay işleyicisi belirtilen özel durumun işlenip. Bu nedenle, özel durum ASP.NET çalışma zamanı kadar yayılması olmaz.

> [!NOTE]
> Şekil 9 ve 10 nedeniyle geçersiz kullanıcı girişi yükseltilmiş özel durumları işlemek için normal bir şekilde gösterir. İdeal olarak, ancak bu tür giriş geçersiz asla ulaşma iş mantığı katmanı ilk başta, ASP.NET sayfası çağırmadan önce kullanıcının girişleri geçerli olduğundan emin olun şekilde `ProductsBLL` sınıfının `UpdateProduct` yöntemi. İş mantığı katmanı için gönderilen veri emin olmak için düzenleme ve ekleme arabirimlerine doğrulama denetimleri ekleme göreceğiz bizim sonraki öğreticide iş kuralları için uygundur. Doğrulama denetimleri yalnızca çağrılmasını engellemek `UpdateProduct` kullanıcı tarafından sağlanan veri geçerlidir, ancak aynı zamanda veri girişi sorunlarını tanımlamak için daha bilgilendirici bir kullanıcı deneyimi sağlamak kadar yöntemi.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>3. adım: Düzgün biçimde BLL düzeyi özel durumları işleme

Eklerken, güncelleştirme ya da verileri silme veri erişim katmanı verilerle ilgili bir hata karşısında bir özel durum. Veritabanı çevrimdışı olabilir, gerekli veritabanı tablo sütunu belirtilen bir değeri uygulanmamış veya bir tablo düzeyi kısıtlaması ihlal. Özel durumlar kesinlikle verilerle ilgili ek olarak, iş mantığı katmanı iş kurallarını ihlal olduğunda belirtmek için özel durumlar kullanın. İçinde [bir iş mantığı katmanı oluşturma](../introduction/creating-a-business-logic-layer-vb.md) öğretici, bir iş kuralı denetimi asıl Örneğin, eklediğimiz `UpdateProduct` aşırı yükleme. Özellikle, kullanıcı bir ürün dönüştürülmesine olarak işaretlemenin oluştu, ürün, sağlayıcısı tarafından sağlanan tek olmaması gereklidir. Bu durum ihlal varsa, bir `ApplicationException` belirtildi.

İçin `UpdateProduct` aşırı oluşturulan Bu öğreticide, yasaklar bir iş kuralı ekleyelim `UnitPrice` katından fazla özgün olan yeni bir değer ayarlanan alanının `UnitPrice` değeri. Bunu gerçekleştirmek için ayarlamak `UpdateProduct` ; böylece bu denetimi gerçekleştirir ve oluşturur aşırı bir `ApplicationException` kuralı ihlal edilirse. Güncelleştirilen yöntemi aşağıdaki gibidir:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Bu değişiklikle katından fazla var olan fiyat herhangi bir fiyat güncelleştirme neden olacak bir `ApplicationException` oluşturulmasına. DAL, bu BLL yükseltilmiş gerçekleştirilen özel durum'olduğu gibi `ApplicationException` algılandı ve GridView içinde 's ele `RowUpdated` olay işleyicisi. Aslında, `RowUpdated` olay işleyicinin kod yazıldığı şekilde doğru bu özel durumun algılar ve görüntüler `ApplicationException`'s `Message` özellik değeri. Şekil 11 bir kullanıcı birden fazla çift kendi geçerli bedelinin $19.95 olduğu Chai fiyat $50,00, güncelleştirmeye çalıştığında ekran gösterir.


[![İş kurallarını birden fazla ürünün fiyatını çift fiyat artışları izin verme](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Şekil 11**: birden fazla ürünün fiyatını çift izin verme fiyat artışları iş kuralları ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))


> [!NOTE]
> İdeal olarak, iş mantığı kuralları dışında yeniden işlenmesi `UpdateProduct` yöntemi aşırı ve ortak bir yöntem olarak. Okuyucu için bu bir alıştırma olarak kalır.


## <a name="summary"></a>Özet

Ekleme, güncelleştirme ve silme işlemleri sırasında veri Web denetimi ve söz konusu ObjectDataSource öncesi ve sonrası düzeyi olayları bu bağlayıcının gerçek işlemi kov. Bu öğretici ve önceki bir düzenlenebilir GridView ile çalışırken GridView's gördüğümüz gibi `RowUpdating` ObjectDataSource tarafından 's ardından olay ateşlenir `Updating` hangi noktada güncelleştirme komutu ObjectDataSource için 's yapıldığında, olayı temel alınan nesnesi. ObjectDataSource's işlemi tamamlandı sonra `Updated` GridView tarafından 's ardından olay ateşlenir `RowUpdated` olay.

Olay işleyicileri giriş parametreleri özelleştirmek için önceden düzeyi olayları ya da inceleyebilir ve işlem sonuçları yanıt için sonrası düzeyi olaylar oluşturabilir. Sonrası düzeyi olay işleyicileri, işlem sırasında bir özel durum oluştu olup olmadığını algılamak için en yaygın olarak kullanılır. Bir özel durum karşısında bu sonrası düzeyi olay işleyicileri isteğe bağlı olarak kendi başlarına özel durumu işleyebilir. Bu öğreticide bir kolay hata iletisi görüntüleyerek böyle bir özel durum işleme gördük.

Sonraki öğreticide sorunları biçimlendirme verilerden oluşan özel durumlar olasılığını azaltmak nasıl göreceğiz (negatif girme gibi `UnitPrice`). Özellikle, düzenleme ve ekleme arabirimleri doğrulama denetimleri ekleme inceleyeceğiz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Liz Shulok oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [sonraki](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
