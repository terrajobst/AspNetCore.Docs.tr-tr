---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
title: Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular (VB) | Microsoft Docs
author: rick-anderson
description: Bu öğreticide biz çeşitli forms kimlik doğrulaması ayarlarını incelemek ve form öğesi aracılığıyla değiştirme konusuna bakın. Bu bir ayrıntılı oluşturulmasını gerektirir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 829d2f56-5c48-445b-b826-3418a450c788
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
msc.type: authoredcontent
ms.openlocfilehash: c6ef046100cf4773da57f6693a88e9bc6ec1790f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="forms-authentication-configuration-and-advanced-topics-vb"></a>Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_vb.pdf)

> Bu öğreticide biz çeşitli forms kimlik doğrulaması ayarlarını incelemek ve form öğesi aracılığıyla değiştirme konusuna bakın. Forms kimlik doğrulaması bileti 's zaman aşımı değeri, bir oturum açma sayfası kullanarak özel bir URL (örneğin, SignIn.aspx Login.aspx yerine) ve cookieless form kimlik doğrulama biletlerini özelleştirme ayrıntılı bir bakış bu oluşturulmasını gerektirir.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](an-overview-of-forms-authentication-vb.md) bir günlük görüntüleme için sayfasında farklı oluşturmak için Web.config dosyasında yapılandırma ayarlarını belirtme gelen bir ASP.NET uygulamasında form kimlik doğrulaması gerçekleştirmek için gerekli adımları inceledik İçerik kimliği doğrulanmış ve anonim kullanıcılar için. Web sitesi modu özniteliğini ayarlayarak forms kimlik doğrulaması kullanacak şekilde yapılandırılmış olduğunu geri çağırma &lt;kimlik doğrulaması&gt; formlara öğesi. &lt;Kimlik doğrulaması&gt; öğesi isteğe bağlı olarak içerebilir bir &lt;forms&gt; alt öğe üzerinden bir form kimlik doğrulama ayarlarını çeşitli belirtilebilir.

Bu öğreticide biz çeşitli forms kimlik doğrulaması ayarlarını incelemek ve bunları aracılığıyla değiştirme konusuna bakın &lt;forms&gt; öğesi. Forms kimlik doğrulaması bileti 's zaman aşımı değeri, bir oturum açma sayfası kullanarak özel bir URL (örneğin, SignIn.aspx Login.aspx yerine) ve cookieless form kimlik doğrulama biletlerini özelleştirme ayrıntılı bir bakış bu oluşturulmasını gerektirir. Biz ayrıca forms kimlik doğrulaması bileti yapısıyla daha yakından inceleyin ve bilet ait verileri denetleme ve izinsiz güvenli olduğundan emin olmak için ASP.NET alır önlemleri bakın. Son olarak, forms kimlik doğrulaması bileti ek kullanıcı verilerini depolamak ve bu verilerine özel bir asıl nesne modeli ele alacağız.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>1. adım: İnceleniyor &lt;forms&gt; yapılandırma ayarları

ASP.NET formları kimlik doğrulama sisteminde bir uygulama tarafından uygulama temelinde özelleştirilebilir yapılandırma ayarları sayısı sunar. Bu gibi ayarları içerir: form kimlik doğrulaması ömrü bilet; ne tür bir koruma bilet uygulanır; altında hangi koşullar cookieless kimlik doğrulama biletlerini kullanılır; oturum açma sayfasının yolu; ve diğer bilgileri. Varsayılan değerleri değiştirmek için add bir [ &lt;forms&gt; öğesi](https://msdn.microsoft.com/library/1d3t3c61.aspx) bir alt öğesi olarak [ &lt;kimlik doğrulaması&gt; öğesi](https://msdn.microsoft.com/library/532aee0e.aspx), bu özellik belirtme XML özniteliği özelleştirmek istediğiniz değerleri bunu ister:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample1.xml)]

Tablo 1 aracılığıyla özelleştirilmiş özellikleri özetler &lt;forms&gt; öğesi. Web.config bir XML dosyası olduğundan, sol sütunda öznitelik adları büyük küçük harfe duyarlıdır.


| <strong>Özniteliği</strong> |                                                                                                                                                                                                                                     <strong>Açıklama</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         cookieless         |                                                                                                                Bu öznitelik, hangi koşullarda URL'de katıştırılmış karşı bir tanımlama bilgisi kimlik doğrulaması bileti depolanır belirtir. İzin verilen değerler: UseCookies; UseUri; Otomatik Algıla; ve UseDeviceProfile (varsayılan). 2. adım, bu ayar daha ayrıntılı inceler.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Kullanıcılar sorgu dizesinde belirtilen Redirecturl'yi değer yoksa oturum açma sayfasından oturum açtıktan sonra yeniden yönlendirileceği URL'yi belirtir. Default.aspx varsayılan değerdir.                                                                                                                                                         |
|           etki alanı           | Tanımlama bilgisi tabanlı kimlik doğrulama biletlerini kullanırken, bu ayar tanımlama bilgisi s etki alanı değerini belirtir. Varsayılan değer tarayıcının içinden (www.yourdomain.com gibi) verilmiş etki alanını kullanmak boş bir dizedir. Bu durumda, tanımlama bilgisi olur <strong>değil</strong> yapma admin.yourdomain.com gibi alt etki alanları için istediğinde gönderilmeyecek. Tüm alt etki alanları için geçirilecek tanımlama bilgisi istiyorsanız justanotherxiodec11.BLOB.Core.Windows.NET ayarı etki alanı özniteliği özelleştirmek gerekir. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Kimliği doğrulanmış kullanıcılar aynı sunucuda diğer web uygulamalarında URL'lere yeniden yönlendirilen zaman hatırlanan gösteren bir Boole değeri. Varsayılan olarak yanlıştır.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      Oturum açma sayfası URL'si. Varsayılan değer, Login.aspx'tir.                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   Tanımlama bilgisi tabanlı kimlik doğrulama biletlerini, tanımlama bilgisinin adını kullanırken. Varsayılandır. ASPXAUTH.                                                                                                                                                                                                   |
|            yol            |                                                                             Tanımlama bilgisi tabanlı kimlik doğrulama biletlerini kullanırken, bu ayar tanımlama bilgisi s yol özniteliği belirtir. Yol özniteliği bir tanımlama bilgisi belirli dizin hiyerarşiye kapsamını sınırlandırmak bir geliştirici sağlar. Varsayılan değer / hangi kimlik doğrulama bileti tanımlama etki alanında yapılan herhangi bir istek göndermek için tarayıcı bildirir.                                                                              |
|         koruma         |                                                                                                                                            Forms kimlik doğrulaması bileti korumak için hangi teknikleri kullanıldığını belirtir. İzin verilen değerler: tüm (varsayılan); Şifreleme; Yok; ve doğrulama. Bu ayarlar, adım 3'te ayrıntılı ele alınmıştır.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Kimlik doğrulama tanımlama bilgisini iletmek için bir SSL bağlantısının gerekli olup olmadığını gösteren bir Boole değeri. Varsayılan değer false'tur.                                                                                                                                                                                |
|     SlidingExpiration değeri      |                                                                                                 Kullanıcı kimlik doğrulama tanımlama bilgisi s zaman aşımı her erişimde sıfırlanacağını olup olmadığını tek bir oturum sırasındaki siteyi ziyaret gösteren bir Boole değeri. Varsayılan değer true olur. Kimlik doğrulama bileti zaman aşımı ilkesini belirtme bölümlerinde daha ayrıntılı ele alınmıştır s zaman aşımı değeri bölüm anahtarı.                                                                                                 |
|          Zaman aşımı           |                                                                                                                               Saat geçtikten sonra kimlik doğrulama bileti tanımlama bilgisinin süresinin dakika cinsinden belirtir. Varsayılan değer 30'dur. Kimlik doğrulama bileti zaman aşımı ilkesini belirtme bölümlerinde daha ayrıntılı ele alınmıştır s zaman aşımı değeri bölüm anahtarı.                                                                                                                               |

**Tablo 1**: bir özetini &lt;forms&gt; öğenin öznitelikleri

ASP.NET 2.0 ve sonrasındaki, varsayılan formlar kimlik doğrulaması .NET Framework FormsAuthenticationConfiguration sınıfında sabit kodlanmış değerlerdir. Web.config dosyasındaki bir uygulama tarafından uygulama temelinde değişiklikleri uygulanmış olması gerekir. Bu ASP.NET tarafından farklıdır 1.x burada varsayılan formlar kimlik doğrulaması değerleri machine.config dosyasında saklanıyordu (ve bu nedenle machine.config düzenleme aracılığıyla değiştirilmiş). ASP.NET konu üzerinde while 1.x, bunu belirtmeyi faydalı forms kimlik doğrulaması sistem ayarlarını sayısı ASP.NET 2. 0 ' farklı bir varsayılan değerlere sahip ve ASP.NET daha ötesine 1.x. Bir ASP.NET 1.x ortamından uygulamanızın geçiriyorsanız, bu farkların bilincinde olmak önemlidir. Başvurun [ &lt;forms&gt; öğesi teknik belgeler](https://msdn.microsoft.com/library/1d3t3c61.aspx) farklar listesi.

> [!NOTE]
> Zaman aşımı, etki alanı ve yol gibi birkaç forms kimlik doğrulaması ayarlarını elde edilen forms kimlik doğrulaması bileti tanımlama bilgisi için ayrıntıları belirtin. Tanımlama bilgileri, nasıl çalıştığını ve çeşitli özellikleri hakkında daha fazla bilgi için okuma [bu tanımlama bilgileri öğretici](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Bilet 's zaman aşımı değeri belirterek

Forms kimlik doğrulaması bileti Kimlikteki temsil eden bir belirteçtir. Tanımlama bilgisi tabanlı kimlik doğrulama biletlerini ile bu belirteç bir tanımlama bilgisi biçiminde tutulur ve her isteğin web sunucusuna gönderilen. Esas olarak, bir belirteç elinde bildirir, ben *kullanıcıadı*, t zaten oturum açtıysanız ve böylece bir kullanıcının kimliğini sayfa ziyareti arasında anımsanabileceğini kullanılır.

Forms kimlik doğrulaması bileti yalnızca kullanıcının kimliğini içerir, ancak ayrıca bütünlük ve belirteç güvenliğini sağlamaya yardımcı olmak için bilgiler içerir. Sonuçta, biz sahte bir belirteç oluşturmak veya bir LEGIT belirteci bazı underhanded şekilde değiştirmek için kullanabilmek için alınan kullanıcı istemezsiniz.

Bu tür bir bit bilet dahil bilgilerin bir *süre sonu*, tarih ve saat fırsattır artık geçerli değil. Bir kimlik doğrulaması bileti FormsAuthenticationModule inceler her zaman anahtar kişinin süre sonu olmayan henüz geçti sağlar. Varsa, anahtar yoksayar ve kullanıcının anonim olarak tanımlar. Bu korumayı yeniden yürütme saldırılarına karşı korumaya yardımcı olur. Bir bilgisayar korsanının ellerini kullanıcının geçerli kimlik doğrulaması biletinin üzerinde-kendi bilgisayara fiziksel erişim sağlamasını ve tanımlama bilgilerine kök dizini değiştirme belki de alabilirsiniz. - Bu çalınan kimlik doğrulaması bileti sunucusuna bir istek gönderebilir, geçerlilik süresi, olmadan ve Giriş elde edilir. Süre sonu bu senaryo engellemez, ancak hangi sırasında bu tür bir saldırının başarılı olabilmesi için pencere sınırlayın.

> [!NOTE]
> Forms kimlik doğrulaması sistem tarafından kimlik doğrulaması bileti korumak için kullanılan 3. adım ayrıntıları ek teknikler.


Kimlik doğrulaması bileti oluştururken, forms kimlik doğrulaması sistem zaman aşımı ayarını bakarak, sona erme belirler. Tablo 1 ' 30 dakika, varsayılan ayarı zaman aşımı belirtildiği gibi forms kimlik doğrulaması bileti oluşturulduğunda, sona erme bir tarih ve saat 30 dakika sonra ayarlanır anlamına gelir.

Süre sonu mutlak süreye form kimlik doğrulama anahtarının süresi dolduğunda bir gelecekte tanımlar. Ancak, geliştiriciler genellikle bir kayan bitiş, bir kullanıcının site revisits her zaman sıfırlanır uygulamak istiyor. Bu davranış slidingExpiration ayarları tarafından belirlenir. FormsAuthenticationModule bir kullanıcı kimlik doğrulaması her zaman (varsayılan) true olarak ayarlanırsa, anahtar kişinin süre sonu güncelleştirir. Süre sonu false olarak ayarlanırsa, her istekte güncelleştirilmezse, böylelikle tam olarak zaman aşımı zaman bilet ilk geçen dakika sayısı süresi dolacak şekilde bilet yol oluşturulur.

> [!NOTE]
> Kimlik doğrulaması bileti depolanan süre sonu mutlak tarih ve saat değeri 2 Ağustos 2008 11:34 AM gibi ' dir. Ayrıca, tarih ve saat web sunucusunun yerel saate göre var. Bu tasarım kararına geçici gün ışığından yararlanma saati (Amerika Birleşik Devletleri'nde saatler şimdi bir (web sunucusu Yaz Saati burada gözlenir yerel ayarda barındırılan varsayılarak) saat taşındığında olan DST), ilginç bazı yan etkileri olabilir. Bir 30 dakikalık süresi dolmak üzere DST başladığı saati ile ASP.NET Web sitesi için ne olacağını düşünün (2: 00'da olmayan). Bir ziyaretçi siteye 11 Mart 2008'de 1: 55'da oturum açtığı düşünün. Bu 2:25 AM (gelecekte 30 dakika) 11 Mart 2008 süresi bir form kimlik doğrulama anahtarının oluşturur. Ancak, geçici bir çözüm 2: 00'da yapar sonra saat 03: 00'da için DST nedeniyle atlar. Kullanıcı yeni bir sayfa (3:01 AM) oturum açtıktan sonra altı dakika yüklediğinde FormsAuthenticationModule biletin süresi doldu ve kullanıcı oturum açma sayfasına yönlendirir notlar. Bu ve diğer kimlik doğrulama bileti zaman aşımı sorunları saklamayı yanı geçici çözümler hakkında daha kapsamlı bir açıklama için Stefan Schackow'in bir kopyasını çekme *Professional ASP.NET 2.0 güvenlik, üyelik ve rol yönetimi* (ISBN: 978-0-7645-9698-8).


Şekil 1 SlidingExpiration değeri false olarak ayarlanır ve zaman aşımı 30 zaman iş akışı gösterilmiştir. Oturum açma sırasında oluşturulan kimlik doğrulaması bileti sona erme tarihini içerir ve bu değeri sonraki isteklerde güncelleştirilmemiş unutmayın. FormsAuthenticationModule biletin süresi doldu bulursa, onu atar ve isteğin anonim olarak değerlendirir.


[![Forms kimlik doğrulaması bileti'nın süre sonu zaman slidingExpiration grafik gösterimi yanlış](forms-authentication-configuration-and-advanced-topics-vb/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image1.png)

**Şekil 01**: Forms kimlik doğrulaması bileti'nın süre sonu zaman slidingExpiration grafik gösterimi A değer false ([tam boyutlu görüntüyü görüntülemek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image3.png))


Şekil 2'SlidingExpiration değeri ayarlandığında, iş akışı gösterilmiştir true ve zaman aşımı 30 ayarlanır. Kimliği doğrulanmış bir isteği (süresi olmayan bilet ile) alındığında, sona erme gelecekte dakika zaman aşımı sayıya güncelleştirilir.


[![Forms kimlik doğrulaması bileti 's grafik gösterimi slidingExpiration olduğunda true](forms-authentication-configuration-and-advanced-topics-vb/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image4.png)

**Şekil 02**: Forms kimlik doğrulaması bileti ait bir grafik gösterimi slidingExpiration olduğunda true ([tam boyutlu görüntüyü görüntülemek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image6.png))


Tanımlama bilgisi tabanlı kimlik doğrulama biletlerini (varsayılan) kullanırken, bu tartışma tanımlama bilgilerini ayrıca belirtilen kendi expiries olabileceği için biraz daha karmaşık haline gelir. Tanımlama bilgisi yok edilmesi durumlarda bir tanımlama bilgisinin süre sonu (veya bunların olmaması) tarayıcıyı yönlendirir. Tanımlama bilgisi geçerlilik süresi eksikse, tarayıcı kapatıldığında yok edilir. Geçerlilik süresi varsa, ancak, tanımlama bilgisi tarihe kadar kullanıcının bilgisayarında saklanır ve süre sonu içinde belirtilen süre geçtikten. Bir tanımlama bilgisi tarayıcı tarafından kaldırıldığı zaman artık web sunucusuna gönderilir. Bu nedenle, site dışında günlüğü kullanıcı için bir tanımlama bilgisinin yok etme benzerdir.

> [!NOTE]
> Elbette, bir kullanıcı önceden bilgisayarlarında depolanan tüm tanımlama bilgilerini kaldırabilir. Internet Explorer 7'de, Araçlar, seçenekleri, gidin ve gözatma geçmişini bölümünde Sil düğmesini. Buradan, Sil tanımlama bilgilerini düğmesini tıklatın.


Forms kimlik doğrulaması sistem oturumu veya sona erme tabanlı tanımlama bilgisi için geçirilen değerine bağlı olarak oluşturur *persistCookie* parametresi. İki giriş parametrelerinde FormsAuthentication sınıfının GetAuthCookie, SetAuthCookie ve RedirectFromLoginPage yöntemlerini ele geri çağırma: *kullanıcıadı* ve *persistCookie*. Önceki öğreticide oluşturduğumuz oturum açma sayfasına kalıcı bir tanımlama bilgisi oluşturulup oluşturulmadığını belirlenen bir Beni anımsa onay kutusunu dahil. Süre sonu tabanlı kalıcı tanımlama bilgileri; kalıcı olmayan tanımlama bilgileri, oturum tabanlı.

Zaten açıklanan zaman aşımı ve slidingExpiration kavramları aynı hem oturum ve süre sonu göre tanımlama bilgilerini uygulayın. Yalnızca bir alt fark yürütmesinde: yarısı belirtilen süre geçtikten birden fazla süre sonu tabanlı tanımlama bilgisi slidingTimeout özelliği true olarak ayarlanmış kullanırken, tanımlama bilgisinin süre sonu yalnızca güncelleştirildiğinde.

Şimdi, zaman aşımı kayan zaman aşımı değeri kullanarak bir saat (60 dakika) sonra anahtarları şekilde bizim Web sitesinin kimlik doğrulama bileti zaman aşımı ilkeleri güncelleştirin. Bu değişiklik efekt, Web.config dosyasını güncelleştirmek için ekleme bir &lt;forms&gt; öğesine &lt;kimlik doğrulaması&gt; aşağıdaki biçimlendirme sahip öğe:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Bir oturum açma sayfası URL'si Login.aspx dışında kullanma

FormsAuthenticationModule, yetkisiz kullanıcıların otomatik olarak oturum açma sayfasına yeniden yönlendirir. olduğundan, oturum açma sayfasının URL'sini bilmesi gerekir. Bu URL loginUrl özniteliği tarafından belirtilen &lt;forms&gt; öğesi ve login.aspx varsayılan değeri. Mevcut bir Web bağlantı noktası oluşturma, farklı bir URL, zaten işareti ve arama motorları tarafından dizinli bir bir oturum açma sayfası zaten sahip olabilir. Varolan oturum açma sayfanız için login.aspx yeniden adlandırma ve son bağlantılar ve kullanıcıların yer işaretleri yerine, bunun yerine oturum açma sayfanız işaret edecek şekilde loginUrl özniteliği değiştirebilirsiniz.

Örneğin, ~/Users/SignIn.aspx loginUrl yapılandırma ayarına işaret oturum açma sayfanız SignIn.aspx idi ve kullanıcıların dizininde bulunan, şu şekilde:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample3.xml)]

Geçerli uygulamamız zaten Login.aspx adlı bir oturum açma sayfası olduğundan, özel bir değer belirtmek için gerek yoktur &lt;forms&gt; öğesi.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>2. adım: Cookieless form kimlik doğrulama biletlerini kullanma

Varsayılan olarak form kimlik doğrulama sistemi tanımlama bilgileri koleksiyonu kendi kimlik doğrulama biletlerini depolamak veya bunlara sitesini ziyaret kullanıcı aracısı temel URL katıştırmak belirler. Tüm temel masaüstü tarayıcıları Internet Explorer, Firefox, Opera ve Safari, destek tanımlama bilgileri gibi çalışır, ancak tüm mobil aygıtları yapın.

Forms kimlik doğrulaması sistem tarafından kullanılan tanımlama bilgisi ilkesi cookieless ayarına bağlıdır &lt;forms&gt; dört değerlerden biri atanabilir öğe:

- UseCookies - belirtir tanımlama bilgisi tabanlı kimlik doğrulama biletlerini her zaman kullanılacaktır.
- UseUri - gösterir tanımlama bilgisi tabanlı kimlik doğrulama biletlerini hiçbir zaman kullanılmayacaktır.
- Cihaz profili tanımlama bilgisi, tanımlama bilgisi tabanlı kimlik doğrulama biletlerini desteklemiyorsa otomatik algıla - kullanılmaz; cihaz profili tanımlama bilgilerini destekliyorsa, bir araştırma mekanizması tanımlama bilgilerinin etkin olup olmadığını belirlemek için kullanılır.
- UseDeviceProfile - varsayılan; cihaz profili tanımlama bilgilerini destekliyorsa tanımlama bilgisi tabanlı kimlik doğrulama biletlerini kullanır. Araştırma mekanizması kullanılır.

Otomatik Algıla ve UseDeviceProfile ayarlarını dayanan bir *aygıt profili* tanımlama bilgisi tabanlı veya cookieless kimlik doğrulama biletlerini kullanıp kullanmayacağınızı ascertaining içinde. ASP.NET bir veritabanı çeşitli aygıtlar ve tanımlama bilgileri destekledikleri, hangi sürümü destekledikleri JavaScript vb. gibi yeteneklerini tutar. Her bir aygıtı ister bir web sayfası gönderir boyunca bir web sunucusundan bir *kullanıcı aracısı* aygıt türünü tanımlayan bir HTTP üstbilgisi. ASP.NET, sağlanan kullanıcı aracısı dizesi otomatik olarak kendi veritabanında belirtilen karşılık gelen bir profili ile eşleşir.

> [!NOTE]
> Bu veritabanı cihaz yeteneklerini uygun bir XML dosya sayısı depolanan [tarayıcı tanım dosyası şeması](https://msdn.microsoft.com/library/ms228122.aspx). Varsayılan cihaz profili dosyalarını % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers bulunur. Uygulamanızın uygulamaya özel dosyaları da ekleyebilirsiniz\_tarayıcılar klasör. Daha fazla bilgi için bkz: [nasıl yapılır: algılamak tarayıcı türleri, ASP.NET Web sayfaları](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Varsayılan ayar UseDeviceProfile olduğundan, site, profili tanımlama bilgilerini desteklemiyor raporları bir cihaz tarafından ziyaret edildiğinde cookieless form kimlik doğrulama biletlerini kullanılır.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>URL'de kimlik doğrulaması bileti kodlama

Her istekte ziyaret aygıt bunları destekliyorsa varsayılan formlar kimlik doğrulama ayarlarını tanımlama bilgilerini kullanmasına neden olan belirli bir Web tarayıcısından bilgi dahil etmek için doğal bir orta tanımlama bilgileridir. Tanımlama bilgilerini desteklenmiyorsa, kimlik doğrulaması bileti istemciden sunucuya geçirme için alternatif bir yol uygulanmalıdır. URL'deki tanımlama bilgisi verileri kodlamak için geçici cookieless ortamlarda kullanılan ortak bir çözüm değildir.

URL içinde gibi bilgileri nasıl katıştırılabilen görmek için en iyi cookieless kimlik doğrulama biletlerini kullanmak için site zorlamak için yoludur. Bu işlem için UseUri cookieless yapılandırma ayarı ayarlayarak gerçekleştirilebilir:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample4.xml)]

Bu değişikliği yaptıktan sonra bir tarayıcı aracılığıyla sitesini ziyaret edin. Tam olarak menülerin önce olduğu gibi anonim kullanıcı olarak ziyaret ederken URL'leri arar. Örneğin, Default.aspx sayfasını ziyaret eden my tarayıcının adres çubuğunda aşağıdaki URL'yi gösterir:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Ancak, URL'de oturum açtıktan sonra forms kimlik doğrulaması bileti katıştırılır. Örneğin, oturum açma sayfasını ziyaret ve Sam oturum açtıktan sonra Default.aspx sayfasına döndürülen ancak bu kez URL'dir:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Forms kimlik doğrulaması bileti URL içinde katıştırılmış. The string (F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2) represents the hex-encoded authentication ticket information, and is the same data that is usually stored within a cookie.

Cookieless kimlik doğrulama biletlerini çalışmak sırasıyla sistem kimlik doğrulaması bilet verileri içerecek şekilde tüm URL'leri sayfasında kodlamak gerekir, kullanıcı bir bağlantıyı tıklattığında Aksi takdirde kimlik doğrulaması bileti kaybolacaktır. Thankfully, bu katıştırma mantığı otomatik olarak gerçekleştirilir. Bu işlevselliğini göstermek için Default.aspx sayfasını açın ve bir köprü denetim Bağlantıyı Sına ve SomePage.aspx, metin ve NavigateUrl özelliklerini sırasıyla ayarı ekleyin. Gerçekten olmadığından bir sayfa Projemizin SomePage.aspx adlı önemli değildir.

Değişiklikleri kaydetmek için Default.aspx ve bir tarayıcı ziyaret edin. Böylece URL'de forms kimlik doğrulaması bileti katıştırılmış siteye oturum açın. Ardından, Default.aspx Bağlantıyı Sına bağlantısına tıklayın. Ne oldu? SomePage.aspx adlı sayfa varsa, 404 hatası oluştu, ancak burada önemli olan değil. Bunun yerine, tarayıcınızın adres çubuğuna odaklanılmaktadır. Bu form kimlik doğrulama anahtarının URL'de içerdiğini unutmayın!

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

Bağlantıda URL SomePage.aspx otomatik olarak kimlik doğrulaması bileti - dahil bir URL'ye dönüştürüldü biz lick kod yazmak zorunda oldu! Form kimlik doğrulaması bileti http:// ile başlayan değil köprüler URL'sini otomatik olarak katıştırılır veya /. Köprü Response.Redirect yapılan bir çağrı, bir köprü denetim veya bağlı HTML bağlayıcı öğesi görünürse önemli değildir (yani, &lt;bir href = "..."&gt;... &lt;/a&gt;). URL şöyle olmadığı sürece http://www.someserver.com/SomePage.aspx veya /SomePage.aspx, forms kimlik doğrulaması bileti katıştırılmış bize.

> [!NOTE]
> Cookieless form kimlik doğrulama biletlerini tanımlama bilgisi tabanlı kimlik doğrulama biletlerini olarak aynı zaman aşımı ilkelerine uyması. Ancak, tanımlama bilgisi olmayan kimlik doğrulama biletlerini doğrudan URL'de kimlik doğrulaması bileti ekli olduğundan yeniden yürütme saldırılarını daha fazladır. Bir Web sitesini ziyaret eder, oturum açtığında ve bir iş arkadaşınıza e-posta içinde URL gönderebilir bir kullanıcı düşünün. Süre sonu ulaşılmadan önce iş arkadaşı bu bağlantıyı tıklattığında, bunlar e-posta gönderen bir kullanıcı olarak kaydedilir!


## <a name="step-3-securing-the-authentication-ticket"></a>3. adım: kimlik doğrulaması bileti güvenliğini sağlama

Forms kimlik doğrulaması bileti kablo üzerinden aktarılan ya da bir tanımlama bilgisi veya doğrudan URL'de katıştırılmış. Kimlik bilgilerine ek olarak kimlik doğrulaması bileti (4. adımda göreceğiz gibi) kullanıcı verilerini de içerebilir. Sonuç olarak, anahtar kişinin veri şifrelenir önemli gözetleyen gözler ve, (hatta daha da önemlisi) Form kimlik doğrulama sistemi ile bilet değiştirilmediğini garanti edebilir.

Bilet ait verilerin gizliliği sağlamak için forms kimlik doğrulaması sistem bilet verilerini şifreleyebilirsiniz. Bilet verileri şifrelemek için hata olası duyarlı bilgileri düz metin kablo üzerinden gönderir.

Bilet 's Orijinallik güvence altına almak için forms kimlik doğrulaması sistem gerekir *doğrulamak* anahtar. Doğrulama verileri belirli bir parçasını değiştirilmedi ve aracılığıyla gerçekleştirilir sağlayarak eylemi olan bir  *[ileti kimlik doğrulama kodu (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)*. Buna koysalar MAC bir küçük (Bu durumda bilet) doğrulanması gereken verileri tanımlayan bilgi parçasıdır. MAC tarafından temsil edilen veri değiştirilirse, daha sonra MAC ve veri eşleşmemesidir değil. Ayrıca, hem verileri değiştirebilir ve kendi MAC değiştirilmiş verilerle karşılık gelecek şekilde oluşturmak bir bilgisayar korsanının pkı'ya zordur.

Oluşturma (veya değiştirme olduğunda) bilet, forms kimlik doğrulaması sistem MAC oluşturur ve bilet ait verileri ekler. Bir sonraki istek ulaştığında, forms kimlik doğrulaması sistem bilet verileri özgünlüğünü doğrulamak için MAC ve bilet verilerini karşılaştırır. Şekil 3 grafik bu iş akışı gösterilmiştir.


[![Bilet'ın Orijinallik Sertifikası ile bir MAC güvence altına](forms-authentication-configuration-and-advanced-topics-vb/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image7.png)

**Şekil 03**: MAC anahtarı'nın Orijinallik güvence altına ([tam boyutlu görüntüyü görüntülemek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image9.png))


Kimlik doğrulaması bileti için hangi güvenlik önlemleri uygulanan koruma ayarına bağlıdır. &lt;forms&gt; öğesi. Koruma ayarı şu üç değerden birini atanabilir:

- Tüm - raporu her ikisi de şifrelenir ve dijital olarak imzalanmış (varsayılan).
- Şifreleme - yalnızca şifreleme uygulanan - hiçbir MAC oluşturulur.
- Hiçbiri - bilet, şifrelenmiş ne dijital olarak imzalanmış.
- Doğrulama - MAC oluşturulur, ancak bilet verileri düz metin kablo üzerinden gönderilir.

Microsoft tüm ayarı kullanarak önerir.

### <a name="setting-the-validation-and-decryption-keys"></a>Doğrulama ve şifre çözme anahtarları ayarlama

Şifreleme ve karma algoritmaları şifrelemek ve kimlik doğrulaması bileti doğrulamak için forms kimlik doğrulaması sistemi tarafından kullanılan özelleştirilebilir aracılığıyla [ &lt;machineKey&gt; öğesi](https://msdn.microsoft.com/library/w8h3skw9.aspx) Web.config dosyasında. Tablo 2 anahatları &lt;machineKey&gt; öğenin özniteliklerini ve olası değerleri.

| **Özniteliği** | **Açıklama** |
| --- | --- |
| şifre çözme | Şifreleme için kullanılan algoritmayı belirtir. Bu öznitelik aşağıdaki dört değerden birini içerebilir: - otomatik - varsayılan; decryptionKey özniteliğinin uzunluğuna göre algoritmasını belirler. -AES - kullanır [Gelişmiş Şifreleme Standardı (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algoritması. -DES - kullanır [veri şifreleme standardı (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) bu algoritma pkı'ya zayıf kabul edilir ve kullanılmamalıdır. -3DES - kullanır [Üçlü DES](http://en.wikipedia.org/wiki/Triple_DES) DES algoritması üç kez Adınızdaki algoritması. |
| decryptionKey | Şifreleme algoritması tarafından kullanılan gizli anahtarı. Bu değer (şifre çözme değeri göre) uygun uzunluğu, otomatik olarak üret veya iki değer, eklenen bir onaltılık dize olmalıdır IsolateApps. IsolateApps ekleme her uygulama için benzersiz bir değer kullanmak için ASP.NET bildirir. Kaydın, IsolateApps varsayılandır. |
| doğrulama | Doğrulama için kullanılan algoritmayı belirtir. Bu öznitelik aşağıdaki dört değerden birini içerebilir: - AES - Gelişmiş Şifreleme Standardı (AES) algoritması kullanır. -MD5 - kullanan [İleti Özeti 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algoritması. -SHA1 - kullanan [SHA1](http://en.wikipedia.org/wiki/Sha1) algoritması (varsayılan). -3DES - Üçlü DES algoritması kullanır. |
| validationKey | Doğrulama algoritması tarafından kullanılan gizli anahtarı. Bu değer (doğrulanması değere göre) uygun uzunluğu, otomatik olarak üret veya iki değer, eklenen bir onaltılık dize olmalıdır IsolateApps. IsolateApps ekleme her uygulama için benzersiz bir değer kullanmak için ASP.NET bildirir. Kaydın, IsolateApps varsayılandır. |

**Tablo 2**: &lt;machineKey&gt; öğesi özniteliklerini

Bu şifreleme ve doğrulama seçenekleri ve uzmanları kapsamlı bir tartışma avantajlarını ve dezavantajlarını çeşitli algoritmalar, Bu öğretici kapsamında değildir. Bir ayrıntılı aramak için kullanmak için şifreleme ve doğrulama algoritmaları yönergeler de dahil olmak üzere, bu sorunları adresindeki kullanmak için hangi anahtar uzunlukları ve bu anahtarları oluştur, başvurmak en iyi nasıl *Professional ASP.NET 2.0 güvenlik, üyelik ve rol yönetimi* .

Varsayılan olarak, şifreleme ve doğrulama için kullanılan anahtarların her uygulama için otomatik olarak oluşturulur ve bu anahtarları yerel güvenlik yetkilisi (LSA) depolanır. Kısacası, varsayılan ayarlarını benzersiz anahtarlar, bir web sunucusu web sunucusu ve uygulama uygulaması temel garanti. Sonuç olarak, bu varsayılan davranışı iki aşağıdaki senaryolar için çalışmaz:

- **Web grupları** - bir [web grubu](http://en.wikipedia.org/wiki/Web_farm) senaryo, bir tek bir web uygulaması, ölçeklenebilirlik ve artıklık amacıyla birden çok web sunucusu üzerinde barındırılır. Her gelen istek, bir kullanıcının oturumunu ömrü boyunca, farklı sunucular çeşitli kendi isteklerini işlemek için kullanılabilir anlamı bir gruptaki sunucusuna gönderilir. Sonuç olarak, forms kimlik doğrulaması bileti oluşturulan her sunucu aynı şifreleme ve doğrulama anahtarları kullanmalıdır, şifrelenmiş ve doğrulanmış üzerinde bir sunucu şifresi ve gruptaki başka bir sunucuya doğrulandı.
- **Bilet uygulama paylaşımı arası** -tek bir web sunucusu birden çok ASP.NET uygulaması barındırabilir. Bu farklı uygulamaları tek forms kimlik doğrulaması bileti paylaşmak ihtiyacınız varsa, kendi şifreleme ve doğrulama anahtarları eşleştiğini zorunludur.

Bir web grubunda ayarlama veya aynı sunucu üzerinde uygulamalar arasında kimlik doğrulama biletlerini paylaşımı çalışırken yapılandırmanız gerekecektir &lt;machineKey&gt; etkilenen uygulamaları öğesinde böylece kendi decryptionKey ve validationKey değerleri eşleşme.

Yukarıdaki senaryoların hiçbiri örnek uygulamamız uygularken size hala açık decryptionKey validationKey değerlerini belirtin ve kullanılacak algoritmaları tanımlayın. Ekleme bir &lt;machineKey&gt; Web.config dosyasına ayarı:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample5.xml)]

Daha fazla bilgi için kullanıma [nasıl yapılır: ASP.NET 2.0 MachineKey Yapılandırma](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> DecryptionKey ve validationKey değerleri gelen gerçekleştirilen [Steve Gibson](http://www.grc.com/stevegibson.htm)'s [kusursuz parolaları web sayfası](https://www.grc.com/passwords.htm), her sayfasını ziyaret edin 64 rastgele onaltılı karakter oluşturur. Bu anahtarları üretim uygulamalarınıza yollarını yapma olasılığını azaltmak için mükemmel parolaları sayfa rastgele oluşturulmuş olanlardan yukarıdaki anahtarları yerine kullanmaları önerilir.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>4. adım: Bilet ek kullanıcı verilerini depolama

Birçok web uygulamaları hakkında bilgileri görüntülemek veya şu anda oturum açmış kullanıcıya sayfanın görüntüleme temel. Örneğin, bir web sayfasında kullanıcı adını ve son her sayfanın üst köşedeki açmışken tarih gösterebilir. Forms kimlik doğrulaması bileti şu anda oturum açmış kullanıcının kullanıcı adını depolar, ancak herhangi bir bilgi gerektiğinde sayfa kimlik doğrulaması bileti depolanmadığından bilgi aramak için kullanıcı deposuna - genellikle bir veritabanı - gitmeniz gerekir.

Biraz kod ile biz forms kimlik doğrulaması bileti ek kullanıcı bilgilerini depolayabilir. Bu tür veriler ifade edilebilir [FormsAuthenticationTicket sınıfı](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)'s [UserData özelliği](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Bu, genellikle gereken kullanıcı hakkındaki bilgileri küçük miktarda koymak için yararlı bir yerdir. UserData özelliği parçası olarak kimlik doğrulaması bileti tanımlama bilgisinin ve diğer bilet alanları gibi dahil edilmiştir, şifrelenir ve doğrulanması için belirtilen değer forms kimlik doğrulaması sistemin yapılandırmasına bağlı olarak. Varsayılan olarak, UserData boş bir dizedir.

Kimlik doğrulaması bileti kullanıcı verilerini depolamak için size kullanıcıya özgü bilgileri alan ve bilet depolayan oturum açma sayfasındaki biraz kod yazmanız gerekir. İçinde depolanan veriler, düzgün UserData dize türünde bir özellik olduğundan, bir dize olarak serileştirilmiş gerekir. Örneğin, her kullanıcının doğum tarihi ve bunların İşveren adını bizim kullanıcı deposu dahil ve biz bu iki özellik değerlerini kimlik doğrulaması bileti saklamak istediğinizi düşünün. Biz bu değerleri bir dize olarak İşveren adından kullanıcının tarih Doğum'ın dizesinin dikey çizgi (|) ile birleştirerek seri hale. Northwind Traders çalışan 15 Ağustos 1974 doğacak bir kullanıcı için biz UserData özelliği dize atamanız gerekir: 1974-08-15 | Northwind Traders.

Bilet depolanan verilere erişmek ihtiyacımız olduğunda, biz geçerli isteğin FormsAuthenticationTicket ele geçirme ve UserData özelliği seri bunu yapabilirsiniz. Doğum ve İşveren adı örneği tarih söz konusu olduğunda, biz UserData dize iki alt dizeler (|) sınırlayıcıyı kullanarak Böl.


[![Ek kullanıcı bilgilerini kimlik doğrulaması bileti depolanabilir](forms-authentication-configuration-and-advanced-topics-vb/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image10.png)

**Şekil 04**: ek kullanıcı bilgileri depolanabilir kimlik doğrulaması bileti ([tam boyutlu görüntüyü görüntülemek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image12.png))


### <a name="writing-information-to-userdata"></a>UserData bilgi yazma

Ne yazık ki, forms kimlik doğrulaması bileti için kullanıcıya özgü bilgileri ekleyerek bir bekleyebilirsiniz kadar basit değil. FormsAuthenticationTicket sınıfının UserData özelliği salt okunurdur ve yalnızca FormsAuthenticationTicket sınıf oluşturucu kullanılarak belirtilebilir. UserData özellik oluşturucuda belirtirken, biz de bilet kullanıcının diğer değerler sağlamanız gerekir: kullanıcı adı, tarihi, sona erme ve benzeri. Oturum açma sayfasına önceki öğreticide oluşturduğunuzda bu tüm bize FormsAuthentication sınıfı tarafından işlendi. UserData FormsAuthenticationTicket eklerken, biz zaten FormsAuthentication sınıfı tarafından sağlanan işlevlerinin çoğunu çoğaltmak için kod yazmanız gerekir.

Kullanıcı kimlik doğrulaması bileti için ilgili ek bilgileri kaydetmek için Login.aspx sayfasına güncelleştirerek UserData ile çalışmak için gerekli kodu inceleyelim. Bizim kullanıcı deposunda kullanıcının çalıştığı şirketin ve bunların başlık hakkında bilgi içerir ve bu bilgileri kimlik doğrulaması bileti yakalamak istiyoruz içeriğini. Login.aspx sayfanın Oturum Aç düğmesini Click olay işleyicisi güncelleştirin, böylece kodu aşağıdaki gibi görünür:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample6.vb)]

Şimdi bu kodu bir çizgi geçen bir kerede adım. Dört dize diziler tanımlayarak yöntemi başlatır: kullanıcıları, parolaları, ŞirketAdı ve titleAtCompany. Bu diziler kullanıcı adları, parolalar, şirket adlarını ve başlıkları kullanıcı hesapları için sistemde, üç var olan tutun: Scott, Jisun ve Sam. Gerçek bir uygulamada, kullanıcı mağazadan sayfanın kaynak kodunda kodlanmış değil, bu değerleri seçmeleri.

Sağlanan kimlik bilgilerinin geçerli olsaydı önceki öğreticide, biz yalnızca, aşağıdaki adımları gerçekleştirilen FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked) adlı:

1. Forms kimlik doğrulaması bileti oluşturuldu
2. Bilet uygun depoya yazıldı. Tanımlama bilgisi tabanlı kimlik doğrulama biletlerini tarayıcının tanımlama bilgileri koleksiyonu kullanılır; cookieless kimlik doğrulama biletlerini URL'ye bilet verilerini seri
3. Kullanıcı uygun sayfaya yönlendirilir.

Bu adımları Yukarıdaki kod çoğaltılır. İlk olarak, biz sonunda UserData özelliğinde depolayacak dize şirket adı ve bir dikey çizgi (|) ile iki değer sınırlandırma başlık birleştirerek biçimlendirilmemiş.

UserDataString As String dim String.Concat(companyName(i), = "|", titleAtCompany(i))

Ardından, kimlik doğrulaması bileti oluşturur, yöntemi çağrılır, FormsAuthentication.GetAuthCookie şifreler ve yapılandırma ayarlarını göre doğrular ve HttpCookie nesneyi yerleştirir.

Dim authCookie As HttpCookie = FormsAuthentication.GetAuthCookie(UserName.Text, RememberMe.Checked)

Tanımlama bilgisi içinde katıştırılmış FormAuthenticationTicket çalışmak için FormAuthentication sınıfının çağırmak ihtiyacımız [şifresini yöntemi](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), geçen tanımlama bilgisi değeri.

Bilet olarak FormsAuthenticationTicket dim FormsAuthentication.Decrypt(authCookie.Value) =

Ardından oluşturuyoruz bir *yeni* FormsAuthenticationTicket örnek tabanlı varolan FormsAuthenticationTicket'ın değerleri. Ancak, bu yeni anahtarı (userDataString) kullanıcıya özgü bilgileri içerir.

NewTicket olarak FormsAuthenticationTicket dim yeni FormsAuthenticationTicket(ticket. = Sürüm, bilet. Anahtar adı. İssueDate, bilet. Süre sonu, bilet. IsPersistent, userDataString)

Biz sonra şifrelemek (ve doğrula) çağırarak yeni FormsAuthenticationTicket örnek [yöntemi şifrelemek](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)ve bu şifrelenmiş (ve doğrulanmış) verileri geri authCookie yerleştirin.

authCookie.Value = FormsAuthentication.Encrypt(newTicket)

Son olarak, authCookie Response.Cookies koleksiyonuna eklenir ve kullanıcıya göndermek için uygun sayfayı belirlemek için GetRedirectUrl yöntemi çağrılır.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample7.vb)]

Bu kod tümünün UserData özelliği salt okunur olduğundan ve FormsAuthentication sınıfı GetAuthCookie, SetAuthCookie veya RedirectFromLoginPage yöntemlerinden UserData bilgileri belirtmek için herhangi bir yöntem sağlamaz gereklidir.

> [!NOTE]
> Biz yalnızca incelenmesi kodunu bir tanımlama bilgisi tabanlı kimlik doğrulaması bileti kullanıcıya özgü bilgileri depolar. Forms kimlik doğrulaması bileti URL'ye serileştirmek için sorumlu .NET Framework iç sınıflarıdır. Yazıyı kısa, kullanıcı verilerini bir tanımlama bilgisi içermeyen forms kimlik doğrulaması bileti depolanamıyor.


### <a name="accessing-the-userdata-information"></a>UserData bilgilerine erişme

Bu noktada her kullanıcının şirket adı ve başlık depolanmış forms kimlik doğrulaması bileti 's UserData özelliğinde oturum açtığında. Bu bilgiler kullanıcı deposuna seyahat gerek kalmadan herhangi bir sayfasında kimlik doğrulaması bileti erişilebilir. Bu bilgiler UserData özelliğinden nasıl alınabilir göstermek için yalnızca kullanıcı adını, ancak ayrıca çalıştıkları şirket ve bunların başlık Hoş Geldiniz iletisi içeren Default.aspx şimdi güncelleştirin.

Şu anda Default.aspx AuthenticatedMessagePanel Masası WelcomeBackMessage adlı bir etiket denetimi içerir. Bu pano, yalnızca kimliği doğrulanmış kullanıcılara görüntülenir. Default.aspx'in sayfa kodunda güncelleştirme\_aşağıdaki gibi görünüyor. böylece olay işleyicisi yükleyin:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample8.vb)]

Request.IsAuthenticated True olan sonra WelcomeBackMessage'nın metin özelliği ilk tekrar Hoş Geldiniz ayarlanır *kullanıcıadı*. Ardından, biz temel FormsAuthenticationTicket erişebilmesi için User.Identity özelliği FormsIdentity nesnesine atamalısınız. Biz FormsAuthenticationTicket sonra biz UserData özelliği başlık ve şirket adı seri durumdan çıkarır. Bu, dikey çizgi karakterinden dizesi ayırarak gerçekleştirilir. Başlık ve şirket adı WelcomeBackMessage etiketinde görüntülenir.

Şekil 5 eylemde Bu ekranının ekran görüntüsü gösterilmektedir. Scott oturum açmayı Scott'ın şirket ve başlık içeren bir Hoş Geldiniz geri iletisi görüntüler.


[![Şu anda oturum açan kullanıcının şirket ve başlığı görüntülenir](forms-authentication-configuration-and-advanced-topics-vb/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image13.png)

**Şekil 05**: şu anda oturum açan kullanıcının şirket ve başlığı görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image15.png))


> [!NOTE]
> Kimlik doğrulaması bileti 's UserData özelliği kullanıcı deposu için bir önbellek olarak görev yapar. Herhangi bir önbellek gibi temel veri değiştirildiğinde güncelleştirilmesi gerekir. Örneğin, kullanıcılar kendi profili güncelleştirebilir bir web sayfası ise, kullanıcı tarafından yapılan değişiklikleri yansıtacak şekilde UserData özelliğinde önbelleğe alanları yenilenmelidir.


## <a name="step-5-using-a-custom-principal"></a>5. adım: özel esas kullanma

Her gelen isteğe göre FormsAuthenticationModule kullanıcının kimliğini doğrulamak çalışır. Süresi dolmuş olmayan kimlik doğrulama anahtarı varsa, FormsAuthenticationModule HttpContext.User özelliği için yeni bir GenericPrincipal nesnesi atar. Bu GenericPrincipal nesne türü forms kimlik doğrulaması bileti için referans içeriyor FormsIdentity bir kimliğe sahip. GenericPrincipal sınıfı IPrincipal uygulayan bir sınıf tarafından gereken tam en az işlevselliği içerir - yalnızca bir kimlik özelliği ve IsInRole yöntemi vardır.

Asıl nesne iki sorumlulukları vardır: kullanıcının ait olduğu için hangi rollerin belirtmek ve kimlik bilgilerini sağlamak için. Bu IPrincipal arabiriminin IsInRole gerçekleştirilir (*roleName*) yöntemini ve kimlik özelliği, sırasıyla. GenericPrincipal sınıfı Rol adlarının dize dizisi için kurucusu belirtilmesine olanak verir; kendi IsInRole (*roleName*) yöntemi yalnızca denetler olmadığını görmek için geçirilen içinde *roleName* içinde dize dizisi yok. FormsAuthenticationModule GenericPrincipal oluşturduğunda, boş bir dize dizisi GenericPrincipal'ın oluşturucuya geçirir. Sonuç olarak, tüm IsInRole çağrısına her zaman False döndürür.

GenericPrincipal sınıfı rolleri değil kullanıldığı çoğu forms tabanlı kimlik doğrulama senaryosu için gereksinimlerini karşılar. Varsayılan rol işleme olduğu yetersiz bu durumlar için ya da özel bir kimlik nesnesi kullanıcı ile ilişkilendirmek gerektiğinde, kimlik doğrulama iş akışı sırasında özel bir IPrincipal nesnesi oluşturun ve HttpContext.User özelliğine atayın.

> [!NOTE]
> Öğreticiler, gelecekte göreceğiz olarak, ASP. NET'in rolleri framework etkin türünde bir özel asıl nesne oluşturur [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) ve forms kimlik doğrulaması oluşturulan GenericPrincipal nesnenin üzerine yazar. Rolleri framework'ün API ile arabirim oluşturmak için sorumlunun IsInRole yöntemi özelleştirmek için bunu yapar.


Biz kendisini rolleriyle henüz kaygı olmayan olduğundan, biz juncture en bu özel bir kural oluşturmak için olması gereken tek nedeni asıl özel bir kimlik nesnesine ilişkilendirilecek olacaktır. Adım 4'te ek kullanıcı bilgilerini özellikle de kimlik doğrulaması bileti 's UserData özelliğinde kullanıcının şirket adını ve bunların başlık depolamak Aranan. Ancak, UserData yalnızca kimlik doğrulaması bileti üzerinden erişilebilir ve biz bilet depolanan kullanıcı bilgilerini görüntülemek istediğiniz zaman UserData özelliği ayrıştırılamıyor ihtiyacımız, yani seri hale getirilmiş bir dize olarak sonra yalnızca bilgilerdir.

IIdentity uygular ve ŞirketAdı ve başlık özellikleri içeren bir sınıf oluşturarak biz geliştirici deneyimi artırabilir. Böylece, başlık ŞirketAdı ve başlık özellikleri olmadan aracılığıyla doğrudan UserData özelliği ayrıştırılamıyor nasıl bilmeniz gereken ve bir geliştirici şu anda oturum açmış kullanıcının şirket adı erişebilirsiniz.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Özel kimlik ve asıl sınıfları oluşturma

Bu öğretici için özel asıl ve kimlik nesneleri uygulamada oluşturalım\_kod klasör. Bir uygulama eklemeye başlayın\_kod projenize klasörü - Çözüm Gezgini'nde proje adına sağ tıklayın, ASP.NET klasörü Ekle seçeneğini belirleyin ve uygulamayı seçin\_kodu. Uygulama\_kodun klasörüdür sınıfı Web sitesine belirli dosyaları içeren özel bir ASP.NET klasör.

> [!NOTE]
> Uygulama\_kod klasörü, yalnızca projenizi Web sitesi proje modeli aracılığıyla yönetirken kullanılmalıdır. Kullanıyorsanız [Web uygulama projesi modeli](https://msdn.microsoft.com/asp.net/Aa336618.aspx), standart bir klasör oluşturun ve sınıflar olarak ekleyebilirsiniz. Örneğin, sınıfları adlı yeni bir klasör ekleyin ve kodunuzu var. yerleştirin.


Ardından, iki yeni sınıf dosyaları için uygulama ekleme\_kod klasörü, bir adlandırılmış CustomIdentity.vb ve bir adlı CustomPrincipal.vb.


[![CustomIdentity ve CustomPrincipal sınıfları projenize ekleyin](forms-authentication-configuration-and-advanced-topics-vb/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image16.png)

**Şekil 06**: CustomIdentity ve CustomPrincipal sınıfları bilgisayarınızı projeye ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image18.png))


AuthenticationType, IsAuthenticated ve ad özelliklerini tanımlar IIdentity arabirimi uygulamak için sorumlu CustomIdentity sınıftır. Bu gerekli özellikleri yanı sıra temel alınan forms kimlik doğrulaması bileti yanı sıra kullanıcının şirket adı ve başlık özelliklerini gösterme de ilginizi duyuyoruz. Aşağıdaki kod CustomIdentity sınıfına girin.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample9.vb)]

Sınıf FormsAuthenticationTicket üye değişkeni içerdiğini unutmayın (\_bilet) ve bu anahtar bilgileri oluşturucu kullanılarak sağlanmalıdır. Bu bilet verileri kimliğin adı döndürme kullanılır; kendi UserData özelliği ŞirketAdı ve başlık özellikleri için değer döndürmek için ayrıştırılır.

Ardından, CustomPrincipal sınıfı oluşturun. Biz juncture en bu rolleri ile ilgili bir kaygınız beri CustomPrincipal sınıfının Oluşturucusu yalnızca CustomIdentity nesnesi kabul eder; kendi IsInRole yöntemi her zaman False değerini döndürür.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample10.vb)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Gelen isteğin güvenlik bağlamı için bir CustomPrincipal nesnesi atama

Şimdi ŞirketAdı ve başlık özellikleri eklemek için varsayılan kimlik belirtimi genişleten bir sınıfı gibi özel kimlik kullanan özel bir asıl sınıfı sunuyoruz. Biz ASP.NET ardışık adımla hazır ve gelen isteğin güvenlik bağlamına bizim Özel asıl nesne atayın.

ASP.NET ardışık gelen bir istek alır ve bir dizi adımı üzerinden işler. Her aşamada ASP.NET ardışık düzenine dokunun ve kendi ömrü belirli noktalarında isteği değiştirmek geliştiricilere edinerek belirli bir olay tetiklenir. FormsAuthenticationModule örneğin yükseltmek ASP.NET için bekleyeceği [AuthenticateRequest olay](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), isteğe bağlı olarak, bu noktada kimlik doğrulama biletini gelen isteği inceler. Kimlik doğrulama biletini bulunursa, bir GenericPrincipal nesnesi oluşturulur ve HttpContext.User özelliğine atanır.

AuthenticateRequest sonra ASP.NET ardışık olayını [PostAuthenticateRequest olay](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), hangi biz FormsAuthenticationModule bir örneği tarafından oluşturulan GenericPrincipal nesneyi burada değiştirmek olduğu bizim CustomPrincipal nesnesi. Şekil 7, bu iş akışı gösterilmektedir.


[![GenericPrincipal CustomPrincipal PostAuthenticationRequest olay ile değiştirilir](forms-authentication-configuration-and-advanced-topics-vb/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image19.png)

**Şekil 07**: GenericPrincipal CustomPrincipal PostAuthenticationRequest olayda almıştır ([tam boyutlu görüntüyü görüntülemek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image21.png))


Bir ASP.NET ardışık düzen olaya yanıt olarak kod yürütmek için size uygun olay işleyicisi Global.asax dosyasında oluşturabilir veya kendi HTTP modülü oluşturun. Bu öğretici için olay işleyicisini Global.asax dosyasında oluşturalım. Web sitenize Global.asax ekleyerek başlayın. Çözüm Gezgini'nde proje adına sağ tıklayın ve genel uygulama sınıfı Global.asax adlı türünde bir öğe ekleyin.


[![Global.asax dosyası Web sitenize ekleyin](forms-authentication-configuration-and-advanced-topics-vb/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image22.png)

**Şekil 08**: Global.asax dosyası için Web sitenizi ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image24.png))


Olay işleyicileri son başlangıç dahil olmak üzere ASP.NET ardışık düzen olayların sayısı için varsayılan Global.asax şablonu içerir ve [hata olayı](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), diğerlerinin yanı sıra. Biz bu uygulama için değil gerektiğinde bu olay işleyicileri kaldırmak çekinmeyin. Biz ilgilendiğiniz PostAuthenticateRequest etkinliğidir. Kendi biçimlendirme aşağıdakine benzer şekilde, Global.asax dosyası güncelleştirin:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample11.aspx)]

Uygulama\_OnPostAuthenticateRequest yöntemini yürütür her zaman ASP.NET çalışma zamanı gelen her sayfa isteğinde bir kez gerçekleşir PostAuthenticateRequest olayını başlatır. Olay işleyicisi, kullanıcının kimliği doğrulanır ve form kimlik doğrulaması kimlik doğrulamasının yapıldığı olmadığını denetleyerek başlar. Bu durumda, yeni bir CustomIdentity nesnesi oluşturulur ve geçerli isteğin kimlik doğrulaması bileti kurucusunda geçirildi. CustomPrincipal nesnesi oluşturulur ve yeni oluşturulan CustomIdentity nesnesi kurucusunda geçirildi. Son olarak, geçerli isteğin güvenlik bağlamı yeni oluşturulan CustomPrincipal nesneye atanmış.

-CustomPrincipal nesnesi isteğin güvenlik bağlamı ile ilişkilendirme - son adımı asıl iki özelliklerine olduğunu unutmayın: HttpContext.User ve Thread.CurrentPrincipal. Bu iki atamaları, ASP.NET güvenlik kapsamları işlenme nedeniyle gereklidir. .NET Framework bir güvenlik bağlamı her çalışan iş parçacığı ile ilişkilendirir; Bu bilgiler IPrincipal nesnesi olarak kullanılabilir [iş parçacığı nesnesi](https://msdn.microsoft.com/library/system.threading.thread.aspx)'s [CurrentPrincipal özelliği](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). Bir kafa karıştırıcı nedir ASP.NET kendi güvenlik bağlamı bilgilerini (HttpContext.User) sahip olur.

Belirli senaryolarda Thread.CurrentPrincipal özelliği güvenlik bağlamı belirlerken incelenir; Diğer senaryolarda HttpContext.User kullanılır. Örneğin, hangi kullanıcıların bildirimli olarak durumuna geliştiriciler izin veren .NET güvenlik özellikleri vardır veya rolleri bir sınıf örneği veya belirli yöntemleri çağırma (bkz [iş ve veri kullanan katmanlar için yetkilendirme kuralları ekleme PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Kapak altında bu bildirim temelli teknikler Thread.CurrentPrincipal özelliği aracılığıyla güvenlik bağlamı belirler.

Diğer senaryolarda HttpContext.User özelliği kullanılır. Örneğin, önceki öğreticide Biz bu özellik şu anda oturum açmış kullanıcının kullanıcı adını görüntülemek için kullanılır. Açıkçası, daha sonra bu güvenlik bağlamı bilgilerini Thread.CurrentPrincipal ve HttpContext.User özelliklerinde eşleştiğini zorunludur.

ASP.NET çalışma zamanı bu özellik değerlerini bize için otomatik olarak eşitlenir. Ancak, bu eşitleme AuthenticateRequest olayından sonra meydana gelir ancak *önce* PostAuthenticateRequest olay. Sonuç olarak, özel bir kural PostAuthenticateRequest olayında eklerken Thread.CurrentPrincipal Aksi takdirde Thread.CurrentPrincipal el ile atamanız emin olmak ihtiyacımız ve HttpContext.User eşitlenmemiş olacaktır. Bkz: [Context.User vs. Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) bu sorunla ilgili daha ayrıntılı bir tartışma için.

### <a name="accessing-the-companyname-and-title-properties"></a>Başlık özellikleri ve ŞirketAdı erişme

Her bir istek ulaştığında ve uygulama, ASP.NET altyapısı gönderilen\_OnPostAuthenticateRequest olay işleyicisini Global.asax yangın. İstek tarafından FormsAuthenticationModule başarıyla doğrulandı, olay işleyicisi forms kimlik doğrulaması bileti dayanan bir CustomIdentity nesnesi ile yeni bir CustomPrincipal nesnesi oluşturur. Yerinde bu mantığı ile şu anda oturum açmış kullanıcının şirket adı ve başlık bilgilerine erişme son derece basittir.

Sayfaya dönmek\_burada adım 4'te yazdığımız form kimlik doğrulaması bileti almak ve kullanıcının şirket adı ve başlık görüntülemek için UserData özelliği ayrıştırılamıyor kod Default.aspx yük olay işleyicisi. Artık kullanımda CustomPrincipal ve CustomIdentity nesneleriyle anahtar kişinin UserData özellik değerleri ayrıştırmak için gerek yoktur. Bunun yerine, yalnızca CustomIdentity nesneye bir başvurusu alın ve ŞirketAdı ve başlık özelliklerini kullanın:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample12.vb)]

## <a name="summary"></a>Özet

Bu öğreticide Web.config aracılığıyla forms kimlik doğrulaması sistemin ayarlarını özelleştirmek nasıl incelendi. Kimlik doğrulaması bileti 's sona erme nasıl işlendiğini ve şifreleme ve doğrulama korumaları bileti denetleme ve değiştirme korumak için nasıl kullanıldığı inceledik. Son olarak, anahtar ek kullanıcı bilgilerini ve bu bilgileri bir daha Geliştirici dostu şekilde kullanıma sunmak için özel asıl ve kimlik nesneleri kullanmayı depolamak için kimlik doğrulaması bileti 's UserData özelliği kullanma açıklanmaktadır.

Bu öğretici ASP.NET forms kimlik doğrulaması bizim incelendiğinde sonlanır. Sonraki öğretici bizim gezisine üyelik framework uygulamasına başlatır.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Form kimlik doğrulaması dissecting](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Açıklanmıştır: ASP.NET 2.0 form kimlik doğrulaması](https://msdn.microsoft.com/library/aa480476.aspx)
- [Nasıl yapılır: ASP.NET 2.0 form kimlik doğrulamasını korumak](https://msdn.microsoft.com/library/ms998310.aspx)
- [Profesyonel ASP.NET 2.0 güvenlik, üyelik ve rol yönetimi](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Oturum açma denetimleri güvenliğini sağlama](https://msdn.microsoft.com/library/ms178346.aspx)
- [&lt;Kimlik doğrulaması&gt; öğesi](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;Forms&gt; öğesi için &lt;kimlik doğrulaması&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [&lt;MachineKey&gt; öğesi](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Tanımlama bilgisi ve Forms kimlik doğrulaması bileti anlama](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Bu öğreticide yer alan konularda video eğitim

- [Forms kimlik doğrulaması özelliklerini değiştirme](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Kurulumu ve kullanımı daha az tanımlama bilgisi kimlik doğrulaması bir ASP.NET uygulamasında nasıl](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [ASP Forms Oturum Açmayı Yeniden Konumlandırma](../../../videos/authentication/asp-forms-login-relocation.md)
- [Forms Oturum Açma Özel Anahtar Yapılandırması](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Kimlik Doğrulama Metoduna Özel Veri Ekleme](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Özel Asıl Nesneler Kullanma](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Alicja Maziarz oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Önceki](an-overview-of-forms-authentication-vb.md)
