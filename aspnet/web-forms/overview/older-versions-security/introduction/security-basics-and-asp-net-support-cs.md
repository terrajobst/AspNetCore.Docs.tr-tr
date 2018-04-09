---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: Güvenlik temel kavramları ve ASP.NET Destek (C#) | Microsoft Docs
author: rick-anderson
description: Partic erişimi yetkilendirme ziyaretçileri web formu aracılığıyla kimlik doğrulaması için teknikleri inceleyeceksiniz eğitim serileri ilk öğreticide budur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2008
ms.topic: article
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b89a8b0dd88505c1d63054db508590c26684158
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="security-basics-and-aspnet-support-c"></a>Güvenlik temel kavramları ve ASP.NET Destek (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF indirin](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Ziyaretçilerin web form aracılığıyla kimlik doğrulaması, belirli sayfalara ve işlevlere erişimi yetkilendirme ve bir ASP.NET uygulamasında kullanıcı hesaplarını yönetme teknikleri inceleyeceksiniz eğitim serileri ilk öğreticide budur.


## <a name="introduction"></a>Giriş

Bir şey forumlar, e-ticaret siteleri, çevrimiçi e-posta Web siteleri, portal Web sitelerinin ve tüm ortak olan sosyal ağ sitelerine nedir? Tüm bunlar sunar *kullanıcı hesaplarını*. Kullanıcı hesapları teklif siteleri hizmetlerin sayısı sağlamanız gerekir. En az bir hesap oluşturmak yeni ziyaretçi gerekir ve döndürmeyi ziyaretçileri oturum açamaz olması gerekir. Bu tür web uygulamaları üzerinde oturum açmış olan kullanıcının dayalı kararlar: bazı sayfaları veya Eylemler kullanıcılar ya da belirli bir alt kümesiyle sınırlanıp; günlüğe yalnızca sınırlı olabilir Diğer sayfalar bilgi için oturum açan kullanıcının belirli gösterebilir veya hangi kullanıcının sayfayı görüntüleme bağlı olarak bilgi, daha az veya gösterebilir.

Ziyaretçilerin web form aracılığıyla kimlik doğrulaması, belirli sayfalara ve işlevlere erişimi yetkilendirme ve bir ASP.NET uygulamasında kullanıcı hesaplarını yönetme teknikleri inceleyeceksiniz eğitim serileri ilk öğreticide budur. Bu öğreticiler süresince inceleyeceğiz nasıl yapılır:

- Tanımlamak ve kullanıcılar bir Web sitesi için günlük
- ASP kullanın. Kullanıcı hesaplarını yönetmek için NET'in üyelik framework
- Oluşturma, güncelleştirme ve kullanıcı hesaplarını silme
- Bir web sayfası, dizin veya oturum açan kullanıcıyı temel alarak belirli işlevleri erişimini sınırlama
- ASP kullanın. Kullanıcı hesapları rolleriyle ilişkilendirmek üzere NET'in rolleri framework
- Kullanıcı rollerini yönetme
- Bir web sayfası, dizin veya oturum açmış kullanıcının rolüne göre belirli işlevler erişimini sınırlama
- Özelleştirme ve ASP genişletebilirsiniz. NET'in güvenlik Web denetimleri

Bu öğreticileri, kısa ve görsel olarak işleminde size kılavuzluk için yeterince ekran görüntüleri ile adım adım yönergeler sağlamak için sağlamıştır. Her öğretici C# ve Visual Basic sürümlerinde kullanılabilir ve kullanılan tam kod yüklenmesini içerir. (Bu ilk öğreticide üst düzey bir bakış açısı gelen güvenlik kavramları odaklanır ve tüm ilişkili kodu içermiyor.)

Bu öğreticide önemli güvenlik kavramları ve hangi tesis ASP.NET forms kimlik doğrulaması, yetkilendirme, kullanıcı hesapları ve rolleri uygulama yardımcı olmak için kullanılabilir olan aşağıdakiler ele alınacaktır. Haydi başlayalım!

> [!NOTE]
> Güvenlik bir önemli yayılan fiziksel, teknolojik herhangi bir uygulama yönüdür ve ilke kararları ve yüksek derecede planlama ve etki alanı bilgi gerektirir. Bu öğretici seri bir kılavuz olarak güvenli web uygulamaları geliştirmek için tasarlanmamıştır. Bunun yerine, özellikle form kimlik doğrulaması, yetkilendirme, kullanıcı hesapları ve roller üzerinde odaklanmıştır. Bu sorunları uzayda bazı güvenlik kavramları bu dizide açıklanan olsa da, diğerlerinin kalan Keşfedilmemiş alanları.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Kimlik doğrulama, yetkilendirme, kullanıcı hesapları ve rolleri

Kimlik doğrulama, yetkilendirme, kullanıcı hesapları ve rolleri web güvenlik bağlamında bu koşulları tanımlamak için hızlı bir dakikanızı ayırın yapmak istiyorsunuz, Bu öğretici seri çok sık kullanılacak dört terimler. Internet gibi bir istemci-sunucu modeli sunucunun isteği yapan istemcinin tanımlamak gereken birçok senaryo vardır. *Kimlik doğrulama* istemcinin kimliğini ascertaining işlemidir. Başarılı bir şekilde tanımlanan bir istemci olarak kabul edilir *kimliği doğrulanmış*. Tanımlanamayan bir istemci olarak kabul edilir *kimliği doğrulanmamış* veya *anonim*.

Güvenli kimlik doğrulama sistemleriyle ilgili aşağıdaki üç modelleri en az biri: [bir şey bildiğiniz bir şey olması ya da bir şey olduğunuz](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). Çoğu web uygulamaları, parola veya PIN gibi istemci bildiği bir şeyi bağlıdır. -Kullanıcı adı ve parola, örneğin her - bir kullanıcıyı tanımlamak için kullanılan bilgileri denir *kimlik bilgileri*. Bu öğretici seri odaklanır *form kimlik doğrulaması*, burada kullanıcılar oturum siteye bir web sayfası formunda kimlik bilgilerini sağlayarak bir kimlik doğrulama modeli olduğu. Biz tüm bu önce kimlik doğrulama türü karşılaşmıştır. Herhangi bir e-ticaret siteye gidin. Kullanıma hazır olduğunuzda, kullanıcı adı ve parola bir web sayfasında metin kutuları girerek oturum istenir.

İstemcileri tanımlamak ek olarak, hangi kaynaklara veya İşlevler isteği yapan istemcinin bağlı olarak erişilebilir sınırlamak bir sunucu gerekebilir. *Yetkilendirme* belirli bir kullanıcının belirli bir kaynak ya da işlevselliğe erişmek yetkisine sahip olup olmadığını belirleme işlemidir.

A *kullanıcı hesabı* belirli bir kullanıcının kalıcı öğrenmek için deposudur. Kullanıcı hesapları, en düşük düzeyde kullanıcının oturum açma adı ve parola gibi kullanıcı benzersiz olarak tanımlayan bilgiler içermesi gerekir. Bu önemli bilgiler yanı sıra kullanıcı hesapları gibi şeyler içerebilir: kullanıcının e-posta adresini; hesabın oluşturulduğu saat ve tarih; bunlar oturum açtığı son saat ve tarih; ad ve Soyadı; telefon numarası; ve posta adresi. Forms kimlik doğrulaması kullanırken, Microsoft SQL Server gibi ilişkisel bir veritabanındaki kullanıcı hesabı bilgileri genellikle depolanır.

Kullanıcı hesaplarını destekleyen web uygulamaları kullanıcılarına isteğe bağlı olarak grup *rolleri*. Yalnızca bir kullanıcıya uygulanan ve yetkilendirme kuralları ve sayfa düzeyinde işlevselliği tanımlamak için bir Özet sunan bir etiket rolüdür. Örneğin, bir Web sitesi, bir yönetici rolü herkesin belirli bir dizi web sayfası erişmek için yönetici ancak engelle yetkilendirme kuralları ile içerebilir. Ayrıca, tüm kullanıcıların (Yönetici olmayanların dahil) erişilebilen sayfaları çeşitli bir ek verileri görüntüleyebilir veya Yöneticiler roldeki kullanıcı tarafından ziyaret edildiğinde ek işlevler sunar. Rolleri kullanarak, size bir rol tarafından rol başına kullanıcı tarafından yerine bu yetkilendirme kurallarını tanımlayabilirsiniz.

## <a name="authenticating-users-in-an-aspnet-application"></a>Kullanıcıların bir ASP.NET uygulamasında kimlik doğrulaması

Bir kullanıcı bir URL, tarayıcının adres penceresi veya tıklama bir bağlantıyı girdiğinde, tarayıcı yapar bir [Köprü Metni Aktarım Protokolü (HTTP)](http://en.wikipedia.org/wiki/HTTP) ASP.NET olması, web sunucusu için belirtilen içerik isteği sayfası, görüntü, JavaScript Dosya veya başka bir içerik türü. Web sunucusu istenen içerik döndüren ile görevli. Bunu yaparken, kimin istekte ve kimlik istenen içeriği almak için yetkili olup olmadığı dahil olmak üzere istekle ilgili birkaç belirlemeniz gerekir.

Varsayılan olarak, tarayıcılar herhangi bir tür kimlik bilgileri eksik HTTP istekleri göndermek. Ancak tarayıcı kimlik doğrulama bilgilerini eklerseniz, web sunucusu isteği yapan istemcinin tanımlamaya çalışır kimlik doğrulama iş akışı başlatır. Web uygulaması tarafından kullanılan kimlik doğrulama türü kimlik doğrulama iş akışı adımları bağlıdır. ASP.NET, üç kimlik doğrulama türlerini destekler: Windows, Passport ve formlar. Bu öğretici seri form kimlik doğrulamasını odaklanır, ancak şimdi karşılaştırır ve Windows kimlik doğrulaması kullanıcı depoları ve iş akışı karşıtlık için bir dakikanızı ayırın.

### <a name="authentication-via-windows-authentication"></a>Windows kimlik doğrulaması üzerinden kimlik doğrulaması

Windows kimlik doğrulaması akışı aşağıdaki kimlik doğrulama teknikleri birini kullanır:

- Temel kimlik doğrulaması
- Özet kimlik doğrulaması
- Windows tümleşik kimlik doğrulaması

Tüm üç teknikleri kabaca aynı şekilde çalışır: bir yetkisiz açtığınızda anonim istek geldiğinde, devam etmek için yetkilendirme belirten bir HTTP yanıtının gereklidir web sunucusuna geri gönderir. Tarayıcı sonra kullanıcıdan kullanıcı adı ve parola (bkz: Şekil 1) için bir modal iletişim kutusu görüntüler. Bu bilgiler daha sonra bir HTTP üstbilgisi aracılığıyla web sunucusuna geri gönderilir.


![Modal bir iletişim kutusu, kullanıcı kendi kimlik bilgilerini ister.](security-basics-and-asp-net-support-cs/_static/image1.png)

**Şekil 1**: Modal bir iletişim kutusu, kullanıcı kendi kimlik bilgilerini ister.


Sağlanan kimlik bilgileri, web sunucusunun Windows kullanıcı depo doğrulanır. Başka bir deyişle, her kimliği doğrulanmış bir kullanıcı, web uygulamanızda kuruluşunuzdaki bir Windows hesabı olmalıdır. Sıradan bir hale intranet senaryolarda budur. Aslında, Windows tümleşik kimlik doğrulaması bir intranet ayarında kullanırken, tarayıcı otomatik olarak web sunucusu böylece Şekil 1'de gösterilen iletişim kutusunu gizleme ağa oturum açmak için kullanılan kimlik bilgileri sağlar. Windows kimlik doğrulaması için intranet uygulamalarını harika olsa da, sitenizde kaydolan her kullanıcı için Windows hesapları oluşturmak istiyor musunuz olduğundan Internet uygulamaları için genellikle unfeasible kalır.

### <a name="authentication-via-forms-authentication"></a>Form kimlik doğrulaması üzerinden kimlik doğrulaması

Form kimlik doğrulaması, diğer yandan, Internet web uygulamaları için idealdir. Form kimlik doğrulamasını geri çağırma bunları web form aracılığıyla kimlik bilgilerini girmesini isteyen kullanıcı tanımlar. Bir kullanıcı yetkisiz bir kaynağa erişmeyi denediğinde, sonuç olarak, bunlar otomatik olarak kendi kimlik bilgilerini girebilecekleri oturum açma sayfasına yeniden yönlendirilir. Gönderilen kimlik bilgileri, ardından özel kullanıcı depo - genellikle bir veritabanı doğrulanır.

Gönderilen kimlik bilgileri doğruladıktan sonra bir *forms kimlik doğrulaması bileti* kullanıcı için oluşturulur. Bu biletin kullanıcı kimliği ve kullanıcı adı gibi tanımlama bilgileri içerir gösterir. Forms kimlik doğrulaması bileti (genellikle) istemci bilgisayarda bir tanımlama bilgisi olarak depolanır. Bu nedenle, sonraki ziyaretlerinizde Web sitesine böylece bunlar oturum açtıktan sonra kullanıcıyı tanımlamak web uygulaması etkinleştiriliyor HTTP isteği, forms kimlik doğrulaması bileti ekleyin.

Şekil 2 bir üst düzey vantage noktasından form kimlik doğrulama iş akışı gösterilmektedir. ASP.NET kimlik doğrulaması ve yetkilendirme parça iki ayrı varlıklar olarak nasıl davranmasını dikkat edin. Forms kimlik doğrulaması sistem kullanıcıyı tanımlar (veya anonim raporları). Hangi kullanıcının istenen kaynak erişimi olup olmadığını belirler yetkilendirme sistemidir. Varsa (Şekil 2'de anonim olarak ProtectedPage.aspx ziyaret çalışılırken oldukları gibi) kullanıcı yetkisiz, kullanıcı, forms kimlik doğrulama sistemi otomatik olarak kullanıcı oturum açma sayfasına yeniden yönlendirmeye neden reddedildiğini yetkilendirme sistemi bildirir.

Kullanıcı başarıyla oturum açtıktan sonra sonraki HTTP istekleri forms kimlik doğrulaması bileti içerir. Forms kimlik doğrulaması sistem yalnızca kullanıcı tanımlayan - istenen kaynak kullanıcı erişip erişemeyeceğini belirler yetkilendirme sistem değildir.


![Form kimlik doğrulama iş akışı](security-basics-and-asp-net-support-cs/_static/image2.png)

**Şekil 2**: form kimlik doğrulama iş akışı


Biz sonraki iki öğreticileri çok daha ayrıntılı form kimlik doğrulamasını içine derinliklerine[form kimlik doğrulaması bir genel bakış](an-overview-of-forms-authentication-cs.md) ve [Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular](forms-authentication-configuration-and-advanced-topics-cs.md). ASP daha fazla bilgi için. NET'in kimlik doğrulama seçenekleri, bkz: [ASP.NET kimlik doğrulaması](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Web sayfaları, dizinler ve sayfa işlevselliği erişimi sınırlandırma

ASP.NET belirli bir kullanıcının belirli bir dosya veya dizin erişmek için yetkili olup olmadığını belirlemek için iki yöntem içerir:

- **Dosya Yetkilendirme** - erişim denetim listeleri (ACL) web sunucusunun dosya sisteminde, bu dosyalara erişimi bulunan dosyaları belirtilebilir gibi ASP.NET sayfaları ve web hizmetleri uygulanan beri. ACL'ler Windows hesaplarına uygulanan izinleri olduğundan dosya yetkilendirme Windows kimlik doğrulaması ile en yaygın olarak kullanılır. Forms kimlik doğrulaması kullanırken, tüm işletim sistemi ve dosya sistemi düzeyinde istekleri sitesini ziyaret kullanıcıya bakılmaksızın aynı Windows hesabına göre çalıştırılır.
- **URL yetkilendirmesi**-URL yetkilendirmesi ile sayfasında Geliştirici Web.config dosyasında yetkilendirme kurallarını belirtir. Bu yetkilendirme kuralları, hangi kullanıcılar ya da roller erişmesine izin verilir veya belirli sayfaları veya uygulama dizinlerde erişimi reddedildi belirtin.

Dosya yetkilendirme ve URL yetkilendirmesi yetkilendirme kuralları belirli bir ASP.NET sayfasının erişmek için veya tüm ASP.NET sayfaları için belirli bir dizinde tanımlayın. Bu teknikleri kullanarak belirli bir kullanıcı için belirli bir sayfa istekleri reddetme veya bir kullanıcı kümesini erişmesine izin vermek ve erişimini herkes için ASP.NET söyleyebilirsiniz. Burada tüm kullanıcıların sayfanın erişebilirsiniz, ancak kullanıcı sayfanın işlevselliği bağlıdır senaryoları hakkında neler? Örneğin, kullanıcı hesaplarını destekleyen birçok sitelerine farklı içerik veya anonim kullanıcılar karşı kimliği doğrulanmış kullanıcılar için veri görüntüleyen sayfalar gerekir. Kimliği doğrulanmış bir kullanıcı, bunun yerine ister, geri Hoş Geldiniz bir ileti görür ancak bir anonim kullanıcı sitede oturum açmak için bir bağlantı görebilirsiniz *kullanıcıadı* yanı sıra oturum kapatma için bir bağlantı. Başka bir örnek: bir artırma sitesinde bir öğe görüntülerken bir fiyatı veren tedarikçiye veya öğeyi auctioning bir olmanıza bağlı olarak farklı bilgiler bakın.

Bu tür sayfa düzeyi ayarlamaları bildirimli olarak veya program aracılığıyla gerçekleştirilebilir. Farklı içerik için göstermek için kimliği doğrulanmış kullanıcılar, yalnızca Sürükle daha anonim bir [LoginView denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) sayfanıza ve uygun içeriğin kendi Anonymous ve LoggedInTemplate şablonlara girin. Alternatif olarak, program aracılığıyla olup geçerli istek kimliği doğrulanmış kullanıcının kim olduğunu ve hangi rollerin belirleyebilirsiniz (varsa) ait. Ardından göstermek veya bir kılavuz veya paneller sütunlarında sayfasında gizlemek için bu bilgileri kullanın.

Bu seri yetkilendirme üzerinde odaklanmak üç öğreticiler içerir. ***Kullanıcı tabanlı bir yetkilendirme***belirli kullanıcı hesapları için; bir sayfa veya sayfaları bir dizinde erişimi sınırlamak nasıl inceler ***Rol tabanlı yetkilendirme*** rolü yetkilendirme kurallarında sağlama sırasında düzeyi; son olarak, görünen ***görüntüleme içerik bağlı olarak şu anda oturum açmış kullanıcı*** öğretici araştırır belirli bir değiştirme Sayfa içeriği ve sayfasını ziyaret kullanıcıyı temel alarak işlevselliği. ASP daha fazla bilgi için. NET'in yetkilendirme seçenekleri bkz [ASP.NET yetkilendirme](https://msdn.microsoft.com/library/wce3kxhd.aspx).


## <a name="user-accounts-and-roles"></a>Kullanıcı hesapları ve rolleri

ASP. NET'in form kimlik doğrulaması, kullanıcıların bir sitede oturum açmak ve sayfa ziyareti arasında hatırlanan kimliği doğrulanmış durumlarına sahip bir altyapı sağlar. Ve URL yetkilendirmesi belirli dosyaları veya klasörleri bir ASP.NET uygulamasında erişimi sınırlandırmak için bir çerçeve sunar. Her iki özellik ancak, kullanıcı hesabı bilgilerini depolamak veya rolleri yönetmek için bir yol sağlar.

ASP.NET 2.0 önce geliştiricilerin kendi kullanıcı ve rol depolarını oluşturmaktan sorumlu. Ayrıca kullanıcı arabirimleri tasarlama ve hesabıyla ilgili sayfaları oturum açma sayfasına ve diğerlerinin yanı sıra yeni bir hesap oluşturmak için sayfa gibi temel kullanıcı için kod yazma kanca üzerinde oldukları. ASP.NET, sorular gibi kendi tasarım kararları gelmesi uygulayan kullanıcı hesapları olan her geliştirici herhangi yerleşik kullanıcı hesabı framework olmadan nasıl ı parolalar ve diğer hassas bilgiler saklamayın? ve hangi yönergeleri ı parola uzunluğu ve gücü zorunlu tuttukları?

Bugün, bir ASP.NET uygulamasındaki kullanıcı hesapları uygulama için daha kolay teşekkür olan *üyelik framework* ve oturum açma Web denetimleri yerleşik. Sınıflarda sayıda üyelik çerçevedir [System.Web.Security ad alanı](https://msdn.microsoft.com/library/system.web.security.aspx) temel kullanıcı hesabıyla ilgili görevleri gerçekleştirmek için işlevselliği sağlayan. Anahtar sınıfı üyelik Framework'te [üyelik sınıfı](https://msdn.microsoft.com/library/system.web.security.membership.aspx), sahip olduğu gibi yöntemleri:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Üyelik çerçevesi kullanır [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), hangi düzgün bir şekilde ayıran üyelik framework'ün API kendi uygulamasından. Bu ortak bir API kullanmak için geliştiricilere olanak sağlar, ancak bunları kendi uygulamanın özel gereksinimleri karşılayan bir uygulamasını güçlendirir. Kısacası, üyelik sınıfı (yöntemler, özellikler ve olaylar) framework'ün temel işlevleri tanımlar, ancak gerçekte tüm uygulama ayrıntılarını sağlamaz. Bunun yerine, üyelik sınıfı yöntemlerinin ne asıl işi yapar olan yapılandırılmış sağlayıcı çağırır. Örneğin, üyelik sınıfı üyelik sınıfının CreateUser yöntemi çağrıldığında, kullanıcı deposunda ayrıntılarını bilmiyor. Kullanıcılar bir veritabanı veya bir XML dosyası veya başka bir deposunda bakım yapılmamaktadır varsa bilmiyor. Üyelik sınıfı çağrısı temsilci seçmek için hangi sağlayıcısı belirlemek için web uygulamasının yapılandırma inceler ve bu sağlayıcı sınıfı yeni kullanıcı hesabının uygun kullanıcı deposunda gerçekten oluşturulmasından sorumludur. Bu etkileşimi Şekil 3'te gösterilmiştir.

Microsoft .NET Framework iki üyelik sağlayıcısı sınıfları gelir:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -Active Directory ve Active Directory uygulama modu (ADAM) sunucuları üyelik API'sini kullanır.
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -bir SQL Server veritabanında üyelik API'sini kullanır.

Bu öğretici seri özel olarak SqlMembershipProvider odaklanır.


[![Sağlayıcı modeli sağlayan farklı sorunsuz bir şekilde takılı içine Framework olmasını uygulamaları&lt;/ güçlü&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Şekil 03**: sağlayıcısı modeli sağlayan farklı sorunsuz bir şekilde takılı içine Framework olmasını uygulamaları ([tam boyutlu görüntüyü görüntülemek için tıklatın](security-basics-and-asp-net-support-cs/_static/image5.png))


Sağlayıcı modelinin avantajı, alternatif uygulamaları kullanılabilir Microsoft, üçüncü taraf satıcılar veya her bir geliştirici tarafından geliştirilen ve sorunsuz bir şekilde üyelik Framework'e takılı olduğunu ' dir. Örneğin, Microsoft yayımladı [Microsoft Access veritabanları için üyelik sağlayıcısı](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Üyelik sağlayıcıları hakkında daha fazla bilgi için başvurmak [sağlayıcı Araç Seti](https://msdn.microsoft.com/asp.net/aa336558.aspx), üyelik sağlayıcıları, örnek özel sağlayıcıları, sağlayıcı modeline belgelerin 100'den sayfaları bir kılavuz içerir ve Kaynak Kodu Yerleşik üyelik sağlayıcıları için (yani, ActiveDirectoryMembershipProvider ve SqlMembershipProvider) tamamlayın.

ASP.NET 2.0 ayrıca rolleri framework sunulmuştur. Üyelik framework gibi rolleri framework sağlayıcı modeli üzerinde oluşturulmuştur. Kendi API aracılığıyla kullanıma sunulan [rolleri sınıfı](https://msdn.microsoft.com/library/system.web.security.roles.aspx) ve .NET Framework ile üç sağlayıcısı sınıfları gelir:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -Active Directory veya ADAM gibi bir Yetkilendirme Yöneticisi ilke deposunda rol bilgilerini yönetir.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -bir SQL Server veritabanında rol uygular.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -ziyaretçi Windows grubuna bağlı olarak rol bilgilerini ilişkilendirir. Bu yöntem, genellikle Windows kimlik doğrulaması ile kullanılır.

Bu öğretici seri özel olarak SqlRoleProvider odaklanır.

Tek bir öne bakan API (üyelik ve roller sınıflar) sağlayıcı modeli içerir olduğundan, uygulama ayrıntıları hakkında endişelenmeye gerek kalmadan bu API geçici işlevselliği oluşturmak mümkün müdür - bu sayfa tarafından seçilen sağlayıcıları tarafından işlenir Geliştirici. Microsoft ve üçüncü taraf satıcılar, üyelik ve roller çerçeveleri arabirimiyle Web denetimleri oluşturmak bu birleşik API sağlar. ASP.NET çeşitli ile birlikte gelen [oturum açma Web denetimleri](https://msdn.microsoft.com/library/ms178329.aspx) ortak kullanıcı hesabının kullanıcı arabirimleri uygulamak için. Örneğin, [oturum açma denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) bir kullanıcıdan kimlik bilgilerinin, bunları doğrular ve bunları form kimlik doğrulaması günlüğe kaydeder. [LoginView denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) kimliği doğrulanmış kullanıcılara karşı anonim kullanıcılar için farklı bir biçimlendirme ya da kullanıcı rolüne göre farklı bir biçimlendirme görüntüleme şablonları sunar. Ve [CreateUserWizard denetim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) yeni bir kullanıcı hesabı oluşturmak için adım adım kullanıcı arabirimi sağlar.

Kapak çeşitli oturum açma denetimleri üyelik ve roller çerçeveleri ile etkileşim. Tek satırlık bir kod yazmak zorunda kalmadan çoğu oturum açma denetimleri uygulanabilir. Sonraki öğreticileri, genişletme ve işlevleri özelleştirmek için teknikleri de dahil olmak üzere, bu denetimlerin daha ayrıntılı inceleyeceğiz.

## <a name="summary"></a>Özet

Kullanıcı hesaplarını destekleyen tüm web uygulamaları benzer özellikleri gibi gereken: oturum açma ve sayfa ziyareti; hatırlanan durum kendi günlük kullanıcılara Hesap oluşturmak yeni ziyaretçi için bir web sayfası; ve hangi kaynak, verileri ve işlevselliği hangi kullanıcılar ya da roller kullanılabilir olduğunu belirtmek için sayfa Geliştirici yeteneği. Kimlik doğrulaması ve Kullanıcıları yetkilendirmek ve kullanıcı hesapları ve rolleri yönetmeyle görevleri form kimlik doğrulaması, URL yetkilendirmesi ve üyelik ve roller çerçeveleri sayesinde ASP.NET uygulamalarında gerçekleştirmek son derece kolaydır.

Sonraki birkaç öğreticileri süresince adım adım bir biçimde sıfırdan çalışan bir web uygulama oluşturmayı tarafından bu yönlerinin inceleyeceğiz. Sonraki iki öğreticide biz form kimlik doğrulaması ayrıntılı inceleyeceksiniz. Göreceğiz eylem, forms kimlik doğrulama akışında forms kimlik doğrulaması bileti dissect, güvenlik sorunlarının ele almaktadır ve forms kimlik doğrulaması sistemi yapılandırmak bkz - tüm web uygulaması oluştururken oturum açma ve oturum kapatma ziyaretçileri sağlar.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET 2.0 üyelik, roller, form kimlik doğrulaması ve güvenlik kaynakları](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [ASP.NET 2.0 güvenlik yönergeleri](https://msdn.microsoft.com/library/ms998258.aspx)
- [ASP.NET kimlik doğrulaması](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [ASP.NET yetkilendirmesi](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [ASP.NET oturum açma denetimleri genel bakış](https://msdn.microsoft.com/library/ms178329.aspx)
- [ASP.NET 2.0'ın inceleniyor üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [I: üyelik ve roller kullanarak Sitem güvenliğini nasıl sağlayabilirim?](https://asp.net/learn/videos/video-45.aspx) (Video)
- [Üyelik giriş](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN güvenlik Geliştirici Merkezi](https://msdn.microsoft.com/security/default.aspx)
- [Profesyonel ASP.NET 2.0 güvenlik, üyelik ve rol yönetimi](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Sağlayıcı Araç Seti](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme serisi pek çok yararlı gözden geçirenler tarafından gözden Bu öğretici oluştu. Bu öğretici için sağlama gözden geçirenler Alicja Maziarz, John Suru ve Teresa Murphy içerir. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](an-overview-of-forms-authentication-cs.md)
