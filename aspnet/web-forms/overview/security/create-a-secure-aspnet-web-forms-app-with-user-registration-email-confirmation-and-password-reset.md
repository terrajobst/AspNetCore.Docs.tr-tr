---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: "Onay ve parola sıfırlama (C#) e-posta ile kullanıcı kaydı, güvenli bir ASP.NET Web Forms uygulaması oluşturma | Microsoft Docs"
author: Erikre
description: "Bu öğretici, kullanıcı kaydı, e-posta onayı ve parola sıfırlama ASP.NET Identity üye kullanarak bir ASP.NET Web Forms uygulamasının nasıl oluşturulacağını gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: b6f3821a8022daa26f5efcc009ab3e6283a76a19
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Onay ve parola sıfırlama (C#) e-posta ile kullanıcı kaydı, güvenli bir ASP.NET Web Forms uygulaması oluşturma
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

> Bu öğretici, kullanıcı kaydı, e-posta onayı ve parola sıfırlama ASP.NET Identity üyelik sistemini kullanarak bir ASP.NET Web Forms uygulamasının nasıl oluşturulacağını gösterir. Bu öğretici Rick Anderson'un üzerinde temel [MVC Öğreticisi](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Giriş

Bu öğreticide Visual Studio ve ASP.NET 4.5 kullanarak güvenli bir Web Forms uygulaması kullanıcı kaydı oluşturma, onay ve parola sıfırlama e-posta için bir ASP.NET Web Forms uygulama oluşturmak için gereken adımlarda size yol gösterir.

### <a name="tutorial-tasks-and-information"></a>Eğitmen görevleri ve bilgileri:

- [Bir ASP.NET Web uygulaması form uygulama](#createWebForms)
- [SendGrid bağlayın](#SG)
- [Oturum açma önce e-posta onayı iste](#require)
- [Parola kurtarma ve sıfırlama](#reset)
- [E-posta onayı bağlantısını yeniden gönder](#rsend)
- [Uygulama sorunlarını giderme](#dbg)
- [Ek kaynaklar](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Bir ASP.NET Web Forms uygulaması oluşturma

Başlangıç yüklenmesi ve çalıştırılması [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Yükleme [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da daha yüksek de.

> [!NOTE]
> Uyarı: Yüklemelisiniz [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da bu öğreticiyi tamamlamak için daha yüksek.


1. Yeni bir proje oluşturun (**dosya**  - &gt; **yeni proje**) seçip **ASP.NET Web uygulaması** şablonu ve en son .NET Framework sürümünden **yeni proje** iletişim kutusu.
2. Gelen **yeni ASP.NET projesi** iletişim kutusunda **Web Forms** şablonu. Varsayılan kimlik doğrulaması olarak bırakın **tek tek kullanıcı hesaplarını**. Uygulamayı azure'da barındırmak istiyorsanız, bırak **bulutta Barındır** onay kutusu işaretli.   
 Ardından **Tamam** yeni proje oluşturmak için.  
    ![Yeni ASP.NET projesi iletişim kutusu](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Proje için Güvenli Yuva Katmanı (SSL) etkinleştirin. Bulunan adımları **proje için SSL etkinleştir** bölümünü [Web Forms öğretici serisi ile çalışmaya başlama](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Uygulamayı çalıştırın, tıklatın **kaydetmek** bağlamak ve yeni bir kullanıcı kaydedin. Bu noktada, e-posta yalnızca doğrulaması dayanır [[EmailAddress]](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) özniteliği e-posta adresi doğru biçimlendirilmiş olduğundan emin olun. E-posta onayı eklemek için kodu değiştirecektir. Tarayıcı penceresini kapatın.
5. İçinde **Sunucu Gezgini** Visual Studio (**Görünüm**  - &gt; **Sunucu Gezgini**), gitmek **veri Connections\ DefaultConnection\Tables\AspNetUsers**sağ tıklayın ve seçin **açmak tablo tanımı**. 

    Aşağıdaki resimde gösterildiği `AspNetUsers` tablo şemasını:

    ![AspNetUsers tablo şeması](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. İçinde **Sunucu Gezgini**, sağ tıklayın **AspNetUsers** tablo ve seçin **Show Table Data**.  
  
    ![AspNetUsers tablo verileri](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 Bu noktada kayıtlı kullanıcı için e-posta onaylanmamıştır.
7. Kullanıcı tıklayın, satır ve silmek üzere delete seçin. Bu e-posta sonraki adımda yeniden ekleyin ve e-posta adresine bir onay iletisi gönder.

## <a name="email-confirmation"></a>E-posta onayı

Bunlar değil kimliğine bürünmek başkası doğrulamak için yeni bir kullanıcı kaydı sırasında e-postayı onaylamak için en iyi bir uygulamadır (diğer bir deyişle, başka birinin e-posta ile kayıtlı olmayan). Bir tartışma Forumu vardı varsayalım önlemek isteyebilirsiniz `"bob@cpandl.com"` olarak kaydetme gelen `"joe@contoso.com"`. E-posta onaysız `"joe@contoso.com"` istenmeyen e-posta uygulamanızdan alabilir. Bob yanlışlıkla kayıtlı varsayalım `"bib@cpandl.com"` ve bunu fark çalıştırmadıysanız kendisine uygulama doğru e-postasını sahip olmadığından parola kurtarma kullanılacak bağlanamayacak. E-posta onayı aracılarını, yalnızca sınırlı koruma sağlar ve belirlenen istenmeyen posta gönderenlerin koruma sağlamaz.

Genellikle, yeni kullanıcılar Web sitenize herhangi bir veri ya da e-posta ile bir SMS mesajı ya da başka bir mekanizma onaylanmıştır önce nakil önlemek isteyebilirsiniz. Aşağıdaki bölümler size e-posta onayı etkinleştirin ve yeni kayıtlı kullanıcılar kendi e-postasının onaylanıp kadar oturum açmayı önlemek için kodu değiştirin. E-posta hizmeti SendGrid Bu öğreticide kullanacaksınız.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>SendGrid bağlayın

Bu öğretici yalnızca e-posta bildirimi aracılığıyla ekleme gösterir, ancak [SendGrid](http://sendgrid.com/), SMTP ve diğer mekanizmalarını kullanarak e-posta gönderebilirsiniz (bkz [ek kaynaklar](#addRes)).

1. Visual Studio'da açın **Paket Yöneticisi Konsolu** (**Araçları**  - &gt; **NuGet Paket Yöneticisi**  - &gt; **Paket Yöneticisi Konsolu**) ve aşağıdaki komutu girin:  
    `Install-Package SendGrid`
2. Git [Azure SendGrid kayıt sayfasına](https://azure.microsoft.com/en-us/gallery/store/sendgrid/sendgrid-azure/) ve ücretsiz SendGrid hesabı kaydedin. Doğrudan üzerinde bir ücretsiz SendGrid hesabına kaydolun yapabilecekleriniz [SendGrid'ın site](http://www.sendgrid.com).
3. Gelen **Çözüm Gezgini** açmak *IdentityConfig.cs* dosyasını *uygulama\_Başlat* klasörü sarıyavurgulananveaşağıdakikoduekleyin`EmailService` yapılandırmak için sınıf **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Ayrıca, aşağıdakileri ekleyin `using` deyimleri başlangıcına *IdentityConfig.cs* dosyası: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Bu örneği basit tutmak için e-posta hizmeti hesabı değerlerde depolayacağınız `appSettings` bölümünü *web.config* dosya. Sarı vurgulanan aşağıdaki XML eklemek *web.config* projenizi kökündeki dosyası:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Güvenlik - hiçbir zaman deposu gizli verileri kaynak kodu. Bu örnekte, hesabı ve kimlik bilgileri depolanmış **appSetting** bölümünü *Web.config* dosya. Azure üzerinde güvenli bir şekilde bu değerleri üzerinde depolayabileceğiniz  **[yapılandırma](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  Azure portalında sekmesi. İlgili bilgi için başlıklı Rick Anderson'un konusuna [en iyi uygulamalar parolalar ve diğer hassas verileri ASP.NET ve Azure dağıtmak için](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Başarılı olan (kullanıcı adı ve parola) SendGrid kimlik doğrulama değerlerini uygulamanızdan e-posta Gönder yansıtmak üzere e-posta hizmeti değerleri ekleyin. SendGrid sağlanan e-posta adresi yerine SendGrid hesap adınızı kullandığınızdan emin olun.

### <a name="enable-email-confirmation"></a>E-posta onayı etkinleştir

 E-posta Onayı etkinleştirmek için aşağıdaki adımları kullanarak kayıt kodu değiştirmeniz.  
 

1. İçinde *hesap* klasörü, açık *Register.aspx.cs* arka plan kod ve güncelleştirme `CreateUser_Click` yöntemi aşağıdaki vurgulanan değişiklikleri etkinleştirmek için: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. İçinde **Çözüm Gezgini**, sağ *Default.aspx* seçip **başlangıç sayfası olarak ayarla**.
3. Uygulama tuşlarına basarak çalıştırma **F5.** Sayfası görüntülendikten sonra tıklatın **kaydetmek** kayıt sayfasını görüntülemek için bağlantı.
4. E-postanızı ve parolanızı girin ve ardından **kaydetmek** SendGrid aracılığıyla bir e-posta iletisi göndermek için düğmesi.  
 Proje ve kod geçerli durumunu kayıt formunu tamamladıktan sonra bunların hesabının onaylanıp henüz olsa bile oturum açmak kullanıcının izin verir.
5. E-posta hesabınızı denetleyin ve e-postanızı onaylamak için bağlantıyı tıklatın.  
 Kayıt form gönderme sonra oturum açmanız.  
    ![Örnek açtığınız Web sitesi-](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Oturum açma önce e-posta onayı iste

E-posta hesabı onayladıktan karşın, bu noktada, tam olarak açmış olması için doğrulama e-postada yer alan bağlantıyı tıklatın ihtiyaç duymaz. Aşağıdaki bölümde yeni kullanıcılar oturum önce onaylanan bir e-posta olmasını (kimliği doğrulanmış) gerektiren kod değiştirecektir.

1. İçinde **Çözüm Gezgini** Visual Studio güncelleştirme `CreateUser_Click` olayında *Register.aspx.cs* içinde yer alan arka plan kod *hesapları* klasörüyle Aşağıdaki vurgulanan değişiklikler: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Güncelleştirme `LogIn` yönteminde *Login.aspx.cs* arka plan kodu aşağıdaki vurgulanan değişiklikleri: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Uygulamayı çalıştırın

 Bir kullanıcının e-posta adresi onaylanıp onaylanmadığını denetlemek için kod uyguladıysanız, her iki işlevselliği kontrol edebilirsiniz **kaydetmek** ve **oturum açma** sayfaları. 

1. Herhangi bir hesabı silmek **AspNetUsers** test etmek istediğiniz e-posta diğer adını içeren tablo.
2. Uygulamayı çalıştırın (**F5**) ve e-posta adresinizi onayladıktan kadar bir kullanıcı olarak kaydedilemiyor doğrulayın.
3. Yeni hesabınız yalnızca gönderildiği e-posta yoluyla onaylamadan önce yeni bir hesapla oturum açmanız girişimi.  
 Oturum açma sorunu yaşıyor ve onaylanan e-posta hesabı olmalıdır görürsünüz.
4. E-posta adresinizi onaylandıktan sonra uygulamaya oturum açın.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Parola kurtarma ve sıfırlama

1. Visual Studio'da yorum karakterlerinden kaldırmak `Forgot` yönteminde *Forgot.aspx.cs* içinde yer alan arka plan kod *hesap* klasörü, böylece yöntemi olarak görünür izler: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Açık *Login.aspx* sayfası. Biçimlendirme sonlarında yer değiştirme **açılması gerekirken LoginForm'a** bölümünde aşağıda vurgulandığı gibi: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Açık *Login.aspx.cs* arka plan kod ve sarı vurgulanan kod aşağıdaki satırı açıklamadan kaldırmasına `Page_Load` olay işleyicisi: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Uygulama tuşlarına basarak çalıştırma **F5.** Sayfası görüntülendikten sonra tıklatın **oturum** bağlantı.
5. Tıklatın **parolanızı mı unuttunuz?** görüntülemek için bağlantıyı **parolamı unuttum** sayfası.
6. E-posta adresinizi girin ve tıklayın **gönderme** parolanızı sıfırlamak üzere olanak tanıyan, adresine bir e-posta göndermek için düğmesi.   
 E-posta hesabınızı kontrol edin ve görüntülemek için bağlantıyı tıklatın **parola sıfırlama** sayfası.
7. Üzerinde **parola sıfırlama** sayfasında, e-posta, parola ve onay parolası girin. Ardından, basın **sıfırlama** düğmesi.  
 Parolanızı başarıyla sıfırladığınızda **parolanın değiştirilmesi** sayfası görüntülenir. Artık yeni parolanızla oturum açabilirsiniz.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>E-posta onayı bağlantısını yeniden gönder

Kullanıcı yeni bir yerel hesabı oluşturduktan sonra bunların kullanıcılar oturum açmadan önce kullanmak için gerekli onay bağlantısı e-posta gönderilir. Onay e-posta yanlışlıkla kullanıcıyı siler veya e-posta hiçbir zaman ulaştığında, bunlar yeniden gönderilen onay bağlantısı gerekir. Aşağıdaki kod değişikliklerini bunu etkinleştirmek nasıl gösterir.

1. Visual Studio'da açın **Login.aspx.cs** arka plan kod ve sonra aşağıdaki olay işleyicisi ekleme `LogIn` olay işleyicisi:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Değiştirme `LogIn` olay işleyicisini *Login.aspx.cs* sarı ile aşağıdaki gibi vurgulanmış kodu değiştirerek arka plan kod: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Güncelleştirme *Login.aspx* sarı ile aşağıdaki gibi vurgulanmış kodu ekleyerek sayfa: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Herhangi bir hesabı silmek **AspNetUsers** test etmek istediğiniz e-posta diğer adını içeren tablo.
5. Uygulamayı çalıştırın (**F5**) ve e-posta adresinizi kaydedin.
6. Yeni hesabınız yalnızca gönderildiği e-posta yoluyla onaylamadan önce yeni bir hesapla oturum açmanız girişimi.  
 Oturum açma sorunu yaşıyor ve onaylanan e-posta hesabı olmalıdır görürsünüz. Ayrıca, e-posta hesabınız artık bir onay iletisi yeniden gönderebilirsiniz.
7. E-posta adresinizi ve parolanızı girin tuşuna basarak **yeniden onay** düğmesi.
8. E-posta adresinizi yeni gönderilen e-posta iletisini temel alarak onaylandıktan sonra uygulamaya oturum açın.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Uygulama sorunlarını giderme

Kimlik bilgilerinizi doğrulamak için bağlantı içeren bir e-posta almazsanız:

- Önemsiz ya da istenmeyen posta klasörünüzü kontrol edin.
- SendGrid hesabınızda oturum açın ve tıklayın [e-posta etkinliğinin bağlantı](https://sendgrid.com/logs/index).
- SendGrid kullanıcı hesabı adı olarak kullanılan emin olmanız bir *Web.config* SendGrid hesabı e-posta adresinizi yerine değer.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [Önerilen bağlantılar ASP.NET Identity için kaynaklar](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Hesap onaylamayı ve parola kurtarma ASP.NET kimliğe sahip](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Web Forms öğretici serisi - bir OAuth 2.0 Sağlayıcısı Ekle](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Güvenli ASP.NET Web Forms uygulama üyeliği, OAuth ve SQL veritabanı ile Azure App Service'e dağıtma](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Forms öğretici serisi - proje için SSL etkinleştir](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
