---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: SMS ve e-posta ile ASP.NET Identity kullanılarak iki öğeli kimlik doğrulama | Microsoft Docs
author: HaoK
description: Bu öğreticide SMS ve e-posta iki öğeli kimlik doğrulamasını (2FA) ayarlama yapmayı gösterir. Bu makale Rick Anderson tarafından yazılmış ( @RickAndMSFT ), formülü...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4454f011960f74c20ac590c3d034028cfc867e7d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753834"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>SMS ve e-posta ile ASP.NET Identity kullanılarak iki öğeli kimlik doğrulama
====================
tarafından [Hao Kung](https://github.com/HaoK), [Pranav Rastogi'nin](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Bu öğreticide SMS ve e-posta iki öğeli kimlik doğrulamasını (2FA) ayarlama yapmayı gösterir.
> 
> Bu makale Rick Anderson tarafından yazılmış ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi'nin ([@rustd](https://twitter.com/rustd)), Hao Kung ve Suhas Joshi. NuGet örnek öncelikle Hao Kung tarafından yazılmıştır.


Bu konu başlığı altında aşağıdakileri içerir:

- [Kimliği örneği oluşturma](#build)
- [SMS ' için iki öğeli kimlik doğrulama ayarlayın](#SMS)
- [İki öğeli kimlik doğrulamayı etkinleştirme](#enable2)
- [İki öğeli kimlik doğrulama sağlayıcısını kaydetme](#reg)
- [Sosyal ve yerel oturum açma hesabını birleştirin](#combine)
- [Deneme yanılma saldırılarına karşı hesap kilitleme](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Kimliği örneği oluşturma

Bu bölümde, bir örnek ile çalışacağız indirmek için NuGet kullanacaksınız. Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Visual Studio yükleme [2013 güncelleştirmesi 2](https://go.microsoft.com/fwlink/?LinkId=390521) veya üzeri.

> [!NOTE]
> Uyarı: Visual Studio yüklemeniz gerekir [2013 güncelleştirmesi 2](https://go.microsoft.com/fwlink/?LinkId=390521) Bu öğreticiyi tamamlamak için.


1. Yeni bir ***boş*** ASP.NET Web projesi.
2. Paket Yöneticisi Konsolu'nda aşağıdaki girin aşağıdaki komutları:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   Bu öğreticide, kullanacağız [SendGrid](http://sendgrid.com/) e-posta göndermek için ve [Twilio](https://www.twilio.com/) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) sms metin için. `Identity.Samples` Çalışmalarımız ile kod paketi yükler.
3. Ayarlama [SSL kullanmak üzere proje](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *İsteğe bağlı*: yönergeleri my [e-posta onayı öğretici](account-confirmation-and-password-recovery-with-aspnet-identity.md) SendGrid kanca uygulamayı çalıştırın ve bir e-posta hesabını kaydetmek için.
5. * İsteğe bağlı: * örnekten tanıtım e-posta bağlantısı onay kodu Kaldır ( `ViewBag.Link` kodunda hesap denetleyicisi. Bkz: `DisplayEmail` ve `ForgotPasswordConfirmation` eylem yöntemleri ve razor görünümleri).
6. <em>İsteğe bağlı: * kaldırmak `ViewBag.Status` kod yönetin ve hesap denetleyicileri ve *Views\Account\VerifyCode.cshtml</em> ve <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor görünümleri. Alternatif olarak, tutabilirsiniz `ViewBag.Status` bağlama ve e-posta ve SMS mesajları göndermek üzere yerel olarak zorunda kalmadan bu uygulamanın nasıl çalıştığını test etmek için görüntüleme.

> [!NOTE]
> Uyarı: Bu örnekte güvenlik ayarlarından herhangi birini değiştirirseniz, üretim uygulamaları yapılan değişiklikleri açıkça çağıran bir güvenlik denetimi geçmeleri gerekir.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>SMS ' için iki öğeli kimlik doğrulama ayarlayın

Bu öğretici, Twilio ya da ASPSMS kullanımıyla ilgili yönergeleri sağlar, ancak herhangi bir SMS Sağlayıcısı kullanabilirsiniz.

1. **Bir SMS Sağlayıcısı ile bir kullanıcı hesabı oluşturma**  
  
   Oluşturma bir [Twilio](https://www.twilio.com/try-twilio) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) hesabı.
2. **Ek paketler yüklenirken veya hizmet başvuruları ekleme**  
  
   Twilio:  
   Paket Yöneticisi konsolunda aşağıdaki komutu girin:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Aşağıdaki servis başvurusu eklenmesi gerekir:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Adresi:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Ad alanı:  
    `ASPSMSX2`
3. **SMS Sağlayıcısı kullanıcı kimlik bilgilerini başarınızda**  
  
   Twilio:  
   Gelen **Pano** sekmesini Twilio hesabınızın kopyalama **hesap SID'si** ve **kimlik doğrulama belirteci**.  
  
   ASPSMS:  
   Hesap ayarlarınıza gidin **Userkey** kopyalayıp, şirket içinde tanımlanan birlikte **parola**.  
  
   Daha sonra bu değerleri değişkenlerinde depolarız `SMSAccountIdentification` ve `SMSAccountPassword` .
4. **Senderıd belirtme / Düzenleyicisi**  
  
   Twilio:  
   Gelen **numaraları** sekmesinde, Twilio telefon numaranızı kopyalayın.  
  
   ASPSMS:  
   İçinde **kilidini düzenleyicileri** menüsünde, bir veya daha fazla düzenleyicileri kilidini veya bir alfasayısal gönderen (tüm ağlar tarafından desteklenmez) seçin.  
  
   Daha sonra bu değer bir değişkende depolarız `SMSAccountFrom` .
5. **SMS Sağlayıcısı kimlik bilgilerinin uygulamaya aktarma**  
  
   Gönderen telefon numarası ve kimlik bilgileri uygulama için kullanılabilir yapın:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki. Örneği basit tutmak için yukarıdaki kod, kimlik bilgilerini ve hesabı eklenir. Jon Atten'ın bkz [ASP.NET MVC: kaynak denetimi özel ayarları dışı tutmak](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Veri aktarımı için SMS Sağlayıcısı uygulama**  
  
   Yapılandırma `SmsService` sınıfını *uygulama\_Start\IdentityConfig.cs* dosya.  
  
   Ya da bağlı olarak kullanılan SMS Sağlayıcısı etkinleştirme **Twilio** veya **ASPSMS** bölümü: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Uygulamayı çalıştırın ve daha önce kaydettiğiniz bir hesapla oturum açın.
8. Tıklayın etkinleştirir, kullanıcı kimliği üzerinde `Index` eylem yönteminde `Manage` denetleyicisi.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Ekle'ye tıklayın.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. Birkaç saniye içinde doğrulama kodunu içeren bir kısa mesaj alırsınız. Girip tuşuna **Gönder**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. Telefon numaranız eklendi Yönet görüntüler.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Kod İnceleme

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index` Eylem yönteminde `Manage` denetleyici, önceki eylemlere göre durum iletisi ayarlar ve yerel parolanızı değiştirmeniz veya yerel bir hesap eklemek için bağlantılar sağlar. `Index` Yöntemi de durumu görüntüler veya, 2fa'yı telefon numarası, dış oturum açma bilgileri, 2fa'yı etkin ve devre dışı 2FA yöntemi (daha sonra açıklanmıştır) bu tarayıcıda unutmayın. Kullanıcı Kimliğinizi (e-posta) başlık çubuğunda tıklayarak bir ileti geçmiyor. Tıklayarak **telefon numarası: kaldırma** bağlantı geçişleri `Message=RemovePhoneSuccess` olarak bir sorgu dizesi.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber` Eylem yöntemi, SMS mesajları alıp bir telefon numarası girmek için bir iletişim kutusu görüntüler.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Tıklayarak **doğrulama kodu Gönder** düğmesi gönderileri telefon numarası için HTTP POST `AddPhoneNumber` eylem yöntemi.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync` Yöntem mesajla ayarlanan güvenlik belirteci oluşturur. SMS hizmet yapılandırılmışsa simge dize olarak gönderilir &quot;güvenlik kodunuz &lt;belirteci&gt;&quot;. `SmsService.SendAsync` Yöntemi için zaman uyumsuz olarak çağrılır, ardından uygulamayı yönlendireceği `VerifyPhoneNumber` (aşağıdaki iletişim kutusunu görüntüler) eylem yöntemine doğrulama kodu girebildiğiniz.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Kodu girin ve Gönder'e tıklayın sonra kod için HTTP POST gönderilen `VerifyPhoneNumber` eylem yöntemi.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync` Yöntemi gönderilen güvenlik koduyla denetler. Kodu doğru ise, telefon numarası için eklenen `PhoneNumber` alanını `AspNetUsers` tablo. Bu çağrı başarılı olursa `SignInAsync` yöntemi çağrılır:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent` Parametresi, kimlik doğrulama oturumunun çoklu istekler arasında tutarlı olup olmayacağını belirler.

Güvenlik profilinizi değiştirdiğinizde, yeni bir güvenlik damgası oluşturulur ve depolanır `SecurityStamp` alanını *AspNetUsers* tablo. Not `SecurityStamp` alandır güvenlik tanımlama bilgisinden farklı. Güvenlik tanımlama bilgisi depolanmaz `AspNetUsers` tabloyu (veya kimlik DB'de başka bir yerde). Güvenlik tanımlama bilgisi belirteci kullanarak kendinden imzalı [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) ve ile oluşturulan `UserId, SecurityStamp` ve sona erme saati bilgileri.

Tanımlama bilgisi Ara her istekte tanımlama bilgisi denetler. `SecurityStampValidator` Yönteminde `Startup` sınıfı DB denk gelir ve güvenlik damgasını düzenli aralıklarla denetleyen ile belirtilen `validateInterval`. Bu yalnızca güvenlik profilinizi değiştirmediğiniz sürece her 30 dakikada bir (örneğimizi) gerçekleşir. 30 dakikalık aralık gelişlerin veritabanına en aza indirmek için seçilmiştir.

`SignInAsync` Yöntemi güvenlik profiline herhangi bir değişiklik yapıldığında çağrılması gerekir. Güvenlik profil değiştiğinde, güncelleştirmeleri veritabanıdır `SecurityStamp` alanındaki ve çağırmadan `SignInAsync` yöntemi, oturum açmış kalın *yalnızca* değiştirene kadar OWIN ardışık düzenini veritabanı İsabetleri ( `validateInterval`). Bunu değiştirerek test `SignInAsync` hemen getirilecek yöntemi ve tanımlama bilgisi ayarları `validateInterval` 30 dakika özelliğine 5 saniye:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Yukarıdaki kod değişikliği olmadan güvenlik profilinizi değiştirebilirsiniz (örneğin durumunu değiştirmeyi tarafından **iki Etmeninin etkinleştirilip**) ve 5 saniye içinde kapatılacak olduğunda `SecurityStampValidator.OnValidateIdentity` yöntemi başarısız. Dönüş satırda kaldırmak `SignInAsync` yöntemi, başka bir güvenlik profili değişiklik yapmak ve, kapatılacak değil. `SignInAsync` Yöntem, yeni güvenlik tanımlama bilgisi oluşturur.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>İki öğeli kimlik doğrulamayı etkinleştirme

Örnek uygulamada, iki öğeli kimlik doğrulamayı (2FA) etkinleştirmek için kullanıcı arabirimini kullanmanız gerekir. 2fa'yı etkinleştirmek için gezinti çubuğunda kullanıcı Kimliğinizi (e-posta diğer adı) tıklayın.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Üzerinde etkinleştirme 2fa'yı tıklayın.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Oturumu kapatın ve yeniden oturum açın. E-posta etkinleştirdiyseniz (bkz my [önceki öğreticide](account-confirmation-and-password-recovery-with-aspnet-identity.md)), SMS veya e-posta için 2fa'yı seçin.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Kod (SMS veya e-posta) girebileceğiniz doğrulayın kod sayfası görüntülenir.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Tıklayarak **bu tarayıcı Hatırlansın** onay kutusunu muaf, bu bilgisayar ve tarayıcı ile oturum açmak için 2fa'yı kullanmaya gerek. 2fa'yı etkinleştirme ve tıklayarak **bu tarayıcı Hatırlansın** bilgisayarınıza erişiminiz yoksa sürece, hesabınıza erişmeye çalışırken kötü niyetli kullanıcıların güçlü 2FA koruma sağlayacaktır. Düzenli olarak kullandığınız özel makine üzerinde bunu yapabilirsiniz. Ayarlayarak **bu tarayıcı Hatırlansın**, düzenli olarak kullanmadığınız bilgisayarlardan 2FA için ek güvenlik alma ve 2FA kendi bilgisayarlarda gitmek zorunda değil üzerinde kolaylık alın. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>İki öğeli kimlik doğrulama sağlayıcısını kaydetme

Yeni bir MVC projesi oluşturduğunuzda *IdentityConfig.cs* dosya iki öğeli kimlik doğrulama sağlayıcısını kaydetmek için aşağıdaki kodu içerir:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>2FA için'bir telefon numarası ekleyin

`AddPhoneNumber` Eylem yönteminde `Manage` denetleyicisi bir güvenlik belirteci oluşturur ve gönderir, telefon numarası olması koşuluyla.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Belirteç gönderdikten sonra onu yeniden yönlendirir `VerifyPhoneNumber` 2FA için SMS kaydetmek için kod girebildiğiniz, eylem yöntemi. Telefon numarası doğrulayana kadar SMS 2fa'yı kullanılmaz.

## <a name="enabling-2fa"></a>2fa'yı etkinleştirme

`EnableTFA` Eylem yöntemine 2FA sağlar:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Not `SignInAsync` etkinleştir 2FA güvenlik profilini bir değişiklik olduğundan çağrılmalıdır. 2fa'yı etkin olduğunda, kullanıcı (SMS ve e-posta örneği'nde) kayıtlı 2FA yaklaşımları kullanarak oturum açma konusundaki 2fa'yı kullanmak gerekir.

QR kod oluşturucuları gibi daha fazla 2FA sağlayıcıları ekleyebilirsiniz veya sahip olduğunuz yazabilirsiniz (bkz [ASP.NET Identity ile Google Authenticator'ı kullanarak](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> 2fa'yı kodları kullanılarak oluşturulan [zamana bağlı bir kerelik parola algoritması](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) ve kodları altı dakika için geçerlidir. Kodu girmek için birden fazla altı dakika sürer, geçersiz kod hata iletisi alırsınız.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Sosyal ve yerel oturum açma hesabını birleştirin

Yerel hem de sosyal hesaplar, e-posta bağlantısına tıklayarak birleştirebilirsiniz. Aşağıdaki sırayla &quot; RickAndMSFT@gmail.com &quot; yerel oturum açma oluşturulur, ancak bir sosyal günlük ilk olarak hesabı oluşturun, ardından bir yerel oturum açma ekleyin.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Tıklayarak **Yönet** bağlantı. Bu hesapla ilişkili 0 dış (sosyal oturum açma bilgileri) unutmayın.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Hizmet başka bir oturum açmak için bağlantıya tıklayın ve uygulama istekleri kabul edin. İki hesap birleştirilmiş, herhangi bir hesabı ile oturum açamayacaktır. Kullanıcılarınız, sosyal kimlik doğrulama hizmeti günlüğü kullanılamıyor veya bunlar sosyal hesaplarına erişim daha büyük bir olasılıkla kaybetmiş durumunda yerel hesapları eklemek isteyebilirsiniz.

Aşağıdaki resimde, bir sosyal oturum açma Tom olduğu (görebileceğiniz gibi **harici oturum açmaları: 1** sayfasında gösterilen).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Tıklayarak **bir parola seçin** , yerel bir günlük eklemek aynı hesabı ile ilişkili sağlar.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Deneme yanılma saldırılarına karşı hesap kilitleme

Hesapları, kullanıcı kilitlemesinin etkinleştirerek uygulamanızdan sözlük saldırılarına karşı koruyabilirsiniz. Aşağıdaki kod içinde `ApplicationUserManager Create` yöntemi kilitleme yapılandırır:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Yukarıdaki kod, yalnızca iki faktörlü kimlik doğrulaması için kilitleme sağlar. Oturum açma bilgileri için kilidi değiştirerek etkinleştirebilirsiniz ancak `shouldLockout` true olarak `Login` yöntemi hesabı denetleyicinin öneririz, etkinleştirmemeniz kilitleme oturum açma hesabı maruz kalabilir yaptığından [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) oturum açma saldırıları. Yönetici hesabı oluşturduğunuz için örnek kodda kilitleme devre dışı bırakıldı `ApplicationDbInitializer Seed` yöntemi:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Bir kullanıcı doğrulanmış bir e-posta hesabına sahip olmasını gerektiren

Aşağıdaki kod bir kullanıcı oturum önce bir doğrulanmış e-posta hesabına sahip olmasını gerektirir:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Nasıl SignInManager 2FA gereksinimini denetler

Hem yerel oturum açma ve sosyal oturum açma 2FA etkin olup olmadığını denetleyin. 2fa'yı etkinleştirilirse, `SignInManager` oturum açma yöntemini döndürür `SignInStatus.RequiresVerification`, ve kullanıcı yönlendirilecek `SendCode` eylem yöntemi, burada Yöneticiler gerekir oturum sırasını tamamlamak için kodu girin. Kullanıcılar yerel tanımlama bilgisinde, kullanıcının RememberMe varsa ayarlanır `SignInManager` döndüreceği `SignInStatus.Success` ve Git 2FA gerekmez.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Aşağıdaki kodda gösterildiği `SendCode` eylem yöntemi. A [Selectlistıtem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) kullanıcı için etkinleştirilmiş tüm 2FA yöntemlerle oluşturulur. [Selectlistıtem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) geçirilir [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) Yardımcısı, 2FA yaklaşım (genellikle e-posta ve SMS) seçmesini sağlar.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

2FA yaklaşım kullanıcı yazılarını sonra `HTTP POST SendCode` eylem yöntemi çağrıldığında `SignInManager` gönderir 2FA kodunu ve kullanıcı yeniden yönlendirilmiş için `VerifyCode` eylem yönteminin nerede oldukları oturum açmanızı tamamlamak için kodunu girin.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2fa'yı kilitleme

Hesap kilitleme oturum açma parola denemesi hatalarında ayarlayabilirsiniz olsa da, bu yaklaşım, oturum açma bilgilerinizi getirir [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) kilitlemeleri uygulayın. Hesap kilitleme yalnızca 2FA ile kullanmanızı öneririz. Zaman `ApplicationUserManager` olan oluşturulan örnek kodu 2FA kilitleme ayarlar ve `MaxFailedAccessAttemptsBeforeLockout` beş. (Yerel hesap veya sosyal hesap) bir kullanıcının oturum açması, 2FA başarısız her teşebbüs depolanır ve en fazla denemesi ulaşıldığında, kullanıcı beş dakika boyunca kilitlenip sonra (zaman kilitleme ayarlayabilirsiniz `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Identity önerilen kaynaklar](../getting-started/aspnet-identity-recommended-resources.md) tam listesi kimlik blogları, videoları, öğreticileri ve mükemmel şekilde bağlar.
- [Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma ile MVC 5 uygulaması](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ayrıca kullanıcıların tablosuna profil bilgilerini ekleme işlemini gösterir.
- [ASP.NET MVC ve kimlik 2.0: temellerini anlama](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) John Atten tarafından.
- [Hesap onaylama ve parola kurtarma ASP.NET Identity ile](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Identity’ye Giriş](../getting-started/introduction-to-aspnet-identity.md)
- [ASP.NET kimlik 2.0.0 RTM Duyurusu](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) Pranav rastogi'nin.
- [ASP.NET Identity 2.0: Hesap doğrulama ve iki Faktörlü yetkilendirmeyi ayarlama](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) John Atten tarafından.
