---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: E-posta onayı ve parola sıfırlama (C#) oturum açma, güvenli bir ASP.NET MVC 5 web uygulaması oluşturma | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide e-posta onayı ve ASP.NET Identity üyelik sistemini kullanarak parola sıfırlama ile bir ASP.NET MVC 5 web uygulaması oluşturma gösterilmektedir. Ca...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 56c1a5c414fdcece8d827d1187144b4948d8eb93
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387049"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>E-posta onayı ve parola sıfırlama (C#) oturum açma, güvenli bir ASP.NET MVC 5 web uygulaması oluşturma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide e-posta onayı ve ASP.NET Identity üyelik sistemini kullanarak parola sıfırlama ile bir ASP.NET MVC 5 web uygulaması oluşturma gösterilmektedir. Tamamlanmış uygulamayı karşıdan yükleyebileceğiniz [burada](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). İndirilen bir e-posta veya SMS Sağlayıcısı ayarlamadan test e-posta onayı ve SMS olanak tanıyan hata ayıklama Yardımcıları bulunur.
> 
> Bu öğretici tarafından yazılmıştır [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Bir ASP.NET MVC uygulaması oluşturma

Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Yükleme [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) veya üzeri.

> [!NOTE]
> Uyarı: Yüklemelisiniz [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da bu öğreticiyi tamamlamak için daha yüksek.


1. Yeni ASP.NET Web projesi oluşturun ve MVC şablonu seçin. Web Forms da destekler ASP.NET Identity şekilde bir web forms uygulaması benzer adımları izleyebilirsiniz.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Varsayılan kimlik doğrulaması olarak bırakın **bireysel kullanıcı hesapları**. Uygulamayı azure'da barındırmak istiyorsanız, onay kutusunu işaretli bırakın. Öğreticide daha sonra Azure'da dağıtacağız. Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Ayarlama [SSL kullanmak üzere proje](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Uygulamayı çalıştırın, tıklayın **kaydetme** bağlamak ve bir kullanıcı kaydı. Yalnızca e-posta doğrulamasını bu noktada, olan [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) özniteliği.
5. Sunucu Gezgini'nde gidin **veri Connections\DefaultConnection\Tables\AspNetUsers**sağ tıklayın ve seçin **tablo tanımını açın**.

    Aşağıdaki görüntüde `AspNetUsers` şema:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Sağ tıklayın **AspNetUsers** tablosunu seçip **tablo verilerini Göster**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 Bu noktada e-posta onaylanmamıştır.
7. Satırına tıklayın ve Sil'i seçin. Bu e-postayı yeniden sonraki adımda ekleyecek ve bir onay e-posta gönderin.

## <a name="email-confirmation"></a>E-posta onayı

Bunlar değil kimliğe bürünerek başka birisi doğrulamak için yeni bir kullanıcı kaydı e-postayı onaylamak için iyi bir uygulamadır (diğer bir deyişle, bunlar başka birinin e-posta ile kayıtlı olmayabilirsiniz). Tartışma Forumu tablonuz olduğunu varsayın engellemek istiyorsunuz `"bob@example.com"` olarak kaydetme gelen `"joe@contoso.com"`. E-posta onayı olmadan `"joe@contoso.com"` uygulamanızdan istenmeyen e-posta alabilir. Bob yanlışlıkla kayıtlı varsayalım `"bib@example.com"` ve bunu fark yüklediniz o uygulamayı doğru e-postasını olmadığı için parola kurtarma kullanın saptayamazdınız. E-posta onayı robotlar yalnızca sınırlı koruma sağlar ve belirlenen istenmeyen posta gönderenlere koruma sağlamaz, sahip oldukları çok sayıda çalışan e-posta diğer adlar kaydetmek için kullanabilirsiniz.

Genellikle, yeni kullanıcıların e-posta, SMS mesajı ya da başka bir mekanizma onaylanmıştır önce web sitenizi herhangi bir veri gönderme engellemek istiyorsunuz. <a id="build"></a>Aşağıdaki bölümler size e-posta onayı etkinleştirin ve yeni kaydettiğiniz kullanıcıların e-postasına onaylanana kadar oturum açmayı engellemek için kodu değiştirin.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>SendGrid bağlama

Bu öğreticide yalnızca e-posta bildirimi aracılığıyla ekleme gösterir, ancak [SendGrid](http://sendgrid.com/), e-posta SMTP veya başka mekanizmalar kullanılarak gönderebilirsiniz (bkz [ek kaynaklar](#addRes)).

1. Paket Yöneticisi konsolunda aşağıdaki komutu girin: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Git [Azure SendGrid kayıt sayfasını](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) ve ücretsiz bir SendGrid hesabı için kaydolun. SendGrid, aşağıdakine benzer bir kod ekleyerek yapılandırma *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Aşağıdakileri içeren eklemeniz gerekir:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Bu örneği basit tutmak için biz uygulama ayarlarında depolayacağınızı *web.config* dosyası:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki. Kimlik ve hesap appSetting içinde depolanır. Azure üzerinde güvenli bir şekilde bu değerleri üzerinde depolamanın **[yapılandırma](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portalında sekmesi. Bkz: [parolalar ve diğer hassas verileri ASP.NET ve Azure'a dağıtmak için en iyi yöntemler](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>E-posta onayı hesabı denetleyicisindeki etkinleştir

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Doğrulama *Views\Account\ConfirmEmail.cshtml* dosyasının doğru razor sözdizimi vardır. (@ Karakteri ilk satırı eksik olabilir. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Uygulamayı çalıştırın ve Kaydet bağlantısına tıklayın. Kayıt formunu gönderildikten sonra oturum açtınız.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

E-posta hesabınızı denetleyin ve e-postanızı doğrulamak için bağlantıya tıklayın.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Oturum açma önce e-posta onayı gerektir

Şu anda bir kullanıcı kayıt formunu tamamladıktan sonra kullanıcılar oturum açtınız. Genellikle bunları oturum açmadan önce e-postasına doğrulamak istersiniz. Aşağıdaki bölümde, biz yeni kullanıcıların oturum önce onaylanan bir e-posta için (kimlik doğrulaması) için kodu değiştirin. Güncelleştirme `HttpPost Register` vurgulanan aşağıdaki değişikliklerle birlikte yöntemi:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Çıkış yorum tarafından `SignInAsync` yöntemi, kullanıcının kaydı tarafından oturumunuz açılacak değil. `TempData["ViewBagLink"] = callbackUrl;` Satır için kullanılabilir [uygulamasında hata ayıklama](#dbg) ve kayıt e-posta göndermeden test edin. `ViewBag.Message` Onayla yönergeleri görüntülemek için kullanılır. [Örneği indirin](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) e-posta onayı e-posta ayarı olmadan test etmek için kodu içerir ve uygulamada hata ayıklamak için de kullanılabilir.

Oluşturma bir `Views\Shared\Info.cshtml` dosyasını açıp aşağıdaki razor işaretlemesi ekleyin:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Ekleme [Authorize özniteliği](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) için `Contact` giriş denetleyici eylem yöntemi. Tıklayabilirsiniz **kişi** anonim kullanıcılar, erişimi yok ve kimliği doğrulanmış kullanıcılara erişimi doğrulamak için bağlantı.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Aynı zamanda güncelleştirmelisiniz `HttpPost Login` eylem yöntemi:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Güncelleştirme *Views\Shared\Error.cshtml* hata iletisini görüntülemek için görünümü:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Herhangi bir hesabı silme **AspNetUsers** test etmek istediğiniz e-posta diğer adı içeren tablo. Uygulamayı çalıştırın ve e-posta adresinizi doğrulayıncaya kadar oturum açamıyorsanız doğrulayın. E-posta adresinizi doğruladıktan sonra tıklayın **kişi** bağlantı.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Parola kurtarma/sıfırlama

Açıklama karakterleri Kaldır `HttpPost ForgotPassword` hesabı denetleyicideki eylem yöntemi:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Açıklama karakterleri Kaldır `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) içinde *Views\Account\Login.cshtml* razor görünüm dosyası:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Oturum açma sayfasında artık parola sıfırlama bağlantısı gösterilir.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>E-posta doğrulama bağlantısını yeniden gönder

Bir kullanıcı yeni bir yerel hesap oluşturulduktan sonra kullanıcılar oturum açmadan önce kullanmak için gerekli onay bağlantısını e-posta gönderilir. Onay e-postası kullanıcının yanlışlıkla siler veya hiç e-posta geldiğinde, bunlar yeniden gönderilen onay bağlantısının gerekir. Aşağıdaki kod değişiklikleri, bunu etkinleştirmek gösterilmektedir.

Aşağıdaki yardımcı yöntemini bölmenizin altına eklemek *Controllers\AccountController.cs* dosyası:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Güncelleştirme kayıt yöntemi, yeni Yardımcısı'nı kullanmak için:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Kullanıcı hesabının onaylanıp değil ise parolayı yeniden göndermek için oturum açma yöntemi güncelleştirin:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Sosyal ve yerel oturum açma hesabını birleştirin

Yerel hem de sosyal hesaplar, e-posta bağlantısına tıklayarak birleştirebilirsiniz. Aşağıdaki sırayla **RickAndMSFT@gmail.com** yerel oturum açma oluşturulur, ancak bir sosyal günlük ilk olarak hesabı oluşturun, ardından bir yerel oturum açma ekleyin.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Tıklayarak **Yönet** bağlantı. Not **harici oturum açmaları: 0** bu hesapla ilişkilendirilmiş.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Hizmet başka bir oturum açmak için bağlantıya tıklayın ve uygulama istekleri kabul edin. İki hesap birleştirilmiş, herhangi bir hesabı ile oturum açamayacaktır. Kullanıcılarınız, sosyal kimlik doğrulama hizmeti günlüğü kullanılamıyor veya bunlar sosyal hesaplarına erişim daha büyük bir olasılıkla kaybetmiş durumunda yerel hesapları eklemek isteyebilirsiniz.

Aşağıdaki resimde, bir sosyal oturum açma Tom olduğu (görebileceğiniz gibi **harici oturum açmaları: 1** sayfasında gösterilen).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Tıklayarak **bir parola seçin** , yerel bir günlük eklemek aynı hesabı ile ilişkili sağlar.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Daha fazla ayrıntılı e-posta onayı

My öğretici [hesap onaylama ve parola kurtarma ASP.NET Identity ile](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) bu konuda daha fazla ayrıntı içeren geçerlidir.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Uygulamayı hata ayıklama

Bağlantıyı içeren bir e-posta alamazsanız:

- Önemsiz ya da istenmeyen posta klasörünüzü kontrol edin.
- SendGrid hesabınızda oturum açın ve tıklayarak [e-posta etkinliğinin bağlantı](https://sendgrid.com/logs/index).

E-posta olmadan doğrulama bağlantısını test etmek için Yükle [tamamlanan örnek](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Onay bağlantısı ve doğrulama kodları sayfada görüntülenir.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [Bağlantılar ASP.NET Identity önerilen kaynaklar](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Onaylama ve parola kurtarma ASP.NET Identity ile hesap](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) parola kurtarma ve Hesap doğrulama hakkında daha fazla ayrıntıya gider.
- [Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma ile MVC 5 uygulaması](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Bu öğreticide bir ASP.NET MVC 5 uygulaması Facebook ve Google OAuth 2 yetkilendirmesiyle yazma işlemi gösterilmektedir. Ayrıca kimlik veritabanı için ek veri ekleme işlemini de gösterir.
- [Membership, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC uygulaması Azure'da dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Bu öğreticide Azure dağıtım ekler rolleri ile uygulamanızı güvenli hale getirmeyi kullanıcılar ve roller ve ek güvenlik özellikleri eklemek için üyelik API'si kullanma.
- [OAuth 2 için Google uygulaması oluşturma ve uygulama projesine bağlanma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Facebook uygulama oluşturma ve uygulama projesine bağlanma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Projedeki SSL ayarlama](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
