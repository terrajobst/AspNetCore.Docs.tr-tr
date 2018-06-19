---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: Birden çok ContentPlaceHolders için ve varsayılan içerik (C#) | Microsoft Docs
author: rick-anderson
description: Ana sayfaya birden çok içerik yer tutucu ekleme ve bunun yanı sıra içinde içerik yer tutucu varsayılan içeriği belirtmek nasıl inceler.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: b60017c21b4cf45081893af08e68186009475fd2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889062"
---
<a name="multiple-contentplaceholders-and-default-content-c"></a>Birden çok ContentPlaceHolders için ve varsayılan içerik (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> Ana sayfaya birden çok içerik yer tutucu ekleme ve bunun yanı sıra içinde içerik yer tutucu varsayılan içeriği belirtmek nasıl inceler.


## <a name="introduction"></a>Giriş

Önceki öğreticide biz nasıl etkinleştir ana sayfa incelenmesi tutarlı bir site genelinde düzen oluşturmak için ASP.NET geliştiricilerinin. Ana sayfalar, tüm içerik sayfalarını ortak biçimlendirme ve sayfa tarafından temelinde özelleştirilebilir bölgeleri tanımlayın. Önceki öğreticide oluşturduğumuz basit bir ana sayfa (`Site.master`) ve iki içerik sayfalarının (`Default.aspx` ve `About.aspx`). Adlı iki ContentPlaceHolders için ana sayfamızı oluşmuştur `head` ve `MainContent`, içinde bulunan `<head>` öğesi ve Web formu, sırasıyla. İçerik sayfaları her iki içerik denetimleri alırken, biz yalnızca karşılık gelen bir işaretleme belirtilen `MainContent`.

İki ContentPlaceHolder denetimlerinde tarafından yi gibi `Site.master`, bir ana sayfa birden fazla ContentPlaceHolders için içerebilir. Daha ana sayfa ContentPlaceHolder denetimleri için varsayılan biçimlendirmesini belirtebilir. Bir içerik sayfasını sonra isteğe bağlı olarak, kendi biçimlendirme belirtebilir veya varsayılan biçimlendirme kullanın. Bu öğreticide ana sayfasında birden çok içerik denetimlerini kullanarak arayın ve varsayılan biçimlendirme ContentPlaceHolder denetimlerinde tanımlamak bkz.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>1. adım: Ana sayfaya ek ContentPlaceHolder denetimleri ekleme

Birçok Web sitesi tasarımı ekranında bir sayfa tarafından temelinde özelleştirilmiş çeşitli alanları içerir. `Site.master`, önceki öğreticide oluşturduğumuz ana sayfa içeren tek bir ContentPlaceHolder adlı Web formu içinde `MainContent`. Özellikle, bu ContentPlaceHolder içinde bulunduğu `mainContent` `<div>` öğesi.

Şekil 1 gösterir `Default.aspx` bir tarayıcıdan görüntülendiğinde. Kırmızı daire içinde karşılık gelen sayfaya özgü biçimlendirme bölgedir `MainContent`.


[![Daire içinde Bölge alanı şu anda özelleştirilebilir bir sayfa tarafından temelinde gösterir](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**Şekil 01**: Circled Bölge alanı şu anda özelleştirilebilir bir sayfa tarafından temelinde gösterir ([tam boyutlu görüntüyü görüntülemek için tıklatın](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))


Şekil 1'de gösterilen bölge ek olarak, biz de dersleri ve haber altındaki sol sütunda sayfaya özgü öğeleri eklemek gerektiğini düşünün bölümler. Bunu başarmak için başka bir ContentPlaceHolder denetimi ana sayfaya ekleriz. Takip açmak `Site.master` ana sayfa Visual Web Developer ve ContentPlaceHolder denetimi tasarımcıya araç sonra haber bölümüne sürükleyin. ContentPlaceHolder's ayarlamak `ID` için `LeftColumnContent`.


[![Ana sayfanın sol sütunda ContentPlaceHolder denetim ekleme](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**Şekil 02**: ana sayfanın sol sütun ContentPlaceHolder denetim ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))


Eklenmesi ile `LeftColumnContent` ContentPlaceHolder ana sayfaya biz tanımlayabilirsiniz içerik bu bölge için bir sayfa tarafından temelinde içerik ekleyerek sayfasında özelliği kontrol `ContentPlaceHolderID` ayarlanır `LeftColumnContent`. Adım 2. Bu işlem inceleyeceğiz.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>2. adım: İçerik yeni ContentPlaceHolder için içerik sayfalarına tanımlama

Yeni bir içerik sayfa Web sitesine eklerken, Visual Web Developer içerik otomatik olarak oluşturur. her ContentPlaceHolder seçili ana sayfa içinde sayfa denetiminde. Eklenen bir `LeftColumnContent` ana sayfamızı 1. adımda yeni ASP.NET sayfaları şimdi ContentPlaceHolder üç içerik denetimleri vardır.

Bunu göstermek için yeni bir içerik sayfasını adlı kök dizinine ekleme `MultipleContentPlaceHolders.aspx` için bağlı `Site.master` ana sayfa. Visual Web Developer bu sayfa ile aşağıdaki bildirim temelli biçimlendirme oluşturur:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

İçerik denetimi başvuran içine bazı içerikler girin `MainContent` ContentPlaceHolders için (`Content2`). Ardından, aşağıdaki biçimlendirme eklemek `Content3` içerik denetimi (hangi başvuruları `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

Bu biçimlendirme eklendikten sonra bir tarayıcı aracılığıyla sayfasını ziyaret edin. Şekil 3'te gösterildiği gibi biçimlendirme yerleştirilen `Content3` içerik denetimi (kırmızı daire içinde) haber bölümü altındaki sol sütunda görüntülenir. İşaretleme yerleştirilen `Content2` (mavi renkte yuvarlak içine alınmıştır) sayfasının sağ kısmında görüntülenir.


[![Sol sütunda şimdi sayfaya özgü haber bölümü altındaki içeriği](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**Şekil 03**: sol sütun şimdi içeren sayfaya özgü içerik altındaki haber bölümü ([tam boyutlu görüntüyü görüntülemek için tıklatın](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Var olan içerik sayfalarında içerik tanımlama

Yeni bir içerik sayfasını otomatik olarak oluşturma, 1. adımda eklediğimiz ContentPlaceHolder denetimi içerir. Ancak, iki mevcut içerik sayfaları - `About.aspx` ve `Default.aspx` -içerik denetimi için sizde `LeftColumnContent` ContentPlaceHolder. Bu iki varolan sayfalarındaki bu ContentPlaceHolder için içeriği belirtmek için size bir içerik denetimi kendisini eklemeniz gerekir.

Çoğu ASP.NET Web denetimleri, bir içerik denetimi öğesi Visual Web Developer araç içermez. Biz el ile kaynak görünüme içerik denetimin bildirim temelli biçimlendirmede yazabilirsiniz, ancak daha kolay ve hızlı bir yaklaşım Tasarım görünümüne kullanmaktır. Açık `About.aspx` sayfasında ve Tasarım görünümüne geçin. Şekil 4'te gösterildiği gibi `LeftColumnContent` ContentPlaceHolder Tasarım görünümünde görüntülenir; üzerine fare, gördüğü başlık okur: "LeftColumnContent (ana)." "Ana" ekleme başlığında bu ContentPlaceHolder için sayfada tanımlı hiçbir içerik denetimi gösterir. Söz konusu olduğu gibi ContentPlaceHolder için içerik denetimi varsa `MainContent`, başlığını okuyun: "*ContentPlaceHolderID* (özel)."

İçerik denetimi için eklemek için `LeftColumnContent` için ContentPlaceHolder `About.aspx`, ContentPlaceHolder'ın akıllı etiket genişletin ve oluşturmak özel içerik bağlantısına tıklayın.


[![LeftColumnContent ContentPlaceHolder About.aspx Tasarım görünümünü gösterir](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**Şekil 04**: Tasarım görünümünü `About.aspx` gösterir `LeftColumnContent` ContentPlaceHolder ([tam boyutlu görüntüyü görüntülemek için tıklatın](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))


Özel içerik oluşturma bağlantı tıklatıldığında oluşturur gerekli içerik denetim sayfası ve kümeleri kendi `ContentPlaceHolderID` ContentPlaceHolder'ın özelliğine `ID`. Örneğin, özel içeriği Oluştur bağlantısını tıklatarak `LeftColumnContent` bölgede `About.aspx` sayfaya aşağıdaki bildirim temelli biçimlendirme ekler:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>İçerik denetimleri atlama

ASP.NET, tüm içerik sayfalarının ana sayfa içinde tanımlanan her ContentPlaceHolder için içerik denetimleri içerir gerektirmez. İçerik denetimi atlanırsa, ASP.NET altyapısı ana sayfasında ContentPlaceHolder içinde tanımlanan biçimlendirme kullanır. Bu biçimlendirme ContentPlaceHolder 's adlandırılır *varsayılan içerik* ve burada içeriği için bazı bölge sayfaları, çoğu arasında ortak ancak gerekiyor küçük bir sayfa sayısı için özelleştirilmiş senaryolarda kullanışlıdır. 3. adım ana sayfanın belirtilmesini varsayılan içeriğinde araştırır.

Şu anda `Default.aspx` için iki içerik denetimi içeren `head` ve `MainContent` ContentPlaceHolders için; içerik denetimi için yok `LeftColumnContent`. Sonuç olarak, ne zaman `Default.aspx` işlenen `LeftColumnContent` ContentPlaceHolder'ın varsayılan içerik kullanılır. Herhangi bir varsayılan içerik için bu ContentPlaceHolder tanımlamak henüz çünkü net biçimlendirme yok Bu bölgede yayınlanır etkisidir. Bu davranış doğrulamak için ziyaret `Default.aspx` bir tarayıcı aracılığıyla. Şekil 5 gösterildiği gibi biçimlendirme yok haber bölümü altındaki sol sütunda yayınlanır.


[![İçerik için LeftColumnContent ContentPlaceHolder işlenir](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**Şekil 05**: Hayır içerik işlenen için `LeftColumnContent` ContentPlaceHolder ([tam boyutlu görüntüyü görüntülemek için tıklatın](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>3. adım: Varsayılan içerik ana sayfasında belirtme

Bazı Web sitesi tasarımları içerikleri bir veya iki özel durumlar dışında sitedeki tüm sayfalar için aynı olan bir bölge içerir. Kullanıcı hesaplarını destekleyen bir Web sitesi göz önünde bulundurun. Bu tür bir site ziyaretçilerini siteye imzalamak için kendi kimlik bilgilerini girebilecekleri bir oturum açma sayfası gerektirir. Oturum açma işlemine hızlandırmak için Web tasarımcılarına kullanıcı adı ve parola metin kutuları açıkça oturum açma sayfasını ziyaret edin gerek kalmadan oturum açmasına olanak vermek için her sayfanın sol alt köşesindeki içerebilir. Bu kullanıcı adı ve parola metin kutuları çoğu sayfalarında yararlı olsa da, bunlar zaten metin kutuları için kullanıcının kimlik bilgilerini içeren oturum açma sayfasındaki gereksizdir.

Bu tasarım uygulamak için ana sayfanın sol üst köşesinde ContentPlaceHolder denetimi oluşturabilirsiniz. Kendi sol üst köşedeki kullanıcı adı ve parola metin kutuları görüntülemek için gereken her bir sayfa için bu ContentPlaceHolder içerik denetimi oluşturun ve ancak gerekli arabirimi ekleyin. Diğer taraftan, oturum açma sayfasına ya da bu ContentPlaceHolder için içerik denetimi ekleme atlayın veya içerik oluşturacak denetimi ile tanımlanan biçimlendirme yok. Bu yaklaşımın bir dezavantajı, biz (dışında oturum açma sayfası) sitesine eklediğimiz her sayfada kullanıcı adı ve parola metin kutuları eklemek anımsamak zorunda ' dir. Bu sorun için istiyor. Biz bu metin kutuları bir sayfaya veya iki eklemek unuttunuz olasılığınız ya da kötüsü, biz arabirimi doğru uygulayabilir değil (belki de iki yerine yalnızca bir metin eklemek).

Kullanıcı adı ve parola metin kutuları ContentPlaceHolder'ın varsayılan içerik tanımlamak daha iyi bir çözümdür. Bunu yaparak, biz yalnızca bu kullanıcı adı ve parola metin kutuları gösterme bu birkaç sayfaları varsayılan içerikte geçersiz kılmanız gerekir (oturum açma sayfası, örneğin). ContentPlaceHolder denetimi için belirten varsayılan içerik göstermek için şimdi yalnızca tartışılan senaryo uygulayın.

> [!NOTE]
> Bu öğreticinin geri kalanında sol sütunda tüm sayfaları ancak oturum açma sayfası için bir oturum açma arabirimi eklemek için Web sitesi güncelleştirir. Ancak, Bu öğretici kullanıcı hesaplarını desteklemek için Web sitesi yapılandırma inceleyin değil. Bu konu hakkında daha fazla bilgi için bkz my [form kimlik doğrulaması, yetkilendirme, kullanıcı hesapları ve rolleri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) öğreticileri.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Bir ContentPlaceHolder ekleme ve varsayılan içeriği belirtme

Açık `Site.master` ana sayfa ve sol sütunda arasında aşağıdaki biçimlendirme eklemek `DateDisplay` etiket ve dersleri bölümünde:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

Bu biçimlendirme ekledikten sonra ana sayfanızın Tasarım görünümü Şekil 6 benzer görünmelidir.


[![Ana sayfa bir oturum açma denetimi içerir](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**Şekil 06**: ana sayfa bir oturum açma denetimi içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))


Bu ContentPlaceHolder `QuickLoginUI`, bir oturum açma Web denetimi varsayılan içeriği olarak sahiptir. Oturum açma denetimi kullanıcıdan kullanıcı adı ve parola oturum aç düğmesine yanı sıra için bir kullanıcı arabirimi görüntüler. Oturum Aç düğmesine tıklatıldığında, oturum açma denetimi dahili olarak kullanıcının kimlik bilgilerini üyelik API'sine karşı doğrular. Bu oturum açma denetimi uygulamada kullanmak için daha sonra üyelik kullanacak şekilde yapılandırmanız gerekir. Bu konu, Bu öğretici kapsamında değildir; başvurmak my [form kimlik doğrulaması, yetkilendirme, kullanıcı hesapları ve rolleri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) öğreticileri kullanıcı hesaplarını destekleyen bir web uygulaması oluşturma hakkında daha fazla bilgi için.

Oturum açma denetimin davranışını veya görünümünü özelleştirmek çekinmeyin. İki özelliklerini ayarladığınız: `TitleText` ve `FailureAction`. `TitleText` "Oturum açma için" varsayılan olarak, özellik değeri, denetimin kullanıcı arabirimi üst kısmında görüntülenir. Böylece "Oturum Aç" metin olarak görüntüler bu özellik ayarladığınız bir `<h3>` öğesi. `FailureAction` Özelliği kullanıcının kimlik bilgileri geçersiz olduğunda yapılması gerekenler gösterir. Değeri varsayılan olarak `Refresh`, kullanıcı aynı sayfa üzerinde bırakır ve oturum açma denetimi içinde bir hata iletisi görüntüler. Kendisine değiştirdiyseniz `RedirectToLoginPage`, gönderdiği kullanıcı oturum açma sayfası geçersiz kimlik bilgileri durumunda. Oturum açma sayfasına ek yönergeler ve kolayca sol sütuna uyar olmayan seçenekler içerebileceğinden bir kullanıcı başka bir sayfaya ancak başarısız oturum açma girişiminde bulunduğunda, kullanıcı oturum açma sayfasına göndermek istemiyorum. Örneğin, oturum açma sayfasına Unutulan parolayı almaya veya yeni bir hesap oluşturmak için seçenekleri içeriyor olabilir.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Oturum açma sayfasına oluşturma ve varsayılan içerik geçersiz kılma

Ana sayfa ile tam bizim sonraki adıma oturum açma sayfasına oluşturmaktır. Bir ASP.NET sayfası, sitenizin kök dizinine adlı Ekle `Login.aspx`, kendisine bağlama `Site.master` ana sayfa. Bunun yapılması oluşturacak bir sayfa ile dört içerik denetimleri, her ContentPlaceHolders için tanımlanan `Site.master`.

Bir oturum açma denetimine ekleme `MainContent` içerik denetimi. Benzer şekilde, herhangi bir içerik eklemekten çekinmeyin `LeftColumnContent` bölge. Ancak, içerik denetimi için bıraktığınızdan emin olun `QuickLoginUI` ContentPlaceHolder boş. Bu, oturum açma denetimi oturum açma sayfasının sol sütununda görünmüyor garanti eder.

İçerik için tanımlama sonra `MainContent` ve `LeftColumnContent` bölgeler, oturum açma sayfanızın bildirim temelli biçimlendirme görünmelidir aşağıdakine benzer:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

Şekil 7 bir tarayıcıdan görüntülendiğinde bu sayfada görüntülenir. Bu sayfa için içerik denetimi belirttiğinden `QuickLoginUI` ContentPlaceHolder, bu geçersiz kılmaları ana sayfasında belirtilen varsayılan içeriği. Ana sayfanın Tasarım görünümü (bkz. Şekil 6) Bu sayfada işlenmez görüntülenen oturum açma denetimi net etkisidir.


[![Oturum açma sayfasına QuickLoginUI ContentPlaceHolder'ın varsayılan içerik Represses](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**Şekil 07**: oturum açma sayfası Represses `QuickLoginUI` ContentPlaceHolder'ın varsayılan içerik ([tam boyutlu görüntüyü görüntülemek için tıklatın](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Yeni sayfaların varsayılan içeriği kullanma

Sol sütunda oturum açma sayfası dışındaki tüm sayfalar için oturum açma denetimi göstermek istiyoruz. Bunun için oturum açma sayfasına hariç tüm içerik sayfalarının içerik denetimi için atlayın `QuickLoginUI` ContentPlaceHolder. İçerik denetimi kaldırarak ContentPlaceHolder'ın varsayılan içerik yerine kullanılır.

Varolan de içerik sayfalarında - `Default.aspx`, `About.aspx`, ve `MultipleContentPlaceHolders.aspx` -içerik denetimi için içermez `QuickLoginUI` ana sayfaya ContentPlaceHolder denetleyen eklediğimiz önce oluşturuldukları olduğundan. Bu nedenle, bu varolan sayfalar güncelleştirilmesi gerekmez. Ancak, içerik denetimi için Web sitesine eklenen yeni sayfalar dahil `QuickLoginUI` varsayılan ContentPlaceHolder. Bu nedenle, bu kaldırılacak hatırlamasını sahibiz içerik eklediğimiz yeni bir içerik sayfası (ContentPlaceHolder'ın varsayılan içerik, geçersiz kılmak oturum açma sayfası olarak söz konusu olduğunda istiyoruz sürece) her zaman denetler.

İçerik denetimi kaldırmak için el ile bildirim temelli biçimlendirme kaynağı görünümünden silebilir veya, Tasarım görünümünden Yöneticisi'nin içerik bağlantısı varsayılan seçin, akıllı etiket gelen. İçerik denetimi her iki yaklaşım kaldırır sayfayı ve üretir aynı etkili net.

Şekil 8 gösterir `Default.aspx` bir tarayıcıdan görüntülendiğinde. Sözcüğünün `Default.aspx` yalnızca kendi bildirim temelli biçimlendirme - biri için belirtilen iki içerik denetimlerine sahip `head` için bir tane `MainContent`. Sonuç olarak, içerik için varsayılan `LeftColumnContent` ve `QuickLoginUI` ContentPlaceHolders için görüntülenir.


[![Varsayılan içerik LeftColumnContent ve QuickLoginUI ContentPlaceHolders için görüntülenir](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**Şekil 08**: varsayılan içerik için `LeftColumnContent` ve `QuickLoginUI` ContentPlaceHolders için görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))


## <a name="summary"></a>Özet

ASP.NET ana sayfa modeli ContentPlaceHolders için ana sayfasında bir rastgele sayı olanak sağlar. Dahası ContentPlaceHolders için karşılık gelen olması durumunda yayılan varsayılan içerik dahil içerik, içerik sayfasındaki denetimi. Bu öğreticide ana sayfasında ek ContentPlaceHolder denetimleri içerir ve bu yeni ContentPlaceHolders için hem yeni hem de mevcut ASP.NET sayfaları için içerik denetimleri tanımlamak gördük. Biz de varsayılan belirtme burada aksi özelleştirmek için sayfaları gereksinimlerini nadiren yalnızca belirli bir bölgedeki içerik standartlaştırılmış senaryolarda yararlı olduğu ContentPlaceHolder'içerik arama.

Sonraki öğreticide biz inceleyeceğiz `head` daha ayrıntılı ContentPlaceHolder bildirimli olarak ve program aracılığıyla başlık, meta etiketler ve diğer HTML üstbilgileri bir sayfa tarafından temelinde nasıl tanımlanacağı görme.

Mutluluk programlama!

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar birden çok ASP/ASP.NET books ve 4GuysFromRolla.com kurucusu, 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Tan adresindeki ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Suchi Banerjee oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](creating-a-site-wide-layout-using-master-pages-cs.md)
> [sonraki](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
