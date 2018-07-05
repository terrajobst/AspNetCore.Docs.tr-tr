---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: Birden çok ContentPlaceHolder ve varsayılan içerik (VB) | Microsoft Docs
author: rick-anderson
description: Ana sayfaya birden çok içerik yer tutucu ekleme ve bunun yanı sıra varsayılan içeriği içerik yer tutucu belirtmek nasıl inceler.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: 5425d59ac1ccec46617400601655ae72f860cab1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395120"
---
<a name="multiple-contentplaceholders-and-default-content-vb"></a>Birden çok ContentPlaceHolder ve varsayılan içerik (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> Ana sayfaya birden çok içerik yer tutucu ekleme ve bunun yanı sıra varsayılan içeriği içerik yer tutucu belirtmek nasıl inceler.


## <a name="introduction"></a>Giriş

Önceki öğreticide size nasıl etkinleştir ana sayfa incelenirken tutarlı site geneli bir düzen oluşturmak için ASP.NET geliştiricilerine. Ana sayfalar, tüm içerik sayfalarını yaygın biçimlendirme hem bir sayfa tarafından temelinde özelleştirilebilir bölgeleri tanımlayın. Önceki öğreticide oluşturduğumuz basit bir ana sayfa (`Site.master`) ve iki içerik sayfalarının (`Default.aspx` ve `About.aspx`). Ana sayfamızı adlı iki ContentPlaceHolder toplamda `head` ve `MainContent`, içinde bulunan `<head>` öğesi ve Web formu, sırasıyla. İçerik sayfaları her iki içerik denetimleri almışken için karşılık gelen bir biçimlendirme yalnızca belirlemiş `MainContent`.

İki ContentPlaceHolder denetimleri tarafından yi gibi `Site.master`, bir ana sayfa birden çok ContentPlaceHolder içerebilir. Bunun da ötesinde, ana sayfaya ContentPlaceHolder denetimler için varsayılan biçimlendirme belirtebilirsiniz. Bir içerik sayfasının sonra isteğe bağlı olarak kendi biçimlendirme belirtebilir veya varsayılan biçimlendirme kullanın. Bu öğreticide birden çok içerik denetimi kullanarak ana sayfasında bakın ve varsayılan biçimlendirme ContentPlaceHolder denetimleri tanımlamak bkz.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>1. adım: Ek ContentPlaceHolder denetimleri ana sayfasına ekleme

Birçok Web sitesi tasarımı ekranında bir sayfa tarafından temelinde özelleştirilmiş çeşitli alanları içerir. `Site.master`, önceki öğreticide oluşturduğumuz ana sayfayı içeren tek bir ContentPlaceHolder adlandırılan Web formu içinde `MainContent`. Özellikle, bu ContentPlaceHolder içinde bulunduğu `mainContent` `<div>` öğesi.

Şekil 1 gösterir `Default.aspx` bir tarayıcıdan görüntülendiğinde. Kırmızı daire içinde karşılık gelen sayfaya özgü biçimlendirme bölgedir `MainContent`.


[![Daire içinde Bölge alanı şu anda özelleştirilebilir sayfa tarafından temelinde gösterir](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**Şekil 01**: sayfa tarafından temelinde alanı şu anda özelleştirilebilir Circled bölgenizi görebilirsiniz ([tam boyutlu görüntüyü görmek için tıklatın](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))


Şekil 1'de gösterilen bölge yanı sıra, biz de sayfaya özgü öğeleri dersler ve haber altındaki sol sütuna eklemeniz gerektiğini Imagine bölümler. Bunu yapmak için biz ContentPlaceHolder denetimi başka bir ana sayfaya ekleyin. Örneği takip etmek için açık `Site.master` ana sayfa Visual Web Developer ve ardından ContentPlaceHolder denetimi araç kutusundan tasarımcıya sonra Haberler bölümü sürükleyin. ContentPlaceHolder'ın ayarlamak `ID` için `LeftColumnContent`.


[![Ana sayfanın sol sütuna ContentPlaceHolder denetim ekleme](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**Şekil 02**: ana sayfanın sol sütuna ContentPlaceHolder denetim ekleme ([tam boyutlu görüntüyü görmek için tıklatın](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))


Ek olarak `LeftColumnContent` ContentPlaceHolder ana sayfaya tanımlarız içerik bu bölge için bir sayfa tarafından temelinde içerik ekleyerek sayfasında ayarlanmış denetim `ContentPlaceHolderID` ayarlanır `LeftColumnContent`. Bu işlem 2. adım inceleyeceğiz.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>2. adım: Yeni ContentPlaceHolder içeriği içerik sayfalarında tanımlama

Yeni bir içerik sayfası Web sitesine eklerken, Visual Web Developer içerik otomatik olarak oluşturur. seçilen ana sayfasında her ContentPlaceHolder sayfasını denetimi. Eklenen bir `LeftColumnContent` ContentPlaceHolder bizim ana sayfaya, adım 1 ' yeni ASP.NET sayfaları artık üç içerik denetimleri vardır.

Bunu açıklamak üzere; adlı kök dizinine yeni bir içerik sayfası Ekle `MultipleContentPlaceHolders.aspx` bağlanan `Site.master` ana sayfa. Visual Web Developer bu sayfa aşağıdaki bildirim temelli biçimlendirme oluşturur:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

İçerik denetimi başvuru içine bazı içeriğini girin `MainContent` ContentPlaceHolder (`Content2`). Ardından, aşağıdaki biçimlendirme eklemek `Content3` içerik denetimi (başvuran `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

Bu işaretleme ekledikten sonra bir tarayıcı aracılığıyla sayfasını ziyaret edin. Şekil 3'te gösterildiği gibi biçimlendirme yerleştirilen `Content3` içerik denetimi sol sütunda (kırmızı daire içinde) Haberler bölümü altında görüntülenir. Biçimlendirme yerleştirilen `Content2` (mavi daire içinde) sayfanın sağ tarafında görüntülenir.


[![Sol sütunda Haberler bölümü altındaki sayfaya özel içeriği artık içerir.](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**Şekil 03**: sol sütunu artık içeren sayfaya özgü içerik altındaki Haberler bölümü ([tam boyutlu görüntüyü görmek için tıklatın](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Mevcut içerik sayfalarındaki tanımlayan içeriği

Yeni içerik sayfası otomatik olarak oluşturma, 1. adımda eklediğimiz ContentPlaceHolder denetimi içerir. Ancak iki mevcut içerik sayfalarımızın - `About.aspx` ve `Default.aspx` -için bir içerik denetimi sizde `LeftColumnContent` ContentPlaceHolder. Bu iki mevcut sayfalarında bu ContentPlaceHolder için içeriği belirtmek için size bir içerik denetimi kendimize eklemeniz gerekir.

Çoğu ASP.NET Web denetimleri farklı olarak, Visual Web Developer araç içerik denetimi öğesi içermiyor. Biz el ile kaynak görünüme içerik denetiminin bildirim temelli işaretlemede yazabilirsiniz, ancak daha kolay ve hızlı bir yaklaşım Tasarım görünümünde kullanmaktır. Açık `About.aspx` sayfasında ve Tasarım görünümüne geçin. Şekil 4'te gösterildiği gibi `LeftColumnContent` üzerine fare, gördüğü başlık okur; ContentPlaceHolder Tasarım görünümünde görüntülenir: "LeftColumnContent (ana)." "Ana" eklenmesi başlık, sayfada bu ContentPlaceHolder için tanımlı hiçbir içerik denetimi olduğunu gösterir. İçin olduğu gibi ContentPlaceHolder için bir içerik denetimi varsa `MainContent`, başlık okuyacaksa: "*ContentPlaceHolderID* (özel)."

İçin bir içerik denetimi eklemek için `LeftColumnContent` ContentPlaceHolder için `About.aspx`ContentPlaceHolder'ın akıllı etiket genişletin ve özel içerik oluşturma bağlantısına tıklayın.


[![LeftColumnContent ContentPlaceHolder About.aspx Tasarım görünümünü gösterir](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**Şekil 04**: Tasarım görünümü `About.aspx` gösterir `LeftColumnContent` ContentPlaceHolder ([tam boyutlu görüntüyü görmek için tıklatın](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))


Özel içerik oluşturma bağlantısını oluşturur gerekli içerik denetimi sayfası ve kümeleri kendi `ContentPlaceHolderID` ContentPlaceHolder'ın özelliğini `ID`. Örneğin, özel içerik oluşturma bağlantısına tıklayarak `LeftColumnContent` bölgede `About.aspx` sayfaya aşağıdaki bildirim temelli biçimlendirme ekler:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>İçerik denetimleri atlama

Tüm içerik sayfalarının ana sayfasında tanımlanan her ContentPlaceHolder için içerik denetimleri içeren ASP.NET gerektirmez. Bir içerik denetimi atlanırsa, ASP.NET altyapısı ContentPlaceHolder ana sayfaya'içinde tanımlanan biçimlendirme kullanır. Bu işaretleme ContentPlaceHolder ın adlandırılır *varsayılan içerik* ve içeriği için bazı bölge sayfaları, çoğu arasında ortak olan ancak gereken yere sayfaları için az sayıda özelleştirilmesi gereken senaryolarda yararlıdır. 3. adım belirten varsayılan içerik ana sayfasında keşfediyor.

Şu anda `Default.aspx` için iki içerik denetimlerini içeren `head` ve `MainContent` ContentPlaceHolder; için bir içerik denetimi yok `LeftColumnContent`. Sonuç olarak, `Default.aspx` işlenen `LeftColumnContent` ContentPlaceHolder'ın varsayılan içerik kullanılır. Herhangi bir varsayılan içerik için bu ContentPlaceHolder tanımlamak henüz, net etkisiyle biçimlendirme yok Bu bölge için yayıldığını olmasıdır. Bu davranış doğrulamak için ziyaret edin `Default.aspx` bir tarayıcı aracılığıyla. Şekil 5 gösterildiği gibi biçimlendirme yok sol sütunda Haberler bölümü altında yayınlanır.


[![İçerik için LeftColumnContent ContentPlaceHolder işlenir](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**Şekil 05**: içerik yok işlenen için `LeftColumnContent` ContentPlaceHolder ([tam boyutlu görüntüyü görmek için tıklatın](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>3. adım: Varsayılan içerik ana sayfada belirtme

Bazı Web sitesi tasarımları bir veya iki özel durum dışında sitedeki tüm sayfalar için aynı içeriğe sahip olan bir bölge içerir. Kullanıcı hesaplarını destekleyen bir Web sitesi göz önünde bulundurun. Böyle bir siteyi siteye ziyaretçileri oturum açmak için kimlik bilgilerini girebilecekleri bir oturum açma sayfası gerektirir. Oturum açma işlemi hızlandırmak için Web tasarımcılarına kullanıcı adı ve parola metin kutuları açıkça oturum açma sayfasını ziyaret etmesine gerek kalmadan oturum açmasına izin vermek için her sayfanın sol alt köşesindeki içerebilir. Bu kullanıcı adı ve parola metin kutuları çoğu sayfalarındaki faydalı olsa da, yedekli oturum açma sayfasında, zaten metin kutuları için kullanıcının kimlik bilgilerini içerir.

Bu tasarımı uygulamanız için ana sayfanın sol alt köşesinde ContentPlaceHolder denetimi oluşturabilirsiniz. Kullanıcı adı ve parola metin kutuları, sol üst köşedeki görüntülemek için gereken her sayfa içerik denetimi oluşturmak için bu ContentPlaceHolder ve ancak gerekli arabirimi ekleyin. Öte yandan, oturum açma sayfası bu ContentPlaceHolder için içerik denetimi ekleme ya da atlamak veya içerik oluşturacak denetimi ile tanımlanmış bir biçimlendirme yok. Bu yaklaşımın bir dezavantajı, biz (oturum açma sayfası dışında) bir siteye eklediğimiz her sayfa için kullanıcı adı ve parola metin kutuları eklemeyi unutmayın sahip olduğunu belirtir. Bu sorun için istiyor. Biz büyük olasılıkla bir sayfa ya da iki bu metin kutuları ekleyin veya daha da kötüsü, biz arabirimi doğru şekilde uygulamıyor olabilir (belki de iki yerine yalnızca bir metin kutusu ekleme).

Kullanıcı adı ve parola metin kutuları ContentPlaceHolder'ın varsayılan içerik olarak tanımlamak daha iyi bir çözümdür. Bunu yaptığınızda, yalnızca bu kullanıcı adı ve parola metin kutuları görüntülenmez birkaç bu sayfalarda varsayılan içerik geçersiz kılmak ihtiyacımız (oturum açma sayfası, örneği için). ContentPlaceHolder denetimi belirten varsayılan içeriğini göstermek için şimdi yalnızca açıklanan senaryo uygulayın.

> [!NOTE]
> Bu öğreticinin geri kalanında sol sütunda, ancak oturum açma sayfasına tüm sayfalar için bir oturum açma arabirimi eklemek için Web sitemizi güncelleştirir. Ancak, Bu öğretici, kullanıcı hesapları desteklemek için Web sitesi yapılandırma incelemez. Bu konu hakkında daha fazla bilgi için benim [form kimlik doğrulaması, yetkilendirme, kullanıcı hesaplarını ve rolleri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) öğreticiler.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Ekleme bir ContentPlaceHolder ve varsayılan içeriği belirtme

Açık `Site.master` ana sayfa ve sol sütunu arasında aşağıdaki işaretlemeyi ekleyin `DateDisplay` etiket ve dersleri bölümü:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

Bu işaretleme ekledikten sonra ana sayfa Tasarım görünümü Şekil 6'ya benzer görünmelidir.


[![Ana sayfada bir oturum açma denetimi içerir.](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**Şekil 06**: ana sayfada bir oturum açma denetimi içerir ([tam boyutlu görüntüyü görmek için tıklatın](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))


Bu ContentPlaceHolder `QuickLoginUI`, bir oturum açma Web denetimi varsayılan içeriği olarak vardır. Oturum açma denetimi için kullanıcı adı ve parola ile oturum aç düğmesine birlikte kullanıcıdan bir kullanıcı arabirimi görüntüler. Oturum Aç düğmesine tıklandıktan sonra oturum açma denetimi dahili olarak bir kullanıcının kimlik bilgilerini üyelik API'si karşı doğrular. Bu oturum açma denetimi uygulamada kullanmak için daha sonra sitenizi üyelik kullanacak şekilde yapılandırmanız gerekir. Bu öğreticinin kapsamı dışındadır konudur; başvurmak my [form kimlik doğrulaması, yetkilendirme, kullanıcı hesaplarını ve rolleri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) öğreticileri kullanıcı hesaplarını destekleyen bir web uygulaması oluşturma hakkında daha fazla bilgi için.

Oturum açma denetimin görünümünü veya davranışını özelleştirmek çekinmeyin. İki özelliklerini ayarladığınız: `TitleText` ve `FailureAction`. `TitleText` "Oturum açın" varsayılan olarak, özellik değeri, denetimin kullanıcı arabirimi üst kısmında görüntülenir. "Oturum açma" metin olarak görüntülenir, böylece bu özellik ayarladığınız bir `<h3>` öğesi. `FailureAction` Özelliği kullanıcının kimlik bilgileri geçersiz olduğunda yapılması gerekenler gösterir. Bu değeri varsayılan olarak `Refresh`, aynı sayfada kullanıcı ayrılsa ve oturum açma denetimi içinde bir hata iletisi görüntüler. Kendisine değiştirdiniz `RedirectToLoginPage`, gönderdiği kullanıcı oturum açma sayfası URL'sini geçersiz kimlik bilgileri durumunda. Oturum açma sayfasına ek yönergeleri ve kolayca sol sütuna yerleştirin değil seçenekleri içerebileceğinden, bir kullanıcı başka bir sayfaya ancak başarısız oturum açma girişiminde bulunduğunda, kullanıcı oturum açma sayfasına göndermek ister. Örneğin, oturum açma sayfasına Unutulan parolayı almak veya yeni bir hesap oluşturmak için seçenekleri içeriyor olabilir.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Oturum açma sayfası oluşturma ve varsayılan içerik geçersiz kılma

Ana sayfa ile tam sonraki adımımız oturum açma sayfasına oluşturmaktır. Bir ASP.NET sayfasını adlı sitenizin kök dizinine ekleme `Login.aspx`, kendisine bağlamayı `Site.master` ana sayfa. Bunun yapılması oluşturur bir sayfa ile dört içerik denetimlerini, her ContentPlaceHolder tanımlanan `Site.master`.

Bir oturum açma denetimine ekleme `MainContent` içerik denetimi. Benzer şekilde, herhangi bir içerik eklemekten çekinmeyin `LeftColumnContent` bölge. Ancak, içerik denetimi için bıraktığınızdan emin olun `QuickLoginUI` ContentPlaceHolder boş. Bu, Denetim, oturum açma sayfasının sol sütununda görünmüyor oturum açma garanti eder.

İçeriğini tanımlayan sonra `MainContent` ve `LeftColumnContent` bölgeler, oturum açma sayfanızın bildirim temelli biçimlendirme görünmelidir aşağıdakine benzer:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

Şekil 7, bir tarayıcıdan görüntülendiğinde bu sayfada görüntülenir. Bu sayfa için içerik denetimi belirttiğinden `QuickLoginUI` ContentPlaceHolder, ana sayfada belirtilen varsayılan içeriği geçersiz. Ana sayfa Tasarım görünümü (bkz. Şekil 6) Bu sayfada işlenmez görüntülenen oturum açma denetimi net etkisidir.


[![Oturum açma sayfasına QuickLoginUI ContentPlaceHolder'ın varsayılan içerik Represses](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**Şekil 07**: oturum açma sayfası Represses `QuickLoginUI` ContentPlaceHolder'ın varsayılan içerik ([tam boyutlu görüntüyü görmek için tıklatın](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Yeni sayfalarında varsayılan içerik kullanma

Oturum açma denetimi sol sütunda oturum açma sayfası dışındaki tüm sayfalar için gösterilecek istiyoruz. Bunu başarmak için oturum açma sayfasına hariç tüm içerik sayfalarının için bir içerik denetimi atlarsanız `QuickLoginUI` ContentPlaceHolder. Bir içerik denetimi gt;(yok) ContentPlaceHolder'ın varsayılan içerik yerine kullanılacaktır.

Mevcut içerik sayfalarımızın - `Default.aspx`, `About.aspx`, ve `MultipleContentPlaceHolders.aspx` -için bir içerik denetimi içermez `QuickLoginUI` ana sayfaya ContentPlaceHolder denetleyen ekledik önce oluşturuldukları olduğundan. Bu nedenle, bu varolan sayfalar güncelleştirilmesi gerekmez. Ancak, Web sitesine eklenen yeni sayfa için içerik denetimi dahil `QuickLoginUI` ContentPlaceHolder, varsayılan olarak. Bu nedenle, bunları kaldırmak basitçe hatırlanan sahibiz içerik denetimlerini her zaman eklediğimiz yeni bir içerik sayfası (biz ContentPlaceHolder'ın varsayılan içerik, geçersiz kılmak oturum açma sayfası olarak söz konusu olduğunda istemediğiniz sürece).

İçerik denetimi kaldırmak için el ile bildirim temelli biçimlendirme kaynağı görünümünden silin veya, varsayılan asıl içerik bağlantısı için Tasarım Görünümü'nden seçin akıllı etiketinde. Her iki yöntemle içerik denetimi kaldırır sayfası ve üretir aynı ağ etkisi.

Şekil 8 gösterir `Default.aspx` bir tarayıcıdan görüntülendiğinde. Bu geri çağırma `Default.aspx` yalnızca iki içerik denetimi, bildirim temelli biçimlendirme - biri için belirtilen sahip `head` , diğeri `MainContent`. Sonuç olarak, içerik için varsayılan `LeftColumnContent` ve `QuickLoginUI` ContentPlaceHolder görüntülenir.


[![Varsayılan içerik LeftColumnContent ve QuickLoginUI ContentPlaceHolder görüntülenir](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**Şekil 08**: varsayılan içerik için `LeftColumnContent` ve `QuickLoginUI` ContentPlaceHolder görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))


## <a name="summary"></a>Özet

ASP.NET ana sayfa modeli ContentPlaceHolder tercihe bağlı sayıda ana sayfasında olanak sağlar. Daha fazla nedir, karşılık gelen olduğu durumlarda yayılan varsayılan içerik ContentPlaceHolder dahil içerik sayfası denetiminde içerik. Bu öğreticide ana sayfasında ek ContentPlaceHolder denetimler ekleme ve bu yeni ContentPlaceHolder içinde yeni ve varolan ASP.NET sayfaları için içerik denetimleri tanımlama gördük. Ayrıca varsayılan belirtme burada sayfalarını gerekir aksi özelleştirme nadiren yalnızca içeriği belirli bir bölgede standart senaryolarda yararlı olan bir ContentPlaceHolder'içerik incelemiştik.

Sonraki öğreticide inceleyeceğiz `head` başlık, meta etiketler ve diğer HTML üst bilgilerini bildirimli ve programlı olarak sayfa sayfa olarak tanımlamak görmeden ContentPlaceHolder daha ayrıntılı olarak.

Mutlu programlama!

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar 1998'de bu yana birden çok ASP/ASP.NET books ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileri ile çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott, konumunda ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Suchi Banerjee oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](creating-a-site-wide-layout-using-master-pages-vb.md)
> [İleri](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
