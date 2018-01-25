---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: "Ana sayfa programlı olarak belirtme (VB) | Microsoft Docs"
author: rick-anderson
description: "İçerik sayfasının ana sayfa PreInit olay işleyicisi aracılığıyla programlı olarak ayarlama arar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 191de7546e2ba913fda0c8c8a8bfd3531b53336e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="specifying-the-master-page-programmatically-vb"></a>Ana sayfa programlı olarak belirtme (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> İçerik sayfasının ana sayfa PreInit olay işleyicisi aracılığıyla programlı olarak ayarlama arar.


## <a name="introduction"></a>Giriş

Bülteninin açılış sayısına örnekte bu yana [ *bir Site genelinde düzenini kullanarak ana sayfalar oluşturma*](creating-a-site-wide-layout-using-master-pages-vb.md), tüm içerik sayfalarının kendi ana sayfa bildirimli olarak aracılığıyla başvurulan `MasterPageFile` özniteliğinde`@Page`yönergesi. Örneğin, aşağıdaki `@Page` yönergesi bağlantılar içerik sayfasını ana sayfaya `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

[ `Page` Sınıfı](https://msdn.microsoft.com/library/system.web.ui.page.aspx) içinde `System.Web.UI` ad alanı içeren bir [ `MasterPageFile` özelliği](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) içerik sayfasının ana sayfa yolunu döndürür; ayarlandığındanbuözelliğidir`@Page` yönergesi. Bu özellik, program aracılığıyla içerik sayfasının ana sayfa belirtmek için de kullanılabilir. Bu yaklaşım, ana sayfa sayfasını ziyaret kullanıcı gibi dış faktörlere göre dinamik olarak atamak istiyorsanız kullanışlıdır.

Bu öğreticide bizim Web sitesine ikinci bir ana sayfa ekleyin ve dinamik olarak çalışma zamanında kullanılacak ana sayfayı karar verin.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>1. adım: Göz sayfa yaşam döngüsü

Web sunucusunda bir içerik sayfasının bir ASP.NET sayfası için bir istek geldiğinde ASP.NET altyapısı sayfanın Sigortası içerik ana sayfa denetimlere karşılık gelen ContentPlaceHolder denetimleri. Bu fusion tipik sayfa yaşam döngüsü sonra devam edebilirsiniz tek denetim hiyerarşisi oluşturur.

Şekil 1 Bu fusion gösterilmektedir. 1. adım Şekil 1'deki ana sayfa denetim hiyerarşileri ve başlangıç içeriğini gösterir. İçeriği PreInit aşama tail sonunda sayfasındaki denetimleri karşılık gelen ContentPlaceHolders için ana sayfa (2. adım) olarak eklenir. Bu fusion sonra ana sayfa Sigortalı denetim hiyerarşisinin kökü görev yapar. Bu denetim Sigortalı hiyerarşi sonra (adım 3) sonlandırılmış denetim hiyerarşisi üretmek için sayfasına eklenir. Net sonucu sayfanın denetim hiyerarşisi Sigortalı denetim hiyerarşisi dahildir.


[![Ana sayfa ve içerik sayfasının denetim hiyerarşileri Sigortalı PreInit aşamasında birbirine](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**Şekil 01**: ana sayfa ve içerik sayfasının denetim hiyerarşileri birleştirilir Sigortalı PreInit aşamasında ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-master-page-programmatically-vb/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>2. adım: Ayarlama`MasterPageFile`kodundan özelliği

Bu fusion hangi ana sayfa partakes değerine bağlıdır `Page` nesnenin `MasterPageFile` özelliği. Ayarı `MasterPageFile` özniteliğini `@Page` yönergesi etkisi atama net `Page`'s `MasterPageFile` ilk sayfa yaşam döngüsü aşaması başlatma aşaması sırasında özelliği. Biz alternatif olarak bu özelliği programlı olarak ayarlayabilirsiniz. Ancak, Şekil 1'deki fusion gerçekleşmeden önce bu özelliğinin ayarlanması zorunludur.

PreInit aşamasının başlangıcında `Page` nesnesini başlatır, [ `PreInit` olay](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) ve çağırır kendi [ `OnPreInit` yöntemi](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Ana sayfaya programlı olarak ayarlamak için daha sonra biz oluşturabilir ya da bir olay işleyicisi için `PreInit` olay veya geçersiz kılma `OnPreInit` yöntemi. Her iki yaklaşımın bakalım.

Başlangıç açarak `Default.aspx.vb`, bizim sitenin giriş sayfası için arka plan kodu sınıf dosyası. Sayfa için bir olay işleyicisi ekleme `PreInit` aşağıdaki kodda yazarak olay:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

Buradan ayarlarız `MasterPageFile` özelliği. Böylece değeri atar kodunu güncelleştirin "~ / Site.master" için `MasterPageFile` özelliği.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

Bir kesme noktası ayarlayın ve hata ayıklama ile başlangıç, göreceksiniz her `Default.aspx` sayfasını ziyaret veya ne zaman bu sayfaya geri gönderimin var. `Page_PreInit` olay işleyicisi yürütür ve `MasterPageFile` özelliği atanması "~ / Site.master".

Alternatif olarak, kılabilirsiniz `Page` sınıfının `OnPreInit` yöntemi ve kümesi `MasterPageFile` özelliği vardır. Bu örnek için şirketinizdeki belirli bir sayfa ana sayfasında ancak bunun yerine kümeden değil `BasePage`. Özel ana sayfa sınıfı oluşturduğumuz geri çağırma (`BasePage`) geri [ *başlık, Meta etiketler ve diğer HTML üstbilgileri ana sayfasında belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) Öğreticisi. Şu anda `BasePage` geçersiz kılmaları `Page` sınıfının `OnLoadComplete` yöntemi, burada bu ayarlar sayfanın `Title` özelliğini ve site haritası verilerine dayalı. Şimdi güncelleştirme `BasePage` ayrıca geçersiz kılmak için `OnPreInit` yöntemi ana sayfa programlı olarak belirtin.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

Bizim tüm içerik sayfalarının türetin çünkü `BasePage`, bunların tümünün artık programlı olarak atanan kendi ana sayfa vardır. Bu noktada `PreInit` olay işleyicisini `Default.aspx.vb` gereksiz; kaldırmaya çekinmeyin.

### <a name="what-about-thepagedirective"></a>Ne hakkında`@Page`yönergesi?

Ne biraz kafa karıştırıcı olabilir içerik sayfaları olan `MasterPageFile` özellikleri artık belirtilen iki yerde: programlı olarak `BasePage` sınıfının `OnPreInit` yöntemi ile de olarak `MasterPageFile` her içerik sayfasının özniteliği `@Page` yönergesi.

Sayfa yaşam döngüsü ilk aşamada başlatma aşaması değil. Bu aşama sırasında `Page` nesnenin `MasterPageFile` özellik değeri atanmış `MasterPageFile` özniteliğini `@Page` (sağlanmışsa) yönergesi. Başlatma aşaması PreInit aşama izler ve burada program aracılığıyla ayarlarız aşağıda verilmiştir `Page` nesnenin `MasterPageFile` özelliği, böylece gelen atanan değer üzerine `@Page` yönergesi. Şu çünkü `Page` nesnenin `MasterPageFile` özelliği program aracılığıyla, biz kaldırmak `MasterPageFile` özniteliğini `@Page` son kullanıcı deneyimi etkilemeden yönergesi. Kendiniz bu sağlamak için bir tane kaldırmak `MasterPageFile` özniteliğini `@Page` yönergesini `Default.aspx` ve ardından bir tarayıcı aracılığıyla sayfasını ziyaret edin. Beklediğiniz gibi öznitelik kaldırıldı önce çıktıyı aynıdır.

Olup olmadığını `MasterPageFile` özelliği aracılığıyla ayarlanır `@Page` yönergesi veya program aracılığıyla son kullanıcı deneyimi önemsizdir. Ancak, `MasterPageFile` özniteliğini `@Page` yönergesi tarafından Visual Studio tasarım-zamanı sırasında WYSIWYG üretmek için kullanılan Görünüm Tasarımcısı'nda. İçin dönerseniz `Default.aspx` Visual Studio'da giderek Designer'a iletisini görürsünüz "ana sayfa hatası: sayfa bir ana sayfa başvurusu gerektiren denetimler içeriyor, ancak hiçbiri belirtilen" (bkz: Şekil 2).

Kısacası, çıkmak ihtiyacınız `MasterPageFile` özniteliğini `@Page` yönergesi bir Visual Studio tasarım-zamanı deneyim keyfini çıkarın.


[![Visual Studio kullanan @Page Tasarım görünümü işlemek için yönergesi'nın MasterPageFile özniteliği](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**Şekil 02**: Visual Studio kullanan `@Page` yönergesi'nın `MasterPageFile` Tasarım görünümüne işleyecek özniteliği ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-master-page-programmatically-vb/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>3. adım: bir alternatif ana sayfa oluşturma

Bir içerik sayfasının ana sayfasında program aracılığıyla çalışma zamanında ayarlanabildiğinden belirli bir ana sayfa bazı dış ölçütlere göre dinamik olarak yüklemek mümkündür. Bu işlev nerede sitenin Düzen farklılık kullanıcı göre gereken durumlarda yararlı olabilir. Örneğin, bir blog altyapısı web uygulaması her düzeni farklı bir ana sayfa ile ilişkili olduğu kullanıcılarına kendi blog için bir düzen seçin izin verebilir. Bir kullanıcının blog ziyaretçisi görüntülerken çalışma zamanında blog düzenini belirlemek ve karşılık gelen ana sayfa ile içerik sayfasını dinamik olarak ilişkilendirmek web uygulaması gerekir.

Bir ana sayfa bazı dış ölçütlere göre çalışma zamanında dinamik olarak yükleme inceleyelim. Bizim Web sitesinde şu anda yalnızca bir ana sayfa bulunur (`Site.master`). Çalışma zamanında bir ana sayfa seçme göstermek için başka bir ana sayfa ihtiyacımız var. Bu adım, oluşturma ve yeni ana sayfa yapılandırma odaklanır. Adım 4 çalışma zamanında kullanılacak hangi ana sayfa belirlemede arar.

Adlı kök klasöründe yeni bir ana sayfa oluşturma `Alternate.master`. Ayrıca yeni bir stil sayfası adlı Web sitesi Ekle `AlternateStyles.css`.


[![Başka bir tane eklemek için Web sitesi dosya ana sayfa ve CSS](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**Şekil 03**: başka bir ana sayfa ekleyin ve Web sitesine CSS dosyası ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-master-page-programmatically-vb/_static/image9.png))


Tasarlanmış `Alternate.master` sayfasının Ortalanan ve Lacivert arka plan üzerinde üstünde gördüğü başlık sağlamak için ana sayfa. Sol sütunu dispensed ve içeriğin altına taşındı `MainContent` şimdi sayfanın tüm genişliği yayılan ContentPlaceHolder denetimi. Ayrıca, sırasız dersleri listesini nixed ve bir yatay listesi yukarıdaki yerini `MainContent`. I da yazı tiplerini ve renkleri ana sayfa tarafından (ve uzantılarının, içerik sayfaları) kullanılan güncelleştirildi. Şekil 4 gösterir `Default.aspx` kullanırken `Alternate.master` ana sayfa.

> [!NOTE]
> ASP.NET içeren tanımlama yeteneği *Temalar*. Bir tema, görüntüler, CSS dosyaları ve çalışma zamanında bir sayfaya uygulanan stil ilgili Web denetim özellik ayarları koleksiyonudur. Temalar, sitenizin düzenleri yalnızca görüntülenen görüntüleri ve bunların CSS kuralları tarafından farklıysa Git yoludur. Düzenleri farklı Web denetimleri kullanarak veya tüketicisinin farklı bir düzen olması gibi daha önemli ölçüde farklıysa ayrı ana sayfalar kullanmanız gerekir. Temalar hakkında daha fazla bilgi için bu öğreticinin sonunda daha fazla bilgi bölümüne başvurun.


[![Bizim içerik sayfaları artık yeni bir görünüm kullanabilirsiniz](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**Şekil 04**: bizim içerik sayfaları artık yeni bir görünüm kullanabilirsiniz ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-master-page-programmatically-vb/_static/image12.png))


Ana ve içerik sayfaları biçimlendirme Sigortalı, `MasterPage` sınıfı her içerik emin olmak için denetimleri içerik sayfasını denetiminde ContentPlaceHolder ana sayfasında başvuruyor. Mevcut olmayan ContentPlaceHolder başvuruda bulunan içerik denetimi bulunursa, bir özel durum oluşur. Diğer bir deyişle, içerik sayfasına atanmasını ana sayfa bir ContentPlaceHolder her biri için olduğunu kesinlik temelli içerik, içerik sayfasındaki denetimi.

`Site.master` Ana sayfa dört ContentPlaceHolder denetimleri içerir:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Bizim Web sitesi içerik sayfalarına bazıları, yalnızca bir veya iki içerik denetimleri içerir; Her kullanılabilir ContentPlaceHolders için içerik denetimi duyabilir. Varsa yeni ana sayfamızı (`Alternate.master`) içinde ContentPlaceHolders için tüm içerik denetimlerine sahip bu içerik sayfalarına hiç atanabilir `Site.master` gerekli ise, `Alternate.master` aynıContentPlaceHolderdenetimdeiçerir`Site.master`.

Almak için `Alternate.master` (bkz: Şekil 4) benim, ana sayfa stillerde tanımlayarak işe başlamak için benzer ana sayfa `AlternateStyles.css` stil sayfası. Aşağıdaki kuralları içine ekleme `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

Ardından, aşağıdaki bildirim temelli biçimlendirme eklemek `Alternate.master`. Gördüğünüz gibi `Alternate.master` aynı dört ContentPlaceHolder denetimleri içeren `ID` değerleri ContentPlaceHolder denetimlerinde olarak `Site.master`. Ayrıca, ASP.NET AJAX framework kullanan bu sayfalar için bizim Web sitesinde gereklidir bir ScriptManager denetimi içerir.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Yeni ana sayfa test etme

Bu yeni ana sayfa güncelleştirme test etmek için `BasePage` sınıfının `OnPreInit` yöntemi böylece `MasterPageFile` özellik değeri atanmış `"~/Alternate.maser"` ve Web sitesini ziyaret edin. Her sayfada iki dışında hatasız çalışması: `~/Admin/AddProduct.aspx` ve `~/Admin/Products.aspx`. Bir ürün DetailsView'da ekleme `~/Admin/AddProduct.aspx` sonuçlanan bir `NullReferenceException` ana sayfanın ayarlamaya çalışır kod satırından `GridMessageText` özelliği. Ziyaret eden `~/Admin/Products.aspx` bir `InvalidCastException` sayfa yükü iletiyle oluşturulur: "nesne türünün ' ASP.alternate\_ana ' türü için ' ASP.site\_ana '."

Bu hatalar nedeniyle oluşur `Site.master` arka plandaki kod sınıfı içeren ortak olaylar, özellikleri ve içinde tanımlı değil yöntemleri `Alternate.master`. Bu iki sayfaları biçimlendirme kısmı sahip bir `@MasterType` başvuruyor yönergesi `Site.master` ana sayfa.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

Ayrıca, DetailsView'un `ItemInserted` olay işleyicisini `~/Admin/AddProduct.aspx` geniş yazılmış çevirir kodu içerir `Page.Master` türünde bir nesne için özellik `Site`. `@MasterType` Yönergesi (Bu şekilde kullanılır) ve cast içinde `ItemInserted` olay işleyicisi sıkı bir şekilde tüm çiftler `~/Admin/AddProduct.aspx` ve `~/Admin/Products.aspx` sayfaları için `Site.master` ana sayfa.

Biz olabilir bu sıkı bağ bölüneceği `Site.master` ve `Alternate.master` Genel üyeler için tanımları içeren genel bir temel sınıf türetin. Biz güncelleştirebilirsiniz `@MasterType` bu ortak bir taban türü başvurmak için yönergesi.

### <a name="creating-a-custom-base-master-page-class"></a>Özel temel ana sayfa sınıfı oluşturma

Yeni bir sınıf dosyası ekleyin `App_Code` adlı klasörü `BaseMasterPage.vb` ve türetilen `System.Web.UI.MasterPage`. Tanımlamak ihtiyacımız `RefreshRecentProductsGrid` yöntemi ve `GridMessageText` özelliğinde `BaseMasterPage`, ancak biz yalnızca bunlardan var. taşıyamazsınız `Site.master` özgü Web denetimleri bu üyeleri çalışmak için `Site.master` ana sayfa ( `RecentProducts` GridView ve `GridMessage` etiketi).

Yapılandırma yapmak ihtiyacımız olan `BaseMasterPage` bu üyeleri var. tanımlı, ancak gerçekte tarafından uygulanan bir şekilde `BaseMasterPage`türetilmiş sınıfları (`Site.master` ve `Alternate.master`). Sınıf olarak işaretleyerek bu tür devralma mümkündür `MustInherit` ve üyeleri olarak `MustOverride`. Kısacası, sınıf ve iki üyeleri bu anahtar sözcükler ekleme, bildirdi `BaseMasterPage` uygulanan kurmadı `RefreshRecentProductsGrid` ve `GridMessageText`türetilmiş sınıflarının olur, ancak.

Ayrıca tanımlamak ihtiyacımız `PricesDoubled` olayda `BaseMasterPage` ve olay yükseltmek için türetilen sınıflar tarafından bir yol sağlar. Bu davranış kolaylaştırmak için .NET Framework kullanılan deseni taban sınıf içinde ortak bir olay oluşturun ve adlı korumalı, geçersiz kılınabilir bir yöntem eklemektir `OnEventName`. Türetilen sınıflar sonra olay yükseltmek için bu yöntemini çağırabilir veya hemen önce veya sonra olay tetiklenir kod yürütmek için geçersiz kılabilirsiniz.

Güncelleştirme, `BaseMasterPage` aşağıdaki kodu içeren sınıf:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

Ardından, Git `Site.master` arka plandaki kod sınıfı ve sahip öğesinden türetilen `BaseMasterPage`. Çünkü `BaseMasterPage` işaretlenmiş üyeleri içeren `MustOverride` burada üyeleri geçersiz kılmak ihtiyacımız `Site.master`. Ekleme `Overrides` yöntem ve özellik tanımları anahtar sözcük. Ayrıca başlatır kodu güncelleştirme `PricesDoubled` olayında `DoublePrice` düğmenin `Click` olay işleyicisi temel sınıfın çağrısıyla `OnPricesDoubled` yöntemi.

Bu değişiklikler sonra `Site.master` arka plandaki kod sınıfı, aşağıdaki kodu içermelidir:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

Ayrıca güncelleştirmek ihtiyacımız `Alternate.master`ait arka plan kodu sınıf türetin `BaseMasterPage` ve iki geçersiz kılma `MustOverride` üyeleri. Ancak çünkü `Alternate.master` en son ürün ya da yeni bir ürün sonra bir ileti görüntüler bir etiket veritabanına eklenmiş listeler, bu yöntemleri bir şey yapmanız gerekmez, GridView içermiyor.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>Temel ana sayfa sınıfının başvurma

Biz tamamladığınıza göre `BaseMasterPage` sınıfı ve genişletmeden bizim iki ana sayfa, son adımımız güncelleştirmek için `~/Admin/AddProduct.aspx` ve `~/Admin/Products.aspx` ortak bu türe başvurmak için sayfaları. Başlangıç değiştirerek `@MasterType` hem sayfalardan yönerge:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

Hedef:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

Bir dosya yolu başvuran yerine `@MasterType` özelliği şimdi başvurur temel türü (`BaseMasterPage`). Sonuç olarak, kesin türü belirtilmiş `Master` her iki sayfa arka plan kodu sınıflarda kullanılan özelliktir şimdi türü `BaseMasterPage` (türü yerine `Site`). Bu değişikliği yerinde yeniden ziyaret `~/Admin/Products.aspx`. Daha önce bunlar bu sayfanın kullanmak için yapılandırıldığı için bir atama hatayla sonuçlandı `Alternate.master` ana sayfa, ancak `@MasterType` başvurulan yönergesi `Site.master` dosya. Ancak şimdi hatasız sayfasını işler. Bunun nedeni, `Alternate.master` ana sayfa türünde bir nesneye dönüştürülür `BaseMasterPage` (Bu, genişletir beri).

İçinde yapılmalıdır bir küçük değişiklik `~/Admin/AddProduct.aspx`. DetailsView denetimin `ItemInserted` olay işleyicisi kullanan her iki kesin türü belirtilmiş `Master` özelliği ve geniş yazılmış `Page.Master` özelliği. Biz güncelleştirildiği biz kesin türü belirtilmiş başvuru sabit `@MasterType` yönergesi, ancak hala geniş yazılmış başvuru güncellemeniz gerekir. Aşağıdaki kod satırını değiştirin:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

Aşağıdaki ile hangi bıraktığı `Page.Master` temel türü için:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>4. adım: Hangi ana sayfa içerik sayfalarına Bind belirleme

Bizim `BasePage` sınıfı, şu anda tüm içerik sayfalarının ayarlar `MasterPageFile` özellikler sabit kodlanmış bir değere sayfa yaşam döngüsünün PreInit aşamasında. Biz bu kodu üzerinde bazı dış faktörü ana sayfa temel güncelleştirebilirsiniz. Belki de yüklemek için ana sayfa şu anda oturum açmış kullanıcı tercihlerinize göre değişir. Bu durumda, biz kod yazmanız gerekir `OnPreInit` yönteminde `BasePage` şu anda ziyaret kullanıcının ana sayfa tercihlerini arar.

-Kullanılacak ana sayfayı seçmesine izin veren bir web sayfası oluşturalım `Site.master` veya `Alternate.master` - ve bu seçenek bir oturum değişkene kaydedin. Başlangıç adlı kök dizininde yeni bir web sayfası oluşturarak `ChooseMasterPage.aspx`. Bu sayfa (veya diğer içerik sayfaları henceforth) oluştururken, ana sayfa programlı olarak ayarlandığından ana sayfaya bağlamak gerekmeyen `BasePage`. Ana sayfaya yeni sayfa bağlamayın ancak, yeni sayfanın varsayılan bildirim temelli biçimlendirme Web formu ve ana sayfa tarafından sağlanan diğer içeriği içerir. Bu biçimlendirme uygun içerik denetimleri ile el ile değiştirmeniz gerekir. Bu nedenle, g yeni ASP.NET sayfası bir ana sayfaya bağlamak daha kolay.

> [!NOTE]
> Çünkü `Site.master` ve `Alternate.master` yeni içerik sayfasını oluştururken seçtiğiniz hangi ana sayfa önemli değildir aynı ContentPlaceHolder denetimlerin ayarladınız. Tutarlılık için t kullanarak önerebileceğiniz `Site.master`.


[![Web sitesine yeni bir içerik sayfa ekleyin](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**Şekil 05**: Web sitesine yeni bir içerik sayfası ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-master-page-programmatically-vb/_static/image15.png))


Güncelleştirme `Web.sitemap` dosya bu ders için bir giriş içerir. Altında aşağıdaki biçimlendirmeleri eklemek `<siteMapNode>` ana sayfalar ve ASP.NET AJAX ders için:


[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

Herhangi bir içerik eklemeden önce `ChooseMasterPage.aspx` sayfaya götüren den türemesi için sayfanın arka plandaki kod sınıfı güncelleştirmek için bir dakikanızı `BasePage` (yerine `System.Web.UI.Page`). Ardından, bir DropDownList denetimi sayfasına ekleme, ayarlayın, `ID` özelliğine `MasterPageChoice`, ve iki ListItems ile eklemek `Text` değerleri "~ / Site.master" ve "~ / Alternate.master".

Bir düğme Web denetimi sayfasına ekleyin ve ayarlayın, `ID` ve `Text` özelliklerine `SaveLayout` ve "Düzeni seçim kaydetme", sırasıyla. Bu noktada sayfanızın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

Sayfa ilk sitesini ziyaret ettiğinizde kullanıcının şu anda seçili ana sayfa seçim görüntülemek gerekir. Oluşturma bir `Page_Load` olay işleyicisi ve aşağıdaki kodu ekleyin:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

Yukarıdaki kod, yalnızca ilk sayfasını ziyaret edin (ve sonraki Geri göndermeler üzerinde değil) yürütür. Bu ilk bakar oturum değişkeni `MyMasterPage` bulunmaktadır. Aşması durumunda içinde eşleşen LISTITEM bulmaya çalışır `MasterPageChoice` DropDownList. Eşleşen bir ListItem bulunursa, kendi `Selected` özelliği ayarlanmış `True`.

Ayrıca, kullanıcının seçimini içine kaydeder kod ihtiyacımız `MyMasterPage` oturum değişkeni. İçin bir olay işleyicisi oluşturun `SaveLayout` düğmenin `Click` olay ve aşağıdaki kodu ekleyin:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> Zamana göre `Click` olay işleyicisi geri göndermede çalıştırır, ana sayfa zaten seçilmiş. Bu nedenle, kullanıcının açılan liste seçimini sonraki sayfasını ziyaret edin kadar etkin olmayacaktır. `Response.Redirect` Yeniden istemek için tarayıcı zorlar `ChooseMasterPage.aspx`.


İle `ChooseMasterPage.aspx` sayfası tümü, bizim son görevdir olmasını `BasePage` atamak `MasterPageFile` özellik değeri temel alarak `MyMasterPage` oturum değişkeni. Oturum değişkeni ayarlanmamış varsa `BasePage` varsayılan `Site.master`.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> Atar kod taşınmış `Page` nesnenin `MasterPageFile` dışı özelliği `OnPreInit` olay işleyicisi ve iki ayrı yöntemlerde. Bu ilk yöntemi `SetMasterPageFile`, atar `MasterPageFile` ikinci yöntemi tarafından döndürülen değer özelliğine `GetMasterPageFileFromSession`. I işaretli `SetMasterPageFile` yöntemi `Overridable` böylece gelecekteki sınıflarını genişletmek `BasePage` isteğe bağlı olarak, özel mantığı uygulamanız gerektiğinde geçersiz kılabilirsiniz. Geçersiz kılma örneği göreceğiz `BasePage`'s `SetMasterPageFile` sonraki öğretici özelliği.


Bu kod yerinde ziyaret `ChooseMasterPage.aspx` sayfası. Başlangıçta, `Site.master` ana sayfasıdır seçili (bkz. Şekil 6), ancak kullanıcı farklı bir ana sayfa aşağı açılan listeden seçim yapabilirsiniz.


[![İçerik sayfaları Site.master ana sayfa kullanarak görüntülenir](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**Şekil 06**: içerik sayfalarıdır görüntülenen kullanarak `Site.master` ana sayfa ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-master-page-programmatically-vb/_static/image18.png))


[![İçerik sayfaları artık Alternate.master ana sayfa kullanarak görüntülenir](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**Şekil 07**: içerik sayfalarıdır şimdi görüntülenen kullanarak `Alternate.master` ana sayfa ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-master-page-programmatically-vb/_static/image21.png))


## <a name="summary"></a>Özet

Bir içerik sayfasını ziyaret edildiğinde içerik denetimlerini kendi ana sayfanın ContentPlaceHolder denetimleriyle Sigortalı. İçerik sayfasının ana sayfa tarafından belirtilir `Page` sınıfının `MasterPageFile` atandığı özelliği `@Page` yönergesi'nın `MasterPageFile` başlatma aşaması sırasında özniteliği. Koyduysa, biz için bir değer atayabilirsiniz Bu öğretici olarak `MasterPageFile` PreInit aşama bitişinden önce biz bunu sürece özelliği. Ana sayfaya programlı olarak belirtmek çalışabilme kapı dış faktörlerini temel alarak bir ana sayfa için bir içerik sayfasını dinamik olarak bağlama gibi daha Gelişmiş senaryolar için açılır.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET sayfası yaşam döngüsü diyagramı](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [ASP.NET sayfası yaşam döngüsüne genel bakış](https://msdn.microsoft.com/library/ms178472.aspx)
- [ASP.NET temalar ve dış genel bakış](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Ana sayfalar: İpuçlarını, püf noktaları ve tuzakları](http://www.odetocode.com/articles/450.aspx)
- [ASP.NET temalar](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar birden çok ASP/ASP.NET books ve 4GuysFromRolla.com kurucusu, 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Tan adresindeki ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Suchi Banerjee oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bir satırında bana bırak[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](master-pages-and-asp-net-ajax-vb.md)
[sonraki](nested-master-pages-vb.md)
