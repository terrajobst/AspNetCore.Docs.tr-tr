---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Pek çok (VB) kullanıcı hesabından birinin seçilmesi için bir arabirim oluşturma | Microsoft Docs
author: rick-anderson
description: Bu öğreticide bir kullanıcı arabirimi ile bir disk belleği, filtrelenebilir kılavuz oluşturulacak. Özellikle, kullanıcı arabirimimizi LinkButtons için bir dizi oluşur...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: bb30c5d3ce6e04f60d8192e8ed0404b89031b4b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756120"
---
<a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Pek çok (VB) kullanıcı hesabından birinin seçilmesi için bir arabirim oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> Bu öğreticide bir kullanıcı arabirimi ile bir disk belleği, filtrelenebilir kılavuz oluşturulacak. Özellikle, kullanıcı arabirimimizi LinkButtons bir dizi kullanıcı adı ve eşleşen kullanıcıları göstermek için bir GridView denetimi başlangıç harfi göre sonuçları filtrelemek için oluşur. Tüm kullanıcı hesaplarına GridView listeleyerek başlayacağız. Ardından, adım 3'te LinkButtons filtre ekleyeceğiz. 4. adım, filtrelenmiş sonuçları disk belleği arar. 4'ten adım 2'de oluşturulan arabirimi sonraki öğreticilerde, belirli bir kullanıcı hesabı için yönetim görevlerini gerçekleştirmek için kullanılır.


## <a name="introduction"></a>Giriş

İçinde <a id="_msoanchor_1"> </a> [ *kullanıcılara roller atama* ](../roles/assigning-roles-to-users-vb.md) öğreticide oluşturduğumuz bir kullanıcı seçin ve kendi rolleri yönetmek bir yöneticinin ilkel bir arabirim. Özellikle, arabirim yönetici bir aşağı açılan listesi tüm kullanıcılar ile sunulur. Böyle bir arabirim vardır, ancak bir düzine kadar kullanıcı hesapları, ancak kullanışsız yüzlerce veya binlerce hesaplarına sahip siteler için uygundur. Bir disk belleği, filtrelenebilir büyük kullanıcı temellerine sahip Web siteleri için daha uygun kullanıcı arabirimi kılavuzdur.

Bu öğreticide bu tür bir kullanıcı arabirimi oluşturulacak. Özellikle, kullanıcı arabirimimizi LinkButtons bir dizi kullanıcı adı ve eşleşen kullanıcıları göstermek için bir GridView denetimi başlangıç harfi göre sonuçları filtrelemek için oluşur. Tüm kullanıcı hesaplarına GridView listeleyerek başlayacağız. Ardından, adım 3'te LinkButtons filtre ekleyeceğiz. 4. adım, filtrelenmiş sonuçları disk belleği arar. 4'ten adım 2'de oluşturulan arabirimi sonraki öğreticilerde, belirli bir kullanıcı hesabı için yönetim görevlerini gerçekleştirmek için kullanılır.

Haydi başlayalım!

## <a name="step-1-adding-new-aspnet-pages"></a>1. adım: Yeni ASP.NET sayfaları ekleme

Bu öğretici ve sonraki iki biz çeşitli yönetimle ilgili işlevleri ve özellikleri İnceleme. ASP.NET sayfaları, Bu öğretici incelenirken konuları uygulamak için bir dizi ihtiyacımız. Şimdi bu sayfaları oluşturun ve site haritası güncelleştirin.

Adlı projede yeni bir klasör oluşturarak başlayın `Administration`. Ardından, her bir sayfa ile bağlama klasörü, iki yeni ASP.NET sayfaları ekleyin `Site.master` ana sayfa. Sayfa adı:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Ayrıca Web sitesinin kök dizinine iki sayfa ekleyin: `ChangePassword.aspx` ve `RecoverPassword.aspx`.

Bu dört sayfaları bu noktada, her ana sayfanın ContentPlaceHolder iki içerik denetimlerine sahip gerekir: `MainContent` ve `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Ana sayfa için varsayılan işaretlemesini göstermek istiyoruz `LoginContent` ContentPlaceHolder bu sayfaları için. Bu nedenle, bildirim temelli biçimlendirme için kaldırma `Content2` içerik denetimi. Bunu yaptıktan sonra sayfalarının biçimlendirme yalnızca bir içerik denetimi içermelidir.

ASP.NET sayfaları içinde `Administration` klasör, yalnızca yönetim kullanıcılarına yöneliktir. Sistemde bir Yöneticiler rolünün ekledik <a id="_msoanchor_2"> </a> [ *oluşturma ve yönetme rolleri* ](../roles/creating-and-managing-roles-vb.md) öğretici; bu rolü iki bu sayfalara erişimi kısıtlayın. Bunu gerçekleştirmek için Ekle bir `Web.config` dosyasını `Administration` klasörüne ve yapılandırma kendi `<authorization>` öğesi havalı. Yöneticiler rolündeki kullanıcılar için ve diğerlerini reddetme.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

Bu noktada, projenizin Çözüm Gezgini, Şekil 1'de gösterilen ekran şuna benzemelidir.


[![Dört yeni sayfalar ve Web.config dosyası Web sitesine eklendi](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Şekil 1**: dört yeni sayfalar ve `Web.config` dosya, Web sitesine eklendi ([tam boyutlu görüntüyü görmek için tıklatın](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))


Son olarak, site haritası güncelleştirin (`Web.sitemap`) bir giriş eklemek için `ManageUsers.aspx` sayfası. Sonra aşağıdaki XML ekleme `<siteMapNode>` rolleri öğreticileri için ekledik.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

Site haritası ile güncelleştirilmiş bir tarayıcı aracılığıyla sitesini ziyaret edin. Şekil 2 gösterildiği gibi sol taraftaki gezinti artık Yönetim öğreticileri için öğeleri içerir.


[![Site Haritası kullanıcı yönetimi başlıklı bir düğüm içerir.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Şekil 2**: düğüm başlıklı bir kullanıcı yönetim Site Haritası içerir ([tam boyutlu görüntüyü görmek için tıklatın](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>2. adım: GridView tüm kullanıcı hesapları listeleme

Bu öğretici için son Hedefimiz yönetici yönetmek için bir kullanıcı hesabı seçmek disk belleğine alınan, filtrelenebilir kılavuz oluşturmaktır. Liste tarafından başlayalım *tüm* GridView kullanıcılar. Bu işlem tamamlandıktan sonra filtreleme ve sayfalama arabirimleri ve işlevsellik ekleyeceğiz.

Açık `ManageUsers.aspx` sayfasını `Administration` klasör ve bir GridView ekleme ayarı kendi `ID` için `UserAccounts` de biz GridView kullanan kullanıcı hesapları kümesi bağlamak için kod yazacaksınız `Membership` sınıfın `GetAllUsers` yöntemi. Önceki öğreticilerde, açıklandığı gibi `GetAllUsers` yöntemi döndürür bir `MembershipUserCollection` bir koleksiyon nesne, `MembershipUser` nesneleri. Her `MembershipUser` koleksiyon gibi özellikler içeren `UserName`, `Email`, `IsApproved`ve benzeri.

GridView'içinde istenen kullanıcı hesabı bilgilerini görüntülemek için GridView'ın ayarlayın `AutoGenerateColumns` özelliğini False olarak ve eklemek için BoundFields `UserName`, `Email`, ve `Comment` özellikleri ve CheckBoxFields için `IsApproved`, `IsLockedOut`, ve `IsOnline` özellikleri. Bu yapılandırma, denetimin bildirim temelli biçimlendirme veya alanları iletişim kutusu aracılığıyla uygulanabilir. Şekil 3 alanları ekran görüntüsü otomatik oluştur alanları kutusunun işareti kaldırıldı ve BoundFields ve CheckBoxFields eklenen yapılandırılır ve sonra iletişim kutusu gösterir.


[![Üç BoundFields ve üç CheckBoxFields GridView'a Ekle](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Şekil 3**: üç BoundFields ekleyin ve GridView için üç CheckBoxFields ([tam boyutlu görüntüyü görmek için tıklatın](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))


GridView yapılandırdıktan sonra bildirim temelli biçimlendirme aşağıdakine benzer olduğundan emin olun:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

Ardından, biz kullanıcı hesaplarını GridView'a bağlayan kod yazmanız gerekir. Adlı bir yöntem oluşturma `BindUserAccounts` bu görevi gerçekleştirmek ve ondan sonra çağırmak için `Page_Load` olay işleyicisini ilk sayfasını ziyaret edin.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

Bir tarayıcı aracılığıyla sayfada test etmek için bir dakikamızı ayıralım. Şekil 4'te gösterildiği gibi `UserAccounts` GridView sistemde kullanıcı adı, e-posta adresi ve diğer tüm kullanıcılar için uygun hesabı bilgileri listeler.


[![Kullanıcı hesaplarını GridView içinde listelenir](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Şekil 4**: GridView kullanıcı hesapları listelenir ([tam boyutlu görüntüyü görmek için tıklatın](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>3. adım: kullanıcı adı ilk harfini sonuçlarını filtreleme

Şu anda `UserAccounts` GridView gösterir *tüm* kullanıcı hesaplarını. Yüzlerce veya binlerce kullanıcı hesapları ile Web siteleri için bu kullanıcı görüntülenen hesapları hızlı bir şekilde küçültmek mümkün olmazsa olmaz. Bu sayfaya LinkButtons filtreleme ekleyerek gerçekleştirilebilir. 27 LinkButtons sayfasına ekleyelim: bir başlıklı bir LinkButton için her alfabedeki tüm birlikte. Ziyaretçi tüm LinkButton tıklarsa, GridView tüm kullanıcıları göster. Bunlar belirli bir harfi tıklarsanız, yalnızca kullanıcı adı seçili harfle başlayan kullanıcılara görüntülenir.

Bizim ilk görev 27 LinkButton denetimleri eklemektir. 27 LinkButtons bildirimli olarak, oluşturmak için bir seçenek olacaktır teker teker. Repeater denetimiyle kullanmak için daha esnek bir yaklaşım olan bir `ItemTemplate` bir LinkButton işler ve yineleyici filtreleme seçenekleri bağlandığı bir `String` dizi.

Repeater denetimiyle yukarıdaki sayfasına ekleyerek başlangıç `UserAccounts` GridView. Repeater'ın ayarlayın `ID` özelliğini `FilteringUI` Repeater'ın şablonları yapılandırma böylece kendi `ItemTemplate` bir LinkButton işler, `Text` ve `CommandName` özellikleri geçerli dizi öğesine bağlıdır. İçinde gördüğümüz gibi <a id="_msoanchor_3"> </a> [ *kullanıcılara roller atama* ](../roles/assigning-roles-to-users-vb.md) öğretici, bu gerçekleştirilebilir kullanarak `Container.DataItem` veri bağlama söz dizimi. Repeater'ın kullanın `SeparatorTemplate` her bağlantı arasındaki bir dikey çizgi görüntülenecek.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

Bu Repeater istenen filtreleme seçenekleri ile doldurmak için adlı bir yöntem oluşturma `BindFilteringUI`. Bu yöntemi çağırmak mutlaka `Page_Load` ilk sayfa yüklenmesinden üzerinde olay işleyicisi.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Bu yöntem, öğeleri olarak filtreleme seçeneklerini belirtir. `String` dizi `filterOptions` dizideki her öğe için bir Linkbutton'a Repeater işlenir, `Text` ve `CommandName` dizinin değer atanmış özellikleri öğe.

Şekil 5 gösterir `ManageUsers.aspx` sayfasında bir tarayıcıdan görüntülendiğinde.


[![Yineleyici 27 filtreleme LinkButtons listeler](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Şekil 5**: yineleyici listeler 27 filtreleme LinkButtons ([tam boyutlu görüntüyü görmek için tıklatın](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))


> [!NOTE]
> Kullanıcı adlarını, sayıları ve noktalama işaretleri dahil olmak üzere, herhangi bir karakterle başlayabilir. Bu hesaplar görüntülemek için yönetici tüm LinkButton seçeneğini kullanmanız gerekecektir. Alternatif olarak, bir sayı ile başlayan tüm kullanıcı hesaplarını döndürülecek bir LinkButton ekleyebilirsiniz. Ben bunu bir alıştırma olarak için okuyucu bırakın.


Herhangi bir filtre LinkButtons tıklayarak geri göndermeye neden olur ve Repeater'ın başlatır `ItemCommand` olay, ancak kılavuzda herhangi bir değişiklik henüz için yaptığımız çünkü sonuçları filtrelemek için herhangi bir kod yazma. `Membership` Sınıfı içeren bir [ `FindUsersByName` yöntemi](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) kullanıcı adı belirtilen arama deseni ile eşleşen bu kullanıcı hesaplarını döndürür. Bu yöntem yalnızca kullanıcı hesapları tarafından belirtilen harfi başlayarak, kullanıcı adlarını almak için kullanabiliriz `CommandName` tıklandığını filtrelenmiş LinkButton biri.

Başlangıç güncelleştirerek `ManageUser.aspx` sayfanın gerideki kod sınıf adlı bir özellik içeren `UsernameToMatch` bu özellik, kullanıcı adı filtre dizesi Geri göndermeler arasında devam ederse:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

`UsernameToMatch` Atanan içine değeri depolar. özellik `ViewState` UsernameToMatch anahtarı kullanarak bir koleksiyon. Bu özelliğin değerini okunduğunda, bir değerin var olup olmadığını görmek için denetler `ViewState` koleksiyonu; Aksi halde, varsayılan değer, boş bir dize döndürür. `UsernameToMatch` Özelliği, yani böylece özellik değişiklikleri geri göndermeler arasında kalıcı durumunu görüntülemek için bir değer kalıcı hale getirmeniz, yaygın bir düzen sergiler. Bu desen hakkında daha fazla bilgi için okuma [anlama ASP.NET görüntüleme durumu](https://msdn.microsoftn-us/library/ms972976.aspx).

Ardından, güncelleştirme `BindUserAccounts` yöntemi çağırmak yerine bu nedenle, `Membership.GetAllUsers`, çağrı `Membership.FindUsersByName`, geçen değerini `UsernameToMatch` SQL joker karakterli eklenen özellik %.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Kullanıcı adı bir harfle başlayan yalnızca bu kullanıcılara görüntülenecek kümesi `UsernameToMatch` özellik a ve sonra çağrı `BindUserAccounts` bu çağrıda sonuçlanır `Membership.FindUsersByName("A%")`, A. döndürmekiçinbenzerşekildeiletümkullanıcılar,kullanıcıadınıdöndüreceğibaşlatır*tüm* kullanıcıları atamak için boş bir dize `UsernameToMatch` özelliği böylece `BindUserAccounts` yöntemi çağırılır `Membership.FindUsersByName("%")`, böylece döndürerek tüm kullanıcı hesapları.

Yineleyici için ait bir olay işleyicisi oluşturun `ItemCommand` olay. Her bir filtrenin LinkButtons tıklatıldığında, bu olay tetiklenir; tıklandı LinkButton's geçirilir `CommandName` aracılığıyla değer `RepeaterCommandEventArgs` nesne. Uygun değere atamak ihtiyacımız `UsernameToMatch` özelliği ve sonra çağrı `BindUserAccounts` yöntemi. Varsa `CommandName` tüm, boş bir dize olarak atama `UsernameToMatch` böylece tüm kullanıcı hesapları görüntülenir. Aksi takdirde Ata `CommandName` değeri `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

Bu kod bir yerde filtreleme işlevini test edin. Sayfa ilk ziyaret edildiğinde, tüm kullanıcı hesapları görüntülenir (Şekil 5'e yeniden bakın). A LinkButton tıklayarak geri göndermeye neden olur ve yalnızca A ile başlayan kullanıcı hesaplarını görüntülemek, sonuçları filtreler.


[![Kullanıcı adı belli bir harfle başlar bu kullanıcıları görüntülemek için filtre LinkButtons kullanın](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Şekil 6**: Bu kullanıcılar Whose kullanıcı adı ile başlayan belirli bir harfi görüntülemek için filtre LinkButtons kullanın ([tam boyutlu görüntüyü görmek için tıklatın](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>4. adım: Disk belleği kullanmayı GridView güncelleştiriliyor

Şekil 5 ve 6'da gösterilen GridView tüm döndürülen kayıtlar listeler `FindUsersByName` yöntemi. Yüzlerce veya binlerce kullanıcı hesapları varsa bu bilgilerin aşırı (tüm LinkButton tıklandığında veya sayfa ilk ziyaret edildiğinde olduğu gibi) tüm hesapları görüntülerken yol açabilir. Kullanıcı hesaplarını daha yönetilebilir yığınlar halinde sunmak amacıyla, bir kerede 10 kullanıcı hesaplarını görüntülemek için GridView yapılandıralım.

GridView denetiminde iki tür disk belleği sunar:

- **Varsayılan disk belleği** - uygulanması kolaydır, ancak verimsiz. Buna koysalar GridView disk belleği varsayılan bekliyor *tüm* veri kaynağından kayıtlar. Ardından yalnızca uygun kayıt sayfasını görüntüler.
- **Özel disk belleği** -uygulamak için daha fazla iş gerektirir, ancak veri sayfalama özel kaynak kayıtları görüntülemek için yalnızca kesin kümesini döndürür; çünkü varsayılan disk belleği değerinden daha verimli olur.

Varsayılan ve özel disk belleği arasındaki performans farkının binlerce kayıt sayfalama oldukça önemli olabilir. Oluşturmakta olduğunuz çünkü bu arabirimi olduğunu varsayarak yüzlerce veya binlerce kullanıcı hesapları, özel disk belleği kullanalım.

> [!NOTE]
> Varsayılan ve özel disk belleği yanı sıra, uygulama özel disk belleği ilgili zorlukları arasındaki farklılıklarla ilgili daha kapsamlı bir açıklama için bkz [verimli bir şekilde sayfalama aracılığıyla büyük miktarda veri](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Bazı varsayılan ve özel disk belleği arasındaki performans farkının analize bakın [SQL Server 2005'te ASP.NET özel disk belleği](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Özel disk belleği uygulamak için önce GridView tarafından görüntülenen kayıtları kesin kümesini almak bazı mekanizma ihtiyacımız var. Güzel bir haberimiz var olan `Membership` sınıfın `FindUsersByName` yöntemi sayfa boyutu ve sayfa dizini belirtmek olanak sağlayan bir aşırı yüklenmiş ve bu kayıtların aralığında kullanıcı hesaplarını döndürür.

Özellikle, aşağıdaki imzası Bu aşırı yüklemesi vardır: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

*PageIndex* parametresi, döndürülecek; kullanıcı hesapları sayfanın belirtir *pageSize* sayfa başına görüntülenecek kaç kayıtları gösterir. *TotalRecords* parametresi bir `ByRef` parametre sayısı toplam kullanıcı hesapları kullanıcı deposunda döndürür.

> [!NOTE]
> Tarafından döndürülen veriler `FindUsersByName` ; username sıralı sıralama ölçütü özelleştirilemez.


GridView kullanan özel disk belleği, ancak yalnızca ne zaman bir ObjectDataSource denetimine bağlı şekilde yapılandırılabilir. İsteğe bağlı olarak ObjectDataSource Denetimi özel disk belleği uygulamak iki yöntem gerektirir: bir başlangıç satır dizini ve görüntülemek için kayıt sayısı geçirilir ve bu aralık içinde; kalan kayıt kesin alt kümesi döndürür ve kayıt toplam sayısını döndüren bir yöntem aracılığıyla disk belleği. `FindUsersByName` Aşırı sayfa dizini ve sayfa boyutu kabul eder ve kayıtlarda toplam sayısını döndürür bir `ByRef` parametresi. Bu nedenle bir arabirim uyuşmazlığı yoktur.

ObjectDataSource bekler ve ardından dahili olarak çağırır arabirimi kullanıma sunan bir proxy sınıfı oluşturmak için bir seçenek olacaktır `FindUsersByName` yöntemi. Başka bir seçenek - ve biri için bu makaleyi kullanacağız - kendi sayfalama arabirimi oluşturun ve GridView'ın yerleşik sayfalama arabirimi yerine kullanan değildir.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Bir ilk önceki, ardından, en son disk belleği arabirimi

Disk belleği arabirimi ilk, Previous, İleri ve son LinkButtons oluşturalım. Önceki ona önceki sayfasına döndürür ancak ilk tıklandığında LinkButton kullanıcı verileri, ilk sayfasına olacaktır. Benzer şekilde, sonraki ve son kullanıcı sonraki ve son sayfasına sırasıyla taşınır. Dört LinkButton denetim altına eklemek `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

Ardından, her LinkButton's için bir olay işleyicisi oluşturun `Click` olayları.

Şekil 7 Visual Web Developer Tasarım görünümü görüntülendiğinde dört LinkButtons gösterir.


[![Ardından ilk, önceki, ekleyin ve GridView altındaki LinkButtons en son](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Şekil 7**: ilk ekleyin, önceki, İleri ve son LinkButtons altındaki GridView ([tam boyutlu görüntüyü görmek için tıklatın](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Geçerli sayfa dizini izler

Ne zaman bir kullanıcı ilk kez ziyaret `ManageUsers.aspx` sayfası veya tıklama filtreleme birini düğmeler, veri'nın ilk sayfasında GridView görüntülemek istiyoruz. Ancak, kullanıcı Gezinti LinkButtons tıkladığında, biz sayfa dizini güncelleştirmeniz gerekir. Sayfa dizini ve sayfa başına görüntülenecek kayıt sayısını korumak için sayfanın arka plan kod sınıfına aşağıdaki iki özelliği ekleyin:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Gibi `UsernameToMatch` özelliği `PageIndex` özelliği devam ederse, durumu görüntülemek için değeri. Salt okunur `PageSize` özelliği 10 bir sabit kodlu değer döndürür. Aynı düzeni olarak kullanmak için bu özelliği güncelleştirmek için ilginizi okuyucu davet ettiğim `PageIndex`ve sonra büyütmek için `ManageUsers.aspx` sayfa başına görüntülenecek kaç kullanıcı hesapları sayfasını ziyaret ederek kişi belirtebilirsiniz, sayfa.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Yalnızca geçerli sayfanın kayıtlarını alma, sayfa dizini güncelleştiriliyor ve etkinleştirme ve disk belleği arabirimi LinkButtons devre dışı bırakma

Yerinde disk belleği arabirimiyle ve `PageIndex` ve `PageSize` özellikler eklenir, biz güncelleştirmeye hazır `BindUserAccounts` BT'nin uygun kullanması için yöntemi `FindUsersByName` aşırı yükleme. Ayrıca, biz etkinleştirmek veya devre dışı disk belleği arabirimi hangi sayfasında görüntülenen bağlı olarak bu yöntem olması gerekir. İlk veri sayfası görüntülendiğinde, ilk ve önceki bağlantıların devre dışı bırakılması gerekir; Daha sonra ve son son sayfayı görüntülerken devre dışı bırakılmalıdır.

Güncelleştirme `BindUserAccounts` yöntemini aşağıdaki kod ile:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Kayıtları aracılığıyla havuzda toplam sayısı son parametresi tarafından belirlenir Not `FindUsersByName` yöntemi. Belirtilen sayfaya kullanıcı hesaplarının döndürülür sonra dört LinkButtons etkin veya olup verilerin ilk veya son sayfasına görüntülenmekte olan bağlı olarak devre dışı.

Son adım dört LinkButtons için kod yazmaktır `Click` olay işleyicileri. Bu olay işleyicileri güncelleştirmeniz gerekiyor `PageIndex` özelliği ve ardından bir çağrı aracılığıyla GridView verileri rebind `BindUserAccounts` ilk, önceki ve sonraki olay işleyicileri çok basittir. `Click` Son LinkButton için olay işleyicisi ancak olduğundan biraz daha karmaşık biz son sayfa dizini belirlemek için kaç tane kaydın görüntüleniyor belirlemeniz gerekir.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

Şekil 8 ve 9 özel disk belleği arabirim uygulamalı olarak göstermek. Şekil 8 gösterir `ManageUsers.aspx` sayfasında veri tüm kullanıcı hesapları için'ın ilk sayfasında görüntülerken. Yalnızca 10 13 hesaplarının görüntüleneceğini unutmayın. İleri'yi veya son bağlantı tıklatıldığında geri gönderme, güncelleştirmeleri `PageIndex` 1 ve ikinci sayfasında kullanıcı hesapları kılavuza bağlar (bkz. Şekil 9).


[![İlk 10 kullanıcı hesapları görüntülenir](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Şekil 8**: ilk 10 kullanıcı hesapları görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))


[![Kullanıcı hesapları'nın ikinci sayfasında İleri bağlantı tıklatıldığında görüntüler](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Şekil 9**: sonraki bağlantısını tıklatarak görüntüler ikinci sayfa kullanıcı planı ([tam boyutlu görüntüyü görmek için tıklatın](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))


## <a name="summary"></a>Özet

Yöneticiler, genellikle bir kullanıcı hesapları listesinden seçmeniz gerekir. Önceki öğreticilerde kullanıcılarla doldurulmuş açılan listesini kullanarak olan incelemiştik, ancak bu yaklaşım iyi ölçeklenmez. Bu öğreticide size daha iyi bir alternatif incelediniz: bir filtrelenebilir arabirimi sonuçlarını, disk belleğine alınan GridView içinde görüntülenir. Bu kullanıcı arabirimi ile yöneticiler hızla ve verimli bir şekilde bulun ve binlerce arasında bir kullanıcı hesabı seçin.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [SQL Server 2005 ile ASP.NET özel disk belleği](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Büyük miktarda veriyi etkili bir şekilde sayfalama](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [Kendi Web sitesi yönetim aracı alınıyor](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET Books yazar ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan  *[Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott, konumunda ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Alicja Maziarz oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak

> [!div class="step-by-step"]
> [Önceki](unlocking-and-approving-user-accounts-cs.md)
> [İleri](recovering-and-changing-passwords-vb.md)
