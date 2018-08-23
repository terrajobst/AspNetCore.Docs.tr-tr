---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
title: Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular (VB) | Microsoft Docs
author: rick-anderson
description: Bu öğreticide size çeşitli forms kimlik doğrulaması ayarlarını inceleyin ve bunları form öğesi aracılığıyla değiştirme konusuna bakın. Bu ayrıntılı bir oluşturulmasını gerektirir...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: 829d2f56-5c48-445b-b826-3418a450c788
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
msc.type: authoredcontent
ms.openlocfilehash: eb533cf763c2f3132ea0a5420b4d4cbea16c61cd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753936"
---
<a name="forms-authentication-configuration-and-advanced-topics-vb"></a>Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_vb.pdf)

> Bu öğreticide size çeşitli forms kimlik doğrulaması ayarlarını inceleyin ve bunları form öğesi aracılığıyla değiştirme konusuna bakın. Forms kimlik doğrulaması bileti ait zaman aşımı değeri, bir oturum açma sayfası (SignIn.aspx Login.aspx yerine gibi) özel bir URL ve cookieless form kimlik doğrulama biletlerini kullanarak özelleştirme ayrıntılı bir bakış bu oluşturulmasını gerektirir.


## <a name="introduction"></a>Giriş

İçinde [önceki öğreticide](an-overview-of-forms-authentication-vb.md) gelen bir günlük görüntüleme için sayfasında farklı oluşturmak için Web.config dosyasında yapılandırma ayarlarını belirten bir ASP.NET uygulamasında form kimlik doğrulaması gerçekleştirmek için gerekli adımları inceledik Kimliği doğrulanmış ve anonim kullanıcılar için içeriği. Web sitesi modu özniteliğini ayarlayarak forms kimlik doğrulaması kullanacak şekilde yapılandırılmış olduğunu anımsayın &lt;kimlik doğrulaması&gt; form öğesi. &lt;Kimlik doğrulaması&gt; öğe isteğe bağlı olarak içerebilir bir &lt;forms&gt; alt öğe üzerinden forms kimlik doğrulaması ayarlarını kaynaklardan belirtilebilir.

Bu öğreticide çeşitli forms kimlik doğrulaması ayarlarını incelemek ve ederiz aracılığıyla nasıl &lt;forms&gt; öğesi. Forms kimlik doğrulaması bileti ait zaman aşımı değeri, bir oturum açma sayfası (SignIn.aspx Login.aspx yerine gibi) özel bir URL ve cookieless form kimlik doğrulama biletlerini kullanarak özelleştirme ayrıntılı bir bakış bu oluşturulmasını gerektirir. Ayrıca, forms kimlik doğrulaması bileti düzenini daha yakından inceleyin eder ve ASP.NET anahtar verileri inceleme ve izinsiz güvenli olmasını sağlamak için gereken güvenlik önlemleri bakın. Forms kimlik doğrulaması bileti içinde ek kullanıcı verilerini depolamak nasıl ve bu verileri özel bir sorumlu nesnesi aracılığıyla nasıl son olarak, atacağız.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>1. adım: İnceleme &lt;forms&gt; yapılandırma ayarları

ASP.NET formları kimlik doğrulama sisteminde bir uygulama tarafından uygulama temelinde özelleştirilebilir yapılandırma ayarları sunar. Bu gibi ayarları içerir: form kimlik doğrulaması ömrünü bilet; ne tür bir koruma bilet uygulanır; biletleri hangi koşullar cookieless kimlik doğrulamasında kullanılan; oturum açma sayfasının yolu; ve diğer bilgiler. Varsayılan değerleri değiştirmek için ekleme bir [ &lt;forms&gt; öğesi](https://msdn.microsoft.com/library/1d3t3c61.aspx) bir alt öğesi olarak [ &lt;kimlik doğrulaması&gt; öğesi](https://msdn.microsoft.com/library/532aee0e.aspx), bu özelliği belirtme XML özniteliği özelleştirmek istediğiniz değerleri bunu ister:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample1.xml)]

Tablo 1 ile özelleştirilebilen özelliklerini özetler &lt;forms&gt; öğesi. Web.config bir XML dosyası olduğundan, sol sütunda öznitelik adları büyük küçük harfe duyarlıdır.


| <strong>Özniteliği</strong> |                                                                                                                                                                                                                                     <strong>Açıklama</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         cookieless         |                                                                                                                Bu öznitelik, hangi koşullar altında URL'de gömülen karşı bir tanımlama bilgisi kimlik doğrulaması bileti depolanacağını belirtir. İzin verilen değerler: UseCookies; UseUri; Otomatik Algıla; ve UseDeviceProfile (varsayılan). 2. adım, bu ayar daha ayrıntılı bir şekilde inceler.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Kullanıcılar sorgu dizesinde belirtilen RedirectUrl değer yoksa oturum açma sayfasında oturum açtıktan sonra yeniden yönlendirileceği URL'yi belirtir. Default.aspx varsayılan değerdir.                                                                                                                                                         |
|           etki alanı           | Bu ayar, tanımlama bilgisi tabanlı kimlik doğrulama biletlerini kullanırken, tanımlama bilgisi s etki alanı değeri belirtir. Tarayıcının içinden (www.yourdomain.com gibi) verilmiş etki alanını kullan boş bir dize varsayılan değerdir. Bu durumda, tanımlama bilgisi olacak <strong>değil</strong> yapma admin.yourdomain.com gibi alt etki alanlarını istediğinde gönderilemez. Tüm alt etki alanlarıyla geçirilecek tanımlama bilgisi isterseniz Özelleştir yourdomain.com için ayarı etki alanı özniteliği gerekir. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Kimliği doğrulanmış kullanıcılar aynı sunucuda diğer web uygulamalarında yeniden yönlendirme URL'leri, anımsanacak gösteren bir Boole değeri. Varsayılan olarak yanlıştır.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      Oturum açma sayfasının URL'si. Varsayılan değer, Login.aspx'tir.                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   Tanımlama bilgisi tabanlı kimlik doğrulaması anahtarları, tanımlama bilgisinin adını kullanırken. Varsayılandır. ASPXAUTH.                                                                                                                                                                                                   |
|            yol            |                                                                             Bu ayar, tanımlama bilgisi tabanlı kimlik doğrulama biletlerini kullanırken, tanımlama bilgisi s yol özniteliğini belirtir. Yol özniteliği bir tanımlama bilgisi için belirli bir dizin sıradüzeni kapsamını sınırlamak bir geliştirici sağlar. Varsayılan değer / hangi kimlik doğrulaması bileti tanımlama bilgisinin etki alanında yapılan herhangi bir istek göndermek için tarayıcı bildirir.                                                                              |
|         koruma         |                                                                                                                                            Forms kimlik doğrulaması bileti korumak için kullanılan hangi teknikleri gösterir. İzin verilen değerler: tüm (varsayılan); Şifreleme; None; ve doğrulama. Bu ayarlar, adım 3'te ayrıntılı ele alınmıştır.                                                                                                                                            |
|         içindeki requireSSL öğesini         |                                                                                                                                                                                Bir SSL bağlantısı kimlik doğrulaması tanımlama bilgisinin iletilip iletilmeyeceğini gerekip gerekmediğini gösteren bir Boole değeri. Varsayılan değer false'tur.                                                                                                                                                                                |
|     ilerlemiş      |                                                                                                 Kullanıcı kimlik doğrulaması s tanımlama bilgisi zaman aşımı her erişimde sıfırlanacağını olup olmadığını tek bir oturumda siteyi ziyaret belirten bir Boolean değer. Varsayılan değer true olur. Kimlik doğrulaması bileti zaman aşımı ilkesini belirtme daha ayrıntılı anlatılan s zaman aşımı değeri bölüm anahtarı.                                                                                                 |
|          zaman aşımı           |                                                                                                                               Saat sonra kimlik doğrulama bileti tanımlama süresi dakika cinsinden belirtir. Varsayılan değer 30'dur. Kimlik doğrulaması bileti zaman aşımı ilkesini belirtme daha ayrıntılı anlatılan s zaman aşımı değeri bölüm anahtarı.                                                                                                                               |

**Tablo 1**: bir özetini &lt;forms&gt; öğenin öznitelikleri

ASP.NET 2.0 ve sonrasında, varsayılan formlar kimlik doğrulaması .NET Framework FormsAuthenticationConfiguration sınıfında sabit kodlanmış değerlerdir. Herhangi bir değişiklik, Web.config dosyasında bir uygulama tarafından uygulama temelinde uygulanmalıdır. Bu, ASP.NET tarafından farklıdır 1.x, burada varsayılan formlar kimlik doğrulama değerlerini machine.config dosyasında depolanan (ve bu nedenle machine.config düzenleme aracılığıyla yapılabilecek). ASP.NET'in konusunda biraz 1.x, onu bahsetmek için faydalı forms kimlik doğrulaması sistem ayarlarını bir dizi ASP.NET 2. 0'farklı varsayılan değerlerine sahip ve daha ötesine ASP.NET'te 1.x. Uygulamanızı bir ASP.NET 1.x ortamından geçiriyorsanız, bu farkların farkında olmak önemlidir. Başvurun [ &lt;forms&gt; öğesi teknik belgeler](https://msdn.microsoft.com/library/1d3t3c61.aspx) listesi farkların için.

> [!NOTE]
> Zaman aşımı, etki alanı ve yol gibi çeşitli forms kimlik doğrulaması ayarlarını elde edilen form kimlik doğrulaması bileti tanımlama için ayrıntıları belirtin. Tanımlama bilgileri, nasıl çalıştıklarını ve bunların çeşitli özellikler hakkında daha fazla bilgi için okuma [bu tanımlama bilgileri öğretici](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Anahtar zaman aşımı değeri belirterek

Forms kimlik doğrulaması bileti bir kimliği temsil eden bir belirteçtir. Tanımlama bilgisi tabanlı kimlik doğrulaması anahtarlarını, bu belirteci bir tanımlama bilgisi biçiminde tutulan ve her isteğin web sunucusuna gönderilir. Esas olarak, belirteç elinde bildirir, ben *kullanıcıadı*, miyim zaten oturum açtıysanız ve böylece bir kullanıcının kimliğini arasında sayfa ziyareti anımsanabileceğini kullanılır.

Forms kimlik doğrulaması bileti yalnızca kullanıcının kimliğini içerir, ancak bütünlük ve güvenliğini belirtecin sağlamaya yardımcı olmak için bilgiler de içerir. Sonuçta, biz sahte bir belirteç oluşturmak veya bir LEGIT belirteç underhanded şekilde değiştirmek için kullanabilmek için alınan kullanıcı istemezsiniz.

Böyle bir bit bileti dahil bilgilerin bir *bitiş*, tarih ve saat anahtar artık geçerli değil. Bir kimlik doğrulaması bileti FormsAuthenticationModule inceler her zaman anahtar süre sonu olmayan henüz geçti sağlar. Varsa, anahtar yoksayar ve kullanıcı anonim olarak tanımlar. Bu korumayı yeniden yürütme saldırılarına karşı korumaya yardımcı olur. Bir bilgisayar korsanının her bir kullanıcının geçerli bir kimlik doğrulama anahtarı - uygulamalı, bilgisayara fiziksel erişim sağlamasını ve tanımlama bilgilerine kök dizini değiştirme belki de alabilirsiniz. - Bu çalınan kimlik doğrulama anahtarı ile sunucu için bir istek gönderebilir, bir bitiş olmadan ve Giriş elde edin. Süre sonu bu senaryo engellemez, ancak pencerenin sırasında hangi tür bir saldırıya başarılı sınırlayın.

> [!NOTE]
> Forms kimlik doğrulaması sistem tarafından kimlik doğrulaması bileti korumak için kullanılan 3. adım ayrıntıları ek teknikler.


Kimlik doğrulaması bileti oluştururken, forms kimlik doğrulaması sistem zaman aşımı ayarını consulting tarafından sona erme belirler. Tablo 1, 30 dakika için varsayılanları ayarlama zaman aşımı belirtildiği gibi forms kimlik doğrulaması biletinin oluşturulduğu sırada, süre sonu bir tarih ve saat 30 dakika sonra ayarlanır anlamına gelir.

Süre sonu mutlak zaman forms kimlik doğrulaması biletinin süresinin dolduğu bir gelecekte tanımlar. Ancak, geliştiriciler genellikle bir kayan bitiş tarihi, kullanıcının site gelişimin her zaman Sıfırlanan bir uygulamak istiyorsunuz. Bu davranış ilerlemiş ayarları tarafından belirlenir. FormsAuthenticationModule kullanıcı kimlik doğrulaması her zaman (varsayılan) true olarak ayarlanırsa, anahtar süre sonu güncelleştirir. Süre sonu false olarak ayarlanırsa her istekte güncelleştirilmezse, dolayısıyla tam olarak zaman aşımı anahtar ilk zaman geçen dakika sayısı süresi dolacak şekilde bilet neden oluşturuldu.

> [!NOTE]
> Kimlik doğrulaması bileti depolanan süre sonu bir mutlak tarih ve saat değeri, 2 Ağustos 2008 11:34: 00'gibi ' dir. Ayrıca, web sunucusunun yerel saat göreli tarih ve saat olan. Bu tasarım kararına, bazı ilginç etrafında Yaz Saati (Amerika Birleşik Devletleri'nde saatler bir saat (web sunucusu Yaz Saati nerede gözlemlenen yerel ayarda barındırılan varsayılarak) önceden taşındığında olan DST), yan etkileri olabilir. DST başladığı saati yakın bir 30 dakikalık süre sonu ile ASP.NET Web sitesi için ne olacağını göz önünde bulundurun (02: 00'da olduğu). Bir ziyaretçi siteye 11 Mart 2008'de 1: 55'da oturum açtığı düşünün. Bu, 02:25:00 (30 dakika sonra) 11 Mart 2008 süresi dolan bir form kimlik doğrulama anahtarının oluşturur. Ancak, saat 02: 00'da geçici bir çözüm yapar sonra nedeniyle DST 03: 00'da için atlar. Kullanıcı altı dakika (3: 01'da) oturum açtıktan sonra yeni bir sayfa yüklendiğinde FormsAuthenticationModule biletin süresi doldu ve kullanıcı oturum açma sayfasına yönlendirir not alır. Bu ve diğer kimlik doğrulaması bileti zaman aşımı farklılıkları yanı geçici çözümler üzerinde daha kapsamlı bir açıklama için çekme Stefan Schackow'ın bir kopyasını *Professional ASP.NET 2.0 güvenlik, üyelik ve rol yönetimi* (ISBN: 978-0-7645-9698-8).


Şekil 1 ilerlemiş false olarak ayarlanır ve zaman aşımı 30 ayarlandığında iş akışı gösterilmektedir. Oturum açma sırasında oluşturulan kimlik doğrulaması bileti sona erme tarihini içerdiğine dikkat edin ve bu değeri sonraki isteklerde authenticateasync güncelleştirilmez. Biletin süresi doldu FormsAuthenticationModule bulursa, atar ve isteğin anonim olarak değerlendirir.


[![Forms kimlik doğrulaması bileti'nın süre sonu olduğunda ilerlemiş grafik gösterimi false'tur](forms-authentication-configuration-and-advanced-topics-vb/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image1.png)

**Şekil 01**: formları kimlik doğrulama anahtarı'nın süre sonu olduğunda ilerlemiş bir grafik gösterimi false ([tam boyutlu görüntüyü görmek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image3.png))


Şekil 2 gösteren iş akışı ilerlemiş ayarlandığında true ve zaman aşımı 30 değerine ayarlanır. Kimliği doğrulanmış bir istek (süresi doldu bilet oluşturun) alındığında, süre sonu gelecekteki dakika zaman aşımı sayısı için güncelleştirilir.


[![Forms kimlik doğrulaması bileti'nın grafik gösterimi zaman slidingExpiration değeri true](forms-authentication-configuration-and-advanced-topics-vb/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image4.png)

**Şekil 02**: Forms kimlik doğrulaması bileti'nın bir grafik gösterimi ilerlemiş olduğunda true ([tam boyutlu görüntüyü görmek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image6.png))


Tanımlama bilgisi tabanlı kimlik doğrulama biletlerini (varsayılan) kullanırken, bu tartışma tanımlama bilgilerini ayrıca belirtilen kendi expiries olabileceği için biraz daha karmaşık hale gelir. Tanımlama bilgisi yok, tarayıcı tanımlama bilgisinin süre sonu (veya yapanın olmaması) bildirir. Bir süre sonu tanımlama bilgisine sahip değilse, tarayıcı kapatıldığında yok edilir. Bir süre sonu varsa, ancak tanımlama bilgisi kullanıcının bilgisayarında tarihe kadar saklanır ve süre sonu içinde belirtilen süre geçtikten. Bir tanımlama bilgisi tarayıcı tarafından kaldırıldığında, bu artık web sunucusuna gönderilir. Bu nedenle, site dışında günlüğü kullanıcı için bir tanımlama bilgisi yok edilmesini benzerdir.

> [!NOTE]
> Elbette, bir kullanıcı kendi bilgisayarınızda depolanan tüm tanımlama bilgilerini proaktif olarak kaldırabilir. Internet Explorer 7'de, Araçlar, Seçenekler gidin ve gözatma geçmişini bölümünde Sil düğmesine tıklayın. Buradan Sil tanımlama bilgilerini düğmesine tıklayın.


Forms kimlik doğrulama sistemi için geçirilen değere bağlı olarak oturum tabanlı veya süre sonu tabanlı tanımlama bilgisi oluşturur *persistCookie* parametresi. İki giriş parametreleri FormsAuthentication sınıfının GetAuthCookie SetAuthCookie ve RedirectFromLoginPage yöntemleri ele geri çağırma: *kullanıcıadı* ve *persistCookie*. Önceki öğreticide oluşturduğumuz oturum açma sayfasına kalıcı bir tanımlama bilgisi oluşturulup oluşturulmadığını belirlenen bir Beni Hatırla onay kutusunu dahil. Kalıcı bir tanımlama bilgisi süre sonu tabanlıdır; oturum tabanlı kalıcı tanımlama bilgileri.

Zaten ele zaman aşımı ve ilerlemiş kavramlar aynı hem oturum ve süre sonu tabanlı tanımlama bilgisi geçerlidir. Yürütme yalnızca bir alt farklılığı: yarısı belirtilen süre geçtikten birden fazla tanımlama bilgisi süre sonu tabanlı slidingTimeout özelliği true olarak ayarlanmış kullanırken, tanımlama bilgisinin süre sonu yalnızca güncelleştirildiğinde.

Kayan zaman aşımı değeri kullanarak zaman aşımından sonra bir saat (60 dakika), biletleri, bu nedenle bizim Web sitesinin kimlik doğrulaması bileti zaman aşımı ilkelerini güncelleştirelim. Bu değişiklik efekt, Web.config dosyasını güncelleştirmek için ekleme bir &lt;forms&gt; öğesine &lt;kimlik doğrulaması&gt; öğesini aşağıdaki işaretlemeyle:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Bu ancak başka bir oturum açma sayfası URL'si kullanma

Bu yana FormsAuthenticationModule, yetkisiz kullanıcıların otomatik olarak oturum açma sayfasına yeniden yönlendirir. oturum açma sayfasının URL'sini de bilmeniz gerekir. Bu URL'yi loginUrl özniteliği tarafından belirtilen &lt;forms&gt; öğesi ve bu ancak Varsayılanları. Mevcut bir Web sitesi bağlantı noktası oluşturma, farklı bir URL, zaten bozulmasına ve arama motorları tarafından dizini oluşturulmuş bir oturum açma sayfasıyla zaten olabilir. Var olan oturum açma sayfanız login.aspx için yeniden adlandırma ve son bağlantılar ve kullanıcıların yer işaretleri yerine, bunun yerine loginUrl öznitelik oturum açma sayfanız işaret edecek şekilde değiştirebilirsiniz.

Örneğin, ~/Users/SignIn.aspx loginUrl yapılandırma ayarı işaret oturum açma sayfanız SignIn.aspx taşıyordu ve kullanıcı dizinde bulunamadı, şu şekilde:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample3.xml)]

Geçerli uygulamamız zaten Login.aspx adlı bir oturum açma sayfası olduğundan, özel bir değer belirtmek için gerek yoktur &lt;forms&gt; öğesi.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>2. adım: Cookieless form kimlik doğrulama biletlerini kullanma

Varsayılan olarak kendi kimlik doğrulama biletlerini tanımlama bilgilerini koleksiyonda depoladığınızda veya siteyi ziyaret kullanıcı aracıda temel URL'si eklemek form kimlik doğrulama sistemini belirler. Tüm temel masaüstü tarayıcıları destek tanımlama bilgileri, Internet Explorer, Firefox, Opera ve Safari, gibi ancak tüm mobil cihazlar yapın.

Forms kimlik doğrulama sistemi tarafından kullanılan tanımlama bilgisi ilkesi cookieless ayarına bağlıdır &lt;forms&gt; dört değerlerden atanabilir öğesi:

- UseCookies - tanımlama bilgisi tabanlı kimlik doğrulama biletlerini her zaman kullanılması belirler.
- UseUri - gösterir tanımlama bilgisi tabanlı kimlik doğrulama biletlerini hiçbir zaman kullanılmayacaktır.
- Otomatik Algıla - cihaz profili tanımlama bilgileri, tanımlama bilgisi tabanlı kimlik doğrulama biletlerini desteklemiyorsa kullanılmaz; cihaz profili tanımlama bilgilerini destekliyorsa tanımlama bilgilerinin etkin olup olmadığını belirlemek için bir yoklama mekanizması kullanılır.
- UseDeviceProfile - varsayılan; cihaz profili tanımlama bilgilerini destekliyorsa tanımlama bilgisi tabanlı kimlik doğrulama biletlerini kullanır. Araştırma mekanizması kullanılır.

Otomatik Algıla ve UseDeviceProfile ayarları kullanan bir *cihaz profili* cookieless veya tanımlama bilgisi tabanlı kimlik doğrulama biletlerini kullanıp kullanmayacağınızı ascertaining içinde. ASP.NET veritabanı çeşitli cihaz ve tanımlama bilgileri destekledikleri, hangi sürümünü destekledikleri JavaScript vb. gibi yeteneklerini tutar. Her bir cihaz istediğinde bir web sayfası gönderir boyunca bir web sunucusundan bir *Kullanıcı aracısını* cihaz türü tanımlayan HTTP üst bilgisi. ASP.NET, sağlanan kullanıcı aracısı dizesi otomatik olarak kendi veritabanında belirtilen karşılık gelen profili ile eşleşir.

> [!NOTE]
> Cihaz özellikleri bu veritabanının izliyor XML dosya sayısı depolanan [tarayıcı tanım dosyası şeması](https://msdn.microsoft.com/library/ms228122.aspx). Varsayılan cihaz profili dosyalarını % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers yer alır. Uygulamanızın uygulamaya özel dosyaları da ekleyebilirsiniz\_tarayıcılar klasör. Daha fazla bilgi için [nasıl yapılır: tarayıcı türlerini algılamak ASP.NET Web Pages'de](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Varsayılan ayar UseDeviceProfile olduğundan, site bir cihaz profili tanımlama bilgilerini desteklemiyor raporlar tarafından ziyaret edildiğinde cookieless form kimlik doğrulama biletlerini kullanılır.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Kimlik doğrulaması bileti URL kodlaması

Her istek için bunları ziyaret cihaz destekliyorsa, varsayılan formlar kimlik doğrulaması ayarları tanımlama bilgileri kullan neden olan belirli bir Web tarayıcısından bilgileri dahil olmak üzere için doğal bir orta tanımlama bilgileridir. Tanımlama bilgilerini desteklenmiyorsa, kimlik doğrulaması bileti istemciden sunucuya geçirmek için alternatif bir yol uygulanmalıdır. Cookieless ortamlarında kullanılan ortak bir geçici çözüm, URL'deki tanımlama bilgisi verileri şifrelemektir.

URL içinde tür bilgilerin nasıl eklenebilir görmek için en iyi siteyi cookieless kimlik doğrulama biletlerini kullanmaya zorlamak için yoludur. Bu işlem için UseUri cookieless yapılandırma ayarı ayarlayarak gerçekleştirilebilir:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample4.xml)]

Bu değişikliği yaptıktan sonra bir tarayıcı aracılığıyla sitesini ziyaret edin. Tam olarak bunlar önceden yaptığınız gibi anonim kullanıcı olarak ziyaret ederken URL'leri görünecektir. Örneğin, Default.aspx sitesini ziyaret ettiğinde my tarayıcının adres çubuğuna aşağıdaki URL'yi gösterir:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Ancak, URL'de oturum açtıktan sonra forms kimlik doğrulaması bileti katıştırılır. Örneğin, oturum açma sayfasını ziyaret ederek ve Sam oturum açtıktan sonra Default.aspx sayfasına döndürülen ancak URL'si şu olur:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Forms kimlik doğrulaması bileti URL içinde gömülü. Dize (F (jaIOIDTJxIr12xYS VVgkqKCVAuIoW30Bu0diWi6flQC FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) onaltılık kodlanmış kimlik doğrulaması bileti bilgileri temsil eder ve genellikle bir tanımlama bilgisi içinde depolanan aynı verileri.

Sırayla çalışması tanımlama bilgisi olmayan kimlik doğrulama biletlerini için sistem kimlik doğrulaması bilet verilerini içerecek şekilde tüm URL'leri sayfasındaki kodlamanız gerekir, kullanıcı bir bağlantıya tıkladığında Aksi takdirde kimlik doğrulaması bileti kaybolacaktır. Ne bu ekleme mantığını otomatik olarak gerçekleştirilir. Bu işlevi göstermek için Default.aspx sayfasını açın ve sırasıyla sına bağlantısına ve SomePage.aspx, metin ve NavigateUrl özelliklerini ayarlayarak bir köprü denetimini ekleyin. Gerçekten olmadığından bir sayfa SomePage.aspx adlı Projemizin önemi yoktur.

Default.aspx için değişiklikleri kaydedin ve bir tarayıcıdan ziyaret edin. Forms kimlik doğrulaması bileti URL'de eklenir, böylece siteye oturum açın. Ardından, Default.aspx Bağlantıyı Sına bağlantısına tıklayın. Ne oldu? SomePage.aspx adlı sayfa varsa, ardından bir 404 hatası oluştu, ancak, ne burada önemli değildir. Bunun yerine, tarayıcınızın adres çubuğuna odaklanır. URL'de, forms kimlik doğrulaması bileti içerdiğine dikkat edin!

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

URL SomePage.aspx bağlantıda kimlik doğrulaması bileti - eklenen bir URL otomatik olarak dönüştürüldü biz bir lick kod yazma gerekmedi! Form kimlik doğrulaması bileti URL'nin http:// ile başlamayan köprüler için otomatik olarak katıştırılır veya /. Köprüyü Response.Redirect çağrısı, bir köprü denetimini veya rotasına bağlı HTML bağlayıcı öğesi görünür olup olmaması önemli değildir (yani, &lt;bir href = "..."&gt;... &lt;/a&gt;). URL şöyle olmadığı sürece http://www.someserver.com/SomePage.aspx veya /SomePage.aspx, forms kimlik doğrulaması bileti katıştırılmış bizim için.

> [!NOTE]
> Cookieless form kimlik doğrulama biletlerini aynı zaman aşımı ilkelerini tanımlama bilgisi tabanlı kimlik doğrulama biletlerini olarak izliyor. Ancak, tanımlama bilgisi olmayan kimlik doğrulama biletlerini URL'yi doğrudan kimlik doğrulaması bileti katıştırılmış olduğundan yeniden yürütme saldırıları daha fazladır. Bir kullanıcı bir Web sitesini ziyaret eder, oturum açtığında ve sonra bir iş arkadaşınıza e URL yapıştırır düşünün. Süre sonu ulaşılmadan önce iş arkadaşınız bu bağlantıya tıkladığında, kullanıcılar e-posta gönderen bir kullanıcı olarak oturumunuz açılır!


## <a name="step-3-securing-the-authentication-ticket"></a>3. adım: kimlik doğrulaması bileti güvenliğini sağlama

Forms kimlik doğrulaması bileti hat üzerinden iletilen ya da bir tanımlama bilgisi veya URL'yi doğrudan içine gömülebilir. Kimlik bilgilerine ek olarak kimlik doğrulaması bileti (4. adımda göreceğiz gibi) kullanıcı verilerini de içerebilir. Sonuç olarak, anahtar veriler şifrelenir önemli olduğu gözetleyen gözler ve, (hatta daha da önemlisi) formları kimlik doğrulama sistemi ile anahtar değiştirilmediğini garanti edebilir.

Anahtar verilerin gizliliği emin olmak için forms kimlik doğrulama sistemi bilet verileri şifreleyebilirsiniz. Bilet verileri şifrelemek için hata olası duyarlı bilgileri düz metin kablo üzerinden gönderir.

Bir anahtar kimlik doğrulaması sağlamak için forms kimlik doğrulama sistemi gerekir *doğrulama* anahtar. Doğrulama belirli bir veri parçasını değiştirilmedi ve aracılığıyla gerçekleştirilir eylemi olan bir  *[ileti kimlik doğrulama kodu (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)*. (Bu durumda anahtar) doğrulanması gereken verileri tanımlayan bilgileri MAC içinde koysalar küçük bir bilgidir. MAC tarafından temsil edilen veri değiştirilirse, daha sonra MAC ve veri eşleşmemesidir değil. Ayrıca, hem verileri değiştirmek ve kendi MAC ile değiştirilen verileri karşılık olarak oluşturmak bir bilgisayar korsanının hesaplama açısından zordur.

Oluşturma (veya değiştirme olduğunda) bir anahtar geçişi, forms kimlik doğrulama sistemi MAC oluşturur ve anahtar veri ekler. Bir sonraki istek ulaştığında, forms kimlik doğrulama sistemi bilet verileri özgünlüğünü doğrulamak için MAC ve bilet verilerini karşılaştırır. Şekil 3'te bu iş akışı grafik gösterilir.


[![Anahtar kimlik doğrulaması ile bir MAC güvence altına](forms-authentication-configuration-and-advanced-topics-vb/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image7.png)

**Şekil 03**: anahtar kimlik doğrulaması ile bir MAC sağlamış ([tam boyutlu görüntüyü görmek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image9.png))


Kimlik doğrulaması bileti için hangi güvenlik önlemleri uygulanan koruma ayarı bağlıdır. &lt;forms&gt; öğesi. Koruma ayarı şu üç değerden birini atanabilir:

- Tüm - anahtar hem de şifrelenir ve (varsayılan) dijital olarak imzalanmış.
- Şifreleme - yalnızca şifreleme uygulanan - hiçbir MAC oluşturulur.
- Hiçbiri - anahtar geçişi, şifrelenmiş ne dijital olarak imzalanmış.
- Doğrulama - MAC oluşturulur, ancak bilet verileri düz metin kablo üzerinden gönderilir.

Microsoft, tüm bu ayarı kullanarak kesinlikle önerir.

### <a name="setting-the-validation-and-decryption-keys"></a>Doğrulama ve şifre çözme anahtarları ayarlama

Şifreleme ve karma algoritmaları, şifreleme ve kimlik doğrulaması bileti doğrulamak için forms kimlik doğrulama sistemi tarafından kullanılan özelleştirilebilir aracılığıyla [ &lt;machineKey&gt; öğesi](https://msdn.microsoft.com/library/w8h3skw9.aspx) Web.config dosyasındaki. Tablo 2 ana hatlarını &lt;machineKey&gt; öğenin özniteliklerini ve olası değerleri.

| **Özniteliği** | **Açıklama** |
| --- | --- |
| şifre çözme | Şifreleme için kullanılan algoritmayı belirtir. Bu öznitelik aşağıdaki dört değerden birini içerebilir: - otomatik - varsayılan; decryptionKey özniteliğinin uzunluğa göre algoritmasını belirler. -AES - kullanır [Gelişmiş Şifreleme Standardı (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algoritması. -DES - kullanan [veri şifreleme standardı (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) bu algoritma hesaplama açısından zayıf kabul edilir ve kullanılmamalıdır. -3DES - kullanan [Üçlü DES](http://en.wikipedia.org/wiki/Triple_DES) algoritmasını üç kez DES algoritması uygulayarak çalışır. |
| decryptionKey | Şifreleme algoritması tarafından kullanılan gizli anahtar. Bu değer bir onaltılık dize (şifre çözme değere göre) uygun uzunluğu, otomatik olarak oluştur veya iki değer, protokolün olması gerektiği IsolateApps. IsolateApps ekleyerek, her uygulama için benzersiz bir değer kullanmak için ASP.NET bildirir. Otomatik olarak oluşturma, IsolateApps varsayılandır. |
| doğrulama | Doğrulama için kullanılan algoritmayı belirtir. Bu öznitelik aşağıdaki dört değerden birini içerebilir: - AES - Gelişmiş Şifreleme Standardı (AES) algoritması kullanır. -MD5 - kullanan [İleti Özeti 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algoritması. -SHA1 - kullanan [SHA1](http://en.wikipedia.org/wiki/Sha1) algoritması (varsayılan). -3DES - Üçlü DES algoritması kullanır. |
| validationKey | Doğrulama algoritması tarafından kullanılan gizli anahtar. Bu değer bir onaltılık dize (doğrulama değere göre) uygun uzunluğu, otomatik olarak oluştur veya iki değer, protokolün olması gerektiği IsolateApps. IsolateApps ekleyerek, her uygulama için benzersiz bir değer kullanmak için ASP.NET bildirir. Otomatik olarak oluşturma, IsolateApps varsayılandır. |

**Tablo 2**: &lt;machineKey&gt; öğenin öznitelikleri

Bu şifreleme ve doğrulama seçenekleri ve uzmanlarının kapsamlı bir tartışma avantajlarını ve dezavantajlarını çeşitli algoritmalar, bu öğreticinin kapsamı dışındadır var. Ayrıntılı bir konum için kullanmak için şifreleme ve doğrulama algoritmaları hakkında yönergeler dahil olmak üzere, bu sorunları sırasında kullanmak için hangi anahtar uzunlukları ve bu anahtarları oluştur, sorun için en iyi nasıl *Professional ASP.NET 2.0 güvenlik, üyelik ve rol yönetimi* .

Varsayılan olarak, şifreleme ve doğrulama için kullanılan anahtarların her uygulama için otomatik olarak oluşturulur ve bu anahtarları yerel güvenlik yetkilisi (LSA) depolanır. Kısacası, varsayılan ayarları, bir web sunucusu tarafından web sunucusunda hem de uygulama uygulama zorlayabilmelidir benzersiz anahtarlar garanti. Sonuç olarak, bu varsayılan davranışı için aşağıdaki iki senaryoda çalışmaz:

- **Web grupları** - bir [web grubu](http://en.wikipedia.org/wiki/Web_farm) senaryosu, tek bir web uygulaması, ölçeklenebilirlik ve yedeklilik amacıyla birden fazla web sunucusu üzerinde barındırılır. Yani bir kullanıcı oturumunun ömrü boyunca, farklı sunucular çeşitli kendi isteklerini işlemek için kullanılabilir bir sunucuya gruptaki her gelen istek gönderilir. Sonuç olarak, forms kimlik doğrulaması bileti oluşturulan her sunucu aynı şifreleme ve doğrulama anahtarları kullanmanız gerekir, şifrelenmiş ve doğrulanmış üzerinde bir sunucu şifresi çözülür ve gruptaki farklı bir sunucuda doğrulandı.
- **Bilet uygulama paylaşımı çapraz** -tek bir web sunucusu birden çok ASP.NET uygulaması barındırabilir. Tek forms kimlik doğrulaması bileti paylaşmak bu farklı uygulamalar için gerekirse, şifreleme ve doğrulama anahtarlarına eşleştiğini zorunludur.

Bir web grubunda ayarlama veya kimlik doğrulama biletlerini aynı sunucuda uygulamalar arasında paylaşma çalışırken yapılandırmanız gerekecektir &lt;machineKey&gt; etkilenen uygulamaları öğesinde böylece kendi decryptionKey ve validationKey değerleri eşleşme.

Yukarıdaki senaryoların hiçbiri örnek uygulamamız için geçerlidir, ancak biz yine de açık decryptionKey ve validationKey değerleri belirtin ve kullanılacak algoritmalar tanımlayın. Ekleme bir &lt;machineKey&gt; Web.config dosyasına ayarlama:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample5.xml)]

Daha fazla bilgi için kullanıma [nasıl yapılır: ASP.NET 2.0 yapılandırma MachineKey](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Öğesinden alınan decryptionKey ve validationKey değerleri [Steve Gibson](http://www.grc.com/stevegibson.htm)'s [mükemmel parolaları web sayfası](https://www.grc.com/passwords.htm), her sayfasını ziyaret edin 64 rastgele onaltılık karakter oluşturur. Bu anahtarlar, üretim uygulamalarınızı aşamalarından yapma olasılığını azaltmak için yukarıdaki anahtarları rastgele oluşturulmuş olanları mükemmel parolaları sayfasından değiştirin teşvik edilmektedir.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>4. adım: Bilete ek kullanıcı verilerini depolama

Birçok web uygulamaları hakkında bilgi görüntülemek veya sayfanın görüntü şu anda oturum açmış kullanıcıya temel. Örneğin, bir web sayfasında kullanıcı adını ve kendisi son her sayfanın üst köşedeki oturum açmış tarih gösterebilir. Forms kimlik doğrulaması bileti o anda oturum açmış kullanıcının kullanıcı adını depolar, ancak herhangi bir bilgi gerektiğinde sayfa içinde kimlik doğrulaması bileti depolanmaz bilgi aramak için kullanıcı deposuna - genellikle bir veritabanı - gitmeniz gerekir.

Biraz kod ile size ek kullanıcı bilgileri forms kimlik doğrulaması bileti depolayabilirsiniz. Bu tür veriler aracılığıyla ifade [FormsAuthenticationTicket sınıfı](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)'s [UserData özelliği](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Bu, genellikle gereklidir kullanıcı hakkındaki bilgileri küçük miktarlarda yerleştirmek için kullanışlı bir yerdir. Forms kimlik doğrulaması sistem yapılandırmasına bağlı olarak değeri UserData özelliği bir parçası olarak kimlik doğrulaması bileti tanımlama bilgisinin ve diğer anahtar alanları gibi dahil, şifrelenir ve doğrulanmış belirtilen. Varsayılan olarak, UserData boş bir dizedir.

Kimlik doğrulaması bileti kullanıcı verilerini depolamak için kullanıcıya özgü bilgileri Dallarınızla ve bilet depolayan oturum açma sayfasında bir bit kod yazmak gerekiyor. UserData dize türünde bir özellik olduğundan, depolanan verilerin düzgün bir dize olarak seri hale getirilmelidir. Örneğin, her kullanıcının doğum tarihi ve İşveren adını kullanıcı mağazamız dahil ve kimlik doğrulaması bileti bu iki özellik değerleri depolamak istedik düşünün. Biz bu değerleri bir dizeye kullanıcının dikey çizgi (|) ile Doğum'ın dize sonu ile birleştirerek İşveren adından önce gelen seri hale. İçin Northwind Traders çalışan 15 Ağustos 1974 üzerinde geliştirilen bir kullanıcı için biz UserData özelliği dize atadığınız: 1974-08-15 | Northwind Traders.

Bilet depolanan verilere erişmek için ihtiyacımız olduğunda, bunu geçerli isteğin FormsAuthenticationTicket yazılımdır ve UserData özelliği seri durumdan çıkarılırken yapabiliriz. Doğum ve İşveren adı örneği tarih söz konusu olduğunda, size iki alt dizeleri (|) sınırlayıcıyı içine UserData dize ayırırsınız.


[![Ek kullanıcı bilgileri kimlik doğrulaması bileti depolanabilir.](forms-authentication-configuration-and-advanced-topics-vb/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image10.png)

**Şekil 04**: ek kullanıcı bilgileri depolanabilir kimlik doğrulaması bileti ([tam boyutlu görüntüyü görmek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image12.png))


### <a name="writing-information-to-userdata"></a>UserData bilgi yazma

Ne yazık ki, bir form kimlik doğrulaması bileti için kullanıcıya özgü bilgileri ekleme bir bekleyebileceğiniz gibi basit değildir. FormsAuthenticationTicket sınıfın UserData özelliği salt okunurdur ve yalnızca FormsAuthenticationTicket sınıfı oluşturucu kullanılarak belirtilebilir. UserData özellik, oluşturucuda belirtirken, aynı zamanda anahtar kullanıcının diğer değerler sağlamak için ihtiyacımız: kullanıcı adı, tarihi, sona erme ve benzeri. Oturum açma sayfasında önceki öğreticide oluşturduğumuz, bu tüm bizim için FormsAuthentication sınıfı tarafından işlenmiş. UserData için FormsAuthenticationTicket eklerken, biz zaten FormsAuthentication sınıfı tarafından sağlanan işlevlerinin çoğunu çoğaltmak için kod yazmanız gerekir.

Kullanıcı kimlik doğrulaması bileti için ilgili ek bilgileri kaydetmek için Login.aspx sayfasına güncelleştirerek UserData ile çalışmak için gerekli kodu araştıralım. Kullanıcı mağazamız kullanıcının çalıştığı şirketin ve bunların title hakkında bilgi içerir ve bu bilgileri kimlik doğrulaması bileti yakalamak istiyoruz anlatabilirsiniz. Bu ancak sayfa Oturum Aç düğmesini tıklama olayı işleyicisi güncelleştirin, böylece kod aşağıdaki gibi görünür:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample6.vb)]

Şimdi bu kod bir çizgi aynı anda adım. Yöntemi dört dize diziler tanımlayarak başlatır: kullanıcılar, parolalar, şirket adı ve titleAtCompany. Bu dizileri kullanıcı adları, parolalar, şirket adı ve başlıkları için kullanıcı hesaplarını bir sistemde, üç var olan tutun: Scott Jisun ve Sam. Gerçek bir uygulamada, kullanıcı mağazadan sayfanın kaynak kodunda kodlanmış değil, bu değerleri seçmeleri.

Varsa sağlanan kimlik bilgilerinin geçerli ve önceki öğreticide biz yalnızca aşağıdaki adımları gerçekleştirilen FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked) çağrılır:

1. Forms kimlik doğrulaması bileti oluşturuldu
2. Bilet uygun deponun yazıldı. Tanımlama bilgileri tabanlı kimlik doğrulama biletlerini tarayıcının tanımlama bilgileri koleksiyonu kullanılır. cookieless kimlik doğrulama biletlerini URL'de bilet verilerini seri
3. Kullanıcı uygun sayfasına yönlendirilirsiniz

Bu adımlar yukarıdaki kodda çoğaltılır. İlk olarak, şirket adı ve iki değer içeren bir dikey çizgi (|) ayıran başlık birleştirerek UserData özelliğinde sonunda depolarız dize oluşturulur.

Dize olarak userDataString dim String.Concat(companyName(i), = "|", titleAtCompany(i))

Ardından, kimlik doğrulaması bileti oluşturur, yöntemi çağrılır, FormsAuthentication.GetAuthCookie şifreler ve yapılandırma ayarlarına göre doğrular ve HttpCookie nesneyi yerleştirir.

AuthCookie olarak HttpCookie dim FormsAuthentication.GetAuthCookie (UserName.Text, RememberMe.Checked) =

Tanımlama bilgisi içinde gömülü FormAuthenticationTicket çalışmak için FormAuthentication sınıfın çağrılacak gerekiyor [şifresini yöntemi](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), tanımlama bilgisi değeri içinde geçen.

Bilet olarak FormsAuthenticationTicket dim FormsAuthentication.Decrypt(authCookie.Value) =

Ardından oluştururuz bir *yeni* FormsAuthenticationTicket örnek tabanlı mevcut FormsAuthenticationTicket'ın değerleri. Ancak, bu yeni bir biletle (userDataString) kullanıcıya özgü bilgileri içerir.

NewTicket olarak FormsAuthenticationTicket dim yeni FormsAuthenticationTicket(ticket. = Sürüm, anahtar. Anahtar adı. İssueDate, bilet. Süre sonu, anahtar. IsPersistent, userDataString)

Biz şifreleme (sonra doğrulama) çağırarak yeni FormsAuthenticationTicket örneği [yöntemi](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx), bu şifrelenmiş (ve doğrulanmış) verileri geri authCookie yerleştirin.

authCookie.Value FormsAuthentication.Encrypt(newTicket) =

Son olarak, authCookie Response.Cookies koleksiyona eklenir ve GetRedirectUrl yöntem, kullanıcının göndermek için uygun bir sayfayı belirlemek için çağrılır.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample7.vb)]

Bu kod tüm UserData özelliği salt okunur olduğundan ve FormsAuthentication sınıfı GetAuthCookie, SetAuthCookie veya RedirectFromLoginPage metotlarını UserData bilgileri belirtmek için herhangi bir yöntem sağlamaz gereklidir.

> [!NOTE]
> Yalnızca incelenen kod kullanıcıya özgü bilgileri bir tanımlama bilgisi tabanlı kimlik doğrulaması bileti depolar. Forms kimlik doğrulaması bileti URL'sine serileştirmek için sorumlu .NET Framework iç sınıflardır. Yazıyı kısa, kullanıcı verilerini bir tanımlama bilgisi olmayan formlar kimlik doğrulaması bileti depolayamaz.


### <a name="accessing-the-userdata-information"></a>UserData bilgilerine erişme

Bu noktada her kullanıcının şirket adı ve başlık depolanan forms kimlik doğrulaması bileti 's UserData özelliğinde oturum açtığında. Bu bilgiler kullanıcı deposuna bir seyahat gerek kalmadan herhangi bir sayfasında kimlik doğrulaması bileti erişilebilir. Bu bilgiler UserData özelliğinden alınabilir nasıl göstermek için Hoş Geldiniz iletisi yalnızca kullanıcının adını, ancak ayrıca şirket için çalışmayan ve kendi başlık içerir, böylece Default.aspx güncelleştirelim.

Şu anda Default.aspx AuthenticatedMessagePanel paneli WelcomeBackMessage adlı bir etiket denetimi içerir. Bu panoya yalnızca kimliği doğrulanmış kullanıcılara görüntülenir. Kodu Default.aspx'ın sayfasında güncelleştirme\_aşağıdaki gibi görünür, böylece olay işleyicisi yükleyin:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample8.vb)]

Request.IsAuthenticated True olduğu sonra WelcomeBackMessage'nın metin özelliği ilk kez tekrar Hoş Geldiniz ayarlanmış *username*. Ardından, biz temel FormsAuthenticationTicket erişebilmesi User.Identity özelliği FormsIdentity nesnesine türüne dönüştürülür. Biz FormsAuthenticationTicket aldıktan sonra biz UserData özelliği şirket adı ve başlığı seri durumdan çıkarır. Bu, dikey çizgi karakterinden dizesine bölerek elde edilir. Ardından şirket adı ve başlığı WelcomeBackMessage etikette görüntülenir.

Şekil 5 eylemi bu ekran görüntüsü gösterilmektedir. Scott oturum açmayı Scott'ın şirket ve başlık içeren geri bir karşılama iletisi görüntüler.


[![Şu anda oturum açan kullanıcının şirket ve başlık görüntülenir.](forms-authentication-configuration-and-advanced-topics-vb/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image13.png)

**Şekil 05**: şu anda oturum açan kullanıcının şirket ve başlık görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image15.png))


> [!NOTE]
> Kimlik doğrulama anahtarı'nın UserData özelliği, kullanıcı deposu için bir önbellek olarak görev yapar. Herhangi bir önbellek gibi temel alınan verileri değiştirildiğinde güncelleştirilmesi gerekir. Örneğin, kullanıcı profillerini güncelleştirebilirsiniz bir web sayfası varsa, kullanıcı tarafından yapılan değişiklikleri yansıtacak şekilde UserData özelliğinde önbelleğe alan yenilenmelidir.


## <a name="step-5-using-a-custom-principal"></a>5. adım: özel asıl kullanma

Her gelen isteğe göre FormsAuthenticationModule kullanıcının kimliğini doğrulamak çalışır. Bir kimlik doğrulama süresi anahtarı varsa, FormsAuthenticationModule HttpContext.User özelliği yeni GenericPrincipal nesnesine atar. Bu GenericPrincipal nesne, forms kimlik doğrulaması bileti için bir başvuru içeren FormsIdentity türünde bir kimliğe sahiptir. GenericPrincipal sınıfı IPrincipal uygulayan bir sınıf tarafından gerekli en düşük tam işlevselliğini içeren - yalnızca bir kimlik özelliği ve IPrincipal yöntemi vardır.

Asıl nesne iki sorumlulukları vardır: kullanıcının ait olduğu için hangi rollerin belirtmek ve kimlik bilgilerini sağlamak için. Bu IPrincipal arabirimin IPrincipal gerçekleştirilir (*roleName*) yöntemi ve kimlik özelliği, sırasıyla. GenericPrincipal sınıfı için rol adlarının dize dizisi oluşturucusuna belirtilmesini sağlar; kendi IPrincipal (*roleName*) yöntemi yalnızca denetler olmadığını görmek için geçirilen içinde *roleName* dize dizisi içinde bulunmaktadır. GenericPrincipal FormsAuthenticationModule oluşturduğu zaman, bir boş dize dizisinde GenericPrincipal'ın oluşturucuya geçirir. Sonuç olarak, tüm çağrıların IPrincipal için her zaman False döndürür.

GenericPrincipal sınıfı rolleri değil kullanıldığı çoğu form tabanlı kimlik doğrulama senaryoları için gereksinimlerini karşılar. Varsayılan rol işleme olduğu yetersiz bu durumlar için veya bir özel IIdentity nesnesi kullanıcıyla ilişkilendirmek gerektiğinde kimlik doğrulama iş akışı sırasında özel bir IPrincipal nesnesi oluşturabilir ve HttpContext.User özelliğine atayın.

> [!NOTE]
> Öğreticiler, gelecekte göreceğiz olarak, ASP. NET rolleri framework etkin türünde bir özel asıl nesnesi oluşturur [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) ve forms kimlik doğrulaması oluşturulan GenericPrincipal nesnenin üzerine yazar. Rolleri framework'ün API ile arabirim oluşturmak için IPrincipal yöntemi sorumlunun özelleştirmek için bunu yapar.


Biz kendimize rolleriyle henüz endişe olmayan olduğundan, biz juncture en bu özel bir kural oluşturmak için sahip tek nedeni bir özel asıl IIdentity nesnesine ilişkilendirilecek olacaktır. Adım 4'te ek kullanıcı bilgileri kimlik doğrulaması bileti 's UserData özelliğinde belirli kullanıcının şirket adını ve bunların başlık depolama konumunda incelemiştik. Ancak, UserData yalnızca kimlik doğrulaması bileti üzerinden erişilebilir ve bilet depolanan kullanıcı bilgileri görüntülemek dilediğiniz UserData özelliği ayrıştırılamıyor ihtiyacımız, yani seri hale getirilmiş bir dize olarak sonra yalnızca bilgilerdir.

IIdentity uygulayan ve CompanyName ve başlık özellikleri içeren bir sınıfı oluşturarak Geliştirici deneyimini geliştirebiliriz. Bu şekilde, başlığı doğrudan CompanyName ve başlık özellikleri aracılığıyla nasıl ayrıştıracağını UserData özelliği için gereken ve bir geliştirici o anda oturum açmış kullanıcının şirket adını erişebilirsiniz.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Özel kimlik ve asıl sınıfları oluşturma

Bu öğreticide, özel asıl ve kimlik nesneleri uygulamada oluşturalım\_kod klasörü. Başlangıç uygulama ekleyerek\_kod projeniz klasörü - Çözüm Gezgini'nde proje adının üzerine sağ tıklayın, ASP.NET klasörü Ekle seçeneğini belirleyin ve uygulamayı seçin\_kod. Uygulama\_kod klasördür sınıfı Web sitesine belirli dosyaları içeren özel bir ASP.NET klasörü.

> [!NOTE]
> Uygulama\_kod klasörü, yalnızca proje Web sitesi proje modeli aracılığıyla yönetirken kullanılmalıdır. Kullanıyorsanız [Web uygulaması proje modeli](https://msdn.microsoft.com/asp.net/Aa336618.aspx), standart bir klasör oluşturun ve sınıfları ekleyin. Örneğin, sınıf adlı yeni bir klasör ekleyin ve kodunuzu buraya getirin.


Ardından, uygulamaya iki yeni sınıf dosyaları ekleme\_kod klasörü, bir adlandırılmış CustomIdentity.vb ve bir adlı CustomPrincipal.vb.


[![CustomPrincipal sınıfları ve CustomIdentity projenize ekleyin.](forms-authentication-configuration-and-advanced-topics-vb/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image16.png)

**Şekil 06**: CustomIdentity ve CustomPrincipal sınıfları için projenize ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image18.png))


AuthenticationType ısauthenticated durumunda olmasını gerektirir ve ad özelliklerini tanımlar IIdentity arabirimini uygulamak için sorumlu CustomIdentity sınıftır. Gerekli özelliklere ek olarak temel forms kimlik doğrulaması bileti yanı sıra kullanıcının şirket adı ve başlık özellikleri kullanıma sunmak istiyorsanız duyuyoruz. CustomIdentity sınıfına aşağıdaki kodu girin.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample9.vb)]

Sınıf FormsAuthenticationTicket üye değişkeni içerdiğini unutmayın (\_bileti) ve bu bilet bilgilerini Oluşturucu üzerinden sağlanmalıdır. Bu bilet verileri kimliğin adını döndüren içinde kullanılır. UserData özelliği CompanyName ve başlık özellikleri için değer döndürmek için ayrıştırılır.

Ardından, CustomPrincipal sınıfı oluşturun. Biz juncture en bu rolleri ile ilgili bir kaygınız olduğundan, CustomPrincipal sınıfın Oluşturucusu yalnızca CustomIdentity nesne kabul eder; kendi IPrincipal yöntemi her zaman false değerini döndürür.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample10.vb)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Gelen isteğin güvenlik bağlamına CustomPrincipal nesne atama

Artık özel kimlik kullanan özel bir asıl sınıf yanı sıra, şirket adı ve başlık özellikleri eklemek için varsayılan IIdentity belirtimi genişleten bir sınıfı var. Biz ASP.NET ardışık düzenini adımla hazır ve özel bizim asıl nesne gelen isteğin güvenlik bağlamına atayın.

ASP.NET ardışık bir gelen isteği alır ve bir dizi adımı üzerinden işler. Her adımda ASP.NET ardışık düzende dokunun ve yaşam döngüsü içinde bazı noktalarda isteği, geliştiricilerin çözmelerine belirli bir olay tetiklenir. FormsAuthenticationModule yükseltmek ASP.NET gibi bekler [AuthenticateRequest olay](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), isteğe bağlı olarak, bu noktada bir kimlik doğrulaması bileti için gelen isteği inceler. Bir kimlik doğrulaması bileti bulunursa GenericPrincipal nesnesi oluşturulur ve HttpContext.User özelliğine atanır.

ASP.NET ardışık düzenini AuthenticateRequest olayından sonra başlatır [PostAuthenticateRequest olay](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), biz örneğiyle birlikte FormsAuthenticationModule tarafından oluşturulan GenericPrincipal nesne burada değiştirin olan bizim CustomPrincipal nesnesi. Şekil 7, bu iş akışı gösterilmektedir.


[![GenericPrincipal PostAuthenticationRequest olayda bir CustomPrincipal değiştirilir](forms-authentication-configuration-and-advanced-topics-vb/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image19.png)

**Şekil 07**: GenericPrincipal PostAuthenticationRequest olayda bir CustomPrincipal değiştirilir ([tam boyutlu görüntüyü görmek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image21.png))


ASP.NET ardışık düzen olaya yanıt kodu yürütmek için size uygun bir olay işleyicisi Global.asax'ta oluşturabilir veya kendi HTTP modülü oluşturun. Bu öğretici için olay işleyicisi Global.asax'ta oluşturalım. Global.asax Web sitenize ekleyerek başlayın. Çözüm Gezgini'nde proje adının üzerine sağ tıklayın ve genel uygulama sınıfı Global.asax adlı türünde bir öğe ekleyin.


[![Global.asax dosyası Web sitenize ekleyin](forms-authentication-configuration-and-advanced-topics-vb/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image22.png)

**Şekil 08**: Web siteniz için Global.asax dosyası ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](forms-authentication-configuration-and-advanced-topics-vb/_static/image24.png))


Olay işleyicileri birkaç başlangıç, bitiş gibi ASP.NET ardışık etkinlikler için varsayılan Global.asax şablonu içerir ve [hata olayı](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), diğerlerinin yanı sıra. Bu olay işleyicilerini kaldırmak bu uygulama için ihtiyacımız yok olarak çekinmeyin. İlgileniriz PostAuthenticateRequest etkinliğidir. Kendi biçimlendirme aşağıdakine benzer şekilde, Global.asax dosyanızı güncelleştirin:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample11.aspx)]

Uygulama\_OnPostAuthenticateRequest ASP.NET çalışma zamanı oluşturur gelen her sayfa isteğinde bir kez gerçekleşir PostAuthenticateRequest olay her zaman yürütülür. Olay işleyicisi, kullanıcı kimlik doğrulaması ve form kimlik doğrulaması doğrulanmış olduğunu görmek için denetleyerek başlar. Bu durumda, yeni bir CustomIdentity nesnesi oluşturulur ve geçerli isteğin kimlik doğrulaması bileti oluşturucusunda geçirildi. CustomPrincipal nesne oluşturulur ve yeni oluşturulan CustomIdentity nesne oluşturucusunda geçirildi. Son olarak, geçerli isteğin güvenlik bağlamı için yeni oluşturulan CustomPrincipal nesnesinde atanır.

Not - CustomPrincipal nesne isteğin güvenlik bağlamı ile ilişkilendirme - son adım, iki özellik için asıl atar: HttpContext.User ve Thread.CurrentPrincipal. Bu iki atamaları, güvenlik kapsamları, ASP.NET'te işlenme şekli nedeniyle gereklidir. .NET Framework güvenlik bağlamı, her bir çalışan iş parçacığı ile ilişkilendirir; Bu bilgiler IPrincipal nesneyle olarak kullanılabilir [iş parçacığı nesnesi](https://msdn.microsoft.com/library/system.threading.thread.aspx)'s [CurrentPrincipal özelliği](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). Bir kafa karıştırıcı nedir, ASP.NET, kendi güvenlik bağlamı bilgilerini (HttpContext.User) sahip olur.

Güvenlik bağlamı belirlerken, belirli senaryolarda Thread.CurrentPrincipal özelliği incelenir; Diğer senaryolarda HttpContext.User kullanılır. Örneğin, hangi kullanıcıların bildirimli olarak durumuna geliştiricilerin .NET güvenlik özellikleri vardır veya rolleri, bir sınıf örneği başlatmanız veya belirli metotları çağırma (bkz [iş ve veri katmanları kullanarak Yetkilendirme kuralları ekleme PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Aslında, bildirim temelli bu teknikler Thread.CurrentPrincipal özelliği aracılığıyla güvenlik bağlamı belirleyin.

Diğer senaryolarda HttpContext.User özelliği kullanılır. Örneğin, önceki öğreticide Biz bu özellik şu anda oturum açmış kullanıcının kullanıcı adını görüntülemek için kullanılır. NET bir şekilde, daha sonra güvenlik bağlamını Thread.CurrentPrincipal ve HttpContext.User özelliklerinde eşleştiğini olmazsa olmazdır.

ASP.NET çalışma zamanı bu özellik değerlerini bizim için otomatik olarak eşitler. Ancak, bu eşitleme AuthenticateRequest olayından sonra gerçekleşir ancak *önce* PostAuthenticateRequest olay. Sonuç olarak, bir özel kural PostAuthenticateRequest olayda eklerken Thread.CurrentPrincipal veya başka Thread.CurrentPrincipal el ile atama emin olmak ihtiyacımız ve HttpContext.User eşitlenmemiş. Bkz: [Context.User vs. Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) bu sorunla ilgili daha ayrıntılı bir açıklaması için.

### <a name="accessing-the-companyname-and-title-properties"></a>Başlık özellikleri ve CompanyName erişme

Her bir istek ulaştığında ve uygulama ASP.NET Altyapısı'na dağıtılan\_OnPostAuthenticateRequest olay işleyicisi içindeki yangın. İstek tarafından FormsAuthenticationModule başarıyla doğrulandı, olay işleyicisi, forms kimlik doğrulaması bileti üzerinde temel CustomIdentity nesnesi ile bir yeni CustomPrincipal nesnesi oluşturacak. Bu mantık yerde o anda oturum açmış kullanıcının şirket adı ve başlık bilgilerine erişme son derece basittir.

Sayfaya dönmek\_yük olay işleyicisinde Default.aspx burada adım 4'te yazdığımız kod form kimlik doğrulaması bileti almak ve kullanıcının şirket adı ve başlık görüntülemek için UserData özelliği ayrıştırılamıyor. Artık kullanımda CustomPrincipal ve CustomIdentity nesneleriyle anahtar UserData özelliği dışında değerleri gerek yoktur. Bunun yerine, yalnızca CustomIdentity nesnesine bir başvuru almak ve şirket adı ve başlık özelliklerini kullanın:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample12.vb)]

## <a name="summary"></a>Özet

Bu öğreticide size forms kimlik doğrulaması sistemin ayarları Web.config aracılığıyla nasıl özelleştirilir incelenir. Kimlik doğrulama anahtarı'nın süre sonu nasıl işlendiğini ve şifreleme ve doğrulama korumaları bileti denetleme ve değiştirme korumak için nasıl kullanılacağını inceledik. Son olarak, anahtar ek kullanıcı bilgileri ve bu bilgileri daha Geliştirici dostu bir biçimde kullanıma sunmak için özel asıl ve kimlik nesneleri kullanma depolamak için kimlik doğrulama anahtarı'nın UserData özelliği kullanılarak ele almıştık.

Bu öğretici, ASP.NET formları kimlik doğrulaması bizim incelenmesi sona eriyor. Sonraki öğreticiye yolculuğumuz üyelik framework uygulamasına başlatır.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Form kimlik doğrulaması ayrıntıları](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Açıklanmıştır: ASP.NET 2.0 formları kimlik doğrulama](https://msdn.microsoft.com/library/aa480476.aspx)
- [Nasıl yapılır: ASP.NET 2.0 form kimlik doğrulamasını korumak](https://msdn.microsoft.com/library/ms998310.aspx)
- [Profesyonel ASP.NET 2.0 güvenlik, üyelik ve rol yönetimi](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Oturum açma denetimleri güvenliğini sağlama](https://msdn.microsoft.com/library/ms178346.aspx)
- [&lt;Kimlik doğrulaması&gt; öğesi](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;Forms&gt; öğesi için &lt;kimlik doğrulaması&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [&lt;MachineKey&gt; öğesi](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Tanımlama bilgisi ve Forms kimlik doğrulaması bileti anlama](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Bu öğreticide yer alan konularda eğitim videosu

- [Forms kimlik doğrulaması özelliklerini değiştirme](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Nasıl bir ASP.NET uygulamasında tanımlama bilgisiz kimlik kurulumu ve kullanımı](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [ASP Forms Oturum Açmayı Yeniden Konumlandırma](../../../videos/authentication/asp-forms-login-relocation.md)
- [Forms Oturum Açma Özel Anahtar Yapılandırması](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Kimlik Doğrulama Metoduna Özel Veri Ekleme](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Özel Asıl Nesneler Kullanma](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET Books yazar ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan  *[Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott, konumunda ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Alicja Maziarz oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Önceki](an-overview-of-forms-authentication-vb.md)
