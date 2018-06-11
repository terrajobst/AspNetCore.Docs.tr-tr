---
title: Hesap doğrulama ve ASP.NET Core parola kurtarma
author: rick-anderson
description: E-posta onayı ve parola sıfırlama ile ASP.NET Core uygulama oluşturmayı öğrenin.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: d7c1aea2b533fc614eb25c537b72bea773e76077
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252288"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Hesap doğrulama ve ASP.NET Core parola kurtarma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette)

Bu öğretici, e-posta onayı ve parola sıfırlama ile ASP.NET Core uygulamasının nasıl oluşturulacağını gösterir. Bu öğretici olan **değil** başına konu. Hakkında bilginiz olması gerekir:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Kimlik Doğrulaması](xref:security/authentication/index)
* [Hesap Onaylama ve Parola Kurtarma](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Bkz: [bu PDF dosyası](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC 1.1 ve 2.x sürümleri için.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>.NET Core CLI ile yeni bir ASP.NET Core projesi oluşturma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* `--auth Individual` Bireysel kullanıcı hesapları proje şablonu belirtir.
* Windows üzerinde eklemek `-uld` seçeneği. Yerel veritabanı yerine SQLite kullanılması gerektiğini belirtir.
* Çalıştırma `new mvc --help` bu komutla ilgili Yardım almak için.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

CLI veya SQLite kullanıyorsanız, bir komut penceresinde aşağıdaki komutu çalıştırın:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Bireysel kullanıcı hesapları proje şablonu belirtir.
* Windows üzerinde eklemek `-uld` seçeneği. Yerel veritabanı yerine SQLite kullanılması gerektiğini belirtir.
* Çalıştırma `new mvc --help` bu komutla ilgili Yardım almak için.

---

Alternatif olarak, Visual Studio ile yeni bir ASP.NET Core proje oluşturabilirsiniz:

* Visual Studio'da yeni bir oluşturma **Web uygulaması** projesi.
* Seçin **ASP.NET Core 2.0**. **.NET core** aşağıdaki görüntüde, seçili ancak seçebilirsiniz **.NET Framework**.
* Seçin **kimlik doğrulamayı Değiştir** ve kümesine **tek tek kullanıcı hesaplarını**.
* Varsayılan tutmak **deposu kullanıcı hesapları uygulama**.

!["Seçilen bireysel kullanıcı hesapları radyo" gösteren yeni proje iletişim kutusu](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Test yeni kullanıcı kaydı

Uygulama, belirleyin **kaydetmek** bağlantı ve bir kullanıcı kaydı. Entity Framework Çekirdek geçişler çalıştırmak için yönergeleri izleyin. Bu noktada, yalnızca doğrulama e-posta ile olan [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) özniteliği. Kayıt gönderdikten sonra uygulamaya günlüğe kaydedilir. E-postalarına doğrulanıncaya kadar yeni kullanıcılar oturum açamaz böylece daha sonra öğreticide kodu güncelleştirilir.

## <a name="view-the-identity-database"></a>Görünüm kimliği veritabanı

Bkz: [SQLite ile ASP.NET Core MVC projesinde iş](xref:tutorials/first-mvc-app-xplat/working-with-sql) SQLite veritabanı görüntülemek yönergeler için.

Visual Studio için:

* Gelen **Görünüm** menüsünde, select **SQL Server Nesne Gezgini** (SSOX).
* Gidin **(localdb) MSSQLLocalDB (SQL Server 13)**. Sağ **dbo. AspNetUsers** > **verileri görüntüleme**:

![SQL Server Nesne Gezgini AspNetUsers tabloda bağlam menüsünde](accconfirm/_static/ssox.png)

Tablonun Not `EmailConfirmed` alanı `False`.

Uygulama bir onay e-posta gönderirken bu e-posta yeniden sonraki adımda kullanmak isteyebilirsiniz. Sağ tıklatın ve satır **silmek**. E-posta diğer silme, aşağıdaki adımlarda kolaylaştırır.

---

## <a name="require-https"></a>HTTPS gerektirir

Bkz: [HTTPS gerektiren](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>E-posta onayı iste

Yeni bir kullanıcı kaydı e-posta onaylamak için en iyi bir uygulamadır. E-posta, bunlar değil kimliğine bürünmek başkası doğrulamak için onayı yardımcı olur (diğer bir deyişle, başka birinin e-posta ile kayıtlı olmayan). Bir tartışma Forumu var ve bu engellemek istediğinizi varsayalım "yli@example.com"Kimden olarak kaydetme"nolivetto@contoso.com". E-posta onaysız "nolivetto@contoso.com" istenmeyen e-posta uygulamanızdan alabilir. Kullanıcı olarak yanlışlıkla kayıtlı varsayalım "ylo@example.com" ve "yli", yazım hatası fark yüklediniz. Parola kurtarma uygulama doğru e-postalarına sahip olmadığından bunlar bağlanamayacak. E-posta onayı yalnızca sınırlı koruma aracılarını sağlar. E-posta onayı birçok e-posta hesaplarıyla kötü niyetli kullanıcıların koruma sağlamaz.

Genellikle, yeni kullanıcılar sahip oldukları onaylanan bir e-posta önce web siteniz için herhangi bir veri gönderme önlemek isteyebilirsiniz.

Güncelleştirme `ConfigureServices` onaylanan bir e-posta istemek için:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` kayıtlı kullanıcıların e-postalarına onaylandıktan kadar oturum açmayı engeller.

### <a name="configure-email-provider"></a>E-posta sağlayıcısı yapılandırma

Bu öğreticide, SendGrid e-posta göndermek için kullanılır. SendGrid hesabı ve e-posta göndermek için anahtarı gerekir. Diğer e-posta sağlayıcısı kullanabilirsiniz. ASP.NET Core 2.x içeren `System.Net.Mail`, uygulamanızdan e-posta Gönder sağlar. E-posta göndermek için SendGrid veya başka bir e-posta hizmeti kullanmanızı öneririz. SMTP güvenli ve düzgün şekilde ayarlanmış zordur.

[Seçenekleri düzeni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için kullanılır. Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).

Güvenli e-posta anahtar getirmek için bir sınıf oluşturun. Bu örnek için `AuthMessageSenderOptions` sınıfı oluşturulur *Services/AuthMessageSenderOptions.cs* dosyası:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Ayarlama `SendGridUser` ve `SendGridKey` ile [gizli Yöneticisi aracını](xref:security/app-secrets). Örneğin:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Windows, gizli Yöneticisi anahtarları/değer çiftleri olarak depolar. bir *secrets.json* dosyasını `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` dizin.

İçeriğini *secrets.json* olmayan dosya şifreli. *Secrets.json* dosya aşağıda gösterilen ( `SendGridKey` değeri kaldırıldı.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Başlangıç AuthMessageSenderOptions kullanacak şekilde yapılandırma

Ekleme `AuthMessageSenderOptions` sonunda hizmet kapsayıcısı `ConfigureServices` yönteminde *haline* dosyası:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>AuthMessageSender sınıfını Yapılandır

Bu öğretici aracılığıyla e-posta bildirimleri eklemeyi gösterir [SendGrid](https://sendgrid.com/), ancak SMTP ve diğer mekanizmalarını kullanarak e-posta gönderebilir.

Yükleme `SendGrid` NuGet paketi:

* Komut satırından:

    `dotnet add package SendGrid`

* Paket Yöneticisi konsolundan aşağıdaki komutu girin:

  `Install-Package SendGrid`

Bkz: [SendGrid ücretsiz olarak başlayın](https://sendgrid.com/free/) ücretsiz SendGrid hesap için kaydedilecek.

#### <a name="configure-sendgrid"></a>SendGrid yapılandırın

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

SendGrid yapılandırmak için aşağıdakine benzer bir kod ekleyin *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Kod ekleme *Services/MessageServices.cs* SendGrid yapılandırmak için aşağıdakine benzer:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Hesap onayı ve parola kurtarmayı etkinleştir

Hesap onaylama ve parolayı kurtarma için kod şablonu yok. Bul `OnPostAsync` yönteminde *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Yeni kaydettiğiniz kullanıcıların otomatik olarak aşağıdaki satırını yorum oluşturma oturum açmış engellemek:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Tam yöntem vurgulanmış değiştirilen satırıyla gösterilir:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Hesap doğrulama etkinleştirmek için aşağıdaki kodu açıklamadan çıkarın:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Not:** kodu yeni kayıtlı kullanıcı otomatik olarak aşağıdaki satırını yorum oluşturma oturum açmış engelleyen:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Kodda uncommenting tarafından parola kurtarmayı etkinleştirme `ForgotPassword` eylemi *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Form öğesinde açıklamadan çıkarın *Views/Account/ForgotPassword.cshtml*. Kaldırmak istediğiniz `<p> For more information on how to enable reset password ... </p>` bu makaleye bir bağlantı içeren öğe.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

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

![Gezinti çubuğu](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Yönet sayfası görüntülenir **profil** sekmesi seçili. **E-posta** e-posta belirten bir onay kutusu onaylanıp gösterir.

![Yönet sayfası](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Bu öğreticide daha sonra belirtiliyor.
![Yönet sayfası](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Test parola sıfırlama

* Oturum açtınız, seçin **oturum kapatma**.
* Seçin **oturum** seçin ve bağlama **parolanızı mı unuttunuz?** bağlantı.
* Hesabını kaydetmek için kullanılan e-posta girin.
* Parolanızı sıfırlamak için bir bağlantı içeren bir e-posta gönderilir. E-postanızı kontrol edin ve parolanızı sıfırlamak için bağlantıya tıklayın. Parolanız başarıyla sıfırlandı sonra e-posta ve yeni parolayla oturum açabilir.

<a name="debug"></a>

### <a name="debug-email"></a>E-posta hata ayıklama

E-posta çalışma alınamıyor ise:

* Oluşturma bir [e-posta göndermek için konsol uygulaması](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Gözden geçirme [e-posta etkinliğinin](https://sendgrid.com/docs/User_Guide/email_activity.html) sayfası.
* İstenmeyen posta klasörünüzü kontrol edin.
* Farklı bir e-posta sağlayıcısı (Microsoft, Yahoo, Gmail, vb.) başka bir e-posta diğer deneyin
* Farklı bir e-posta hesaplarına göndermeyi deneyin.

**En iyi güvenlik uygulaması** için **değil** test ve geliştirme üretim gizlilikleri kullanın. Uygulamayı Azure'a yayımlama, uygulama ayarları Azure Web uygulaması portal olarak SendGrid parolaları ayarlayabilirsiniz. Yapılandırma sistemi ortam değişkenlerinin anahtarları okumak için ayarlanır.

## <a name="combine-social-and-local-login-accounts"></a>Sosyal ve yerel oturum açma hesaplarını birleştirmek

Bu bölümde tamamlamak için önce bir dış kimlik doğrulama sağlayıcısı etkinleştirmeniz gerekir. Bkz: [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).

Yerel ve sosyal hesapları, e-posta bağlantısını tıklatarak birleştirebilirsiniz. Aşağıdaki sırayla "RickAndMSFT@gmail.com" ilk bir yerel oturum açma; oluşturulur ancak, ilk sosyal bir oturum açma hesabı oluşturmak sonra yerel oturum açma ekleyin.

![Web uygulaması: RickAndMSFT@gmail.com kimliği doğrulanmış kullanıcı](accconfirm/_static/rick.png)

Tıklayın **Yönet** bağlantı. Bu hesapla ilişkilendirilen 0 dış (sosyal oturum açma bilgileri) unutmayın.

![Görünüm yönetme](accconfirm/_static/manage.png)

Başka bir oturum açma hizmeti bağlantısını tıklatın ve uygulama isteklerini kabul edin. Aşağıdaki görüntüde, Facebook Dış kimlik doğrulama sağlayıcısı'dır:

![Harici oturum görünümünüzü Facebook listeleme yönetme](accconfirm/_static/fb.png)

İki hesap birleştirilmiştir. Herhangi bir hesabı ile oturum açabilir. Kullanıcılarınızın kendi sosyal oturum açma kimlik doğrulama hizmeti çalışmıyor ya da erişim sosyal hesaplarında daha büyük bir olasılıkla kaybettiğini durumda yerel hesaplar eklemek isteyebilirsiniz.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Kullanıcıların bir siteye atandıktan sonra Hesap doğrulama etkinleştir

Kullanıcılarla etkinleştirme Hesap doğrulama sitesinde var olan tüm kullanıcıların kilitler. Hesaplarında onaylanıp değil çünkü mevcut kullanıcılar kilitlenir. Varolan kullanıcı kilitlemesinin olarak çözmek için aşağıdaki yaklaşımlardan birini kullanın:

* Onaylanan olarak tüm mevcut kullanıcılar işaretlemek için veritabanını güncelleştirin.
* Mevcut kullanıcıları onaylayın. Örneğin, batch onay bağlantılar ile e-postaları gönderme.
