---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Hesap onaylamayı ve parola kurtarma ASP.NET Identity (C#) ile | Microsoft Docs
author: HaoK
description: Bu öğretici ilk tamamlanmalıdır yapmadan önce oturum açma, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturun. Bu öğretici...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0167388cf6b488b72ca36f583a7794690dbf9900
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876039"
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Hesap onaylamayı ve parola kurtarma ile ASP.NET Identity (C#)
====================
tarafından [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Bu öğreticiyi gerçekleştirmeden önce ilk tamamlanacaksa [günlüğünde, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Bu öğretici, daha fazla ayrıntı içerir ve yerel hesap onayı için e-posta ayarlayın ve ASP.NET Identity kendi Unutulan parolayı sıfırlamak kullanıcıların nasıl yapacağınızı gösterir. Bu makalede Rick Anderson tarafından yazılan ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung ve Suhas Joshi. NuGet örnek öncelikle Hao Kung tarafından yazıldı.


Kullanıcı hesabı için bir parola oluşturmak bir yerel kullanıcı hesabı gerektirir ve bu parolayı web uygulamasında (güvenli) depolanır. ASP.NET kimliği, uygulama için bir parola oluşturmak kullanıcı gerektirmeyen sosyal hesapları da destekler. [Sosyal hesapları](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) kullanıcıların kimliklerini doğrulamak için bir üçüncü taraf (örneğin, Google, Twitter, Facebook veya Microsoft) kullanın. Bu konu aşağıdakileri kapsar:

- [Bir ASP.NET MVC uygulaması oluşturma](#createMvc) ve ASP.NET Identity özelliklerini keşfedin.
- [Kimliği örneği oluşturma](#build)
- [E-posta onayı ayarlama](#email)

Yeni kullanıcılar yerel bir hesap oluşturur, e-posta diğer kaydedin.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Kaydet düğmesi kendi e-posta adresine bir doğrulama belirteci içeren bir onay e-posta gönderir.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

Kullanıcı hesapları için bir onay belirteci ile bir e-posta gönderilir.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Bağlantı tıklatıldığında hesap onaylar.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Parola kurtarma/sıfırlama

Kendi Parolanızı unutmanız yerel kullanıcılar kendi e-posta hesabı için gönderilen bir güvenlik belirteci, parolasını sıfırlamak etkinleştirmeden olabilir.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Kullanıcı parolasını sıfırlamayı vermeden içeren bir bağlantı yakında bir e-posta alırsınız.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Bağlantı tıklatıldığında bunları sıfırlama sayfasına gideceksiniz.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Tıklatarak **sıfırlama** düğmesi, parola sıfırlama olmadığını onaylayın.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Bir ASP.NET Web uygulaması oluşturma

Başlangıç yüklenmesi ve çalıştırılması [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Visual Studio yükleme [2013 güncelleştirme 2](https://go.microsoft.com/fwlink/?LinkId=390521) ya da daha yüksek.

> [!NOTE]
> Uyarı: Visual Studio yüklemelisiniz [2013 güncelleştirme 2](https://go.microsoft.com/fwlink/?LinkId=390521) Bu öğreticiyi tamamlamak için.


1. Yeni bir ASP.NET Web projesi oluşturun ve MVC şablonunu seçin. Web Forms da destekler ASP.NET kimliği için bir web forms uygulamasında benzer adımları izleyebilirsiniz.
2. Varsayılan kimlik doğrulaması olarak bırakın **tek tek kullanıcı hesaplarını**.
3. Uygulamayı çalıştırın, tıklatın **kaydetmek** bağlantı ve bir kullanıcı kaydı. Bu noktada, yalnızca doğrulama e-posta ile olan [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) özniteliği.
4. Server Explorer'da gidin **veri Connections\DefaultConnection\Tables\AspNetUsers**sağ tıklayın ve seçin **açmak tablo tanımı**.

    Aşağıdaki resimde gösterildiği `AspNetUsers` şeması:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Sağ tıklayın **AspNetUsers** tablo ve seçin **Show Table Data**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   Bu noktada e-posta onaylanmamıştır.

ASP.NET kimliği için varsayılan veri deposu, Entity Framework olmakla birlikte, diğer veri depoları kullanmak üzere ve ek alanlar eklemek için yapılandırabilirsiniz. Bkz: [ek kaynaklar](#addRes) Bu öğreticinin sonunda bölüm.

[OWIN başlangıç sınıfı](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *haline* ) uygulama başlar ve çağırır adlı `ConfigureAuth` yönteminde *uygulama\_Start\Startup.Auth.cs*, OWIN ardışık düzenini yapılandırır ve ASP.NET Identity başlatır. İncelemek `ConfigureAuth` yöntemi. Her `CreatePerOwinContext` çağrısı bir geri çağırma kaydeder (kaydedilmiş `OwinContext`), çağrılır kez belirtilen türün bir örneğini oluşturmak için istek başına. Oluşturucuda bir kesme noktası ayarlayabilirsiniz ve `Create` her tür yöntemi (`ApplicationDbContext, ApplicationUserManager`) ve her istekte adlı bunlar doğrulayın. Bir örneğini `ApplicationDbContext` ve `ApplicationUserManager` uygulama genelinde erişilebilir OWIN bağlamında depolanır. ASP.NET kimliği için OWIN ardışık düzeni tanımlama bilgisi ara yazılım aracılığıyla içine kanca oluşturur. Daha fazla bilgi için bkz: [UserManager ASP.NET Identity sınıfında isteği ömür yönetimini başına](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Güvenlik profilinizi değiştirdiğinizde, yeni bir güvenlik damgası oluşturulur ve depolanır `SecurityStamp` alanını *AspNetUsers* tablo. Not `SecurityStamp` alan güvenlik tanımlama bilgisinden farklıdır. Güvenlik tanımlama bilgisi depolanmaz `AspNetUsers` tablosu (veya kimlik DB'de başka herhangi bir yerde). Güvenlik tanımlama bilgisi belirteci kullanarak kendinden imzalı [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) ve ile oluşturulan `UserId, SecurityStamp` ve sona erme zamanı bilgileri.

Tanımlama bilgisi Ara her istekte tanımlama bilgisi denetler. `SecurityStampValidator` Yönteminde `Startup` sınıfı DB İsabetli ve güvenlik damgasını düzenli olarak denetler ile belirtildiği gibi `validateInterval`. Güvenlik profilinizi değiştirmediğiniz sürece bu yalnızca her 30 dakikada bir (bizim örnek) gerçekleşir. 30 dakikalık zaman aralığı veritabanı gelişler en aza indirmek için seçildi. Bkz: my [iki öğeli kimlik doğrulama öğretici](index.md) daha fazla ayrıntı için.

Kod açıklamaları başına `UseCookieAuthentication` yöntemi tanımlama bilgisi kimlik doğrulamasını destekler. `SecurityStamp` Alan ve ilişkili kodu ek bir katman sağlar, parola değiştirdiğinizde uygulamanıza güvenliğinin, oturum açarken tarayıcı dışında günlüğe kaydedilir. `SecurityStampValidator.OnValidateIdentity` Yöntemi, parola değiştirdiğinizde veya dış oturum kullandığınızda kullanılan uygulama kullanıcı, oturum açtığında güvenlik belirteci doğrulamak etkinleştirir. Bu, eski parola ile oluşturulan herhangi bir belirtece (tanımlama bilgileri) geçersiz kılınır emin olmak için gereklidir. Örnek Proje değiştirirseniz kullanıcı için oluşturulan kullanıcıların parola sonra yeni bir belirteç, herhangi bir önceki belirtece geçersiz kılınır ve `SecurityStamp` alanı güncelleştirilir.

Kimlik sistemi izin Uygulamanızı yapılandırmak için bunu kullanıcılar güvenlik profili değiştiğinde (örneğin, oturum açma kullanıcı parolalarını veya değişiklikleri değiştiğinde ilişkili (Facebook, Google, örneğin Microsoft hesabı, vb.), kullanıcı dışında tüm günlüğe kaydedilir Tarayıcı örnekleri. Örneğin, görüntü gösterir aşağıda [çoklu oturum kapatma örnek](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) tüm tarayıcı örnekleri (Bu durumda, IE, Firefox ve Chrome) dışında bir düğmesine tıklayarak oturum olanak tanır. uygulama. Alternatif olarak, örnek, yalnızca belirli tarayıcı örneği dışında oturum olanak tanır.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[Çoklu oturum kapatma örnek](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) uygulama gösterir ASP.NET Identity nasıl güvenlik belirteci yeniden olanak tanır. Bu, eski parola ile oluşturulan herhangi bir belirtece (tanımlama bilgileri) geçersiz kılınır emin olmak için gereklidir. Bu özellik, fazladan bir uygulamanız için güvenlik katmanı sağlar; Parolanızı değiştirdiğinizde, bu uygulamaya burada kapattınız kaydedilir.

*Uygulama\_Start\IdentityConfig.cs* dosyasını içeren `ApplicationUserManager`, `EmailService` ve `SmsService` sınıfları. `EmailService` Ve `SmsService` her uygulama sınıfları `IIdentityMessageService` yaygın yöntemleri e-posta ve SMS yapılandırmak için her sınıfta olması için arabirim. Bu öğretici yalnızca e-posta bildirimi aracılığıyla ekleme gösterir, ancak [SendGrid](http://sendgrid.com/), SMTP ve diğer mekanizmalarını kullanarak e-posta gönderebilirsiniz.

`Startup` Sınıfı ayrıca bkz: my öğretici sosyal oturumları (Facebook, Twitter, vb.) eklemek için kazan kalıbı içerir [MVC 5 uygulamayla Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) daha fazla bilgi için.

İncelemek `ApplicationUserManager` kullanıcıların kimlik bilgilerini içeren ve aşağıdaki özellikleri yapılandırır sınıfı:

- Parola gücünü gereksinimleri.
- Kullanıcı kilitleme (denemeleri ve süresi).
- İki faktörlü kimlik doğrulamasını (2FA). Başka bir öğreticide 2FA ve SMS değineceğiz.
- E-posta ve SMS hizmetlerini takma. (I SMS başka bir öğreticide ele alacağız).

`ApplicationUserManager` Sınıfı türetilen genel `UserManager<ApplicationUser>` sınıfı. `ApplicationUser` türetilen [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` Genel türetilen `IdentityUser` sınıfı:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Yukarıdaki özellikleri özelliklerinde ile çakıştığı `AspNetUsers` yukarıda gösterilen tablo.

Genel bağımsız değişkenler üzerinde `IUser` farklı türleri için birincil anahtarı kullanarak bir sınıf türetin olanak sağlar. Bkz: [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) birincil anahtarı dizeden int veya GUID için nasıl değiştirileceğini gösteren örnek.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) tanımlanan *Models\IdentityModels.cs* olarak:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Yukarıdaki vurgulanmış kodu oluşturan bir [Claimsıdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET kimlik ve OWIN tanımlama bilgisi kimlik doğrulaması talep tabanlı, bu nedenle oluşturmak için uygulama çerçevesi gerektirir bir `ClaimsIdentity` kullanıcı için. `ClaimsIdentity` bilgileri gibi kullanıcının adını, kullanıcı için tüm talepler hakkında geçerlilik süresi ve kullanıcının hangi rollerin ait. Bu aşamada daha fazla kullanıcı talebini de ekleyebilirsiniz.

OWIN `AuthenticationManager.SignIn` yöntemi geçirir `ClaimsIdentity` ve kullanıcı oturum açtığında:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[MVC 5 uygulamayla Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) ek özellikler nasıl ekleyebileceğiniz gösterilmektedir `ApplicationUser` sınıfı.

## <a name="email-confirmation"></a>E-posta onayı

Bunlar değil kimliğine bürünmek başkası doğrulamak için yeni bir kullanıcı kayıt e-postayı onaylamak için iyi bir fikirdir (diğer bir deyişle, başka birinin e-posta ile kayıtlı olmayan). Bir tartışma Forumu vardı varsayalım önlemek isteyebilirsiniz `"bob@example.com"` olarak kaydetme gelen `"joe@contoso.com"`. E-posta onaysız `"joe@contoso.com"` istenmeyen e-posta uygulamanızdan alabilir. Bob yanlışlıkla kayıtlı varsayalım `"bib@example.com"` ve bunu fark çalıştırmadıysanız kendisine uygulama doğru e-postasını sahip olmadığından parola kurtarma kullanmak bağlanamayacak. E-posta onayı aracılarını, yalnızca sınırlı koruma sağlar ve koruma belirlendiği istenmeyen posta gönderenlerin sağlamaz, sahip oldukları çok sayıda çalışan e-posta diğer adlar kaydetmek için kullanabilirsiniz. Aşağıdaki örnekte, kullanıcı hesaplarında onaylanıp kadar parolasını değiştirmesi mümkün olmayacaktır (bunları tarafından bir onay bağlantıyı tıklatmak alınan kayıtlı ile e-posta hesabı.) Örneğin onaylayın ve profillerini ve benzeri değiştirdikten sonra kullanıcı bir e-posta gönderme Yöneticisi tarafından oluşturulan yeni hesapları parola sıfırlamak için bağlantı gönderme diğer senaryolar için bu iş akışı uygulayabilirsiniz. Genellikle, yeni kullanıcılar e-posta ile bir SMS metin iletisi ya da başka bir mekanizma onaylanmıştır önce web siteniz için herhangi bir veri gönderme önlemek isteyebilirsiniz. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>Daha eksiksiz bir örnek oluşturma

Bu bölümde, biz çalışıp çalışmayacağını daha eksiksiz bir örnek indirmek için NuGet kullanma.

1. Yeni bir ***boş*** ASP.NET Web projesi.
2. Paket Yöneticisi konsolunda aşağıdaki girin aşağıdaki komutlar: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   Bu öğreticide, kullanacağız [SendGrid](http://sendgrid.com/) e-posta göndermek için. `Identity.Samples` Çalışmalarımız ile kod paketi yükler.
3. Ayarlama [SSL kullanmak üzere proje](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Test yerel hesap oluşturma tıklayarak uygulamayı çalıştırarak **kaydetmek** bağlamak ve nakil kayıt formu.
5. E-posta onayı benzetimini yapar tanıtım e-posta bağlantısını tıklatın.
6. Tanıtım e-posta bağlantısı onay kodu örnekten kaldırın ( `ViewBag.Link` hesap denetleyicisi kodu. Bkz: `DisplayEmail` ve `ForgotPasswordConfirmation` eylem yöntemleri ve razor görünümleri).

> [!NOTE]
> Uyarı: Bu örnekte güvenlik ayarlarından herhangi birini değiştirirseniz, açıkça yapılan değişiklikleri çağıran bir güvenlik denetimi uygulanabilecek üretim uygulamaları gerekir.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Uygulama kodu incelemek\_Start\IdentityConfig.cs

Örnek bir hesap oluşturun ve ona ekleyen gösterilmektedir *yönetici* rol. Örnekteki e-posta için yönetici hesabı kullanarak e-posta ile değiştirmeniz gerekir. Şu anda bir yönetici hesabı oluşturmak için en kolay yolu programlı olarak kullanımda `Seed` yöntemi. Oluşturmanıza ve kullanıcıları ve rolleri yönetmenize olanak tanıyan bir araç gelecekte olmasını umuyoruz. Örnek kod oluşturma ve kullanıcıları ve rolleri yönetmenize izin vermez ancak ilk rol ve kullanıcı yönetici sayfaları çalıştırmak için Yöneticiler hesabınız olması gerekir. Bu örnekte, yönetici hesabı oluşturulduğunda DB sağlanmış.

Parolayı değiştirmek ve e-posta bildirimleri almanıza bir hesap adını değiştirin.

> [!WARNING]
> Güvenlik - hiçbir zaman deposu gizli verileri kaynak kodu.

Daha önce belirtildiği gibi `app.CreatePerOwinContext` başlangıç sınıfı çağrısında ekler için geri çağırmaları `Create` yöntemi sınıfların uygulama DB içeriği, Kullanıcı Yöneticisi ve Rol Yöneticisi. OWIN kanal çağrıları `Create` yöntemi bu sınıfların her istek için ve her sınıf bağlamının depolar. Kullanıcı Yöneticisi'nden (OWIN bağlamı içerir) HTTP bağlamı hesap denetleyicisi sunar:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Bir kullanıcı bir yerel hesap kaydettiğinde `HTTP Post Register` yöntemi çağrılır:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Yukarıdaki kod, e-posta ve girilen parolayı kullanarak yeni bir kullanıcı hesabı oluşturmak için model verileri kullanır. E-posta diğer veri deposunda ise, hesap oluşturma başarısız olur ve formu yeniden görüntülenir. `GenerateEmailConfirmationTokenAsync` Yöntemi güvenli onay belirteci oluşturur ve ASP.NET Identity veri deposunda saklar. [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) yöntemi oluşturur içeren bir bağlantı `UserId` ve onay belirteci. Bu bağlantıyı daha sonra kullanıcıya e-posta ile, kullanıcı hesabını onaylamak için kendi e-posta uygulaması bağlantıya tıklayabilirsiniz.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>E-posta onayı ayarlama

Git [Azure SendGrid kayıt sayfasını](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) ve ücretsiz hesap kaydedin. SendGrid yapılandırmak için aşağıdaki kodu ekleyin:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> E-posta istemcileri sık yalnızca metin iletisi (HTML yok) kabul edin. Metin ve HTML'dir iletisinde sağlamalıdır. Yukarıdaki SendGrid örnekte bu gerçekleştirilir `myMessage.Text` ve `myMessage.Html` yukarıda gösterilen kodu.


Aşağıdaki kodu kullanarak e-posta Gönder gösterilmektedir [posta iletisi](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) sınıf nereye `message.Body` yalnızca bağlantı döndürür.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Güvenlik - hiçbir zaman deposu gizli verileri kaynak kodu. Hesabı ve kimlik bilgileri appSetting depolanır. Azure üzerinde güvenli bir şekilde bu değerleri üzerinde depolayabileceğiniz **[yapılandırma](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portalında sekmesi. Bkz: [en iyi uygulamalar parolalar ve diğer hassas verileri ASP.NET ve Azure dağıtmak için](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


SendGrid kimlik bilgilerinizi girin, uygulamayı çalıştırın, e-posta diğer ad ile kayıt e-postanızdaki Onayla bağlantıyı tıklatabilirsiniz. Bunu yapmak nasıl görmek için [Outlook.com](http://outlook.com) e-posta hesabı, John Atten'ın bkz [Outlook.Com SMTP konak için SMTP yapılandırması C#](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) ve parolasını[ASP.NET Identity 2.0: Hesap doğrulama ayarı ve iki öğeli yetkilendirme](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) gönderir.

Bir kullanıcı, bir kez **kaydetmek** düğmesi doğrulama belirteci içeren bir onay e-posta, kullanıcıların e-posta adresine gönderilir.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

Kullanıcı hesapları için bir onay belirteci ile bir e-posta gönderilir.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Kodu inceleyin

Aşağıdaki kodda gösterildiği `POST ForgotPassword` yöntemi.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Kullanıcı e-posta doğrulanmamış yöntemi sessizce başarısız olur. Geçersiz e-posta adresi için bir hata oluşturursa, kötü niyetli kullanıcılar saldırmak için geçerli UserID (e-posta diğer adlar) bulmak için bu bilgileri kullanabilirsiniz.

Aşağıdaki kodda gösterildiği `ConfirmEmail` kullanıcı onlara gönderilen e-posta onayı bağlantıya tıkladığında çağrılan hesap denetleyicisi yönteminde:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Bir kez unutulmuş parola belirteci kullanıldıktan sonra geçersiz kılınır. Aşağıdaki kod değişikliği `Create` yöntemi (içinde *uygulama\_Start\IdentityConfig.cs* dosyası) 3 saat içinde süresi dolacak şekilde belirteçleri ayarlar.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Yukarıdaki kodu ile birlikte Unutulan parolayı ve e-posta onayı belirteçleri 3 saat içinde sona erecek. Varsayılan `TokenLifespan` bir gündür.

Aşağıdaki kod e-posta onayı yöntemini gösterir:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Uygulamanızı daha güvenli hale getirmek için ASP.NET Identity iki faktörlü kimlik doğrulamasını (2FA) destekler. Bkz: [ASP.NET Identity 2.0: Hesap doğrulama ve iki öğeli yetkilendirme ayarlama](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) John Atten tarafından. Oturum açma parola denemesi hatalarında hesap kilitleme belirlenebiliyor olsa da, bu yaklaşım, oturum açma getirir [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) kilitlemeleri. Hesap kilitleme yalnızca 2FA ile kullanmanızı öneririz.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Identity için Özel Depolama Sağlayıcılarına Genel Bakış](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [MVC 5 uygulamayla Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ayrıca kullanıcıların tabloya profil bilgilerini eklemeyi gösterir.
- [ASP.NET MVC ve kimlik 2.0: temel kavramları anlama](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) John Atten tarafından.
- [ASP.NET Identity’ye Giriş](../getting-started/introduction-to-aspnet-identity.md)
- [ASP.NET Identity 2.0.0 RTM Duyurusu](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) Pranav Rastogi tarafından.
