---
title: ASP.NET Core SMS ile iki öğeli kimlik doğrulama
author: rick-anderson
description: İki öğeli kimlik doğrulamayı (2FA) ile ASP.NET Core uygulaması ayarlama konusunda bilgi edinin.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 6f20928b0dec9b235fa17c1b44c81a48d031e9e0
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121667"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>ASP.NET Core SMS ile iki öğeli kimlik doğrulama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [İsviçre geliştiriciler](https://github.com/Swiss-Devs)

>[!WARNING]
> Kullanarak bir zamana bağlı kerelik parola algoritması (TOTP), iki öğeli kimlik doğrulamayı (2FA) kimlik doğrulayıcısı uygulamalarını önerilen yaklaşımı 2FA için sektöre var. 2fa'yı kullanarak TOTP SMS 2FA için tercih edilir. Daha fazla bilgi için [ASP.NET core'da TOTP authenticator uygulamaları için etkinleştirme QR kodu oluşturmayı](xref:security/authentication/identity-enable-qrcodes) ASP.NET Core 2.0 ve üzeri.

Bu öğreticide, SMS kullanarak iki öğeli kimlik doğrulamasını (2FA) ayarlama işlemi gösterilmektedir. Yönergeler için verilir [twilio](https://www.twilio.com/) ve [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ancak herhangi bir SMS Sağlayıcısı kullanabilirsiniz. Tamamlamanız önerilir [hesap onaylama ve parola kurtarma](xref:security/authentication/accconfirm) bu öğreticiye başlamadan önce.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Karşıdan yükleme](xref:index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Yeni bir ASP.NET Core projesi oluşturma

Adlı yeni bir ASP.NET Core web uygulaması oluşturma `Web2FA` bireysel kullanıcı hesapları ile. Bölümündeki yönergeleri [SSL'yi zorunlu bir ASP.NET Core uygulaması](xref:security/enforcing-ssl) ayarlamak ve SSL gerektirmek için.

### <a name="create-an-sms-account"></a>SMS hesap oluşturma

Örneğin, bir SMS hesap oluşturma [twilio](https://www.twilio.com/) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Kimlik doğrulama bilgileri kaydetmek (twilio'dan: accountSid ve ASPSMS için bir authToken: Userkey ve parola).

#### <a name="figuring-out-sms-provider-credentials"></a>SMS Sağlayıcısı kimlik bilgilerini başarınızda

**Twilio:** Twilio hesabınızın Pano sekmesinden kopyalama **hesap SID'si** ve **kimlik doğrulama belirteci**.

**ASPSMS:** , hesap ayarlarınıza gidin **Userkey** ve birlikte kopyalayın, **parola**.

Daha sonra bu değerleri gizli dizi Yöneticisi Aracı anahtarları içinde oturum depolarız `SMSAccountIdentification` ve `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Senderıd belirtme / Düzenleyicisi

**Twilio:** numaraları sekmesinden, Twilio kopyalama **telefon numarası**.

**ASPSMS:** kilidini düzenleyicileri menüsünde içinde bir veya daha fazla düzenleyicileri kilidini veya bir alfasayısal gönderen (tüm ağlar tarafından desteklenmez) seçin.

Gizli dizi Yöneticisi Aracı anahtarı içinde bu değer daha sonra depolarız `SMSAccountFrom`.


### <a name="provide-credentials-for-the-sms-service"></a>SMS hizmet için kimlik bilgilerini sağlayın

Kullanacağız [seçenekleri deseni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için.

   * Güvenli SMS anahtarını getirmek için bir sınıf oluşturun. Bu örnek için `SMSoptions` sınıf oluşturulduğu *Services/SMSoptions.cs* dosya.

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Ayarlama `SMSAccountIdentification`, `SMSAccountPassword` ve `SMSAccountFrom` ile [gizli dizi Yöneticisi aracını](xref:security/app-secrets). Örneğin:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* SMS Sağlayıcısı için NuGet paketini ekleyin. Paket Yöneticisi Konsolu (çalıştırma PMC'yi gelen):

**Twilio:**
`Install-Package Twilio`

**ASPSMS:**
`Install-Package ASPSMS`


* Kodda *Services/MessageServices.cs* SMS etkinleştirmek için dosya. Twilio veya ASPSMS bölüm kullanın:


**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Başlangıç kullanmak için yapılandırma `SMSoptions`

Ekleme `SMSoptions` hizmet kapsayıcısını `ConfigureServices` yönteminde *Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>İki öğeli kimlik doğrulamayı etkinleştirme

Açık *Views/Manage/Index.cshtml* Razor görünüm dosyası ve yorum karakterleri (hiçbir işaretleme bırakmayın olduğundan) kaldırın.

## <a name="log-in-with-two-factor-authentication"></a>İki öğeli kimlik bilgilerinizle oturum açın

* Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı

![Web uygulama kayıt görünümü Microsoft Edge'de Aç](2fa/_static/login2fa1.png)

* Etkinleştirir, kullanıcı adına dokunun `Index` Yönet denetleyicideki eylem yöntemi. Telefon numarası'e dokunun **Ekle** bağlantı.

![Görünüm yönetme - "Ekle" bağlantısına dokunun](2fa/_static/login2fa2.png)

* Doğrulama kodu almak ve dokunun telefon numarası ekleme **doğrulama kodu Gönder**.

![Telefon numarası Sayfası Ekle](2fa/_static/login2fa3.png)

* Doğrulama kodunu içeren bir kısa mesaj alırsınız. Girin ve dokunun **Gönder**

![Telefon numarası sayfanın doğrulayın](2fa/_static/login2fa4.png)

Kısa mesaj alamazsanız, twilio günlüğü sayfasında bakın.

* Telefon numaranızı başarıyla eklendi yönet görünümü gösterir.

![Telefon numarası başarıyla eklendi görünümü - yönetme](2fa/_static/login2fa5.png)

* Dokunun **etkinleştirme** iki öğeli kimlik doğrulamasını etkinleştirmek için.

![Görünüm yönetme - iki öğeli kimlik doğrulamayı etkinleştirme](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>İki öğeli kimlik doğrulamasını Sına

* Oturumunuzu kapatın.

* Oturum aç.

* Kullanıcı hesabı, iki öğeli kimlik doğrulama, ikinci faktör kimlik doğrulaması sağlamak zorunda etkinleştirdi. Bu öğreticide, telefon doğrulama etkinleştirmiş olmanız gerekir. Yerleşik şablonlar, e-posta ikinci öğe olarak ayarlamanıza olanak sağlar. QR kodları gibi kimlik doğrulaması için ek ikinci faktör ayarlayabilirsiniz. Dokunun **gönderme**.

![Doğrulama kodu görünümü Gönder](2fa/_static/login2fa7.png)

* Mesajla aldığınız kodu girin.

* Tıklayarak **bu tarayıcı Hatırlansın** onay kutusunu muaf, 2FA aynı cihaz ve tarayıcı kullanarak oturum açmak için kullandığınız gerek. 2fa'yı etkinleştirme ve tıklayarak **bu tarayıcı Hatırlansın** cihazınıza erişiminiz yoksa sürece, hesabınıza erişmeye çalışırken kötü niyetli kullanıcıların güçlü 2FA koruma sağlayacaktır. Düzenli olarak kullandığınız özel cihaz üzerinde bunu yapabilirsiniz. Ayarlayarak **bu tarayıcı Hatırlansın**, düzenli olarak kullanmadığınız cihazlardan 2FA eklenen güvenliğini size ve kendi cihazlarında 2FA gitmek zorunda değil üzerinde kolaylık alın.

![Görünüm doğrulayın](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Deneme yanılma saldırılarına karşı korumak için hesap kilitleme

Hesap kilitleme 2FA ile önerilir. Bir kullanıcı bir yerel hesap veya sosyal hesap ile oturum açtıktan sonra başarısız girişimleri 2FA'konumunda depolanır. En çok başarısız erişim denemesi ulaşıldığında, kullanıcının kilitli (varsayılan: 5 erişim denemesi başarısız olduktan sonra 5 dakika kilitleme). Başarılı bir kimlik doğrulaması başarısız erişim denemesi sayısını sıfırlar ve saatini sıfırlar. En fazla erişim denemesi başarısız oldu ve kilitleme süresi ile ayarlanabilir [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) ve [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). Aşağıdaki hesap kilitleme 10 erişim denemesi başarısız olduktan sonra 10 dakika için yapılandırır:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Onaylayın [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) ayarlar `lockoutOnFailure` için `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
