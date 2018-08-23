---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: Temel güvenlik kavramları ve ASP.NET desteği (VB) | Microsoft Docs
author: rick-anderson
description: Bu, partic erişimi yetkilendirme ziyaretçilerin web formu aracılığıyla kimlik doğrulaması için teknikleri inceleyeceksiniz öğretici serisinin ilk öğreticisidir...
ms.author: riande
ms.date: 01/13/2008
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: c293f4946001d0940c7e12b67f4fbc63cf5ca18c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755575"
---
<a name="security-basics-and-aspnet-support-vb"></a>Temel güvenlik kavramları ve ASP.NET desteği (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF'yi indirin](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> Ziyaretçilerin web formu aracılığıyla kimlik doğrulaması, erişimi belirli sayfalara ve işlevselliği için yetkilendirme ve bir ASP.NET uygulamasında kullanıcı hesaplarını yönetme teknikleri inceleyeceksiniz öğretici serisinin ilk öğreticide budur.


## <a name="introduction"></a>Giriş

Bir şey forumlar, e-ticaret siteleri, çevrimiçi e-posta Web siteleri, portal Web sitelerinin ve tüm ortak olmayan sosyal ağ siteleri nedir? Tüm sundukları *kullanıcı hesaplarını*. Kullanıcı hesaplarını teklif siteleri birkaç hizmet sağlamanız gerekir. En az bir hesap oluşturmak yeni ziyaretçiler gerekir ve dönen ziyaretçiler oturum açamaz. Bu web uygulamaları üzerinde oturum açmış olan kullanıcının kararlar verebilen: bazı sayfaları veya Eylemler kullanıcılar veya belirli bir alt kümesiyle; oturum yalnızca sınırlı olabilir diğer sayfalara bilgi oturum açmış kullanıcıya belirli gösterebilir veya daha fazla veya daha az bilgi, hangi kullanıcı, sayfa görüntüleme bağlı olarak gösterebilir.

Ziyaretçilerin web formu aracılığıyla kimlik doğrulaması, erişimi belirli sayfalara ve işlevselliği için yetkilendirme ve bir ASP.NET uygulamasında kullanıcı hesaplarını yönetme teknikleri inceleyeceksiniz öğretici serisinin ilk öğreticide budur. Bu eğitim kursunda inceleyeceğiz nasıl yapılır:

- Tanımlamak ve kullanıcıların bir Web sitesinde oturum
- ASP kullanın. Kullanıcı hesaplarını yönetmek için NET üyelik framework
- Kullanıcı hesaplarını oluşturma ve güncelleştirme
- Bir web sayfası, dizin veya belirli işlevleri oturum açmış kullanıcıya göre erişimi sınırlayın
- ASP kullanın. Kullanıcı hesaplarını rolleriyle ilişkilendirmek üzere NET'in rolleri framework
- Kullanıcı rollerini yönetme
- Bir web sayfası, dizin veya belirli işlevleri oturum açmış kullanıcının rol tabanlı erişimi sınırlayın
- Özelleştirebilir ve ASP genişletebilirsiniz. NET güvenlik Web denetimleri

Bu öğretici, kısa ve görsel olarak işleminde size kılavuzluk için yeterince ekran görüntüleri ile adım adım yönergeler sağlamak için sağlamıştır. Her öğretici, C# ve Visual Basic sürümlerinde kullanılabilir ve kullanılan tüm kod indirilmesini içerir. (Bu ilk öğreticide üst düzey bir bakış açısı gelen güvenlik kavramları odaklanır ve ilişkili herhangi bir kod içermiyor.)

Bu öğreticide önemli güvenlik kavramları ve ASP.NET forms kimlik doğrulaması, yetkilendirme, kullanıcı hesaplarını ve rol uygulama yardımcı olmak için hangi tesislerde kullanılabilir ele alınacaktır. Haydi başlayalım!

> [!NOTE]
> Güvenlik yayılan fiziksel, teknolojik herhangi bir uygulamanın önemli bir parçasıdır ve ilke kararları ve yüksek derecede planlama ve etki alanı bilgilerini gerektirir. Bu öğretici serisinde, güvenli web uygulamaları geliştirmek için bir kılavuz olarak tasarlanmamıştır. Bunun yerine, özel form kimlik doğrulaması, yetkilendirme, kullanıcı hesaplarını ve rolleri üzerinde odaklanır. Bu seride bu sorunların çözüm uzayda bazı güvenlik kavramları ele alınmıştır, ancak diğerleri bırakılır keşfedilmemiş.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Kimlik doğrulaması, yetkilendirme, kullanıcı hesaplarını ve rolleri

Kimlik doğrulaması, yetkilendirme, kullanıcı hesaplarını ve rolleri web güvenlik bağlamında bu kullanım koşullarını tanımlamak için hızlı bir dakikanızı ayırın istiyor musunuz böylece Bu öğretici serisinin çok sık kullanılacak dört terimlerdir. Internet gibi bir istemci-sunucu modeli sunucu isteği yapan istemcinin belirlemek gereken birçok senaryo vardır. *Kimlik doğrulaması* istemcinin kimliğini ascertaining işlemidir. Başarılı bir şekilde tanımlanan bir istemci olarak kabul edilir *kimliği doğrulanmış*. Tanımlanamayan bir istemci olarak kabul edilir *kimliği doğrulanmamış* veya *anonim*.

Güvenli kimlik doğrulama sistemleri aşağıdaki üç modelleri en az birini içeren: [bildiğiniz bir şey, sahip olduğunuz bir şey veya bir şey olduğunuz](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). Çoğu web uygulaması, bir parola veya PIN gibi istemci bildiği bir şeyi dayanır. -Her kullanıcı adı ve parola, örneğin - bir kullanıcıyı tanımlamak için kullanılan bilgileri verilir *kimlik bilgilerini*. Bu öğretici serisinin odaklanır *form kimlik doğrulaması*, burada kullanıcılar oturum siteye bir web sayfası formundan kendi kimlik bilgilerini sağlayarak bir kimlik doğrulama modeli olduğu. Biz tüm bu tür bir kimlik önce karşılaşmıştır. Tüm e-ticaret sitesine gidin. Kullanıma hazır olduğunuzda metin kutuları içinde bir web sayfasında kullanıcı adı ve parola girerek oturum istenir.

İstemcileri belirlemeye ek olarak, hangi işlevleri veya kaynaklar isteği yapan istemcinin bağlı olarak erişilebilir sınırlamak bir sunucu gerekebilir. *Yetkilendirme* belirli bir kullanıcı belirli bir kaynak ya da işlevselliğe erişmek için yetkili olup olmadığını belirleme işlemidir.

A *kullanıcı hesabı* belirli bir kullanıcı hakkında kalıcı bilgi deposudur. Kullanıcı hesaplarına en düşük düzeyde kullanıcı, kullanıcı oturum açma adı ve parola gibi benzersiz olarak tanımlayan bilgiler içermelidir. Bu temel bilgilerin yanı sıra, kullanıcı hesapları gibi şeyler içerebilir: kullanıcının e-posta adresi; hesabının oluşturulduğu saat ve tarih; Tarih ve saat, son oturum; adı ve Soyadı; telefon numarası; ve posta adresi. Forms kimlik doğrulaması kullanırken kullanıcı hesabı bilgileri genellikle Microsoft SQL Server gibi ilişkisel bir veritabanında depolanır.

Kullanıcı hesaplarını destekleyen web uygulamaları kullanıcılarına isteğe bağlı olarak grup *rolleri*. Yalnızca Yetkilendirme kuralları ve sayfa düzeyi işlevselliği tanımlamak için bir Özet sağlar ve bir kullanıcı için uygulanan bir etiket rolüdür. Örneğin, bir Web sitesi web sayfaları belirli bir kümesini erişmek için yönetici anahtarlarınızla engelle yetkilendirme kuralları ile bir yönetici rolü içerebilir. Ayrıca, çeşitli sayfalar (Yönetici olmayanlar dahil) tüm kullanıcılar için erişilebilir olan bir ek verileri görüntüleyebilir veya Yöneticiler rolündeki kullanıcılar tarafından ziyaret edildiğinde fazladan işlevsellik sunar. Rolleri kullanarak, biz bir rol tarafından rol başına kullanıcı tarafından yerine bu Yetkilendirme kuralları tanımlayabilirsiniz.

## <a name="authenticating-users-in-an-aspnet-application"></a>Bir ASP.NET uygulamasında kullanıcı kimlik doğrulaması

Bir kullanıcı bir URL, tarayıcının adres penceresinde veya tıklama bağlantı girdiğinde, tarayıcı yapar bir [Köprü Metni Aktarım Protokolü (HTTP)](http://en.wikipedia.org/wiki/HTTP) web sunucusu için belirtilen içerik isteği, bir ASP.NET olması sayfası, resim, bir JavaScript Dosya veya başka bir içerik türü. Web sunucusu ile talep edilen içeriği döndüren görev. Bunu yaparken istek kimin yaptığını ve kimlik istenen içeriği almak için yetkili olup olmadığı dahil olmak üzere istekle ilgili birkaç belirlemeniz gerekir.

Varsayılan olarak, her türlü kimlik bilgileri eksik HTTP isteklerini tarayıcılar gönderin. Ancak, kimlik doğrulama bilgileri tarayıcı içeriyorsa web sunucusu isteği yapan istemcinin tanımlamaya çalışır kimlik doğrulama iş akışı başlatır. Web uygulaması tarafından kullanılan kimlik doğrulaması türü kimlik doğrulama iş akışı adımları bağlıdır. ASP.NET, üç kimlik doğrulaması türünü destekler: Windows, Passport ve form. Bu öğretici serisinde, form kimlik doğrulamasını odaklanır, ancak şimdi karşılaştırın ve Windows kimlik doğrulaması kullanıcı depoları ve iş akışı için bir dakikanızı ayırın.

### <a name="authentication-via-windows-authentication"></a>Windows kimlik doğrulaması üzerinden kimlik doğrulaması

Windows kimlik doğrulama iş akışı aşağıdaki kimlik doğrulama tekniklerden birini kullanır:

- Temel kimlik doğrulaması
- Özet kimlik doğrulaması
- Windows tümleşik kimlik doğrulaması

Tüm üç teknikleri yaklaşık aynı şekilde çalışır: bir yetkisiz olduğunda anonim istek geldiğinde, devam etmek için bu yetkilendirme belirten bir HTTP yanıtı gereklidir web sunucusuna geri gönderir. Tarayıcı, ardından kullanıcıdan kullanıcı adı ve parola (bkz. Şekil 1) için kalıcı bir iletişim kutusu görüntüler. Bu bilgiler daha sonra bir HTTP üst aracılığıyla web sunucuya geri gönderilir.


![Kalıcı bir iletişim kutusu, kullanıcının kendi kimlik bilgilerini ister.](security-basics-and-asp-net-support-vb/_static/image1.png)

**Şekil 1**: kalıcı bir iletişim kutusu, kullanıcının kendi kimlik bilgilerini ister.


Web sunucusunun Windows kullanıcı Store karşı sağlanan kimlik bilgilerinin doğrulanır. Başka bir deyişle, web uygulamanızın kimliği doğrulanmış her kullanıcı, kuruluşunuzdaki bir Windows hesabı olması gerekir. Sıradan bir hale intranet senaryolarda budur. Aslında, bir intranet ayarı Windows tümleşik kimlik doğrulaması kullanırken, tarayıcının otomatik olarak web sunucusu böylece Şekil 1'de gösterilen iletişim kutusu gizleme ağ oturum açmak için kullanılan kimlik bilgilerini sağlar. Windows kimlik doğrulamasını intranet uygulamaları için mükemmel olmakla birlikte, sitenizde kaydolan her kullanıcı için Windows hesapları oluşturmak istiyor musunuz beri Internet uygulamaları için genellikle seçeneğinin.

### <a name="authentication-via-forms-authentication"></a>Form kimlik doğrulaması üzerinden kimlik doğrulaması

Form kimlik doğrulaması, diğer taraftan, Internet web uygulamaları için idealdir. Form kimlik doğrulamasını geri çağırma kullanıcı web formu aracılığıyla kimlik bilgilerini isteyerek tanımlar. Bir kullanıcı, yetkisiz bir kaynağa erişmeye çalıştığında, sonuç olarak, bunlar otomatik olarak kullanıcıların kimlik bilgilerini girebilecekleri oturum açma sayfasına yönlendirilirsiniz. Gönderilen kimlik bilgilerini, ardından özel bir kullanıcı deposu karşı - genellikle bir veritabanı doğrulanır.

Gönderilen kimlik doğruladıktan sonra bir *forms kimlik doğrulaması bileti* kullanıcı için oluşturulur. Bu biletin kullanıcının doğrulandığını ve kullanıcı adı gibi tanımlama bilgilerini içeren gösterir. Forms kimlik doğrulaması bileti (genellikle) istemci bilgisayarda bir tanımlama bilgisi olarak depolanır. Bu nedenle, bundan sonraki ziyaretlerinizde Web sitesine böylece kullanıcılar oturum açtıktan sonra kullanıcıyı tanımlamak web uygulaması etkinleştirme HTTP isteği, forms kimlik doğrulaması bileti içerir.

Şekil 2, üst düzey bir görüş noktasını forms kimlik doğrulaması akışından gösterilir. ASP.NET kimlik doğrulaması ve yetkilendirme parçaları iki ayrı varlıklar olarak nasıl davranmasını dikkat edin. Forms kimlik doğrulama sistemi kullanıcıyı tanımlayan (veya anonim raporları). Hangi kullanıcının istenen kaynağa erişim izni olup olmadığını belirler. yetkilendirme sistemidir. Varsa (Şekil 2'de anonim olarak ProtectedPage.aspx ziyaret etmeye çalışırken olduğu gibi) kullanıcı yetkilendirilmemiş, kullanıcı, forms kimlik doğrulama sistemi otomatik olarak kullanıcı oturum açma sayfasına yeniden yönlendirmesine neden engellendiğini yetkilendirme sistemi bildirir.

Kullanıcı başarıyla oturum açtıktan sonra forms kimlik doğrulaması bileti sonraki HTTP istekleri içerir. Forms kimlik doğrulama sistemi yalnızca kullanıcı tanımlayan - yetkilendirme sistem, kullanıcının istenen kaynak erişip erişemeyeceğini belirler.


![Form kimlik doğrulama iş akışı](security-basics-and-asp-net-support-vb/_static/image2.png)

**Şekil 2**: form kimlik doğrulama iş akışı


Biz sonraki iki öğreticilerde, form kimlik doğrulaması çok daha ayrıntılı olarak incelemek[form kimlik doğrulaması bir genel bakış](an-overview-of-forms-authentication-vb.md) ve [Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular](forms-authentication-configuration-and-advanced-topics-vb.md). ASP daha fazla bilgi için. NET kimlik doğrulama seçenekleri görmek [ASP.NET kimlik doğrulaması](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Web sayfaları, dizinler ve sayfa işlevselliği erişimi sınırlandırma

ASP.NET, belirli bir kullanıcı belirli dosya veya dizine erişmek için yetkili olup olmadığını belirlemek için iki yol içerir:

- **Yetkilendirme dosyası** - olduğundan erişim denetim listeleri (ACL'ler), web sunucusunun dosya sisteminde bu dosyalara erişimi bulunan dosyaları belirtilebilir gibi ASP.NET sayfaları ve web hizmetleri uygulanır. ACL'ler Windows hesapları için geçerli izinleri olduğundan dosya yetkilendirmesi, Windows kimlik doğrulaması ile en yaygın olarak kullanılır. Forms kimlik doğrulaması kullanırken, tüm işletim sistemi ve dosya sistemi düzeyinde istekleri sitesinden kullanıcı bağımsız olarak aynı Windows hesabı tarafından yürütülür.
- **URL yetkilendirmesi**-URL yetkilendirmesi ile sayfasında Geliştirici Web.config dosyasında yetkilendirme kuralları belirtir. Bu yetkilendirme kuralları, hangi kullanıcı veya rol erişmesine izin verilir veya belirli sayfaları veya dizinleri uygulama erişimi engellendi belirtin.

Dosya yetkilendirmesi ve URL yetkilendirmesi belirli bir dizindeki tüm ASP.NET sayfaları veya belirli bir ASP.NET sayfasına erişmek için yetkilendirme kuralları tanımlayın. Bu teknikler kullanarak belirli bir kullanıcı için belirli bir sayfa istekleri reddetme veya bir kullanıcı kümesine erişmesine izin vermek ve diğer herkes için erişimi engellemek için ASP.NET bildirebilirsiniz. Burada tüm kullanıcıların sayfayı erişebilir, ancak kullanıcı sayfanın işlevselliği bağlıdır senaryoları hakkında neler diyeceksiniz? Örneğin, kullanıcı hesaplarını destekleyen birçok siteler farklı içerik veya kimliği doğrulanmış kullanıcılar ve anonim kullanıcılar için veri görüntüleyen sayfalar sahip. Anonim kullanıcı kimliği doğrulanmış bir kullanıcı, bunun yerine geri Hoş Geldiniz, gibi bir ileti görür ancak siteye oturum açmak için bir bağlantı görebilirsiniz *kullanıcıadı* oturumu bağlantısını yanı sıra. Başka bir örnek: bir artırma sitedeki bir öğeyi görüntülerken bir fiyatı veren tedarikçiye veya bir öğe auctioning olmanıza bağlı olarak farklı bilgiler görürsünüz.

Bu sayfa düzeyi ayarlamaları bildirimli olarak veya programlama yoluyla gerçekleştirilebilir. Farklı içeriğini göstermek için kimliği doğrulanmış kullanıcılara, yeterlidir anonim bir [LoginView denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) sayfanıza ve uygun içeriğin kendi Anonymous ve LoggedInTemplate şablonlara girin. Alternatif olarak, program aracılığıyla olup geçerli istek kimliği doğrulanmış kullanıcının kim olduğunu ve hangi rollerin belirleyebilirsiniz (varsa) ait. Ardından göstermek veya bir kılavuz veya panel sayfasında sütunları gizlemek için bu bilgileri kullanabilirsiniz.

Bu seri, yetkilendirme odaklanan üç öğreticiler içerir. ***Kullanıcı tabanlı yetkilendirme***belirli kullanıcı hesapları için bir sayfa veya sayfalarda bir dizinde erişimi sınırlamak nasıl inceler. ***Rol tabanlı yetkilendirme*** rol yetkilendirme kurallarında sağlama sırasında düzeyi; son olarak, görünür ***görüntüleme içerik temel alan şu anda oturum açmış kullanıcı*** öğretici, belirli bir değiştirme açıklar sayfanın içerik ve İşlevler sayfasını ziyaret ederek kullanıcıya bağlı. ASP daha fazla bilgi için. NET yetkilendirme seçenekleri görmek [ASP.NET yetkilendirme](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Kullanıcı hesaplarını ve rolleri

ASP. NET form kimlik doğrulaması, kullanıcıların bir site için oturum açın ve sayfa ziyareti arasında anımsanacak kimliği doğrulanmış durumlarına sahip bir altyapı sağlar. Ve URL yetkilendirmesi, belirli dosyaları veya klasörleri bir ASP.NET uygulamasında erişimi sınırlamak için bir çerçeve sunar. Her iki özellik, ancak kullanıcı hesabı bilgileri depolamak veya rolleri yönetmek için bir yol sağlar.

ASP.NET 2.0 önce geliştiriciler kendi kullanıcı ve rol depoları oluşturmaktan sorumlu. Bunlar ayrıca kanca sayfaları hesabıyla ilgili oturum açma sayfası ve diğerlerinin yanı sıra yeni bir hesap oluşturmak için sayfanın gibi temel kullanıcı için kod yazma ve kullanıcı arabirimleri tasarlama için çalışıyor. ASP.NET'te, her geliştirici kendi tasarım kararları, aşağıdaki gibi sorulara gelmesi uygulama kullanıcı hesapları olan herhangi bir yerleşik kullanıcı hesabı çerçeveyi olmadan nasıl miyim parolalar ve diğer hassas bilgiler depolama? ve hangi yönergeleri miyim parola uzunluğu ve gücü ile ilgili dayatır?

Bugün, bir ASP.NET uygulamasında kullanıcı hesaplarını uygulama için çok daha kolay teşekkür olan *üyelik framework* ve oturum açma Web denetimleri yerleşik. Sınıflarda birkaç üyelik çerçevedir [System.Web.Security ad alanı](https://msdn.microsoft.com/library/system.web.security.aspx) önemli kullanıcı hesabı ile ilgili görevleri gerçekleştirmek için işlevsellik sağlayan. Anahtar sınıfı üyelik Framework [üyelik sınıfı](https://msdn.microsoft.com/library/system.web.security.membership.aspx), sahip olduğu gibi yöntemleri:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Üyelik framework kullanan [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), hangi düzgün bir şekilde ayıran üyelik framework'ün API kendi uygulamasından. Bu, geliştiricilerin ortak API kullanmasına olanak sağlar, ancak bunları özel uygulama gereksinimlerini karşılayan bir uygulamasını sağlar. Kısacası, üyelik sınıfı (yöntemler, özellikler ve olaylar) framework'ün temel işlevleri tanımlar, ancak gerçekte herhangi bir uygulama ayrıntılarını sağlamaz. Bunun yerine, bırakma işini gerçekleştiren ne olduğunu yapılandırılan sağlayıcı üyelik sınıfının yöntemlerini çağırır. Örneğin, üyelik sınıfı üyelik sınıfın CreateUser yöntemi çağrıldığında kullanıcı deposu ayrıntılarını bilmez. Kullanıcılar bir veritabanı veya bir XML dosyası veya başka bir deposunda işlenen, bilmez. Üyelik sınıfı temsilci çağrısı hangi sağlayıcısı belirlemek için web uygulamasının yapılandırmasını inceler ve bu sağlayıcı sınıfı uygun kullanıcı deposunda gerçekten yeni kullanıcı hesabı oluşturmak için sorumludur. Şekil 3'te, bu etkileşim gösterilmiştir.

Microsoft .NET Framework iki üyelik sağlayıcısı sınıfları birlikte gelir:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -Active Directory ve Active Directory uygulama modu (ADAM) sunucuları üyelik API'si uygular.
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -bir SQL Server veritabanında üyelik API'si uygular.

Bu öğretici serisinde, özel olarak SqlMembershipProvider üzerinde odaklanır.


[![Sağlayıcı modeli sağlayan farklı sorunsuz bir şekilde takılı içine Framework olmasını uygulamaları](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**Şekil 03**: sağlayıcı modeli sağlayan farklı sorunsuz bir şekilde takılı içine Framework olmasını uygulamaları ([tam boyutlu görüntüyü görmek için tıklatın](security-basics-and-asp-net-support-vb/_static/image5.png))


Sağlayıcı modelinin diğer uygulamaları Microsoft, üçüncü taraf satıcılar veya bireysel geliştiriciler tarafından geliştirilebileceği ve sorunsuz bir şekilde üyelik altyapısına takılı olduğunu avantajdır. Örneğin, Microsoft yayımladı [Microsoft Access veritabanları için üyelik sağlayıcısı](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Üyelik sağlayıcıları hakkında daha fazla bilgi için [sağlayıcı Araç Seti](https://msdn.microsoft.com/asp.net/aa336558.aspx), üyelik sağlayıcıları, örnek özel sağlayıcıları, sağlayıcı modeli belgelerinin 100'den fazla sayfa ilişkin bir kılavuz içerir ve Kaynak Kodu Yerleşik üyelik sağlayıcıları için (yani, ActiveDirectoryMembershipProvider ve SqlMembershipProvider) tamamlayın.

ASP.NET 2.0, rol framework de kullanıma sunulmuştur. Üyelik framework gibi rolleri framework sağlayıcı modeli yerleşik olarak bulunur. Kendi API aracılığıyla gösterilir [rolleri sınıfı](https://msdn.microsoft.com/library/system.web.security.roles.aspx) ve .NET Framework üç sağlayıcısı sınıfları ile birlikte gelir:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -Active Directory veya ADAM gibi bir Yetkilendirme Yöneticisi'ni ilke deposunda rol bilgilerini yönetir.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -bir SQL Server veritabanında rol uygular.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -rol bilgilerini ziyaretçi Windows grubuna göre ilişkilendirir. Bu yöntem, genellikle Windows kimlik doğrulaması ile kullanılır.

Bu öğretici serisinde, özel olarak SqlRoleProvider üzerinde odaklanır.

Sağlayıcı modeli tek bir ön yüzdeki API'si (üyelik ve roller sınıflar) içerdiğinden, uygulama ayrıntıları hakkında endişelenmenize gerek kalmadan bu API geçici olarak işlevselliğini oluşturulabilir mi - bu sayfa tarafından seçilen sağlayıcıları tarafından işlenir Geliştirici. Bu arabirim üyelik ve roller çerçevelerle Web denetimleri oluşturmak Microsoft ve üçüncü taraf satıcılar için bu birleşik API sağlar. ASP.NET, bir dizi ile birlikte gelen [oturum açma Web denetimleri](https://msdn.microsoft.com/library/ms178329.aspx) ortak kullanıcı hesabının kullanıcı arabirimlerini uygulamak için. Örneğin, [Login denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) bir kullanıcıdan kimlik bilgilerini, bunları doğrular ve bunları form kimlik doğrulaması günlüğe kaydeder. [LoginView denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) kimliği doğrulanmış kullanıcılar ve anonim kullanıcılara farklı biçimlendirme ya da kullanıcı rolüne bağlı olarak farklı biçimlendirme görüntüleme şablonları sunar. Ve [CreateUserWizard denetim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) yeni bir kullanıcı hesabı oluşturmak için adım adım kullanıcı arabirimi sağlar.

Aslında, üyelik ve roller çerçevelerle çeşitli oturum açma denetimleri etkileşim kurun. Çoğu oturum açma denetimleri, tek satırlık bir kod yazmak zorunda kalmadan uygulanabilir. Sonraki öğreticilerde, işlevleriyle özelleştirme ve genişletme yöntemleri dahil olmak üzere bu denetimleri daha ayrıntılı olarak inceleyeceğiz.

## <a name="summary"></a>Özet

Kullanıcı hesaplarını destekleyen tüm web uygulamaları gibi benzer özellikleri gerektirir: oturum açın ve kendi günlük sayfa ziyareti arasında; anımsanacak durumu sağlamak kullanıcıların bir web sayfası için bir hesap oluşturmak yeni ziyaretçiler ve hangi kaynak, veri ve işlevsellik hangi kullanıcılar ya da roller kullanılabilir olduğunu belirtmek için sayfa Geliştirici yeteneği. Kimlik doğrulama ve kullanıcıları yetkilendirme ve kullanıcı hesaplarını ve rolleri yönetme görevleri form kimlik doğrulaması, URL yetkilendirmesi ve üyelik ve roller çerçeveleri sayesinde ASP.NET uygulamalarında gerçekleştirmek son derece kolay.

Sonraki birkaç eğitim kursunda adım adım bir şekilde çalışan bir web uygulama sıfırdan oluşturmayı tarafından bu görünüşler inceleyeceğiz. Sonraki iki öğreticide biz ayrıntılı form kimlik doğrulaması keşfedin. Göreceğiz eylemi, forms kimlik doğrulama akışında forms kimlik doğrulaması bileti ayrıntılı olarak inceleyelim, güvenlik kaygıları tartışmanıza ve formları kimlik doğrulama sistemini yapılandırmak bkz - tüm web uygulaması oluştururken ziyaretçileri oturum açma ve oturum kapatmasını sağlar.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET 2.0 üyelik, roller, form kimlik doğrulaması ve güvenlik kaynakları](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [ASP.NET 2.0 güvenlik yönergeleri](https://msdn.microsoft.com/library/ms998258.aspx)
- [ASP.NET kimlik doğrulaması](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [ASP.NET yetkilendirmesi](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [ASP.NET oturum açma denetimleri genel bakış](https://msdn.microsoft.com/library/ms178329.aspx)
- [ASP.NET 2.0'ın İnceleme üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [I: üyelik ve rolleri kullanarak sitemin güvenliğini nasıl sağlayabilirim?](https://asp.net/learn/videos/video-45.aspx) (Video)
- [Üyelik giriş](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN güvenlik Geliştirici Merkezi](https://msdn.microsoft.com/security/default.aspx)
- [Profesyonel ASP.NET 2.0 güvenlik, üyelik ve rol yönetimi](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Sağlayıcı Araç Seti](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı Gözden Geçiren, Bu öğretici serisinin birçok yararlı Gözden Geçiren tarafından gözden geçirildi oluştu. Bu öğretici için müşteri adayı gözden geçirenler Alicja Maziarz ve John Suru Teresa Murphy içerir. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](forms-authentication-configuration-and-advanced-topics-cs.md)
> [İleri](an-overview-of-forms-authentication-vb.md)
