---
title: ASP.NET Core SMS ile iki öğeli kimlik doğrulaması
author: rick-anderson
description: Bir ASP.NET Core uygulama ile iki öğeli kimlik doğrulamasını (2FA) ayarlamak öğrenin.
manager: wpickett
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 20f00c2307e140d81e716304c96a143340d934d0
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>ASP.NET Core SMS ile iki öğeli kimlik doğrulaması

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [İsviçre Devs](https://github.com/Swiss-Devs)

Bkz: [ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma](xref:security/authentication/identity-enable-qrcodes) ASP.NET Core 2.0 ve daha sonra.

Bu öğretici, SMS kullanarak iki faktörlü kimlik doğrulamasını (2FA) ayarlamak gösterilmiştir. Yönergeler için verilen [twilio](https://www.twilio.com/) ve [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ancak herhangi bir SMS sağlayıcıyı kullanabilirsiniz. Tamamlamanız önerilir [hesap ve parola kurtarma](xref:security/authentication/accconfirm) bu öğreticiye başlamadan önce.

Görünüm [tamamlanan örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Karşıdan yükleme](xref:tutorials/index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Yeni bir ASP.NET Core projesi oluşturma

Adlı yeni bir ASP.NET Core web uygulaması oluşturma `Web2FA` bireysel kullanıcı hesapları ile. ' Ndaki yönergeleri izleyin [Zorla SSL bir ASP.NET Core uygulamasında](xref:security/enforcing-ssl) ayarlamak ve SSL gerektirmek için.

### <a name="create-an-sms-account"></a>Bir SMS hesabı oluşturma

Örneğin, bir SMS hesabı oluşturmak [twilio](https://www.twilio.com/) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Kimlik doğrulama bilgilerini kaydetmek (twilio için: accountSid ve ASPSMS için authToken: Userkey ve parola).

#### <a name="figuring-out-sms-provider-credentials"></a>SMS Sağlayıcısı kimlik bilgilerini açık

**Twilio:**  
Twilio hesabınızı Pano sekmesinden kopyalama **hesabının SID** ve **kimlik doğrulama belirteci**.

**ASPSMS:**  
Hesap ayarlarınızı, gitmek **Userkey** ve ile birlikte kopyalayın, **parola**.

Biz daha sonra bu değerler anahtarları içinde gizli Yöneticisi aracını oturum depolayacaktır `SMSAccountIdentification` ve `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Senderıd belirtme / gönderen

**Twilio:**  
Sayı sekmesinden, Twilio kopyalama **telefon numarası**. 

**ASPSMS:**  
Kilidini başlatıcılarını menüsünde içinde bir veya daha fazla başlatıcılarını kilidini veya bir alfasayısal başlatanın (tüm ağları tarafından desteklenmez) seçin. 

Bu değer anahtar içindeki gizli Yöneticisi aracıyla depolarız daha sonra `SMSAccountFrom`.


### <a name="provide-credentials-for-the-sms-service"></a>SMS hizmet için kimlik bilgilerini sağlayın

Kullanacağız [seçenekleri düzeni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için. 

   * Güvenli SMS anahtar getirmek için bir sınıf oluşturun. Bu örnek için `SMSoptions` sınıfı oluşturulur *Services/SMSoptions.cs* dosya.

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Ayarlama `SMSAccountIdentification`, `SMSAccountPassword` ve `SMSAccountFrom` ile [gizli Yöneticisi aracını](xref:security/app-secrets). Örneğin:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* NuGet paketi için SMS Sağlayıcısı ekleyin. Paket Yöneticisi Konsolu (çalıştırmak PMC gelen):

**Twilio:**  
`Install-Package Twilio`

**ASPSMS:**  
`Install-Package ASPSMS`


* Kod ekleme *Services/MessageServices.cs* SMS etkinleştirmek için dosya. Twilio veya ASPSMS bölüm kullanın:


**Twilio:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Başlangıç kullanmak için yapılandırma `SMSoptions`

Ekleme `SMSoptions` hizmet kapsayıcısında için `ConfigureServices` yönteminde *haline*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>İki faktörlü kimlik doğrulamasını etkinleştir

Açık *Views/Manage/Index.cshtml* Razor görünüm dosyası ve yorum karakterleri (out commnted biçimlendirme yok olacak şekilde) kaldırma.

## <a name="log-in-with-two-factor-authentication"></a>İki faktörlü kimlik bilgilerinizle oturum açın

* Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı

![Web uygulama kayıt görünümü Microsoft Edge'de Aç](2fa/_static/login2fa1.png)

* Etkinleştirir, kullanıcı adına dokunun `Index` Yönet denetleyicideki eylem yöntemi. Telefon numarası dokunun **Ekle** bağlantı.

![Görünüm yönetme](2fa/_static/login2fa2.png)

* Doğrulama kodunu alma ve dokunun telefon numarası ekleme **Gönder doğrulama kodu**.

![Telefon numarası Sayfası Ekle](2fa/_static/login2fa3.png)

* Doğrulama kodunu içeren bir kısa mesaj alırsınız. Girin ve dokunun **Gönder**

![Telefon numarası sayfasında doğrulayın](2fa/_static/login2fa4.png)

SMS mesajı alamazsanız twilio günlük sayfasına bakın.

* Telefon numaranız başarıyla eklendi yönet görünümü gösterir.

![Görünüm yönetme](2fa/_static/login2fa5.png)

* Dokunun **etkinleştirmek** iki faktörlü kimlik doğrulamasını etkinleştirmek için.

![Görünüm yönetme](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>İki faktörlü kimlik doğrulamasını Sına

* Oturumunuzu kapatın.

* Oturum aç.

* Böylece ikinci faktörlü kimlik doğrulaması sağlamak zorunda iki faktörlü kimlik doğrulaması, kullanıcı hesabı etkinleştirdi. Bu öğreticide, telefon doğrulama etkinleştirmiş olmanız gerekir. Yerleşik şablonlar, e-posta ikinci öğe olarak ayarlamanıza olanak sağlar. QR kodlarını gibi kimlik doğrulaması için ek ikinci Etkenler ayarlayabilirsiniz. Dokunun **gönderme**.

![Doğrulama kodu görünüm Gönder](2fa/_static/login2fa7.png)

* KISA mesajla aldığınız kodu girin.

* Tıklayarak **bu tarayıcı unutmayın** onay kutusunu muaf, aynı aygıt ve tarayıcı kullanarak oturum açmak için 2FA kullanmaya gerek. Etkinleştirme 2FA ve tıklayarak **bu tarayıcı unutmayın** aygıtınıza erişimi olmayan sürece, hesabınıza erişmeniz çalışılırken kötü niyetli kullanıcıların güçlü 2FA korumasıyla sağlayacaktır. Düzenli olarak kullanmak istediğiniz özel cihazda bunu yapabilirsiniz. Ayarlayarak **bu tarayıcı unutmayın**, düzenli olarak kullanmama aygıtlardan 2FA ek güvenlik alın ve 2FA kendi cihazlarda gitmek zorunda değil üzerinde kolaylık alın.

![Görünüm doğrulayın](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Deneme yanılma saldırılarına karşı korumak için hesap kilitleme

Hesap kilitleme ile 2FA önerilir. Bir kullanıcı bir yerel hesabı veya sosyal hesabı ile oturum açtıktan sonra her başarısız girişimden 2FA konumunda depolanır. En fazla başarısız erişim denemesi ulaşıldığında, kullanıcının kilitli (varsayılan: 5 erişim denemesi başarısız olduktan sonra 5 dakika kilitleme). Başarılı bir kimlik doğrulaması başarısız erişim denemesi sayısını sıfırlar ve saatini sıfırlar. En fazla başarısız erişim denemesi ve kilitleme süresini ayarlayabilirsiniz ile [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) ve [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). Aşağıdaki hesap kilitleme 10 erişim denemesi başarısız olduktan sonra 10 dakika için yapılandırır:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Onaylayın [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) ayarlar `lockoutOnFailure` için `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
