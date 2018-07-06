---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: E-posta onayı ve parola sıfırlama (C#) ile kullanıcı kaydı, güvenli bir ASP.NET Web Forms uygulaması oluşturma | Microsoft Docs
author: Erikre
description: Bu öğretici, kullanıcı kaydı, e-posta onayı ve ASP.NET Identity üye kullanarak parola sıfırlama ile bir ASP.NET Web formları uygulamasının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
ms.date: 10/02/2014
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 1c230c3f33bd8261312485e9d77f6f88adb49e9e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812775"
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>E-posta onayı ve parola sıfırlama (C#) ile kullanıcı kaydı, güvenli bir ASP.NET Web Forms uygulaması oluşturma
====================
tarafından [Erik Reitan](https://github.com/Erikre)

> Bu öğretici, kullanıcı kaydı, e-posta onayı ve ASP.NET Identity üyelik sistemini kullanarak parola sıfırlama ile bir ASP.NET Web Forms uygulaması derleme işlemini göstermektedir. Bu öğreticide Rick Anderson'un üzerinde temel [MVC Öğreticisi](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Giriş

Bu öğreticide Visual Studio ve ASP.NET 4.5 kullanarak kullanıcı kaydı ile güvenli bir Web Forms uygulaması oluşturma, e-posta onayı ve parola sıfırlama için bir ASP.NET Web Forms uygulaması oluşturmak için gereken adımlarda size kılavuzluk eder.

### <a name="tutorial-tasks-and-information"></a>Öğretici görevleri ve bilgileri:

- [Oluşturma bir ASP.NET Web Forms uygulaması](#createWebForms)
- [SendGrid bağlama](#SG)
- [Oturum açma önce e-posta onayı gerektir](#require)
- [Parola kurtarma ve sıfırlama](#reset)
- [E-posta doğrulama bağlantısını yeniden gönder](#rsend)
- [Uygulama sorunlarını giderme](#dbg)
- [Ek kaynaklar](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Bir ASP.NET Web Forms uygulaması oluşturma

Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Yükleme [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) veya üzeri de.

> [!NOTE]
> Uyarı: Yüklemelisiniz [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da bu öğreticiyi tamamlamak için daha yüksek.


1. Yeni bir proje oluşturun (**dosya**  - &gt; **yeni proje**) seçip **ASP.NET Web uygulaması** şablon ve en son .NET Framework sürümünden **yeni proje** iletişim kutusu.
2. Gelen **yeni ASP.NET projesi** iletişim kutusunda **Web Forms** şablonu. Varsayılan kimlik doğrulaması olarak bırakın **bireysel kullanıcı hesapları**. Uygulamayı azure'da barındırmak istiyorsanız, bırakın **bulutta Barındır** onay kutusunu işaretli.   
 ' A tıklayarak **Tamam** yeni projeyi oluşturmak için.  
    ![Yeni ASP.NET projesi iletişim kutusu](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Proje için Güvenli Yuva Katmanı (SSL) etkinleştirin. Bulunan adımları **projesi için SSL'yi etkinleştir** bölümünü [öğretici serisinin Web Forms ile çalışmaya başlama](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Uygulamayı çalıştırın, tıklayın **kaydetme** bağlamak ve yeni bir kullanıcı kaydı. Bu noktada, yalnızca doğrulama e-posta dayanır [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) e-posta adresi doğru biçimlendirildiğinden emin olmak için özniteliği. E-posta onayı eklenecek kodu değiştirir. Tarayıcı penceresini kapatın.
5. İçinde **Sunucu Gezgini** Visual Studio'nun (**görünümü**  - &gt; **Sunucu Gezgini**), gitmek **veri Connections\ DefaultConnection\Tables\AspNetUsers**sağ tıklayın ve seçin **tablo tanımını açın**. 

    Aşağıdaki görüntüde `AspNetUsers` tablo şeması:

    ![AspNetUsers tablosu şeması](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. İçinde **Sunucu Gezgini**, sağ **AspNetUsers** tablosunu seçip **tablo verilerini Göster**.  
  
    ![AspNetUsers tablo verileri](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 Bu noktada kayıtlı kullanıcı için e-posta onaylanmamıştır.
7. Kullanıcı tıklayın, satır ve silmek üzere delete seçin. Bu e-postayı yeniden sonraki adımda ekleyecek ve e-posta adresine bir onay iletisi gönderin.

## <a name="email-confirmation"></a>E-posta onayı

Bunlar değil kimliğe bürünerek başka birisi doğrulamak için yeni bir kullanıcı kaydı sırasında e-postayı onaylamak için iyi bir uygulamadır (diğer bir deyişle, bunlar başka birinin e-posta ile kayıtlı olmayabilirsiniz). Tartışma Forumu tablonuz olduğunu varsayın engellemek istiyorsunuz `"bob@cpandl.com"` olarak kaydetme gelen `"joe@contoso.com"`. E-posta onayı olmadan `"joe@contoso.com"` uygulamanızdan istenmeyen e-posta alabilir. Bob yanlışlıkla kayıtlı varsayalım `"bib@cpandl.com"` ve bunu fark yüklediniz o uygulamayı doğru e-postasını olmadığı için parola kurtarmayı kullanın saptayamazdınız. E-posta onayı robotlar yalnızca sınırlı koruma sağlar ve belirlenen istenmeyen posta gönderenlere koruma sağlamaz.

Genellikle, yeni kullanıcıların ya da e-posta, SMS mesajı ya da başka bir mekanizma onaylanmıştır önce Web sitenizi herhangi bir veri gönderme engellemek istiyorsunuz. Aşağıdaki bölümler size e-posta onayı etkinleştirin ve yeni kaydettiğiniz kullanıcıların e-postasına onaylanana kadar oturum açmayı engellemek için kodu değiştirin. SendGrid e-posta hizmeti Bu öğreticide kullanacaksınız.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>SendGrid bağlama

Bu öğreticide yalnızca e-posta bildirimi aracılığıyla ekleme gösterir, ancak [SendGrid](http://sendgrid.com/), e-posta SMTP veya başka mekanizmalar kullanılarak gönderebilirsiniz (bkz [ek kaynaklar](#addRes)).

1. Visual Studio'da açın **Paket Yöneticisi Konsolu** (**Araçları**  - &gt; **NuGet Paket Yöneticisi**  - &gt; **Paket Yöneticisi Konsolu**) ve aşağıdaki komutu girin:  
    `Install-Package SendGrid`
2. Git [Azure SendGrid kayıt sayfasına](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) ve SendGrid hesabı ücretsiz kaydolun. Ayrıca doğrudan açık bir ücretsiz SendGrid hesabı için kaydolun yapabilecekleriniz [SendGrid site](http://www.sendgrid.com).
3. Gelen **Çözüm Gezgini** açın *IdentityConfig.cs* dosyası *uygulama\_Başlat* klasör içinsarırenklevurgulanırveaşağıdakikoduekleyin`EmailService` yapılandırmak için sınıf **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Ayrıca, aşağıdaki ekleyin `using` başlangıcına deyimleri *IdentityConfig.cs* dosyası: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Bu örneği basit tutmak için e-posta hizmeti hesabı değerleri depolayacağınızı `appSettings` bölümünü *web.config* dosya. Sarı ile vurgulanmış aşağıdaki XML'i ekleyin *web.config* projenizin kökünde dosyası:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki. Bu örnekte ve hesabın kimlik bilgileri depolanır **appSetting** bölümünü *Web.config* dosya. Azure üzerinde güvenli bir şekilde bu değerleri üzerinde depolamanın **[yapılandırma](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portalında sekmesi. İlgili bilgiler için Rick Anderson'un başlıklı konusuna bakın [parolalar ve diğer hassas verileri ASP.NET ve Azure'a dağıtmak için en iyi yöntemler](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Başarılı olan (kullanıcı adı ve parola) SendGrid kimlik doğrulama değerlerini uygulamanızdan e-posta Gönder yansıtmak üzere e-posta hizmeti değerleri ekleyin. SendGrid sağlanan e-posta adresi yerine SendGrid hesap adınızı kullandığınızdan emin olun.

### <a name="enable-email-confirmation"></a>E-posta onayı etkinleştir

 E-posta Onayı etkinleştirmek için aşağıdaki adımları kullanarak kayıt kodu değiştireceksiniz.  
 

1. İçinde *hesabı* açık klasör *Register.aspx.cs* arka plan kod ve güncelleştirme `CreateUser_Click` yöntemine aşağıdaki vurgulanmış değişiklikleri etkinleştirmek için: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. İçinde **Çözüm Gezgini**, sağ *Default.aspx* seçip **başlangıç sayfası olarak ayarla**.
3. Tuşlarına basarak uygulamayı çalıştırma **F5.** Sayfası görüntülendikten sonra tıklayın **kaydetme** kayıt sayfasını görüntülemek için bağlantı.
4. E-postanızı ve parolanızı girin ve ardından tıklayın **kaydetme** SendGrid aracılığıyla bir e-posta iletisi göndermek için düğme.  
   Proje ve kod geçerli durumu, kullanıcının hesabını onaylanan henüz olsa da bunlar kayıt formunu tamamladıktan sonra oturum açmak izin verir.
5. E-posta hesabınızı denetleyin ve e-postanızı doğrulamak için bağlantıya tıklayın.  
   Kayıt formunu gönderildikten sonra oturumunuz açılır.  
    ![Örnek Web sitesi - oturum açıldı](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Oturum açma önce e-posta onayı gerektir

E-posta hesabı onayladıktan olsa da, bu noktada, tam olarak oturum açmış olması ve doğrulama e-postada yer alan bağlantıyı tıklatın ihtiyaç duymaz. Aşağıdaki bölümde, yeni kullanıcılar oturum önce onaylanan bir e-posta için (kimlik doğrulaması) gerektiren kodu değiştirir.

1. İçinde **Çözüm Gezgini** Visual Studio güncelleştirme `CreateUser_Click` olayında *Register.aspx.cs* gerideki bulunan *hesapları* klasörü Aşağıdaki vurgulanmış değişiklikler: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Güncelleştirme `LogIn` yönteminde *Login.aspx.cs* arka plan kod vurgulanan aşağıdaki değişikliklerle birlikte: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Uygulamayı çalıştırın

 Bir kullanıcının e-posta adresi onaylanıp onaylanmadığını denetlemek için kod uyguladıysanız, hem de işlevlerini denetleyebilirsiniz **kaydetme** ve **oturum açma** sayfaları. 

1. Herhangi bir hesabı silme **AspNetUsers** test etmek istediğiniz e-posta diğer adını içeren tablo.
2. Uygulamayı çalıştırın (**F5**) ve e-posta adresinizi doğrulayıncaya kadar bir kullanıcı olarak kaydedilemiyor doğrulayın.
3. Yeni bir hesapla oturum açmak yeni hesabınız yalnızca gönderildiği e-posta yoluyla onaylamadan önce çalışır.  
 Oturum ve onaylanan e-posta hesabına ihtiyacınız vardır görürsünüz.
4. E-posta adresinizi doğruladıktan sonra uygulamaya oturum açın.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Parola kurtarma ve sıfırlama

1. Visual Studio'da yorum karakterleri Kaldır `Forgot` yöntemi *Forgot.aspx.cs* gerideki bulunan *hesabı* yöntemi olarak görünmesi klasörü izler: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Açık *Login.aspx* sayfası. Sonundaki biçimlendirmeyi Değiştir **loginForm** aşağıda vurgulanan bölüm: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Açık *Login.aspx.cs* arka plan kod ve sarı renkle vurgulanmış kod aşağıdaki satırı açıklamadan kaldırmasına `Page_Load` olay işleyicisi: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Tuşlarına basarak uygulamayı çalıştırma **F5.** Sayfası görüntülendikten sonra tıklayın **oturum** bağlantı.
5. Tıklayın **parolanızı mı unuttunuz?** görüntülemek için bağlantıyı **parolamı unuttum** sayfası.
6. E-posta adresinizi girip __iade **Gönder** bir e-posta yığınlamanız parolanızı sıfırlamak, adresine gönderecek şekilde düğmesi.   
   E-posta hesabınızı kontrol edin ve görüntülemek için bağlantıya tıklayın **parola sıfırlama** sayfası.
7. Üzerinde **parola sıfırlama** , e-posta, parola ve onay parolası girin. Ardından, basın **sıfırlama** düğmesi.  
   Parolanızı başarıyla sıfırladığınızda **parolanın değiştirilmesi** sayfası görüntülenir. Artık yeni parolanızla oturum açabilirsiniz.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>E-posta doğrulama bağlantısını yeniden gönder

Bir kullanıcı yeni bir yerel hesap oluşturulduktan sonra kullanıcılar oturum açmadan önce kullanmak için gerekli onay bağlantısını e-posta gönderilir. Onay e-postası kullanıcının yanlışlıkla siler veya hiç e-posta geldiğinde, bunlar yeniden gönderilen onay bağlantısının gerekir. Aşağıdaki kod değişiklikleri, bunu etkinleştirmek gösterilmektedir.

1. Visual Studio'da açın **Login.aspx.cs** arka plan kod ve sonra aşağıdaki olay işleyicisini eklemek `LogIn` olay işleyicisi:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Değiştirme `LogIn` olay işleyicisinde *Login.aspx.cs* gibi sarı ile vurgulanmış kodu değiştirerek kodu arka plan: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Güncelleştirme *Login.aspx* gibi sarı ile vurgulanmış kodu ekleyerek, sayfa: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Herhangi bir hesabı silme **AspNetUsers** test etmek istediğiniz e-posta diğer adını içeren tablo.
5. Uygulamayı çalıştırın (**F5**) ve e-posta adresinizi kaydedin.
6. Yeni bir hesapla oturum açmak yeni hesabınız yalnızca gönderildiği e-posta yoluyla onaylamadan önce çalışır.  
   Oturum ve onaylanan e-posta hesabına ihtiyacınız vardır görürsünüz. Ayrıca, e-posta hesabınız artık bir onay iletisi yeniden gönderebilirsiniz.
7. E-posta adresi ve parola girmeniz tuşuna **yeniden gönder onay** düğmesi.
8. Yeni gönderilen e-posta iletisini temel alarak e-posta adresinizi doğruladıktan sonra uygulamaya oturum açın.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Uygulama sorunlarını giderme

Kimlik bilgilerinizi doğrulamak için bağlantı içeren bir e-posta almazsanız:

- Önemsiz ya da istenmeyen posta klasörünüzü kontrol edin.
- SendGrid hesabınızda oturum açın ve tıklayarak [e-posta etkinliğinin bağlantı](https://sendgrid.com/logs/index).
- Olun, SendGrid kullanıcı hesabı adı olarak kullanılan belirli bir *Web.config* SendGrid hesabı e-posta adresinizi yerine değeri.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [Bağlantılar ASP.NET Identity önerilen kaynaklar](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Hesap onaylama ve parola kurtarma ASP.NET Identity ile](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Web Forms öğretici serisinin - bir OAuth 2.0 Sağlayıcısı Ekle](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Azure App Service'e bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET Web Forms uygulaması dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Forms öğretici serisinin - projesi için SSL'yi etkinleştir](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
