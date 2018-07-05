---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Üyelik | Microsoft Docs
author: microsoft
description: ASP.NET üyelik oluşturur Forms kimlik doğrulaması modelin başarı ASP.NET tarafından 1.x. ASP.NET formları kimlik doğrulamasını incorp için kullanışlı bir yol sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: ebba0c25fd3c7d6de7182c14559add7902cafe83
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388402"
---
<a name="membership"></a>Üyelik
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET üyelik oluşturur Forms kimlik doğrulaması modelin başarı ASP.NET tarafından 1.x. ASP.NET formları kimlik doğrulaması, oturum açma formu ASP.NET uygulamanıza eklemenize ve bir veritabanı veya başka bir veri deposunda kullanıcıları doğrulamak için kullanışlı bir yol sağlar.


ASP.NET üyelik oluşturur Forms kimlik doğrulaması modelin başarı ASP.NET tarafından 1.x. ASP.NET formları kimlik doğrulaması, oturum açma formu ASP.NET uygulamanıza eklemenize ve bir veritabanı veya başka bir veri deposunda kullanıcıları doğrulamak için kullanışlı bir yol sağlar. FormsAuthentication sınıf üyelerinin kimlik doğrulaması için tanımlama bilgilerini işlemek için geçerli bir oturum açma denetleyin, bir kullanıcının oturumunu vb. oturum mümkün kılar. Ancak, form kimlik doğrulaması, bir ASP.NET 1.x uygulamasında uygulama ciddi miktarda bir kod gerektirebilir.

ASP.NET 2.0 büyük bir ilerleme kullanarak form kimlik doğrulaması başına üzerinden gerekliliktir. (Üyelik form kimlik doğrulaması ile sıkı bağlı olduğunda en güçlü ancak form kimlik doğrulaması kullanarak bir gereksinim değildir.) Kısa süre içinde anlatıldığı gibi güçlü üyelik sistemi kadar kod yazmadan uygulamak için ASP.NET 2.0 ile ASP.NET üyelik ve oturum açma denetimleri kullanabilirsiniz.

## <a name="implementing-membership-in-aspnet-20"></a>ASP.NET 2.0 uygulama üyeliği

Üyelik, dört adımları izleyerek uygulanır. De uygulanabilir isteğe bağlı yapılandırma yanı sıra söz konusu olan çok sayıda alt adım olduğunu aklınızda bulundurun. Bu adımları üyelik yapılandırma büyük resmi göstermeye yöneliktir.

1. (SQL Server üyelik deposu kullanılır.) üyelik veritabanınızı oluşturun
2. Uygulamaları Yapılandırma dosyalarınızda üyelik seçenekleri belirtin. (Üyelik varsayılan olarak etkindir.)
3. Kullanmak istediğiniz üyelik deposu türünü belirler. Seçenekler şunlardır: 

    - Microsoft SQL Server (sürüm 7.0 veya üzeri)
    - Active Directory Store
    - Özel üyelik sağlayıcısı
4. Uygulama ASP.NET formları kimlik doğrulaması için yapılandırın. Bir kez daha, üyelik, form kimlik doğrulaması yararlanmak için tasarlanmıştır ancak form kimlik doğrulaması kullanarak bir gereksinim değildir.
5. Üyelik için kullanıcı hesapları tanımlayın ve isterseniz rollerini yapılandırın.

## <a name="creating-the-membership-database"></a>Üyelik veritabanı oluşturma

SQL Server 7.0 kullanarak e ya da ASP.NET üyelik deponuz olarak daha sonra kullanabileceğiniz\_regsql yardımcı programını (en kolay Visual Studio .NET 2005 Komut İstemi'nden kullanılabilir) veritabanınızı yapılandırmak için. ASP.NET\_regsql yardımcı programı, bir komut istemi aracı olarak veya bir GUI Sihirbazı aracılığıyla kullanılabilir. Sihirbazı yöntemi, veritabanınızı yapılandırmak için en kolay yoludur. Sihirbaza erişmek için yalnızca aşağıdaki komutu çalıştırın:

`aspnet_regsql W`

Bu komutu çalıştırdıktan sonra ASP.NET SQL Sunucusu Kurulum Sihirbazı ile aşağıda gösterildiği gibi açılır.


![](membership/_static/image1.jpg)

**Şekil 1**


ASP.NET SQL Sunucusu Kurulum Sihirbazı, sihirbazda belirttiğiniz örnekteki Web sitesi oluşturur. Ancak, veritabanınıza bağlanmak için ASP.NET bağlantı dizesini machine.config dosyasında kullanırsınız. Varsayılan olarak, bu bağlantı dizesini bir SQL Server 2005 örneğine işaret edecek şekilde bir SQL Server 2000 veya SQL Server 7.0 örneği kullanıyorsanız, machine.config dosyasında bağlantı dizesini değiştirmeniz gerekir. Bağlantı dizesini burada bulunabilir:

[!code-xml[Main](membership/samples/sample1.xml)]

Bağlantı dizesi değiştirmez, ne yazık ki, ASP.NET, açıklayıcı hata vermeyiz. Bu yalnızca veritabanı oluşturmadınız bildiren şikayet devam edecektir. Yukarıdaki durumda ı bağlantı dizesi, yerel SQL Server 2000 Örneğim işaret edecek şekilde değiştirdiniz.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Yapılandırma ve ekleme kullanıcıları ve rolleri belirtme

Üyelik yapılandırma sonraki adım, uygulamanın web.config dosyasına gerekli bilgileri eklemektir. ASP.NET'te 1.x web.config dosyasını değiştirerek, bazen lowerCamelCase kullanımını ve IntelliSense olmaması nedeniyle zordu. Visual Studio .NET 2005 görev yapılandırma dosyaları için IntelliSense ile çok daha kolaylaştırır, ancak ASP.NET 2.0 yapılandırma dosyalarını düzenlemek için bir Web arabirimi sağlayan bir adım daha ileri gider.

Web arabirimi, aşağıda gösterildiği gibi Çözüm Gezgini araç çubuğunda ASP.NET yapılandırma düğmesini tıklatarak başlatabilirsiniz. Web arabirimi üzerinden oturum açma denetimleri eklendiğinde görüntülenen açılır pencereleri da başlatabilirsiniz.


![](membership/_static/image2.jpg)

**Şekil 2**


Bu, aşağıda gösterilen ASP.NET Web sitesi yönetim aracını çalıştırır. ASP.NET Web sitesi yönetimi, uygulama ayarlarını yönetmek kolay bir dört sekme arabirimidir. Aşağıdaki sekmeleri kullanılabilir:

- **Giriş**
- **Güvenlik** kullanıcıları, rolleri ve erişim yapılandırın.
- **Uygulama** uygulama ayarlarını yapılandırın.
- **Sağlayıcı** yapılandırma ve test, uygulamaları üyelik sağlayıcısı.

Web Sitesi Yönetim Aracı'nı kolayca yeni kullanıcı oluşturma, yeni roller oluşturmanız ve kullanıcıları ve rolleri yönetmek için sağlar. Bu özellik Windows arabiriminin kullanılabilir değil. Windows arabirimi kolayca yetkilendirme ayarları tanımlamak için ekleme, silme ve sağlayıcıları, Web Sitesi Yönetim Aracı'nda olmayan özellikleri yönet sağlar.

Windows arabirimini başlatmak için Internet Information Services ek bileşenini açın, uygulamanız üzerinde sağ tıklayın ve Özellikler'i seçin. ASP.NET sekmesine tıklayın ve ardından yapılandırmasını düzenle düğmesine tıklayın. (Uygulamanın etkinleştirilmesi için yapılandırmasını Düzenle düğmesi için ASP.NET 2.0'altında çalışıyor olması gerekir. Verze technologie ASP.NET ASP.NET iletişim kutusundaki de yapılandırabilirsiniz.) ASP.NET yapılandırma ayarları iletişim kutusu, aşağıda gösterildiği gibi görüntülenir.


![](membership/_static/image3.jpg)

**Şekil 3**


Genel sekmesinde, bağlantı dizeleri ve uygulama ayarları listelenir. İtalik herhangi bir ayarı bir üst yapılandırma dosyasındaki (machine.config veya web.config daha yüksek bir düzeyde) tanımlanır ve uygulama yapılandırma dosyasından ayarlardır italik içinde değil. Bir ayar eklenirse, kaldırıldı veya uygulama düzeyinde düzenlendiğinde bunlarla ASP.NET eklemek, kaldırmak veya içinden devralınır yapılandırma dosyasından ayar kaldırma yerine uygulama düzeyi web.config ayarı değiştirmek.

Kimlik doğrulama sekmesine aşağıda gösterilmiştir. Üyelik ayarlarınızı yapılandıracağınız budur. Forms kimlik doğrulama ayarları, üyelik sağlayıcıları ve rol sağlayıcıları buradan yapılandırılabilir.


![](membership/_static/image4.jpg)

**Şekil 4**


## <a name="implementing-membership-in-your-application"></a>Uygulamanızda üyelik uygulama

ASP.NET 2.0 üyelik uygulamanıza en kolay yolu, sağlanan oturum açma denetimleri kullanmaktır. Bu yöntem herhangi bir kod yazmadan üyelik ASP.NET 2.0 temelleri olanak tanır.

Aşağıdaki oturum açma denetimleri, ASP.NET 2.0 sürümünde mevcuttur:

## <a name="login-control"></a>Login denetimi

Oturum açma denetimi, birisi üyelik sisteminize oturum için bir arabirim sağlar. Bu, bir kullanıcı adı ve parola textboxt ve oturum açma düğmesi sağlar. Diğer birçok ortak özelliği bundan sonraki ziyaretlerinizde otomatik olarak oturum açma için kullanıcı sağlayan bir onay kutusu bu yüzden henüz yapmadıysanız kişilerin kaydolmak için bir bağlantı, bağlantı için bir parola anımsatıcı, vb. gibi. Oturum açma denetimi özelliklerinin tümünü denetimin özellikleri özelleştirilemez.

ASP.NET'te 1.x, geliştiricilerin ciddi miktarda bir form kimlik doğrulaması kullanırken bir arama yapmak için kod yazmak vardı. ASP.NET 2.0 üyelikte, kullanıcıların herhangi bir kod yazmadan doğrulayabilirsiniz. ASP.NET aramasında kullanıcının sizin için otomatik olarak yapar. (ASP.NET üyeliği kullanmadan oturum açma denetimi kullanıyorsanız, kullanabileceğiniz **OnAuthenticate** kullanıcıyı doğrulamak için yöntem.)

## <a name="loginview-control"></a>LoginView denetimi

Bir LoginView denetimi varsayılan iki şablonu sağlayan şablonlu bir denetimdir; Anonymous ve LoggedInTemplate. Görüntülenen şablonu üyelik sisteminize kullanıcı bırakmadığınıza kaydedilir tarafından belirlenir. Bu denetim genellikle kullanıcının oturum açmış olduğu bir kullanıcı henüz oturum değil oturum açma denetime ve bir LoginStatus denetimi ve/veya diğer oturum açma denetimlerini görüntülemek için kullanılır. ASP.NET uygulamanızı rol yönetimi kullanıyorsanız, kullanıcı rolüne dayalı belirli bir şablon LoginView denetimi görüntüleyebilirsiniz. (Daha üzerinde ASP.NET rol yönetimi daha sonra ele alınacaktır.)

## <a name="passwordrecovery-control"></a>PasswordRecovery denetimi

Kullanıcıların geçerli parolasını içeren bir e-posta veya parolasını sıfırlama PasswordRecovery denetimi sağlar. Düz metin ve şifrelenmiş parolalar kurtarıldı ve kullanıcılara e-posta ile. Parola karma, kurtarılamaz. Bunun yerine kullanıcının parolayı sıfırlaması gerçekleştirmek için gerekli olacaktır.

## <a name="loginstatus-control"></a>LoginStatus denetimi

Bu denetim, oturum açmış kullanıcılar bir oturum açma belirteci açmadıysanız kullanıcılar ve oturum kapatma göstergesi görüntülemek için kullanılır. Request.IsAuthenticated özelliği görüntülemek için hangi göstergesi belirlemek için kullanılır. Metin LoginStatus denetimi tarafından görüntülenen göstergesi olabilir (aracılığıyla uygulanan **LoginText** ve **LogoutText** özellikleri) veya resimleri (aracılığıyla uygulanan **LoginImageUrl**ve **LogoutImageUrl** özelliklerini.)

LoginStatus denetimi bir kullanıcının oturumunu kapatır, isterse tarafından belirtilen URL'ye yeniden yönlendirilir **LogoutPageUrl** özelliği. Bu özellik ayarlanmamışsa, geçerli sayfa yenilenir. Site olası form kimlik doğrulamasını korumalı olduğundan, geçerli sayfa yenileme kullanıcı site için oturum açma sayfasına yönlendirir.

## <a name="loginname-control"></a>LoginName denetimi

Bir LoginName denetimi siteye şu anda oturum açmış kullanıcının kullanıcı adı görüntüler.

## <a name="createuserwizard-control"></a>CreateUserWizard denetimi

Üyelik sisteminize kaydetmek için kullanışlı bir yol kullanıcılarla CreateUserWizard denetim sağlar. Aşağıda gösterilen arabirimi üzerinden (WizardSteps koleksiyonu olarak uygulanmış) adımlar ekleyebilirsiniz.


![](membership/_static/image5.jpg)

**Şekil 5**


CreateUserWizard Sihirbazı sınıftan türetilen ve aşağıdaki şablonlardan sağlayan şablonlu bir denetimdir:

- **HeaderTemplate** Bu şablon Sihirbazı üstbilgisi görünümünü denetler.
- **SideBarTemplate'i** Bu şablon Sihirbazı kenar görünümünü denetler.
- **StartNavigationTemplate'i** Gezinti görünümünü olan adım başlangıcı sırasında sihirbazın bu şablonu kontrol eder.
- **StepNavigationTemplate'i** Bu şablon Gezinti alanına değil, başlangıç veya bitiş adımı görünümünü denetler.
- **FinishNavigationTemplate'i** Bu şablon, sonlandırma adımında Gezinti alan görünümünü denetler.

Ayrıca, sihirbaza eklediğiniz her adım için ASP.NET bir ContentTemplate hem de bu adımı için bir CustomNavigationTemplate'i içeren özel bir şablon oluşturur. VS.NET 2005 belgeleri CreateUserWizard özelleştirme hakkında tam Ayrıntılar için bkz:

## <a name="changepassword-control"></a>ChangePassword denetimi

ChangePassword denetimi kullanıcıların parolasını değiştirmesine izin verir. DisplayUserName özelliği (varsayılan olarak false olduğu) true ise, bunlar açmadıysanız, kullanıcı parolasını değiştirebilir. Kullanıcının *olduğu* zaten oturum açmış ve DisplayUserName özelliği true olarak ayarlandığında, kullanıcı bu kullanıcının kullanıcı kimliği alışık oldukları sağlama konusunda oturum açmamış başka bir kullanıcının parolasını değiştirmesi mümkün olacaktır.

Kullanıcıların oturum açmak zorunda kalmadan parolalarını değiştirmek istiyorsanız, ChangePassword denetimi görüntülendiği sayfayı anonim erişim verdiğinden emin olmak gereken göz önünde bulundurun. Kuşkusuz, kullanıcıların parolalarını değiştirmek için eski parola sağlamanız gerekir.

## <a name="role-management"></a>Rol yönetimi

Rol yönetimi, kullanıcıların belirli bir role atayın ve ardından belirli dosya veya klasörleri, rol tabanlı erişimi sağlar. Rol yönetimi ayrıca bir API sağlar, böylece program aracılığıyla bağlayıcının rol belirleyebilir veya belirli bir roldeki tüm kullanıcıları belirlemek ve uygun şekilde yanıt verin.

Rol yönetimi ASP.NET üyeliği bir gereksinim değildir ve üyelik rol yönetimini kullanmak için bir gereksinimdir. Ancak, iki birbirine güzelce desteklemek ve geliştiricilerin bunları birbirine birlikte kullanacağını olasılığı yüksektir.

Uygulamanızdaki rol yönetimini etkinleştirmek için web.config dosyanızda aşağıdaki değişikliği yapın:

[!code-xml[Main](membership/samples/sample2.xml)]

Zaman **cacheRolesInCookie** özniteliği true, ASP.NET bir kullanıcı rolü üyeliği istemcide bir tanımlama bilgisinde önbelleğe. Bu rol aramaları RoleProvider çağrılar olmaksızın gerçekleşmesine izin verir. Bu öznitelik kullanırken, geliştiricilerin emin olmak için kullanmaları **cookieProtection** özniteliği için tüm ayarlanır. (Varsayılan ayar budur.) Bu tanımlama bilgisi verileri şifrelenir ve tanımlama bilgileri içeriği değiştirilmediğini sağlamaya yardımcı olur sağlar. Web Sitesi Yönetim Aracı'nı kullanarak rolleri eklenebilir. Kolayca rolleri tanımlamak, bu rollere göre site bölümlerini erişimi yapılandırma ve kullanıcıları rollere atarsınız olanak tanır.


![](membership/_static/image6.jpg)

**Şekil 6**


Yukarıda gösterildiği gibi yalnızca rolün adını girip ardından Rol Ekle tıklayarak yeni rolleri eklenebilir. Var olan rolleri yönetilen veya var olan rolleri listesinden uygun bağlantıyı tıklatarak silinmiş.

Bir rol yönetirken ekleyebilir veya kullanıcılar aşağıda gösterildiği gibi kaldırın.


![](membership/_static/image7.jpg)

**Şekil 7**


Kullanıcı rolü onay kutusunu işaretleyerek, belirli bir role kullanıcı kolayca ekleyebilirsiniz. ASP.NET, uygun girişleri ile üyelik veritabanınızı otomatik olarak güncelleştirecektir. Ayrıca, uygulamanız için erişim kurallarını yapılandırmak isteyeceksiniz. ASP.NET 1.x geliştiriciler aracılığıyla bunu ile tanıdık &lt;yetkilendirme&gt; web.config dosyasını ve bu seçenek öğesinde ASP.NET 2.0 hala kullanılabilir. Ancak, erişimi yönetmek kolay, Web sitesi yönetim aracını aşağıda gösterilen şekilde kullanarak kurallar.


![](membership/_static/image8.jpg)

**Şekil 8**


Bu durumda, yönetim klasörü vurgulanır (kendi zor olduğundan aracı açık gri renkte vurgular görmek) ve yöneticiler rolüne erişim verildi. Diğer tüm kullanıcılar izin verilmez. Bir kural seçin ve ardından Yukarı Taşı ve Aşağı Taşı düğmeleri kuralları düzenlemek için baş simgesine tıklayabilirsiniz. ASP.NET ile &lt;yetkilendirme&gt; öğesi, kuralları, göründükleri sırayla işlenir. Yukarıdaki görüntüsü kurallarında sırasını tersine çevrilmiş, ASP.NET karşılaşacağınız ilk kural herkesin klasörüne engellediği kural olacağından başka bir deyişle, hiç erişim yönetim klasöre gerekir.

ASP.NET 2.0 klasöre erişim kuralı belirten bir web.config dosyası ekler. Erişim kuralları, yapılandırma dosyası veya Web sitesi yönetim aracı yoluyla düzenlenebilir. Diğer bir deyişle, Web Sitesi Yönetim Aracı'nı yapılandırma dosyası, kullanıcı dostu bir ortamda düzenlenebilir basit bir arabirimdir.

## <a name="using-roles-in-code"></a>Rolleri, kod içinde kullanma

Rol yönetimi için API sürümünden bu yana değişmemiştir 1.x. **IPrincipal** yöntemi, bir kullanıcının belirli bir rolde olup olmadığını belirlemek için kullanılır.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET, ayrıca geçerli bağlam üyesi olarak bir RolePrincipal örneği oluşturur. RolePrincipal nesnesi, tüm kullanıcı gibi ait olduğu rollerin elde etmek için kullanılabilir:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>RoleGroups LoginView denetimi ile kullanma

Rol yönetimi hem de üyelik bir anlayışa sahip olduğunuza göre kısaca nasıl LoginView denetimi bu özellik ASP.NET 2. 0'yararlanır tartışmanıza olanak tanır. Daha önce bahsedildiği gibi varsayılan olarak iki şablon bulunur şablonlu bir denetim LoginView denetimi olduğunu; Anonymous ve LoggedInTemplate. LoginView iletişim (aşağıda gösterilen) bir bağlantıdır kolekci RoleGroups olanak tanıyan içindeki görevlerin.


![](membership/_static/image9.jpg)

**Şekil 9**


Her RoleGroup nesne RoleGroup uygulandığı hangi rollerin tanımlayan bir dize dizisi içerir. Yeni bir RoleGroup LoginView denetimi eklemek için RoleGroups Düzenle bağlantısına tıklayın. Yukarıdaki görüntüde, Yöneticiler için yeni bir RoleGroup eklediğinizden emin görebilirsiniz. Bu RoleGroup seçerek (RoleGroup[0]) görünümleri açılan listeden, ı yapılandırabilir yalnızca Yöneticiler rolünün bir üyesi için görüntülenecek bir şablon. Aşağıdaki görüntüde, satış rolünde ve dağıtım rolü üyelerine uygulayan yeni bir RoleGroup ekledim. Bu ikinci RoleGroup LoginView görevleri iletişim kutusu görünüm açılır menüden ekler ve bu şablona eklediğiniz herhangi bir şey satış veya dağıtım içindeki herhangi bir kullanıcı tarafından görünür olacak rol.


![](membership/_static/image10.jpg)

**Şekil 10**


## <a name="overriding-the-existing-membership-provider"></a>Mevcut üyelik sağlayıcısı geçersiz kılma

ASP.NET üyelik işlevselliğini genişletin yoldan birkaç vardır. İlk olarak, açıkça SqlMembershipProvider sınıfın var olan işlevselliği bundan devralan ve metotlarını geçersiz kılma değiştirebilirsiniz. Örneğin, kullanıcıların oluşturulduğunda kendi işlevselliği uygulamak istiyorsanız, aşağıdaki gibi SqlMembershipProvider devralan kendi sınıfınızı oluşturabilirsiniz:

[!code-csharp[Main](membership/samples/sample5.cs)]

Öte yandan, kendi sağlayıcınızı (Üyelik bilgilerinizi depolamak Access veritabanında, örneğin için) oluşturmak istiyorsanız, kendi sağlayıcınızı oluşturabilirsiniz.

## <a name="creating-your-own-membership-provider"></a>Kendi üyelik sağlayıcısı oluşturma

Kendi üyelik sağlayıcısı oluşturmak için öncelikle MembershipProvider sınıfından devralan bir sınıf oluşturmanız gerekecektir. Visual Studio 2005 VB.NET kullanıyorsanız, tüm geçersiz kılmak için gereken yöntemleri için saplamalar ekler. C#, saptamalar eklemeyi, maksimum kullanıyorsanız.

Aşağıdaki geçersiz kılmak şunlar gerekir:

- ApplicationName özelliği
- ChangePassword işlevi
- ChangePasswordQuestionAndAnswer işlevi
- CreateUser işlevi
- DeleteUser işlevi
- EnablePasswordReset özelliği
- EnablePasswordRetrieval özelliği
- FindUsersByEmail işlevi
- FindUsersByName işlevi
- GetAllUsers işlevi
- GetNumberOfUsersOnline işlevi
- GetPassword işlevi
- GetUser işlevi
- GetUserNameByEmail işlevi
- MaxInvalidPasswordAttempts özelliği
- MinRequiredNonAlphanumericCharacters özelliği
- MinRequiredPasswordLength özelliği
- PasswordAttemptWindow özelliği
- PasswordFormat özelliği
- PasswordStrengthRegularExpression özelliği
- RequiresQuestionAndAnswer özelliği
- RequiresUniqueEmail özelliği
- ResetPassword işlevi
- Unlock kullanıcı işlevi
- UpdateUser işlevi
- ValidateUser işlevi

Thats bir C# geliştiricisi olarak uygulamak için tam anlamıyla bir listesi. Sınıf içinde VB.NET herhangi bir uygulama oluşturun ve C# kodu dönüştürülecek .NET Reflector veya benzer bir araç kullanın daha kolay bulabilirsiniz.

Bağlantı dizesi ve diğer özellikleri değerlerinde Initialize yöntemi olarak ayarlamanız gerekir. (Sağlayıcının çalışma zamanında yüklendiğinde Initialize yöntemi tetiklenir.) Initialize yöntemi ikinci parametresi System.Collections.Specialized.NameValueCollection türüdür ve referans &lt;ekleme&gt; web.config dosyasında özel sağlayıcınızla ilişkili öğe. Bu giriş, aşağıdaki gibi görünür:

[!code-xml[Main](membership/samples/sample6.xml)]

Initialize yöntemi örneği aşağıda verilmiştir.

[!code-csharp[Main](membership/samples/sample7.cs)]

Bunlar, oturum açma formu gönderdiğinde, kullanıcıyı doğrulamak için ValidateUser yöntemi kullanmanız gerekir. Kullanıcı oturum açma denetimi oturum açma düğmeye tıkladığında bu yöntemi tetikler. Bu yöntem kullanıcı arama yapan kodunuzu yerleştirmeniz gerekir.

Gördüğünüz gibi kendi üyelik sağlayıcısı yazma zor değildir ve bu güçlü ASP.NET 2.0 genişletmek sağlar.
