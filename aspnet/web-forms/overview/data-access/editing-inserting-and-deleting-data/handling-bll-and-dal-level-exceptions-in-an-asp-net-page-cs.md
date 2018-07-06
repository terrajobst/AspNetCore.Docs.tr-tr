---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
title: (C#) bir ASP.NET sayfasında BLL ve DAL düzeyi özel durumları işleme | Microsoft Docs
author: rick-anderson
description: Bu öğreticide bir ekleme, güncelleştirme veya silme işlemi sırasında bir özel durum gerçekleşmelidir kolay, bilgilendirici bir hata iletisi görüntülemek nasıl göreceğiz...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 49d8a66c-3ea8-4087-839f-179d1d94512a
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 13d33251a8552409a17b5ff9a1dcc07d78506b12
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819503"
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-c"></a>(C#) bir ASP.NET sayfasında BLL ve DAL düzeyi özel durumları işleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_CS.exe) veya [PDF olarak indirin](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/datatutorial18cs1.pdf)

> Bu öğreticide bir INSERT, update veya Web denetimi bir ASP.NET veri silme işlemi sırasında bir özel durum gerçekleşmesi gereken bir kolay, bilgilendirici hata iletisi görüntülemek nasıl göreceğiz.


## <a name="introduction"></a>Giriş

Katmanlı Uygulama mimarisi kullanarak bir ASP.NET web uygulamasından veri ile çalışma, aşağıdaki üç genel adımları içerir:

1. İş mantığı katmanı, hangi yöntemin çağrılması gerekir ve bunu geçirmek için hangi parametre değerleri belirler. Giriş kullanıcı tarafından girilen veya parametre değerlerini sabit kodlanmış, programlı olarak atanmış olabilir.
2. Yöntemi çağır.
3. Sonuçları işleyebilirsiniz. Veri döndüren bir BLL metodu çağrılırken, bu verileri Web denetimi için veri bağlama gerektirebilir. Verileri değiştirme BLL yöntemleri için bir dönüş değerini temel alarak veya adım 2'de çıkan herhangi bir özel durumların işlenmesi bazı eylemler gerçekleştirme içerebilir.

İçinde gördüğümüz gibi [önceki öğreticide](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md), hem ObjectDataSource hem de veri Web denetimleri sıra 1. ve 3 için genişletilebilirlik noktaları sağlar. Örneğin, GridView harekete kendi `RowUpdating` önce alan değerlerinin kendi ObjectDataSource atama olay `UpdateParameters` koleksiyonu; `RowUpdated` ObjectDataSource işlemi tamamlandıktan sonra olayı oluşturulur.

Biz zaten yangın sırasında adım 1 ve nasıl giriş parametrelerini özelleştirebilir veya işlemi iptal etmek için kullanılabilirler gördünüz olayları incelendi. Bu öğreticide size uygulamamızla işlemi tamamlandıktan sonra tetiklenen olayları açacağım. Bu sonrası düzeyi olay işleyicileri, diğerlerinin yanı sıra mümkün olan bir özel durum işlemi sırasında oluşup olmadığını belirlemek ve düzgün bir şekilde, standart ASP.NET için varsayılan olarak ayarlanıyor yerine kolay, bilgilendirici bir hata iletisi ekranda görüntüleme işlemesi özel durum sayfası.

Bu düzey sonrası olaylar ile çalışmayla göstermek için düzenlenebilir bir GridView ürünleri listeleyen bir sayfa oluşturalım. Bir özel durum, bir ürün güncelleştiriliyor başlatıldığında bizim ASP.NET sayfası bir sorun oluştu açıklayan GridView yukarıda kısa bir ileti görüntüler. Haydi başlayalım!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>1. adım: bir düzenlenebilir GridView ürün oluşturuluyor

Önceki öğreticide düzenlenebilir bir GridView yalnızca iki alan, oluşturduğumuz `ProductName` ve `UnitPrice`. Bu gerekli için ek bir aşırı yükleme oluşturma `ProductsBLL` sınıfın `UpdateProduct` yöntemi, yalnızca üç giriş parametrelerini (ürün adı, birim fiyatı ve kimliği) her ürün alanı için karşılıklı olarak bir parametre kabul eden. Bu öğretici için ürün adı, miktar birimi, birim fiyatı ve birim başına stokta görüntüler, ancak yalnızca adı, birim fiyatı ve birimler stokta düzenlenmesine izin verir, düzenlenebilir bir GridView oluşturma bu tekniği yeniden uygulama yapalım.

Bu senaryoya uyum sağlamak için başka bir aşırı yüklemesini gerekir `UpdateProduct` yöntemi, dört parametre kabul eden bir: Ürün adı, birim fiyatı, stok ve kimliği birim Aşağıdaki yöntemi ekleyin `ProductsBLL` sınıfı:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample1.cs)]

Bu yöntem tamamlandı, bu dört belirli ürün alanları düzenleme için izin veren ASP.NET sayfası oluşturmak hazırız. Açık `ErrorHandling.aspx` sayfasını `EditInsertDelete` klasörü ve sayfa tasarımcıyı aracılığıyla GridView ekleyin. GridView bağlamak için yeni bir ObjectDataSource, eşleme `Select()` yönteme `ProductsBLL` sınıfın `GetProducts()` yöntemi ve `Update()` yönteme `UpdateProduct` oluşturduğunuz aşırı yükleme.


[![Dört giriş parametrelerini kabul eden UpdateProduct yöntemi aşırı yüklemesini kullanın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image1.png)

**Şekil 1**: kullanım `UpdateProduct` yöntemi aşırı yükleme olduğunu kabul eden dört girdi parametreleri ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image3.png))


Bu bir ObjectDataSource ile oluşturacak bir `UpdateParameters` her ürün alanı için dört parametre ve bir alanla GridView koleksiyonu. ObjectDataSource bildirim temelli biçimlendirme atar `OldValuesParameterFormatString` özellik değeri `original_{0}`, neden olacak bir özel durum bizim BLL sınıfın adlı giriş parametresi beklemiyoruz beri `original_productID` geçirilmesi. Bu tamamen bildirim temelli söz dizimi ayarlanması kaldırmayı unutmayın (veya varsayılan değerlerin belirlenmiş `{0}`).

Ardından, yalnızca dahil etmek GridView küçültmek `ProductName`, `QuantityPerUnit`, `UnitPrice`, ve `UnitsInStock` BoundFields. Ayrıca gerekli bulduğunuz tüm alan düzeyinde biçimlendirme uygulamak rahatça (değiştirme gibi `HeaderText` özellikleri).

Önceki öğreticide, nasıl biçimlendirileceğini incelemiştik `UnitPrice` BoundField salt okunur modda ve düzenleme modunda bir para birimi olarak. Aynı burada yapalım. Bu BoundField'ın ayarlanması gereken geri çağırma `DataFormatString` özelliğini `{0:c}`, kendi `HtmlEncode` özelliğini `false`ve onun `ApplyFormatInEditMode` için `true`Şekil 2'de gösterildiği gibi.


[![Görüntülenecek UnitPrice BoundField bir para birimi olarak yapılandırma](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image4.png)

**Şekil 2**: yapılandırma `UnitPrice` BoundField bir para birimi olarak görüntülenecek ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image6.png))


Biçimlendirme `UnitPrice` düzenleme arabiriminde bir para birimi GridView için ait bir olay işleyicisi oluşturuluyor gerektirdiği `RowUpdating` içine para birimi ile biçimlendirilmiş dizeyi ayrıştırır olay bir `decimal` değeri. Bu geri çağırma `RowUpdating` olay işleyicisi son öğreticiden de kullanıma kullanıcı tarafından sağlanan emin olmak için bir `UnitPrice` değeri. Ancak, Bu öğretici için şimdi fiyat atlamak kullanıcının verin.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample2.cs)]

Bizim GridView içeren bir `QuantityPerUnit` BoundField, ancak bu BoundField yalnızca görüntüleme amaçları için olmalıdır ve kullanıcı tarafından düzenlenebilir olmamalıdır. Bu düzenlemek için BoundFields ayarlamanız yeterlidir `ReadOnly` özelliğini `true`.


[![QuantityPerUnit BoundField salt okunur yapma](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image7.png)

**Şekil 3**: olun `QuantityPerUnit` BoundField salt okunur ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image9.png))


Son olarak, GridView'ın akıllı etiketinde düzenlemeyi etkinleştir onay kutusunu işaretleyin. Bu adımları tamamladıktan sonra `ErrorHandling.aspx` sayfanın Tasarımcısı, Şekil 4'e benzer görünmelidir.


[![Tüm gerekli BoundFields ve onay Kaldır onay kutusunu düzenlemeyi etkinleştir](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image10.png)

**Şekil 4**: kaldırma dışındaki tüm gerekli BoundFields ve onay düzenlemeyi etkinleştir onay ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image12.png))


Bu noktada, tüm ürünlerin listesini sahibiz `ProductName`, `QuantityPerUnit`, `UnitPrice`, ve `UnitsInStock` alanları; ancak, yalnızca `ProductName`, `UnitPrice`, ve `UnitsInStock` alanları düzenlenebilir.


[![Kullanıcılar artık kolayca ürünlerin adlarını, fiyatları ve birimler stok alanları düzenleyebilir](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image13.png)

**Şekil 5**: kullanıcılar olabilir artık kolayca Düzenle ürünler adları, fiyatları ve birimler hisse senedi alanları ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>2. adım: Düzgün bir şekilde DAL düzeyi özel durumları işleme

Geçersiz değerler girerek, kullanıcıların yasal değerler düzenlenen ürün adı, fiyatı ve stoktaki birimleri girdiğinizde bizim düzenlenebilir GridView son derece yapmaya çalışırken, bir özel durum oluşur. Örneğin, atlama `ProductName` değeri neden bir [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) bu yana durum `ProductName` özelliğinde `ProdcutsRow` sınıfında kendi `AllowDBNull` özelliğini `false`; veritabanının kapalı olduğu bir `SqlException` TableAdapter ile veritabanına bağlanma girişiminde bulunduğunuzda oluşturulur. Herhangi bir işlem yapmadan bu özel durumlar Kabarcık yukarı veri erişim katmanından iş mantığı katmanı ve ardından ASP.NET sayfası ve son olarak ASP.NET çalışma zamanı.

Web uygulamanızı nasıl yapılandırıldığını ve uygulamadan ziyaret ettiğiniz olup olmadığına bağlı olarak `localhost`, genel sunucu hatası sayfası, ayrıntılı hata raporu veya kullanıcı dostu bir web sayfası içinde işlenmeyen bir özel durum neden olabilir. Bkz: [Web uygulama hata işleme ASP.NET](http://www.15seconds.com/issue/030102.htm) ve [customErrors öğesi](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) ASP.NET çalışma zamanı için yakalanmayan bir özel durum nasıl yanıt vereceğini hakkında daha fazla bilgi.

Şekil 6 belirtmeden bir ürün güncellemeye çalışırken karşılaşılan ekranın gösterildiği `ProductName` değeri. Ayrıntılı hata raporu görüntülenen gelen olduğunda varsayılan `localhost`.


[![Ürün adı olacak görünen özel durum ayrıntıları atlama](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image16.png)

**Şekil 6**: Ürün adı olur görünen özel durum ayrıntıları atlama ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image18.png))


Bu tür özel durum ayrıntıları bir uygulamayı test ederken yararlı olsa da, böyle bir ekran karşılaşıldığında bir özel durum ile bir son kullanıcı sunma küçüktür idealdir. Büyük olasılıkla son kullanıcı tanıdığınız değil bir `NoNullAllowedException` olduğu veya neden neden oldu. Kullanıcının ürün güncelleştirilmeye çalışılıyor sorunlar olduğunu açıklayan daha kullanıcı dostu bir iletiyle sunması daha iyi bir yaklaşımdır.

Bir işlemi gerçekleştirirken bir özel durum oluşursa, ObjectDataSource hem de veri Web denetimi sonrası düzeyinde olaylar algılaması ve ASP.NET çalışma zamanı kadar tırmanma öğesinden özel durum iptal etmek için bir yol sağlar. Örneğin, bir olay işleyicisi GridView için 's oluşturalım `RowUpdated` bir özel durum harekete ve bu durumda, bir etiket Web denetimi özel durum ayrıntıları görüntüler, belirleyen olay.

Başlangıç etiketi ayarlamak ASP.NET sayfasına ekleyerek kendi `ID` özelliğini `ExceptionDetails` ve temizleme kendi `Text` özelliği. Bu ileti kullanıcının gözünden çizmek için ayarlamanız, `CssClass` özelliğini `Warning`, eklediğimiz için bir CSS sınıfı olduğu `Styles.css` önceki öğreticide dosya. Bu bir CSS sınıfı kırmızı, italik, kalın, çok büyük yazı tipiyle görüntülenecek etiketin metni neden olduğunu hatırlayın.


[![Etiket Web denetimi sayfasına ekleme](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image19.png)

**Şekil 7**: sayfasına bir etiket Web denetim ekleme ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image21.png))


Bu etiketin Web denetimi yalnızca hemen sonra görünür olmasını istiyoruz. bu yana bir özel durum oluştu, ayarla, `Visible` özelliği false olarak `Page_Load` olay işleyicisi:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample3.cs)]

İlk sayfasını ziyaret edin ve sonraki Geri göndermeler bu kodla `ExceptionDetails` denetimi olacaktır, `Visible` özelliğini `false`. GridView ' kişinin saptanabilir bir DAL veya BLL düzeyi özel durumu karşılaşıldığında `RowUpdated` olay işleyicisi, biz ayarlayacak `ExceptionDetails` denetimin `Visible` özelliği true. Web denetim olay işleyicilerini sonra gerçekleşmediği `Page_Load` sayfa yaşam döngüsü olay işleyicisinde etiketi gösterilir. Ancak, sonraki geri gönderme üzerinde `Page_Load` olay işleyicisi geri dönecek `Visible` özelliğini yeniden `false`, yeniden görünümden gizlemeyi.

> [!NOTE]
> Alternatif olarak, biz ayarının yeterlilikte kaldırabilirsiniz `ExceptionDetails` denetimin `Visible` özelliğinde `Page_Load` atayarak, `Visible` özelliği `false` bildirim temelli söz dizimi ve görünüm durumunu (kendi ayarınıdevredışıbırakma`EnableViewState` özelliğini `false`). Bir sonraki Öğreticide bu alternatif yaklaşım kullanacağız.


GridView'için ın olay işleyicisi oluşturmak için sonraki adımımız eklenen etiket denetimi ile olan `RowUpdated` olay. GridView tasarımcıda seçin, özellikleri penceresine gidin ve GridView'ın olaylarını listelemek ışık Şimşek simgesine tıklayın. Ayrıca bir giriş var GridView'ın için zaten olmalıdır `RowUpdating` olay, bir olay işleyicisi bu olayı için Bu öğreticide daha önce oluşturduğumuz gibi. İçin bir olay işleyicisi oluşturun `RowUpdated` de olay.


![GridView'ın RowUpdated olayı için olay işleyicisi oluşturun](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image22.png)

**Şekil 8**: GridView için ait bir olay işleyicisi oluşturun `RowUpdated` olay


> [!NOTE]
> Arka plan kod sınıf dosyasının en üstünde açılan listeler yoluyla olay işleyicisini de oluşturabilirsiniz. GridView soldaki aşağı açılan listeden seçin ve `RowUpdated` sağdaki bir olay.


Bu olay işleyicisi oluşturma aşağıdaki kodu için ASP.NET sayfa arka plan kod sınıfı ekleyin:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample4.cs)]

Bu olay işleyicinin ikinci giriş parametresi türü bir nesne, [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), ilgilenilen özel durumları işlemek için üç özelliğe sahiptir:

- `Exception` oluşan özel durum başvuru; hiçbir özel durum, bu özellik değerine sahip `null`
- `ExceptionHandled` özel durum, işlenmiş olup olmadığını gösteren bir Boole değeri `RowUpdated` olay işleyicisi if `false` (varsayılan), ASP.NET çalışma zamanı kadar percolating durum yeniden istisnadır
- `KeepInEditMode` varsa kümesine `true` düzenlenen GridView satır düzenleme modunda; kalır `false` (varsayılan), GridView satır, salt okunur moduna geri döner.

Kodumuzu, daha sonra olmadığını görmek için denetlemelisiniz `Exception` değil `null`, işlem gerçekleştirilirken bir özel durum oluştu, anlamına gelir. Bu durumda, istiyoruz:

- Kullanıcı dostu bir ileti görüntüler `ExceptionDetails` etiketi
- Özel durumun işlendiğini gösterir
- Düzenleme modunda GridView satır tutun

Bu aşağıdaki kod bu hedefler yerine getirir:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample5.cs)]

Bu olay işleyicisi olmadığını denetleyerek başlar `e.Exception` olduğu `null`. Bu değilse `ExceptionDetails` etiketin `Visible` özelliği `true` ve kendi `Text` özelliğini, "Ürün güncelleştirilirken bir sorun oluştu." Oluşturulan gerçek özel durumun ayrıntılarını bulunan `e.Exception` nesnenin `InnerException` özelliği. Bu iç özel duruma incelenir ve belirli bir tür ise, ek, faydalı bir ileti eklenen `ExceptionDetails` etiketin `Text` özelliği. Son olarak, `ExceptionHandled` ve `KeepInEditMode` özellikleri her ikisi de ayarlanmış `true`.

Şekil 9, ürün adını atlandığında bu sayfanın ekran görüntüsü gösterir. Şekil 10 geçersiz girerken sonuçları gösteren `UnitPrice` değeri (-50).


[![ProductName BoundField bir değeri içermelidir](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image23.png)

**Şekil 9**: `ProductName` BoundField bir değer bulunmalıdır ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image25.png))


[![İzin negatif UnitPrice değerler şunlardır:](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image26.png)

**Şekil 10**: negatif `UnitPrice` değerler izin verilmiyor ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image28.png))


Ayarlayarak `e.ExceptionHandled` özelliğini `true`, `RowUpdated` olay işleyicisi belirtilen özel durumun işlenip. Bu nedenle, özel durum, ASP.NET çalışma zamanı kadar yay olmaz.

> [!NOTE]
> Şekil 9 ve 10 geçersiz kullanıcı girişi nedeniyle harekete geçirilen özel durumları işlemek için normal bir şekilde gösterir. İdeal olarak, ancak böyle geçersiz giriş asla ulaşma iş mantığı katmanı ilk başta, ASP.NET sayfasını çağırmadan önce kullanıcının girişler geçerli olduğundan emin olun şekilde `ProductsBLL` sınıfın `UpdateProduct` yöntemi. İş mantığı katmanı için gönderilen veriler emin olmak için düzenleme ve ekleme arabirimlerine doğrulama denetimleri ekleme görüyoruz sonraki müşterilerimize öğreticide iş kuralları için uygundur. Doğrulama denetimleri yalnızca çağırmayı önlemek `UpdateProduct` yöntemi kadar kullanıcı tarafından sağlanan veriler geçerli, ancak aynı zamanda veri girişi sorunlarını tanımlamak için daha bilgilendirici bir kullanıcı deneyimi sağlar.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>3. adım: Düzgün bir şekilde BLL düzeyi özel durumları işleme

Veri erişim katmanı, güncelleştirmek veya veri silme eklerken, verilerle ilgili bir hata ile karşılaşıldığında bir özel durum oluşturabilir. Tablo düzeyi kısıtlaması ihlal veya gerekli veritabanı tablo sütunu bir değer vardı değil veya veritabanı çevrimdışı olabilir. Özel durumlar kesinlikle verilerle ilgili ek olarak, iş mantığı katmanı iş kurallarını ihlal belirten özel durumları kullanın. İçinde [iş mantığı katmanı oluşturma](../introduction/creating-a-business-logic-layer-cs.md) öğretici, bir iş kuralı denetimi özgün gibi ekledik `UpdateProduct` aşırı yükleme. Özellikle, kullanıcı bir ürün kullanımdan olarak işaretlenmesi oluştu, ürün, sağlayıcı tarafından sağlanan tek olmaması gereklidir. Bu koşulun ihlâl edildiğini, bir `ApplicationException` oluşturuldu.

İçin `UpdateProduct` aşırı yükleme, bu öğreticide oluşturulan, yasaklar bir iş kuralı ekleyelim `UnitPrice` özgün katından daha yeni bir değere ayarlandığı alanını `UnitPrice` değeri. Bunu gerçekleştirmek için ayarlamak `UpdateProduct` bu denetimi gerçekleştirir ve oluşturur, aşırı bir `ApplicationException` kuralı ihlal edilirse. Güncelleştirilmiş yöntem aşağıdaki gibidir:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample6.cs)]

Bu değişiklik, birden çok kez mevcut fiyatı herhangi bir fiyat güncelleştirme neden olacak bir `ApplicationException` oluşturulması için. DAL, bu BLL yükseltilmiş harekete geçirilen özel durum'olduğu gibi `ApplicationException` algılandı ve GridView kişinin işlenen `RowUpdated` olay işleyicisi. Aslında, `RowUpdated` olay işleyicinin kodunu yazıldığı gibi doğru bu özel durumun algılar ve görüntüler `ApplicationException`'s `Message` özellik değeri. Şekil 11 bir kullanıcı birden fazla çift $19.95 kendi geçerli fiyatını olduğu Chai fiyatı $50,00 için güncelleştirmeye çalıştığında ekran gösterilir.


[![İş kurallarını birden fazla ürünün fiyatını çift fiyat artışları izin vermeyin.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image29.png)

**Şekil 11**: birden fazla ürünün fiyatını çift izin verme fiyat artışları iş kuralları ([tam boyutlu görüntüyü görmek için tıklatın](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image31.png))


> [!NOTE]
> İdeal olarak iş mantığı kurallarımızın tanesi düzenlenmeye `UpdateProduct` yöntemi aşırı yüklemeleri ve yaygın bir yöntemdir. Okuyucu için bu bir alıştırma olarak kalır.


## <a name="summary"></a>Özet

Ekleme, güncelleme ve silme işlemleri sırasında hem veri Web denetimi hem de katılan ObjectDataSource öncesi ve sonrası düzeyi olaylar gerçek işlemi bu bağlayıcının kov. Bu öğretici ve önceki bir düzenlenebilir bir GridView ile çalışırken GridView'ın gördüğümüz gibi `RowUpdating` ObjectDataSource tarafından izlenen olay harekete geçirilir `Updating` hangi noktada güncelleştirme komut ObjectDataSource yapıldığında olayı temel alınan nesne. Tamamlandıktan sonra ObjectDataSource `Updated` olayı tetiklendiğinde, GridView tarafından 's ardından `RowUpdated` olay.

Olay işleyicileri veya sonrası düzeyi olayları inceleyin ve işlemin sonuçlarını yanıt için giriş parametrelerini özelleştirmek için önceden düzeyinde olaylar için oluşturabiliriz. Sonrası düzeyi olay işleyicileri, işlem sırasında bir özel durum oluştu olup olmadığını algılamak için en yaygın olarak kullanılır. Bir özel durum karşılaşıldığında sonrası düzeyi olay işleyicilere isteğe bağlı olarak, kendi özel durumu işleyebilir. Bu öğreticide bir kolay hata iletisi görüntüleyerek gibi bir özel durumu işlemek nasıl gördük.

Sonraki öğreticide sorunları biçimlendirme verilerden kaynaklanan özel durumları olasılığını azaltmak nasıl görüyoruz (bir negatif girerek gibi `UnitPrice`). Özellikle, düzenleme ve ekleme arabirimlerine doğrulama denetimleri ekleme şu konuları inceleyeceğiz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Liz Shulok oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
> [İleri](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
