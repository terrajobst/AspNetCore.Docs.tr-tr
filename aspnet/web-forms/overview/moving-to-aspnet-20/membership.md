---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: "Üyelik | Microsoft Docs"
author: microsoft
description: "ASP.NET üyelik derlemeler Forms kimlik doğrulaması modeli başarı ASP.NET tarafından 1.x. ASP.NET formları kimlik doğrulamasını incorp için kullanışlı bir yol sağlayan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: 7e6bff33e9ec1de03c591de6eee2e632c7b7509d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="membership"></a>Üyelik
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET üyelik derlemeler Forms kimlik doğrulaması modeli başarı ASP.NET tarafından 1.x. ASP.NET Forms kimlik doğrulaması, ASP.NET uygulamanıza oturum açma formu birleştirme ve bir veritabanı veya başka bir veri deposunda karşı kullanıcıları doğrulamak için kullanışlı bir yol sağlar.


ASP.NET üyelik derlemeler Forms kimlik doğrulaması modeli başarı ASP.NET tarafından 1.x. ASP.NET Forms kimlik doğrulaması, ASP.NET uygulamanıza oturum açma formu birleştirme ve bir veritabanı veya başka bir veri deposunda karşı kullanıcıları doğrulamak için kullanışlı bir yol sağlar. FormsAuthentication sınıfı üyeleri kimlik doğrulaması için tanımlama bilgileri işlemek için geçerli bir oturum denetleyin, vb. çıkışı bir kullanıcı oturum mümkün kılar. Ancak, bir ASP.NET 1.x uygulamasındaki form kimlik doğrulaması uygulama eşit miktarda kod gerektirebilir.

ASP.NET 2.0 ana terfi form kimlik doğrulaması başına kullanmaktansa gerekliliktir. (Üyelik Forms kimlik doğrulaması ile birlikte, en güçlü bağlıdır, ancak form kimlik doğrulaması kullanarak zorunlu değildir.) En kısa sürede anlatıldığı gibi güçlü üyelik sistemi kadar kod yazmadan uygulamak için ASP.NET 2.0 ile ASP.NET üyelik ve oturum açma denetimleri kullanabilirsiniz.

## <a name="implementing-membership-in-aspnet-20"></a>ASP.NET 2.0 üyelik uygulama

Üyelik dört adımları izleyerek uygulanır. De uygulanabilir isteğe bağlı yapılandırma hem de dahil birçok alt adım olduğunu aklınızda bulundurun. Bu adımları üyelik yapılandırma büyük resmi göstermeye yöneliktir.

1. (SQL Server üyelik depo olarak kullanılır.), üyelik veritabanının oluşturun
2. Uygulamaları Yapılandırma dosyalarınızda üyelik seçeneklerini belirtin. (Üyelik varsayılan olarak etkindir.)
3. Kullanmak istediğiniz üyelik deposu türünü belirler. Seçenekler şunlardır: 

    - Microsoft SQL Server (sürüm 7.0 veya üstü)
    - Active Directory deposu
    - Özel üyelik sağlayıcısı
4. Uygulama ASP.NET Forms kimlik doğrulaması için yapılandırın. Bir kez daha, üyelik form kimlik doğrulaması yararlanmak için tasarlanmıştır ancak form kimlik doğrulaması kullanarak zorunlu değildir.
5. Üyelik için kullanıcı hesapları tanımlayın ve isterseniz rollerini yapılandırın.

## <a name="creating-the-membership-database"></a>Üyelik veritabanı oluşturma

SQL Server 7.0 kullanarak e veya ASP.NET üyelik deponuz olarak daha sonra kullanabileceğiniz\_regsql yardımcı programını (en kolay Visual Studio .NET 2005 komut isteminden kullanılabilir) veritabanınızı yapılandırmak için. Aspnet\_regsql yardımcı programı, bir komut istemi aracı olarak veya bir GUI Sihirbazı aracılığıyla kullanılabilir. Sihirbazı yöntemi, veritabanınızı yapılandırmak için en kolay yoludur. Sihirbazı'na erişmek için yalnızca aşağıdaki komutu çalıştırın:

`aspnet_regsql W`

Bu komutu çalıştırdıktan sonra ASP.NET SQL Server Kurulum Sihirbazı ile aşağıda gösterildiği gibi sunulur.


![](membership/_static/image1.jpg)

**Şekil 1**


ASP.NET SQL Server Kurulum Sihirbazı, sihirbazda belirttiğiniz örneğinde Web sitesi oluşturur. Ancak, veritabanınıza bağlanmak için ASP.NET bağlantı dizesi machine.config dosyasındaki kullanırsınız. Varsayılan olarak, bu bağlantı dizesini bir SQL Server 2005 örneğine işaret edecek şekilde bir SQL Server 2000 veya SQL Server 7.0 örneği kullanıyorsanız, machine.config dosyasındaki bağlantı dizesi değiştirmeniz gerekir. Bu bağlantı dizesi burada bulunabilir:

[!code-xml[Main](membership/samples/sample1.xml)]

Bağlantı dizesi değiştirmeyin, ne yazık ki, ASP.NET, açıklayıcı bir hata veremezsiniz. Veritabanı oluşturmadınız bildiren şikayetçi yalnızca devam eder. Yukarıdaki durumda ı my yerel SQL Server 2000 örneğini göstermesi için bağlantı dizesi değiştirdiniz.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Yapılandırma ve ekleme kullanıcıları ve rolleri belirtme

Üyelik yapılandırma bir sonraki adım için gerekli bilgileri uygulamanın web.config dosyasına eklemektir. ASP.NET 1.x, web.config dosyasında değişiklik bazen lowerCamelCase kullanımını ve IntelliSense eksikliği nedeniyle zordu. Visual Studio .NET 2005 görev yapılandırma dosyaları için IntelliSense ile daha kolay hale getirir, ancak ASP.NET 2.0 yapılandırma dosyalarını düzenlemek için bir Web arabirimi sağlayarak bir adım gider.

Web arabirimi aşağıda gösterildiği gibi Çözüm Gezgini araç çubuğundaki ASP.NET yapılandırması düğmesini tıklatarak başlatabilirsiniz. Oturum açma denetimleri eklendiğinde, görüntülenen açılır pencereleri aracılığıyla Web arabirimi de başlatabilirsiniz.


![](membership/_static/image2.jpg)

**Şekil 2**


Bu, ASP.NET Web sitesi yönetimi aşağıda gösterilen aracını çalıştırır. ASP.NET Web sitesi yönetimi, uygulama ayarlarını yönetmek kolaylaştıran bir dört sekme arabirimidir. Aşağıdaki sekmeleri kullanılabilir:

- **Giriş**
- **Güvenlik** kullanıcıları, rolleri ve erişim yapılandırın.
- **Uygulama** uygulama ayarlarını yapılandırın.
- **Sağlayıcı** yapılandırma ve test uygulamaları üyelik sağlayıcısı.

Web Sitesi Yönetim Aracı'nı kolayca create new users, yeni rolleri oluşturun ve kullanıcıları ve rolleri yönetmenize olanak sağlar. Bu özelliği Windows arabiriminde kullanılabilir değil. Windows arabirimini kolayca yetkilendirme ayarlarını tanımlamak için ve ekleme, silme ve sağlayıcıları, Web Sitesi Yönetim Aracı'nda olmayan özellikleri yönetmenize olanak sağlar.

Windows arabirimini başlatmak için Internet Information Services ek bileşenini açın, uygulamanızın üzerinde sağ tıklatın ve Özellikler'i seçin. ASP.NET sekmesini tıklatın ve ardından yapılandırmasını düzenle düğmesine tıklayın. (Uygulama etkinleştirilmesi, yapılandırmasını Düzenle düğmesi için ASP.NET 2.0'altında çalışmalıdır. "ASP.NET sürümünü de ASP.NET iletişim kutusunda yapılandırabilirsiniz.) ASP.NET yapılandırma ayarları iletişim kutusu, aşağıda gösterildiği gibi görüntülenir.


![](membership/_static/image3.jpg)

**Şekil 3**


Genel sekmesinde, bağlantı dizeleri ve uygulama ayarlarını listelenir. İtalik herhangi bir ayarı bir üst yapılandırma dosyasındaki (machine.config veya web.config daha yüksek bir düzeyde) tanımlanır ve uygulama yapılandırma dosyasından ayarlardır italik içinde değil. Bir ayar eklediyseniz, kaldırıldı veya uygulama düzeyinde düzenlenmiş ASP.NET eklemek, kaldırmak veya aralarından devralınıyor yapılandırma dosyasından ayar kaldırma yerine uygulama düzeyleri web.config ayarı değiştirmek.

Kimlik doğrulama sekmesi aşağıda gösterilmiştir. Üyelik ayarlarınızı burada yapılandıracağınız budur. Kimlik doğrulama ayarları, üyelik sağlayıcıları, formlar ve rol sağlayıcıları burada yapılandırılabilir.


![](membership/_static/image4.jpg)

**Şekil 4**


## <a name="implementing-membership-in-your-application"></a>Uygulamanızda üyelik uygulama

ASP.NET 2.0 üyelik uygulamanızda uygulamak için en kolay yolu, sağlanan oturum açma denetimleri kullanmaktır. Bu yöntem, ASP.NET 2.0 üyelik temelleri hiçbir kod yazmadan uygulamanızı sağlar.

ASP.NET 2.0 ile aşağıdaki oturum açma denetimleri mevcuttur:

## <a name="login-control"></a>Oturum açma denetimi

Oturum açma denetimi birisi, üyelik sisteme oturum için bir arabirim sağlar. Bu, bir kullanıcı adı ve parola textboxt ve oturum açma düğmesi sağlar. Birçok ortak gibi diğer özelliklere bir onay kutusu otomatik olarak oturum açma sonraki ziyaretlerinizde kullanıcıya izin veren şekilde henüz yapmadıysanız kişilerin kaydetmek için bir bağlantı, bir parola anımsatıcısı, vb. için bir bağlantı. Oturum açma denetimi tüm özellikleri denetim özelliklerini özelleştirilebilir.

ASP.NET 1.x, geliştiricilerin gerekiyordu eşit miktarda Forms kimlik doğrulaması kullanırken bir arama yapmak için kod yazma. ASP.NET 2.0 üyelikle hiçbir kod yazmadan kullanıcılar doğrulayabilirsiniz. ASP.NET kullanıcı arama sizin için otomatik olarak yapar. (ASP.NET üyelik kullanmadan oturum açma denetimi kullanıyorsanız, kullanabileceğiniz **OnAuthenticate** kullanıcıyı doğrulamak için yöntem.)

## <a name="loginview-control"></a>LoginView denetimi

LoginView denetim varsayılan olarak iki şablonları sağlayan şablonlu denetimdir; Anonymous ve LoggedInTemplate. Görüntülenen şablon kullanıcı olsun veya olmasın, üyelik sisteme oturum tarafından belirlenir. Bu denetim, genellikle bir kullanıcı henüz oturum açtıktan değil, bir oturum açma denetimi ve bu denetim ve/veya diğer oturum açma denetimleri kullanıcının oturum açtığı zaman görüntülemek için kullanılır. ASP.NET uygulamanızı rol yönetimi kullanıyorsanız, LoginView denetimi kullanıcı rolüne göre belirli bir şablon görüntüleyebilirsiniz. (Daha üzerindeki ASP.NET rol yönetimi daha sonra ele alınacaktır.)

## <a name="passwordrecovery-control"></a>PasswordRecovery denetimi

PasswordRecovery denetimi kullanıcıların geçerli parolasını içeren bir e-posta alamıyor veya parolasını sıfırlama olanak tanır. Düz metin ve şifrelenmiş parolalar kurtarıldı ve kullanıcılara e-postayla. Parola karma, kurtarılamıyor. Bunun yerine kullanıcının parola sıfırlama işlemini gerçekleştirmek için gerekli olacaktır.

## <a name="loginstatus-control"></a>Bu denetim

Bu denetim, oturum açmış kullanıcılar için oturum açma gösterge açmadınız kullanıcılara ve oturum kapatma göstergesi görüntülemek için kullanılır. Request.IsAuthenticated özelliği görüntülemek için hangi göstergesi belirlemek için kullanılır. Bu denetim tarafından görüntülenen göstergesi metin olabilir (aracılığıyla uygulanan **LoginText** ve **LogoutText** özellikleri) veya görüntüleri (aracılığıyla uygulanan **LoginImageUrl**ve **LogoutImageUrl** özelliklerini.)

Bu denetim bir kullanıcı oturumu kapattıktan sonra çözemiyorsa tarafından belirtilen URL'ye yeniden yönlendirilir **LogoutPageUrl** özelliği. Bu özellik ayarlanmamışsa, geçerli sayfa yenilenir. Site olası form kimlik doğrulamasını korumalı olduğundan, geçerli sayfa yenileme kullanıcı site için oturum açma sayfasına yönlendirir.

## <a name="loginname-control"></a>LoginName denetimi

LoginName denetimi siteye şu anda oturum açmış kullanıcı adını görüntüler.

## <a name="createuserwizard-control"></a>CreateUserWizard denetimi

CreateUserWizard denetim kullanıcılara üyelik sisteminizi kaydetmek için kolay bir yol sağlar. Aşağıda gösterilen arabirimi aracılığıyla (WizardSteps koleksiyonu olarak uygulanan) adımlar ekleyebilirsiniz.


![](membership/_static/image5.jpg)

**Şekil 5**


CreateUserWizard Sihirbazı sınıfından türetilen ve aşağıdaki şablonlardan sağlayan bir şablonlu denetimidir:

- **HeaderTemplate** Bu şablon Sihirbazı'nın başlığı görünümünü denetler.
- **SideBarTemplate'i** Bu şablon Sihirbazı'nın kenar görünümünü denetler.
- **StartNavigationTemplate'i** Gezinti görünümünü olan adım başlangıcı sırasında sihirbazın bu şablonu denetler.
- **StepNavigationTemplate'i** Bu şablon Gezinti bölmesinde değil olduğunda başlangıç veya bitiş adımı görünümünü denetler.
- **FinishNavigationTemplate'i** bu şablonu olduğunda son adımı Gezinti alan görünümünü denetler.

Ayrıca, Sihirbazı'na ekleme her adımı için ASP.NET bir ContentTemplate ve bu adım için bir CustomNavigationTemplate'i içeren özel bir şablon oluşturur. CreateUserWizard özelleştirme ile ilgili tam Ayrıntılar için VS.NET 2005 belgelerine bakın:

## <a name="changepassword-control"></a>Bu denetim

Bu denetim kullanıcıların parolasını değiştirmesine izin verir. DisplayUserName özelliği (varsayılan olarak false) true ise, bunlar açmadıysanız, kullanıcı parolasını değiştirebilirsiniz. Kullanıcı *olan* zaten oturum açmış ve DisplayUserName özelliği true olarak ayarlandığında, kullanıcı o kullanıcının kullanıcı Kimliğini bildikleri sağlama oturum açmamış başka bir kullanıcının parolasını değiştirmesi mümkün olacaktır.

Kullanıcıların oturum açmak zorunda kalmadan parolalarını değiştirmek istiyorsanız, bu denetim görüntülendiği sayfa anonim erişimi verdiğinden emin olmak gereken göz önünde bulundurun. Belli ki, kullanıcıların eski parolalarını parolalarını değiştirmeleri için sağlamanız gerekir.

## <a name="role-management"></a>Rol yönetimi

Rol yönetimi, kullanıcıların belirli bir role atamak ve belirli dosya veya klasörleri bu rolüne dayalı erişimi kısıtlamak sağlar. Program aracılığıyla someones rolünü belirleme veya belirli bir roldeki tüm kullanıcılara belirleyebilir ve uygun şekilde yanıt böylece rol yönetimi ayrıca bir API sağlar.

Rol yönetimi ASP.NET üyelik bir gereksinim değildir ve üyelik rol yönetimi kullanmak için bir gereksinimdir. Ancak, iki birbirine sorunsuz şekilde tamamlamak ve geliştiricilerin bunları birbirleri ile birlikte kullanacağını olasıdır.

Uygulamanızdaki rol yönetimi etkinleştirmek için aşağıdaki web.config dosyanızda değişiklik yapılabilir:

[!code-xml[Main](membership/samples/sample2.xml)]

Zaman **cacheRolesInCookie** özniteliği true, ASP.NET bir tanımlama bilgisinde istemci üzerinde kullanıcı rolü üyeliği önbelleğe. Bu rol aramaları RoleProvider çağrılarını ortaya sağlar. Bu öznitelik kullanırken, geliştiricilerin emin olmak için kullanmaları **cookieProtection** özniteliği tümüne ayarlanır. (Varsayılan ayar budur.) Bu tanımlama bilgisi veri şifrelenir ve tanımlama bilgileri içeriği değiştirilmiş henüz sağlamaya yardımcı olur sağlar. Web Sitesi Yönetim Aracı'nı kullanarak rolleri eklenebilir. Kolayca rolleri tanımlama, bu rollere göre sitesinin bölümlerini erişimi yapılandırma ve kullanıcıları rollere atarsınız imkan tanır.


![](membership/_static/image6.jpg)

**Şekil 6**


Yukarıda gösterildiği gibi yeni rolleri yalnızca Rol adını girmek ve Rol Ekle tıklatarak eklenebilir. Varolan rolleri yönetilen veya varolan rollerinin listesinden ilgili bağlantıyı tıklatarak silindi.

Bir rolü yönetirken ekleyebilir veya aşağıda gösterildiği gibi kaldırabilirsiniz.


![](membership/_static/image7.jpg)

**Şekil 7**


Kullanıcı rolü onay kutusunu işaretleyerek, belirli bir rol için bir kullanıcı kolayca ekleyebilirsiniz. ASP.NET ile ilgili girdileri otomatik olarak, üyelik veritabanının güncelleştirir. Uygulamanız için erişim kurallarını yapılandırmak isteyeceksiniz. ASP.NET 1.x geliştiricilerinin aracılığıyla bunu ile tanıdık &lt;yetkilendirme&gt; web.config dosyasını ve bu seçenek öğedir ASP.NET 2. 0 ' hala kullanılabilir. Ancak, kendi daha kolay erişimi yönetmek Web sitesi yönetim aracı gösterildiği gibi aşağıdaki kuralları kullanarak.


![](membership/_static/image8.jpg)

**Şekil 8**


Bu durumda, yönetim klasör vurgulanır (kendi zor aracı açık gri renkte vurgular çünkü görmek) ve yöneticiler rolünün erişim verildi. Diğer tüm kullanıcılara izin verilmez. Bir kural seçin ve ardından Yukarı Taşı ve Aşağı Taşı düğmeleri kuralları düzenlemek için baş simgesine tıklayabilirsiniz. ASP.NET ile &lt;yetkilendirme&gt; öğesi, kurallar, göründükleri sırada işlenir. Yukarıdaki görüntüsündeki kuralların sırasını ters kaydedildi, ASP.NET karşılaşacağınız ilk kural klasörü herkese engellediği kural olacağından diğer bir deyişle, hiç kimse Yönetim klasörüne erişebilir.

ASP.NET 2.0 bir erişim kuralı belirtme klasörüne bir web.config dosyası ekler. Erişim kuralları, yapılandırma dosyası aracılığıyla veya Web Sitesi Yönetim Aracı'nı aracılığıyla düzenlenebilir. Diğer bir deyişle, Web sitesi yönetim aracı üzerinden kullanıcı dostu bir ortamda yapılandırma dosyasını düzenlenebilir yalnızca bir arabirimdir.

## <a name="using-roles-in-code"></a>Rolleri kod içinde kullanma

Rol yönetimi için API sürümünden bu yana değişmemiştir 1.x. **IsInRole** yöntemi, bir kullanıcının belirli bir rolde olup olmadığını belirlemek için kullanılır.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET ayrıca geçerli bağlamı bir üyesi olarak bir RolePrincipal örneği oluşturur. RolePrincipal nesnesi, tüm kullanıcı gibi ait olduğu rollerin elde etmek için kullanılabilir:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>RoleGroups LoginView denetimi ile kullanma

Rol yönetimi hem de üyelik bilgiye sahip olduğunuza göre nasıl LoginView denetimi bu yetenek ASP.NET 2.0 ile yararlanan kısaca ele olanak sağlar. Daha önce ele alındığı gibi LoginView denetim varsayılan olarak iki şablonlarını içeren şablonlu denetimdir; Anonymous ve LoggedInTemplate. LoginView iletişim (aşağıda gösterilen) bir bağlantıdır RoleGroups düzenlemenize olanak sağlayan görevleri içinde.


![](membership/_static/image9.jpg)

**Şekil 9**


Her RoleGroup nesnesi RoleGroup uygulandığı hangi rollerin tanımlayan bir dizeler dizisi içerir. LoginView denetimine yeni RoleGroup eklemek, düzenlemek RoleGroups bağlantıya tıklayın. Yukarıdaki resimde, Yöneticiler için yeni bir RoleGroup eklediğinizden emin görebilirsiniz. Bu RoleGroup seçerek (RoleGroup[0]) görünümleri açılır öğesinden t yapılandırabilir sadece yöneticiler rolünün üyelerine gösterilecek bir şablon. Aşağıdaki resimde ı Satış rolü ve dağıtım rolü üyelerine uygulayan yeni bir RoleGroup eklediniz. Bu ikinci RoleGroup LoginView görevler iletişim görünümleri açılır ekler ve bu şablon için eklenen herhangi bir şey satış veya dağıtım içindeki herhangi bir kullanıcı tarafından görülebilir rol.


![](membership/_static/image10.jpg)

**Şekil 10**


## <a name="overriding-the-existing-membership-provider"></a>Varolan üyelik sağlayıcısı geçersiz kılma

Çeşitli şekillerde ASP.NET üyelik işlevselliğini genişletebilirsiniz vardır. İlk olarak, açıkça SqlMembershipProvider sınıfı mevcut işlevselliğini ondan devralan ve metotlarını geçersiz kılma değiştirebilirsiniz. Örneğin, kullanıcıların oluşturulduğunda kendi işlevselliği uygulamak istiyorsanız, aşağıdaki gibi SqlMembershipProvider devralan kendi sınıf oluşturabilirsiniz:

[!code-csharp[Main](membership/samples/sample5.cs)]

Diğer taraftan, kendi sağlayıcınızı (Üyelik bilgilerinizi depolamak Access veritabanında, örneğin için) oluşturmak istiyorsanız, kendi sağlayıcınızı oluşturabilirsiniz.

## <a name="creating-your-own-membership-provider"></a>Kendi üyelik sağlayıcısı oluşturma

Kendi üyelik sağlayıcısı oluşturmak için önce MembershipProvider sınıfından devralan bir sınıf oluşturmanız gerekir. VB.NET kullanıyorsanız, Visual Studio 2005 tüm geçersiz kılmak için gereken yöntemleri için yer tutucular ekleyin. C#, yer tutucular eklemek için de kendi yukarı kullanıyorsanız.

Aşağıdaki geçersiz kılmak gerekir:

- ApplicationName özelliği
- Parola değiştirme işlevi
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

Thats C# Geliştirici olarak uygulamak için oldukça bir listesi. Sınıf içinde VB.NET herhangi bir uygulaması oluşturun ve ardından kod C# dönüştürmek için .NET Reflector veya benzer bir araç kullanın daha kolay bulabilirsiniz.

Bağlantı dizesi ve diğer özellikleri varsayılanlarına Initialize yöntemi olarak ayarlanması gerekir. (Çalışma zamanında sağlayıcı yüklendiğinde Initialize yöntemi tetiklenir.) Initialize yöntemi için ikinci parametre türü System.Collections.Specialized.NameValueCollection ve başvuru &lt;ekleme&gt; web.config dosyasında özel sağlayıcınız ile ilişkili öğe. Bu girişi aşağıdaki gibi görünür:

[!code-xml[Main](membership/samples/sample6.xml)]

Initialize yöntemi bir örneği burada verilmiştir.

[!code-csharp[Main](membership/samples/sample7.cs)]

Kullanıcı, oturum açma formu gönderdiğinde doğrulamak için ValidateUser yöntemi kullanmanız gerekecektir. Bu yöntem, kullanıcı oturum açma denetimi oturum açma düğmesini tıklattığında ateşlenir. Bu yöntem kullanıcı arama yapan kodunuzu yerleştirir.

Gördüğünüz gibi kendi üyelik sağlayıcısı yazmak zor değil ve bu güçlü ASP.NET 2.0 işlevselliğini genişletmek sağlar.
