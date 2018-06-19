---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: Bir kullanıcı hesabı birçok (C# ' dan) seçmek için bir arabirim oluşturma | Microsoft Docs
author: rick-anderson
description: Bu öğreticide biz disk belleği, filtrelenebilir kılavuz sahip bir kullanıcı arabirimi oluşturacaksınız. Özellikle, kullanıcı arabirimimizi LinkButtons için bir dizi oluşur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 304505b18e330425ea1dc8df87a552f3d8cd15f3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890677"
---
<a name="building-an-interface-to-select-one-user-account-from-many-c"></a>Bir kullanıcı hesabı birçok (C# ' dan) seçmek için bir arabirim oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip) veya [PDF indirin](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> Bu öğreticide biz disk belleği, filtrelenebilir kılavuz sahip bir kullanıcı arabirimi oluşturacaksınız. Özellikle, bizim kullanıcı arabirimi, başlangıç harfini kullanıcı adı ve eşleşen kullanıcıları göstermek için bir GridView denetimi üzerinde temel sonuçları filtrelemek için LinkButtons bir dizi oluşur. Tüm kullanıcı hesaplarını GridView içinde listeleyerek başlayacağız. Ardından, adım 3'te LinkButtons filtre ekleyeceğiz. 4. adım, filtrelenmiş sonuçlar disk belleği hızında arar. 4'ten adım 2'de oluşturulan arabirimi sonraki öğreticilerde, belirli bir kullanıcı hesabı için yönetim görevlerini gerçekleştirmek için kullanılır.


## <a name="introduction"></a>Giriş

İçinde <a id="_msoanchor_1"> </a> [ *kullanıcıları rollere atama* ](../roles/assigning-roles-to-users-cs.md) öğretici, oluşturduğumuz bir kullanıcı seçin ve kendi rolleri yönetmek bir yöneticinin ilkel arabirimi. Özellikle, arabirim yönetici aşağı açılan listesi tüm kullanıcıları ile sunulan. Böyle bir arabirim vardır, ancak bir düzine kadar kullanıcı hesapları, ancak yönetilmeleri yüzlerce veya binlerce hesapları ile siteler için uygundur. Disk belleği, filtrelenebilir kılavuz büyük kullanıcı temellerine ile Web siteleri için daha uygun kullanıcı arabirimi ' dir.

Bu öğreticide Biz bu tür bir kullanıcı arabirimi oluşturacaksınız. Özellikle, bizim kullanıcı arabirimi, başlangıç harfini kullanıcı adı ve eşleşen kullanıcıları göstermek için bir GridView denetimi üzerinde temel sonuçları filtrelemek için LinkButtons bir dizi oluşur. Tüm kullanıcı hesaplarını GridView içinde listeleyerek başlayacağız. Ardından, adım 3'te LinkButtons filtre ekleyeceğiz. 4. adım, filtrelenmiş sonuçlar disk belleği hızında arar. 4'ten adım 2'de oluşturulan arabirimi sonraki öğreticilerde, belirli bir kullanıcı hesabı için yönetim görevlerini gerçekleştirmek için kullanılır.

Haydi başlayalım!

## <a name="step-1-adding-new-aspnet-pages"></a>1. adım: Yeni ASP.NET sayfaları ekleme

Bu öğretici ve sonraki iki biz çeşitli yönetim ilgili işlevleri ve özellikleri inceleniyor. ASP.NET sayfaları bu öğreticileri incelenmesi konuları uygulamak için bir dizi ihtiyacımız. Şimdi bu sayfaları oluşturun ve site haritası güncelleştirin.

Başlangıç adlı projeye yeni bir klasör oluşturarak `Administration`. Ardından, her sayfasıyla bağlama klasöre iki yeni ASP.NET sayfaları ekleme `Site.master` ana sayfa. Sayfa adı:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Ayrıca Web sitesinin kök dizinine iki sayfa ekleyin: `ChangePassword.aspx` ve `RecoverPassword.aspx`.

Bu dört sayfaları bu noktada, her ana sayfanın ContentPlaceHolders için iki içerik denetimlerine sahip gerekir: `MainContent` ve `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

Ana sayfanın varsayılan biçimlendirme için göstermek istiyoruz `LoginContent` ContentPlaceHolder bu sayfaları için. Bu nedenle, kaldırmak için bildirim temelli biçimlendirme `Content2` içerik denetimi. Bunu yaptıktan sonra sayfaları biçimlendirme yalnızca bir içerik denetimi içermelidir.

ASP.NET sayfaları `Administration` klasörü, yalnızca yönetici kullanıcılar için yöneliktir. Sistemde bir Administrators rolünün eklediğimiz <a id="_msoanchor_2"> </a> [ *oluşturma ve yönetme rolleri* ](../roles/creating-and-managing-roles-cs.md) öğretici; bu rol için iki bu sayfalara erişimi kısıtlama. Bunu başarmak için add bir `Web.config` dosya `Administration` klasörüne ve yapılandırma kendi `<authorization>` öğesi haklıymış Yöneticiler rolündeki kullanıcılar için ve diğerlerini engellemek istiyorsanız.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

Bu noktada, projenizin Solution Explorer Şekil 1'de gösterilen ekran benzer görünmelidir.


[![Dört yeni sayfalar ve bir Web.config dosyası Web sitesine eklendi](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**Şekil 1**: dört yeni sayfalar ve `Web.config` dosyası, Web sitesine eklendi ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))


Son olarak, site haritası güncelleştirme (`Web.sitemap`) bir giriş eklemek için `ManageUsers.aspx` sayfası. Aşağıdaki XML sonra eklemek `<siteMapNode>` rolleri öğreticileri için eklediğimiz.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

Site haritasını güncelleştirilmiş bir tarayıcı aracılığıyla sitesini ziyaret edin. Şekil 2'de görüldüğü gibi sol taraftaki gezinti artık Yönetim öğreticileri için öğeleri içerir.


[![Kullanıcı Yönetimi başlıklı bir düğüm Site eşlemesini içerir](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**Şekil 2**: bir düğümü başlıklı kullanıcı yönetimi Site Haritası içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>2. adım: GridView içinde tüm kullanıcı hesaplarını listeleme

Bu öğretici için son Amacımız yönetici yönetmek için bir kullanıcı hesabı seçim yapabileceğiniz bir disk belleği, filtrelenebilir kılavuz oluşturmaktır. Liste tarafından başlayalım *tüm* GridView kullanıcılar. Bu işlem tamamlandıktan sonra filtreleme ve disk belleği arabirimleri ve işlevsellik ekleyeceğiz.

Açık `ManageUsers.aspx` sayfasındaki `Administration` klasör ve bir GridView eklemek ayarını kendi `ID` için `UserAccounts`. Biraz bekledikten sonra size kullanıcı hesapları kümesi GridView kullanmaya bağlamak için kod yazacaksınız `Membership` sınıfının `GetAllUsers` yöntemi. Önceki eğitimlerine anlatıldığı gibi GetAllUsers yöntemi döndürür bir `MembershipUserCollection` bir nesne, `MembershipUser` nesneleri. Her `MembershipUser` koleksiyonda gibi özellikler içeren `UserName`, `Email`, `IsApproved`ve benzeri.

GridView istenen kullanıcı hesabı bilgilerini görüntülemek için GridView's ayarlamanız `AutoGenerateColumns` özelliğini false olarak ayarlayın ve eklemek için BoundFields `UserName`, `Email`, ve `Comment` özellikleri ve CheckBoxFields için `IsApproved`, `IsLockedOut`, ve `IsOnline` özellikleri. Bu yapılandırma, denetimin bildirim temelli biçimlendirme veya alanları iletişim kutusu aracılığıyla uygulanabilir. Şekil 3 alanları ekran görüntüsü otomatik oluşturma alanları onay işareti kaldırıldı ve BoundFields ve CheckBoxFields eklenen yapılandırılmış ve sonra iletişim kutusunu gösterir.


[![Üç BoundFields ve üç CheckBoxFields GridView ekleme](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**Şekil 3**: üç BoundFields ekleyin ve GridView için üç CheckBoxFields ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))


GridView yapılandırdıktan sonra bildirim temelli biçimlendirme aşağıdakine benzer emin olun:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

Ardından, biz kullanıcı hesapları için GridView bağlar kod yazmanız gerekir. Adlı bir yöntem oluşturma `BindUserAccounts` bu görevi gerçekleştirmek ve ondan çağırmak için `Page_Load` olay işleyicisini ilk sayfasını ziyaret edin.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

Bir tarayıcı aracılığıyla bir sayfayı test etmek için bir dakikanızı ayırın. Şekil 4'te gösterildiği gibi `UserAccounts` GridView, sistemde kullanıcı adı, e-posta adresini ve diğer tüm kullanıcılar için uygun hesap bilgilerini listeler.


[![Kullanıcı hesaplarını GridView listelenir](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**Şekil 4**: kullanıcı hesapları GridView listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>3. adım: kullanıcı adı ilk harfini sonuçlarını filtreleme

Şu anda `UserAccounts` GridView gösterir *tüm* kullanıcı hesaplarını. Yüzlerce veya binlerce kullanıcı hesapları ile Web siteleri için bu kullanıcının hızla görüntülenen hesapları Karşılaştır mümkün zorunludur. Bu sayfaya LinkButtons filtreleme ekleyerek gerçekleştirilebilir. 27 LinkButtons sayfasına ekleyelim: biri başlıklı tüm her alfabedeki için bir LinkButton yanı sıra. Bir ziyaretçi tüm LinkButton tıklarsa, GridView tüm kullanıcıları göster. Belirli bir harf tıklarsanız, yalnızca kullanıcı adı seçilen harfle başlayan kullanıcılar görüntülenir.

Bizim ilk 27 LinkButton denetim eklemek için bir görevdir. 27 LinkButtons bildirimli olarak, oluşturmak için bir seçenek olacaktır birer birer. Yineleyici denetimi ile kullanmak için daha esnek bir yaklaşım olan bir `ItemTemplate` bir LinkButton işler ve filtreleme seçenekleri, yineleyici bağlar bir `string` dizi.

Yukarıdaki sayfasına yineleyici denetim ekleyerek başlangıç `UserAccounts` GridView. Yineleyici 's ayarlamak `ID` özelliğine `FilteringUI`. Yineleyici'nın şablonlarını yapılandırma böylece kendi `ItemTemplate` bir LinkButton işler, `Text` ve `CommandName` özellikleri geçerli dizi öğesine bağlı. İçinde gördüğümüz gibi <a id="_msoanchor_3"> </a> [ *kullanıcıları rollere atama* ](../roles/assigning-roles-to-users-cs.md) öğretici, bu gerçekleştirilebilir kullanarak `Container.DataItem` databinding sözdizimi. Yineleyici 's kullanmak `SeparatorTemplate` her bağlantı arasında dikey bir çizgi görüntülemek için.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

Bu yineleyici istenen filtreleme seçenekleri ile doldurmak için adlı bir yöntem oluşturun `BindFilteringUI`. Bu yöntemi çağırmak mutlaka `Page_Load` ilk sayfa yükü olay işleyicisi.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

Bu yöntem, öğeleri olarak filtreleme seçenekleri belirtir. `string` dizi `filterOptions`. Dizideki her öğe için bir LinkButton ile yineleyici kılacak kendi `Text` ve `CommandName` özellikleri dizi öğesinin değer atanmış.

Şekil 5 gösterir `ManageUsers.aspx` sayfasında bir tarayıcıdan görüntülendiğinde.


[![Yineleyici 27 filtreleme LinkButtons listeler](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**Şekil 5**: yineleyici listeler 27 filtreleme LinkButtons ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))


> [!NOTE]
> Kullanıcı adları, sayıları ve noktalama işaretlerini dahil olmak üzere herhangi bir karakter ile başlayabilir. Bu hesapları görüntülemek için tüm LinkButton bu seçeneği kullanmak yöneticinin gerekir. Alternatif olarak, bir rakamla başlayamaz tüm kullanıcı hesapları döndürülecek LinkButton ekleyebilirsiniz. I bunu bir alıştırma olarak okuyucuya bırakın.


Herhangi bir filtre LinkButtons tıklayarak geri gönderimin neden olur ve yineleyici 's başlatır `ItemCommand` olay, ancak kılavuzda herhangi bir değişiklik için biz henüz olduğunuz çünkü sonuçlara filtre uygulamak için herhangi bir kod yazma. `Membership` Sınıfı içeren bir [ `FindUsersByName` yöntemi](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) kullanıcı adı ile eşleşen bir belirtilen arama deseni bu kullanıcı hesaplarını döndürür. Biz yalnızca bu kullanıcı hesapları, kullanıcı adları Başlat tarafından belirtilen harfiyle almak için bu yöntemi kullanabilirsiniz `CommandName` tıklandığını filtrelenmiş LinkButton biri.

Başlangıç güncelleştirerek `ManageUser.aspx` sayfanın arka plandaki kod sınıfı adlı bir özellik içeren `UsernameToMatch`. Bu özellik kullanıcıadı filtre dizesi Geri göndermeler devam ederse:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

`UsernameToMatch` Özellik değerini atanan içine depolar `ViewState` UsernameToMatch anahtarı kullanarak koleksiyonu. Bu özelliğin değeri okurken, bir değer olup olmadığını denetler `ViewState` koleksiyonu; değilse, onu varsayılan değeri, boş bir dize döndürür. `UsernameToMatch` Özelliği öğesine özelliği yapılan değişikliklerin Geri göndermeler arasında kalıcı şekilde durumunu görüntülemek için bir değer kalıcı genel bir desen sergiler. Bu desen hakkında daha fazla bilgi için okuma [ASP.NET görünüm durumunu anlama](https://msdn.microsoft.com/library/ms972976.aspx).

Ardından, güncelleştirme `BindUserAccounts` yöntemi çağırmak yerine bu nedenle, `Membership.GetAllUsers`, çağırır `Membership.FindUsersByName`, değeri geçen `UsernameToMatch` SQL joker karakterle eklenen özellik %.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

Kullanıcı adı bir harfle başlayan yalnızca bu kullanıcılara görüntülenecek ayarlamak `UsernameToMatch` özelliğini A ve ardından arama `BindUserAccounts`. Bu çağrı neden olacağından `Membership.FindUsersByName("A%")`, tüm kullanıcıların kullanıcı adı geri dönme başlatır A. döndürmek için benzer şekilde ile *tüm* kullanıcılar, boş bir dize olarak atamak `UsernameToMatch` özelliği böylece `BindUserAccounts` yöntemi olacaktır Çağırma `Membership.FindUsersByName("%")`, böylece tüm kullanıcı hesapları döndürüyor.

Yineleyici için kullanıcının bir olay işleyicisi oluşturun `ItemCommand` olay. Her bir filtrenin LinkButtons tıklatıldığında bu olay tetiklenir; Tıklatılan LinkButton ait geçirilen `CommandName` aracılığıyla değer `RepeaterCommandEventArgs` nesnesi. Uygun değere atamak ihtiyacımız `UsernameToMatch` özelliği ve ardından arama `BindUserAccounts` yöntemi. Varsa `CommandName` tüm, boş bir dize olarak atamak `UsernameToMatch` böylece tüm kullanıcı hesapları görüntülenir. Aksi takdirde, Ata `CommandName` değeri `UsernameToMatch`.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

Bu kod yerinde filtreleme işlevselliğini test etmek. Sayfa ilk sitesini ziyaret ettiğinizde, tüm kullanıcı hesapları görüntülenir (Şekil 5'e yeniden bakın). A LinkButton tıklayarak geri gönderimin neden olur ve ile başlayan kullanıcı hesapları görüntüleme sonuçları filtrelenir.


[![Kullanıcı adı belirli bir harfle başlayan kullanıcılarla görüntülemek için filtre LinkButtons kullanın](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**Şekil 6**: Bu kullanıcıların Whose kullanıcı adı ile başlayan belirli bir harf görüntülemek için filtre LinkButtons kullanın ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>4. adım: Sayfalama kullanacak biçimde GridView güncelleştiriliyor

Şekil 5 ve 6'da gösterilen GridView döndürülen kayıtların tümünü listeler `FindUsersByName` yöntemi. Yüzlerce veya binlerce kullanıcı hesaplarının varsa bu bilgileri aşırı yüklemesine (tüm LinkButton tıklatıldığında veya başlangıçta sayfasını ziyaret zaman olduğu gibi) tüm hesapları görüntülerken neden olabilir. Kullanıcı hesaplarını daha kolay yönetilebilir yığınlar halinde sunmak yardımcı olmak için bir kerede 10 kullanıcı hesaplarını görüntülemek için GridView yapılandıralım.

GridView denetimini iki tür disk belleği sunar:

- **Varsayılan disk belleği** - uygulamak kolay, ancak verimsiz. GridView disk belleği varsayılan olarak koysalar bekliyor *tüm* veri kaynağından kayıtlar. Ardından yalnızca kayıt uygun sayfasını görüntüler.
- **Özel sayfalama** -uygulamak için daha fazla iş gerektirir, ancak verileri disk belleği özel kaynak kayıtları görüntülemek için yalnızca kesin kümesini döndürür; çünkü varsayılan disk belleği değerinden daha verimli olur.

Varsayılan ve özel sayfalama arasındaki performans farkının binlerce kayıt sayfalama oldukça önemli olabilir. Oluşturmakta olduğunuz çünkü bu arabirim, olmadığı varsayılarak yüzlerce veya binlerce kullanıcı hesaplarının olması, özel sayfalama kullanalım.

> [!NOTE]
> Varsayılan ve özel sayfalama yanı sıra, uygulama özel disk belleği söz konusu zorluklar arasındaki farklar hakkında daha kapsamlı bir açıklama için bkz [verimli bir şekilde disk belleği üzerinden büyük miktarlarda veri](https://asp.net/learn/data-access/tutorial-25-cs.aspx). Bazı varsayılan ve özel sayfalama arasındaki performans farkının analizi için bkz: [SQL Server 2005'te ASP.NET özel disk belleği](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


İlk GridView tarafından görüntülenen kayıtları hassas alt almak bazı mekanizma ihtiyacımız özel sayfalama uygulamak için. İyi haber olan `Membership` sınıfının `FindUsersByName` yöntemi bize sayfa dizini ve sayfa boyutunu belirtmek izin veren bir aşırı sahiptir ve bu kayıtların aralığında kullanıcı hesaplarını döndürür.

Özellikle, bu aşırı aşağıdaki imzası vardır: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

*PageIndex* parametresi, döndürülecek; kullanıcı hesapları sayfa belirtir *pageSize* sayfa başına görüntülemek için kaç tane kayıtları gösterir. *TotalRecords* parametresi bir `out` parametre sayısı toplam kullanıcı hesapları, kullanıcı deposunda döndürür.

> [!NOTE]
> Tarafından döndürülen veri `FindUsersByName` kullanıcı adı tarafından; sıralanmış sıralama ölçütü özelleştirilemez.


GridView, özel disk belleği kullanmasına, ancak yalnızca zaman ObjectDataSource denetimine bağlı şekilde yapılandırılabilir. ObjectDataSource Denetimi özel sayfalama uygulamak iki yöntem gerektiriyor: başlangıç satır dizini ve en fazla görüntülemek için kayıt sayısı geçirilen bir ve bu aralık içinde; kalan kayıtları hassas alt küme döndürür ve kayıtlarının toplam sayısı döndüren bir yöntem aracılığıyla havuzda. `FindUsersByName` Aşırı sayfa dizini ve sayfa boyutunu kabul edip kayıtlarda toplam sayısını döndürür bir `out` parametresi. Bu nedenle bir arabirim uyuşmazlığı var.

ObjectDataSource bekler ve ardından dahili olarak çağırır bir arabirimi kullanıma sunan bir proxy sınıfı oluşturmak için bir seçenek olacaktır `FindUsersByName` yöntemi. Başka bir seçenek - ve biri için bu makalenin kullanacağız - kendi disk belleği arabirimi oluşturmak ve, GridView'ın yerleşik disk belleği arabirimi yerine kullanmaktır.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Bir ilk oluşturma önceki, ardından, en son disk belleği arabirimi

Şimdi bir disk belleği arabirimi ilk, önceki, sonraki ve son LinkButtons oluşturun. Önceki ona önceki sayfaya döner ancak ilk tıklatıldığında LinkButton kullanıcı verileri, ilk sayfasına yönlendirir. Benzer şekilde, sonraki ve son kullanıcı sonraki ve son sayfasına sırasıyla taşınır. Altına dört LinkButton denetimleri ekleme `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

Ardından, her LinkButton's için bir olay işleyicisi oluşturun `Click` olaylar.

Şekil 7 Visual Web Developer Tasarım görünümü görüntülendiğinde dört LinkButtons gösterir.


[![İlk, önceki, sonraki ekleyin ve GridView altındaki LinkButtons son](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**Şekil 7**: ekleme ilk, önceki, sonraki ve son LinkButtons altındaki GridView ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Geçerli sayfa dizini izler

Ne zaman bir kullanıcı ilk kez ziyaret `ManageUsers.aspx` sayfa veya tıklama filtreleme birini düğmeleri, GridView veri'nın ilk sayfasında görüntülenecek istiyoruz. Ancak, kullanıcı LinkButtons Gezinti tıklattığında biz sayfa dizini güncelleştirmeniz gerekir. Sayfa dizini ve sayfa başına görüntülenecek kayıt sayısını korumak için aşağıdaki iki özelliği sayfanın arka plan kodu sınıfına ekleyin:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

Gibi `UsernameToMatch` özelliği, `PageIndex` kalıcı durumunu görüntülemek için değeri. Salt okunur `PageSize` özelliği 10 bir sabit kodlanmış değeri döndürür. Aynı deseni olarak kullanmak için bu özelliği güncelleştirmek için ilgi okuyucu davet `PageIndex`ve ardından büyütmek için `ManageUsers.aspx` sağlayacak şekilde sayfasını ziyaret kişi başına görüntülemek için kaç kullanıcı hesapları sayfa belirtin sayfasında.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Yalnızca geçerli sayfanın kayıtları alma, sayfa dizini güncelleştirme ve etkinleştirme ve disk belleği arabirimi LinkButtons devre dışı bırakma

Disk belleği arabirimi yerinde ve `PageIndex` ve `PageSize` özellikler eklenir, biz güncelleştirmeye hazır `BindUserAccounts` olan uygun kullanan yöntemi `FindUsersByName` aşırı yükleme. Ayrıca, biz etkinleştirmek veya devre dışı hangi sayfa görüntülenen bağlı olarak disk belleği arabirimi bu yöntem olması gerekir. Veri'nın ilk sayfasında görüntülerken, ilk ve önceki bağlantılar devre dışı bırakılması gerekir; Sonraki ve son son sayfayı görüntülerken devre dışı bırakılmalıdır.

Güncelleştirme `BindUserAccounts` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

Aracılığıyla havuzda kayıtlarının toplam sayısı son parametresiyle belirlenir Not `FindUsersByName` yöntemi. Bu bir `out` parametresi, bu nedenle öncelikle bu değer tutmak için bir değişken bildirin gerekir (`totalRecords`) ve ardından önüne `out` anahtar sözcüğü.

Belirtilen sayfa kullanıcı hesaplarının döndürülür sonra dört LinkButtons etkin ya da olup verilerin ilk veya son sayfasına görüntülenmekte olan bağlı olarak devre dışı.

Son adım dört LinkButtons için kod yazmaktır `Click` olay işleyicileri. Bu olay işleyicileri güncelleştirmeniz gerektiğinde `PageIndex` özelliği ve çağrısıyla GridView verileri yeniden bağlayın `BindUserAccounts`. İlk, önceki ve sonraki olay işleyicileri çok basittir. `Click` Son LinkButton için olay işleyicisini ancak olduğundan biraz daha karmaşık biz son sayfa dizini belirlemek için kaç tane kayıt görüntülendiğinden belirlemeniz gerekir.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

Şekil 8 ve 9 özel disk belleği arabirimi eylemde gösterir. Şekil 8 gösterir `ManageUsers.aspx` sayfasında veri tüm kullanıcı hesapları için'ın ilk sayfasında görüntülerken. Yalnızca 10 13 hesaplarının görüntüleneceğini unutmayın. İleri'yi veya son bağlantı tıklatıldığında geri gönderimin, güncelleştirmelerinin `PageIndex` 1 ve ikinci sayfasında kullanıcı hesapları kılavuza bağlar (bkz. Şekil 9).


[![İlk 10 kullanıcı hesapları görüntülenir](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**Şekil 8**: ilk 10 kullanıcı hesapları görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))


[![Sonraki bağlantı tıklatıldığında kullanıcı hesapları'nın ikinci sayfasında görüntüler](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**Şekil 9**: sonraki bağlantı tıklatıldığında, ikinci sayfa olan kullanıcı hesaplarını görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))


## <a name="summary"></a>Özet

Yöneticiler genellikle bir kullanıcı hesapları listesinden seçmeniz gerekir. Önceki eğitimlerine biz kullanıcılarla doldurulmuş bir açılır liste kullanarak Aranan, ancak bu yaklaşım iyi ölçeklenmez. Biz bu öğreticide daha iyi bir alternatif incelediniz: sonuçlarını bir disk belleğine alınan GridView görüntülenen filtrelenebilir bir arabirim. Bu kullanıcı arabirimi ile yöneticiler hızlı ve verimli şekilde bulun ve binlerce arasında bir kullanıcı hesabını seçin.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [SQL Server 2005 ile ASP.NET özel disk belleği](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Verimli bir şekilde büyük miktarlarda verinin disk belleği](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [Kendi Web sitesi yönetim aracı alınıyor](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Alicja Maziarz oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bir satırında bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](recovering-and-changing-passwords-cs.md)
