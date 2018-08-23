---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: ASP.NET MVC ve Web sayfalarında XSRF/CSRF önleme | Microsoft Docs
author: Rick-Anderson
description: Siteler arası istek sahteciliği (XSRF veya CSRF olarak da bilinir), barındırılan web uygulamaları kötü amaçlı bir web sitesi Etkileşi yapabildiği etkileyebilir karşı bir saldırı olma...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: cd1b8de51c180471ab273c4541959368ffbd48a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755182"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>ASP.NET MVC ve Web sayfalarında XSRF/CSRF önleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Siteler arası istek sahteciliği (XSRF veya CSRF olarak da bilinir) barındırılan web uygulamaları kötü amaçlı bir web sitesi istemci tarayıcısına ve bu tarayıcı tarafından güvenilen bir web sitesi arasındaki etkileşimi yapabildiği etkileyebilir karşı bir saldırıdır. Web tarayıcıları bir web sitesi için kimlik doğrulama belirteçlerinizi her istek ile otomatik olarak gönderir, çünkü bu saldırıların yapılabilir. ASP gibi bir kimlik doğrulama tanımlama bilgisi kurallı örnektir. NET form kimlik doğrulaması bileti. Ancak, tüm kalıcı kimlik doğrulama mekanizması (örneğin, Windows kimlik doğrulaması, temel ve diğerleri) kullanan web siteleri tarafından bu saldırıların hedefleyebilir.
> 
> Bir kimlik avı saldırıdan XSRF saldırının farklıdır. Kimlik avı saldırıları, victim etkileşimini gerektirir. Bir kimlik avı saldırı kötü amaçlı bir siteyi hedef web sitesi benzetimini yapacak ve victim saldırgan için hassas bilgileri sağlayarak içine sizi kandırmalarına. XSRF saldırısında, genellikle bir etkileşim yoktur victim gerekli. Bunun yerine, tüm ilgili tanımlama bilgilerini hedef web sitesine otomatik olarak göndermeye tarayıcıda saldırgan kullanmaktadır.
> 
> Daha fazla bilgi için [açık Web uygulaması güvenlik projesi](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Bir saldırı anatomisi

XSRF saldırı yürütmek için bazı çevrimiçi bankacılık işlemler gerçekleştirmek isteyen bir kullanıcı göz önünde bulundurun. Bu kullanıcı, kendi kimlik doğrulama tanımlama bilgisi içerir, bu noktada yanıt üst bilgisi WoodgroveBank.com ve günlükleri, ilk ziyaret:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Kimlik doğrulama tanımlama oturum tanımlama bilgisi olduğundan, tarayıcı işlem çıktığında bunu otomatik olarak tarayıcı tarafından temizlenir. Ancak, o zamana kadar tarayıcının otomatik olarak tanımlama bilgisi ile her isteğin WoodgroveBank.com içerecektir. Kullanıcı artık Filiz bankacılık sitesinde formunu doldurur ve tarayıcı bu sunucuya istekte başka bir hesaba, 1000 ABD Doları aktarmak istiyor:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Bu işlem (parasal bir işlem başlatır) bir yan etkisi olduğundan, bu işlemi başlatmak için bir HTTP POST gerektirecek şekilde bankacılık site seçti. Sunucu istekten kimlik doğrulama belirteci okur, geçerli kullanıcının hesabı arar, fon var ve ardından hedef hesabını harekete başlatır doğrular.

Her çevrimiçi tam bankacılık, kullanıcı bankacılık siteyi uzağa gider ve başka konumlara Web'de ziyaret eder. Bu sitelerin – fabrikam.com – birini içeren aşağıdaki biçimlendirme içinde katıştırılmış bir sayfada bir &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Daha sonra bu istekte bulunmak tarayıcı neden olur:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

Saldırgan, kullanıcı yine de bir hedef web sitesi için geçerli bir kimlik doğrulama belirteci sahip olabilir ve tarayıcı bir HTTP POST hedef siteye otomatik hale getirmek neden Filiz küçük bir kod parçacığı JavaScript kullanıyor olgu kullanıyordur. Kimlik doğrulama belirteci hala geçerliyse, bankacılık site bir saldırganın seçme hesaba 250 $ aktarımını başlatır.

### <a name="ineffective-mitigations"></a>Etkisiz risk azaltma işlemleri

Yukarıdaki senaryoda WoodgroveBank.com SSL erişilen ve bir SSL-yalnızca kimlik doğrulama tanımlama bilgisi vardı olgu saldırı ihlalini savuşturmanın yetersiz olduğunu unutmayın ilginçtir. Saldırgan belirtebilirsiniz [URI şeması](http://en.wikipedia.org/wiki/URI_scheme) (https), her &lt;form&gt; öğesi ve tarayıcısı bu tanımlama bilgileri URI'sı ile uyumlu olduğu sürece, süresi dolmamış tanımlama bilgilerini hedef siteye göndermek sürdürür Hedef düzeni.

Bir kullanıcı yalnızca güvenilen siteler yalnızca çevrimiçi güvenli kalmasına yardımcı olarak ziyaret güvenilmeyen siteleri ziyaret etmeniz gerekir değil, buna. Bu bazı truth yoktur, ancak ne yazık ki bu öneri her zaman pratik olmaz. Belki de kullanıcı "Yerel Haberler site ConsolidatedMessenger güvenir". ConsolidatedMessenger.com ve bunun yerine sitesini ziyaret edin, ancak o siteye giden bir saldırganın aynı fabrikam.com üzerinde çalışan kod parçacığını eklemesine olanak tanıyan bir XSS Güvenlik açığı vardır.

Gelen istekleri sahip olduğunuzu doğrulayabilirsiniz bir [Referer üst bilgisi](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) etki alanınızı başvuruyor. Bu, bir üçüncü taraf etki alanından istemeden gönderilen istekleri durdurur. Ancak, bazı kişiler, tarayıcının Referer üst bilgisi gizlilik nedeniyle devre dışı bırakın ve saldırganlar kurbanın güvensiz belirli yazılımların yüklü varsa bazen bu başlığı davranabilir. Doğrulama [Referer üst bilgisi](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) XSRF saldırılarını önleme güvenli bir yaklaşım olarak kabul edilmez.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Web yığın çalışma zamanı XSRF risk azaltma işlemleri

Bir ASP.NET Web yığını çalışma zamanı kullanan [Eşitleyici belirteci deseni](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) XSRF saldırılarına karşı korumak için. İki anti-XSRF belirteci sunucu (ek kimlik doğrulama belirteci) her HTTP POST ile gönderilen Eşitleyici belirteci deseni genel formu şöyledir: bir tanımlama bilgisi ve diğer bir form değeri olarak bir belirteç. ASP.NET çalışma zamanı tarafından oluşturulan belirteci değerleri, belirleyici veya saldırgan tarafından tahmin edilebilir değildir. Belirteçleri gönderildiğinde sunucusu isteğin yalnızca her iki simge bir karşılaştırma denetimi başarılı olursa izin verir.

XSRF istek doğrulamayı *Oturum belirteci* bir HTTP tanımlama bilgisi depolanır ve şu anda yük aşağıdakileri içerir:

- Bir rastgele 128-bit tanıtıcısı oluşan bir güvenlik belirteci.   
 Internet Explorer F12 Geliştirici araçlarıyla görüntülenen XSRF istek doğrulama Oturum belirteci aşağıdaki resimde gösterilmektedir: (Bu geçerli bir uygulamasıdır ve konu Not değiştirmek bile olabilir.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*Token pole* olarak depolanan bir `<input type="hidden" />` ve yük aşağıdakileri içerir:

- Oturum açan kullanıcının kullanıcı adı (kimlik doğrulaması gerekiyorsa).
- Tarafından sağlanan herhangi bir ek veriyi bir [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Anti-XSRF belirteçlerin yükü şifrelenir ve imzalandı, belirteçleri incelemek için araçları kullanılırken kullanıcı adı görüntüleyemezsiniz şekilde. Web uygulamasını ASP.NET 4.0 hedeflenirken Şifreleme Hizmetleri tarafından sağlanan [MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) yordamı. Ne zaman web uygulamasına ASP.NET 4.5 hedeflediği ya da daha yüksek, Şifreleme Hizmetleri tarafından sağlanan [MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) daha iyi performans, genişletilebilirlik ve güvenlik sağlayan yordamı. Daha fazla ayrıntı için şu blog gönderilerini bakın:

- [ASP.NET 4.5 şifreleme geliştirmeleri, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [ASP.NET 4.5 şifreleme geliştirmeleri, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [ASP.NET 4.5 şifreleme geliştirmeleri, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Belirteçleri oluşturmak

Anti-XSRF belirteçleri oluşturmak için çağrı [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) bir MVC görünümü yönteminden veya @AntiForgery.GetHtmlbir Razor sayfası (). Çalışma zamanı, ardından aşağıdaki adımları gerçekleştireceksiniz:

1. Geçerli HTTP isteği bir anti-XSRF Oturum belirteci içeriyorsa (anti-XSRF tanımlama bilgisi \_ \_RequestVerificationToken), güvenlik belirteci ondan ayıklanır. HTTP isteği bir anti-XSRF Oturum belirteci içermiyorsa veya güvenlik belirtecinin ayıklama başarısız olursa, yeni bir rastgele anti-XSRF belirteci oluşturulur.
2. Anti-XSRF token pole adım (1) yukarıdaki ve geçerli oturum açmış kullanıcının kimliğini güvenlik belirteci kullanılarak oluşturulur. (Kullanıcı kimliğini belirleme hakkında daha fazla bilgi için bkz. **[özel destek senaryolarıyla](#_Scenarios_with_special)** bölümüne bakın.) Ayrıca, bir [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) olan yapılandırılmış, çalışma zamanı çağıracak kendi [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) yöntemi ve döndürülen dize alanı içermesi. (Bkz **[yapılandırma ve genişletilebilirlik](#_Configuration_and_extensibility)** bölümünde daha fazla bilgi için.)
3. Adım (1) yeni bir anti-XSRF belirteci üretilmişse, yeni oturum belirteci içerdiği için oluşturulacak ve giden HTTP tanımlama bilgileri koleksiyona eklenir. Token pole adım (2) içinde sarmalanmış bir `<input type="hidden" />` öğesi ve bu HTML biçimlendirmeyi dönüş değeri olacak `Html.AntiForgeryToken()` veya `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Belirteçleri doğrulama

Anti-XSRF gelen belirteçleri doğrulamak, geliştirici içeren bir [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) kendi MVC eylem veya denetleyici veya SEH çağrıları özniteliği `@AntiForgery.Validate()` kendi Razor sayfası. Çalışma zamanı, aşağıdaki adımları gerçekleştireceksiniz:

1. Token pole ve gelen oturum belirteci okuma ve her birinden anti-XSRF belirteci ayıklanır. Anti-XSRF belirteçleri oluşturma yordamı (2) adımda başına aynı olmalıdır.
2. Geçerli kullanıcı kimlik doğrulaması gerekiyorsa, her kullanıcı adı alanı belirteci depolanan kullanıcı adı ile karşılaştırılır. Kullanıcı adlarıyla eşleşmelidir.
3. Varsa bir [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) yapılandırılır, çalışma zamanı çağrıları kendi *ValidateAdditionalData* yöntemi. Yöntemi, Boole değeri döndürmelidir *true*.

Doğrulama başarılı olursa, isteği devam etmesine izin verilir. Doğrulama başarısız olursa, framework oluşturmaz bir *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Hata koşulları

ASP.NET Web yığını çalışma zamanı v2 ile tüm başlangıç *HttpAntiForgeryException* sırasında oluşturulan doğrulama neyin yanlış gittiğini hakkında ayrıntılı bilgi içerir. Şu anda tanımlı hata koşulları şunlardır:

- Oturum belirteci veya form belirteci istekte mevcut değil.
- Oturum belirteci veya form belirteci okunamaz durumda. Bunun en olası nedeni, ASP.NET Web yığını çalışma zamanı veya grubunu eşleşmeyen sürümlerini çalıştıran bir grup olduğu yere &lt;machineKey&gt; Web.config öğesinde makineler arasında farklılık gösterir. Bu özel durum ya da anti-XSRF belirteci ile kurcalama tarafından zorlamak için Fiddler gibi bir araç kullanabilirsiniz.
- Token pole ve oturum belirteci değiştirildi.
- Oturum belirteci ve token pole eşleşmeyen güvenlik belirteçleri bulunur.
- Token pole içinde gömülü kullanıcı adı, geçerli oturum açmış kullanıcının kullanıcı adı eşleşmiyor.
- *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* yöntemi döndürülen *false*.

Anti-XSRF özellikleri, ayrıca belirteç oluşturma veya doğrulama sırasında ek denetimi gerçekleştirebilir ve hataları bu denetimler sırasında oluşturulan özel durumlar neden olabilir. Bkz: [WIF / ACS / talep tabanlı kimlik doğrulaması](#_WIF_ACS) ve **[yapılandırma ve genişletilebilirlik](#_Configuration_and_extensibility)** bölümlerde daha fazla bilgi için.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Özel destek senaryoları

### <a name="anonymous-authentication"></a>Anonim kimlik doğrulama

Anti-XSRF sistem burada "anonim" tanımlı bir kullanıcı olarak, anonim kullanıcılar için özel desteği içerir. burada *IIdentity.IsAuthenticated* özelliği döndürür *false*. Senaryolar içerir (kullanıcının kimliğinin önce) oturum açma sayfası ve burada uygulamanın kullandığı bir mekanizma dışında özel kimlik doğrulama düzenleri XSRF koruma sağlayan *IIdentity* kullanıcıları tanımlamak için.

Bu senaryoları desteklemek için oturum ve alan belirteçleri bir 128-bit rastgele oluşturulmuş donuk tanımlayıcısı bir güvenlik belirteci tarafından katıldığını hatırlayın. Bu güvenlik belirteci, etkili bir şekilde amacı anonim bir tanımlayıcı, hizmet için o site gider gibi tek bir kullanıcının oturumunu izlemek için kullanılır. Boş bir dize yerine bir kullanıcı adı, yukarıda açıklanan oluşturma ve doğrulama rutinleri için kullanılır.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS / talep tabanlı kimlik doğrulaması

Normalde, *IIdentity* sınıfları .NET Framework'e yerleşik olarak sahip özellik, *IIdentity.Name* belirli bir kullanıcı belirli bir uygulama içinde benzersiz olarak tanımlanabilmesi yeterlidir. Örneğin, *FormsIdentity.Name* (Bu, veritabanı bağlı olarak tüm uygulamalar için benzersiz olan) üyelik veritabanında depolanan kullanıcı döndürür *WindowsIdentity.Name* döndürür tam etki alanı kimlik kullanıcı ve benzeri. Bu sistemler yalnızca kimlik doğrulaması; sağlayın Bunlar ayrıca *tanımlamak* kullanıcılara bir uygulama.

Talep tabanlı kimlik doğrulaması, diğer taraftan, mutlaka belirli bir kullanıcı olup olmadığınızı belirlemek gerektirmez. Bunun yerine, *ClaimsPrincipal* ve *Claimsıdentity* türleridir kümesiyle ilişkili *talep* örnekleri, burada bireysel talepleri olabilir "18 + yaşında bir" veya " Yönetici diğer her şey için olan". Kullanıcı mutlaka tanımlanan taşınmadığından olduğundan, çalışma zamanı kullanamazsınız *ClaimsIdentity.Name* özelliği belirli bir kullanıcı için benzersiz bir tanımlayıcı olarak. Takım, gerçek dünyadan örnekler yakaladı burada *ClaimsIdentity.Name* döndürür *null*(görüntüleme) kolay adı döndürür veya benzersiz bir tanımlayıcı olarak kullanım için uygun olmayan bir dize döndürür kullanıcı için.

Talep tabanlı kimlik doğrulaması kullanan dağıtımların çoğu kullanarak [Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) özellikle. ACS yapılandırmak Geliştirici bireysel sağlayan *kimlik sağlayıcıları* (Microsoft Account sağlayıcısı, ADFS gibi Openıd sağlayıcılarının ister Yahoo!, vb.), ve kimlik sağlayıcılarını döndüren *tanımlayıcılarıadı*. Bir özel kişisel tanımlayıcı (PPID gibi) anonimleştirilmesini veya bu adı tanımlayıcılar bir e-posta adresi gibi kişisel bilgilerin (PII) içerebilir. ASP.NET Web yığını çalışma zamanı kullanıcı adı yerine bir tanımlama grubu oluşturulurken kullanabilmeniz için o site gözatması sırada ne olursa olsun, tanımlama grubu (Kimlik sağlayıcısı, ad tanımlayıcısı) yeterince belirli bir kullanıcı için bir uygun İzleme belirteci görür ve Anti-XSRF alan belirteçleri doğrulama. Kimlik sağlayıcısı ve ad tanımlayıcısı için belirli bir URI'leri şunlardır:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(bkz. Bu [ACS belge sayfasında](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) daha fazla bilgi için.)

Oluşturma veya bir belirteci doğrulanırken, ASP.NET Web yığını çalışma zamanı çalışma zamanında türlerine bağlama deneyin:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (İçin WIF SDK.)
- `System.Security.Claims.ClaimsIdentity` (.NET 4.5).

Bu tür varsa ve geçerli kullanıcının *IIIIdentity* uygular veya alt sınıflarından birini türleri, anti-XSRF tesis kullanır (Kimlik sağlayıcısı, ad tanımlayıcısı) tanımlama grubu oluşturulurken kullanılan kullanıcı adı yerine ve belirteçleri doğrulama. Böyle bir tanımlama grubu varsa, istek anti-XSRF sistem kullanımda belirli talep tabanlı kimlik doğrulama mekanizması anlamak için nasıl yapılandıracağınızı öğrenmek için geliştirici açıklayan bir hata ile başarısız olur. Bkz: **[yapılandırma ve genişletilebilirlik](#_Configuration_and_extensibility)** bölümünde daha fazla bilgi için.

### <a name="oauth--openid-authentication"></a>OAuth / Openıd kimlik doğrulama

Son olarak, anti-XSRF olanağı, OAuth veya Openıd kimlik doğrulaması kullanan uygulamalar için özel destek sunar. Bu destek, buluşsal yöntem tabanlı: varsa geçerli *IIdentity.Name* kullanıcıadı karşılaştırmalar bitti sonra http:// veya https://, ile başlar varsayılan Ordinalıgnorecase karşılaştırıcı yerine sıralı bir karşılaştırıcı kullanılarak.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Yapılandırma ve genişletilebilirlik

Bazen, geliştiricilerin anti-XSRF oluşturma ve doğrulama davranışları sıkı denetime isteyebilirsiniz. Örneğin, belki de otomatik olarak yanıt olarak HTTP tanımlama bilgilerini ekleme, MVC ve Web sayfaları Yardımcıları varsayılan davranışı istenmeyen bir durumdur ve geliştirici belirteçleri başka bir yerde kalıcı hale getirmek isteyebilirsiniz. Bu konuda yardımcı olmak üzere iki API mevcuttur:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens* olarak yöntemi alır (null olabilir) bir mevcut XSRF istek doğrulama Oturum belirteci girdi ve çıktı olarak üretir yeni XSRF istek doğrulama Oturum belirteci ve token pole. Belirteçler, hiçbir deseni ile yalnızca donuk dizelerdir; *formToken* değeri örneği için değil içinde kaydırılır bir &lt;giriş&gt; etiketi. *NewCookieToken* değeri null; bu meydana gelirse, ardından *oldCookieToken* değerdir hala geçerli ve yeni bir yanıt tanımlama bilgisi ayarlamanız gerekir. Çağıran *GetTokens* tüm gerekli yanıt tanımlama bilgileri kalıcı hale veya gerekli tüm biçimlendirme; oluşturmak için sorumludur *GetTokens* yöntemin kendisi değil yanıtı bir yan etkisi olarak değiştirin. *Doğrulama* yöntemi gelen oturum alır ve alan belirteçler ve yukarıda sözü edilen doğrulama mantığı bunlar üzerinde çalışır.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Geliştirici uygulama anti-XSRF sistemden yapılandırabilirsiniz\_başlatın. Programlı yapılandırmadır. Statik özellikler *AntiForgeryConfig* türü aşağıda açıklanmıştır. Çoğu kullanıcı taleplerini kullanmak UniqueClaimTypeIdentifier özelliği ayarlamak istersiniz.

| **Özelliği** | **Açıklama** |
| --- | --- |
| **AdditionalDataProvider** | Bir [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) belirteci oluşturma sırasında ek verileri sağlar ve belirteci doğrulama sırasında ek verileri kullanır. Varsayılan değer *null*. Daha fazla bilgi için [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) bölümü. |
| **CookieName** | Anti-XSRF Oturum belirteci depolamak için kullanılan HTTP tanımlama bilgisinin adını sağlayan bir dize. Bu değer ayarlanmazsa, bir adı uygulamanın dağıtılan sanal yola göre otomatik olarak oluşturulur. Varsayılan değer *null*. |
| **İçindeki requireSSL öğesini** | Anti-XSRF belirteçleri SSL korumalı bir kanal üzerinden gönderilmesi gerekip gerekmediğini belirleyen bir Boole değeri. Bu değer ise *true*, otomatik olarak oluşturulan tanımlama bilgilerini "güvenli" bayrağı ayarlanmış olacaktır ve anti-XSRF API'leri SSL gönderilmeyen bir istek ve çağrılırsa oluşturmaz. Varsayılan değer *false*. |
| **SuppressIdentityHeuristicChecks** | Anti-XSRF sistem, beyana dayalı kimlikler desteğini devre dışı bırakmanız gerekir olup olmadığını belirleyen bir Boole değeri. Bu değer ise *true*, sistem varsayar *IIdentity.Name* kullanıcı başına benzersiz tanımlayıcısı olarak kullanım için uygundur ve özel durum denemez *IClaimsIdentity*veya *ClClaimsIdentity* açıklandığı [WIF / ACS / talep tabanlı kimlik doğrulaması](#_WIF_ACS) bölümü. Varsayılan değer `false` şeklindedir. |
| **UniqueClaimTypeIdentifier** | Hangi talep türü gösteren bir dize, kullanıcı başına benzersiz tanımlayıcısı olarak kullanım için uygundur. Bu değer kümesi ve geçerli ise *IIdentity* beyana dayalı, sistemin bir talep türü ayıklanacak deneyecek tarafından belirtilen *UniqueClaimTypeIdentifier*, ve karşılık gelen değer kullanılır token pole oluştururken kullanıcının raporlamasında kullanıcı adı. Talep türü bulunamadı, sistem istek başarısız olur. Varsayılan değer *null*, gösterir sistem kullanması gerektiğini (Kimlik sağlayıcısı, ad tanımlayıcısı) kullanıcının kullanıcı adı yerine daha önce açıklanan tanımlama grubu. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

*[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* türü gidiş dönüşü ek verilerinizi her belirteç anti-XSRF sistemi davranışını genişletmek geliştiricilerin kullanmasına izin verir. *GetAdditionalData* yöntemi her çağrıldığında bir alan belirteç oluşturulur ve dönüş değeri içinde oluşturulan belirtecin katıştırılır. Bir uygulayan Bu yöntemden bir zaman damgası, nonce veya Filiz isteyen herhangi bir değer döndürebilir.

Benzer şekilde, *ValidateAdditionalData* yöntemi her çağrıldığında token pole doğrulanır ve yönteme geçirilen belirteç katıştırılan "ek verileri" dizesi. Doğrulama yordamına (geçerli saat belirteç oluşturulduğunda, depolanan zamana karşı denetleyerek) bir zaman aşımı uygulayabilirsiniz, mantıksal yordamı veya diğer denetleme nonce istenen.

## <a name="design-decisions-and-security-considerations"></a>Tasarım kararlarını ve güvenlik konuları

Bağlantı oturumu ve alan belirteçleri güvenlik belirteci teknik yalnızca Anonim / kimliği doğrulanmamış kullanıcılar XSRF saldırılarına karşı korumak çalışırken gereklidir. Kullanıcının kimliği doğrulandığında, kimlik doğrulama belirteci kendisi (büyük olasılıkla bir tanımlama bilgisi biçiminde gönderilen) olarak kullanılabilecek bir Eşitleyici yarısını simge çifti. Ancak, kimliği doğrulanmamış kullanıcılar tarafından isabet oturum açma sayfaları korumak için geçerli senaryo vardır ve her zaman kimliği doğrulanmış kullanıcılar için bile güvenlik belirteci doğrulamak ve oluşturma tarafından anti-XSRF mantığı daha basit yapıldı. Token pole gizliliğinin olarak ayarlamak veya başka bir hurdle üstesinden gelmek saldırganın oturum belirteci olacaktır denemelerle tahmin eden bir saldırgan tarafından tehlikeye girmesi durumunda, ayrıca bazı ek koruma sağlar.

Geliştiriciler, çoklu uygulamaları tek bir etki alanı içinde barındırıldığında dikkat kullanmanız gerekir. Örneğin, olsa bile *example1.cloudapp.net* ve *example2.cloudapp.net* farklı konaklar tüm konaklar arasında örtük güven ilişkisi yoktur  *\*. cloudapp.net* etki alanı. Bu örtük güven ilişkisi [birbirlerinin tanımlama bilgilerini etkilemek potansiyel olarak güvenilmeyen konakları sağlayan](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (AJAX isteklerini yöneten bir aynı çıkış noktası ilkeleri mutlaka HTTP tanımlama bilgileri için geçerli değildir). Kötü amaçlı bir alt etki alanı oturum belirteci üzerine mümkün olsa bile, kullanıcı için geçerli bir alan belirteci oluşturamıyor olacak şekilde kullanıcı adı alanı belirteci katıştırılır, ASP.NET Web yığını çalışma zamanı bazı risk azaltma sağlar. Ancak, bu tür bir ortamda barındırıldığında yerleşik anti-XSRF yordamlar hala oturumu ele geçirme veya oturum açma XSRF karşı koruyun olamaz.

Anti-XSRF yordamlar şu anda savunması değil [clickjacking](https://www.owasp.org/index.php/Clickjacking). Kendilerini clickjacking karşı korumak istediğiniz uygulamaları kolayca bunu yapabilir bir X-Frame-Options göndererek: her yanıt SAMEORIGIN üstbilgiyle. Bu üst bilgi, tüm yeni tarayıcılar tarafından desteklenir. Daha fazla bilgi için [IE blog](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), [SDL blog](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), ve [OWASP](https://www.owasp.org/index.php/Clickjacking). ASP.NET Web yığını çalışma zamanı bazı gelecekteki sürüm yap MVC olabilir ve uygulamaların otomatik olarak bu saldırıya karşı korunması Web sayfaları anti-XSRF Yardımcıları otomatik olarak bu başlığı ayarlayın.

Web geliştiricileri sitelerindeki XSS saldırılarına karşı savunmasız olmadığından emin olmak devam etmelidir. XSS saldırılarını çok güçlü ve başarılı yararlanma ASP.NET Web yığını çalışma zamanı savunmaları XSRF saldırılarına karşı da Böl.

## <a name="acknowledgment"></a>Bildirim

[@LeviBroderick](https://twitter.com/LeviBroderick), kimin yazdığı ASP.NET güvenlik kodunu çoğunu bu bilgilerin toplu.
