---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: ASP.NET AJAX kimlik doğrulaması ve profil uygulaması hizmetlerini anlama | Microsoft Docs
author: scottcate
description: Kimlik doğrulama hizmeti kullanıcıların bir kimlik doğrulama tanımlama bilgisi almak için kimlik bilgilerini sağlamanız olanak tanır ve özel kullanıcı izin vermek için ağ geçidi hizmeti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 323fec56f18281b5b5a3d312a2e4c4c7133e3f03
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393067"
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>ASP.NET AJAX kimlik doğrulaması ve profil uygulaması hizmetlerini anlama
====================
tarafından [Scott Cate](https://github.com/scottcate)

[PDF'yi indirin](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Özel kullanıcı profillerine izin vermek için ağ geçidi hizmeti, ASP.NET tarafından sağlanan ve kullanıcıların bir kimlik doğrulama tanımlama bilgisi almak için kimlik bilgilerini sağlamak kimlik doğrulama hizmeti sağlar. ASP.NET AJAX kimlik doğrulama hizmetinin standart ASP.NET formları kimlik doğrulama ile uyumlu olduğundan şu anda form kimlik doğrulaması kullanan uygulamalar (oturum açma ile denetim gibi) AJAX kimlik doğrulama hizmeti yükselterek bozuk durumda olacaktır değil.


## <a name="introduction"></a>Giriş

.NET Framework 3.5 bir parçası olarak Microsoft, hacimle ortam yükseltme iletmektir; yalnızca yeni bir geliştirme ortamı kullanılabilir, ancak yeni dil ile tümleşik sorgu (LINQ) özellikleri ve diğer dil geliştirmeleri gelecek. Ayrıca, diğer araç takımları, özellikle de ASP.NET AJAX uzantılarını özelliklerinden bazıları hakkında bilgi sahibi birinci sınıf üyelerinin .NET Framework temel sınıf kitaplığı dahil edildiği. Bu uzantıları istemci komut dosyası (profil oluşturma API'si ASP.NET dahil olmak üzere) ve kapsamlı bir istemci tarafı API Web hizmetlerine erişme olanağını tam sayfa yenileme gerek kalmadan sayfaların kısmi işleme dahil olmak üzere, birçok yeni zengin istemci özelliklerini etkinleştirme ASP.NET sunucu tarafı denetim kümesinde görüldüğü denetim düzenleri birçoğu yansıtmak üzere tasarlanmıştır.

Bu teknik incelemede uygulama ve ASP.NET profil oluşturma kullanımını arar ve formları kimlik doğrulama hizmetleri Microsoft ASP.NET AJAX ExtensionsThe AJAX uzantıları tarafından kullanıma sunulan yapın form kimlik doğrulaması desteği, olarak son derece kolay (yanı Profil oluşturma hizmeti), bir Web hizmeti proxy betiği kullanıma sunulur. AJAX uzantıları AuthenticationServiceManager sınıfı aracılığıyla özel kimlik doğrulama da destekler.

Bu teknik incelemede, Beta 2 sürümünü Visual Studio 2008 ve .NET Framework 3.5 dayanır. Bu Teknik İnceleme de Visual Studio 2008 Beta 2 ile de değil Visual Web Developer Express, çalışma ve izlenecek yollar Visual Studio'nun kullanıcı arabirimi göre sağlayacak varsayar. Kod örnekleri, Visual Web Developer Express kullanılabilir proje şablonları değerlendirebilir.

## <a name="profiles-and-authentication"></a>*Profiller ve kimlik doğrulaması*

Microsoft ASP.NET profilleri ve kimlik doğrulama hizmetleri ASP.NET formları kimlik doğrulamasını sistem tarafından sağlanır ve ASP.NET standart bileşenleridir. ASP.NET AJAX uzantılarını hizmetlerin Sys.Services ad alanı AJAX istemci Kitaplığı'nın altında oldukça basit bir modeli aracılığıyla betik proxy'leri aracılığıyla betik erişim sağlar.

Özel kullanıcı profillerine izin vermek için ağ geçidi hizmeti, ASP.NET tarafından sağlanan ve kullanıcıların bir kimlik doğrulama tanımlama bilgisi almak için kimlik bilgilerini sağlamak kimlik doğrulama hizmeti sağlar. ASP.NET AJAX kimlik doğrulama hizmetinin standart ASP.NET formları kimlik doğrulama ile uyumlu olduğundan şu anda form kimlik doğrulaması kullanan uygulamalar (oturum açma ile denetim gibi) AJAX kimlik doğrulama hizmeti yükselterek bozuk durumda olacaktır değil.

Profil hizmeti kimlik doğrulama hizmeti tarafından sağlanan üyeliğine göre kullanıcı verilerini depolama ve otomatik tümleştirme sağlar. Depolanan veriler, web.config dosyasında belirtilen ve veri yönetimi çeşitli profil oluşturma hizmet sağlayıcıları tanıtıcı. Şu anda ASP.NET Profil Hizmeti özelliklerini bir araya getiren sayfaları AJAX desteği dahil ederek bozuk durumda değil, kimlik doğrulama hizmetiyle olduğu gibi AJAX profil hizmeti standart ASP.NET profili hizmeti ile uyumludur.

ASP.NET kimlik doğrulaması ve profil oluşturma hizmetleri kendilerini bir uygulamaya ekleme, bu teknik incelemede kapsamı dışında olan. Konu hakkında daha fazla bilgi için bkz. MSDN Kitaplığı Başvurusu üyeliği kullanarak kullanıcıları yönetme makale [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET üyelik ASP.NET üyelik için varsayılan kimlik doğrulama hizmeti sağlayıcısı olan bir SQL sunucusu, otomatik olarak ayarlamak için bir yardımcı programı da içerir. Daha fazla bilgi için ASP.NET SQL Server kayıt aracı makalesine bakın (Aspnet\_regsql.exe) adresindeki [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*ASP.NET AJAX kimlik doğrulama Hizmeti'ni kullanma*

Web.config dosyasına ASP.NET AJAX kimlik doğrulama hizmetinin etkinleştirilmesi gerekir:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Kimlik doğrulama hizmetinin etkinleştirilmesi için ASP.NET formları kimlik doğrulaması gerektirir ve tanımlama bilgileri (cookieless oturumları URL parametrelerini gerektirdiğinden bir komut dosyası içermeyen oturum etkinleştirilemiyor) istemci tarayıcısına etkinleştirilmesini gerektirir.

AJAX kimlik doğrulama hizmetinin etkin ve yapılandırıldıktan sonra istemci betiği hemen Sys.Services.AuthenticationService nesne yararlanabilirsiniz. İstemci betik yararlanmak öncelikle isteyeceksiniz `login` yöntemi ve `isLoggedIn` özelliği. Çok sayıda parametre kabul edebileceği oturum açma yöntemi için varsayılanlar sağlamak için çeşitli özellikler mevcuttur.

*Sys.Services.AuthenticationService üyeleri*

*oturum açma yöntemi:*

Tanımlar: login() yöntemi, kullanıcının kimlik bilgilerini doğrulamak için bir istek başlar. Bu yöntem, zaman uyumsuz ve yürütme engellemez.

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| Kullanıcı adı | Gerekli. Kimlik doğrulaması için kullanıcı adı. |
| Parola | İsteğe bağlı (varsayılan olarak null). Kullanıcının parolası. |
| isPersistent | İsteğe bağlı (varsayılan false). Kullanıcının kimlik doğrulama tanımlama bilgisi oturumdan oturuma kalıcı olup. False ise, kullanıcının tarayıcı kapatıldı veya oturum süresi dolduğunda oturumunuzu. |
| redirectUrl | İsteğe bağlı (varsayılan olarak null). Başarılı kimlik doğrulamadan sonra tarayıcının yeniden yönlendirileceği URL. Bu parametre null veya boş bir dize ise, hiçbir yeniden yönlendirme gerçekleşir. |
| customInfo | İsteğe bağlı (varsayılan olarak null). Bu parametre, şu anda kullanılmayan ve gelecekte kullanılmak üzere ayrılmıştır. |
| loginCompletedCallback | İsteğe bağlı (varsayılan olarak null). Oturum açma başarıyla tamamlandığında aranacak işlev. Bu parametre belirtilmişse defaultLoginCompleted özelliğini geçersiz kılar. |
| failedCallback | İsteğe bağlı (varsayılan olarak null). Oturum açma başarısız olduğunda çağrılacak işlev. Bu parametre belirtilmişse defaultFailedCallback özelliğini geçersiz kılar. |
| userContext | İsteğe bağlı (varsayılan olarak null). Geri arama işlevlerine geçirilen özel kullanıcı bağlam verileri. |

*Dönüş değeri:*

Bu işlev dönüş değeri içermez. Ancak, davranışları sayısı bu işlev çağrısı tamamlanmasından sonra dahil edilir:

- Geçerli sayfa, değiştirilmesi veya yenilenmiş olacaktır `redirectUrl` parametresi olan null ya da boş bir dize.
- Ancak, parametre null veya boş bir dize ise `loginCompletedCallback` parametresi veya `defaultLoginCompletedCallback` özelliğin çağırılır.
- Web hizmetine yapılan çağrı başarısız olursa `failedCallback` parametresinin `defaultFailedCallback` özelliğin çağırılır.

*oturum kapatma yöntemi:*

Logout() yöntemi kimlik tanımlama bilgisinin kaldırır ve geçerli kullanıcının web uygulamasından günlüğe kaydeder.

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| redirectUrl | İsteğe bağlı (varsayılan olarak null). Başarılı kimlik doğrulamadan sonra tarayıcının yeniden yönlendirileceği URL. Bu parametre null veya boş bir dize ise, hiçbir yeniden yönlendirme gerçekleşir. |
| logoutCompletedCallback | İsteğe bağlı (varsayılan olarak null). Oturum kapatma başarıyla tamamlandığında aranacak işlev. Bu parametre belirtilmişse defaultLogoutCompleted özelliğini geçersiz kılar. |
| failedCallback | İsteğe bağlı (varsayılan olarak null). Oturum açma başarısız olduğunda çağrılacak işlev. Bu parametre belirtilmişse defaultFailedCallback özelliğini geçersiz kılar. |
| userContext | İsteğe bağlı (varsayılan olarak null). Geri arama işlevlerine geçirilen özel kullanıcı bağlam verileri. |

*Dönüş değeri:*

Bu işlev dönüş değeri içermez. Ancak, davranışları sayısı bu işlev çağrısı tamamlanmasından sonra dahil edilir:

- Geçerli sayfa, değiştirilmesi veya yenilenmiş olacaktır `redirectUrl` parametresi olan null ya da boş bir dize.
- Ancak, parametre null veya boş bir dize ise `logoutCompletedCallback` parametresi veya `defaultLogoutCompletedCallback` özelliğin çağırılır.
- Web hizmetine yapılan çağrı başarısız olursa `failedCallback` parametresinin `defaultFailedCallback` özelliğin çağırılır.

*defaultFailedCallback özelliği (get, set):*

Bu özellik, web hizmetiyle iletişim kurmak için bir hata oluşursa çağrılması gereken bir işlevi belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası sahip olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| Hata | Hata bilgilerini belirtir. |
| userContext | Oturum açma veya kapatma işlevi çağrıldığında sağlanan kullanıcı bağlam bilgileri belirtir. |
| methodName | Çağıran yöntemin adı. |

*defaultLoginCompletedCallback özelliği (get, set):*

Bu özellik, oturum açma web hizmeti çağrısı tamamlandığında çağrılması gereken bir işlevi belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası sahip olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| validCredentials | Kullanıcı geçerli kimlik bilgileri sağlanan olup olmadığını belirtir. `true` Kullanıcı başarıyla oturum açtı Aksi takdirde `false`. |
| userContext | Oturum açma işlevi çağrıldığında sağlanan kullanıcı bağlam bilgileri belirtir. |
| methodName | Çağıran yöntemin adı. |

*defaultLogoutCompletedCallback özelliği (get, set):*

Bu özellik, oturum kapatma web hizmeti çağrısı tamamlandığında çağrılması gereken bir işlevi belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası sahip olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| Sonuç | Bu parametre her zaman olacak `null`; gelecekte kullanılmak üzere ayrılmıştır. |
| userContext | Oturum açma işlevi çağrıldığında sağlanan kullanıcı bağlam bilgileri belirtir. |
| methodName | Çağıran yöntemin adı. |

*isLoggedIn özelliği (get):*

Bu özellik, geçerli kullanıcının kimlik doğrulama durumunu alır; bir sayfa isteği sırasında ScriptManager nesne tarafından ayarlanır.

Bu özellik döndürür `true` ; tersi durumda kullanıcı şu anda oturum ise döndürür `false`.

*Path özelliği (get, set):*

Bu özellik, program aracılığıyla kimlik doğrulaması web hizmeti konumunu belirler. Varsayılan kimlik doğrulama sağlayıcısı yanı sıra bir geçersiz kılmak için kullanılabilir ScriptManager denetimin AuthenticationService alt düğüm yolu özelliğindeki bildirimli olarak ayarlama (kullanarak daha fazla bilgi için bkz. bir özel kimlik doğrulama hizmet sağlayıcısı konu aşağıdaki).

Varsayılan kimlik doğrulama hizmeti konumunu değişmez unutmayın. Ancak, ASP.NET AJAX ASP.NET AJAX kimlik doğrulama hizmeti proxy'si olarak aynı sınıf arabirimi sağlayan bir web hizmeti konumunu belirtmenize olanak sağlar.

Ayrıca, bu özellik geçerli sitenin dışına betik isteği yönlendiren bir değere ayarlanmamalıdır olduğunu unutmayın. Geçerli uygulamanın kimlik doğrulama bilgilerini almaz çünkü gereksiz olur; Ayrıca, teknolojisi temel AJAX siteler arası istek gönderin değil ve bir güvenlik özel durumu, bir istemci tarayıcısında oluşturabilir.

Bu özellik bir `String` kimlik doğrulama web hizmeti yolu temsil eden nesne.

*zaman aşımı özelliği (get, set):*

Bu özellik, başarısız kimlik doğrulama hizmeti için oturum açma isteği varsayılarak önce beklenecek sürenin uzunluğunu belirler. Bir çağrı tamamlanması beklenirken zaman aşımı süresi dolarsa, geri çağırma isteği başarısız oldu olarak adlandırılır ve çağrı tamamlanmaz.

Bu özellik bir `Number` kimlik doğrulama hizmeti sonuçları için beklenecek milisaniye sayısını temsil eden nesne.

*Kod örneği: Kimlik doğrulama hizmetine oturum açarak*

Aşağıdaki biçimlendirmede AuthenticationService sınıfının oturum açma ve oturum kapatma yöntemlerini basit bir komut dosyası çağrısı ile bir örnek ASP.NET sayfasıdır.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>ASP.NET profil oluşturma verilerini AJAX üzerinden erişme

Hizmet Profil ASP.NET'i de ASP.NET AJAX uzantıları kullanıma sunulur. ASP.NET Profil Hizmeti, depolamak ve kullanıcı verilerini almak zengin ve ayrıntılı bir API sağlar. bu yana bu mükemmel üretkenlik aracı olabilir.

Web.config dosyasında profil hizmeti etkinleştirilmelidir; Varsayılan olarak değil. Bunu yapmak için olun `profileService` alt öğesi etkinleştirilmiş = true olarak belirtilen web.config ve hangi özellikler okunabilir ve aşağıdaki gibi yazılır belirttiniz:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Profil hizmeti de yapılandırılması gerekir. Profil oluşturma hizmeti bu teknik incelemede kapsamı dışında olsa da, faydalı profili yapılandırma ayarlarında tanımlanan grupların alt grup adı özellikleri olarak erişilebilir olacağını unutmayın. Belirtilen Örneğin, aşağıdaki profil bölümü ile:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

İstemci komut dosyası adı, Address.Line1, Address.Line2, TextBoxCity.Text = PC, TextBoxState.Text = PC, Address.Zip ve BackgroundColor özellikler alanı ProfileService sınıfının özellikleri olarak erişebilmesi.

AJAX profil oluşturma hizmet yapılandırıldıktan sonra sayfalarında hemen kullanılabilir olması; Ancak, bu kez kullanılmadan önce yüklenmesi gerekir.

*Sys.Services.ProfileService üyeleri*

*Özellikler alanı:*

Özellikler alanı tüm yapılandırılmış profil verileri, dot işleci adı kuralı tarafından başvurulan alt özellikleri olarak kullanıma sunar. Alt özellik gruplarını özellikleri GroupName.PropertyName adlandırılır. Yukarıda gösterilen örnek profili yapılandırmada, kullanıcı durumunu almak için aşağıdaki tanımlayıcı kullanabilirsiniz:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*load yöntemi:*

Sunucudan Seçili listeyi veya tüm özellikleri yükler.

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| propertyNames | İsteğe bağlı (varsayılan olarak null). Sunucudan yüklenecek özellikleri. |
| loadCompletedCallback | İsteğe bağlı (varsayılan olarak null). Yükleme tamamlandığında çağrılacak işlev. |
| failedCallback | İsteğe bağlı (varsayılan olarak null). Bir hata oluşursa çağrılacak işlev. |
| userContext | İsteğe bağlı (varsayılan olarak null). Geri çağırma işlevine geçirilecek bağlam bilgileri. |

Yük işlevin dönüş değeri yok. Çağrısı başarıyla tamamlandı, ya da çağırır, `loadCompletedCallback` parametresi veya `defaultLoadCompletedCallback` özelliği. Çağrı başarısız oldu veya zaman aşımı süresi, ya da `failedCallback` parametresi veya `defaultFailedCallback` özelliği çağrılabilir.

Varsa `propertyNames` parametresi sağlanmadı, tüm özellikleri oku yapılandırılmış sunucudan alınır.

*save yöntemi:*

()'i yöntemi, kullanıcının ASP.NET profil için belirtilen özellik listesi (veya tüm özellikleri) kaydeder.

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| propertyNames | İsteğe bağlı (varsayılan olarak null). Sunucuya kaydedilecek özellikleri. |
| saveCompletedCallback | İsteğe bağlı (varsayılan olarak null). Kaydetme sırasında aranacak işlev tamamlandı. |
| failedCallback | İsteğe bağlı (varsayılan olarak null). Bir hata oluşursa çağrılacak işlev. |
| userContext | İsteğe bağlı (varsayılan olarak null). Geri çağırma işlevine geçirilecek bağlam bilgileri. |

Kaydetme işlevi dönüş değeri yok. Arama işlemi başarıyla tamamlarsa, ya da çağıracak `saveCompletedCallback` parametresi veya `defaultSaveCompletedCallback` özelliği. Çağrı başarısız oldu veya zaman aşımı süresi, ya da `failedCallback` veya `defaultFailedCallback` özelliği çağrılabilir.

Varsa `propertyNames` parametresi null, tüm profil özelliklerini, sunucuya gönderilir ve sunucunun hangi özelliklerin kaydedilebilir ve hangi olamaz karar verir.

*defaultFailedCallback özelliği (get, set):*

Bu özellik, web hizmetiyle iletişim kurmak için bir hata oluşursa çağrılması gereken bir işlevi belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası sahip olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| Hata | Hata bilgilerini belirtir. |
| userContext | Belirtir, sağlanan kullanıcı bağlam bilgileri yük veya işlev çağrıldı. |
| methodName | Çağıran yöntemin adı. |

*defaultSaveCompleted özelliği (get, set):*

Bu özellik, kullanıcı profili verileri kaydetme tamamlanmasından sonra çağrılmalıdır bir işlevi belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası sahip olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| numPropsSaved | Kaydedilen özellikleri sayısını belirtir. |
| userContext | Belirtir, sağlanan kullanıcı bağlam bilgileri yük veya işlev çağrıldı. |
| methodName | Çağıran yöntemin adı. |

*defaultLoadCompleted özelliği (get, set):*

Bu özellik, kullanıcı profili verilerini yüklemeyi tamamladıktan sonra çağrılmalıdır bir işlevi belirtir. Bir temsilci (veya işlev başvurusu) almanız gerekir.

Bu özelliği tarafından belirtilen işlev başvurusu aşağıdaki imzası sahip olmalıdır:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parametreler:*

| **Parametre adı** | **Anlamı** |
| --- | --- |
| numPropsLoaded | Yüklenen özellikleri sayısını belirtir. |
| userContext | Belirtir, sağlanan kullanıcı bağlam bilgileri yük veya işlev çağrıldı. |
| methodName | Çağıran yöntemin adı. |

*Path özelliği (get, set):*

Bu özellik, program aracılığıyla profili web hizmetinin konumunu belirler. Varsayılan profil hizmet sağlayıcısı, aynı zamanda bir geçersiz kılmak için kullanılabilir ScriptManager denetimin ProfileService alt düğüm yolu özelliğindeki bildirimli olarak ayarlama.

Varsayılan profil hizmetinin konumunu değişmez unutmayın. Ancak, ASP.NET AJAX ASP.NET AJAX kimlik doğrulama hizmeti proxy'si olarak aynı sınıf arabirimi sağlayan bir web hizmeti konumunu belirtmenize olanak sağlar.

Ayrıca, bu özellik geçerli sitenin dışına betik isteği yönlendiren bir değere ayarlanmamalıdır olduğunu unutmayın. Teknoloji temel AJAX siteler arası istek gönderin değil ve bir güvenlik özel durumu, bir istemci tarayıcısında oluşturabilir.

Bu özellik bir `String` profili web hizmetine yolu temsil eden nesne.

*zaman aşımı özelliği (get, set):*

Bu özellik profil hizmeti için önce yük varsayılarak bekleyin ya da istek kaydetmek için gereken süre uzunluğunu başarısız oldu belirler. Bir çağrı tamamlanması beklenirken zaman aşımı süresi dolarsa, geri çağırma isteği başarısız oldu olarak adlandırılır ve çağrı tamamlanmaz.

Bu özellik bir `Number` profil hizmeti sonuçları için beklenecek milisaniye sayısını temsil eden nesne.

*Kod örneği: sayfa yükleme profil verileri yükleniyor*

Aşağıdaki kod, bir kullanıcının kimlik doğrulamasını olup olmadığını görmek için kontrol eder ve bu durumda, kullanıcının tercih edilen arka plan rengi sayfanın yüklenir.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Özel kimlik doğrulama hizmet sağlayıcısını kullanma*

ASP.NET AJAX uzantılarını işlevinizi özel web hizmeti aracılığıyla bir özel betik kimlik doğrulama hizmeti sağlayıcısı oluşturmanızı sağlar. Kullanılabilmesi için web hizmetiniz iki yöntem kullanıma `Login` ve `Logout`; ve bu yöntemleri aynı yöntem imzaları varsayılan ASP.NET AJAX kimlik doğrulaması web hizmeti olarak belirtilmelidir.

Özel web hizmeti oluşturulduktan sonra kodunda programlı olarak veya istemci komut dosyası aracılığıyla, bildirimli olarak sayfanızda, ya da yolunu belirtmeniz gerekir.

*Yolun bildirimli olarak ayarlamak için:*

Yolun bildirimli olarak ayarlamak için ASP.NET sayfasında ScriptManager nesnesinin AuthenticationService alt şunlardır:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Kodda yolunu ayarlamak için:*

Yolun program üzerinden ayarlamak için betik yöneticinize örneği aracılığıyla yolu belirtin:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Komut dosyası yolunu ayarlamak için:*

Yolu betikte program üzerinden ayarlamak için yazılımınız `path` AuthenticationService sınıfın özelliği:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Özel kimlik doğrulaması için örnek Web hizmeti*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Özet

ASP.NET Hizmetleri - özellikle profil oluşturma, üyelik ve kimlik doğrulama hizmetleri - JavaScript için istemci tarayıcısına kolayca kullanıma sunulur. Bu, geliştiricilerin kaynaklanan ağır yüklerden yapmak için UpdatePanels gibi denetimler bağlı olmadan sorunsuz bir şekilde, istemci tarafı kodlarını kimlik doğrulama mekanizması ile tümleştirmek sağlar. Profil verileri de istemciden web yapılandırma ayarlarını yararlanarak korunabilir; Varsayılan olarak kullanılabilir veri yok ve geliştiriciler için profil özellikleri katılımı gerekir.

Ayrıca, bu iç ASP.NET hizmetleri için özel betik sağlayıcıları ile eşdeğer yöntem imzaları basitleştirilmiş bir web hizmeti uygulamalar oluşturarak, geliştiricilerin oluşturabilirsiniz. Bu teknikler desteği, geliştiricilerin çok çeşitli belirli gereksinimlerini karşılayacak şekilde esneklik sağlayarak zengin istemci uygulamaları geliştirilmesini basitleştirir.

## <a name="bio"></a>*Bio*

Scott Cate 1997'den beri Microsoft Web teknolojileri ile çalışmakta olduğu ve myKB.com Yardımcısı ([www.myKB.com](http://www.myKB.com)) tabanlı Bilgi Bankası yazılım çözümlerinizi odaklı uygulamaları burada kendisinin ASP.NET yazma konusunda uzmanlaşmış. Scott temas kurulabileceğini e-posta aracılığıyla [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) veya kendi blog'da [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Önceki](understanding-asp-net-ajax-updatepanel-triggers.md)
> [İleri](understanding-asp-net-ajax-localization.md)
