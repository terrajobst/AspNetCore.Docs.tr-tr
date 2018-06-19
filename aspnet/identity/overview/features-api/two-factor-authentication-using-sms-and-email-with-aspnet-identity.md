---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: SMS ve e-posta ile ASP.NET Identity kullanan iki öğeli kimlik doğrulaması | Microsoft Docs
author: HaoK
description: Bu öğretici SMS ve e-posta kullanarak iki faktörlü kimlik doğrulamasını (2FA) ayarlamak nasıl yapacağınızı gösterir. Bu makalede Rick Anderson tarafından yazılan ( @RickAndMSFT ), başına...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c8f628d177004a8569dde2651469ed591e48591e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876143"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>SMS ve e-posta ile ASP.NET Identity kullanan iki öğeli kimlik doğrulaması
====================
tarafından [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Bu öğretici SMS ve e-posta kullanarak iki faktörlü kimlik doğrulamasını (2FA) ayarlamak nasıl yapacağınızı gösterir.
> 
> Bu makalede Rick Anderson tarafından yazılan ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung ve Suhas Joshi. NuGet örnek öncelikle Hao Kung tarafından yazıldı.


Bu konu aşağıdakileri kapsar:

- [Kimliği örneği oluşturma](#build)
- [İki faktörlü kimlik doğrulaması için SMS ayarlayın](#SMS)
- [İki faktörlü kimlik doğrulamasını etkinleştir](#enable2)
- [İki öğeli kimlik doğrulama sağlayıcısını kaydetme](#reg)
- [Sosyal ve yerel oturum açma hesaplarını birleştirmek](#combine)
- [Deneme yanılma saldırılarına karşı hesap kilitleme](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Kimliği örneği oluşturma

Bu bölümde, biz ile çalışacak bir örnek indirmek için NuGet kullanma. Başlangıç yüklenmesi ve çalıştırılması [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Visual Studio yükleme [2013 güncelleştirme 2](https://go.microsoft.com/fwlink/?LinkId=390521) ya da daha yüksek.

> [!NOTE]
> Uyarı: Visual Studio yüklemelisiniz [2013 güncelleştirme 2](https://go.microsoft.com/fwlink/?LinkId=390521) Bu öğreticiyi tamamlamak için.


1. Yeni bir ***boş*** ASP.NET Web projesi.
2. Paket Yöneticisi konsolunda aşağıdaki girin aşağıdaki komutlar:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   Bu öğreticide, kullanacağız [SendGrid](http://sendgrid.com/) e-posta göndermek için ve [Twilio](https://www.twilio.com/) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) sms metin için. `Identity.Samples` Çalışmalarımız ile kod paketi yükler.
3. Ayarlama [SSL kullanmak üzere proje](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *İsteğe bağlı*:'ndaki yönergeleri izleyin my [e-posta onayı öğretici](account-confirmation-and-password-recovery-with-aspnet-identity.md) SendGrid kanca uygulamayı çalıştırın ve bir e-posta hesabını kaydetmek için.
5. * İsteğe bağlı: * tanıtım e-posta bağlantısı onay kodu örnekten kaldırın ( `ViewBag.Link` hesap denetleyicisi kodu. Bkz: `DisplayEmail` ve `ForgotPasswordConfirmation` eylem yöntemleri ve razor görünümleri).
6. <em>İsteğe bağlı: * kaldırmak `ViewBag.Status` kod yönetin ve hesap denetleyicileri ve *Views\Account\VerifyCode.cshtml</em> ve <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor görünümleri. Alternatif olarak, tutabilirsiniz `ViewBag.Status` bu uygulamayı yerel olarak takma ve e-posta ve SMS iletileri göndermek zorunda kalmadan nasıl çalıştığını test etmek için görüntüleme.

> [!NOTE]
> Uyarı: Bu örnekte güvenlik ayarlarından herhangi birini değiştirirseniz, açıkça yapılan değişiklikleri çağıran bir güvenlik denetimi uygulanabilecek üretim uygulamaları gerekir.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>İki faktörlü kimlik doğrulaması için SMS ayarlayın

Bu öğretici Twilio veya ASPSMS kullanma yönergeleri sağlar ancak herhangi bir SMS sağlayıcıyı kullanabilirsiniz.

1. **Bir SMS Sağlayıcısı ile bir kullanıcı hesabı oluşturma**  
  
   Oluşturma bir [Twilio](https://www.twilio.com/try-twilio) veya bir [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) hesabı.
2. **Ek paketler yükleme veya hizmet başvuruları ekleme**  
  
   Twilio:  
   Paket Yöneticisi konsolunda aşağıdaki komutu girin:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Aşağıdaki hizmet başvurusu eklenmesi gerekir:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Adresi:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Ad alanı:  
    `ASPSMSX2`
3. **SMS Sağlayıcısı kullanıcı kimlik bilgilerini açık**  
  
   Twilio:  
   Gelen **Pano** Twilio hesabınızı kopyalama sekmesinde **hesabının SID** ve **kimlik doğrulama belirteci**.  
  
   ASPSMS:  
   Hesap ayarlarınızı, gitmek **Userkey** ve, kendi kendine tanımlı birlikte kopyalayın **parola**.  
  
   Daha sonra bu değerler değişkenlerde depolarız `SMSAccountIdentification` ve `SMSAccountPassword` .
4. **Senderıd belirtme / gönderen**  
  
   Twilio:  
   Gelen **numaraları** sekmesinde, Twilio telefon numaranızı kopyalayın.  
  
   ASPSMS:  
   İçinde **kilidini başlatıcılarını** menüsünde, bir veya daha fazla başlatıcılarını kilidini veya bir alfasayısal başlatanın (tüm ağları tarafından desteklenmez) seçin.  
  
   Daha sonra bu değer değişken depolarız `SMSAccountFrom` .
5. **SMS Sağlayıcısı kimlik bilgileri bir uygulamaya aktarma**  
  
   Kimlik bilgileri ve gönderen telefon numarasını uygulama kullan:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Güvenlik - hiçbir zaman deposu gizli verileri kaynak kodu. Hesabı ve kimlik bilgileri örneği basit tutmak için yukarıdaki kod eklenir. Tan Atten'ın bkz [ASP.NET MVC: kaynak denetimi özel ayarları dışında tutmak](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Veri aktarımı için SMS sağlayıcı uygulaması**  
  
   Yapılandırma `SmsService` sınıfını *uygulama\_Start\IdentityConfig.cs* dosya.  
  
   Ya da bağlı olarak kullanılan SMS sağlayıcı etkinleştirme **Twilio** veya **ASPSMS** bölümü: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Uygulamayı çalıştırın ve daha önce kaydettiğiniz hesabıyla oturum açın.
8. Tıklatın etkinleştirir, kullanıcı kimliği üzerinde `Index` eylem yönteminde `Manage` denetleyicisi.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Ekle'yi tıklatın.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. Birkaç saniye içinde doğrulama kodunu içeren bir kısa mesaj alırsınız. Girip tuşuna **gönderme**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. Telefon numaranız eklendi yönet görünümü gösterir.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Kodu inceleyin

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index` Eylem yönteminde `Manage` denetleyici, önceki eylemine dayalı durum iletisi ayarlar ve yerel parolanızı değiştirmek veya yerel bir hesap eklemek için bağlantılar sağlar. `Index` Yöntemi de durumu görüntüler veya sizin 2FA telefon numarası, dış oturum açma bilgileri, etkin 2FA ve 2FA yöntemi (daha sonra açıklanmıştır) bu tarayıcı için unutmayın. Kullanıcı Kimliğinizi (e-posta) başlık çubuğunda tıklayarak bir ileti geçmiyor. Tıklayarak **telefon numarası: kaldırmak** bağlantı geçişleri `Message=RemovePhoneSuccess` bir sorgu dizesi olarak.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber` Eylem yöntemi SMS iletileri alabilen bir telefon numarası girmek için bir iletişim kutusu görüntüler.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Tıklayarak **Gönder doğrulama kodu** düğmesi yazılarını telefon numarası için HTTP POST `AddPhoneNumber` eylem yöntemi.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync` Yöntem kısa mesajla ayarlanan güvenlik belirteci oluşturur. SMS hizmet yapılandırdıysanız simge dize olarak gönderilir &quot;güvenlik kodunuz &lt;belirteci&gt;&quot;. `SmsService.SendAsync` Yöntemi için zaman uyumsuz olarak çağrılır ve ardından uygulamayı yönlendireceği `VerifyPhoneNumber` (aşağıdaki iletişim kutusunu görüntüler) eylem yöntemi girdiğiniz doğrulama kodu.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Kodu girin ve Gönder'i tıklatın ardından, kod HTTP POST nakledilir `VerifyPhoneNumber` eylem yöntemi.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync` Yöntemi gönderilen güvenlik kodunu denetler. Kodu doğru ise, telefon numarası eklenen `PhoneNumber` alanını `AspNetUsers` tablo. Bu çağrı başarılı olursa `SignInAsync` yöntemi çağrılır:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent` Parametresi, kimlik doğrulama oturumunun çoklu istekler arasında tutarlı olup olmadığını belirler.

Güvenlik profilinizi değiştirdiğinizde, yeni bir güvenlik damgası oluşturulur ve depolanır `SecurityStamp` alanını *AspNetUsers* tablo. Not `SecurityStamp` alan güvenlik tanımlama bilgisinden farklıdır. Güvenlik tanımlama bilgisi depolanmaz `AspNetUsers` tablosu (veya kimlik DB'de başka herhangi bir yerde). Güvenlik tanımlama bilgisi belirteci kullanarak kendinden imzalı [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) ve ile oluşturulan `UserId, SecurityStamp` ve sona erme zamanı bilgileri.

Tanımlama bilgisi Ara her istekte tanımlama bilgisi denetler. `SecurityStampValidator` Yönteminde `Startup` sınıfı DB İsabetli ve güvenlik damgasını düzenli olarak denetler ile belirtildiği gibi `validateInterval`. Güvenlik profilinizi değiştirmediğiniz sürece bu yalnızca her 30 dakikada bir (bizim örnek) gerçekleşir. 30 dakikalık zaman aralığı veritabanı gelişler en aza indirmek için seçildi.

`SignInAsync` Yöntemi güvenlik profili herhangi bir değişiklik yapıldığında çağrılması gerekir. Güvenlik profili değiştiğinde, güncelleştirmelerinin veritabanıdır `SecurityStamp` alan ve çağırmadan `SignInAsync` içinde oturum kalmak yöntemi *yalnızca* sonraki kadar OWIN ardışık düzenini veritabanı isabetler ( `validateInterval`). Bu değiştirerek test `SignInAsync` hemen döndürülecek yöntemi ve tanımlama bilgisi ayarı `validateInterval` 30 dakika özelliğinden 5 saniye:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Yukarıdaki kod değişikliklerle güvenlik profilinizi değiştirebilirsiniz (durumunu değiştirerek Örneğin, **iki Faktörlü etkin**) 5 saniye içinde kaydedilir zaman `SecurityStampValidator.OnValidateIdentity` yöntemi başarısız olur. Dönüş satırda kaldırmak `SignInAsync` yöntemi, başka bir güvenlik profili değişiklik yapabilir ve, günlüğe kaydedilmez. `SignInAsync` Yöntem yeni bir güvenlik tanımlama bilgisi oluşturur.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>İki faktörlü kimlik doğrulamasını etkinleştir

Örnek uygulamasında iki faktörlü kimlik doğrulamasını (2FA) etkinleştirmek için kullanıcı arabirimini kullanmanız gerekir. 2FA etkinleştirmek için gezinti çubuğunda kullanıcı kimliği (e-posta diğer ad) tıklayın.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Üzerinde 2FA Etkinleştir'i tıklatın.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Oturumu kapatın ve tekrar açın. E-posta etkinleştirdiyseniz (bkz my [önceki öğretici](account-confirmation-and-password-recovery-with-aspnet-identity.md)), SMS veya e-posta 2FA için seçebilirsiniz.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Kod (SMS veya e-posta) girebileceğiniz doğrulayın kod sayfası görüntülenir.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Tıklayarak **bu tarayıcı unutmayın** onay kutusunu muaf, o bilgisayar ve tarayıcı ile oturum açmak için 2FA kullanmaya gerek. Etkinleştirme 2FA ve tıklayarak **bu tarayıcı unutmayın** bilgisayarınıza erişimi olmayan sürece, hesabınıza erişmeniz çalışılırken kötü niyetli kullanıcıların güçlü 2FA korumasıyla sağlayacaktır. Düzenli olarak kullandığınız özel makine üzerinde bunu yapabilirsiniz. Ayarlayarak **bu tarayıcı unutmayın**, düzenli olarak kullanmama bilgisayarlardan 2FA ek güvenlik almak ve 2FA kendi bilgisayarlarda gitmek zorunda değil üzerinde kolaylık alın. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>İki öğeli kimlik doğrulama sağlayıcısını kaydetme

Yeni bir MVC projesi oluşturduğunuzda *IdentityConfig.cs* dosyası iki öğeli kimlik doğrulama sağlayıcısını kaydetmek için aşağıdaki kodu içerir:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>2FA için bir telefon numarası ekleme

`AddPhoneNumber` Eylem yönteminde `Manage` denetleyicisi bir güvenlik belirteci oluşturur ve bu telefon numarası gönderir olması koşuluyla.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Belirteç gönderdikten sonra onu yönlendirir `VerifyPhoneNumber` eylem yöntemi, burada girebilirsiniz 2FA için SMS kaydetmek için kod. Telefon numarası olana kadar SMS 2FA kullanılmaz.

## <a name="enabling-2fa"></a>2FA etkinleştirme

`EnableTFA` Eylem yöntemi 2FA sağlar:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Not `SignInAsync` etkinleştir 2FA güvenlik profili için bir değişiklik olduğundan çağrılmalıdır. 2FA etkinleştirildiğinde, kullanıcı oturum açma 2FA kullanın (SMS ve e-posta örnekteki) kayıtlı 2FA yaklaşımlar kullanılarak gerekir.

QR kodu oluşturucuları gibi daha fazla 2FA sağlayıcıları ekleyebilir veya sahip olduğunuz yazabilirsiniz (bkz [kullanarak Google Authenticator ile ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> 2FA kodları kullanılarak oluşturulan [zamana dayalı kerelik parola algoritması](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) ve kodları altı dakika için geçerlidir. Altı kodu girmek için dakikadan uzun sürerse geçersiz kod hata iletisi alırsınız.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Sosyal ve yerel oturum açma hesaplarını birleştirmek

Yerel ve sosyal hesapları, e-posta bağlantısını tıklatarak birleştirebilirsiniz. Aşağıdaki sırayla &quot; RickAndMSFT@gmail.com &quot; yerel bir oturum olarak oluşturulur, ancak ilk sosyal günlüğüne olarak hesabı oluşturun, sonra yerel oturum açma ekleyin.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Tıklayın **Yönet** bağlantı. Bu hesapla ilişkilendirilen 0 dış (sosyal oturum açma bilgileri) unutmayın.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Hizmet başka bir oturum açmak için bağlantıya tıklayın ve uygulama isteklerini kabul edin. İki hesap birlikte, her iki hesabı ile oturum açabilecek olacaktır. Kullanıcının sosyal oturum açtığında kimlik doğrulama hizmeti çalışmıyor ya da daha büyük bir olasılıkla bunlar erişim sosyal hesaplarında kesilmiş durumda yerel hesapları eklemek için kullanıcılarınızın isteyebilirsiniz.

Aşağıdaki görüntüde, sosyal günlüğüne zel olduğu (gelen görebileceğiniz **dış oturum açma: 1** sayfasında gösterilen).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Tıklayarak **parola çekme** yerel günlüğe eklemek aynı hesabı ile ilişkili sağlar.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Deneme yanılma saldırılarına karşı hesap kilitleme

Hesapları, kullanıcı kilitlemesinin etkinleştirerek uygulamanızdan sözlük saldırılarına karşı koruyabilirsiniz. Aşağıdaki kod `ApplicationUserManager Create` yöntemi kilitlenme yapılandırır:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Yukarıdaki kod kilitleme için yalnızca iki faktörlü kimlik doğrulamasını etkinleştirir. Oturum açma kilitlenme değiştirerek etkinleştirebilirsiniz sırada `shouldLockout` true `Login` yöntemi hesap denetleyicisinin öneririz, etkinleştirmemeniz kilitlenme oturum açma hesabı açıktır. yaptığından [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) oturum açma saldırıları. Oluşturduğunuz yönetici hesabı için örnek kodda kilitleme devre dışı `ApplicationDbInitializer Seed` yöntemi:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Doğrulanmış e-posta hesabına sahip bir kullanıcının gerektiren

Aşağıdaki kod, bir kullanıcı oturum önce bir doğrulanmış e-posta hesabınız gerektirir:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Nasıl SignInManager 2FA gereksinimini denetler

Hem yerel oturum açma ve sosyal günlüğüne 2FA etkin olup olmadığını denetleyin. 2FA etkinleştirilirse, `SignInManager` oturum açma yöntemini döndürür `SignInStatus.RequiresVerification`, ve kullanıcı yönlendirilecek `SendCode` sahip oldukları sırada oturum tamamlamak için kodu girmek için eylem yöntemi. Kullanıcılar yerel tanımlama bilgisinde, kullanıcının RememberMe varsa ayarlanır `SignInManager` döndürülecek `SignInStatus.Success` ve 2FA Git gerekmez.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Aşağıdaki kodda gösterildiği `SendCode` eylem yöntemi. A [Selectlistıtem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) kullanıcı için etkinleştirilmiş tüm 2FA yöntemleriyle oluşturulur. [Selectlistıtem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) geçirilir [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) 2FA yaklaşım (genellikle e-posta ve SMS) seçmesini sağlayan Yardımcısı.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Kullanıcı 2FA yaklaşım yazılarını sonra `HTTP POST SendCode` eylem yöntemi çağrıldıktan `SignInManager` 2FA kod ve kullanıcı için yönlendirildiği gönderir `VerifyCode` eylem yönteminin nerede bunlar oturum açmanızı tamamlamak için kodunu girin.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA kilitleme

Oturum açma parola denemesi hatalarında hesap kilitleme belirlenebiliyor olsa da, bu yaklaşım, oturum açma getirir [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) kilitlemeleri. Hesap kilitleme yalnızca 2FA ile kullanmanızı öneririz. Zaman `ApplicationUserManager` olan oluşturulan örnek kodu 2FA kilitleme ayarlar ve `MaxFailedAccessAttemptsBeforeLockout` beş. Bir kullanıcı (yerel hesap veya sosyal hesap aracılığıyla), oturum sonra her başarısız girişimden 2FA konumunda depolanır ve en fazla deneme ulaşıldığında, kullanıcı beş dakika kilitlenmeden (süresiyle kilitleme ayarlayabilirsiniz `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Identity önerilen kaynakları](../getting-started/aspnet-identity-recommended-resources.md) tam listesi kimlik bloglar, videoları, eğitim ve mükemmel şekilde bağlar.
- [MVC 5 uygulamayla Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ayrıca kullanıcıların tabloya profil bilgilerini eklemeyi gösterir.
- [ASP.NET MVC ve kimlik 2.0: temel kavramları anlama](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) John Atten tarafından.
- [Hesap onaylamayı ve parola kurtarma ASP.NET kimliğe sahip](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Identity’ye Giriş](../getting-started/introduction-to-aspnet-identity.md)
- [ASP.NET Identity 2.0.0 RTM Duyurusu](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) Pranav Rastogi tarafından.
- [ASP.NET Identity 2.0: Hesap doğrulama ve iki öğeli yetkilendirme ayarlama](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) John Atten tarafından.
