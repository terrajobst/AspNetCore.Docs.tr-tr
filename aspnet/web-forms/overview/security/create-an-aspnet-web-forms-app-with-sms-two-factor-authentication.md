---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Oluşturma bir ASP.NET Web Forms uygulaması SMS iki öğeli kimlik doğrulama (C#) ile | Microsoft Docs
author: Erikre
description: Bu öğreticide iki öğeli kimlik doğrulaması ile bir ASP.NET Web formları uygulamasının nasıl oluşturulacağını gösterir. Bu öğreticide, Cr başlıklı öğreticiyi tamamlamak üzere tasarlanmıştır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1231ed0cb238ad35c12405dd9a264ccfd7332960
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401298"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Oluşturma bir ASP.NET Web Forms uygulaması ile SMS iki öğeli kimlik doğrulama (C#)
====================
tarafından [Erik Reitan](https://github.com/Erikre)

[E-posta ve SMS iki öğeli kimlik doğrulaması ile ASP.NET Web Forms uygulaması'nı indirin](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Bu öğreticide iki öğeli kimlik doğrulaması ile bir ASP.NET Web formları uygulamasının nasıl oluşturulacağını gösterir. Bu öğreticide başlıklı öğreticiyi tamamlamak için tasarlanmış [kullanıcı kaydı ile güvenli bir ASP.NET Web Forms uygulaması oluşturma, e-posta onayı ve parola sıfırlama](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Ayrıca, bu öğreticide Rick Anderson'un üzerinde temel [MVC Öğreticisi](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Giriş

Bu öğreticide, iki öğeli Visual Studio kullanarak kimlik doğrulamasını destekleyen bir ASP.NET Web Forms uygulaması oluşturmak için gereken adımlarda size kılavuzluk eder. İki öğeli kimlik doğrulama ek kullanıcı kimlik doğrulaması adımdır. Bu ek adım, oturum açma sırasında benzersiz bir kişisel kimlik numarası (PIN) oluşturur. PIN, kullanıcıya bir e-posta veya SMS mesajı olarak yaygın olarak gönderilir. Uygulamanızın kullanıcı ardından PIN bir ek kimlik doğrulaması önlemi olarak oturum açma girer.

### <a name="tutorial-tasks-and-information"></a>Öğretici görevleri ve bilgileri:

- [Bir ASP.NET Web Forms uygulaması oluşturma](#createWebForms)
- [SMS ve iki öğeli kimlik doğrulaması kurulumu](#SMS)
- [Kayıtlı kullanıcı için iki öğeli kimlik doğrulamasını etkinleştirme](#use2FA)
- [Ek kaynaklar](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Bir ASP.NET Web Forms uygulaması oluşturma

Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Yükleme [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) veya üzeri de. Ayrıca, oluşturmanız gerekecektir bir [Twilio](https://www.twilio.com/try-twilio) aşağıda açıklandığı gibi hesap.

> [!NOTE]
> Önemli: Yüklemelisiniz [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da bu öğreticiyi tamamlamak için daha yüksek.


1. Yeni bir proje oluşturun (**dosya**  - &gt; **yeni proje**) seçip **ASP.NET Web uygulaması** .NET Framework ile birlikte şablonu 4.5.2 sürümünden **yeni proje** iletişim kutusu.
2. Gelen **yeni ASP.NET projesi** iletişim kutusunda **Web Forms** şablonu. Varsayılan kimlik doğrulaması olarak bırakın **bireysel kullanıcı hesapları**. ' A tıklayarak **Tamam** yeni projeyi oluşturmak için.  
    ![Yeni ASP.NET projesi iletişim kutusu](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Proje için Güvenli Yuva Katmanı (SSL) etkinleştirin. Bulunan adımları **projesi için SSL'yi etkinleştir** bölümünü [öğretici serisinin Web Forms ile çalışmaya başlama](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Visual Studio'da açın **Paket Yöneticisi Konsolu** (**Araçları**  - &gt; **NuGet Paket Yöneticisi**  - &gt; **Paket Yöneticisi Konsolu**) ve aşağıdaki komutu girin:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>SMS ve iki öğeli kimlik doğrulaması kurulumu

Bu öğreticide, Twilio kullanılır, ancak herhangi bir SMS sağlayıcı kullanabilirsiniz.

1. Oluşturma bir [Twilio](https://www.twilio.com/try-twilio) hesabı.
2. Gelen **Pano** sekmesini Twilio hesabınızın kopyalama **hesap SID'si** ve **kimlik doğrulama belirteci.** Uygulamanıza daha sonra eklediğiniz.
3. Gelen **numaraları** sekmesinde, kopyalayın, Twilio **telefon numarası** de.
4. Twilio olun **hesap SID'si**, **kimlik doğrulama belirteci** ve **telefon numarası** uygulama tarafından kullanılabilir. Basit bir anlatım gözetildiği için bu değerleri depolayacak *web.config* dosya. Azure'a dağıtırken, güvenli bir şekilde giriş değerleri depolayabilir **appSettings** bölümü web sitesinde yapılandırma sekmesi. Ayrıca, telefon numarası eklerken, yalnızca numaralarını kullanın.   
   SendGrid kimlik bilgileri de ekleyebilirsiniz dikkat edin. SendGrid e-posta bildirim hizmetidir. SendGrid, etkinleştirme hakkında bilgi için başlıklı öğreticiyi 'Kurmak SendGrid kanca' bölümünü [e-posta onayı ve parola sıfırlama, kullanıcı kaydı ile Güvenli ASP.NET Web Forms uygulaması oluşturun.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki. Bu örnekte ve hesabın kimlik bilgileri depolanır **appSettings** bölümünü *Web.config* dosya. Azure üzerinde güvenli bir şekilde bu değerleri üzerinde depolamanın **[yapılandırma](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portalında sekmesi. İlgili bilgi için başlıklı Rick Anderson'un konusuna [parolalar ve diğer hassas verileri ASP.NET ve Azure'a dağıtmak için en iyi yöntemler](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Yapılandırma `SmsService` sınıfını *uygulama\_Start\IdentityConfig.cs* aşağıdakileri yaparak dosyası vurgulanmış sarı değiştirir: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Aşağıdaki `using` başlangıcına deyimleri *IdentityConfig.cs* dosyası: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Güncelleştirme *Account/Manage.aspx* sarı ile vurgulanmış satırları kaldırarak dosya:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. İçinde `Page_Load` işleyicisi *Manage.aspx.cs* arka plan kod, aşağıdaki şekilde olarak görünmesi, sarı ile vurgulanmış kod satırının açıklamalarını kaldırın: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. Codebehind içinde *hesabı*/*TwoFactorAuthenticationSignIn.aspx.cs*, güncelleştirme `Page_Load` sarı ile vurgulanmış aşağıdaki kodu ekleyerek işleyicisi: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Yukarıdaki kodu değiştirmek yaparak, kimlik doğrulama seçeneklerini içeren "Sağlayıcı" DropDownList ilk değerine sıfırlanır değil. Bu, kullanıcı başarıyla kimlik doğrulaması yapılırken yalnızca ilk tüm seçeneklerini belirlemek üzere izin verir.
10. İçinde **Çözüm Gezgini**, sağ *Default.aspx* seçip **başlangıç sayfası olarak ayarla**.
11. Uygulamanızı test ederek, ilk uygulama oluşturma (**Ctrl**+**Shift**+**B**) ve ardından uygulamayı çalıştırın (**F5**) ve yi **kaydetme** yeni bir kullanıcı hesabı oluşturun veya seçin **oturum** kullanıcı hesabı zaten kayıtlı.
12. (Kullanıcı) olarak oturum açtıktan sonra kullanıcı Kimliğine (e-posta adresi) görüntülemek için gezinti çubuğunda tıklatın **hesabı Yönet** sayfa (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Tıklayın **Ekle** yanındaki **telefon numarası** üzerinde **hesabı Yönet** sayfası.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. (Kullanıcı) olarak istediğiniz SMS mesajları (kısa mesaj) ve telefon numarası ekleyin **Gönder** düğmesi.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    Bu noktada, uygulama kimlik bilgilerinden kullanır *Web.config* Twilio başvurma. Kullanıcı hesabı ile ilişkili telefon (kısa mesaj) SMS mesajı gönderilir. Twilio Panosu görüntüleyerek Twilio ileti gönderilip gönderilmediğini de doğrulayabilirsiniz.
15. Birkaç saniye içinde kullanıcı hesabı ile ilişkili telefon doğrulama kodunu içeren bir kısa mesaj alırsınız. Doğrulama kodunu ve enter tuşuna basın **Gönder**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Kayıtlı kullanıcı için iki öğeli kimlik doğrulamasını etkinleştirme

Bu noktada, iki öğeli kimlik doğrulaması için uygulamanızı etkinleştirdiniz. Bir kullanıcı iki öğeli kimlik doğrulamasını kullanmak bunlar yalnızca kullanıcı arabirimini kullanarak ayarları değiştirebilirsiniz. 

1. Uygulamanızın bir kullanıcı olarak, iki öğeli kimlik doğrulama belirli hesabınız için kullanıcı kimliği (e-posta diğer adı) tıklayarak görüntülemek için gezinti çubuğunda etkinleştirebilirsiniz **hesabı Yönet** sayfası. Ardından **etkinleştirme** hesabı için iki öğeli kimlik doğrulamasını etkinleştirmek için bağlantı.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Oturumu kapatın ve yeniden oturum açın. E-posta etkinleştirdiyseniz, SMS veya e-posta iki öğeli kimlik doğrulama için seçebilirsiniz. E-posta etkinleştirmediyseniz, başlıklı öğreticiyi bkz [kullanıcı kaydı, e-posta onayı ve parola sıfırlama ile Güvenli ASP.NET Web Forms uygulaması oluşturma](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Kod (SMS veya e-posta) girebileceğiniz iki öğeli kimlik doğrulaması sayfası görüntülenir.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Tıklayarak **bu tarayıcı Hatırlansın** onay kutusunu muaf, cihaz ve tarayıcı onay kutusunun seçili olduğu kullanırken oturum açmak için iki öğeli kimlik doğrulamayı kullanmaya gerek. Kötü amaçlı kullanıcılar cihazınıza erişemezler sürece, iki öğeli kimlik doğrulamayı etkinleştirme ve tıklayarak **bu tarayıcı Hatırlansın** hala strong korurken kullanışlı bir adım parola erişimle sağlayacaktır. güvenilir olmayan cihazlardan tüm erişimi için iki öğeli kimlik doğrulama koruma. Düzenli olarak kullandığınız özel cihaz üzerinde bunu yapabilirsiniz.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Identity ile SMS ve e-posta kullanılan iki öğeli kimlik doğrulaması](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Bağlantılar ASP.NET Identity önerilen kaynaklar](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Bir Azure Web sitesine bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET Web Forms uygulaması dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Forms öğretici serisinin - bir OAuth 2.0 Sağlayıcısı Ekle](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web Forms öğretici serisinin - projesi için SSL'yi etkinleştir](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Hesap onaylama ve parola kurtarma ASP.NET Identity ile](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Facebook uygulama oluşturma ve uygulama projesine bağlanma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
