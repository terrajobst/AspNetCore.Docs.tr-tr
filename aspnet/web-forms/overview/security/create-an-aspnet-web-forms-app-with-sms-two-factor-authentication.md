---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: "Bir ASP.NET Web oluşturmak Forms uygulama SMS iki faktörlü kimlik doğrulaması (C#) ile | Microsoft Docs"
author: Erikre
description: "Bu öğretici, iki öğeli kimlik doğrulama ile ASP.NET Web Forms uygulamasının nasıl oluşturulacağını gösterir. Bu öğretici Cr başlıklı öğreticiyi tamamlamak üzere tasarlanmıştır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 92ab5f2d7a9a29089f3d340849e229d015613509
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Bir ASP.NET Web oluşturmak Forms uygulamayla SMS iki faktörlü kimlik doğrulaması (C#)
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

[E-posta ve SMS iki faktörlü kimlik doğrulaması ile ASP.NET Web Forms uygulamasını indir](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Bu öğretici, iki öğeli kimlik doğrulama ile ASP.NET Web Forms uygulamasının nasıl oluşturulacağını gösterir. Bu öğretici başlıklı öğreticiyi tamamlamak üzere tasarlanmıştır [kullanıcı kaydı ile güvenli bir ASP.NET Web Forms uygulaması oluşturma, onay ve parola sıfırlama e-posta](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Ayrıca, Bu öğretici Rick Anderson'un üzerinde temel [MVC Öğreticisi](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Giriş

Bu öğreticide Visual Studio kullanarak iki faktörlü kimlik doğrulamasını destekleyen bir ASP.NET Web Forms uygulama oluşturmak için gereken adımlarda size yol gösterir. İki öğeli kimlik doğrulama ek kullanıcı kimlik doğrulaması adımdır. Bu ek adım, oturum açma sırasında bir benzersiz bir kişisel kimlik numarası (PIN) oluşturur. PIN, genellikle kullanıcıya bir e-posta veya SMS mesajı olarak gönderilir. Kullanıcı, uygulamanızın ardından PIN bir ek kimlik doğrulaması ölçü olarak oturum açma girer.

### <a name="tutorial-tasks-and-information"></a>Eğitmen görevleri ve bilgileri:

- [Bir ASP.NET Web Forms uygulaması oluşturma](#createWebForms)
- [SMS ve iki öğeli kimlik doğrulama Kurulumu](#SMS)
- [Kayıtlı kullanıcı için iki faktörlü kimlik doğrulamasını etkinleştirin](#use2FA)
- [Ek kaynaklar](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Bir ASP.NET Web Forms uygulaması oluşturma

Başlangıç yüklenmesi ve çalıştırılması [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Yükleme [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da daha yüksek de. Ayrıca, oluşturmanız gerekecektir bir [Twilio](https://www.twilio.com/try-twilio) , aşağıda açıklandığı gibi hesap.

> [!NOTE]
> Önemli: Yüklemelisiniz [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da bu öğreticiyi tamamlamak için daha yüksek.


1. Yeni bir proje oluşturun (**dosya**  - &gt; **yeni proje**) seçip **ASP.NET Web uygulaması** .NET Framework ile birlikte şablonu 4.5.2 sürümünden **yeni proje** iletişim kutusu.
2. Gelen **yeni ASP.NET projesi** iletişim kutusunda **Web Forms** şablonu. Varsayılan kimlik doğrulaması olarak bırakın **tek tek kullanıcı hesaplarını**. Ardından **Tamam** yeni proje oluşturmak için.  
    ![Yeni ASP.NET projesi iletişim kutusu](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Proje için Güvenli Yuva Katmanı (SSL) etkinleştirin. Bulunan adımları **proje için SSL etkinleştir** bölümünü [Web Forms öğretici serisi ile çalışmaya başlama](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Visual Studio'da açın **Paket Yöneticisi Konsolu** (**Araçları**  - &gt; **NuGet Paket Yöneticisi**  - &gt; **Paket Yöneticisi Konsolu**) ve aşağıdaki komutu girin:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>SMS ve iki öğeli kimlik doğrulama Kurulumu

Bu öğretici Twilio kullanır, ancak herhangi bir SMS sağlayıcıyı kullanabilirsiniz.

1. Oluşturma bir [Twilio](https://www.twilio.com/try-twilio) hesabı.
2. Gelen **Pano** Twilio hesabınızı kopyalama sekmesinde **hesabının SID** ve **kimlik doğrulama belirteci.** Bunları uygulamanıza daha sonra ekleyeceksiniz.
3. Gelen **numaraları** sekmesinde, sizin Twilio Kopyala **telefon numarası** de.
4. Twilio olun **hesabının SID**, **kimlik doğrulama belirteci** ve **telefon numarası** uygulama için kullanılabilir. Örneği basit tutmak için bu değerleri depolayacak *web.config* dosya. Azure'a dağıtırken değerlerin güvenli bir şekilde içinde saklayabilir **appSettings** bölüm web sitesinde yapılandırma sekmesi. Ayrıca, telefon numarası eklerken, yalnızca numaralarını kullanın.   
 SendGrid kimlik bilgileri de ekleyebilirsiniz dikkat edin. SendGrid bir e-posta bildirim hizmetidir. SendGrid, etkinleştirme hakkında ayrıntıları görmek için başlıklı öğreticinin 'Kanca yukarı SendGrid' bölümünde [kullanıcı kaydı Güvenli ASP.NET Web Forms uygulaması oluşturma, onay ve parola sıfırlama e-posta.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Güvenlik - hiçbir zaman deposu gizli verileri kaynak kodu. Bu örnekte, hesabı ve kimlik bilgileri depolanmış **appSettings** bölümünü *Web.config* dosya. Azure üzerinde güvenli bir şekilde bu değerleri üzerinde depolayabileceğiniz  **[yapılandırma](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  Azure portalında sekmesi. İlgili bilgi için başlıklı Rick Anderson'un konusuna [en iyi uygulamalar parolalar ve diğer hassas verileri ASP.NET ve Azure dağıtmak için](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Yapılandırma `SmsService` sınıfını *uygulama\_Start\IdentityConfig.cs* aşağıdakileri yaparak dosyası vurgulanmış sarı değiştirir: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Aşağıdakileri ekleyin `using` deyimleri başlangıcına *IdentityConfig.cs* dosyası: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Güncelleştirme *Account/Manage.aspx* sarı ile vurgulanmış satırları kaldırarak dosya:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. İçinde `Page_Load` işleyicisine *Manage.aspx.cs* arka plan kodu, bu BT olarak görünmesi sarı ile vurgulanan kod satırını izleyen açıklamadan çıkarın: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. İçinde arkasındaki koda *hesap*/*TwoFactorAuthenticationSignIn.aspx.cs*, güncelleştirme `Page_Load` sarı ile vurgulanmış aşağıdaki kodu ekleyerek işleyici: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

 Yukarıdaki kodunu değiştir yaparak, kimlik doğrulama seçeneklerini içeren "Sağlayıcı" DropDownList ilk değerine sıfırlanır değil. Bu, kullanıcının kimlik doğrulamasını yaparken, yalnızca ilk kullanmak için tüm seçenekleri başarıyla seçmesine izin verir.
10. İçinde **Çözüm Gezgini**, sağ *Default.aspx* seçip **başlangıç sayfası olarak ayarla**.
11. Uygulamanızı test ederek ilk uygulaması oluşturma (**Ctrl**+**Shift**+**B**) ve ardından uygulamayı çalıştırın (**F5**) ve Her iki select **kaydetmek** yeni bir kullanıcı hesabı oluşturun veya seçin **oturum** kullanıcı hesabı zaten kayıtlı.
12. (Kullanıcı) olarak oturum açtıktan sonra kullanıcı kimliği (e-posta adresi) görüntülemek için Gezinti Çubuğu'nu tıklatın **hesabı Yönet** sayfa (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Tıklatın **Ekle** yanına **telefon numarası** üzerinde **hesabı Yönet** sayfası.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Burada (kullanıcı) olarak istediğiniz SMS iletileri (metin iletileri) almak ve tıklayın telefon numarası eklemek **gönderme** düğmesi.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
 Bu noktada, uygulama kimlik bilgilerini kullanır *Web.config* Twilio başvurma. Kullanıcı hesabı ile ilişkili telefon SMS iletisi (SMS) gönderilir. Twilio panoyu görüntüleyerek Twilio iletinin gönderildiği doğrulayabilirsiniz.
15. Birkaç saniye içinde kullanıcı hesabı ile ilişkili telefon doğrulama kodunu içeren bir kısa mesaj alırsınız. Tuşuna basın ve doğrulama kodu girin **gönderme**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Kayıtlı bir kullanıcı için iki faktörlü kimlik doğrulamasını etkinleştirin

Bu noktada, iki öğeli kimlik doğrulama uygulamanız için etkinleştirdiniz. Bir kullanıcı iki öğeli kimlik doğrulamasını kullanmak, bunlar yalnızca kullanıcı arabirimini kullanarak ayarlarını değiştirebilirsiniz. 

1. Uygulamanızı bir kullanıcı olarak, iki öğeli kimlik doğrulama belirli hesabınız için kullanıcı kimliği (e-posta diğer adı) tıklayarak görüntülemek için gezinti çubuğunda etkinleştirebilirsiniz **hesabı Yönet** sayfası. Ardından, tıklatın **etkinleştirmek** hesap iki faktörlü kimlik doğrulamasını etkinleştirmek için bağlantı.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Oturumu kapatın, sonra tekrar açın. E-posta etkinleştirdiyseniz, SMS veya e-posta iki faktörlü kimlik doğrulaması için seçebilirsiniz. E-posta etkinleştirmediyseniz başlıklı öğretici bkz [kullanıcı kaydı, e-posta onayı ve parola sıfırlama ile Güvenli ASP.NET Web Forms uygulaması oluşturma](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. İki öğeli kimlik doğrulama sayfasında, kod (SMS veya e-posta) girebileceğiniz görüntülenir.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Tıklayarak **bu tarayıcı unutmayın** onay kutusunu muaf, tarayıcı ve cihaz Burada onay kutusunun seçili kullanırken oturum açmak için iki öğeli kimlik doğrulama kullanmanıza gerek. Kötü niyetli kullanıcılar Cihazınızı erişim sağlayamadığı sürece, iki faktörlü kimlik doğrulamasını etkinleştirme ve tıklayarak **bu tarayıcı unutmayın** hala strong korurken kullanışlı bir adım parola erişimle sağlayacaktır güvenilir olmayan cihazlardan tüm erişim için iki öğeli kimlik doğrulama koruma. Düzenli olarak kullanmak istediğiniz özel cihazda bunu yapabilirsiniz.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [SMS ve e-posta ile ASP.NET Identity kullanan iki öğeli kimlik doğrulaması](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Önerilen bağlantılar ASP.NET Identity için kaynaklar](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Azure Web sitesini Güvenli ASP.NET Web Forms uygulama üyeliği, OAuth ve SQL veritabanı ile dağıtma](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Forms öğretici serisi - bir OAuth 2.0 Sağlayıcısı Ekle](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web Forms öğretici serisi - proje için SSL etkinleştir](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Hesap onaylamayı ve parola kurtarma ASP.NET kimliğe sahip](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [İçinde Facebook uygulaması oluşturma ve uygulama projesine bağlanma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
