---
title: Hesap onaylama ve parola kurtarma ASP.NET Core
author: rick-anderson
description: E-posta onayı ve parola sıfırlama ile ASP.NET Core uygulaması oluşturmayı öğrenin.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803278"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Hesap onaylama ve parola kurtarma ASP.NET Core

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)

Bu öğretici, e-posta onayı ve parola sıfırlama ile bir ASP.NET Core uygulaması derleme işlemini göstermektedir. Bu öğretici **değil** başına konu. Sahibi olmalısınız:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Kimlik Doğrulaması](xref:security/authentication/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Bkz: [bu PDF dosyası](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC 1.1 ve 2.x'i sürümleri.

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
* Windows üzerinde ekleme `-uld` seçeneği. Bu, LocalDB yerine SQLite kullanılması gerektiğini belirtir.
* Çalıştırma `new mvc --help` bu komutla ilgili Yardım almak için.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

CLI veya SQLite kullanıyorsanız, bir komut penceresinde aşağıdaki komutu çalıştırın:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Bireysel kullanıcı hesapları proje şablonu belirtir.
* Windows üzerinde ekleme `-uld` seçeneği. Bu, LocalDB yerine SQLite kullanılması gerektiğini belirtir.
* Çalıştırma `new mvc --help` bu komutla ilgili Yardım almak için.

---

Alternatif olarak, Visual Studio ile yeni bir ASP.NET Core projesi oluşturabilirsiniz:

* Visual Studio'da yeni bir oluşturma **Web uygulaması** proje.
* Seçin **ASP.NET Core 2.0**. **.NET core** aşağıdaki görüntüde, seçilen ancak seçebileceğiniz **.NET Framework**.
* Seçin **kimlik doğrulamayı Değiştir** ayarlanmış ve **bireysel kullanıcı hesapları**.
* Varsayılan tutun **Store kullanıcı hesaplarını uygulama**.

!["Bireysel kullanıcı hesapları radyo seçili" gösteren yeni proje iletişim kutusu](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Test yeni kullanıcı kaydı

Uygulamayı çalıştırın, seçin **kaydetme** bağlamak ve bir kullanıcı kaydı. Entity Framework Code migrations'ı çalıştırmak için yönergeleri izleyin. Yalnızca e-posta doğrulamasını bu noktada, olan [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) özniteliği. Kayıt gönderdikten sonra uygulamaya günlüğe kaydedilir. E-postasına doğrulanıncaya kadar yeni kullanıcılar oturum açamaz böylece daha sonra öğreticide kod güncelleştirilir.

## <a name="view-the-identity-database"></a>Kimlik veritabanı görünümü

Bkz: [ASP.NET Core MVC projesinde SQLite ile çalışma](xref:tutorials/first-mvc-app-xplat/working-with-sql) SQLite veritabanını görüntülemek yönergeler.

Visual Studio için:

* Gelen **görünümü** menüsünde **SQL Server Nesne Gezgini** (SSOX).
* Gidin **(localdb) (SQL Server 13) ifadesini MSSQLLocalDB**. Sağ **dbo. AspNetUsers** > **verileri görüntüleme**:

![SQL Server Nesne Gezgini AspNetUsers tablosunda bağlamsal menü](accconfirm/_static/ssox.png)

Tablonun Not `EmailConfirmed` alandır `False`.

Bu e-posta uygulaması bir onay e-posta gönderdiğinde, yeniden sonraki adımda kullanmak isteyebilirsiniz. Sağ tıklatın ve satır **Sil**. E-posta diğer adı silmek, aşağıdaki adımlarda kolaylaştırır.

---

## <a name="require-https"></a>HTTPS'yi zorunlu

Bkz: [HTTPS'yi zorunlu](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>E-posta onayı gerektir

Yeni bir kullanıcı kaydı e-postayı onaylamak için iyi bir uygulamadır. E-posta, bunlar değil kimliğe bürünerek başka birisi doğrulamak için onay yardımcı olur (diğer bir deyişle, bunlar başka birinin e-posta ile kayıtlı olmayabilirsiniz). Tartışma Forumu var ve bu önlemek istediğinizi varsayalım "yli@example.com"olarak kaydetme"Kimdennolivetto@contoso.com". E-posta onayı olmadan "nolivetto@contoso.com" istenmeyen e-posta uygulamanızdan alabilir. Kullanıcı olarak yanlışlıkla kayıtlı varsayalım "ylo@example.com" ve "yli", yazım hatası fark yüklediniz. Parola kurtarma uygulamayı doğru e-postasına olmadığından bunlar saptayamazdınız. E-posta onayı robotlar yalnızca sınırlı koruma sağlar. E-posta onayı, çok sayıda e-posta hesaplarına sahip kötü niyetli kullanıcıların koruma sağlamaz.

Genellikle, yeni kullanıcıların onaylanan e-posta sahip oldukları önce web sitenizi herhangi bir veri gönderme engellemek istiyorsunuz.

Güncelleştirme `ConfigureServices` onaylanan e-posta gerektirmek için:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` kayıtlı kullanıcıların e-postasına onaylanana kadar oturum açma engeller.

### <a name="configure-email-provider"></a>E-posta sağlayıcısı yapılandırma

Bu öğreticide, SendGrid e-posta göndermek için kullanılır. SendGrid hesabı ve e-posta göndermek için anahtar ihtiyacınız var. Diğer e-posta sağlayıcılarının kullanabilirsiniz. ASP.NET Core 2.x içerir `System.Net.Mail`, uygulamanızdan e-posta göndermenize olanak tanıyan. E-posta göndermek için SendGrid veya başka bir e-posta hizmeti kullanmanızı öneririz. SMTP güvenli ve doğru bir şekilde ayarlamak zordur.

[Seçenekleri deseni](xref:fundamentals/configuration/options) kullanıcı hesabı ve anahtarı ayarlarına erişmek için kullanılır. Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).

Güvenli e-posta anahtarı almak için bir sınıf oluşturun. Bu örnek için `AuthMessageSenderOptions` sınıf oluşturulduğu *Services/AuthMessageSenderOptions.cs* dosyası:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Ayarlama `SendGridUser` ve `SendGridKey` ile [gizli dizi Yöneticisi aracını](xref:security/app-secrets). Örneğin:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Windows üzerinde gizli dizi Yöneticisi'ni anahtar/değer çiftleri olarak depolar. bir *secrets.json* dosyası `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` dizin.

İçeriğini *secrets.json* olmayan dosya şifrelenmiş. *Secrets.json* aşağıda gösterilmektedir dosyanın ( `SendGridKey` değer kaldırıldı.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Başlangıç AuthMessageSenderOptions kullanmak için yapılandırma

Ekleme `AuthMessageSenderOptions` sonunda hizmet kapsayıcıya `ConfigureServices` yönteminde *Startup.cs* dosyası:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>AuthMessageSender sınıfını Yapılandır

Bu öğretici, e-posta bildirimleri aracılığıyla ekleneceği gösterilmiştir [SendGrid](https://sendgrid.com/), ancak e-posta SMTP veya başka mekanizmalar kullanılarak gönderebilirsiniz.

Yükleme `SendGrid` NuGet paketi:

* Komut satırından:

    `dotnet add package SendGrid`

* Paket Yöneticisi konsolundan aşağıdaki komutu girin:

  `Install-Package SendGrid`

Bkz: [SendGrid ile ücretsiz olarak kullanmaya başlayın](https://sendgrid.com/free/) ücretsiz SendGrid hesabı kaydedilecek.

#### <a name="configure-sendgrid"></a>SendGrid yapılandırın

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

SendGrid yapılandırmak için aşağıdakine benzer bir kod ekleme *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Kodda *Services/MessageServices.cs* SendGrid yapılandırmak için aşağıdakine benzer:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Hesap onaylama ve parola kurtarmayı etkinleştir

Şablon hesap onaylama ve parola kurtarma kodu var. Bulma `OnPostAsync` yönteminde *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Yeni kaydettiğiniz kullanıcıların otomatik olarak aşağıdaki satırı açıklama satırı yaparak oturum engellemek:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Vurgulanmış satır ile tam yöntemi gösterilir:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Hesap onaylama etkinleştirmek için aşağıdaki kodun açıklamasını kaldırın:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Not:** kodu yeni kaydettiğiniz kullanıcı otomatik olarak aşağıdaki satırı açıklama satırı yaparak oturum engelleyen:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Kodda uncommenting tarafından parola kurtarmayı etkinleştirmek `ForgotPassword` eylemi *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Form öğesinde açıklamadan çıkarın *Views/Account/ForgotPassword.cshtml*. Kaldırmak istediğiniz `<p> For more information on how to enable reset password ... </p>` bu makalede bir bağlantı içeren öğe.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Kaydetme, e-posta onayı ve parola sıfırlama

Web uygulamasını çalıştırın ve hesap onaylama ve parola kurtarma akışı test edin.

* Uygulamayı çalıştırın ve yeni bir kullanıcı kaydı

  ![Web uygulaması görünümü hesabı Kaydet](accconfirm/_static/loginaccconfirm1.png)

* Hesap onay bağlantısı için e-postanızı kontrol edin. Bkz: [hata ayıklama, e-posta](#debug) e-posta alırsanız yok.
* E-postanızı doğrulamak için bağlantıya tıklayın.
* E-posta ve parolayla oturum.
* Oturumunuzu kapatın.

### <a name="view-the-manage-page"></a>Yönet sayfasını görüntüle

Tarayıcıda kullanıcı adınızı seçin: ![tarayıcı penceresi ile kullanıcı adı](accconfirm/_static/un.png)

Kullanıcı adı görmek için Gezinti genişletmeniz gerekebilir.

![Gezinti çubuğu](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

İle Yönet sayfasında görüntülenen **profili** sekmesi seçili. **E-posta** e-posta belirten bir onay kutusu onaylanan gösterir.

![Yönet sayfası](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Bu öğreticinin ilerleyen bölümlerinde açıklanan.
![Yönet sayfası](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Test parola sıfırlama

* Oturum açmadıysanız, seçin **oturum kapatma**.
* Seçin **oturum** seçin ve bağlama **parolanızı mı unuttunuz?** bağlantı.
* Hesap kaydolmak için kullandığınız e-posta girin.
* Parolanızı sıfırlamak için bir bağlantı içeren bir e-posta gönderilir. E-postanızı kontrol edin ve parolanızı sıfırlamak için bağlantıya tıklayın. Parolanız başarıyla sıfırlandı sonra e-posta ve yeni bir parola ile oturum açabilir.

<a name="debug"></a>

### <a name="debug-email"></a>E-posta hata ayıklama

E-posta çalışma erişemiyorsanız:

* Oluşturma bir [e-posta göndermek için konsol uygulaması](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Gözden geçirme [e-posta etkinlik](https://sendgrid.com/docs/User_Guide/email_activity.html) sayfası.
* İstenmeyen posta klasörünüzü kontrol edin.
* Farklı bir e-posta sağlayıcısı (Microsoft, Yahoo, Gmail, vb.) başka bir e-posta diğer adı deneyin
* Farklı bir e-posta hesaplarına göndermeyi deneyin.

**En iyi güvenlik uygulaması** için **değil** test ve geliştirme üretim gizli dizileri kullanın. Uygulamayı Azure'a yayımlama, Web uygulamasını Azure Portalı'nda Uygulama ayarları olarak SendGrid parolaları ayarlayabilirsiniz. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.

## <a name="combine-social-and-local-login-accounts"></a>Sosyal ve yerel oturum açma hesabını birleştirin

Bu bölümde tamamlamak için önce bir dış kimlik doğrulama sağlayıcısı etkinleştirmeniz gerekir. Bkz: [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).

Yerel hem de sosyal hesaplar, e-posta bağlantısına tıklayarak birleştirebilirsiniz. Aşağıdaki sırayla "RickAndMSFT@gmail.com" ilk olarak bir yerel oturum açın; oluşturulur ancak, bir sosyal oturum açma ilk hesap oluşturun, sonra bir yerel oturum açma ekleme.

![Web uygulaması: RickAndMSFT@gmail.com kimliği doğrulanmış kullanıcı](accconfirm/_static/rick.png)

Tıklayarak **Yönet** bağlantı. Bu hesapla ilişkili 0 dış (sosyal oturum açma bilgileri) unutmayın.

![Görünümü yönetme](accconfirm/_static/manage.png)

Başka bir oturum açma hizmeti bağlantısını tıklayın ve uygulama isteklerini kabul etmek. Aşağıdaki görüntüde, Facebook Dış kimlik doğrulama sağlayıcısı'dır:

![Facebook listeleme görünümünüzü harici oturum açmaları yönetme](accconfirm/_static/fb.png)

İki hesap birleştirilmiştir. Herhangi bir hesabı ile oturum açabilir. Kullanıcılarınızın kendi sosyal oturum açma kimlik doğrulama hizmeti çalışmıyor veya bunlar sosyal hesaplarına erişim daha büyük bir olasılıkla kaybettiğinizde durumunda yerel hesaplar eklemek isteyebilirsiniz.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Kullanıcılar bir siteye sahip olduktan sonra hesap onaylama etkinleştir

Var olan tüm kullanıcıları etkinleştirme hesap onaylama sitesinde kullanıcılarla kilitler. Varolan kullanıcı hesaplarını onaylanan değildir çünkü kilitlidir. Mevcut kullanıcı kilitlemesinin geçici olarak çözmek için aşağıdaki yaklaşımlardan birini kullanın:

* Tüm mevcut kullanıcılar onaylanıp olarak işaretlemek için veritabanı güncelleştirin.
* Mevcut kullanıcıları onaylayın. Örneğin, batch onay bağlantılarının ile e-posta gönderme.
