---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: Rol tabanlı yetkilendirme (VB) | Microsoft Docs
author: rick-anderson
description: Bu öğreticide nasıl rolleri framework bir kullanıcının rollerini, güvenlik bağlamı ile ilişkilendirir göz başlar. Sonra rol tabanlı URL uygulamak nasıl inceler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: b8cf8cc5fa802eb812482b236c8b2825423027e7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390991"
---
<a name="role-based-authorization-vb"></a>Rol tabanlı yetkilendirme (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> Bu öğreticide nasıl rolleri framework bir kullanıcının rollerini, güvenlik bağlamı ile ilişkilendirir göz başlar. Sonra rol tabanlı URL yetkilendirme kurallarını uygulamak nasıl inceler. Biz bildirim temelli ve programlı anlamına gelir. verilerin görüntülenmesi ve bir ASP.NET sayfası tarafından sunulan işlevselliği değiştirmeden için kullanacaksınız, aşağıdaki.


## <a name="introduction"></a>Giriş

İçinde <a id="_msoanchor_1"> </a> [ *kullanıcı tabanlı yetkilendirme* ](../membership/user-based-authorization-vb.md) gördüğümüz sayfaları belirli bir kümesini hangi kullanıcıların ziyaret edin belirtmek için URL yetkilendirmesi kullanma Öğreticisi. Ufak yalnızca bir biçimlendirme içinde `Web.config`, biz bir sayfayı ziyaret etmek yalnızca kimliği doğrulanmış kullanıcılara izin vermek için ASP.NET isteyin. Veya size yalnızca kullanıcıların Tito ve Bob izni olan dikte Sam hariç tüm kimliği doğrulanmış kullanıcılara izin gösterir.

URL yetkilendirmesi yanı sıra, ayrıca görüntülenen verileri ve ziyaret kullanıcıya dayanarak bir sayfa tarafından sunulan işlevselliği denetlemek için bildirim temelli ve programlı teknikleri inceledik. Özellikle, geçerli dizin içeriğini listelenen bir sayfa oluşturduk. Herkes bu sayfayı ziyaret, ancak yalnızca kimliği doğrulanmış kullanıcılar dosyaların içeriğini görüntüleyebilir ve yalnızca Tito dosyaları silinemedi.

Yetkilendirme kuralları kullanıcı tarafından olarak uygulama içinde bir muhasebe onarımı kabus büyüyebilir. Rol tabanlı yetkilendirme kullanmak daha sürdürülebilir bir yaklaşımdır. Yetkilendirme kuralları uygulamak için sunduğumuz elden araçları eşit çalışma güzel bir haberimiz var olan kullanıcı hesapları için yapmasını rolleriyle yanı sıra. URL yetkilendirme kuralları, kullanıcıların yerine rolleri belirtebilirsiniz. Kimliği doğrulanmış ve anonim kullanıcılar için farklı bir çıkış oluşturur, LoginView denetimi, oturum açmış kullanıcı rollerine göre farklı içeriği görüntülemek için yapılandırılabilir. Ve rolleri API oturum açmış kullanıcının rolleri belirlemek için yöntemler içerir.

Bu öğreticide nasıl rolleri framework bir kullanıcının rollerini, güvenlik bağlamı ile ilişkilendirir göz başlar. Sonra rol tabanlı URL yetkilendirme kurallarını uygulamak nasıl inceler. Biz bildirim temelli ve programlı anlamına gelir. verilerin görüntülenmesi ve bir ASP.NET sayfası tarafından sunulan işlevselliği değiştirmeden için kullanacaksınız, aşağıdaki. Haydi başlayalım!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>İle ilişkili anlama nasıl rolleri bir kullanıcının güvenlik bağlamı

Bir istek ASP.NET ardışık düzenini girer her istek sahibine tanımlama bilgileri içeren bir güvenlik bağlamı ile ilişkilidir. Forms kimlik doğrulaması kullanırken, bir kimlik doğrulaması bileti bir kimlik belirteci olarak kullanılır. Açıkladığımız gibi <a id="_msoanchor_2"> </a> [ *form kimlik doğrulaması bir genel bakış* ](../introduction/an-overview-of-forms-authentication-vb.md) ve <a id="_msoanchor_3"> </a> [ *formlar Kimlik doğrulaması yapılandırması ve Gelişmiş konular* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) öğreticiler, `FormsAuthenticationModule` sırasında mevcut isteyenin belirlemekten sorumludur [ `AuthenticateRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Geçerli, süresi dolmuş olmayan kimlik doğrulaması bileti bulunursa `FormsAuthenticationModule` sahibinin kimliğini belirlemek üzere kodunu çözer. Yeni bir oluşturur `GenericPrincipal` nesne ve bu şekilde atar `HttpContext.User` nesne. İster bir asıl amacı `GenericPrincipal`, kimliği doğrulanmış kullanıcının adını ve hangi belirlemektir rolleri Filiz aittir. Bu amaca sahip tüm asıl nesneleri gerçeğiyle yetkisiz değiştirmeye karşı korumalı bir `Identity` özelliği ve `IsInRole(roleName)` yöntemi. `FormsAuthenticationModule`, Ancak rol bilgilerini kaydetme sırasında ilgili değildir ve `GenericPrincipal` nesnesi oluşturur, herhangi bir rolü belirtmiyor.

Rolleri framework etkinleştirilirse, [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) sonra HTTP modülü adımları `FormsAuthenticationModule` ve sırasında kimliği doğrulanmış kullanıcının rolleri tanımlar [ `PostAuthenticateRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), hangi sonra ateşlenir `AuthenticateRequest` olay. İstek Kimliği doğrulanmış bir kullanıcıdan ise `RoleManagerModule` üzerine yazar `GenericPrincipal` tarafından oluşturulan nesne `FormsAuthenticationModule` ve ile değiştirir bir [ `RolePrincipal` nesne](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). `RolePrincipal` Sınıfı kullanıcının ait olduğu için hangi rolleri belirlemek üzere rolleri API kullanır.

Şekil 1, form kimlik doğrulaması ve rolleri Framework'ü kullanırken, ASP.NET ardışık düzen iş gösterilmektedir. `FormsAuthenticationModule` Yürütür, kullanıcının kendi kimlik doğrulama anahtarı aracılığıyla tanımlar ve yeni bir oluşturur `GenericPrincipal` nesne. Ardından, `RoleManagerModule` adımları ve üzerine yazar `GenericPrincipal` nesnesi ile bir `RolePrincipal` nesne.

Anonim kullanıcı site tipleri ziyaret ederse `FormsAuthenticationModule` ya da `RoleManagerModule` sorumlusu nesnesi oluşturur.


[![Form kimlik doğrulaması ve rolleri Framework kullanılırken bir kimliği doğrulanmış kullanıcı için ASP.NET ardışık düzenini olayları](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**Şekil 1**: bir kimliği doğrulanmış kullanıcı kullanarak form kimlik doğrulaması ve rolleri Framework için ASP.NET ardışık düzen olayları ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Rol bilgileri bir tanımlama bilgisinde önbelleğe alma

`RolePrincipal` Nesnenin `IsInRole(roleName)` yöntem çağrılarını `Roles`.`GetRolesForUser` kullanıcının üyesi olup olmadığını belirlemek için kullanıcı rollerini almak için *roleName*. Kullanırken `SqlRoleProvider`, bu rol deposu veritabanı için bir sorgu sonuçları. Rol tabanlı URL yetkilendirme kuralları kullanırken `RolePrincipal`'s `IsInRole` rol tabanlı URL yetkilendirme kuralları tarafından korunan bir sayfaya her istekte yöntemi çağrılır. Her istekte veritabanında rol bilgilerini aramak için yerine `Roles` framework kullanıcının rolleri bir tanımlama bilgisinde önbelleğe almak için bir seçenek içerir.

Kullanıcı rolleri bir tanımlama bilgisinde önbelleğe almak için rolleri framework yapılandırılmışsa `RoleManagerModule` ASP.NET ardışık düzen sırasında tanımlama bilgisi oluşturur [ `EndRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Bu tanımlama bilgisi sonraki isteklerinde kullanılan `PostAuthenticateRequest`, ne zaman olduğu `RolePrincipal` nesnesi oluşturulur. Tanımlama bilgisinin geçerli olduğunu ve süresi geçmemiş, tanımlama bilgisi verileri ayrıştırılır ve böylece kaydetme, kullanıcının rolleri doldurmak için kullanılan `RolePrincipal` çağrı yapmak zorunda `Roles` kullanıcı rolleri belirlemek için sınıf. Şekil 2, bu iş akışı gösterilmektedir.


[![Kullanıcının rol bilgilerini performansını artırmak için bir tanımlama bilgisinde depolanabilir.](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**Şekil 2**: kullanıcının rol bilgilerini depolanabilir performansı arttırmak için bir tanımlama bilgisinde ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image6.png))


Varsayılan olarak, rol önbellek tanımlama bilgisi mekanizması devre dışıdır. Aracılığıyla etkinleştirilebilir `<roleManager>`; yapılandırma işaretlemede `Web.config`. Kullanarak ele almıştık [ `<roleManager>` öğesi](https://msdn.microsoft.com/library/ms164660.aspx) rol sağlayıcılarında belirtmek için <a id="_msoanchor_4"> </a> [ *oluşturma ve yönetme rolleri* ](creating-and-managing-roles-vb.md) öğretici Bu öğe zaten uygulamanızın olmalıdır böylece `Web.config` dosya. Rol önbellek tanımlama bilgisi ayarları bir öznitelik olarak belirtilen `<roleManager>`; öğesi ve bu tablo 1'de özetlenmiştir.

> [!NOTE]
> Tablo 1'de listelenen yapılandırma ayarlarını, sonuçta elde edilen rol önbellek tanımlama bilgisinin özelliklerini belirtin. Tanımlama bilgileri, nasıl çalıştıklarını ve bunların çeşitli özellikler hakkında daha fazla bilgi için okuma [bu tanımlama bilgileri öğretici](http://www.quirksmode.org/js/cookies.html).


| <strong>Özelliği</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Açıklama</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Tanımlama bilgisinin önbelleğe almanın kullanılıp kullanılmadığını gösteren bir Boole değeri. Varsayılan olarak `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Rol önbellek tanımlama bilgisinin adı. Varsayılan değer ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                Rol adı tanımlama bilgisinin yolu. Yol özniteliği bir tanımlama bilgisi için belirli bir dizin sıradüzeni kapsamını sınırlamak bir geliştirici sağlar. Varsayılan değer "/", hangi kimlik doğrulaması bileti tanımlama bilgisinin etki alanında yapılan herhangi bir istek göndermek için tarayıcı bildirir.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Rol önbellek tanımlama bilgisinin korumak için kullanılan hangi teknikleri gösterir. İzin verilen değerler: `All` (varsayılan); `Encryption`; `None`; ve `Validation`. Adım 3'e yeniden bakın <a id="_anchor_5"> </a> [ *Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) Bu koruma düzeyleri hakkında daha fazla bilgi için öğretici.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Bir SSL bağlantısı kimlik doğrulaması tanımlama bilgisinin iletilip iletilmeyeceğini gerekip gerekmediğini gösteren bir Boole değeri. Varsayılan değer `false` şeklindedir.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Tanımlama bilgisinin zaman aşımı her erişimde sıfırlanacağını olmadığını kullanıcı tek bir oturumda siteyi ziyaret belirten bir Boolean değer. Varsayılan değer `false` şeklindedir. Bu değer yalnızca uygun olduğunda `createPersistentCookie` ayarlanır `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Saat sonra kimlik doğrulama bileti tanımlama süresi dakika cinsinden belirtir. Varsayılan değer `30` şeklindedir. Bu değer yalnızca uygun olduğunda `createPersistentCookie` ayarlanır `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Rol önbellek tanımlama bilgisinin bir oturum tanımlama bilgisinin kalıcı bir tanımlama bilgisi olup olmadığını belirten bir Boole değeri. Varsa `false` (varsayılan), tarayıcı kapatıldığında silinir oturum tanımlama bilgisi kullanılır. Varsa `true`, kalıcı bir tanımlama bilgisi kullanılır; dolmadan `cookieTimeout` oluşturulduktan sonra dakika veya değerine bağlı olarak önceki sayfasını ziyaret edin sonra sayı `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Tanımlama bilgisinin etki alanı değeri belirtir. Tarayıcının içinden (www.yourdomain.com gibi) verilmiş etki alanını kullan boş bir dize varsayılan değerdir. Bu durumda, tanımlama bilgisi olacak <strong>değil</strong> yapma admin.yourdomain.com gibi alt etki alanlarını istediğinde gönderilemez. Tüm alt etki alanlarıyla geçirilecek tanımlama bilgisi istiyorsanız özelleştirmeniz gerektiğini `domain` özniteliği, "yourdomain.com için" ayarı.                                                                                                                                                 |
|    `maxCachedResults`     | Bir tanımlama bilgisinde önbelleğe rol adları en fazla sayısını belirtir. Varsayılan değer 25'tir. `RoleManagerModule` Ait kullanıcılar için bir tanımlama bilgisi oluşturmaz birden fazla `maxCachedResults` rolleri. Sonuç olarak, `RolePrincipal` nesnenin `IsInRole` yöntemi kullanacağı `Roles` kullanıcı rolleri belirlemek için sınıf. Nedeni `maxCachedResults` mevcut birçok kullanıcı aracıları 4.096 bayttan büyük tanımlama bilgilerine izin verme olmasıdır. Bu nedenle bu boyut sınırlaması aşan olasılığını azaltmak için bu sınır yöneliktir. Son derece uzun rol adları varsa, daha küçük belirtmeyi düşünün isteyebilirsiniz `maxCachedResults` değeri; contrariwise, son derece kısa rol adları varsa, bu değer büyük olasılıkla artırabilirsiniz. |

**Tablo 1**: rol önbellek tanımlama bilgisi yapılandırma seçenekleri

Kalıcı olmayan rol önbellek tanımlama bilgileri kullanmasına izin uygulamamız yapılandıralım. Bunu yapmak için güncelleştirme `<roleManager>` öğesinde `Web.config` tanımlama bilgisi ile ilgili aşağıdaki öznitelikler eklemek için:

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

Ben güncelleştirilmiş `<roleManager>`; üç öznitelikleri ekleyerek öğenin: `cacheRolesInCookie`, `createPersistentCookie`, ve `cookieProtection`. Ayarlayarak `cacheRolesInCookie` için `true`, `RoleManagerModule` kullanıcının rol bilgileri her istek için arama yapmak zorunda yerine artık otomatik olarak kullanıcı rolleri bir tanımlama bilgisinde önbelleğe alır. Açık olarak `createPersistentCookie` ve `cookieProtection` özniteliklerini `false` ve `All`sırasıyla. Teknik olarak, yalnızca bunları varsayılan değerlerine atadım, ancak ben bunları açıkça yapmak koyalım olduğundan bu öznitelikler için değerleri kalıcı tanımlama bilgileri kullanmıyorum ve tanımlama bilgisi hem olduğunu şifrelenir ve doğrulanmış temizleme belirtmek ihtiyacım kalmadı.

Tüm İşte bu kadar kolay! Henceforth, rolleri framework kullanıcıların rollerini tanımlama bilgilerini önbelleğe alır. Kullanıcının tarayıcı tanımlama bilgileri desteklemiyor veya tanımlama bilgilerine silindi veya kayıp, bu şekilde, bu büyük bir olay – `RolePrincipal` nesne yalnızca kullanacağı `Roles` sınıfı tanımlama bilgisi (veya geçersiz veya süresi dolmuş bir) kullanılabilir durumda.

> [!NOTE]
> Microsoft desenleri &amp; yöntemler grup, rol kalıcı önbellek tanımlama bilgilerini kullanarak gerçekleştirilmesini önler. Bir bilgisayar korsanının şekilde geçerli kullanıcının tanımlama bilgisi erişim elde edebilir, rol önbellek tanımlama bilgisinin elinde rol üyeliğini kanıtlamak yeterli olduğundan kendisi, kullanıcının kimliğine bürünebilirsiniz. Kullanıcının tarayıcısında tanımlama bilgisinin kalıcı değilse bunun olasılığını artırır. Bu güvenlik önerisi yanı sıra diğer güvenlik konuları hakkında daha fazla bilgi için başvurmak [ASP.NET 2.0 için Güvenlik sorusu listesi](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>1. adım: URL rol tabanlı yetkilendirme kurallarını tanımlama

Bölümünde açıklandığı gibi <a id="_msoanchor_6"> </a> [ *kullanıcı tabanlı yetkilendirme* ](../membership/user-based-authorization-vb.md) Öğreticisi, URL yetkilendirmesi, kullanıcı tarafından veya bir rolü rollü sayfalar kümesi erişimi kısıtlamak için bir yol sunar temel. URL yetkilendirme kuralları, İl `Web.config` kullanarak [ `<authorization>` öğesi](https://msdn.microsoft.com/library/8d82143t.aspx) ile `<allow>` ve `<deny>` alt öğeleri. Önceki öğreticilerde, ele alınan kullanıcı ile ilgili yetkilendirme kurallarının yanı sıra her `<allow>` ve `<deny>` alt öğesi de dahil edebilirsiniz:

- Belirli bir rol
- Rollerin virgülle ayrılmış listesi

Örneğin, URL yetkilendirme kuralları yöneticileri ve denetçiler rollerindeki kullanıcılara erişim vermek ancak diğerlerini erişimini reddet:

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

`<allow>` Yukarıdaki biçimlendirme öğesi durumları Yöneticiler ve denetçiler rollerine izin verildiğini; `<deny>`; öğe bildirir *tüm* kullanıcılar engellenir.

Uygulamamızı yapılandıralım böylece `ManageRoles.aspx`, `UsersAndRoles.aspx`, ve `CreateUserWizardWithRoles.aspx` sayfaları erişilebilir Yöneticiler rolündeki kullanıcılara yalnızca sırada `RoleBasedAuthorization.aspx` sayfasında tüm ziyaretçiler için erişilebilir kalır.

Bunu gerçekleştirmek için ekleyerek başlayın bir `Web.config` dosyasını `Roles` klasör.


[![Bir Web.config dosyası rolleri dizine ekleme](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**Şekil 3**: ekleme bir `Web.config` dosyasını `Roles` dizin ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image9.png))


Ardından, aşağıdaki yapılandırma işaretlemede ekleyin `Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

`<authorization>` Öğesinde `<system.web>` bölümü gösterir yalnızca Yöneticiler rolündeki kullanıcılar ASP.NET kaynaklara erişebilir `Roles` dizin. `<location>` Öğe tanımlar alternatif URL yetkilendirme kuralları kümesi `RoleBasedAuthorization.aspx` sayfası, sayfayı ziyaret etmek tüm kullanıcıların.

Yaptığınız değişiklikler kaydedildikten sonra `Web.config`, yöneticiler rolüne olmayan bir kullanıcı olarak oturum açın ve sonra korumalı sayfaları birini ziyaret deneyin. `UrlAuthorizationModule` İstenen kaynak; ziyaret etmek için izne sahip olmayan algılar sonuç olarak, `FormsAuthenticationModule` oturum açma sayfasına yönlendirir. Oturum açma sayfasına, sonra yönlendireceği `UnauthorizedAccess.aspx` sayfa (bkz: Şekil 4). Bu son yeniden yönlendirme için oturum açma sayfasından `UnauthorizedAccess.aspx` ekledik 2. Adım'da oturum açma sayfası kod nedeniyle gerçekleşir <a id="_msoanchor_7"> </a> [ *kullanıcı tabanlı yetkilendirme* ](../membership/user-based-authorization-vb.md) öğretici. Özellikle, oturum açma sayfasına herhangi bir kimliği doğrulanmış kullanıcı için otomatik olarak yönlendiren `UnauthorizedAccess.aspx` sorgu dizesini içeriyorsa bir `ReturnUrl` parametresi, bu parametre olarak gösterir kendisi değil bir sayfayı görüntülemek denemeden sonra kullanıcı oturum açma sayfasına geldiğini görüntüleme yetkiniz.


[![Yalnızca Yöneticiler rolündeki kullanıcılar korumalı sayfaları görüntüleyebilirsiniz.](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**Şekil 4**: yalnızca Yöneticiler rolündeki kullanıcılar, korumalı sayfaları görüntüleyebilirsiniz ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image12.png))


Oturumu kapatın ve uygulamasındaki Yöneticiler rolünün bir kullanıcı olarak oturum açın. Artık üç korumalı sayfası görmesini olmalıdır.


[![Tito ziyaret UsersAndRoles.aspx sayfa çünkü o Yöneticiler rolünde olduğu](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**Şekil 5**: Tito ziyaret `UsersAndRoles.aspx` sayfa çünkü He's uygulamasındaki Yöneticiler rolünün ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> URL yetkilendirme kuralları – rollerin veya kullanıcıların – belirtirken çözümlenen birer birer, üstten aşağı kurallardır aklınızda bulundurun önemlidir. Bir eşleşme bulunduğu sürece kullanıcı izni veya erişimi reddedilen, açıksa bağlı olarak eşleşmenin bir `<allow>` veya `<deny>` öğesi. **Eşleşme bulunursa, kullanıcıya erişim izni verilir.** Bir veya daha fazla kullanıcı hesabı için erişimi kısıtlamak istiyorsanız, bu nedenle, bunu kullanmanız zorunludur bir `<deny>` son URL yetkilendirme yapılandırma öğesi olarak öğesi. **URL yetkilendirme kurallarınızı dahil etmezseniz bir**`<deny>`**öğesi, tüm kullanıcıların erişim verilecek.** URL yetkilendirme kuralları nasıl analiz edilen bir daha kapsamlı bilgi için kiracıurl "nasıl göz `UrlAuthorizationModule` vermek veya erişimini engellemek için yetkilendirme kuralları kullanır" bölümünü <a id="_msoanchor_8"> </a> [  *Kullanıcı tabanlı yetkilendirme* ](../membership/user-based-authorization-vb.md) öğretici.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>2. adım: şu anda oturum açmış kullanıcı rollerine göre işlevselliğini sınırlama

URL yetkilendirme yapar kaba yetkilendirme belirtmek kolay, hangi kimliklerin bu duruma kuralları izin verilir ve hangilerinin belirli bir sayfayı (veya bir klasörü ve alt klasörlerindeki tüm sayfaları) görüntülemesini reddedilir. Ancak, bazı durumlarda bir sayfasını ziyaret edin, ancak sayfa işlevselliği ziyaret kullanıcı rollerine göre sınırlamak tüm kullanıcıların istiyoruz. Bu kullanıcı rolüne göre veya belirli bir role ait kullanıcılar için ek işlevler sunan gösterme veya gizleme veri gerektirdiği.

Bu tür hedeflemediğinizden rol tabanlı yetkilendirme kuralları, bildirimli olarak veya program aracılığıyla (veya ikisinin birleşimi aracılığıyla) uygulanabilir. Sonraki bölümde bildirim temelli hedeflemediğinizden yetkilendirme LoginView denetimi aracılığıyla uygulama göreceğiz. Programlama teknikleri inceleyeceksiniz. Biz hedeflemediğinizden yetkilendirme kuralları uygulamayı göz atmadan önce ancak biz öncelikle bir sayfa, ziyaret kullanıcı rolünde olan işlevselliği bağlıdır oluşturmanız gerekir.

GridView'ndaki Sistem tüm kullanıcı hesaplarını listeler bir sayfa oluşturalım. GridView her kullanıcının kullanıcı adı, e-posta adresi, son oturum açma tarihi ve kullanıcı hakkında yorumlar içerir. Her kullanıcının bilgilerini görüntülemeye ek olarak, GridView düzenleme içerir ve özellikleri silin. Başlangıçta bu sayfayı düzenleme oluşturur ve silme işlevlerini tüm kullanıcılar tarafından kullanılabilir. Etkinleştirme veya ziyaret kullanıcı rolüne bağlı olarak bu özellikleri devre dışı bırakma "LoginView denetimi kullanarak" ve "Program aracılığıyla sınırlama işlevselliğini" bölümlerde göreceğiz.

> [!NOTE]
> Biz hakkında oluşturmak için ASP.NET sayfası, kullanıcı hesaplarını görüntülemek için bir GridView denetimi kullanır. Seri form kimlik doğrulaması, yetkilendirme, kullanıcı hesaplarını ve rolleri üzerinde odaklanır. Bu öğreticide bu yana GridView denetiminde iç işleyişini tartışma çok fazla vakit geçirmeyi istemiyorum. Bu öğretici, bu sayfası ayarlama belirli adım adım yönergeler sağlar. ancak, bazı seçenekler neden yapıldığı veya hangi işlenmiş çıktı belirli özellikleri etkili sahip detayına anlamak için delve değil. GridView denetiminde tam incelenmesi için kullanıma my *[ASP.NET 2.0 verilerle çalışmaya](../../data-access/index.md)* öğretici serisi.


Başlangıç açarak `RoleBasedAuthorization.aspx` sayfasını `Roles` klasör. GridView Tasarımcısı ve kümesi sayfaya sürükleyin, `ID` için `UserGrid`. Birazdan biz çağıran kod yazacaksınız `Membership`.`GetAllUsers` yöntem ve ortaya çıkan bağlar `MembershipUserCollection` GridView için nesne. `MembershipUserCollection` İçeren bir `MembershipUser` ; sistemdeki her kullanıcı hesabı için nesne `MembershipUser` nesneleri gibi özelliklere sahip `UserName`,`Email`,`LastLoginDate` ve benzeri.

Şimdi biz kullanıcı hesaplarını kılavuza bağlayan bir kod yazmadan önce ilk GridView'ın alanları tanımlayın. GridView'ın akıllı etiketten alanları iletişim başlatmak için "Sütunları Düzenle" bağlantısına tıklayın (bkz. Şekil 6) kutusunda. Buradan, alt sol üst köşedeki "alanları otomatik olarak oluştur" onay kutusunun işaretini kaldırın. Bu yana bir CommandField ekleme düzenleme ve silme özelliklerini içerir ve ayarlamak için bu GridView istiyoruz, `ShowEditButton` ve `ShowDeleteButton` özellikleri true. Ardından, görüntülemek için dört alan Ekle `UserName`, `Email`, `LastLoginDate`, ve `Comment` özellikleri. İki salt okunur özelliği için bir BoundField kullanın (`UserName` ve `LastLoginDate`) ve iki düzenlenebilir alanlar için TemplateField (`Email` ve `Comment`).

İlk BoundField görüntülemesi `UserName` özelliği; kümesi kendi `HeaderText` ve `DataField` "UserName" özellikleri. Bu alan olacak şekilde ayarlamanız düzenlenebilir olmayacaktır, `ReadOnly` özelliği true. Yapılandırma `LastLoginDate` ayarlayarak BoundField kendi `HeaderText` "Son oturum açma" ve kendi `DataField` "LastLoginDate" için. Yalnızca tarih (tarih ve saat yerine) görüntülenir, böylece şimdi bu BoundField çıktısını biçimlendirin. Bunu gerçekleştirmek için bu BoundField's ayarlamak `HtmlEncode` özelliğini False olarak ve kendi `DataFormatString` özelliğini "{0:d}". Ayrıca `ReadOnly` özelliği true.

Ayarlama `HeaderText` iki TemplateField "E-posta" ve "Açıklama" özellikleri.


[![GridView'ın alanları alanları iletişim kutusu aracılığıyla yapılandırılabilir.](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**Şekil 6**: GridView'ın alanları olabilir olması yapılandırılmış aracılığıyla alanları iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image18.png))


Artık tanımlamak ihtiyacımız `ItemTemplate` ve `EditItemTemplate` "Email" ve "Açıklama" TemplateField. Her biri için bir etiket Web denetimi ekleme `ItemTemplates` ve bağlama bunların `Text` özelliklerine `Email` ve `Comment` özellikleri, sırasıyla.

"E-posta" TemplateField için adlı bir metin kutusu ekleme `Email` için kendi `EditItemTemplate` ve bağlama kendi `Text` özelliğini `Email` çift yönlü veri bağlamasını kullanma özelliği. Bir RequiredFieldValidator ekleyip için RegularExpressionValidator `EditItemTemplate` e-posta özelliği düzenlemeden ziyaretçisi geçerli bir eposta adresi girdiğinden emin olmak için. "Açıklama" TemplateField için adlı çok satırlı metin kutusu ekleme `Comment` için kendi `EditItemTemplate`. Metin kutusunun ayarlayın `Columns` ve `Rows` 40 ve 4, özellikleri sırasıyla ve ardından kendi `Text` özelliğini `Comment` çift yönlü veri bağlamasını kullanma özelliği.

Bu TemplateField yapılandırdıktan sonra bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

Düzenlerken veya kullanıcının bilmeniz gerekir ki bir kullanıcı hesabının silinmesi `UserName` özellik değeri. GridView'ın ayarlamak `DataKeyNames` "UserName" özelliğini böylece bu bilgileri GridView kişinin kullanılabilir `DataKeys` koleksiyonu.

Son olarak, ValidationSummary denetimi sayfaya ekleyip ayarlayın, `ShowMessageBox` özelliği True olarak ve kendi `ShowSummary` özelliğini False. Bu ayarlarla kullanıcının eksik veya geçersiz bir e-posta adresine sahip bir kullanıcı hesabını düzenleyerek çalışırsa, ValidationSummary bir istemci-tarafı uyarısı görüntüler.

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

Biz bu sayfanın bildirim temelli biçimlendirme tamamladınız. Bizim sıradaki görev, kullanıcı hesapları kümesi GridView'a bağlamaktır. Adlı bir yöntem ekleyin `BindUserGrid` için `RoleBasedAuthorization.aspx` bağlar sayfa arka plan kod sınıfı `MembershipUserCollection` tarafından döndürülen `Membership.GetAllUsers` için `UserGrid` GridView. Bu yöntemi çağırın `Page_Load` olay işleyicisini ilk sayfasını ziyaret edin.

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

Bu kod bir yerde bir tarayıcı aracılığıyla sayfasını ziyaret edin. Şekil 7 gösterildiği gibi sistemdeki her kullanıcı hesabı hakkında bilgi listeleyen bir GridView görmeniz gerekir.


[![UserGrid GridView sistemde her kullanıcı hakkındaki bilgileri listeler](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**Şekil 7**: `UserGrid` GridView listeler bilgi hakkında her kullanıcı sistemindeki ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> `UserGrid` GridView belleksiz arabirimindeki tüm kullanıcıları listeler. Bu kılavuz basit arabirim senaryoları için uygun değil birkaç düzine ya da daha fazla kullanıcı olduğu. GridView'disk belleğini etkinleştirmek için yapılandırma bir seçenektir. `Membership.GetAllUsers` Yönteminin iki aşırı yüklemesi vardır: biri, hiç giriş parametresi kabul eden ve tüm kullanıcılar ve sayfa dizini ve sayfa boyutu tamsayı değerlerini alır ve kullanıcıların yalnızca belirtilen alt döndüren bir döndürür. Kullanıcılar daha verimli bir şekilde bir sayfadan için kullanıcı hesapları yalnızca kesin kümesini döndürdüğünden ikinci aşırı yüklemesi kullanılabilir yerine *tüm* biri. Binlerce kullanıcı hesapları varsa, filtre tabanlı bir arabirim, yalnızca bu kullanıcılar, kullanıcı adı örneği için seçilen bir karakterle başlayan gösteren bir düşünmek isteyebilirsiniz. [ `Membership.FindUsersByName` Yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) filtre tabanlı kullanıcı arabirimi oluşturmak için idealdir. Biz, böyle bir arabirim bir sonraki öğreticide oluşturmayı görünecektir.


GridView denetiminde yerleşik düzenleme ve denetim SqlDataSource veya ObjectDataSource gibi düzgün bir şekilde yapılandırılmış veri kaynak denetimine bağlı olduğunda destek silme sunar. `UserGrid` GridView, ancak, program aracılığıyla veriye sahiptir; bu nedenle, ki bu iki görevleri gerçekleştirmek için kod yazmanız gerekir. Özellikle, GridView için kullanıcının olay işleyicilerini oluşturma ihtiyacımız `RowEditing`, `RowCancelingEdit`, `RowUpdating`, ve `RowDeleting` ziyaretçi GridView'ın tıklattığında tetiklenen olayları, düzenleme, iptal, Update veya Sil düğmeleri.

GridView'için ın olay işleyicileri oluşturarak başlayın `RowEditing`, `RowCancelingEdit`, ve `RowUpdating` olayları ve ardından aşağıdaki kodu ekleyin:

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

`RowEditing` Ve `RowCancelingEdit` olay işleyicileri ayarlamanız yeterlidir GridView'ın `EditIndex` özelliği ve sonra yeniden bağlamasını kılavuza hesaplarına kullanıcı listesi. İlgi çekici şeyler de olur `RowUpdating` olay işleyicisi. Veriler geçerliyse ve ardından Dallarınızla sağlayarak bu olay işleyicisi başlar `UserName` düzenlenen kullanıcı hesabından değerini `DataKeys` koleksiyonu. `Email` Ve `Comment` metin kutuları içinde iki TemplateField `EditItemTemplate` s sonra program aracılığıyla başvuru. Kendi `Text` düzenlenen e-posta adresi ve yorum özellikleri içerir.

İhtiyacımız ilk çağrı yoluyla yaptığımız kullanıcının bilgileri almak bir kullanıcı hesabı üyelik API'si üzerinden güncelleştirmek için `Membership.GetUser(userName)`. Döndürülen `MembershipUser` nesnenin `Email` ve `Comment` özelliklerini düzenleme arabiriminden iki metin kutularına girilen değerlerle sonra güncelleştirilir. Son olarak, bu değişiklikleri çağrısı ile kaydedilmiş [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). `RowUpdating` Olay işleyicisi, önceden düzenleme arabirimiyle GridView'a dönerek tamamlar.

Ardından, oluşturma `RowDeleting` RowDeleting olay işleyicisi ve ardından aşağıdaki kodu ekleyin:

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

Yukarıdaki olay işleyicisi tarafından kapmasını başlar `UserName` GridView'ın değerini `DataKeys` koleksiyonu; bu `UserName` değeri üyelik sınıfının sonra geçirilen [ `DeleteUser` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). `DeleteUser` Yöntemi sisteminden ilgili üyeliğini verilerine (hangi rolleri gibi bu kullanıcının ait olduğu) dahil olmak üzere kullanıcı hesabını siler. Kullanıcının, kılavuz sildikten sonra `EditIndex` durumunda (başka bir satır düzenleme modunda ederken kullanıcı silme tıklandığında) -1 olarak ayarlanır ve `BindUserGrid` yöntemi çağrılır.

> [!NOTE]
> Sil düğmesini her tür kullanıcı hesabını silmeden önce kullanıcıdan onay gerektirmez. Yanlışlıkla silinen bir hesap olasılığını azaltmak için kullanıcı onayı çeşit eklemek için öneriyoruz. Eylemi onaylamak için en kolay yollarından biri bir istemci-tarafı Onayla iletişim kutusudur. Bu yöntem hakkında daha fazla bilgi için bkz. [ekleme istemci tarafı doğrulama zaman silme](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Bu sayfa beklendiği gibi çalıştığını doğrulayın. Yanı sıra herhangi bir kullanıcının e-posta adresi ve yorum düzenlemek herhangi bir kullanıcı hesabı silmek mümkün olması gerekir. Bu yana `RoleBasedAuthorization.aspx` sayfasıdır tüm kullanıcılar için erişilebilir, herhangi bir kullanıcı – bile anonim ziyaretçiler – bu sayfasını ziyaret edin ve düzenleyebilir ve kullanıcı hesaplarını! Böylece denetçilere ve yöneticilere roller yalnızca kullanıcılara bir kullanıcının e-posta adresi ve açıklamayı düzenleyebilirsiniz ve yalnızca Yöneticiler bir kullanıcı hesabını silebilmeniz için bu sayfayı güncelleştirelim.

"LoginView denetimi kullanma" bölümüne yönergeleri kullanıcı rolü için belirli gösterilecek LoginView denetimi kullanarak arar. Yöneticiler rolünün bir kişi bu sayfayı ziyaret ederse, düzenleme ve kullanıcıların silme hakkında yönergeler göstereceğiz. Denetçiler roldeki bir kullanıcı bu sayfayı ulaşırsa yönergeleri kullanıcıları düzenleme üzerinde göstereceğiz. Ve ziyaretçi anonim veya Yöneticiler veya denetçiler rolde değil, biz, düzenleyemez veya kullanıcı hesabı bilgilerini silme olduğunu açıklayan bir ileti görüntülenir. "Program aracılığıyla sınırlama işlevler" bölümüne biz programlı olarak gösterir veya gizler kullanıcı rolüne dayalı Düzenle ve Sil düğmeleri kod yazacaksınız.

### <a name="using-the-loginview-control"></a>Bir LoginView denetimi kullanma

Önceki öğreticilerde anlatıldığı gibi LoginView denetimi, kimliği doğrulanmış ve anonim kullanıcılar için farklı arabirimler görüntülemek için yararlıdır, Ancak LoginView denetimi, kullanıcı rollerine göre farklı biçimlendirme görüntülemek için de kullanılabilir. Bir LoginView denetimi ziyaret kullanıcı rolüne bağlı olarak farklı yönergeleri görüntülemek için kullanalım.

Bir LoginView yukarıdaki ekleyerek başlangıç `UserGrid` GridView. Daha önce ele aldığımız gibi iki yerleşik şablonlar LoginView denetimi var: `AnonymousTemplate` ve `LoggedInTemplate`. Kısa bir ileti, düzenleyemez veya herhangi bir kullanıcı bilgisi silmek, kullanıcıya bildirir. Bu şablonları ikisinde girin.

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

Ek olarak `AnonymousTemplate` ve `LoggedInTemplate`, LoginView denetimi içerebilir *RoleGroups*, role özgü şablonları olduğu. Her RoleGroup tek bir özellik içeren `Roles`, hangi rolleri RoleGroup uygulandığı belirtir. `Roles` Özelliği (örneğin, "Yöneticiler") tek bir rol veya rolleri (örneğin, "yöneticileri, denetçilere") virgülle ayrılmış bir listesi için ayarlanabilir.

RoleGroups yönetmek için denetimin akıllı etiket'kurmak RoleGroup Koleksiyonu Düzenleyicisi getirmek için "RoleGroups Düzenle" bağlantıyı tıklayın. İki yeni RoleGroups ekleyin. İlk RoleGroup's ayarlamak `Roles` özelliğini "Yöneticiler" ve ikinci kişinin "Denetçiler".


[![LoginView'ın Role özgü şablonları aracılığıyla RoleGroup Koleksiyonu Düzenleyicisi yönetme](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**Şekil 8**: LoginView'ın Role özgü şablonları aracılığıyla RoleGroup Koleksiyonu Düzenleyicisi yönetme ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image24.png))


RoleGroup Koleksiyonu Düzenleyicisi'ni kapatmak için Tamam'a tıklayın; Bu güncelleştirmeler dahil etmek için bildirim temelli biçimlendirme LoginView'ın bir `<RoleGroups>` ile bölümünde bir `<asp:RoleGroup>` alt öğe için her RoleGroup RoleGroup Koleksiyonu Düzenleyicisi'nde tanımlanan. Ayrıca, "Görünümler" açılan listesinde LoginView'ın akıllı başlangıçta listelenen etiketinde - yalnızca `AnonymousTemplate` ve `LoggedInTemplate` – artık ek RoleGroups de içerir.

RoleGroups denetçiler roldeki kullanıcılar, kullanıcı hesapları Yöneticiler rolündeki kullanıcılar düzenleme ve silmeye yönelik yönergeleri gösterilmekte iken düzenlemek görüntülenen yönergeleri olacak şekilde düzenleyin. Bu değişiklikleri yaptıktan sonra LoginView'ın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir.

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

Bu değişiklikleri yaptıktan sonra sayfayı kaydedin ve bir tarayıcıdan ziyaret edin. Anonim kullanıcı olarak ilk sayfasını ziyaret edin. İleti gösterilmesi, ", sisteme günlüğe kaydedilmez. Bu nedenle, düzenleyemez veya herhangi bir kullanıcı bilgisi silemezsiniz." Ardından bir kimliği doğrulanmış kullanıcı, ancak ne denetçiler ya da yöneticiler rolündeki bir olarak oturum açın. Bu kez ", denetçilere ya da yöneticiler rollerinin bir üyesi değildir. Bu iletiyi görmeniz gerekir Bu nedenle, düzenleyemez veya herhangi bir kullanıcı bilgisi silemezsiniz."

Ardından, denetçilere rolünün bir üyesi olan bir kullanıcı oturum açın. Bu süre, role özgü denetçiler gördüğünüz ileti (bkz. Şekil 9). Ve rol role özgü Yöneticiler gördüğünüz ileti (bkz. Şekil 10) Yöneticiler bir kullanıcı olarak oturum açın.


[![Bruce denetçiler Role özel ileti gösterilir](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**Şekil 9**: Bruce denetçiler Role özel ileti gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image27.png))


[![Tito Yöneticiler Role özel ileti gösterilir](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**Şekil 10**: Tito Yöneticiler Role özel ileti gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image30.png))


Birden çok şablonları uygulamak bile LoginView yalnızca tek bir şablonda ekran görüntüleri, Şekil 9 ve 10 Göster işler. Bruce ve Tito hem oturum kullanıcıları, ancak yalnızca eşleşen RoleGroup LoginView işler ve `LoggedInTemplate`. Ayrıca, hem Yöneticiler hem de Denetçiler rollerine Tito ait henüz Yöneticiler role özgü şablon denetçiler yerine bir LoginView denetimi oluşturur.

Şekil 11 LoginView denetimi tarafından işlenecek şablonu belirlemek için kullanılan iş akışı gösterilmektedir. Varsa birden fazla RoleGroup belirtilen LoginView şablonun oluşturulduğunu unutmayın *ilk* eşleşen RoleGroup. Biz ilk RoleGroup olarak denetçiler RoleGroup ve ikinci olarak yöneticiler yerleştirdiğiniz, Tito bu sayfayı ziyaret edildiğinde diğer bir deyişle, ardından o denetçiler ileti görür.


[![İşleme için hangi şablonun belirlemek için LoginView denetimin iş akışı](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**Şekil 11**: belirleme hangi şablonun işleme LoginView denetimi'nın iş akışı ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Program aracılığıyla işlevselliğini sınırlama

Bir LoginView denetimi sayfasını ziyaret ederek kullanıcı rolüne bağlı olarak farklı yönergeler görüntülerken, düzenleme ve İptal düğmeleri tüm görünür kalır. Program aracılığıyla anonim ziyaretçiler ve denetçiler ne Yöneticiler rolü olan kullanıcılar için Düzenle ve Sil düğmeleri gizlemek ihtiyacımız var. Yönetici olmayan herkes için Sil düğmesine gizlemek ihtiyacımız var. Bunu gerçekleştirmek için biz biraz CommandField'ın Düzenle ve Sil LinkButtons ve kümeleri programlı olarak başvuran kod yazacaksınız kendi `Visible` özelliklerine `False`, gerekirse.

Program aracılığıyla bir CommandField denetimlerinde başvurmak için en kolay yolu, bir şablona dönüştürmeniz sağlamaktır. Bunu gerçekleştirmek için GridView'ın akıllı etiketinde "Sütunları Düzenle" bağlantısına tıklayın, sonra da CommandField geçerli alanlar listesinden seçin ve "Bu alanı bir TemplateField dönüştürün" bağlantısına tıklayın. Bu CommandField ile bir TemplateField kapatır bir `ItemTemplate` ve `EditItemTemplate`. `ItemTemplate` Düzenle ve Sil LinkButtons sırasında içeren `EditItemTemplate` LinkButtons iptal etme ve güncelleştirme barındırır.


[![Bir TemplateField CommandField Dönüştür](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**Şekil 12**: CommandField içine bir TemplateField dönüştürme ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image36.png))


Düzenle ve Sil LinkButtons içinde güncelleştirme `ItemTemplate`ayarını kendi `ID` değerlerini özelliklerine `EditButton` ve `DeleteButton`sırasıyla.

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

Veri GridView'a bağlı her GridView kayıtları sıralar, `DataSource` özelliği ve karşılık gelen oluşturur `GridViewRow` nesne. Her `GridViewRow` nesnesi oluşturulur, `RowCreated` olay tetiklenir. Yetkisiz kullanıcılar için Düzenle ve Sil düğmeleri gizlemek için bu olay için bir olay işleyicisi oluşturma ve düzenleme ve silme ayarını LinkButtons programlı olarak başvurmak gerekiyor kendi `Visible` özellikleri uygun şekilde.

Bir olay işleyicisi oluşturun `RowCreated` olay ve ardından aşağıdaki kodu ekleyin:

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

Aklınızda `RowCreated` olayı tetikler için *tüm* üstbilgi, altbilgi, çağrı cihazı arabirimi ve diğerleri dahil olmak üzere GridView satır. Yalnızca (düzenleme modunda satırı güncelleştir ve İptal düğmeleri düzenleme ve silme yerine olduğundan) biz düzenleme modunda olmayan bir veri satırı ile uğraşıyorsanız düzenleme ve silme LinkButtons programlı olarak başvurmak istiyoruz. Bu denetimi tarafından işlenen `If` deyimi.

Biz düzenleme modunda olmayan bir veri satırı ile uğraşıyorsanız, Düzenle ve Sil LinkButtons başvurulur ve bunların `Visible` özellikleri tarafından döndürülen Boole değerleri baz alınarak ayarlanır `User` nesnenin `IsInRole(roleName)` yöntemi. `User` Nesneye başvuruda tarafından oluşturulan sorumlusu `RoleManagerModule`; sonuç olarak, `IsInRole(roleName)` yöntemi geçerli ziyaretçi ait olup olmadığını belirlemek için rolleri API kullanan *roleName*.

> [!NOTE]
> Rolleri sınıfı doğrudan, biz kullanabilirdik çağrısı değiştirerek `User.IsInRole(roleName)` çağrısıyla [ `Roles.IsUserInRole(roleName)` yöntemi](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Asıl nesne kullanmak karar `IsInRole(roleName)` yöntemi bu örnekte rolleri API doğrudan kullanmaktan daha etkili olduğundan. Bu öğreticide daha önce şu Rol Yöneticisi kullanıcı rolleri bir tanımlama bilgisinde önbelleğe almak için yapılandırılmış. Bu tanımlama bilgisi verileri önbelleğe yalnızca, kullanılan sorumlunun `IsInRole(roleName)` yöntemi çağrılır; doğrudan rolleri API çağrıları her zaman etmek rol deposu içerir. Asıl nesne rolleri bir tanımlama bilgisinde önbelleğe alınmamış olsa bile, çağırma `IsInRole(roleName)` yöntemi olduğundan genellikle daha verimli bir istek sırasında ilk kez sonuçlarını önbelleğe alır için ne zaman çağrılır. Öte yandan, rolleri API önbelleğe alma gerçekleştirmez. Çünkü `RowCreated` olay tetiklenir kez GridView her satır için kullanarak `User.IsInRole(roleName)` rol deposu için yalnızca bir seyahat ederken içerir `Roles.IsUserInRole(roleName)` gerektirir *N* dönüşle burada *N* olduğu Kılavuzda görüntülenen kullanıcı hesabı sayısı.


Düzenle düğmesinin `Visible` özelliği `True` varsa bu sayfasını ziyaret ederek kullanıcı Yöneticiler veya denetçiler rolünde; Aksi takdirde ayarlanmış `False`. Delete düğmenin `Visible` özelliği `True` yalnızca kullanıcı Yöneticiler rolüne ise.

Bu sayfa bir tarayıcı aracılığıyla test edin. Anonim bir ziyaretçi veya bir yönetici ya da bir yönetici bir kullanıcı olarak sayfayı ziyaret ederse, CommandField boştur; hala var ancak olarak ince bir Silver düzenleme veya silme olmadan düğmeler.

> [!NOTE]
> CommandField gizlemek mümkündür tamamen ne zaman bir yönetici olmayan ve yönetici olmayan ziyaret sayfası. Ben bunu bir alıştırma olarak için okuyucu bırakın.


[![Düzenle ve Sil düğmeleri olmayan denetçilere ve yönetici olmayan kullanıcılar için gizli](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**Şekil 13**: Düzenle ve Sil düğmeleri gizli olmayan denetçilere ve yönetici olmayan kullanıcılar için ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image39.png))


Denetçiler rolüne (ancak yöneticileri rolü için değil) ait bir kullanıcı ziyaret ederse, yalnızca Düzenle düğmesini görür.


[![Düzenle düğmesi denetçiler kullanılabilir olsa da, Sil düğmesi gizlenir](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**Şekil 14**: sırada Düzenle düğmesini denetçiler için kullanılabilir, Sil düğmesini gizli ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image42.png))


Ve yönetici ziyaret ederse, kendisi için Düzenle ve Sil düğmeleri erişimi vardır.


[![Düzenle ve Sil düğmeleri kullanılabilir yalnızca Yöneticiler için olan](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**Şekil 15**: Düzenle ve Sil düğmeleri, kullanılabilir yalnızca Yöneticiler ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>3. adım: Sınıflar ve yöntemler için rol tabanlı yetkilendirme kuralları uygulama

Biz sınırlı 2. adımda denetçilere ve yöneticilerin rolleri özelliklerini düzenleyin ve yalnızca Yöneticiler olanağı silin. Bu işlem, yetkisiz kullanıcıların programlama teknikleri ile ilişkili kullanıcı arabirimi öğeleri gizleyerek gerçekleştirilmiştir. Böyle ölçüler yetkisiz bir kullanıcı ayrıcalıklı bir eylemi gerçekleştiremediği olacağını garanti. Daha sonra eklenen kullanıcı arabirimi öğeleri veya biz yetkisiz kullanıcılar için gizlemek unutmuş olabilir. Veya bir bilgisayar korsanının istenen metodunu yürütmek için ASP.NET sayfasını almak için başka bir şekilde keşfedebilir.

Bu sınıf ya da yöntemiyle donatmak için işlevsellik belirli bir parçasını yetkisiz kullanıcılar tarafından erişilmediğinden emin olmak için kolay bir yol olan [ `PrincipalPermission` özniteliği](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). .NET çalışma zamanı kullanan bir sınıf veya yöntemlerinden birini yürütür, geçerli güvenlik bağlamı izni olduğundan emin olmak için denetler. `PrincipalPermission` Özniteliği yoluyla tanımlarız bu kuralları bir mekanizma sağlar.

Kullanarak incelemiştik `PrincipalPermission` geri özniteliğini <a id="_msoanchor_9"> </a> [ *kullanıcı tabanlı yetkilendirme* ](../membership/user-based-authorization-vb.md) öğretici. GridView'ın donatmak nasıl özellikle gördüğümüz `SelectedIndexChanged` ve `RowDeleting` olay işleyicisi, böylece bunlar yalnızca kimliği doğrulanmış kullanıcılar ve Tito, tarafından sırasıyla yürütülebilir. `PrincipalPermission` Özniteliği rolleriyle da çalışır.

Kullanarak gösterelim `PrincipalPermission` GridView'ın özniteliği `RowUpdating` ve `RowDeleting` yetkili olmayan kullanıcılar için yürütme önlemek için olay işleyicileri. Yapmamız gereken şey her işlev tanımı üzerine uygun özniteliğini ekleyin:

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

Öznitelik için `RowUpdating` olay işleyicisi belirleyen Yöneticiler veya denetçiler roller yalnızca kullanıcılara olay işleyicisi, burada özniteliği olarak yürütebilir `RowDeleting` olay işleyicisi yürütme yöneticileri kullanıcılara sınırlar rolü.


> [!NOTE]
> `PrincipalPermission` Özniteliği bir sınıf olarak temsil edilir `System.Security.Permissions` ad alanı. Eklediğinizden emin olun bir `Imports System.Security.Permissions` bu ad alanı içeri aktarmak için arka plan kod sınıfı dosyanızın üst kısmındaki deyimi.


Bu şekilde, yönetici olmayan yürütmeyi denerse, `RowDeleting` olay işleyicisi veya yürütülecek olmayan yönetici veya yönetici olmayan bir girişiminde bulunursa `RowUpdating` .NET çalışma zamanı olay işleyicisi yükseltmek bir `SecurityException`.


[![Güvenlik bağlamı metodunu yürütmek için yetkili değil, bir SecurityException oluşturulur](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**Şekil 16**: metodunu yürütmek için güvenlik içeriğini Yetkilendirilmemişse bir `SecurityException` oluşturulur ([tam boyutlu görüntüyü görmek için tıklatın](role-based-authorization-vb/_static/image48.png))


ASP.NET sayfaları yanı sıra birçok uygulama ayrıca iş mantığı ve veri erişim katmanları gibi çeşitli katmanları içeren bir mimari var. Bu katman, genellikle sınıf kitaplıkları uygulanır ve sınıflar ve iş mantığı ve verileri ilgili işlevleri gerçekleştirmek için yöntemler sağlar. `PrincipalPermission` Özniteliği, bu katmanlara yetkilendirme kurallarını uygulamak için kullanışlıdır.

Kullanma hakkında daha fazla bilgi için `PrincipalPermission` başvurmak yetkilendirme kuralları sınıfları ve yöntemleri tanımlamak için öznitelik [Scott Guthrie](https://weblogs.asp.net/scottgu/)ın blog girişine [iş ve veri katmanları kullanarak Yetkilendirmekurallarıekleme`PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Özet

Bu öğreticide kaba belirtmek ne incelemiştik ve hedeflemediğinizden yetkilendirme kuralları kullanıcı rollerine göre. ASP. Hangi kimlikleri izin veya hangi sayfalarına erişimi reddedildi belirtmek bir sayfa Geliştirici NET'in URL yetkilendirme özelliği sağlar. Geri gördüğümüz gibi <a id="_msoanchor_10"> </a> [ *kullanıcı tabanlı yetkilendirme* ](../membership/user-based-authorization-vb.md) Öğreticisi, URL yetkilendirme kuralları kullanıcı tarafından olarak uygulanabilir. Bu öğreticinin 1. adım gördüğümüz gibi bir rol rollü temelinde de uygulanabilir.

Hedeflemediğinizden yetkilendirme kuralları, bildirimli olarak veya programlama yoluyla uygulanabilir. 2. adımda ziyaret kullanıcı rollerine göre farklı bir çıkış işleme için LoginView denetimin RoleGroups özelliğini kullanarak incelemiştik. Ayrıca programlı olarak bir kullanıcı belirli bir rolü ve sayfanın işlevselliği uygun şekilde ayarlama konusunda ait olup olmadığını belirlemek için yol inceledik.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İş ve veri katmanlarını kullanmak için yetkilendirme kuralları ekleme `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET 2.0'ın İnceleme üyelik, roller ve profil: rolleriyle çalışma](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [ASP.NET 2.0 için Güvenlik sorusu listesi](https://msdn.microsoft.com/library/ms998375.aspx)
- [İçin teknik belgeler `<roleManager>` öğesi](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET Books yazar ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan  *[Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott, konumunda ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel performanstan...

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Suchi Banerjee ve Teresa Murphy içerir. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](assigning-roles-to-users-vb.md)
