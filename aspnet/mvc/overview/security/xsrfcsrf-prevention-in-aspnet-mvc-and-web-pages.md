---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: ASP.NET MVC ve Web sayfaları XSRF/CSRF önleme | Microsoft Docs
author: Rick-Anderson
description: Siteler arası istek sahtekarlığı (XSRF veya CSRF olarak da bilinir), kötü amaçlı web sitesini Etkileşi yapabildiği etkileyebilir web barındırılan uygulamalara yönelik bir saldırı olduğunu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2013
ms.topic: article
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 6cf30daa7ed966b11405cec715c5bc803b567249
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
ms.locfileid: "28034002"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>ASP.NET MVC ve Web sayfaları XSRF/CSRF önleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Siteler arası istek sahtekarlığı (XSRF veya CSRF olarak da bilinir) kötü amaçlı web sitesini istemci tarayıcısına ve bu tarayıcı tarafından güvenilen bir web sitesi arasındaki etkileşim yapabildiği etkileyebilir web barındırılan uygulamalara yönelik bir saldırı olduğunu. Web tarayıcıları bir web sitesi için kimlik doğrulama belirteçleri her istek ile otomatik olarak gönderir çünkü bu saldırıların olası yapılır. Klasik ASP gibi bir kimlik doğrulama tanımlama bilgisini bir örneğidir. NET'in form kimlik doğrulaması bileti. Ancak, tüm kalıcı kimlik doğrulama mekanizması (örneğin, Windows kimlik doğrulaması, temel ve benzeri) kullanan web siteleri tarafından bu saldırıların hedeflenebilir.
> 
> Bir kimlik avı saldırıdan XSRF saldırının farklıdır. Kimlik avı saldırıları kurbanın müdahalesini gerektirir. Bir kimlik avı saldırısı zararlı bir web sitesini hedef web sitesi benzetimini yapacak ve kurbanın saldırgan için hassas bilgileri sağlayarak içine sizi kandırmalarına. Bir XSRF saldırısında, genellikle bir etkileşim yoktur kurban gerekli. Bunun yerine, tüm ilgili tanımlama bilgilerini otomatik olarak hedef web sitesine göndermeye tarayıcının saldırgan bağlı.
> 
> Daha fazla bilgi için bkz: [açık Web uygulaması güvenlik projesi](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Saldırının anatomisi

XSRF saldırının yürütmek için bazı çevrimiçi bankacılık işlemler gerçekleştirmek isteyen bir kullanıcının göz önünde bulundurun. Bu kullanıcı ilk WoodgroveBank.com ve günlükleri, bu noktada yanıt üstbilgisi kendi kimlik doğrulama tanımlama bilgisi içerecektir ziyaret eder:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Kimlik doğrulama tanımlama bilgisini bir oturum tanımlama bilgisi olduğundan, tarayıcı işlemi çıktığında, otomatik olarak tarayıcı tarafından temizlenir. Ancak, o zamana kadar tarayıcı otomatik olarak tanımlama bilgisi WoodgroveBank.com her istek ile içerecektir. Kullanıcı artık aynen bankacılık sitesinde form doldurur ve tarayıcı bu sunucuya istekte $1000 başka bir hesaba aktarmak istiyor:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Bu işlem bir yan etkisi (para bir işlem başlatır) olduğundan, bu işlemi başlatmak için bir HTTP POST gerektirecek şekilde bankacılık sitesi seçti. Sunucu kimlik doğrulama belirteci isteğini okur, geçerli kullanıcının hesap numarası görünüyor, fon mevcut ve hedef hesap harekete başlatır doğrular.

Her çevrimiçi tam bankacılık, kullanıcı çıktığınızda bankacılık sitesi gider ve başka konumlara Web'de ziyaret eder. İçinde katıştırılmış bir sayfada aşağıdaki biçimlendirme sitelere – fabrikam.com – birini içeren bir &lt;IFRAME&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Daha sonra bu isteği yapmak tarayıcı neden olur:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

Saldırgan, kullanıcı hedef web sitesi için geçerli bir kimlik doğrulama belirteci sahip ve kendisi bir HTTP POST hedef siteye otomatik olarak yapmak tarayıcı neden küçük parçacık JavaScript kullanıyorsa olgu kullanıyordur. Kimlik doğrulama belirteci hala geçerliyse, bankacılık sitesi $250 aktarımını saldırganın seçme hesaba başlatır.

### <a name="ineffective-mitigations"></a>Etkisiz Azaltıcı Etkenler

Yukarıdaki senaryoda WoodgroveBank.com SSL erişilen ve yalnızca SSL kimlik doğrulama tanımlama bilgisi içeriyor olgusu saldırı korunmanıza yetersiz olduğuna dikkat çekmek üzere ilginç olacaktır. Saldırgan belirtin yapabiliyor [URI düzeni](http://en.wikipedia.org/wiki/URI_scheme) (https) içinde her &lt;form&gt; öğesi ve tarayıcı devam bu tanımlama bilgileri URI'sı ile tutarlı olduğu sürece, süresi dolmamış tanımlama bilgileri hedef siteye göndermek Hedef düzeni.

Bir kullanıcı yalnızca güvenilen siteler, yalnızca çevrimiçi güvenli kalmasına yardımcı ziyaret olarak güvenilmeyen siteleri ziyaret değil karşıdır. Bazı gerçekte bu yoktur, ancak ne yazık ki bu öneriler her zaman pratik olmaz. Belki de kullanıcı "yerel haberleri site ConsolidatedMessenger güvenir". ConsolidatedMessenger.com ve bunun yerine site ziyaret edin, ancak bu siteye giden bir saldırganın aynı fabrikam.com üzerinde çalışan kod parçacığını eklemesine olanak tanıyan bir XSS Güvenlik açığı vardır.

Gelen istekleri sahip olduğunuzu doğrulayın bir [başvuran üstbilgi](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) etki alanınızın başvuruyor. Bu, istemeden bir üçüncü taraf etki alanından gönderilen istekleri durdurur. Ancak, bazı kişiler gizlilikle ilgili nedenlerden dolayı tarayıcının başvuran üstbilgisi devre dışı bırakın ve belirli güvenli olmayan yazılımı yüklü kurbanın varsa, saldırganlar bu başlığı bazen taklit edebilir. Doğrulama [başvuran üstbilgi](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) XSRF saldırılarını önleme için güvenli bir yöntem olarak kabul edilmez.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Web yığını çalışma zamanı XSRF Azaltıcı Etkenler

ASP.NET Web yığını çalışma zamanı bir türevi kullanır [Eşitleyici belirteci düzeni](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) XSRF saldırılarına karşı korumak için. Genel Eşitleyici belirteci düzeni iki anti-XSRF belirteci her HTTP POST (ek olarak kimlik doğrulama belirteci) sunucusuyla gönderilir biçimidir: olarak bir tanımlama bilgisi ve diğer form değer olarak bir belirteç. ASP.NET çalışma zamanı tarafından oluşturulan belirteç değerleri belirleyici veya saldırgan tarafından tahmin edilebilir değil. Belirteçleri gönderildiğinde, sunucunun yalnızca her iki simge bir karşılaştırma denetimi başarılı olursa isteğin sağlayacaktır.

XSRF istek doğrulaması *Oturum belirteci* bir HTTP tanımlama bilgisi olarak depolanır ve şu anda kendi yükte aşağıdaki bilgileri içerir:

- Rastgele bir 128-bit tanıtıcısı oluşan bir güvenlik belirteci.   
 Aşağıdaki resimde Internet Explorer F12 Geliştirici Araçları'ndaki görüntülenen XSRF istek doğrulama Oturum belirteci gösterilmiştir: (Bu geçerli uygulamasıdır ve konu Not değiştirmek bile olabilir.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*Alan belirteci* olarak saklanan bir `<input type="hidden" />` ve kendi yükte aşağıdaki bilgileri içerir:

- Oturum açan kullanıcının kullanıcı adı (kimlik doğrulaması gerekiyorsa).
- Tarafından sağlanan herhangi bir ek veriyi bir [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Anti-XSRF belirteçleri yükü şifrelenir ve imzalanmış belirteçleri incelemek için araçları kullanılırken kullanıcı adı görüntüleyemezsiniz şekilde. Web uygulamasını ASP.NET 4.0 hedeflerken Şifreleme Hizmetleri tarafından sağlanan [MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) yordamı. Ne zaman web uygulamasını ASP.NET 4.5 hedefleme ya da daha yüksek, Şifreleme Hizmetleri tarafından sağlanan [MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) daha iyi performans, genişletilebilirlik ve güvenlik sunar yordamı. Daha fazla bilgi için şu blog yazılarını bakın:

- [ASP.NET 4.5 şifreleme geliştirmeler, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [ASP.NET 4.5 şifreleme geliştirmeler, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [ASP.NET 4.5 şifreleme geliştirmeler, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Belirteçler üreten

Anti-XSRF belirteçleri oluşturmak için çağrı [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) MVC görünümü yönteminden veya @AntiForgery.GetHtmlRazor sayfasından (). Çalışma zamanı, ardından aşağıdaki adımları gerçekleştirmeniz gerekmektedir:

1. Geçerli HTTP isteği zaten bir anti-XSRF Oturum belirteci içeriyorsa (anti-XSRF tanımlama bilgisi \_ \_RequestVerificationToken), güvenlik belirteci şuradan ayıklanır. HTTP isteği bir anti-XSRF Oturum belirteci içermiyorsa veya güvenlik belirteci ayıklama başarısız olursa, yeni bir rastgele anti-XSRF belirteci oluşturulur.
2. Anti-XSRF alan belirteci (1) Yukarıdaki adımı ve geçerli oturum açan kullanıcının kimliğini güvenlik belirteci kullanılarak oluşturulur. (Kullanıcı kimliği belirleme hakkında daha fazla bilgi için bkz: **[özel desteğiyle senaryoları](#_Scenarios_with_special)** bölümüne bakın.) Ayrıca, varsa bir [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) olan yapılandırılmış, çalışma zamanı çağıracak kendi [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) yöntemi ve alan belirteci döndürülen dize içerir. (Bkz **[yapılandırma ve genişletilebilirlik](#_Configuration_and_extensibility)** bölümü daha fazla bilgi için.)
3. Yeni bir anti-XSRF belirteci adım (1) oluşturulduysa, yeni oturum belirteci içermesi için oluşturulacak ve giden HTTP tanımlama bilgileri koleksiyonu eklenir. (2) adımdan alan belirteci içinde kaydırılan bir `<input type="hidden" />` öğesi ve bu HTML biçimlendirmesi dönüş değerini olacak `Html.AntiForgeryToken()` veya `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Belirteçleri doğrulanıyor

Gelen anti-XSRF belirteçleri doğrulamak için geliştirici içeren bir [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) kendi MVC eylem veya denetleyici veya SEH çağrıları öznitelikte `@AntiForgery.Validate()` kendi Razor sayfasından. Çalışma zamanı, aşağıdaki adımları gerçekleştirmeniz gerekmektedir:

1. Gelen oturum belirteci ve alan belirtecini okuyun ve anti-XSRF belirteci her birinden ayıklanır. Anti-XSRF belirteçleri oluşturma yordamına (2) adımda başına aynı olmalıdır.
2. Geçerli kullanıcı kimlik doğrulaması gerekiyorsa, her kullanıcı adı alanı belirteçte depolanan kullanıcı adı ile karşılaştırılır. Kullanıcı adları eşleşmelidir.
3. Varsa bir [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) yapılandırılır, çalışma zamanı çağrıları kendi *ValidateAdditionalData* yöntemi. Yöntem Boolean değeri döndürmelidir *doğru*.

Doğrulama başarılı olursa, istek devam etmesine izin verilir. Doğrulama başarısız olursa, framework özel durum oluşturacak bir *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Hata koşulları

Herhangi bir ASP.NET Web yığını çalışma zamanı v2 ile başlayan *HttpAntiForgeryException* sırasında oluşturulan doğrulama nelerin yanlış gittiğini hakkında ayrıntılı bilgi içerir. Şu anda tanımlanmış hata koşulları şunlardır:

- Oturum belirteci veya form belirteci istekte mevcut değil.
- Oturum belirteci ya da form simgesi okunamaz. ASP.NET Web yığını Runtime veya grubunu eşleşmeyen sürümlerini çalıştıran bir grubu Bunun en olası nedeni olan nerede &lt;machineKey&gt; Web.config öğesinde makineler arasında farklılık gösterir. Bu özel durum ya da anti-XSRF belirteci ile oynanmasını tarafından zorlamak için Fiddler gibi bir araç kullanın.
- Oturum belirteci ve alan belirteci değiştirildi.
- Oturum belirteci ve alan belirteci eşleşmeyen güvenlik belirteçleri içerir.
- İçinde alan belirteci katıştırılmış kullanıcı adı, geçerli oturum açan kullanıcının kullanıcı adı eşleşmiyor.
- *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* döndürülen yöntemi *false*.

Anti-XSRF özellikleri, ayrıca belirteci oluşturma veya doğrulama sırasında ek denetimi gerçekleştirebilir ve hataları bu denetimleri sırasında oluşturulan özel durumları neden olabilir. Bkz: [WIF / ACS / talep tabanlı kimlik doğrulaması](#_WIF_ACS) ve **[yapılandırma ve genişletilebilirlik](#_Configuration_and_extensibility)** bölümlerde daha fazla bilgi için.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Özel desteğiyle senaryoları

### <a name="anonymous-authentication"></a>Anonim kimlik doğrulama

Burada "anonim" tanımlanmış bir kullanıcı olarak anonim kullanıcılar için özel destek anti-XSRF sistemini içeren nerede *IIdentity.IsAuthenticated* özelliği döndürür *false*. Senaryolar içerir (kullanıcının kimliği doğrulanır önce) oturum açma sayfası ve burada uygulamanın kullandığı bir mekanizma dışında özel kimlik doğrulama şemasını XSRF koruma sağlayan *IIdentity* kullanıcıları tanımlamak için.

Bu senaryoları desteklemek için oturum ve alan belirteçleri bir 128-bit rastgele oluşturulan donuk tanımlayıcısı bir güvenlik belirteci tarafından katılan geri çağırma. Bu güvenlik belirteci, etkili bir şekilde anonim bir tanımlayıcı amaca hizmet eder şekilde aynen site gider gibi tek bir kullanıcının oturumu izlemek için kullanılır. Boş bir dize, yukarıda açıklanan oluşturma ve doğrulama yordamları için kullanıcı adı yerine kullanılır.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS / talep tabanlı kimlik doğrulaması

Normalde, *IIdentity* .NET Framework yerleşik sınıfları özelliğe sahip, *IIdentity.Name* belirli bir kullanıcının belirli bir uygulama içinde benzersiz şekilde tanımlamak yeterlidir. Örneğin, *FormsIdentity.Name* (Bu, tüm uygulamalara bağlı olarak bu veritabanı için benzersiz olan) üyelik veritabanında depolanan kullanıcı adını döndürür *WindowsIdentity.Name* döndürür etki alanı tam kimliği kullanıcı ve benzeri. Bu sistemler yalnızca kimlik doğrulaması sağlamak; Bunlar ayrıca *tanımlamak* kullanıcılara bir uygulama.

Talep tabanlı kimlik doğrulama, diğer yandan, mutlaka belirli bir kullanıcının tanımlama gerektirmez. Bunun yerine, *ClaimsPrincipal* ve *Claimsıdentity* türleri bir kümesiyle ilişkili *talep* örnekleri, burada bireysel talepler olabilir "yaşın 18 +" veya " Yönetici başka bir şey için olan". Kullanıcı mutlaka tanımlanan kurmadı olduğundan, çalışma zamanı kullanamazsınız *ClaimsIdentity.Name* özelliği bu kullanıcı için benzersiz bir tanımlayıcı olarak. Takım gerçek örneklerde görüldüğü nerede *ClaimsIdentity.Name* döndürür *null*kolay (Görüntüle) adı döndürür veya aksi takdirde benzersiz bir tanımlayıcı olarak kullanmak için uygun olmayan bir dize döndürür kullanıcı için.

Talep tabanlı kimlik doğrulaması kullanan dağıtımlar çoğunu kullanarak [Azure erişim denetimi hizmeti](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) özellikle. ACS yapılandırmak Geliştirici tek tek verir *kimlik sağlayıcıları* (Microsoft Account sağlayıcısı, ADFS gibi Openıd sağlayıcılarının ister Yahoo!, vb.), ve kimlik sağlayıcılarını döndüren *tanımlayıcılarıadı*. Bir özel kişisel tanımlayıcı (PPID gibi) anonim veya bu adı tanımlayıcılar bir e-posta adresi gibi kişisel bilgilerin (PII) içerebilir. ASP.NET Web yığını çalışma zamanı kullanıcı adı yerine yer alan tanımlama grubu oluştururken kullanabileceğiniz şekilde aynen site gözatması sırada ne olursa olsun, (Kimlik sağlayıcısı, ad tanımlayıcısı) tanımlama grubu yeterince belirli bir kullanıcı için bir uygun İzleme belirteci görevi görür ve Anti-XSRF alan belirteçleri doğrulanıyor. Kimlik sağlayıcısı ve ad tanımlayıcısı için belirli URI'ler şunlardır:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(Bu bkz [ACS belge sayfası](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) daha fazla bilgi için.)

Oluşturulurken veya bir belirteci doğrulamak, ASP.NET Web yığını çalışma zamanı çalışma zamanında türleri bağlamayı deneyin:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (WIF SDK için.)
- `System.Security.Claims.ClaimsIdentity` (İçin .NET 4.5).

Bu tür varsa ve geçerli kullanıcının *IIIIdentity* uygular veya alt sınıfların bunlardan birini türleri, anti-XSRF tesis kullanır (Kimlik sağlayıcısı, ad tanımlayıcısı) tanımlama grubu oluştururken kullanıcı adı yerine ve belirteçleri doğrulanıyor. Böyle bir tanımlama grubu mevcut değilse, istek geliştiriciye kullanımda belirli talep tabanlı kimlik doğrulama mekanizması anlamak için anti-XSRF sisteminin nasıl yapılandırılacağına açıklayan bir hata ile başarısız olur. Bkz: **[yapılandırma ve genişletilebilirlik](#_Configuration_and_extensibility)** daha fazla bilgi için bölüm.

### <a name="oauth--openid-authentication"></a>OAuth / Openıd kimlik doğrulama

Son olarak, anti-XSRF tesis, OAuth veya Openıd kimlik doğrulama kullanan uygulamalar için özel desteğe sahiptir. Bu Destek Tanı dayalıdır: varsa geçerli *IIdentity.Name* kullanıcıadı karşılaştırmaları bitti sonra http:// veya https://, ile başlayan varsayılan Ordinalıgnorecase karşılaştırıcı yerine bir sıra karşılaştırıcı.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Yapılandırma ve genişletilebilirlik

Bazen, geliştiricilerin anti-XSRF oluşturma ve doğrulama davranışları üzerinde daha sıkı denetim isteyebilirsiniz. Örneğin, belki de yanıta HTTP tanımlama bilgilerini otomatik olarak ekleme MVC ve Web sayfaları Yardımcıları varsayılan istenmeyen bir davranıştır ve geliştirici belirteçleri başka bir yerde kalıcı hale getirmek isteyebilirsiniz. Bu konuda yardımcı olmak üzere iki API'leri mevcuttur:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens* olarak yöntemi alır (boş olabilir) bir varolan XSRF istek doğrulama Oturum belirteci giriş ve üretir olarak çıkış yeni XSRF istek doğrulama Oturum belirteci ve alan belirteci. Hiçbir decoration yalnızca donuk dizelerle belirteçleridir; *formToken* değeri örneği için değil Sarmalanan içinde bir &lt;giriş&gt; etiketi. *NewCookieToken* değeri null; bu meydana gelirse, sonra *oldCookieToken* değeri hala geçerli ve yeni bir yanıt tanımlama bilgisi olarak ayarlanması. Çağıranın *GetTokens* tüm gerekli yanıt tanımlama bilgilerini kalıcı yapma veya gerekli tüm biçimlendirme; oluşturmak için sorumlu *GetTokens* yöntemin kendisi değil yanıt olarak bir yan etkisi alter. *Doğrulama* yöntemi gelen oturum alır ve alan belirteçler ve bunları daha önce bahsedilen doğrulama mantığını çalıştırır.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Geliştirici uygulama anti-XSRF sistemden yapılandırabilirsiniz\_başlatın. Programsal yapılandırmadır. Statik özellikler *AntiForgeryConfig* türü aşağıda açıklanmıştır. Talep kullanan çoğu kullanıcılar UniqueClaimTypeIdentifier özelliğini ayarlamak isteyeceksiniz.

| **Özelliği** | **Açıklama** |
| --- | --- |
| **AdditionalDataProvider** | Bir [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) belirteci oluşturma sırasında ek verileri sağlar ve belirteç doğrulama sırasında ek verileri kullanır. Varsayılan değer *null*. Daha fazla bilgi için bkz: [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) bölümü. |
| **CookieName** | Anti-XSRF Oturum belirteci depolamak için kullanılan HTTP tanımlama bilgisinin adını sağlayan bir dize. Bu değer ayarlanmamışsa, bir ad uygulamanın dağıtılan sanal yola göre otomatik olarak oluşturulur. Varsayılan değer *null*. |
| **requireSsl** | Anti-XSRF belirteçleri SSL korumalı bir kanal üzerinden gönderilmesi gerekip gerekmediğini belirleyen bir Boole değeri. Bu değer ise *doğru*, hiçbir otomatik olarak oluşturulan tanımlama bilgisi "güvenli" bayrağı ayarlanmış sahip olur ve anti-XSRF API'leri SSL gönderilmeyen bir istek ve adlı varsa atar. Varsayılan değer *false*. |
| **SuppressIdentityHeuristicChecks** | Anti-XSRF sistem beyana dayalı kimlikler için desteğini devre dışı bırakmanız gerekir olup olmadığını belirleyen bir Boole değeri. Bu değer ise *true*, sistem varsayar *IIdentity.Name* kullanıcı başına benzersiz tanımlayıcısı olarak kullanım için uygundur ve özel durum denemez *IClaimsIdentity*veya *ClClaimsIdentity* açıklandığı gibi [WIF / ACS / talep tabanlı kimlik doğrulaması](#_WIF_ACS) bölümü. Varsayılan değer `false` şeklindedir. |
| **UniqueClaimTypeIdentifier** | Hangi talep türü belirten bir dize, kullanıcı başına benzersiz tanımlayıcısı olarak kullanım için uygundur. Bu değer kümesi ve geçerli ise *IIdentity* tarafından talep tabanlı, sistem bir talep türü ayıklamak deneyecek belirtildiğinden *UniqueClaimTypeIdentifier*, ve karşılık gelen değer kullanılır alan belirteci oluştururken kullanıcının kullanıcı adı yerine. Talep türü bulunamadı, sistem isteği başarısız olur. Varsayılan değer *null*, belirten sistem kullanması gerektiğini (Kimlik sağlayıcısı, ad tanımlayıcısı) daha önce kullanıcının kullanıcı adı yerine açıklandığı gibi tanımlama grubu. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

*[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* türü her belirteci ek veriler gidiş tarafından anti-XSRF sistem davranışını genişletmek geliştiricilere sağlar. *GetAdditionalData* yöntemi her çağrıldığında bir alan belirteci oluşturulur ve dönüş değeri içinde oluşturulan belirteç katıştırılır. Bir uygulayan bir zaman damgası, nonce veya aynen istediği herhangi bir değer bu yönteminden döndürebilirsiniz.

Benzer şekilde, *ValidateAdditionalData* yöntemi her çağrıldığında bir alan belirteci doğrulanır ve belirtecin içinde katıştırılan "ek verileri" dizesi yönteme geçirilen. (Geçerli saati belirteç oluşturulduğunda depolanan zaman karşı denetleyerek) zaman aşımı doğrulama yordamını uygulamak, yordamın veya diğer denetimi nonce mantığı istenen.

## <a name="design-decisions-and-security-considerations"></a>Tasarım kararlarına ve güvenlik konuları

Oturum ve alan belirteçleri bağlantılar güvenlik belirteci teknik olarak yalnızca Anonim / kimliği doğrulanmamış kullanıcıları XSRF saldırılarına karşı korumak çalışırken gereklidir. Kullanıcının kimliği doğrulandığında, kimlik doğrulama belirteci kendisini (büyük olasılıkla bir tanımlama bilgisi biçiminde gönderilen) olarak kullanılabilecek bir Eşitleyici yarısı belirteci çifti. Ancak, kimliği doğrulanmamış kullanıcılar tarafından isabet oturum açma sayfaları korumak için geçerli senaryo vardır ve her zaman oluşturma ve kimliği doğrulanmış kullanıcılar için bile güvenlik belirteci doğrulama tarafından anti-XSRF mantığı daha basit yapıldı. Bir alan belirteci herhangi bir zamanda ayarlama veya oturum belirteci ve bu da saldırganın üstesinden gelmek için başka bir hurdle olacaktır tahmin olarak bağlı olarak, bir saldırgan tarafından tehlikeye durumunda, ayrıca, bazı ek koruma sağlar.

Tek bir etki alanında birden çok uygulama barındırıldığında geliştiriciler dikkatli kullanmanız gerekir. Örneğin, olsa bile *example1.cloudapp.net* ve *example2.cloudapp.net* farklı ana altındaki tüm konaklarda arasında örtük güven ilişkisi yoktur  *\*. cloudapp.net* etki alanı. Bu örtük güven ilişkisi [potansiyel olarak güvenilmeyen ana birbirlerinin tanımlama bilgilerini etkileyen olanak tanır](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (AJAX istekleri yöneten kaynak aynı ilkeleri mutlaka HTTP tanımlama bilgileri için geçerli değildir). Kötü amaçlı bir alt etki alanı oturum belirteci üzerine mümkün olsa bile, kullanıcı için geçerli bir alan belirteci oluşturamıyor gerçekleşmesi için kullanıcı adı alanı belirteci katıştırılır, ASP.NET Web yığını çalışma zamanı bazı azaltma sağlar. Ancak, bu tür bir ortamda barındırıldığında yerleşik anti-XSRF yordamlar hala oturumu ele geçirme veya oturum açma XSRF karşı korumaya olamaz.

Anti-XSRF yordamları şu anda savunması değil [clickjacking öğesini](https://www.owasp.org/index.php/Clickjacking). Kendilerini clickjacking öğesini karşı korumak istediğiniz uygulamaları kolayca bunu yapabilir bir X-Frame-Options göndererek: her yanıtı ile SAMEORIGIN üstbilgisi. Bu üst tüm yeni tarayıcılar tarafından desteklenir. Daha fazla bilgi için bkz: [IE blog](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), [SDL blog](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), ve [OWASP](https://www.owasp.org/index.php/Clickjacking). ASP.NET Web yığını çalışma zamanı bazı gelecekteki sürüm yapma MVC olabilir ve uygulamaları otomatik olarak bu saldırıya karşı korunmaya Web sayfaları anti-XSRF Yardımcıları bu başlığı otomatik olarak ayarlayın.

Web geliştiricileri kendi site XSS saldırılara açık olmadığından emin olmak devam etmelidir. XSS saldırılarını çok güçlü ve başarılı yararlanma XSRF saldırılarına karşı ASP.NET Web yığını çalışma zamanı savunma da Böl.

## <a name="acknowledgment"></a>Bildirim

[@LeviBroderick](https://twitter.com/LeviBroderick), kimin yazdığı ASP.NET güvenlik kodu çoğunu bu bilgileri toplu.
