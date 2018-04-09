---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: ASP.NET AJAX kimlik doğrulaması ve profil uygulaması hizmetlerini anlama | Microsoft Docs
author: scottcate
description: Kimlik doğrulama hizmeti, bir kimlik doğrulama tanımlama bilgisi almak için kimlik bilgilerini sağlamak kullanıcıların sağlar ve özel kullanıcı izin vermek için ağ geçidi hizmeti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 0bf6538d0c4ae9488e6ac29ccba6d4b243cf070e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>ASP.NET AJAX kimlik doğrulaması ve profil uygulaması hizmetlerini anlama
====================
tarafından [Scott göstermek](https://github.com/scottcate)

[PDF indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Bir kimlik doğrulama tanımlama bilgisi almak için kimlik bilgilerini sağlamak kullanıcıların kimlik doğrulama hizmeti sağlar ve özel kullanıcı profillerine izin vermek için ağ geçidi hizmeti ASP.NET tarafından sağlanır. Şu anda form kimlik doğrulaması kullanan uygulamalar (ile oturum açma denetim gibi) AJAX kimlik doğrulama hizmeti yükselterek ayrılmış olarak gösterilir olmayan şekilde ASP.NET AJAX kimlik doğrulama hizmeti kullanımını standart ASP.NET Forms kimlik doğrulaması ile uyumludur.


## <a name="introduction"></a>Giriş

.NET Framework 3.5 bir parçası olarak, Microsoft boyutlandırılabilir ortamında yükseltme iletmektir; yalnızca yeni bir geliştirme ortamı kullanılabilir, ancak yeni dil ile tümleşik sorgu (LINQ) özellikleri ve diğer dili geliştirmeleri yeni çıkacak. Ayrıca, diğer toolsets, ASP.NET AJAX uzantıları özellikle bilinen bazı özellikleri birinci sınıf .NET Framework temel sınıf kitaplığı'nın üyeleri olarak dahil. Bu uzantıları tam sayfa yenileme, Web Hizmetleri (profil oluşturma API'si ASP.NET dahil) istemci komut dosyası ve kapsamlı bir istemci-tarafı API aracılığıyla erişim olanağı gerek kalmadan sayfaların kısmi işleme dahil olmak üzere, birçok yeni zengin istemci özelliklerini etkinleştirme ASP.NET sunucu tarafı denetimi kümesinde görüldüğü denetim düzenleri çoğunu yansıtmak üzere tasarlanmıştır.

Bu teknik uygulama ve ASP.NET Profil kullanımını arar ve Forms kimlik doğrulama hizmetleri Microsoft ASP.NET AJAX ExtensionsThe AJAX uzantıları tarafından sunulan kılın form kimlik doğrulaması desteği, olarak son derece kolay (yanı Profil oluşturma hizmeti), bir Web hizmeti proxy komut dosyası sunulur. AJAX uzantıları da AuthenticationServiceManager sınıfı aracılığıyla özel kimlik doğrulama desteği.

Bu teknik inceleme, Visual Studio 2008 Beta 2 sürümünü ve .NET Framework 3.5 dayanır. Bu Teknik İnceleme de Visual Studio 2008 Beta 2, değil Visual Web Developer Express, çalışma ve Visual Studio kullanıcı arabirimi göre izlenecek yollar sağlar varsayar. Bazı kod örnekleri, proje şablonları Visual Web Developer Express kullanılamaz değerlendirebilir.

## <a name="profiles-and-authentication"></a>*Profilleri ve kimlik doğrulama*

Microsoft ASP.NET profilleri ve kimlik doğrulama hizmetleri ASP.NET formları kimlik sistemi tarafından sağlanan ve ASP.NET standart bileşenleridir. ASP.NET AJAX uzantıları istemci AJAX kitaplığı Sys.Services ad alanı altında oldukça basit bir modeli aracılığıyla betik proxy'leri aracılığıyla bu hizmetleri betik erişim sağlar.

Bir kimlik doğrulama tanımlama bilgisi almak için kimlik bilgilerini sağlamak kullanıcıların kimlik doğrulama hizmeti sağlar ve özel kullanıcı profillerine izin vermek için ağ geçidi hizmeti ASP.NET tarafından sağlanır. Şu anda form kimlik doğrulaması kullanan uygulamalar (ile oturum açma denetim gibi) AJAX kimlik doğrulama hizmeti yükselterek ayrılmış olarak gösterilir olmayan şekilde ASP.NET AJAX kimlik doğrulama hizmeti kullanımını standart ASP.NET Forms kimlik doğrulaması ile uyumludur.

Profil hizmeti kimlik doğrulama hizmeti tarafından sağlanan üyeliğine göre kullanıcı verilerinin depolanması ve otomatik tümleştirme sağlar. Web.config dosyasında depolanan verilerin belirtilen ve çeşitli profil hizmet sağlayıcıları veri yönetimini işlemesine. Böylece şu anda ASP.NET Profil Hizmeti özelliklerini uygulayıp sayfaları AJAX destek ekleyerek bozuk değil hizmetiyle kimlik doğrulaması gibi AJAX profil hizmeti standart ASP.NET Profil Hizmeti ile uyumludur.

ASP.NET kimlik doğrulaması ve profil oluşturma hizmetleri kendilerini bir uygulamaya ekleme, bu teknik kapsamı dışında değil. Konu hakkında daha fazla bilgi için bkz. MSDN Kitaplığı Başvurusu üyeliği kullanarak kullanıcıları yönetme makale [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET üyelik varsayılan kimlik doğrulama hizmet sağlayıcısı ASP.NET üyelik için bir SQL Server ile otomatik olarak ayarlamak için bir yardımcı programı da içerir. Daha fazla bilgi için ASP.NET SQL Server kayıt aracı makalesine bakın (Aspnet\_regsql.exe) adresindeki [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*ASP.NET AJAX kimlik doğrulama Hizmeti'ni kullanma*

Web.config dosyasında ASP.NET AJAX kimlik doğrulama hizmetinin etkinleştirilmesi gerekir:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Kimlik doğrulama hizmetinin etkinleştirilmesi için ASP.NET Forms kimlik doğrulaması gerektirir ve tanımlama bilgilerinin (tanımlama bilgisi içermeyen oturum URL parametreler gerektiren bu yana bir komut dosyası bir tanımlama bilgisi içermeyen oturum etkinleştiremiyor) istemci tarayıcısına etkinleştirilmesine gerektirir.

AJAX kimlik doğrulama hizmeti etkin ve yapılandırıldıktan sonra istemci komut dosyası hemen Sys.Services.AuthenticationService nesnesinin yararlanabilir. Öncelikle, istemci komut dosyası yararlanmak istersiniz `login` yöntemi ve `isLoggedIn` özelliği. Çok sayıda parametre kabul edebileceği oturum açma yöntemi için varsayılanları sağlamak için çeşitli özellikler yok.

*Sys.Services.AuthenticationService üyeleri*

*oturum açma yöntemi:*

Tanımlar: login() yöntemi, kullanıcının kimlik bilgilerini doğrulamak için bir istek başlar. Bu yöntem, zaman uyumsuz olarak çağrılır ve yürütme engellemez.

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| Kullanıcı adı | Gerekli. Kimlik doğrulaması için kullanıcı adı. |
| Parola | İsteğe bağlı (varsayılan değeri null). Kullanıcının parolası. |
| isPersistent | İsteğe bağlı (varsayılan değeri FALSE). Olup kullanıcının kimlik doğrulama tanımlama bilgisi oturumlarında kalıcı olması. False ise, kullanıcı tarayıcı kapatıldığında veya oturumun süresi dolduğunda oturumunuzu. |
| redirectUrl | İsteğe bağlı (varsayılan değeri null). Başarılı kimlik doğrulamasından sonra tarayıcıya yönlendirileceği URL. Bu parametre null veya boş bir dize ise, hiçbir yeniden yönlendirme gerçekleşir. |
| customInfo | İsteğe bağlı (varsayılan değeri null). Bu parametre, şu anda kullanılmayan ve gelecekte kullanılmak üzere ayrılmıştır. |
| loginCompletedCallback | İsteğe bağlı (varsayılan değeri null). Oturum açma başarıyla tamamlandığında çağrılacak işlev. Belirtilmişse, bu parametre defaultLoginCompleted özelliğini geçersiz kılar. |
| failedCallback | İsteğe bağlı (varsayılan değeri null). Oturum açma başarısız olduğunda çağrılacak işlev. Belirtilmişse, bu parametre defaultFailedCallback özelliğini geçersiz kılar. |
| userContext | İsteğe bağlı (varsayılan değeri null). Geri arama işlevleri iletilmesi gereken özel kullanıcı bağlam verileri. |

*Dönüş değeri:*

Bu işlev bir dönüş değeri içermez. Ancak, davranışı sayısını bu işlevi çağrısı tamamlanmasından sonra eklenir:

- Geçerli sayfa ya da yenilenecek veya varsa değiştirilmesi `redirectUrl` parametresi null ya da boş bir dize idi.
- Ancak, parametre null veya boş bir dize ise `loginCompletedCallback` parametresi veya `defaultLoginCompletedCallback` özelliği çağrılır.
- Web hizmeti çağrısı başarısız olursa, `failedCallback` parametresinin `defaultFailedCallback` özelliği çağrılır.

*oturum kapatma yöntemi:*

Logout() yöntemi kimlik tanımlama bilgileri kaldırır ve geçerli kullanıcının web uygulamasından çıkışı günlüğe kaydeder.

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| redirectUrl | İsteğe bağlı (varsayılan değeri null). Başarılı kimlik doğrulamasından sonra tarayıcıya yönlendirileceği URL. Bu parametre null veya boş bir dize ise, hiçbir yeniden yönlendirme gerçekleşir. |
| logoutCompletedCallback | İsteğe bağlı (varsayılan değeri null). Oturum kapatma başarıyla tamamlandığında çağrılacak işlev. Belirtilmişse, bu parametre defaultLogoutCompleted özelliğini geçersiz kılar. |
| failedCallback | İsteğe bağlı (varsayılan değeri null). Oturum açma başarısız olduğunda çağrılacak işlev. Belirtilmişse, bu parametre defaultFailedCallback özelliğini geçersiz kılar. |
| userContext | İsteğe bağlı (varsayılan değeri null). Geri arama işlevleri iletilmesi gereken özel kullanıcı bağlam verileri. |

*Dönüş değeri:*

Bu işlev bir dönüş değeri içermez. Ancak, davranışı sayısını bu işlevi çağrısı tamamlanmasından sonra eklenir:

- Geçerli sayfa ya da yenilenecek veya varsa değiştirilmesi `redirectUrl` parametresi null ya da boş bir dize idi.
- Ancak, parametre null veya boş bir dize ise `logoutCompletedCallback` parametresi veya `defaultLogoutCompletedCallback` özelliği çağrılır.
- Web hizmeti çağrısı başarısız olursa, `failedCallback` parametresinin `defaultFailedCallback` özelliği çağrılır.

*defaultFailedCallback özelliği (get, kümesi):*

Bu özellik web hizmetiyle iletişim kurmak için bir hata oluşursa çağrılmalıdır bir işlev belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| Hata | Hata bilgilerini belirtir. |
| userContext | Oturum açma veya oturum kapatma işlevi çağrıldığında sağlanan kullanıcı bağlam bilgilerini belirtir. |
| methodName | Arama yöntemin adı. |

*defaultLoginCompletedCallback özelliği (get, kümesi):*

Bu özellik, oturum açma web hizmeti çağrısı tamamlandığında çağrılmalıdır bir işlev belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| validCredentials | Kullanıcı geçerli kimlik bilgileri sağlanan olup olmadığını belirtir. `true` Kullanıcı başarıyla oturum Aksi takdirde `false`. |
| userContext | Oturum açma işlevi çağrıldığında sağlanan kullanıcı bağlam bilgilerini belirtir. |
| methodName | Arama yöntemin adı. |

*defaultLogoutCompletedCallback özelliği (get, kümesi):*

Bu özellik, oturum kapatma web hizmeti çağrısı tamamlandığında çağrılmalıdır bir işlev belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| Sonuç | Bu parametre her zaman olacaktır `null`; gelecekte kullanılmak üzere ayrılmış. |
| userContext | Oturum açma işlevi çağrıldığında sağlanan kullanıcı bağlam bilgilerini belirtir. |
| methodName | Arama yöntemin adı. |

*isLoggedIn özelliği (get):*

Bu özellik, geçerli kullanıcı kimlik doğrulama durumunu alır; Bu sayfa isteği sırasında ScriptManager nesne tarafından ayarlanır.

Bu özellik döndürür `true` ; tersi durumda kullanıcı şu anda oturum ise döndürür `false`.

*Path özelliği (get, kümesi):*

Bu özellik, program aracılığıyla kimlik doğrulaması web hizmetinin konumunu belirler. Varsayılan kimlik doğrulama sağlayıcısı olarak biri geçersiz kılmak için kullanılabilir ScriptManager denetimin AuthenticationService alt düğüm yolu özelliğinde bildirimli olarak ayarlama (kullanarak daha fazla bilgi için bkz: bir özel kimlik doğrulama hizmeti sağlayıcısı aşağıdaki konuya).

Varsayılan kimlik doğrulama hizmetinin konumunu değişmeyen unutmayın. Ancak, ASP.NET AJAX ASP.NET AJAX kimlik doğrulama hizmeti proxy'si olarak aynı sınıf arabirimi sağlayan bir web hizmeti konumu belirtmenize olanak tanır.

Ayrıca, bu özellik geçerli sitenin dışına komut isteğinin yönlendiren bir değere ayarlanmamalıdır olduğunu unutmayın. Geçerli uygulamanın kimlik doğrulaması bilgilerini almaz çünkü gereksiz olur; Ayrıca, teknolojisini temel AJAX siteler arası istekleri gönderme değil ve bir güvenlik özel bir istemci tarayıcısında oluşturabilir.

Bu özellik bir `String` kimlik doğrulama web hizmeti yolu temsil eden nesne.

*zaman aşımı özelliği (get, kümesi):*

Bu özellik, kimlik doğrulama hizmeti için oturum açma isteği varsayılarak önce beklenecek süreyi başarısız oldu belirler. Bir çağrı tamamlanması beklenirken zaman aşımı süresi dolarsa, başarısız istek geri çağırma çağrılır ve çağrı tamamlanmaz.

Bu özellik bir `Number` kimlik doğrulama hizmeti sonuçlarından için beklenecek süreyi milisaniye olarak sayısını temsil eden nesne.

*Kod örneği: kimlik doğrulama hizmetine günlüğe kaydetme*

Aşağıdaki biçimlendirmede bir AuthenticationService sınıfının oturum açma ve oturum kapatma yöntemlerini basit bir komut dosyası çağrısıyla örnek ASP.NET sayfasıdır.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>ASP.NET profil oluşturma verilerini AJAX üzerinden erişme

Hizmet profili oluşturma ASP.NET de ASP.NET AJAX uzantıları sunulur. ASP.NET Profil Hizmeti, depolamak ve kullanıcı verilerini almak zengin ve ayrıntılı bir API sağlar olduğundan, bu bir mükemmel üretkenlik aracı olabilir.

Web.config dosyasında profil hizmeti etkinleştirilmelidir; Varsayılan olarak değil. Bunu yapmak için emin `profileService` alt öğesi etkinleştirilmiş = true olarak belirtilen web.config ve hangi özelliklerin okuma ya da aşağıdaki gibi yazılan belirttiğiniz:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Profil hizmeti de yapılandırılması gerekir. Profil oluşturma hizmeti bu teknik kapsamı dışında olsa da, profil yapılandırma ayarlarında tanımlanan gruplar alt özelliklerini grup adı erişilebilir olacağını unutmayın faydalı olur. Belirtilen Örneğin, aşağıdaki profil bölümü ile:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

İstemci komut dosyası adı, Address.Line1, Address.Line2, TextBoxCity.Text = PC, TextBoxState.Text = PC, Address.Zip ve BackgroundColor ProfileService sınıfının özellikleri alanı özellikleri olarak erişebilmeleri.

Profil oluşturma AJAX hizmeti yapılandırıldıktan sonra sayfalarında hemen kullanılabilir olur; Ancak, kez kullanılmadan önce yüklenmesi gerekir.

*Sys.Services.ProfileService üyeleri*

*Özellikler alanı:*

Özellikler alanı tüm yapılandırılmış profil verileri dot işleci adı kuralı tarafından başvurulan alt özellikleri olarak kullanıma sunar. Özellik grupları alt özellikler GroupName.PropertyName adlandırılır. Yukarıda gösterilen örnek profili yapılandırması, kullanıcı durumunu almak için aşağıdaki tanımlayıcıyı kullanabilirsiniz:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*load yöntemi:*

Seçilen liste veya tüm özellikleri sunucudan yükler.

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| propertyNames | İsteğe bağlı (varsayılan değeri null). Sunucudan yüklenecek özellikleri. |
| loadCompletedCallback | İsteğe bağlı (varsayılan değeri null). Yükleme tamamlandığında çağrılacak işlev. |
| failedCallback | İsteğe bağlı (varsayılan değeri null). Bir hata oluşursa çağrılacak işlev. |
| userContext | İsteğe bağlı (varsayılan değeri null). Geri çağırma işlevi geçirilecek bağlam bilgileri. |

Yük işlevi bir dönüş değeri yok. Çağrısı başarıyla tamamlandı, bunu ya da çağıracak varsa `loadCompletedCallback` parametresi veya `defaultLoadCompletedCallback` özelliği. Çağrı başarısız oldu veya zaman aşımı süresi, ya da `failedCallback` parametresi veya `defaultFailedCallback` özelliği çağrılır.

Varsa `propertyNames` parametresi belirtilmedi, tüm özellikleri oku yapılandırılmış sunucusundan alınır.

*save yöntemi:*

Save() yöntemi, kullanıcının ASP.NET profili için belirtilen özellik listesi (veya tüm özellikleri) kaydeder.

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| propertyNames | İsteğe bağlı (varsayılan değeri null). Sunucuya kaydedilmesi için özellikler. |
| saveCompletedCallback | İsteğe bağlı (varsayılan değeri null). Kaydederken çağrılacak işlevin tamamlandı. |
| failedCallback | İsteğe bağlı (varsayılan değeri null). Bir hata oluşursa çağrılacak işlev. |
| userContext | İsteğe bağlı (varsayılan değeri null). Geri çağırma işlevi geçirilecek bağlam bilgileri. |

Kaydetme işlevi bir dönüş değeri yok. Çağrı başarıyla tamamlarsa, ya da çağıracak `saveCompletedCallback` parametresi veya `defaultSaveCompletedCallback` özelliği. Çağrı başarısız oldu veya zaman aşımı süresi, ya da `failedCallback` veya `defaultFailedCallback` özelliği çağrılır.

Varsa `propertyNames` parametresi null, tüm profil özellikleri sunucuya gönderilir ve sunucunun hangi özelliklerin kaydedilebilir ve hangi olamaz karar verir.

*defaultFailedCallback özelliği (get, kümesi):*

Bu özellik web hizmetiyle iletişim kurmak için bir hata oluşursa çağrılmalıdır bir işlev belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| Hata | Hata bilgilerini belirtir. |
| userContext | Ne zaman sağlanan kullanıcı bağlamını belirtir yük veya işlevi çağrıldı. |
| methodName | Arama yöntemin adı. |

*defaultSaveCompleted özelliği (get, kümesi):*

Bu özellik kullanıcı profili verileri kaydetme tamamlandığında çağrılmalıdır işlevi belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| numPropsSaved | Kaydedildi özellikleri sayısını belirtir. |
| userContext | Ne zaman sağlanan kullanıcı bağlamını belirtir yük veya işlevi çağrıldı. |
| methodName | Arama yöntemin adı. |

*defaultLoadCompleted özelliği (get, kümesi):*

Bu özellik kullanıcı profili verileri yüklenmesi tamamlandıktan sonra çağrılmalıdır işlevi belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| numPropsLoaded | Yüklenen özellikler sayısını belirtir. |
| userContext | Ne zaman sağlanan kullanıcı bağlamını belirtir yük veya işlevi çağrıldı. |
| methodName | Arama yöntemin adı. |

*Path özelliği (get, kümesi):*

Bu özellik, program aracılığıyla profil web hizmetinin konumunu belirler. Varsayılan profil hizmet sağlayıcısı olarak biri geçersiz kılmak için kullanılabilir ScriptManager denetimin ProfileService alt düğüm yolu özelliğindeki bildirimli olarak ayarlayın.

Varsayılan profil hizmet konumunu değişmeyen unutmayın. Ancak, ASP.NET AJAX ASP.NET AJAX kimlik doğrulama hizmeti proxy'si olarak aynı sınıf arabirimi sağlayan bir web hizmeti konumu belirtmenize olanak tanır.

Ayrıca, bu özellik geçerli sitenin dışına komut isteğinin yönlendiren bir değere ayarlanmamalıdır olduğunu unutmayın. Teknolojisini temel AJAX siteler arası istekleri gönderme değil ve bir güvenlik özel bir istemci tarayıcısında oluşturabilir.

Bu özellik bir `String` profil web hizmetine yolunu temsil eden nesne.

*zaman aşımı özelliği (get, kümesi):*

Bu özellik, profil hizmeti yük varsayılarak önce bekleyin ya da istek kaydetmek için süre başarısız oldu belirler. Bir çağrı tamamlanması beklenirken zaman aşımı süresi dolarsa, başarısız istek geri çağırma çağrılır ve çağrı tamamlanmaz.

Bu özellik bir `Number` profil hizmeti sonuçlarından için beklenecek süreyi milisaniye olarak sayısını temsil eden nesne.

*Kod örneği: sayfa yükleme profil verileri yükleniyor*

Aşağıdaki kod bir kullanıcı kimliğinin doğrulanıp doğrulanmadığını görmek için kontrol eder ve bu durumda, kullanıcının tercih edilen arka plan rengi sayfanın yüklenir.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Özel kimlik doğrulama hizmeti sağlayıcısını kullanma*

ASP.NET AJAX uzantıları özel web hizmeti aracılığıyla, işlevselliği göstererek bir özel komut dosyası kimlik doğrulama hizmeti sağlayıcısı oluşturmanızı sağlar. Kullanılması için web hizmetiniz iki yöntem kullanıma `Login` ve `Logout`; ve bu yöntemler, varsayılan ASP.NET AJAX kimlik doğrulaması web hizmeti olarak aynı yöntemi imzaları ile belirtilmelidir.

Özel web hizmeti oluşturduktan sonra program aracılığıyla kodda veya istemci komut dosyası aracılığıyla, bildirimli olarak sayfanızda, ya da yolunu belirtmek gerekir.

*Yolun bildirimli olarak ayarlamak için:*

Yolun bildirimli olarak ayarlamak için ASP.NET sayfanızda ScriptManager nesnesinin AuthenticationService alt şunları içerir:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Kodda yolunu ayarlamak için:*

Yolun programlı olarak ayarlamak için komut dosyası yöneticinize örneğini aracılığıyla yolunu belirtin:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Komut dosyası yolunu ayarlamak için:*

Yolun komut dosyasında programlı olarak ayarlamak için kullanma `path` AuthenticationService sınıfın özelliği:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Özel kimlik doğrulama için örnek Web hizmeti*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Özet

ASP.NET Hizmetleri - özellikle profil, üyelik ve kimlik doğrulama Hizmetleri - istemci tarayıcısına kolayca JavaScript sunulur. Bu, geliştiricilerin ağır lifting yapmak için UpdatePanels gibi denetimleri bağlı olarak olmadan sorunsuz bir şekilde, kendi istemci-tarafı kodu ile kimlik doğrulama mekanizması tümleştirmenize olanak sağlar. Profil verileri de istemci tarafından web yapılandırma ayarlarını yararlanarak korunabilir; Varsayılan olarak kullanılabilir veri yok ve geliştiriciler için profil özellikleri katılımı gerekir.

Ayrıca, eşdeğer yöntemi imzalarla Basitleştirilmiş web hizmeti uygulamaları oluşturarak, geliştiriciler bu iç ASP.NET hizmetleri için özel bir komut dosyası sağlayıcıları oluşturabilir. Bu teknikler desteği, çok çeşitli belirli gereksinimlerini karşılamak için esneklik geliştiriciler sağlarken zengin istemci uygulamaları geliştirme basitleştirir.

## <a name="bio"></a>*Bio*

Tan göstermek Microsoft Web teknolojileri ile bu yana 1997 çalışma ve myKB.com Başkanı ise ([www.myKB.com](http://www.myKB.com)) kendisine ASP.NET yazılırken burada uzmanlaşmış tabanlı Bilgi Bankası yazılım çözümlerini odaklanmış uygulamaları. Tan temas kurulabileceğini doğrula e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog adresindeki [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Önceki](understanding-asp-net-ajax-updatepanel-triggers.md)
> [sonraki](understanding-asp-net-ajax-localization.md)
