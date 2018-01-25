---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-cs
title: "Rol tabanlı yetkilendirme (C#) | Microsoft Docs"
author: rick-anderson
description: "Bu öğretici rolleri framework bir kullanıcının rollerini kendi güvenlik bağlamı ile nasıl ilişkilendirdiğini bir göz başlar. Sonra rol tabanlı URL uygulamak nasıl inceler..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 4d9b63fa-c3d4-4e85-82b1-26ae3ba3ca1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a71c463f94bafa80b7fd2f97f381b5d8cb5dcaa
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="role-based-authorization-c"></a>Rol tabanlı yetkilendirme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.11.zip) veya [PDF indirin](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_cs.pdf)

> Bu öğretici rolleri framework bir kullanıcının rollerini kendi güvenlik bağlamı ile nasıl ilişkilendirdiğini bir göz başlar. Sonra rol tabanlı URL yetkilendirme kuralları uygulamak nasıl inceler. Biz görüntülenen verileri ve ASP.NET sayfası tarafından sunulan işlevselliği değiştirilmesi için bildirim temelli ve programlı bir şekilde kullanarak görüneceğini, aşağıdaki.


## <a name="introduction"></a>Giriş

İçinde <a id="_msoanchor_1"> </a> [ *kullanıcı tabanlı bir yetkilendirme* ](../membership/user-based-authorization-cs.md) URL yetkilendirmesi sayfaları belirli bir dizi hangi kullanıcıların ziyaret ettiği belirtmek için nasıl kullanılacağını gördüğümüz öğretici. Biraz içinde biçimlendirme ile `Web.config`, yalnızca kimliği doğrulanmış kullanıcıların bir sayfasını ziyaret izin vermek için ASP.NET istemeniz. Veya biz yalnızca kullanıcıların Tito ve Bob izin verilmekteydi dikte Sam hariç tüm kimliği doğrulanmış kullanıcılara izin gösterir.

URL yetkilendirmesi yanı sıra, aynı zamanda görüntülenen verileri ve ziyaret kullanıcıyı temel alarak bir sayfa tarafından sunulan işlevselliği denetlemek için bildirim temelli ve programlama teknikleri inceledik. Özellikle, geçerli dizin içeriğini listelenen bir sayfa oluşturduk. Herkes bu sayfayı ziyaret ancak yalnızca kimliği doğrulanan kullanıcılar dosyaların içeriğini görüntüleyebilir ve yalnızca Tito dosyaları silinemedi.

Yetkilendirme kuralları bir kullanıcı tarafından temelinde uygulama muhasebe onarımı kabus büyüyebilir. Rol tabanlı yetkilendirme kullanmak daha rahat bir yaklaşımdır. Yetkilendirme kuralları uygulamak için bizim elden araçları eşit olarak çalıştığını iyi haber olan kullanıcı hesapları için yaptığınız gibi rolleriyle yanı sıra. URL yetkilendirme kuralları, kullanıcıların yerine rolleri belirtebilirsiniz. Kimliği doğrulanmış ve anonim kullanıcılar için farklı bir çıkış işler, LoginView denetimi, oturum açmış kullanıcının rollere göre farklı içeriği görüntülemek için yapılandırılabilir. Ve rolleri API oturum açmış kullanıcının rollerini belirlemek için yöntemler içerir.

Bu öğretici rolleri framework bir kullanıcının rollerini kendi güvenlik bağlamı ile nasıl ilişkilendirdiğini bir göz başlar. Sonra rol tabanlı URL yetkilendirme kuralları uygulamak nasıl inceler. Biz görüntülenen verileri ve ASP.NET sayfası tarafından sunulan işlevselliği değiştirilmesi için bildirim temelli ve programlı bir şekilde kullanarak görüneceğini, aşağıdaki. Haydi başlayalım!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Anlama nasıl rolleri ile ilişkili bir kullanıcının güvenlik bağlamı

Bir isteği ASP.NET ardışık girer her istek tanımlama bilgileri içeren bir güvenlik bağlamı ile ilişkilidir. Forms kimlik doğrulaması kullanırken, kimlik doğrulama biletini kimlik belirteç olarak kullanılır. Biz anlatıldığı gibi <a id="_msoanchor_2"> </a> [ *form kimlik doğrulaması bir genel bakış* ](../introduction/an-overview-of-forms-authentication-cs.md) ve <a id="_msoanchor_3"> </a> [ *formlar Kimlik doğrulama yapılandırması ve Gelişmiş konular* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) öğreticileri, `FormsAuthenticationModule` sırasında mevcut istek sahibinin kimliğini belirlemek için sorumlu [ `AuthenticateRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Geçerli, süresi dolan olmayan kimlik doğrulama bileti bulunursa, `FormsAuthenticationModule` isteyenin kimliğini onaylaması için kodunu çözer. Yeni bir oluşturur `GenericPrincipal` nesne ve bunun için atar `HttpContext.User` nesnesi. Bir asıl amacı ister `GenericPrincipal`, kimliği doğrulanmış kullanıcının adını ve hangi belirlemek için mi ait rolleri. Bu amaçla tüm asıl nesneleri olan olgusu açıktır bir `Identity` özelliği ve bir `IsInRole(roleName)` yöntemi. `FormsAuthenticationModule`, Ancak rol bilgilerini kaydetme sırasında ilgilenen değildir ve `GenericPrincipal` oluşturduğu nesne herhangi bir rol belirtmiyor.

Rolleri framework etkinleştirilirse, [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) sonra HTTP modülü adımları `FormsAuthenticationModule` ve kimliği doğrulanmış kullanıcının rollerini sırasında tanımlayan [ `PostAuthenticateRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), hangi sonra ateşlenir `AuthenticateRequest` olay. İstek Kimliği doğrulanmış bir kullanıcıdan geliyorsa `RoleManagerModule` üzerine yazar `GenericPrincipal` tarafından oluşturulan nesne `FormsAuthenticationModule` ve ile değiştirir bir [ `RolePrincipal` nesne](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). `RolePrincipal` Sınıfı, kullanıcının ait olduğu için hangi rolleri belirlemek üzere rolleri API kullanır.

Şekil 1, form kimlik doğrulaması ve rolleri framework kullanılırken ASP.NET ardışık düzen iş akışı gösterilmektedir. `FormsAuthenticationModule` İlk yürütür, kullanıcının kendi kimlik doğrulama bileti aracılığıyla tanımlar ve yeni bir `GenericPrincipal` nesnesi. Ardından, `RoleManagerModule` adımları ve üzerine yazar `GenericPrincipal` nesnesi ile bir `RolePrincipal` nesnesi.

Adsız bir kullanıcı, siteyi ziyaret `FormsAuthenticationModule` veya `RoleManagerModule` asıl nesnesi oluşturur.


[![Form kimlik doğrulaması ve rolleri Framework kullanılırken kimliği doğrulanmış bir kullanıcı için ASP.NET ardışık düzenini olayları](role-based-authorization-cs/_static/image2.png)](role-based-authorization-cs/_static/image1.png)

**Şekil 1**: bir kimliği doğrulanmış kullanıcı olduğunda kullanarak form kimlik doğrulaması ve rolleri Framework için ASP.NET ardışık düzen olayları ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Rol bilgileri bir tanımlama bilgisinde önbelleğe alma

`RolePrincipal` Nesnenin `IsInRole(roleName)` yöntem çağrılarını `Roles.GetRolesForUser` kullanıcının üyesi olup olmadığını belirlemek için kullanıcı rollerini almak için *roleName*. Kullanırken `SqlRoleProvider`, bu rol deposu veritabanı için bir sorgu sonuçları. Rol tabanlı URL yetkilendirme kuralları kullanırken `RolePrincipal`'s `IsInRole` rol tabanlı URL yetkilendirme kuralları tarafından korunan bir sayfaya her istekte yöntemi çağrılır. Her istekte veritabanında rol bilgilerini aramak sahip yerine rolleri framework kullanıcının rolleri bir tanımlama bilgisinde önbelleğe almak için bir seçenek içerir.

Rolleri framework kullanıcının rolleri bir tanımlama bilgisinde önbelleğe almak için yapılandırılmışsa, `RoleManagerModule` ASP.NET ardışık düzen sırasında tanımlama bilgisi oluşturur [ `EndRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Bu tanımlama bilgisi sonraki isteklerinde kullanılan `PostAuthenticateRequest`, ne zaman olduğu `RolePrincipal` nesnesi oluşturulur. Tanımlama bilgisinin geçerli olduğu ve süresi geçmemiş, tanımlama bilgisi verileri ayrıştırılır ve kullanıcı rolleri, böylece kaydetme doldurmak için kullanılan `RolePrincipal` çağırmaya gerek kalmadan gelen `Roles` kullanıcının rollerini belirlemek için sınıf. Şekil 2, bu iş akışı gösterilmektedir.


[![Kullanıcının rolünü bilgileri performansını artırmak için bir tanımlama bilgisinde depolanabilir](role-based-authorization-cs/_static/image5.png)](role-based-authorization-cs/_static/image4.png)

**Şekil 2**: kullanıcının rol bilgilerini depolanabilir performansı artırmak için bir tanımlama bilgisinde ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image6.png))


Varsayılan olarak, rol önbellek tanımlama bilgisi mekanizması devre dışıdır. Aracılığıyla etkinleştirilebilir `<roleManager>` yapılandırma biçimlendirmede `Web.config`. Kullanma ele [ `<roleManager>` öğesi](https://msdn.microsoft.com/library/ms164660.aspx) rol sağlayıcıları belirtmek için <a id="_msoanchor_4"> </a> [ *oluşturma ve yönetme rolleri* ](creating-and-managing-roles-cs.md) öğretici Bu öğe zaten uygulamanızın olması gerekir böylece `Web.config` dosya. Rol önbellek tanımlama bilgisi ayarları özniteliklerini belirtilen `<roleManager>` öğesi ve bu tablo 1'de özetlenmiştir.

> [!NOTE]
> Tablo 1'de listelenen yapılandırma ayarlarını elde edilen rol önbellek tanımlama bilgisinin özelliklerini belirtin. Tanımlama bilgileri, nasıl çalıştığını ve çeşitli özellikleri hakkında daha fazla bilgi için okuma [bu tanımlama bilgileri öğretici](http://www.quirksmode.org/js/cookies.html).


| **Özelliği** | **Açıklama** |
| --- | --- |
| `cacheRolesInCookie` | Tanımlama bilgisi önbelleği kullanılıp kullanılmadığını gösteren bir Boole değeri. Varsayılan olarak `false`. |
| `cookieName` | Rol önbellek tanımlama bilgisinin adı. Varsayılan değer ". ASPXROLES". |
| `cookiePath` | Rol adı tanımlama bilgisi yolu. Yol özniteliği bir tanımlama bilgisi belirli dizin hiyerarşiye kapsamını sınırlandırmak bir geliştirici sağlar. Varsayılan değer "/", hangi kimlik doğrulama bileti tanımlama etki alanında yapılan herhangi bir istek göndermek için tarayıcı bildirir. |
| `cookieProtection` | Hangi teknikleri rol önbellek tanımlama bilgisinin korumak için kullanıldığını belirtir. İzin verilen değerler: `All` (varsayılan); `Encryption`; `None`; ve `Validation`. Adım 3'e yeniden bakın <a id="_anchor_5"> </a> [ *Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) öğretici Bu koruma düzeyleri hakkında daha fazla bilgi için. |
| `cookieRequireSSL` | Kimlik doğrulama tanımlama bilgisini iletmek için bir SSL bağlantısının gerekli olup olmadığını gösteren bir Boole değeri. Varsayılan değer `false` şeklindedir. |
| `cookieSlidingExpiration` | Her zaman sıfırlama tanımlama bilgisinin zaman aşımı olup olmadığını kullanıcı tek bir oturum sırasındaki siteyi ziyaret gösteren bir Boole değeri. Varsayılan değer `false` şeklindedir. Bu değer yalnızca uygun olduğunda `createPersistentCookie` ayarlanır `true`. |
| `cookieTimeout` | Saat geçtikten sonra kimlik doğrulama bileti tanımlama bilgisinin süresinin dakika cinsinden belirtir. Varsayılan değer `30` şeklindedir. Bu değer yalnızca uygun olduğunda `createPersistentCookie` ayarlanır `true`. |
| `createPersistentCookie` | Rol önbellek tanımlama bilgisinin bir oturum tanımlama bilgisi veya kalıcı bir tanımlama bilgisi olduğunu belirten bir Boole değeri. Varsa `false` (varsayılan), tarayıcı kapatıldığında, silinmiş oturum tanımlama bilgisi kullanılır. Varsa `true`, kalıcı bir tanımlama bilgisi kullanılır; süresi dolmadan `cookieTimeout` oluşturulduktan sonra dakika veya değerine bağlı olarak önceki ziyaret edin sonra sayı `cookieSlidingExpiration`. |
| `domain` | Tanımlama bilgisinin etki alanı değeri belirtir. Varsayılan değer tarayıcının içinden (www.yourdomain.com gibi) verilmiş etki alanını kullanmak boş bir dizedir. Bu durumda, tanımlama bilgisi olur **değil** yapma admin.yourdomain.com gibi alt etki alanları için istediğinde gönderilmeyecek. Tüm alt etki alanları için geçirilecek tanımlama bilgisi istiyorsanız özelleştirmeniz gerekiyorsa `domain` özniteliği, "etkialaniniz.com" ayarlama. |
| `maxCachedResults` | Önbelleğe alınan rol adları en fazla sayısını tanımlama bilgisinde belirtir. Varsayılan değer 25'tir. `RoleManagerModule` Ait kullanıcılar için bir tanımlama bilgisi oluşturmaz birden fazla `maxCachedResults` rolleri. Sonuç olarak, `RolePrincipal` nesnenin `IsInRole` yöntemi kullanacak `Roles` kullanıcının rollerini belirlemek için sınıf. Nedeni `maxCachedResults` var. çok sayıda kullanıcı aracıları 4.096 bayttan büyük tanımlama bilgilerine izin verme olmasıdır. Bu nedenle bu harfin bu boyutu sınırlaması aşılması olasılığını azaltmak için tasarlanmıştır. Aşırı uzun rol adları varsa, daha küçük belirtmeyi göz önünde bulundurun isteyebilirsiniz `maxCachedResults` değeri; contrariwise, son derece kısa rol adları varsa, bu değer büyük olasılıkla artırabilirsiniz. |

**Tablo 1:** rol önbellek tanımlama bilgisi yapılandırma seçenekleri

Şimdi uygulamamız kalıcı olmayan rol önbellek tanımlama bilgilerini kullanmak üzere yapılandırın. Bunu başarmak için güncelleştirme `<roleManager>` öğesinde `Web.config` tanımlama bilgisi ile ilgili aşağıdaki öznitelikler eklemek için:

[!code-xml[Main](role-based-authorization-cs/samples/sample1.xml)]

I güncelleştirilmiş `<roleManager>` üç öznitelikleri ekleyerek öğesi: `cacheRolesInCookie`, `createPersistentCookie`, ve `cookieProtection`. Ayarlayarak `cacheRolesInCookie` için `true`, `RoleManagerModule` kullanıcının rol bilgileri her istek için arama yapmak zorunda yerine artık otomatik olarak bir tanımlama bilgisi kullanıcının rollerinde önbelleğe alır. Açık olarak ayarlanıp `createPersistentCookie` ve `cookieProtection` özniteliklerini `false` ve `All`sırasıyla. Teknik olarak, kalıcı tanımlama bilgileri kullanmıyorum ve tanımlama bilgisi hem olduğunu ve şifrelenmiş doğrulanmış ı yalnızca varsayılan değerlerine atanmış, ancak ı bunları burada açıkça yapmak için put beri bu öznitelikler için değerleri temizleyin belirtmek ihtiyacım kalmadı.

Tüm olan İşte bu kadar! Henceforth, rolleri framework kullanıcıların rollerini tanımlama bilgilerini önbelleğe alır. Kullanıcının tarayıcı tanımlama bilgilerini desteklemez veya tanımlama bilgilerine silinmiş veya kayıp, şekilde, hiçbir sorun teşkil – olmasına `RolePrincipal` nesne yalnızca kullanacağınız `Roles` sınıfı tanımlama bilgisi (veya geçersiz veya süresi dolmuş bir) kullanılabilir durumda.

> [!NOTE]
> Microsoft'un desenleri &amp; yöntemler Grup zorlaştırır kalıcı rol önbellek tanımlama bilgilerini kullanma. Bir korsan şekilde geçerli kullanıcının tanımlama bilgisi erişebilir, rol önbellek tanımlama bilgisinin elinde rol üyeliğini kanıtlamak için yeterli olduğundan kendisinin o kullanıcının kimliğine bürünebilir. Kullanıcının tarayıcıda tanımlama bilgisinin kalıcı değilse bunun olasılığını artırır. Bu güvenlik önerisi yanı sıra diğer güvenlik sorunları hakkında daha fazla bilgi için başvurmak [ASP.NET 2.0 için Güvenlik sorusu listesi](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>1. adım: Rol tabanlı URL yetkilendirme kurallarını tanımlama

' Da anlatıldığı gibi <a id="_msoanchor_6"> </a> [ *kullanıcı tabanlı bir yetkilendirme* ](../membership/user-based-authorization-cs.md) öğretici, URL yetkilendirmesi, bir kullanıcı tarafından veya rol tarafından rol sayfalar kümesi erişimi sınırlamak için bir yol sunar temel. URL yetkilendirme kuralları içinde yazıldığından `Web.config` kullanarak [ `<authorization>` öğesi](https://msdn.microsoft.com/library/8d82143t.aspx) ile `<allow>` ve `<deny>` alt öğeleri. Önceki eğitimlerine ele alınan kullanıcı ilgili yetkilendirme kuralları yanı sıra her `<allow>` ve `<deny>` alt öğesi de dahil edebilirsiniz:

- Belirli bir rol
- Rollerin virgülle ayrılmış bir listesi

Örneğin, URL yetkilendirme kuralları kullanıcılarla yöneticileri ve denetçiler rollerdeki erişim vermek ancak diğerlerini erişimini:

[!code-xml[Main](role-based-authorization-cs/samples/sample2.xml)]

`<allow>` Yukarıdaki biçimlendirme öğesinde durumları Yöneticiler ve denetçiler rollerini verildiğini; `<deny>` öğesi bildirir *tüm* kullanıcılar engellenir.

Uygulamamız yapılandıralım böylece `ManageRoles.aspx`, `UsersAndRoles.aspx`, ve `CreateUserWizardWithRoles.aspx` sayfalarıdır yalnızca Yöneticiler rolündeki kullanıcılar için erişilebilir sırada `RoleBasedAuthorization.aspx` sayfa tüm ziyaretçiler için erişilebilir kalır.

Bunu gerçekleştirmek için ekleyerek başlayın bir `Web.config` dosya `Roles` klasör.


[![Rolleri dizinine bir Web.config dosyası ekleme](role-based-authorization-cs/_static/image8.png)](role-based-authorization-cs/_static/image7.png)

**Şekil 3**: ekleme bir `Web.config` dosya `Roles` dizini ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image9.png))


Ardından, aşağıdaki yapılandırma biçimlendirme eklemek `Web.config`:

[!code-xml[Main](role-based-authorization-cs/samples/sample3.xml)]

`<authorization>` Öğesinde `<system.web>` bölümü gösterir yalnızca kullanıcılar Yöneticiler rolünde ASP.NET kaynaklarına erişebilir `Roles` dizin. `<location>` Öğesi tanımlar bir alternatif URL yetkilendirme kuralları kümesi `RoleBasedAuthorization.aspx` sayfası, tüm kullanıcıların sayfasını ziyaret edin olanak tanır.

Değişikliklerinizi kaydetmeden sonra `Web.config`, yöneticiler rolünün olmayan bir kullanıcı olarak oturum açın ve sonra korumalı sayfaları birini ziyaret deneyin. `UrlAuthorizationModule` İstenen kaynak; ziyaret etmek için izne sahip değil algılar sonuç olarak, `FormsAuthenticationModule` oturum açma sayfasına yönlendirir. Oturum açma sayfasına sonra size yönlendirir `UnauthorizedAccess.aspx` sayfa (Şekil 4'e bakın). Bu son yeniden yönlendirme için oturum açma sayfasından `UnauthorizedAccess.aspx` eklediğimiz 2. adım oturum açma sayfası kod nedeniyle oluşur <a id="_msoanchor_7"> </a> [ *kullanıcı tabanlı bir yetkilendirme* ](../membership/user-based-authorization-cs.md) Öğreticisi. Otomatik olarak herhangi bir kimliği doğrulanmış kullanıcı özellikle, oturum açma sayfasına yönlendirir `UnauthorizedAccess.aspx` sorgu dizesini içeriyorsa, bir `ReturnUrl` parametresi, bu parametre olarak gösterir kendisinin değildi sayfasını görüntülemek denemeden sonra kullanıcı oturum açma sayfasına geldiğini görüntüleme izni.


[![Yalnızca Yöneticiler rolündeki kullanıcılar korumalı sayfaları görüntüleyebilir](role-based-authorization-cs/_static/image11.png)](role-based-authorization-cs/_static/image10.png)

**Şekil 4**: yalnızca Yöneticiler rolündeki kullanıcılar, korumalı sayfaları görüntüleyebilirsiniz ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image12.png))


Oturumu kapatın ve yöneticiler rolünün bir kullanıcı olarak oturum açın. Artık üç korumalı sayfaları görmeye olması gerekir.


[![Tito ziyaret UsersAndRoles.aspx sayfa çünkü kendisine Yöneticiler rolde olup](role-based-authorization-cs/_static/image14.png)](role-based-authorization-cs/_static/image13.png)

**Şekil 5**: Tito ziyaret `UsersAndRoles.aspx` sayfa çünkü kendisine olan yöneticiler rolünün ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image15.png))


> [!NOTE]
> URL yetkilendirme kuralları – rollerin veya kullanıcıların – belirtirken çözümlenen birer birer, üstten aşağı kurallardır göz önünde bulundurmanız önemlidir. Bir eşleşme olarak kullanıcı verilen veya erişimi reddedilen, açıksa bağlı olarak eşleşmenin bir `<allow>` veya `<deny>` öğesi. **Eşleşme bulunamazsa, kullanıcıya erişim verilir.** Bir veya daha fazla kullanıcı hesaplarına erişimi kısıtlamak istiyorsanız, bu nedenle, onu kullanmanız zorunludur bir `<deny>` öğesi URL yetkilendirme yapılandırma son öğesi olarak. **URL yetkilendirme kuralları dahil etmezseniz bir**`<deny>`**öğesi, tüm kullanıcılara erişim verilecektir.** URL yetkilendirme kuralları nasıl analiz bir daha kapsamlı bilgi için geri başvurun "nasıl göz `UrlAuthorizationModule` erişim vermek veya reddetmek için yetkilendirme kuralları kullanır" bölümünü <a id="_msoanchor_8"> </a> [  *Kullanıcı tabanlı bir yetkilendirme* ](../membership/user-based-authorization-cs.md) Öğreticisi.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>2. adım: şu anda oturum açmış kullanıcının rollere göre işlevselliği sınırlama

URL yetkilendirme yapar kaba yetkilendirme belirtmek kolay, bu durumda hangi kimlikleri kuralları izin verilen ve hangilerinin belirli bir sayfa (veya bir klasöründeki ve onun alt klasörlerindeki tüm sayfaları) görüntüleme reddedilir. Ancak, belirli durumlarda biz bir sayfasını ziyaret edin, ancak ziyaret kullanıcı rollerine göre sayfanın işlevselliği sınırlamak tüm kullanıcılara izin vermek isteyebilirsiniz. Bu kullanıcı rolüne göre veya belirli bir role ait kullanıcılar için ek işlevler sunumu gösterme veya gizleme veri gerektirdiği.

Bu tür ince çizgisi rol tabanlı yetkilendirme kuralları, bildirimli olarak veya program aracılığıyla (ya da iki şunların bir birleşimi yoluyla) uygulanabilir. Sonraki bölümde nasıl bildirim temelli ince çizgisi yetkilendirme LoginView denetimi aracılığıyla uygulanacağı göreceğiz. Programlı teknikleri inceleyeceksiniz. Biz ince çizgisi yetkilendirme kurallarını uygulayarak en bakabilirsiniz önce ancak biz öncelikle olan işlevselliği, ziyaret eden kullanıcı role dayalı olarak değişir bir sayfa oluşturmanız gerekir.

Tüm kullanıcı hesaplarını GridView'ndaki Sistem listeleyen bir sayfa oluşturalım. GridView, her kullanıcının kullanıcı adı, e-posta adresi, son oturum açma tarihi ve kullanıcı hakkındaki açıklamalar dahil edilir. Her kullanıcının bilgilerini görüntülemeye ek olarak, GridView ekleme Düzenle ve yetenekleri silin. Başlangıçta bu sayfayı düzenleme oluşturur ve silme işlevlerinin tüm kullanıcılar tarafından kullanılabilir. Etkinleştirme veya ziyaret kullanıcı rolüne dayalı bu özellikleri devre dışı bırakma "LoginView denetimi kullanma" ve "Program aracılığıyla sınırlama işlevselliği" bölümlerde göreceğiz.

> [!NOTE]
> Yaklaşık yapı duyuyoruz ASP.NET sayfası kullanıcı hesaplarını görüntülemek için GridView denetimini kullanır. Form kimlik doğrulaması, yetkilendirme, kullanıcı hesapları ve rolleri serisi odaklanan Bu öğretici itibaren çalışmalar GridView denetimini ele çok fazla süre beklemesini istemiyorum. Bu öğretici bu sayfası ayarlama belirli adım adım yönergeler sağlar, ancak bunu neden belirli seçimler yapılan veya ne işlenmiş çıktı etkisi belirli özelliklere sahip ayrıntılarını içine inceleyin değil. GridView denetiminin kapsamlı incelenmesi için kullanıma my  *[, ASP.NET 2.0 verilerle çalışma](../../data-access/index.md)*  öğretici serisi.


Başlangıç açarak `RoleBasedAuthorization.aspx` sayfasındaki `Roles` klasör. GridView Tasarımcısı ve kümesi sayfaya sürükleyin kendi `ID` için `UserGrid`. Birazdan biz çağıran kodu yazacak `Membership.GetAllUsers` yöntemi ve elde edilen bağlar `MembershipUserCollection` GridView nesnesine. `MembershipUserCollection` İçeren bir `MembershipUser` sistemindeki; her kullanıcı hesabı için nesnesi `MembershipUser` nesneler gibi özelliklere sahip `UserName`, `Email`, `LastLoginDate`, vb.

Şimdi biz kullanıcı hesaplarını kılavuza bağlar kod yazmadan önce ilk GridView'ın alanları tanımlayın. GridView kullanıcının akıllı etiketten alanları iletişim başlatmak için "Edit Columns" bağlantısını tıklatın (bkz. Şekil 6) kutusunda. Buradan, sol alt köşedeki "alanlar otomatik oluştur" onay kutusunun işaretini kaldırın. Bir CommandField eklemek dahil düzenleme ve özellikleri, silme ve belirlemek için bu GridView istiyoruz beri kendi `ShowEditButton` ve `ShowDeleteButton` özellikleri true. Ardından, görüntülemek için dört alanları ekleyin `UserName`, `Email`, `LastLoginDate`, ve `Comment` özellikleri. İki salt okunur özellikler için bir BoundField kullanın (`UserName` ve `LastLoginDate`) ve iki düzenlenebilir alanlar için TemplateFields (`Email` ve `Comment`).

İlk BoundField görüntü sahip `UserName` özellik; kümesi kendi `HeaderText` ve `DataField` "UserName" özellikleri. Bu alan olacak şekilde ayarlamanız düzenlenebilir olmaz kendi `ReadOnly` özelliğinin True. Yapılandırma `LastLoginDate` ayarlayarak BoundField kendi `HeaderText` "Son oturum açma" ve kendi `DataField` "LastLoginDate" için. Şimdi yalnızca tarih (tarih ve saat yerine) görüntülenmesi için bu BoundField çıktısını biçimlendirin. Bunu gerçekleştirmek için bu BoundField's ayarlamak `HtmlEncode` özelliğini false olarak ayarlayın ve onun `DataFormatString` özelliğini "{0: d}". Ayrıca `ReadOnly` özelliğinin True.

Ayarlama `HeaderText` "E-posta" ve "Comment" iki TemplateFields özelliklerini.


[![GridView'ın alanları alanları iletişim kutusu aracılığıyla yapılandırılabilir](role-based-authorization-cs/_static/image17.png)](role-based-authorization-cs/_static/image16.png)

**Şekil 6**: GridView's alanları olabilir olması yapılandırılmış aracılığıyla alanları iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image18.png))


Şimdi tanımlamak ihtiyacımız `ItemTemplate` ve `EditItemTemplate` "E-posta" ve "Comment" TemplateFields. Bir etiket Web denetimi her birine ekleyin `ItemTemplate` s ve bağ kendi `Text` özelliklerine `Email` ve `Comment` özellikleri, sırasıyla.

"E-posta" TemplateField için adlı bir TextBox ekleyin `Email` için kendi `EditItemTemplate` ve bağlama kendi `Text` özelliğine `Email` iki yönlü veri bağlamasını kullanma özelliği. Bir RequiredFieldValidator ekleyip RegularExpressionValidator için `EditItemTemplate` e-posta özelliği düzenleme ziyaretçisi geçerli bir eposta adresi girdiğinden emin olmak için. "Comment" TemplateField için adlı çok satırlı metin kutusu ekleyin `Comment` için kendi `EditItemTemplate`. TextBox's ayarlamak `Columns` ve `Rows` özelliklerine 40 ve 4 ' sırasıyla ve ardından bağlamak kendi `Text` özelliğine `Comment` iki yönlü veri bağlamasını kullanma özelliği.

Bu TemplateFields yapılandırdıktan sonra bunların bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](role-based-authorization-cs/samples/sample4.aspx)]

Düzenlerken veya biz kullanıcının bilmeniz gereken bir kullanıcı hesabının silinmesi `UserName` özellik değeri. GridView's ayarlamak `DataKeyNames` "UserName" özelliğine böylece bu bilgileri GridView kişinin kullanılabilir `DataKeys` koleksiyonu.

Son olarak, bir ValidationSummary denetimi sayfasına ekleyin ve ayarlayın, `ShowMessageBox` özelliği True olarak ve kendi `ShowSummary` özelliğini false olarak ayarlayın. Bu ayarlarla kullanıcının eksik veya geçersiz bir e-posta adresi olan bir kullanıcı hesabı düzenlemek denerse ValidationSummary bir istemci-tarafı uyarısı görüntülenir.

[!code-aspx[Main](role-based-authorization-cs/samples/sample5.aspx)]

Biz bu sayfanın bildirim temelli biçimlendirme tamamladınız. Bizim sonraki kullanıcı hesapları kümesi GridView bağlamak için bir görevdir. Adlı bir yöntem ekleyin `BindUserGrid` için `RoleBasedAuthorization.aspx` bağlar sayfanın arka plandaki kod sınıfı `MembershipUserCollection` tarafından döndürülen `Membership.GetAllUsers` için `UserGrid` GridView. Bu yöntemi çağırın `Page_Load` olay işleyicisini ilk sayfasını ziyaret edin.

[!code-csharp[Main](role-based-authorization-cs/samples/sample6.cs)]

Bu kod yerinde bir tarayıcı aracılığıyla sayfasını ziyaret edin. Şekil 7'de görüldüğü gibi sistemdeki her kullanıcı hesabıyla ilgili bilgileri listeleyen bir GridView görmeniz gerekir.


[![UserGrid GridView her kullanıcı hakkındaki bilgileri sistemde listeler.](role-based-authorization-cs/_static/image20.png)](role-based-authorization-cs/_static/image19.png)

**Şekil 7**: `UserGrid` GridView listeler bilgi hakkında her kullanıcı sisteminde ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image21.png))


> [!NOTE]
> `UserGrid` GridView disk belleği olmayan arabiriminde tüm kullanıcıları listeler. Bu basit ızgara arabirim senaryoları için uygun değil birkaç düzine veya daha fazla kullanıcı olduğu. Disk belleği etkinleştirmek için GridView yapılandırma bir seçenektir. `Membership.GetAllUsers` Yöntemi iki aşırı sahiptir: hiç giriş parametresi kabul eder ve tüm kullanıcıların ve sayfa dizini ve sayfa boyutu tamsayı değerlerini alır ve yalnızca belirtilen kullanıcı alt kümesini döndüren bir döndürür. Kullanıcı hesapları yalnızca kesin kısmı döndürdüğünden ikinci aşırı kullanıcıları daha verimli bir şekilde bir sayfadan kullanılabilir yerine *tüm* bunların. Kullanıcı hesapları binlerce varsa, filtre tabanlı bir arabirim, yalnızca kullanıcı adı seçilen bir karakterle örneği için bu kullanıcıları gösteren bir isteyebilirsiniz. [ `Membership.FindUsersByName method` ](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) Bir filtre tabanlı kullanıcı arabirimi oluşturmak için idealdir. Böyle bir arabirim gelecekteki bir öğreticide derlemeye ele alacağız.


GridView denetiminin yerleşik düzenleme ve Denetim doğru yapılandırılmış veri kaynağı denetimi, SqlDataSource veya ObjectDataSource gibi bağlandığında destek silme sunar. `UserGrid` GridView, Bununla birlikte, kendi veri program aracılığıyla bağlı vardır; bu nedenle, biz bu iki görevleri gerçekleştirmek için kod yazmanız gerekir. Özellikle, biz olay işleyicileri GridView için 's oluşturmanız gereken `RowEditing`, `RowCancelingEdit`, `RowUpdating`, ve `RowDeleting` ziyaretçi GridView ait tıklattığında tetiklenen olayları, düzenleme, iptal, güncelleştirme veya silme düğmeleri.

Başlangıç GridView için 's olay işleyicileri oluşturarak `RowEditing`, `RowCancelingEdit`, ve `RowUpdating` olayları ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](role-based-authorization-cs/samples/sample7.cs)]

`RowEditing` Ve `RowCancelingEdit` olay işleyicileri yalnızca GridView's ayarlamak `EditIndex` özelliğini ve kullanıcı listesini hesapları kılavuza sonra yeniden bağlayın. İçinde ilginç öğe olur `RowUpdating` olay işleyicisi. Veriler geçerli değil ve ardından alan sağlayarak bu olay işleyicisini başlatır `UserName` düzenlenen kullanıcı hesabından değerini `DataKeys` koleksiyonu. `Email` Ve `Comment` iki TemplateFields kutularındaki `EditItemTemplate` s ardından program aracılığıyla başvuru. Kendi `Text` düzenlenen e-posta adresi ve açıklama özellikleri içerir.

İhtiyacımız ilk biz çağrısıyla mu kullanıcının bilgi almak bir kullanıcı hesabı üyelik API'si aracılığıyla güncelleştirmek için `Membership.GetUser(userName)`. Döndürülen `MembershipUser` nesnenin `Email` ve `Comment` özelliklerini düzenleme arabiriminden iki metin kutuları girdiğiniz değerlerle sonra güncelleştirilir. Son olarak, bu değişiklikleri çağrısıyla kaydedilir [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). `RowUpdating` Olay işleyicisi tamamlandıktan önceden düzenleme arabirimine GridView dönerek.

Ardından, oluşturun `RowDeleting` olay işleyicisi ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](role-based-authorization-cs/samples/sample8.cs)]

Yukarıdaki olay işleyicisi kapmasını tarafından başlatılır `UserName` GridView'ın değerinden `DataKeys` koleksiyonu; bu `UserName` değeri üyelik sınıfının ardından geçirilen [ `DeleteUser` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). `DeleteUser` Yöntemi (hangi rolleri gibi bu kullanıcının ait olduğu) ilgili üyelik verileri de dahil olmak üzere sistemden kullanıcı hesabını siler. Kullanıcının, kılavuz sildikten sonra `EditIndex` (başka bir satır düzenleme modunda çalışırken kullanıcı silme tıklattınız durumda) -1 olarak ayarlanır ve `BindUserGrid` yöntemi çağrılır.

> [!NOTE]
> Sil düğmesini herhangi bir tür kullanıcı hesabını silmeden önce kullanıcıdan onay gerektirmez. Yanlışlıkla silinen bir hesap olasılığını azaltmak için kullanıcı onayı çeşit eklemenizi öneririz. Eylemi onaylamak için en kolay yollarından biri bir istemci-tarafı Onayla iletişim kutusudur. Bu teknik hakkında daha fazla bilgi için bkz: [ekleme istemci-tarafı onay olduğunda silme](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Bu sayfayı beklendiği gibi çalıştığını doğrulayın. Yanı sıra herhangi bir kullanıcının e-posta adresi ve yorum Düzenle herhangi bir kullanıcı hesabı silmek mümkün olması gerekir. Bu yana `RoleBasedAuthorization.aspx` sayfa tüm kullanıcılar için erişilebilir, herhangi bir kullanıcı – bile anonim ziyaretçiler – bu sayfasını ziyaret edin ve düzenleyebilir ve kullanıcı hesapları silebilirsiniz! Şimdi bu sayfa yalnızca kullanıcılar denetçiler ve yöneticiler rollerinde bir kullanıcının e-posta adresini ve yorum düzenleyebilir ve yalnızca Yöneticiler kullanıcı hesabı silebilirsiniz şekilde güncelleştirin.

"LoginView denetimi kullanma" bölümüne yönergeleri kullanıcı rolü için belirli göstermek için LoginView denetimi kullanarak arar. Yöneticiler rolünün bir kişi bu sayfayı ziyaret eder, düzenlemek ve kullanıcıları silme hakkında yönergeler göstereceğiz. Denetçiler rolündeki bir kullanıcı bu sayfayı ulaşırsa, yönergeleri kullanıcıları düzenleme göstereceğiz. Ve ziyaretçi anonim olduğu veya denetçiler ya da yöneticiler rolde değil, biz bunlar düzenleyemez veya kullanıcı hesabı bilgilerini silmek olduğunu belirten bir ileti görüntülenir. "Program aracılığıyla sınırlama işlevselliği" bölümünde biz program aracılığıyla gösterir veya gizler kullanıcı rolüne dayalı düzenleme ve silme düğmeleri kod yazacaksınız.

### <a name="using-the-loginview-control"></a>LoginView denetimi kullanma

Geçmiş eğitimlerine anlatıldığı gibi LoginView denetimi kimliği doğrulanmış ve anonim kullanıcılar için farklı arabirimlerini görüntülemek için yararlıdır, Ancak LoginView denetimi, kullanıcı rollerine göre farklı bir biçimlendirme görüntülemek için de kullanılabilir. Şimdi LoginView denetimi ziyaret kullanıcı rolüne göre farklı yönergeleri görüntülemek için kullanın.

Yukarıdaki bir LoginView ekleyerek başlangıç `UserGrid` GridView. Biz daha önce bahsedilen gibi iki yerleşik şablonlar LoginView denetimi vardır: `AnonymousTemplate` ve `LoggedInTemplate`. Kısa bir ileti hem de kullanıcı bunlar düzenleyemez veya silemezsiniz herhangi bir kullanıcı bilgisi olduğunu bildiren Bu şablonların girin.

[!code-aspx[Main](role-based-authorization-cs/samples/sample9.aspx)]

Ek olarak `AnonymousTemplate` ve `LoggedInTemplate`, LoginView denetimi içerebilir *RoleGroups*, role özel şablonları olduğu. Her RoleGroup tek bir özellik içeren `Roles`, RoleGroup uygulandığı hangi rollerin belirtir. `Roles` Özelliği ayarlanabilir (örneğin, "Yöneticiler") tek bir rol veya rolleri (örneğin, "Yöneticiler, denetçiler") virgülle ayrılmış listesi.

RoleGroups yönetmek için yukarı RoleGroup koleksiyon Düzenleyicisi'ni getirmek için denetimin akıllı etiket "RoleGroups Düzenle" bağlantısını tıklatın. İki yeni RoleGroups ekleyin. İlk RoleGroup's ayarlamak `Roles` özelliğini "Yöneticiler" ve ikinci 's "Denetçiler".


[![(Link)'ın Role özgü şablonlarını aracılığıyla RoleGroup koleksiyon Düzenleyicisi'ni yönetme](role-based-authorization-cs/_static/image23.png)](role-based-authorization-cs/_static/image22.png)

**Şekil 8**: (link) 's Role özgü şablonları RoleGroup koleksiyon Düzenleyicisi aracılığıyla yönetmek ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image24.png))


RoleGroup koleksiyon Düzenleyicisi'ni kapatmak için Tamam'ı tıklatın; Bu dahil etmek için bildirim temelli biçimlendirme (link)'ın güncelleştirmeleri bir `<RoleGroups>` ile bölümünde bir `<asp:RoleGroup>` alt öğesi her RoleGroup için tanımlanan RoleGroup koleksiyon Düzenleyicisi'nde. Ayrıca, "Görünüm" aşağı açılan listeden (link) kullanıcının akıllı başlangıçta listelenen etiketinde - yalnızca `AnonymousTemplate` ve `LoggedInTemplate` – artık ek RoleGroups de içerir.

Denetçiler roldeki kullanıcılar kullanıcı hesapları, düzenleme ve silme yönergelerini Yöneticiler rolündeki kullanıcılar gösterilmese düzenlemek görüntülenen yönergeler; böylece RoleGroups düzenleyin. Bu değişiklikleri yaptıktan sonra LoginView'ın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir.

[!code-aspx[Main](role-based-authorization-cs/samples/sample10.aspx)]

Bu değişiklikleri yaptıktan sonra sayfanın kaydedin ve bir tarayıcı ziyaret edin. Anonim kullanıcı olarak ilk sayfasını ziyaret edin. İletinin gösterilmesi gereken, ", sisteme günlüğe kaydedilmez. Bu nedenle, düzenleyemez veya herhangi bir kullanıcı bilgisi silemezsiniz." Sonra kimliği doğrulanmış bir kullanıcı, ancak ne denetçiler ya da yöneticiler rolündeki bir olarak oturum açın. Bu süre ", denetçiler ya da yöneticiler rollerinin bir üyesi değildir. Bu iletiyi görmeniz gerekir Bu nedenle, düzenleyemez veya herhangi bir kullanıcı bilgisi silemezsiniz."

Ardından, denetçiler rolünün bir üyesi olan bir kullanıcı oturum açın. İleti role özgü denetçiler gördüğünüz bu saati (bkz. Şekil 9). Ve yöneticiler role özgü gördüğünüz rol iletisi (bkz. Şekil 10) yöneticileri, bir kullanıcı olarak oturum açın.


[![Bruce'a denetçiler Role özgü iletisi görüntülenir](role-based-authorization-cs/_static/image26.png)](role-based-authorization-cs/_static/image25.png)

**Şekil 9**: Bruce'a denetçiler Role özgü iletisi gösterilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image27.png))


[![Tito Yöneticiler Role özgü iletisi gösterilir](role-based-authorization-cs/_static/image29.png)](role-based-authorization-cs/_static/image28.png)

**Şekil 10**: Tito Yöneticiler Role özgü iletisi gösterilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image30.png))


Birden çok şablonları uygulamak olsa bile (link) yalnızca tek bir şablonda Şekil 9'daki ekran görüntüleri ve 10 Göster işler. Bruce'a ve Tito hem de oturum kullanıcılar henüz yalnızca eşleşen RoleGroup (link) oluşturur ve `LoggedInTemplate`. Ayrıca, hem yöneticilerin hem de Denetçiler rollerine Tito ait henüz LoginView denetimi yöneticileri role özgü şablon denetçiler yerine bir işler.

Şekil 11 LoginView denetimi tarafından işlenecek şablonu belirlemek için kullanılan iş akışı gösterilmiştir. Varsa birden fazla RoleGroup belirtilen, LoginView şablon işler Not *ilk* eşleşen RoleGroup. Biz ilk RoleGroup olarak denetçiler RoleGroup ve ikinci olarak yöneticiler girdiyseniz Tito bu sayfayı ziyaret edildiğinde diğer bir deyişle, ardından kendisinin denetçiler ileti görür.


[![İşleme için şablonu belirlemek için LoginView denetimin iş akışı](role-based-authorization-cs/_static/image32.png)](role-based-authorization-cs/_static/image31.png)

**Şekil 11**: belirleme ne şablonuna işleme için LoginView denetimin iş akışı ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Program aracılığıyla sınırlama işlevi

LoginView denetimi sayfasını ziyaret kullanıcı rolüne göre farklı yönergeleri gösterirken, düzenleme ve İptal düğmeleri tümüne görünür kalır. Program aracılığıyla anonim ziyaretçiler ve denetçiler ne yöneticileri rolü olan kullanıcılar için düzenleme ve silme düğmeleri gizlemek gerekir. Yönetici olmayan herkes için de Sil düğmesini gizlemek gerekir. Bunu gerçekleştirmek için şu biraz CommandField'ın düzenleme ve silme LinkButtons ve ayarlar programlı olarak başvuran kod yazacaksınız kendi `Visible` özelliklerine `false`gerekirse,.

Program aracılığıyla bir CommandField denetimlerinde başvurmak için kolay bir şablona dönüştürmeniz için yoludur. Bunu gerçekleştirmek için GridView kullanıcının akıllı etiket "Edit Columns" bağlantısını tıklatın, CommandField geçerli alanlar listesinden seçin ve "Bu alan TemplateField'a dönüştürme" bağlantısını tıklatın. Bu CommandField TemplateField ile kapatır bir `ItemTemplate` ve `EditItemTemplate`. `ItemTemplate` Düzenleme ve silme LinkButtons sırasında içeren `EditItemTemplate` iptal LinkButtons ve güncelleştirme barındırır.


[![CommandField TemplateField'a Dönüştür](role-based-authorization-cs/_static/image35.png)](role-based-authorization-cs/_static/image34.png)

**Şekil 12**: CommandField içine bir TemplateField dönüştürme ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image36.png))


Düzenle ve Sil LinkButtons içinde güncelleştirme `ItemTemplate`, ayar kendi `ID` değerlerini özelliklerine `EditButton` ve `DeleteButton`sırasıyla.

[!code-aspx[Main](role-based-authorization-cs/samples/sample11.aspx)]

Veri GridView bağlı olduğunda, GridView kayıtları numaralandırır kendi `DataSource` özelliği ve karşılık gelen oluşturur `GridViewRow` nesnesi. Her `GridViewRow` nesnesi oluşturulur, `RowCreated` olay tetiklenir. Yetkisiz kullanıcılar için düzenleme ve silme düğmeleri gizlemek için bu olay için bir olay işleyicisi oluşturma ve düzenleme ve silme ayarını LinkButtons programlı olarak başvurmak için ihtiyacımız kendi `Visible` özellikleri uygun şekilde.

Olay işleyicisi oluşturun `RowCreated` olay ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](role-based-authorization-cs/samples/sample12.cs)]

Aklınızda `RowCreated` olay harekete *tüm* üstbilgi, altbilgi, çağrı cihazı arabirimi ve diğerleri de dahil olmak üzere GridView satır. Yalnızca (düzenleme modunda satır güncelleştir ve İptal düğmeleri düzenleme ve silme yerine olduğundan) size bir veri satırı düzenleme modunda ile çalışıyorsanız, düzenleme ve silme LinkButtons programlı olarak başvurmak istiyoruz. Bu onay tarafından işlenen `if` deyimi.

Düzenleme modunda değil bir veri satırı ile çalışıyorsanız, düzenleme ve silme LinkButtons başvurulan ve bunların `Visible` özellikleri ayarlandığında tabanlı tarafından döndürülen Boole değerleri `User` nesnesinin `IsInRole(roleName)` yöntemi. Kullanıcı nesnesi tarafından oluşturulan asıl başvuran `RoleManagerModule`; sonuç olarak, `IsInRole(roleName)` yöntemi geçerli ziyaretçi ait olup olmadığını belirlemek için rolleri API kullanan *roleName*.

> [!NOTE]
> Rolleri sınıfı doğrudan, biz kullanabilirdik çağrısı değiştirme `User.IsInRole(roleName)` çağrısıyla [ `Roles.IsUserInRole(roleName)` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Asıl nesnenin kullanmak karar `IsInRole(roleName)` yöntemi bu örnekte, rolleri API doğrudan kullanmaktan daha etkili olduğundan. Bu öğreticide daha önce bir tanımlama bilgisi kullanıcının rollerinde önbelleğe almak için Rol Yöneticisi yapılandırılmış. Bu tanımlama bilgisi verileri önbelleğe yalnızca ne zaman göstermesi sorumlusunun `IsInRole(roleName)` yöntemi çağrılır; doğrudan rolleri API çağrıları seyahat rol deposu için her zaman içerir. Rolleri bir tanımlama bilgisinde önbelleğe alınmamış olsa bile, asıl nesnenin çağırma `IsInRole(roleName)` yöntemi nedeni genellikle daha verimli bir istek sırasında ilk kez sonuçları önbelleğe alır için ne zaman çağrılır. Rolleri API, diğer yandan, önbelleğe alma işlemini gerçekleştirir. Çünkü `RowCreated` olay şu anda GridView her satır için kullanarak `User.IsInRole(roleName)` ancak rol deposu için yalnızca bir seyahat içerir `Roles.IsUserInRole(roleName)` gerektirir *N* dönüşle nerede *N* olduğu Kılavuzda görüntülenen kullanıcı hesaplarının sayısı.


Düzen düğmenin `Visible` özelliği ayarlanmış `true` bu sayfayı ziyaret eden kullanıcının Administrators veya denetçiler; roldeyse, aksi takdirde ayarlanır `false`. Delete düğmenin `Visible` özelliği ayarlanmış `true` yalnızca kullanıcı Yöneticiler rolüne ise.

Bu sayfa bir tarayıcı aracılığıyla sınayın. Anonim bir ziyaretçi veya bir yönetici ya da bir yönetici olan bir kullanıcı olarak sayfasını ziyaret edin, CommandField boş ise. hala var ancak ince Silver düzenleme veya silme olmadan olarak düğmeler.

> [!NOTE]
> CommandField Gizle mümkündür tamamen ne zaman bir yönetici olmayan ve yönetici olmayan ziyaret sayfası. I bunu bir alıştırma olarak okuyucuya bırakın.


[![Düzenle ve Sil düğmeleri olmayan denetçiler ve yönetici olmayanlar için gizli](role-based-authorization-cs/_static/image38.png)](role-based-authorization-cs/_static/image37.png)

**Şekil 13**: Düzenle ve Sil düğmeleri gizli olmayan denetçiler ve yönetici olmayanlar için ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image39.png))


Denetçiler rol (ancak yöneticiler rolünün) ait bir kullanıcı ziyaret, yalnızca Düzenle düğmesini görür.


[![Düzenle düğmesi denetçiler kullanılabilir olsa da, Sil düğmesini gizli](role-based-authorization-cs/_static/image41.png)](role-based-authorization-cs/_static/image40.png)

**Şekil 14**: sırada Düzenle düğmesini denetçiler için kullanılabilir, Sil düğmesini gizli ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image42.png))


Ve yönetici ziyaret eder, aynen düzenleme ve silme düğmeleri erişimi vardır.


[![Düzenleme ve silme düğmeleri kullanılabilir yalnızca Yöneticiler için hazırlanmıştır](role-based-authorization-cs/_static/image44.png)](role-based-authorization-cs/_static/image43.png)

**Şekil 15**: Düzenle ve Sil düğmeleri, kullanılabilir yalnızca Yöneticiler için ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>3. adım: Sınıflar ve yöntemler için rol tabanlı yetkilendirme kuralları uygulama

Denetçiler ve yöneticiler rollerdeki kullanıcılar yetenekleri biz sınırlı 2. adımda düzenleyebilir ve yalnızca Yöneticiler olanağı silebilirsiniz. Bu, yetkisiz kullanıcıların programlama teknikleri ile ilişkili kullanıcı arabirimi öğelerinin gizleme tarafından gerçekleştirilmiştir. Bu tür ölçüleri yetkisiz bir kullanıcının ayrıcalıklı bir eylem gerçekleştiremez olacağını garanti değil. Daha sonra eklenen kullanıcı arabirimi öğeleri veya biz yetkisiz kullanıcılar için gizlemek unuttunuz olabilir. Veya bir bilgisayar korsanının istenen yöntemi yürütmek için ASP.NET sayfası almak için başka bir şekilde bulabilirsiniz.

Bu sınıf veya yöntemin ile işaretleme işlevsellik belirli bir parçasını yetkisiz kullanıcılar tarafından erişilen emin olmak için kolay bir yoludur [ `PrincipalPermission` özniteliği](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). .NET çalışma zamanı sınıf kullanır veya yöntemlerinden birini yürütür, geçerli güvenlik bağlamı izni olduğundan emin olmak için denetler. `PrincipalPermission` Özniteliği üzerinden biz tanımlayabilirsiniz bu kurallar bir mekanizma sağlar.

Kullanarak Aranan `PrincipalPermission` geri özniteliğini <a id="_msoanchor_9"> </a> [ *kullanıcı tabanlı bir yetkilendirme* ](../membership/user-based-authorization-cs.md) Öğreticisi. GridView's tasarlamanız nasıl özellikle gördüğümüz `SelectedIndexChanged` ve `RowDeleting` olay işleyicisi böylece bunlar yalnızca kimliği doğrulanmış kullanıcılar ve Tito, tarafından sırasıyla yürütülebilir. `PrincipalPermission` Özniteliği rolleriyle da çalışır.

Kullanarak gösterelim `PrincipalPermission` GridView'ın öznitelikte `RowUpdating` ve `RowDeleting` yetkili olmayan kullanıcılar için yürütme engellemek için olay işleyicileri. Yapmamız gereken tek şey her işlev tanımı üzerinde uygun özniteliği ekleyin:

[!code-csharp[Main](role-based-authorization-cs/samples/sample13.cs)]

Öznitelik için `RowUpdating` olay işleyicisi belirleyen burada özniteliği olarak Yöneticiler veya denetçiler rolleri yalnızca kullanıcıların olay işleyicisi yürütebilir `RowDeleting` olay işleyicisini yöneticilerinin kullanıcılara yürütme sınırlar rolü.


> [!NOTE]
> `PrincipalPermission` Özniteliği bir sınıf olarak temsil edilir `System.Security.Permissions` ad alanı. Eklediğinizden emin olun bir `using System.Security.Permissions` deyimini dosyanın üst kısmındaki bu ad alanı içe aktarmak için arka plandaki kod sınıfı.


Bu şekilde, yönetici olmayan yürütmeyi denediğinde, `RowDeleting` olay işleyicisi veya çalıştırmak için yönetici olmayan veya yönetici olmayan bir girişiminde bulunursa `RowUpdating` .NET çalışma zamanı olay işleyicisi yükseltmek bir `SecurityException`.


[![Güvenlik bağlamı yöntemi yürütmek için yetkili değil, bir SecurityException oluşturulur](role-based-authorization-cs/_static/image47.png)](role-based-authorization-cs/_static/image46.png)

**Şekil 16**: güvenlik bağlamını yöntemi yürütmek için yetkili değil, bir `SecurityException` atılır ([tam boyutlu görüntüyü görüntülemek için tıklatın](role-based-authorization-cs/_static/image48.png))


Birçok uygulama, ASP.NET sayfaları ek olarak, iş mantığı ve verileri erişim katmanları gibi çeşitli katmanları içeren bir mimari de vardır. Bu katmanlar genellikle sınıf kitaplıkları uygulanır ve sınıfları ve iş mantığı ve verileri ilgili işlevleri gerçekleştirmek için yöntemleri sunar. `PrincipalPermission` Özniteliği bu katmanlara yetkilendirme kuralları uygulamak için yararlıdır.

Kullanma hakkında daha fazla bilgi için `PrincipalPermission` sınıflar ve yöntemler yetkilendirme kuralları tanımlamak, başvurmak için öznitelik [Scott Guthrie](https://weblogs.asp.net/scottgu/)'s blog girdisi [iş ve veri kullanan katmanlar içinyetkilendirmekurallarıekleme`PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Özet

Bu öğreticide biz ne kaba belirtmek arama ve ince çizgisi yetkilendirme kuralları kullanıcı rollerine göre. ASP. Hangi kimlikleri izin verilen ya da hangi sayfaların erişim belirtmek bir sayfa Geliştirici NET'in URL yetkilendirme özelliği sağlar. Geri gördüğümüz gibi <a id="_msoanchor_10"> </a> [ *kullanıcı tabanlı bir yetkilendirme* ](../membership/user-based-authorization-cs.md) Öğreticisi, URL yetkilendirme kuralları, bir kullanıcı tarafından temelinde uygulanabilir. Adım 1'de bu öğreticinin gördüğümüz gibi bir rol tarafından rol temelinde da uygulanabilir.

İnce çizgisi yetkilendirme kuralları, bildirimli olarak veya program aracılığıyla uygulanabilir. 2. adımda ziyaret kullanıcı rollerine göre farklı çıkış işleme için LoginView denetimin RoleGroups özelliğini kullanarak Aranan. Ayrıca program aracılığıyla bir kullanıcı belirli bir rol ve sayfanın işlevselliği uygun şekilde ayarlama konusunda ait olup olmadığını belirlemek için yol inceledik.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İş ve veri katmanlarını kullanmak için yetkilendirme kuralları ekleme`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET 2.0'ın inceleniyor üyelik, roller ve profil: rolleriyle çalışma](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [ASP.NET 2.0 için Güvenlik sorusu listesi](https://msdn.microsoft.com/library/ms998375.aspx)
- [İçin teknik belgeler `<roleManager>` öğesi](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler...

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Suchi Banerjee ve Teresa Murphy içerir. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bir satırında bana bırak[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](assigning-roles-to-users-cs.md)
[sonraki](creating-and-managing-roles-vb.md)
