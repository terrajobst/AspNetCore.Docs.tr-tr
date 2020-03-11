---
title: ASP.NET Core SMS ile iki öğeli kimlik doğrulama
author: rick-anderson
description: İki öğeli kimlik doğrulamayı (2FA) ile ASP.NET Core uygulaması ayarlama konusunda bilgi edinin.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 424d21e446de02b39daa7685205a00931361b326
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661457"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>ASP.NET Core SMS ile iki öğeli kimlik doğrulama

[Rick Anderson](https://twitter.com/RickAndMSFT) ve [Isviçre-Devs](https://github.com/Swiss-Devs) tarafından

>[!WARNING]
> Kullanarak bir zamana bağlı kerelik parola algoritması (TOTP), iki öğeli kimlik doğrulamayı (2FA) kimlik doğrulayıcısı uygulamalarını önerilen yaklaşımı 2FA için sektöre var. 2fa'yı kullanarak TOTP SMS 2FA için tercih edilir. Daha fazla bilgi için bkz. ASP.NET Core 2,0 ve üzeri için [ASP.NET Core ' de TOTP Authenticator uygulamaları IÇIN QR kodu oluşturmayı etkinleştirme](xref:security/authentication/identity-enable-qrcodes) .

Bu öğreticide, SMS kullanarak iki öğeli kimlik doğrulamasını (2FA) ayarlama işlemi gösterilmektedir. Yönergeler [Twilio](https://www.twilio.com/) ve [aspsms](https://www.aspsms.com/asp.net/identity/core/testcredits/)için verilmiştir, ancak başka herhangi bir SMS sağlayıcısını kullanabilirsiniz. Bu Öğreticiyi başlatmadan önce [Hesap onaylama ve parola kurtarma](xref:security/authentication/accconfirm) işleminin tamamlanmasını öneririz.

[Örnek kodu görüntüleyin veya indirin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Nasıl indirileceği](xref:index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Yeni bir ASP.NET Core projesi oluşturma

Bireysel kullanıcı hesaplarıyla `Web2FA` adlı yeni bir ASP.NET Core Web uygulaması oluşturun. HTTPS 'yi ayarlamak ve istemek için <xref:security/enforcing-ssl> içindeki yönergeleri izleyin.

### <a name="create-an-sms-account"></a>SMS hesap oluşturma

Örneğin, [Twilio](https://www.twilio.com/) veya [aspsms](https://www.aspsms.com/asp.net/identity/core/testcredits/)adresinden bir SMS hesabı oluşturun. Kimlik doğrulama bilgileri kaydetmek (twilio'dan: accountSid ve ASPSMS için bir authToken: Userkey ve parola).

#### <a name="figuring-out-sms-provider-credentials"></a>SMS Sağlayıcısı kimlik bilgilerini başarınızda

**Twilio**

Twilio hesabınızın Pano sekmesinden **Hesap SID** 'Sini ve **kimlik doğrulama belirtecini**kopyalayın.

**ASPSMS:**

Hesap ayarlarınızda **userKey** ' e gidin ve **parolanızla**birlikte kopyalayın.

Bu değerleri daha sonra anahtarlar `SMSAccountIdentification` ve `SMSAccountPassword`içindeki gizli-Manager aracıyla depolayacağız.

#### <a name="specifying-senderid--originator"></a>Senderıd belirtme / Düzenleyicisi

**Twilio:** Sayılar sekmesinden Twilio **telefon numaranızı**kopyalayın.

**Aspsms:** Kilit açma/kaldırma menüsünde, bir veya daha fazla kaynaktan yararlanın veya alfasayısal bir kaynağı (tüm ağlar tarafından desteklenmez) seçin.

Bu değeri daha sonra anahtar `SMSAccountFrom`içinde gizli-Manager aracı ile depolayacağız.

### <a name="provide-credentials-for-the-sms-service"></a>SMS hizmet için kimlik bilgilerini sağlayın

Kullanıcı hesabına ve anahtar ayarlarına erişmek için [Seçenekler modelini](xref:fundamentals/configuration/options) kullanacağız.

* Güvenli SMS anahtarını getirmek için bir sınıf oluşturun. Bu örnekte, `SMSoptions` sınıfı *Services/SMSoptions. cs* dosyasında oluşturulur.

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

`SMSAccountIdentification`, `SMSAccountPassword` ve `SMSAccountFrom` [gizli-Manager aracı](xref:security/app-secrets)ile ayarlayın. Örnek:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* SMS Sağlayıcısı için NuGet paketini ekleyin. Paket Yöneticisi Konsolu (çalıştırma PMC'yi gelen):

**Twilio**

`Install-Package Twilio`

**ASPSMS:**

`Install-Package ASPSMS`

* SMS 'yi etkinleştirmek için *Services/MessageServices. cs* dosyasına kod ekleyin. Twilio veya ASPSMS bölüm kullanın:

**Twilio**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Başlangıç `SMSoptions` kullanılacak şekilde yapılandırma

*Startup.cs*yönteminde hizmet `ConfigureServices` kapsayıcısına `SMSoptions` ekleyin:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>İki öğeli kimlik doğrulamayı etkinleştirme

*Views/Manage/Index. cshtml* Razor görünüm dosyasını açın ve açıklama karakterlerini kaldırın (Bu nedenle biçimlendirme yok).

## <a name="log-in-with-two-factor-authentication"></a>İki öğeli kimlik bilgilerinizle oturum açın

* Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı

![Web uygulama kayıt görünümü Microsoft Edge'de Aç](2fa/_static/login2fa1.png)

* Kullanıcı adınızla, yönetim denetleyicisindeki `Index` eylem yöntemini etkinleştiren ' a dokunun. Ardından telefon numarası **Ekle** bağlantısına dokunun.

![Görünüm yönetme - "Ekle" bağlantısına dokunun](2fa/_static/login2fa2.png)

* Doğrulama kodunu alacak bir telefon numarası ekleyin ve **doğrulama kodu gönder**' e dokunun.

![Telefon numarası Sayfası Ekle](2fa/_static/login2fa3.png)

* Doğrulama kodunu içeren bir kısa mesaj alırsınız. Girin ve **Gönder** 'e dokunun

![Telefon numarası sayfanın doğrulayın](2fa/_static/login2fa4.png)

Kısa mesaj alamazsanız, twilio günlüğü sayfasında bakın.

* Telefon numaranızı başarıyla eklendi yönet görünümü gösterir.

![Telefon numarası başarıyla eklendi görünümü - yönetme](2fa/_static/login2fa5.png)

* İki öğeli kimlik doğrulamayı etkinleştirmek için **Etkinleştir** ' e dokunun.

![Görünüm yönetme - iki öğeli kimlik doğrulamayı etkinleştirme](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>İki öğeli kimlik doğrulamasını Sına

* Oturumunuzu kapatın.

* Oturum aç.

* Kullanıcı hesabı, iki öğeli kimlik doğrulama, ikinci faktör kimlik doğrulaması sağlamak zorunda etkinleştirdi. Bu öğreticide, telefon doğrulama etkinleştirmiş olmanız gerekir. Yerleşik şablonlar, e-posta ikinci öğe olarak ayarlamanıza olanak sağlar. QR kodları gibi kimlik doğrulaması için ek ikinci faktör ayarlayabilirsiniz. **Gönder**' e dokunun.

![Doğrulama kodu görünümü Gönder](2fa/_static/login2fa7.png)

* Mesajla aldığınız kodu girin.

* **Bu tarayıcıya** göz atma onay kutusunu tıklatmak, aynı cihaz ve tarayıcıyı kullanırken oturum açmak için 2FA kullanmaya gerek duymasını sağlar. 2FA 'yı etkinleştirmek ve **Bu tarayıcıyı unutmamak** , cihazınıza erişimi olmadığı sürece hesabınıza erişmeye çalışan kötü amaçlı kullanıcılardan güçlü 2FA koruması sağlar. Düzenli olarak kullandığınız özel cihaz üzerinde bunu yapabilirsiniz. **Bu tarayıcıyı aklınızda**bulundurarak, düzenli olarak kullanmayan CIHAZLARDAN 2FA 'nın ek güvenliğine sahip olursunuz ve kendi cihazlarınızda 2FA 'yı kullanmaya gerek kalmadan rahatlığını elde edersiniz.

![Görünüm doğrulayın](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Deneme yanılma saldırılarına karşı korumak için hesap kilitleme

Hesap kilitleme 2FA ile önerilir. Bir kullanıcı bir yerel hesap veya sosyal hesap ile oturum açtıktan sonra başarısız girişimleri 2FA'konumunda depolanır. En çok başarısız erişim denemesi ulaşıldığında, kullanıcının kilitli (varsayılan: 5 erişim denemesi başarısız olduktan sonra 5 dakika kilitleme). Başarılı bir kimlik doğrulaması başarısız erişim denemesi sayısını sıfırlar ve saatini sıfırlar. Başarısız olan en fazla erişim denemesi sayısı ve kilitleme süresi [Maxfailedaccessattempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) ve [Defaultlockouttimespan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan)ile ayarlanabilir. Aşağıdaki hesap kilitleme 10 erişim denemesi başarısız olduktan sonra 10 dakika için yapılandırır:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

[Passwordsignınasync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) kümesinin `true``lockoutOnFailure` olduğunu doğrulayın:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
