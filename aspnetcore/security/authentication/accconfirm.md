---
title: "Hesap doğrulama ve ASP.NET Core parola kurtarma"
author: rick-anderson
description: "E-posta onayı ve parola sıfırlama ile ASP.NET Core uygulamasının nasıl oluşturulacağını gösterir."
manager: wpickett
ms.author: riande
ms.date: 12/1/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 459f1793b1f1f73792bb6537856cb739774c6261
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Hesap doğrulama ve ASP.NET Core parola kurtarma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette) 

Bu öğretici, e-posta onayı ve parola sıfırlama ile ASP.NET Core uygulamasının nasıl oluşturulacağını gösterir.

## <a name="create-a-new-aspnet-core-project"></a>Yeni bir ASP.NET Core projesi oluşturma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Bu adım Windows Visual Studio için geçerlidir. CLI yönergeleri için sonraki bölüme bakın.

Öğretici, Visual Studio 2017 Preview 2 veya üstünü gerektirir.

* Visual Studio'da yeni bir Web uygulaması projesi oluşturun.
* Seçin **ASP.NET Core 2.0**. Aşağıdaki resim Göster **.NET Core** seçildi, ancak seçebilirsiniz **.NET Framework**.
* Seçin **kimlik doğrulamayı Değiştir** ve kümesine **tek tek kullanıcı hesaplarını**.
* Varsayılan tutmak **deposu kullanıcı hesapları uygulama**.

!["Seçilen bireysel kullanıcı hesapları radyo" gösteren yeni proje iletişim kutusu](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Öğretici Visual Studio 2017 veya üstünü gerektirir.

* Visual Studio'da yeni bir Web uygulaması projesi oluşturun.
* Seçin **kimlik doğrulamayı Değiştir** ve kümesine **tek tek kullanıcı hesaplarını**.

!["Seçilen bireysel kullanıcı hesapları radyo" gösteren yeni proje iletişim kutusu](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a>MacOS ve Linux için .NET core CLI proje oluşturma

CLI veya SQLite kullanıyorsanız, bir komut penceresinde aşağıdaki komutu çalıştırın:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual`Bireysel kullanıcı hesapları şablonu belirtir.
* Windows üzerinde eklemek `-uld` seçeneği. `-uld` Seçenek bir yerel veritabanı bağlantı dizesi yerine bir SQLite veritabanı oluşturur.
* Çalıştırma `new mvc --help` bu komutla ilgili Yardım almak için.

## <a name="test-new-user-registration"></a>Test yeni kullanıcı kaydı

Uygulama, belirleyin **kaydetmek** bağlantı ve bir kullanıcı kaydı. Entity Framework Çekirdek geçişler çalıştırmak için yönergeleri izleyin. Bu noktada, yalnızca doğrulama e-posta ile olan [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) özniteliği. Kayıt gönderdikten sonra uygulamaya günlüğe kaydedilir. E-postalarına doğrulanıncaya kadar yeni kullanıcılar oturum açamaz böylece daha sonra öğreticide Biz bu değiştireceksiniz.

## <a name="view-the-identity-database"></a>Görünüm kimliği veritabanı

# <a name="sql-servertabsql-server"></a>[SQL Server](#tab/sql-server)

* Gelen **Görünüm** menüsünde, select **SQL Server Nesne Gezgini** (SSOX). 
* Gidin **(localdb) MSSQLLocalDB (SQL Server 13)**. Sağ **dbo. AspNetUsers** > **verileri görüntüleme**:

![SQL Server Nesne Gezgini AspNetUsers tabloda bağlam menüsünde](accconfirm/_static/ssox.png)

Not `EmailConfirmed` alanı `False`.

Uygulama bir onay e-posta gönderirken bu e-posta yeniden sonraki adımda kullanmak isteyebilirsiniz. Sağ tıklatın ve satır **silmek**. E-posta diğer adı şimdi silme aşağıdaki adımlarda kolaylaştırır.

# <a name="sqlitetabsqlite"></a>[SQLite](#tab/sqlite)

Bkz: [SQLite ile ASP.NET Core MVC projesinde çalışma](xref:tutorials/first-mvc-app-xplat/working-with-sql) SQLite DB görüntülemek yönergeler için. 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a>SSL gerektirecek ve IIS Express için SSL Kurulumu

Bkz: [SSL zorlamayı](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>E-posta onayı iste

Bunlar değil kimliğine bürünmek başkası doğrulamak için yeni bir kullanıcı kaydı e-posta onaylamak için en iyi bir uygulamadır (diğer bir deyişle, başka birinin e-posta ile kayıtlı olmayan). Bir tartışma Forumu var ve bu engellemek istediğinizi varsayalım "yli@example.com"Kimden olarak kaydetme"nolivetto@contoso.com." E-posta onaysız "nolivetto@contoso.com" istenmeyen e-posta uygulamanızdan alabilir. Kullanıcı olarak yanlışlıkla kayıtlı varsayalım "ylo@example.com" ve yazım hatası fark yüklediniz "yli," uygulama doğru e-postalarına sahip olmadığından parola kurtarma kullanılacak karşılayamazlar. E-posta onayı aracılarını, yalnızca sınırlı koruma sağlar ve kaydetmek için kullanabilirsiniz çok sayıda çalışan e-posta diğer adları olan belirlendiği istenmeyen posta gönderenlerin koruma sağlamaz.

Genellikle, yeni kullanıcılar sahip oldukları onaylanan bir e-posta önce web siteniz için herhangi bir veri gönderme önlemek isteyebilirsiniz. 

Güncelleştirme `ConfigureServices` onaylanan bir e-posta istemek için:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
Önceki satır kayıtlı kullanıcıların e-postalarına onaylandıktan kadar oturum açılmış engeller. Ancak, o satırdaki yeni kullanıcıların bunlar kaydettikten sonra oturum açılmış engellemek değil. Bunlar kaydettikten sonra varsayılan kod kullanıcı olarak günlüğe kaydeder. Oturum kapatma sonra bunlar kaydoluncaya kadar yeniden oturum açmak değiştiremezler. Daha sonra değiştirmek öğreticide kadar yeni kaydedilen kod kullanıcısıysanız **değil** oturum.

### <a name="configure-email-provider"></a>E-posta sağlayıcısı yapılandırma

Bu öğreticide, SendGrid e-posta göndermek için kullanılır. SendGrid hesabı ve e-posta göndermek için anahtarı gerekir. Diğer e-posta sağlayıcısı kullanabilirsiniz. ASP.NET Core 2.x içeren `System.Net.Mail`, uygulamanızdan e-posta Gönder sağlar. E-posta göndermek için SendGrid veya başka bir e-posta hizmeti kullanmanızı öneririz.

[Seçenekleri düzeni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için kullanılır. Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).

Güvenli e-posta anahtar getirmek için bir sınıf oluşturun. Bu örnek için `AuthMessageSenderOptions` sınıfı oluşturulur *Services/AuthMessageSenderOptions.cs* dosya.

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Ayarlama `SendGridUser` ve `SendGridKey` ile [gizli Yöneticisi aracını](../app-secrets.md). Örneğin:

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Windows, gizli Yöneticisi anahtarları/değer çiftleri olarak depolar. bir *secrets.json* %APPDATA%/Microsoft/UserSecrets/ < WebAppName-userSecretsId > dizindeki dosyayı.

İçeriğini *secrets.json* dosya şifrelenmez. *Secrets.json* dosya aşağıda gösterilen ( `SendGridKey` değeri kaldırıldı.)

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Başlangıç AuthMessageSenderOptions kullanacak şekilde yapılandırma

Ekleme `AuthMessageSenderOptions` sonunda hizmet kapsayıcısı `ConfigureServices` yönteminde *haline* dosyası:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>AuthMessageSender sınıfını Yapılandır

Bu öğretici aracılığıyla e-posta bildirimleri eklemeyi gösterir [SendGrid](https://sendgrid.com/), ancak SMTP ve diğer mekanizmalarını kullanarak e-posta gönderebilir.

* Yükleme `SendGrid` NuGet paketi. Paket Yöneticisi konsolundan aşağıdaki girin aşağıdaki komutu:

  `Install-Package SendGrid`

* Bkz: [SendGrid ücretsiz olarak başlayın](https://sendgrid.com/free/) ücretsiz SendGrid hesap için kaydedilecek.

#### <a name="configure-sendgrid"></a>SendGrid yapılandırın

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* Kod ekleme *Services/EmailSender.cs* SendGrid yapılandırmak için aşağıdakine benzer:

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* Kod ekleme *Services/MessageServices.cs* SendGrid yapılandırmak için aşağıdakine benzer:

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Hesap onayı ve parola kurtarmayı etkinleştir

Hesap onaylama ve parolayı kurtarma için kod şablonu yok. Bul `[HttpPost] Register` yönteminde *AccountController.cs* dosya.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Yeni kaydettiğiniz kullanıcıların otomatik olarak aşağıdaki satırını yorum oluşturma oturum açmış engellemek:

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

Tam yöntem vurgulanmış değiştirilen satırıyla gösterilir:

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

Not: uygulamanız önceki kod başarısız olur `IEmailSender` ve düz metin e-posta gönderin. Bkz: [bu sorunu](https://github.com/aspnet/Home/issues/2152) daha fazla bilgi ve geçici bir çözüm için.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Hesap doğrulama etkinleştirmek için kodun açıklamasını kaldırın.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

Not: Biz de kullanıcı yeni kayıtlı otomatik olarak aşağıdaki satırını yorum oluşturma oturum açmış engelleyen:

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Kodda uncommenting tarafından parola kurtarmayı etkinleştirme `ForgotPassword` eylemde *Controllers/AccountController.cs* dosya.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Form öğesinde açıklamadan çıkarın *Views/Account/ForgotPassword.cshtml*. Kaldırmak istediğiniz `<p> For more information on how to enable reset password ... </p>` bu makaleye bir bağlantı içeren öğe.

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Kayıt, e-posta onaylayın ve parola sıfırlama

Web uygulaması çalıştırın ve hesap ve parola kurtarma akışı sınayın.

* Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı

 ![Web uygulaması hesabını kaydetmek görünümü](accconfirm/_static/loginaccconfirm1.png)

* Hesap onay bağlantısı için e-postanızı kontrol edin. Bkz: [Debug e-posta](#debug) e-posta alamazsanız.
* E-postanızı onaylamak için bağlantıyı tıklatın.
* E-posta ve parola ile oturum açın.
* Oturumunuzu kapatın.

### <a name="view-the-manage-page"></a>Yönet sayfasını görüntüleyin

Kullanıcı adınızı tarayıcıda seçin: ![kullanıcı adıyla bir tarayıcı penceresi](accconfirm/_static/un.png)

Kullanıcı adı görmek için gezinti çubuğu genişletmeniz gerekebilir.

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Yönet sayfası görüntülenir **profil** sekmesi seçili. **E-posta** e-posta belirten bir onay kutusu onaylanıp gösterir. 

![Yönet sayfası](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Biz, öğreticide daha sonra bu sayfa hakkında konuşun.
![Yönet sayfası](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Test parola sıfırlama

* Oturum açtınız, seçin **oturum kapatma**.  
* Seçin **oturum** seçin ve bağlama **parolanızı mı unuttunuz?** bağlantı.
* Hesabını kaydetmek için kullanılan e-posta girin.
* Parolanızı sıfırlamak için bir bağlantı içeren bir e-posta gönderilir. E-postanızı kontrol edin ve parolanızı sıfırlamak için bağlantıya tıklayın.  Parolanızı başarıyla sıfırladıktan sonra e-posta ve yeni bir parola ile oturum açabilirsiniz.

<a name="debug"></a>

### <a name="debug-email"></a>E-posta hata ayıklama

E-posta çalışma alınamıyor ise:

* Gözden geçirme [e-posta etkinliğinin](https://sendgrid.com/docs/User_Guide/email_activity.html) sayfası.
* İstenmeyen posta klasörünüzü kontrol edin.
* Farklı bir e-posta sağlayıcısı (Microsoft, Yahoo, Gmail, vb.) başka bir e-posta diğer deneyin
* Oluşturma bir [e-posta göndermek için konsol uygulaması](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Farklı bir e-posta hesaplarına göndermeyi deneyin.

**Not:** en iyi güvenlik uygulaması test ve geliştirme üretim parolalarında kullanmamaktır. Uygulamayı Azure'a yayımlama, uygulama ayarları Azure Web uygulaması portal olarak SendGrid parolaları ayarlayabilirsiniz. Ortam değişkenlerinin anahtarları okumak için kurulum yapılandırma sistemidir.

## <a name="prevent-login-at-registration"></a>Kayıt sırasında oturum açma engelle

Bir kullanıcı kayıt formunu tamamladıktan sonra geçerli şablonlarıyla bunlar oturum açtınız (kimliği doğrulanmış). Genellikle, oturum açmayı önce e-postalarına doğrulamak istersiniz. Aşağıdaki bölümde, biz gerektirecek şekilde kodu değiştirecek yeni kullanıcınız bunlar oturum açtınız önce onaylanan bir e-posta. Güncelleştirme `[HttpPost] Login` eylemde *AccountController.cs* aşağıdaki vurgulanan değişikliklerle dosya.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

**Not:** en iyi güvenlik uygulaması test ve geliştirme üretim parolalarında kullanmamaktır. Uygulamayı Azure'a yayımlama, uygulama ayarları Azure Web uygulaması portal olarak SendGrid parolaları ayarlayabilirsiniz. Ortam değişkenlerinin anahtarları okumak için kurulum yapılandırma sistemidir.


## <a name="combine-social-and-local-login-accounts"></a>Sosyal ve yerel oturum açma hesaplarını birleştirmek

Not: Bu bölüm, yalnızca ASP.NET Core geçerlidir 1.x. ASP.NET 2.x çekirdek bkz [bu](https://github.com/aspnet/Docs/issues/3753) sorun.

Bu bölümde tamamlamak için önce bir dış kimlik doğrulama sağlayıcısı etkinleştirmeniz gerekir. Bkz: [Facebook, Google ve diğer dış sağlayıcılarını kullanarak kimlik doğrulamasını etkinleştirme](social/index.md).

Yerel ve sosyal hesapları, e-posta bağlantısını tıklatarak birleştirebilirsiniz. Aşağıdaki sırayla "RickAndMSFT@gmail.com" ilk bir yerel oturum açma; oluşturulur ancak, ilk sosyal bir oturum açma hesabı oluşturmak sonra yerel oturum açma ekleyin.

![Web uygulaması: RickAndMSFT@gmail.com kimliği doğrulanmış kullanıcı](accconfirm/_static/rick.png)

Tıklayın **Yönet** bağlantı. Bu hesapla ilişkilendirilen 0 dış (sosyal oturum açma bilgileri) unutmayın.

![Görünüm yönetme](accconfirm/_static/manage.png)

Başka bir oturum açma hizmeti bağlantısını tıklatın ve uygulama isteklerini kabul edin. Aşağıdaki resimde Facebook Dış kimlik doğrulama sağlayıcısı şöyledir:

![Harici oturum görünümünüzü Facebook listeleme yönetme](accconfirm/_static/fb.png)

İki hesap birleştirilmiştir. Herhangi bir hesabı ile oturum açabilecek olacaktır. Yerel hesaplar durumunda kullanıcının sosyal oturum açtığında kimlik doğrulama hizmeti çalışmıyor ya da erişim sosyal hesaplarında daha büyük bir olasılıkla kaybettiğini eklemek için kullanıcılarınızın isteyebilirsiniz.
