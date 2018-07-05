---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Hesap onaylama ve parola kurtarma ASP.NET Identity (C#) ile | Microsoft Docs
author: HaoK
description: Önce tamamlamanız gereken Bu öğreticiyi gerçekleştirmeden önce oturum açma, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturun. Bu öğreticide...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: ''
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 38b908d145986102ff1b1734cdcef75b320bd0ab
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384137"
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Hesap onaylama ve parola kurtarma ile ASP.NET Identity (C#)
====================
tarafından [Hao Kung](https://github.com/HaoK), [Pranav Rastogi'nin](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Önce tamamlamanız gereken Bu öğreticiyi tamamlamadan önce [oturum açma, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Bu öğretici, daha fazla ayrıntı içerir ve e-posta için yerel hesap onaylama ve ASP.NET ıdentity'de Unutulan parolalarını sıfırlamasına izin yapmayı gösterir. Bu makale Rick Anderson tarafından yazılmış ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi'nin ([@rustd](https://twitter.com/rustd)), Hao Kung ve Suhas Joshi. NuGet örnek öncelikle Hao Kung tarafından yazılmıştır.


Kullanıcı hesabı için bir parola oluşturmak bir yerel kullanıcı hesabı gerektirir ve bu parola web uygulamasında (güvenli) depolanır. ASP.NET Identity, kullanıcının uygulama için bir parola oluşturması gerektirmeyen sosyal medya hesaplarını da destekler. [Sosyal medya hesaplarını](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) kullanıcıların kimliğini doğrulamak için bir üçüncü taraf (örneğin, Google, Twitter, Facebook veya Microsoft) kullanın. Bu konu başlığı altında aşağıdakileri içerir:

- [Bir ASP.NET MVC uygulaması oluşturma](#createMvc) ve ASP.NET Identity özelliklerini keşfedin.
- [Kimliği örneği oluşturma](#build)
- [E-posta onayı ayarlama](#email)

Yeni kullanıcılar yerel bir hesap oluşturur, e-posta diğer kaydedin.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Kaydet düğmesine tıklayarak bir e-posta adresi doğrulama belirteci içeren bir onay e-posta gönderir.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

Kullanıcı, kendi hesabı için bir onay belirteci ile bir e-posta gönderilir.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Bağlantıya tıklamak, hesap onaylar.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Parola kurtarma/sıfırlama

Kendi parolanızı unutursanız yerel kullanıcılar, bunları kullanıcının parolasını sıfırlamak etkinleştirme, e-posta hesabına gönderilen bir güvenlik belirteci olabilir.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Kullanıcı, yakında kullanıcının parolasını sıfırlamak için bir bağlantı ile bir e-posta alırsınız.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Bağlantıya tıklayarak bunları sıfırlama sayfasına gideceksiniz.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Tıklayarak **sıfırlama** düğmesi, parola sıfırlama olmadığını onaylayın.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Bir ASP.NET Web uygulaması oluşturma

Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Visual Studio yükleme [2013 güncelleştirmesi 2](https://go.microsoft.com/fwlink/?LinkId=390521) veya üzeri.

> [!NOTE]
> Uyarı: Visual Studio yüklemeniz gerekir [2013 güncelleştirmesi 2](https://go.microsoft.com/fwlink/?LinkId=390521) Bu öğreticiyi tamamlamak için.


1. Yeni ASP.NET Web projesi oluşturun ve MVC şablonu seçin. Web Forms da destekler ASP.NET Identity şekilde bir web forms uygulaması benzer adımları izleyebilirsiniz.
2. Varsayılan kimlik doğrulaması olarak bırakın **bireysel kullanıcı hesapları**.
3. Uygulamayı çalıştırın, tıklayın **kaydetme** bağlamak ve bir kullanıcı kaydı. Yalnızca e-posta doğrulamasını bu noktada, olan [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) özniteliği.
4. Sunucu Gezgini'nde gidin **veri Connections\DefaultConnection\Tables\AspNetUsers**sağ tıklayın ve seçin **tablo tanımını açın**.

    Aşağıdaki görüntüde `AspNetUsers` şema:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Sağ tıklayın **AspNetUsers** tablosunu seçip **tablo verilerini Göster**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   Bu noktada e-posta onaylanmamıştır.

ASP.NET kimliği için varsayılan veri deposu, Entity Framework olmakla birlikte, ek alanlar eklenecek ve diğer veri depoları kullanmak üzere yapılandırabilirsiniz. Bkz: [ek kaynaklar](#addRes) Bu öğreticinin sonunda bölüm.

[OWIN başlangıç sınıfı](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) uygulama başladığında ve çağırır olduğunda Aranan `ConfigureAuth` yönteminde *uygulama\_Start\Startup.Auth.cs*, OWIN ardışık düzenini yapılandırır ve ASP.NET Identity başlatır. İnceleme `ConfigureAuth` yöntemi. Her `CreatePerOwinContext` çağrısı, bir geri çağırma kaydeder (kayıtlı `OwinContext`), çağrılacağı kez belirtilen türün bir örneğini oluşturmak için istek başına. Oluşturucu içinde bir kesme noktası ayarlayabilirsiniz ve `Create` yöntemi her türün (`ApplicationDbContext, ApplicationUserManager`) ve her istekte çağırılır bunlar doğrulayın. Bir örneğini `ApplicationDbContext` ve `ApplicationUserManager` uygulamanın tamamında erişilebilir OWIN bağlamında depolanır. OWIN ardışık düzenini tanımlama bilgisi ara yazılım aracılığıyla içine ASP.NET Identity kancaları. Daha fazla bilgi için [UserManager ASP.NET Identity sınıfında ömür yönetimini istek başına](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Güvenlik profilinizi değiştirdiğinizde, yeni bir güvenlik damgası oluşturulur ve depolanır `SecurityStamp` alanını *AspNetUsers* tablo. Not `SecurityStamp` alandır güvenlik tanımlama bilgisinden farklı. Güvenlik tanımlama bilgisi depolanmaz `AspNetUsers` tabloyu (veya kimlik DB'de başka bir yerde). Güvenlik tanımlama bilgisi belirteci kullanarak kendinden imzalı [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) ve ile oluşturulan `UserId, SecurityStamp` ve sona erme saati bilgileri.

Tanımlama bilgisi Ara her istekte tanımlama bilgisi denetler. `SecurityStampValidator` Yönteminde `Startup` sınıfı DB denk gelir ve güvenlik damgasını düzenli aralıklarla denetleyen ile belirtilen `validateInterval`. Bu yalnızca güvenlik profilinizi değiştirmediğiniz sürece her 30 dakikada bir (örneğimizi) gerçekleşir. 30 dakikalık aralık gelişlerin veritabanına en aza indirmek için seçilmiştir. Bkz: benim [iki öğeli kimlik doğrulama öğreticisini](index.md) daha fazla ayrıntı için.

Kod açıklamaları başına `UseCookieAuthentication` yöntemi tanımlama bilgisi kimlik doğrulamasını destekler. `SecurityStamp` Alan ve ilişkili kod ek bir koruma katmanı sağlar, parola değiştirdiğinizde, uygulamanıza güvenlik, oturumunuz ile tarayıcı dışında günlüğe kaydedilir. `SecurityStampValidator.OnValidateIdentity` , Parola değiştirdiğinizde veya dış oturum kullandığınızda kullanılan uygulama kullanıcı oturum açtığında güvenlik belirteci doğrulamak yöntem sağlar. Bu, eski parola ile oluşturulan tüm belirteçleri (tanımlama) geçersiz emin olmak için gereklidir. Örnek Proje değiştirirseniz kullanıcı için kullanıcı parolalarının sonra yeni bir belirteç oluşturulur, önceki tarafından istenen belirteçleri geçersiz kılınır ve `SecurityStamp` alan güncelleştirilir.

Kimlik sistemi izin Uygulamanızı yapılandırmak için bunu kullanıcılar güvenlik profil değiştiğinde (örneğin, kullanıcı parolalarını veya değişiklikleri değiştiğinde giriş ilişkili (Facebook, Google'nın gelenler gibi Microsoft hesabı, vb.), tüm kullanıcı oturum Tarayıcı örnekleri. Örneğin, görüntüyü aşağıda gösterildiği [çoklu oturum kapatma örnek](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) tüm tarayıcı örnekleri (Bu durumda, IE, Firefox ve Chrome) dışında bir düğmeye tıklayarak oturum açmasını sağlayan uygulama. Alternatif olarak, örnek bir belirli bir tarayıcı örneğinde dışında yalnızca oturum sağlar.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[Çoklu oturum kapatma örnek](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) uygulama gösterir nasıl ASP.NET Identity güvenlik belirteci yeniden sağlar. Bu, eski parola ile oluşturulan tüm belirteçleri (tanımlama) geçersiz emin olmak için gereklidir. Bu özellik, güvenlik uygulamanıza ek bir koruma katmanı sağlar. Parolanızı değiştirdiğinizde, bu uygulamaya burada kapattınız kaydedilir.

*Uygulama\_Start\IdentityConfig.cs* dosyasını içeren `ApplicationUserManager`, `EmailService` ve `SmsService` sınıfları. `EmailService` Ve `SmsService` her uygulama sınıfları `IIdentityMessageService` arabirim için e-posta ve SMS yapılandırmak için her sınıfta ortak yöntemler vardır. Bu öğreticide yalnızca e-posta bildirimi aracılığıyla ekleme gösterir, ancak [SendGrid](http://sendgrid.com/), SMTP ve başka mekanizmalar kullanılarak bir e-posta gönderebilirsiniz.

`Startup` Sınıfı ayrıca içerir (Facebook, Twitter, vb.), sosyal oturum açma bilgilerini görmek my öğretici eklemek için kazan blondan [Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma ile MVC 5 uygulaması](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) daha fazla bilgi için.

İnceleme `ApplicationUserManager` sınıfı kullanıcıların kimlik bilgilerini içeren ve aşağıdaki özellikleri yapılandırır:

- Parola gücü gereksinimleri.
- Kullanıcı kilitleme (denemeleri ve süresi).
- İki öğeli kimlik doğrulamayı (2FA). Başka bir öğreticinin 2FA ve SMS değineceğiz.
- E-posta ve SMS hizmetlerini takma. (Ben SMS başka bir öğreticinin ele alacağız).

`ApplicationUserManager` Sınıfı türetilen genel `UserManager<ApplicationUser>` sınıfı. `ApplicationUser` öğesinden türetilen [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` Genel türetilen `IdentityUser` sınıfı:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Yukarıdaki özellikleri Özellikler ile çakıştığı `AspNetUsers` yukarıda gösterilen tablo.

Genel bağımsız değişkenler üzerinde `IUser` farklı türleri için birincil anahtarı kullanarak bir sınıf türetin olanak sağlar. Bkz: [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) int veya GUID dizeden birincil anahtarı değiştirmek nasıl gösteren bir örnek.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) tanımlanan *Models\IdentityModels.cs* olarak:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Yukarıdaki vurgulanan kod oluşturur bir [Claimsıdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity ve OWIN tanımlama bilgisi kimlik doğrulaması talep tabanlı, bu nedenle framework oluşturulacak uygulamayı gerektirir bir `ClaimsIdentity` kullanıcı. `ClaimsIdentity` bilgileri gibi kullanıcının adını, kullanıcı için tüm talepleri hakkında yaş ve kullanıcının hangi rolleri ait. Bu aşamada kullanıcı için daha fazla talep de ekleyebilirsiniz.

OWIN `AuthenticationManager.SignIn` yöntemi de geçer `ClaimsIdentity` ve kullanıcı oturum açtığında:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma ile MVC 5 uygulaması](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) ek özelliklerini nasıl ekleyebileceğinizi gösterir `ApplicationUser` sınıfı.

## <a name="email-confirmation"></a>E-posta onayı

Bunlar değil kimliğe bürünerek başka birisi doğrulamak için ile yeni bir kullanıcı kaydı e-postayı onaylamak için iyi bir fikirdir (diğer bir deyişle, bunlar başka birinin e-posta ile kayıtlı olmayabilirsiniz). Tartışma Forumu tablonuz olduğunu varsayın engellemek istiyorsunuz `"bob@example.com"` olarak kaydetme gelen `"joe@contoso.com"`. E-posta onayı olmadan `"joe@contoso.com"` uygulamanızdan istenmeyen e-posta alabilir. Bob yanlışlıkla kayıtlı varsayalım `"bib@example.com"` ve bunu fark yüklediniz o uygulamayı doğru e-postasını olmadığı için parola kurtarma kullanın saptayamazdınız. E-posta onayı robotlar yalnızca sınırlı koruma sağlar ve belirlenen istenmeyen posta gönderenlere koruma sağlamaz, sahip oldukları çok sayıda çalışan e-posta diğer adlar kaydetmek için kullanabilirsiniz. Aşağıdaki örnekte, kullanıcı hesabını onaylanana kadar parola değiştirmesi mümkün olmayacaktır (bunları tarafından alınan bir onay bağlantısına tıklayarak kayıtlı ile e-posta hesabı.) Diğer senaryolarda, örneğin onaylamak ve profillerini vb. değiştirdikten sonra kullanıcı bir e-posta gönderme Yöneticisi tarafından oluşturulan yeni hesapları parola sıfırlama için bağlantı gönderme, bu iş akışı uygulayabilirsiniz. Genellikle, yeni kullanıcıların e-posta, SMS mesajı ya da başka bir mekanizma onaylanmıştır önce web sitenizi herhangi bir veri gönderme engellemek istiyorsunuz. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>Daha eksiksiz bir örnek oluşturma

Bu bölümde, NuGet ile çalışacağız daha eksiksiz bir örnek yüklemek için kullanacaksınız.

1. Yeni bir ***boş*** ASP.NET Web projesi.
2. Paket Yöneticisi Konsolu'nda aşağıdaki girin aşağıdaki komutları: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   Bu öğreticide, kullanacağız [SendGrid](http://sendgrid.com/) e-posta göndermek için. `Identity.Samples` Çalışmalarımız ile kod paketi yükler.
3. Ayarlama [SSL kullanmak üzere proje](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Yerel hesap oluşturma tıklayarak uygulamayı çalıştırarak test **kaydetme** bağlantısını ve kayıt form gönderme.
5. E-posta onayı benzetim tanıtım e-posta bağlantısına tıklayın.
6. Tanıtım e-posta bağlantısı onay kodu örnekten Kaldır ( `ViewBag.Link` kodunda hesap denetleyicisi. Bkz: `DisplayEmail` ve `ForgotPasswordConfirmation` eylem yöntemleri ve razor görünümleri).

> [!NOTE]
> Uyarı: Bu örnekte güvenlik ayarlarından herhangi birini değiştirirseniz, üretim uygulamaları yapılan değişiklikleri açıkça çağıran bir güvenlik denetimi geçmeleri gerekir.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Uygulama kodu inceleyin\_Start\IdentityConfig.cs

Örnek bir hesap oluşturun ve ona ekleme işlemi gösterilmektedir *yönetici* rol. Örnek e-postada, yönetici hesabı kullanarak e-posta ile değiştirmeniz gerekir. Şu anda bir yönetici hesabı oluşturmak için en kolay yolu program `Seed` yöntemi. Gelecekte oluşturmak ve kullanıcıları ve rolleri yönetmek sağlayacak bir araç olmasını umuyoruz. Örnek kod oluşturma ve kullanıcıları ve rolleri yönetme izin vermez ancak öncelikle rol ve kullanıcı yönetici sayfaları çalıştırmak için Yöneticiler hesabınız olması gerekir. Bu örnekte, çekirdek değeri oluşturulmuş bir veritabanı yönetici hesabı oluşturulur.

Parola değiştirme ve e-posta bildirimlerini nereden alacağınızı bir hesap için adı değiştirin.

> [!WARNING]
> Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki.

Daha önce de belirtildiği `app.CreatePerOwinContext` başlangıç sınıfı çağrısında ekler geri çağırmalar için `Create` yöntemi sınıfların DB uygulama içeriği, Kullanıcı Yöneticisi ve Rol Yöneticisi. OWIN ardışık düzen çağrıları `Create` yöntemi bu sınıfların her istek için ve her sınıf için bağlam depolar. Hesap denetleyicisi (OWIN bağlamını içeren) HTTP bağlamdan Kullanıcı Yöneticisi'ni kullanıma sunar:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Yerel hesap olarak bir kullanıcı kayıt olurkenki `HTTP Post Register` yöntemi çağrılır:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Yukarıdaki kod, e-posta ve girilen parolayı kullanarak yeni bir kullanıcı hesabı oluşturmak için model verileri kullanır. E-posta diğer veri deposunda ise, hesap oluşturma başarısız olur ve formu yeniden görüntülenir. `GenerateEmailConfirmationTokenAsync` Yöntemi güvenli onay belirteci oluşturur ve ASP.NET Identity veri deposunda saklar. [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) yöntemi oluşturur bağlantısı içeren `UserId` ve onay simgesi. Bu bağlantının ardından kullanıcıya e-posta ile, kullanıcı hesabını onaylamak için kendi e-posta uygulaması bağlantıya tıklayabilirsiniz.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>E-posta onayı ayarlama

Git [Azure SendGrid kayıt sayfasını](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) ve ücretsiz hesap kaydedin. SendGrid yapılandırmak için aşağıdaki kodu ekleyin:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> E-posta istemcileri genellikle yalnızca metin iletileri (HTML yok) kabul edin. Metin ve HTML iletisinde sağlamanız gerekir. SendGrid yukarıdaki örnekte, bu ile yapılır `myMessage.Text` ve `myMessage.Html` yukarıda gösterilen kodu.


Aşağıdaki kodu kullanarak e-posta göndermek nasıl gösterir [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) sınıfı nerede `message.Body` yalnızca bağlantıyı döndürür.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki. Kimlik ve hesap appSetting içinde depolanır. Azure üzerinde güvenli bir şekilde bu değerleri üzerinde depolamanın **[yapılandırma](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portalında sekmesi. Bkz: [parolalar ve diğer hassas verileri ASP.NET ve Azure'a dağıtmak için en iyi yöntemler](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


SendGrid kimlik bilgilerinizi girin, uygulamayı çalıştırın, bir e-posta takma ad ile kayıt e-postanıza onaylayın bağlantısına tıklayabilirsiniz. İle bunun nasıl yapılacağını görmek için [Outlook.com](http://outlook.com) hesabı e-posta, John Atten'ın bkz [Outlook.Com SMTP konağı için SMTP yapılandırması C#](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) ve parolasını[ASP.NET Identity 2.0: Hesap doğrulama ayarı ve iki Faktörlü yetkilendirmeyi](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) gönderir.

Kullanıcı bir kez **kaydetme** düğmesi için kendi e-posta adresi doğrulama belirteci içeren bir onay e-posta gönderilir.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

Kullanıcı, kendi hesabı için bir onay belirteci ile bir e-posta gönderilir.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Kod İnceleme

Aşağıdaki kodda gösterildiği `POST ForgotPassword` yöntemi.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Kullanıcı e-posta onaylanmamıştır yöntemi sessizce başarısız olur. Bir hata için bir geçersiz e-posta adresi gönderildi, kötü amaçlı kullanıcıların saldırmak için geçerli kullanıcı (e-posta takma adlardır) bulmak için bu bilgileri kullanabilirsiniz.

Aşağıdaki kodda gösterildiği `ConfirmEmail` kullanıcı kendilerine gönderilen e-posta onayı bağlantıya tıkladığında çağrılır hesap denetleyicideki yöntemi:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Unutulan parolayı belirteci kullanıldıktan sonra geçersiz kılınır. Aşağıdaki kod değişikliği `Create` yöntemi (içinde *uygulama\_Start\IdentityConfig.cs* dosya) 3 saat içinde süresi dolacak şekilde belirteçleri ayarlar.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Yukarıdaki kod ile Unutulan parolayı ve e-posta onayı belirteçleri 3 saat içinde dolacak. Varsayılan `TokenLifespan` bir gündür.

Aşağıdaki kod, e-posta onayı yöntemi gösterir:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Uygulamanızı daha güvenli hale getirmek için iki öğeli kimlik doğrulamayı (2FA) ASP.NET Identity destekler. Bkz: [ASP.NET Identity 2.0: Hesap doğrulama ve iki Faktörlü yetkilendirmeyi ayarlama](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) John Atten tarafından. Hesap kilitleme oturum açma parola denemesi hatalarında ayarlayabilirsiniz olsa da, bu yaklaşım, oturum açma bilgilerinizi getirir [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) kilitlemeleri uygulayın. Hesap kilitleme yalnızca 2FA ile kullanmanızı öneririz.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Identity için Özel Depolama Sağlayıcılarına Genel Bakış](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma ile MVC 5 uygulaması](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ayrıca kullanıcıların tablosuna profil bilgilerini ekleme işlemini gösterir.
- [ASP.NET MVC ve kimlik 2.0: temellerini anlama](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) John Atten tarafından.
- [ASP.NET Identity’ye Giriş](../getting-started/introduction-to-aspnet-identity.md)
- [ASP.NET kimlik 2.0.0 RTM Duyurusu](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) Pranav rastogi'nin.
