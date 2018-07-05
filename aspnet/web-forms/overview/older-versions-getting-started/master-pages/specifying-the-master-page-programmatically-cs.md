---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
title: Ana sayfayı programlı olarak belirtme (C#) | Microsoft Docs
author: rick-anderson
description: İçerik sayfasının ana sayfayı programlı olarak PreInit olay işleyicisi aracılığıyla ayarlanırken arar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 7c4a3445-2440-4aee-b9fd-779c05e6abb2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b3fbd95a8088e20382c426fcdcc6a4b6e67d44b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370396"
---
<a name="specifying-the-master-page-programmatically-c"></a>Ana sayfayı programlı olarak belirtme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.pdf)

> İçerik sayfasının ana sayfayı programlı olarak PreInit olay işleyicisi aracılığıyla ayarlanırken arar.


## <a name="introduction"></a>Giriş

Bülteninin açılış sayısına örnekte beri [ *bir Site genelinde düzenini kullanarak ana sayfa oluşturma*](creating-a-site-wide-layout-using-master-pages-cs.md), tüm içerik sayfaları, kendi ana sayfa ile bildirimli olarak başvurulan `MasterPageFile` özniteliğinde`@Page`yönergesi. Örneğin, aşağıdaki `@Page` yönergesi bağlantılar içerik sayfası ana sayfaya `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample1.aspx)]

[ `Page` Sınıfı](https://msdn.microsoft.com/library/system.web.ui.page.aspx) içinde `System.Web.UI` ad alanı içeren bir [ `MasterPageFile` özelliği](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) ana sayfaya içerik sayfasının yolunu döndürür; tarafındanayarlanmışolanbuözellik,`@Page` yönergesi. Bu özellik, içerik sayfasının ana sayfayı programlı olarak belirtmek için de kullanılabilir. Bu yaklaşım, ana sayfaya sayfasını ziyaret ederek kullanıcı gibi dış faktörlerden göre dinamik olarak atamak istiyorsanız kullanışlıdır.

Bu öğreticide sitemizin için ikinci bir ana sayfa ekleyin ve dinamik olarak çalışma zamanında kullanılacak ana sayfasını karar verin.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>1. adım: Göz sayfa yaşam döngüsü

Web sunucusunda bir içerik sayfasının bir ASP.NET sayfası için bir istek geldiğinde ASP.NET altyapısı sayfanın Sigortası içerik denetimleri ana sayfasına ContentPlaceHolder denetimleri karşılık gelen. Bu fusion ardından tipik sayfa yaşam döngüsü ile devam bir tek denetim hiyerarşisi oluşturur.

Şekil 1, bu fusion gösterilmektedir. 1. adım Şekil 1'ana sayfası denetimi hiyerarşileri ve başlangıç içeriğini gösterir. İçerik PreInit aşama tail sonunda sayfasındaki denetimleri ana sayfa (Adım 2) içindeki karşılık gelen ContentPlaceHolder eklenir. Bu fusion sonra ana sayfaya çarpım denetim hiyerarşisinin kökü görev yapar. Bu denetim çarpım hiyerarşi ardından sonlandırılmış denetim hiyerarşisi (adım 3) üretmek için sayfasına eklenir. Sayfanın denetim hiyerarşisi çarpım denetim hiyerarşisi içerdiğini net sonucudur.


[![Ana sayfa ve içerik sayfasının denetim hiyerarşileri çarpım PreInit aşamasında birbirine](specifying-the-master-page-programmatically-cs/_static/image2.png)](specifying-the-master-page-programmatically-cs/_static/image1.png)

**Şekil 01**: ana sayfa ve içerik sayfasının denetim hiyerarşileri birleştirilir çarpım PreInit aşamasında ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-master-page-programmatically-cs/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>2. adım: Ayar`MasterPageFile`koddan özelliği

Hangi ana sayfa içinde bu fusion partakes değerine bağlıdır `Page` nesnenin `MasterPageFile` özelliği. Ayarı `MasterPageFile` özniteliğini `@Page` yönergesi etkisi atama net `Page`'s `MasterPageFile` özelliği ilk sayfa yaşam döngüsü aşaması başlatma aşamasında. Biz bunun yerine bu özelliği programlı olarak ayarlayabilirsiniz. Ancak, Şekil 1'deki fusion gerçekleşmeden önce bu özelliğin ayarlanması zorunlu değildir.

PreInit aşamasının başlangıcında `Page` nesnesini başlatır, [ `PreInit` olay](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) ve kendi [ `OnPreInit` yöntemi](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Ana sayfayı programlı olarak ayarlamak için daha sonra ya da bir olay işleyicisi için oluşturabiliriz `PreInit` olay veya geçersiz kılma `OnPreInit` yöntemi. Her iki yaklaşım göz atalım.

Başlangıç açarak `Default.aspx.cs`, bizim sitesinin giriş sayfası için arka plan kod sınıf dosyası. Sayfa için bir olay işleyicisi ekleme `PreInit` aşağıdaki kod yazarak olay:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample2.cs)]

Buradan ayarladığımız `MasterPageFile` özelliği. Değeri atar, böylece kodu güncelleştirmeniz "~ / Site.master" için `MasterPageFile` özelliği.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample3.cs)]

Bir kesme noktası ayarlayın ve hata ayıklama ile Başlat, göreceksiniz her `Default.aspx` sayfasını ziyaret edildiğinde veya herhangi bir zamanda bu sayfaya geri gönderme var. `Page_PreInit` olay işleyicisi yürütülür ve `MasterPageFile` özelliği atanır "~ / Site.master".

Alternatif olarak, kılabilirsiniz `Page` sınıfın `OnPreInit` yöntemi ve kümesi `MasterPageFile` özelliği vardır. Bu örnekte, belirli bir sayfada ana sayfasında ancak bunun yerine gelen ayarlayalım değil `BasePage`. Özel taban sayfası sınıfı oluşturduk geri çağırma (`BasePage`) geri [ *ana sayfada başlık, Meta etiketler ve diğer HTML üst bilgilerini belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) öğretici. Şu anda `BasePage` geçersiz kılmalar `Page` sınıfın `OnLoadComplete` yöntemi, sayfanın ayarlar burada `Title` özelliği, site haritası verileri temel alarak. Güncelleştirelim `BasePage` ayrıca geçersiz kılmak için `OnPreInit` ana sayfayı programlı olarak belirtmek için yöntemi.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample4.cs)]

Tüm içerik sayfalarımızın türetmek için `BasePage`, kendi ana sayfayı programlı olarak atanan tümünün artık gerekir. Bu noktada `PreInit` olay işleyicisinde `Default.aspx.cs` gereksiz; kaldırmak çekinmeyin.

### <a name="what-about-thepagedirective"></a>Peki`@Page`yönergesi?

Ne biraz kafa karıştırıcı içerik sayfalarını olan `MasterPageFile` özellikleri artık belirtilen iki yerde: programlı olarak `BasePage` sınıfın `OnPreInit` yöntemi de üzerinden `MasterPageFile` her içerik sayfasının özniteliği `@Page` yönergesi.

İlk sayfa yaşam döngüsü aşamasında başlatma aşamadır. Bu aşamasında `Page` nesnenin `MasterPageFile` özellik değerini atandığı `MasterPageFile` özniteliğini `@Page` yönergesi (belirtilmişse). Başlatma aşaması PreInit aşama izler ve biz programlı olarak ayarlandığı aşağıda verilmiştir `Page` nesnenin `MasterPageFile` özelliği, böylece gelen atanan değerin üzerine `@Page` yönergesi. Şu çünkü `Page` nesnenin `MasterPageFile` özelliğini program aracılığıyla, biz kaldırın `MasterPageFile` özniteliğini `@Page` son kullanıcı deneyimini etkilemeden yönergesi. Kendinizi bu tür kötü yazılımlar için devam edin ve kaldırma `MasterPageFile` özniteliğini `@Page` yönergesini `Default.aspx` ve sonra da bir tarayıcı aracılığıyla sayfasını ziyaret edin. Beklediğiniz gibi öznitelik kaldırıldı önce çıktıyı aynıdır.

Olmadığını `MasterPageFile` özelliği aracılığıyla ayarlanır `@Page` yönergesi veya programlama yoluyla son kullanıcı deneyimi için önemsizdir. Ancak, `MasterPageFile` özniteliğini `@Page` yönergesi tarafından Visual Studio tasarım zamanı sırasında WYSIWYG üretmek için kullanılan Görünüm Tasarımcısı'nda. İçin dönüş yaparsa `Default.aspx` Visual Studio'da ve gidin tasarımcıya iletisini görürsünüz "ana sayfa hatası: sayfa bir ana sayfa başvurusu gerektiren denetimleri var, ancak hiçbiri belirtilen" (bkz: Şekil 2).

Kısacası, çıkmak ihtiyacınız `MasterPageFile` özniteliğini `@Page` yönergesi Visual Studio'da zengin bir tasarım zamanı deneyimin keyfini çıkarmak için.


[![Visual Studio kullanan @Page Tasarım görünümü işlemek için yönergesinin MasterPageFile özniteliği](specifying-the-master-page-programmatically-cs/_static/image5.png)](specifying-the-master-page-programmatically-cs/_static/image4.png)

**Şekil 02**: Visual Studio kullanan `@Page` yönergesinin `MasterPageFile` Tasarım görünümüne işleyecek özniteliği ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-master-page-programmatically-cs/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>3. adım: bir alternatif ana sayfa oluşturma

Bir içerik sayfasının ana sayfayı programlı olarak çalışma zamanında ayarlanabildiğinden bazı dış ölçütlere göre belirli bir ana sayfa dinamik olarak yüklemek mümkündür. Bu işlev, sitenin düzeni değiştirmek için kullanıcı temelinde gereken yere durumlarda yararlı olabilir. Örneğin, bir blog altyapısı web uygulaması her Düzen farklı bir ana sayfayla ilişkili olduğu, kullanıcıların kendi blog için bir düzen seçin izin verebilir. Bir kullanıcının blog ziyaretçisi görüntülerken çalışma zamanında, blog düzenini belirlemek ve dinamik olarak karşılık gelen ana sayfa içerik sayfası ile ilişkilendirmek web uygulaması gerekir.

Bir ana sayfa bazı dış ölçütlere göre çalışma zamanında dinamik olarak yükleme inceleyelim. Web sitemizde şu anda yalnızca bir ana sayfa içerir (`Site.master`). Çalışma zamanında bir ana sayfa seçme göstermek için başka bir ana sayfa ihtiyacımız var. Bu adım, oluşturma ve ana sayfada yapılandırma üzerinde odaklanır. 4. adım, çalışma zamanında kullanmak için hangi ana sayfa belirlemede arar.

Adlı kök klasöründe yeni bir ana sayfa oluşturma `Alternate.master`. Ayrıca adlı bir Web sitesi için yeni bir stil sayfası Ekle `AlternateStyles.css`.


[![Başka bir Web sitesine dosya ana sayfa ve CSS](specifying-the-master-page-programmatically-cs/_static/image8.png)](specifying-the-master-page-programmatically-cs/_static/image7.png)

**Şekil 03**: başka bir ana sayfa ekleyin ve Web sitesine CSS dosyası ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-master-page-programmatically-cs/_static/image9.png))


Şekilde değiştirdik `Alternate.master` Lacivert arka plan üzerinde ortalanmış sayfasında ve üst gördüğü başlık sağlamak için ana sayfa. Sol sütununda dispensed ve içeriğin altına taşındı `MainContent` artık sayfayı genişliğinin tamamını kapsayan ContentPlaceHolder denetimi. Ayrıca, sırasız dersleri listesi nixed ve bir yatay liste yukarıdaki yerine `MainContent`. Ben ayrıca yazı tipleri ve renkler ana sayfa (ve uzantısı, içerik sayfalarını) kullanılan güncelleştirildi. Şekil 4'te gösterildiği `Default.aspx` kullanırken `Alternate.master` ana sayfa.

> [!NOTE]
> ASP.NET tanımlama yeteneği içerir *Temalar*. Bir tema, görüntüler, CSS dosyaları ve çalışma zamanında bir sayfaya uygulanan stil Web denetimi özellik ayarları koleksiyonudur. Temalar, sitenizin düzenleri yalnızca görüntülenen görüntüleri ve bunların CSS kurallarını farklıysa Git yoludur. Düzenleri kullanarak farklı Web denetimleri veya önemli ölçüde farklı bir düzene sahip gibi daha önemli ölçüde farklıysa ayrı ana sayfalar kullanmanız gerekir. Temalar hakkında daha fazla bilgi için bu öğreticinin sonunda daha fazla bilgi bölümüne bakın.


[![İçerik Sayfalarımızın artık yeni bir görünüme kullanabilirsiniz](specifying-the-master-page-programmatically-cs/_static/image11.png)](specifying-the-master-page-programmatically-cs/_static/image10.png)

**Şekil 04**: içerik Sayfalarımızın artık yeni bir görünüme kullanabilirsiniz ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-master-page-programmatically-cs/_static/image12.png))


Ana ve içerik sayfalarını biçimlendirme çarpım, `MasterPage` sınıfı her içerik emin olmak için denetimleri içerik sayfasındaki denetimi ana sayfasında bir ContentPlaceHolder başvuruyor. Mevcut olmayan ContentPlaceHolder başvuran bir içerik denetimi bulunursa, bir özel durum oluşturulur. Diğer bir deyişle, ana sayfa için içerik sayfası ile atanan bir ContentPlaceHolder her biri için olmasını zorunlu içerik sayfası denetiminde içerik.

`Site.master` Ana sayfa dört ContentPlaceHolder denetim içerir:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Bazı Web içerik sayfalarında yalnızca bir veya iki içerik denetimlerini içerir. Her bir kullanılabilir ContentPlaceHolder için bir içerik denetimi duyabilir. Varsa yeni ana sayfamızı (`Alternate.master`) hiç olmadığı kadar yalnızca tüm ContentPlaceHolder içinde içerik denetimlerine sahip içerik sayfalar atanabilir `Site.master` önemli ise, `Alternate.master` aynıContentPlaceHolderdenetimdeiçerir`Site.master`.

Almak için `Alternate.master` aşağıdakine benzer (bkz: Şekil 4) Madencilik, ana sayfanın stillerde tanımlayarak işe başlamak için ana sayfa `AlternateStyles.css` stil sayfası. İçine aşağıdaki kurallar ekleme `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-cs/samples/sample5.css)]

Ardından, aşağıdaki bildirim temelli işaretlemede ekleyin `Alternate.master`. Gördüğünüz gibi `Alternate.master` aynı dört ContentPlaceHolder denetimleri içeren `ID` değerleri ContentPlaceHolder denetimleri olarak `Site.master`. Ayrıca, ASP.NET AJAX framework kullanan bu sayfaları için Web sitemizi gerekli olan bir ScriptManager denetimi içerir.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Yeni ana sayfasını test etme

Bu yeni bir ana sayfa güncelleştirmesi test etmek için `BasePage` sınıfın `OnPreInit` yöntemi böylece `MasterPageFile` özelliği, bir değerin atandığı "~ / Alternate.master" ve ardından Web sitesini ziyaret edin. Her sayfanın dışında iki hatasız çalışmalıdır: `~/Admin/AddProduct.aspx` ve `~/Admin/Products.aspx`. Bir ürün DetailsView ekleyerek `~/Admin/AddProduct.aspx` sonuçlanır bir `NullReferenceException` ana sayfanın ayarlama girişiminde kod satırından `GridMessageText` özelliği. Ziyaret `~/Admin/Products.aspx` bir `InvalidCastException` sayfa yükleme iletisi oluşturulur: "nesne türünün yayımlanamıyor ' ASP.alternate\_ana ' türü için ' ASP.site\_ana '."

Bu hatalar meydana `Site.master` arka plan kod sınıfı içeren genel olaylar, özellikler ve yöntemler içinde tanımlı değil `Alternate.master`. Bu iki sayfa biçimlendirme kısmı sahip bir `@MasterType` başvuran yönergesi `Site.master` ana sayfa.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample7.aspx)]

Ayrıca, DetailsView's `ItemInserted` olay işleyicisinde `~/Admin/AddProduct.aspx` geniş yazılmış e yayınlayan kod içeren `Page.Master` türünde bir nesne özelliğini `Site`. `@MasterType` Yönergesi (Bu şekilde kullanılır) ve yayın `ItemInserted` olay işleyicisi sıkı bir şekilde tüm çiftler `~/Admin/AddProduct.aspx` ve `~/Admin/Products.aspx` sayfaları için `Site.master` ana sayfa.

Olan bu sıkı bağ bölüneceği `Site.master` ve `Alternate.master` ortak üyeleri için tanımları içeren genel bir temel sınıf türetin. Biz güncelleştirebilirsiniz `@MasterType` başvuru bu ortak temel türüne yönergesi.

### <a name="creating-a-custom-base-master-page-class"></a>Özel bir temel ana sayfa sınıfı oluşturma

Yeni bir sınıf dosyası ekleyin `App_Code` adlı klasöre `BaseMasterPage.cs` ve türetilen `System.Web.UI.MasterPage`. Tanımlamak ihtiyacımız `RefreshRecentProductsGrid` yöntemi ve `GridMessageText` özelliğinde `BaseMasterPage`, ancak biz yalnızca bunlardan var. taşıyamazsınız `Site.master` bu üyeleri için özel Web denetimlerini çalışmak için `Site.master` ana sayfa ( `RecentProducts` GridView ve `GridMessage` etiketi).

Yapılandırma yapmak ihtiyacımız olan `BaseMasterPage` bu üyeleri var. tanımlandı, ancak gerçekte tarafından uygulanan bir şekilde `BaseMasterPage`türetilmiş sınıflar (`Site.master` ve `Alternate.master`). Sınıf ve onun üyeleri olarak işaretleyerek bu tür bir devralma mümkündür `abstract`. Kısacası, ekleme `abstract` anahtar sözcüğü bu iki üyelere duyuruyor `BaseMasterPage` uygulanan taşınmadığından `RefreshRecentProductsGrid` ve `GridMessageText`ondan türetilen sınıflardan olur, ancak.

Ayrıca tanımlamak ihtiyacımız `PricesDoubled` olayında `BaseMasterPage` ve olay oluşturmak için türetilmiş sınıfları tarafından bir yol sağlar. .NET Framework'teki bu davranışı kolaylaştırmak için kullanılan Düzen temel sınıfta ortak olay oluşturun ve korumalı, eklemektir `virtual` adlı yöntem `OnEventName`. Türetilen sınıfların ardından olayı oluşturmak için bu yöntemi çağırabilir veya hemen önce veya sonra olayı tetikleyen kod yürütmek için geçersiz kılabilirsiniz.

Güncelleştirme, `BaseMasterPage` aşağıdaki kodu içeren sınıf:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample8.cs)]

Ardından, Git `Site.master` arka plan kod sınıfı ve varsa, türetilen `BaseMasterPage`. Çünkü `BaseMasterPage` olduğu `abstract` geçersiz kılar ihtiyacımız `abstract` burada üyeleri `Site.master`. Ekleme `override` yöntem ve özellik tanımları için anahtar sözcüğü. Ayrıca oluşturan kodu güncelleştirmeniz `PricesDoubled` olayında `DoublePrice` düğmenin `Click` olay işleyicisi temel sınıfın çağrısıyla `OnPricesDoubled` yöntemi.

Bu değişiklikler sonra `Site.master` arka plan kod sınıfı aşağıdaki kodu içermelidir:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample9.cs)]

Biz de güncelleştirmeniz gerekiyor `Alternate.master`'s türetmek için arka plan kod sınıfı `BaseMasterPage` ve iki geçersiz kılma `abstract` üyeleri. Ancak `Alternate.master` listeleri, en son ürün ya da sonra yeni bir ürün ileti görüntüleyen bir etiket veritabanına eklenen bu yöntemlerin herhangi bir şey gerekmez, GridView içermiyor.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample10.cs)]

### <a name="referencing-the-base-master-page-class"></a>Temel ana sayfası sınıfı başvurma

Biz tamamladığınıza göre `BaseMasterPage` sınıfı ve genişletmeden bizim iki ana sayfa, son adımımız güncelleştirilecek `~/Admin/AddProduct.aspx` ve `~/Admin/Products.aspx` ortak bu türe başvurmak amacıyla sayfaları. Değiştirerek başlayın `@MasterType` hem sayfalardan yönergesi:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample11.aspx)]

Hedef:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample12.aspx)]

Bir dosya yolu başvuran yerine `@MasterType` özelliği, temel türünü artık başvurur (`BaseMasterPage`). Sonuç olarak, kesin türü belirtilmiş `Master` her iki sayfa arka plan kod sınıflarda kullanılan özelliği, artık türü `BaseMasterPage` (tür yerine `Site`). Bu değişikliği yerinde yeniden ziyaret `~/Admin/Products.aspx`. Daha önce bu bir atama hatayla sonuçlandı sayfa kullanmak üzere yapılandırılmış olduğundan `Alternate.master` ana sayfaya, ancak `@MasterType` başvurulan yönergesi `Site.master` dosya. Ancak artık sayfayı hatasız işler. Bunun nedeni, `Alternate.master` ana sayfa türü bir nesneye yayımlanabilir `BaseMasterPage` (bunu genişletir beri).

İçinde yapılması gereken bir küçük değişiklik `~/Admin/AddProduct.aspx`. DetailsView denetimin `ItemInserted` olay işleyicisi kullandığı hem de türü kesin belirlenmiş `Master` özelliği ve geniş yazılmış `Page.Master` özelliği. Kesin tür belirtilmiş başvuru düzelttik güncelleştirdik olduğunda `@MasterType` yönergesi, ancak yine de ihtiyacınız geniş yazılmış başvurusu güncelleştirilecek. Aşağıdaki kod satırını değiştirin:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample13.cs)]

Şunlarla birlikte hangi bıraktığı `Page.Master` temel türü için:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample14.cs)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>4. adım: Hangi ana sayfa içerik sayfalarını bağlanacağı belirleme

Bizim `BasePage` sınıfı, şu anda tüm içerik sayfalarının ayarlar `MasterPageFile` PreInit sayfa yaşam döngüsü aşaması sabit kodlu değer özellikleri. Biz bu kodu ana sayfaya bazı dış faktörünün temel güncelleştirebilirsiniz. Belki de yüklemek için ana sayfa şu anda oturum açmış kullanıcı tercihlerine göre değişir. Bu durumda, kod yazmaya ihtiyacımız `OnPreInit` yönteminde `BasePage` şu anda ziyaret kullanıcının ana sayfa tercihlerini arar.

Kullanılacak - ana sayfasını seçmesine izin veren bir web sayfası oluşturalım `Site.master` veya `Alternate.master` - ve bu seçenek bir oturumu değişkene kaydedin. Adlı kök dizininde yeni bir web sayfası oluşturarak başlayın `ChooseMasterPage.aspx`. Bu sayfayı (veya diğer içerik sayfaları henceforth) oluştururken ana sayfayı programlı olarak ayarlandığından, bağlama ana sayfaya gerekmez `BasePage`. Ana sayfaya yeni sayfa bağlamayın ancak bildirim temelli varsayılan biçimlendirme, yeni sayfa bir Web formu ve ana sayfa tarafından sağlanan diğer içerik içerir. Bu işaretleme uygun içerik denetimleri ile el ile değiştirmeniz gerekir. Bu nedenle, ben yeni ASP.NET sayfası için bir ana sayfa bağlamak daha kolay.

> [!NOTE]
> Çünkü `Site.master` ve `Alternate.master` hangi ana sayfaya yeni içerik sayfası oluştururken seçtiğiniz farketmez aynı ContentPlaceHolder denetimleri ayarladınız. Tutarlılık sağlamak için kullanarak miyim önermek `Site.master`.


[![Web sitesine yeni bir içerik sayfası Ekle](specifying-the-master-page-programmatically-cs/_static/image14.png)](specifying-the-master-page-programmatically-cs/_static/image13.png)

**Şekil 05**: Web sitesine yeni bir içerik sayfası ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-master-page-programmatically-cs/_static/image15.png))


Güncelleştirme `Web.sitemap` bu ders için bir giriş eklemek için dosya. Altında aşağıdaki işaretlemeyi ekleyin `<siteMapNode>` ana sayfalar ve ASP.NET AJAX ders için:


[!code-xml[Main](specifying-the-master-page-programmatically-cs/samples/sample15.xml)]

Herhangi bir içerik eklemeden önce `ChooseMasterPage.aspx` sayfasına gitmek türetildiği için sayfa arka plan kod sınıfı güncelleştirmeye biraz `BasePage` (yerine `System.Web.UI.Page`). Ardından, sayfaya bir DropDownList denetimi ekleyin, kendi `ID` özelliğini `MasterPageChoice`, ve iki ListItems ile ekleme `Text` değerlerini "~ / Site.master" ve "~ / Alternate.master".

Bir düğme Web denetimi için bir sayfa ekleyin ve ayarlayın, `ID` ve `Text` özelliklerine `SaveLayout` ve "Düzen Seçimi Kaydet", sırasıyla. Bu noktada bildirim temelli işaretleme, sayfanın aşağıdakine benzer görünmelidir:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample16.aspx)]

Sayfa ilk ziyaret edildiğinde kullanıcının şu anda seçili ana sayfa seçimini görüntülenecek ihtiyacımız var. Oluşturma bir `Page_Load` olay işleyicisi ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample17.cs)]

Yukarıdaki kod, yalnızca ilk sayfasını ziyaret edin (ve sonraki Geri göndermeler göre değil) yürütür. Bu ilk olup olmadığını denetler oturum değişkeni `MyMasterPage` bulunmaktadır. Aksi halde, eşleşen ListItem bulmaya çalışır `MasterPageChoice` DropDownList. Eşleşen bir ListItem bulunursa, `Selected` özelliği `true`.

Ayrıca, kullanıcının seçimini içine kaydettiği kod ihtiyacımız `MyMasterPage` oturum değişkeni. İçin bir olay işleyicisi oluşturun `SaveLayout` düğmenin `Click` olay ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample18.cs)]

> [!NOTE]
> Zamana göre `Click` olay işleyicisi geri göndermede yürütür, ana sayfa zaten seçilmiş. Bu nedenle, sonraki sayfaya ziyaret ederek kullanıcının açılan liste seçimine etkin olmayacaktır. `Response.Redirect` Yeniden istemek için tarayıcı zorlar `ChooseMasterPage.aspx`.


İle `ChooseMasterPage.aspx` sayfası tümü, bizim son görevdir olmasını `BasePage` atama `MasterPageFile` özellik değerini temel alarak `MyMasterPage` oturum değişkeni. Oturum değişkeni ayarlanmamış varsa `BasePage` varsayılan `Site.master`.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample19.cs)]

> [!NOTE]
> Ben atar kod taşınabilir `Page` nesnenin `MasterPageFile` tanesi özelliği `OnPreInit` olay işleyicisi ve iki ayrı yöntemlerde. Bu ilk yöntem `SetMasterPageFile`, atar `MasterPageFile` ikinci yöntem tarafından döndürülen değer özelliğini `GetMasterPageFileFromSession`. Yaptığım `SetMasterPageFile` yöntemi `virtual` böylece gelecekteki sınıfları, genişleten `BasePage` isteğe bağlı olarak, özel mantığı uygulamak için gerekirse geçersiz kılabilirsiniz. Geçersiz kılma örneği görüyoruz `BasePage`'s `SetMasterPageFile` sonraki öğreticide özelliği.


Bu kod bir yerde ziyaret `ChooseMasterPage.aspx` sayfası. Başlangıçta `Site.master` ana sayfasıdır seçilir (bkz. Şekil 6), ancak kullanıcı farklı bir ana sayfa aşağı açılan listeden seçim yapabilirsiniz.


[![İçerik sayfaları Site.master ana sayfa kullanan görüntülenir](specifying-the-master-page-programmatically-cs/_static/image17.png)](specifying-the-master-page-programmatically-cs/_static/image16.png)

**Şekil 06**: içerik sayfalarıdır görüntülenen kullanarak `Site.master` ana sayfa ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-master-page-programmatically-cs/_static/image18.png))


[![İçerik sayfaları, artık Alternate.master ana sayfa kullanan görüntülenir](specifying-the-master-page-programmatically-cs/_static/image20.png)](specifying-the-master-page-programmatically-cs/_static/image19.png)

**Şekil 07**: içerik sayfalarıdır şimdi görüntülenen kullanarak `Alternate.master` ana sayfa ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-master-page-programmatically-cs/_static/image21.png))


## <a name="summary"></a>Özet

İçerik denetimlerini, bir içerik sayfasını ziyaret edildiğinde, kendi ana sayfanın ContentPlaceHolder denetimleriyle çarpım. Ana sayfa içerik sayfasının çağrılarınızla `Page` sınıfın `MasterPageFile` atandığı özelliği `@Page` yönergesinin `MasterPageFile` başlatma aşaması sırasında öznitelik. Bu öğretici gösterdi biz için bir değer atayabilirsiniz `MasterPageFile` PreInit aşamanın bitiminden önce biz bunu sürece özelliği. Ana sayfayı programlı olarak belirtmek için kapının dış unsurlarına göre ana sayfa içerik sayfası dinamik olarak bağlama gibi daha gelişmiş senaryoları için açılır.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET sayfası yaşam döngüsü diyagramı](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [ASP.NET sayfası yaşam döngüsüne genel bakış](https://msdn.microsoft.com/library/ms178472.aspx)
- [ASP.NET temaları ve dış genel bakış](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Ana sayfalar: İpuçları, püf noktalarını ve tuzakları](http://www.odetocode.com/articles/450.aspx)
- [ASP.NET temaları](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar 1998'de bu yana birden çok ASP/ASP.NET books ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileri ile çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott, konumunda ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Suchi Banerjee oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](master-pages-and-asp-net-ajax-cs.md)
> [İleri](nested-master-pages-cs.md)
