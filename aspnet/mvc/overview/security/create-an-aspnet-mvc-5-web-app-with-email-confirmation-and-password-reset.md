---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Onay ve parola sıfırlama (C#) e-posta ile oturum açma, Güvenli ASP.NET MVC 5 web uygulaması oluşturma | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici, e-posta onayı ve parola sıfırlama ASP.NET Identity üyelik sistemini kullanarak bir ASP.NET MVC 5 web uygulamasının nasıl oluşturulacağını gösterir. Ca...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: bfa5d52019be81374c7a544e255ab7ffb301fa7b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "34452574"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Onay ve parola sıfırlama (C#) e-posta ile oturum açma, Güvenli ASP.NET MVC 5 web uygulaması oluşturma
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici, e-posta onayı ve parola sıfırlama ASP.NET Identity üyelik sistemini kullanarak bir ASP.NET MVC 5 web uygulamasının nasıl oluşturulacağını gösterir. Tamamlanan uygulama indirebilirsiniz [burada](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Yükleme, bir e-posta veya SMS Sağlayıcısı ayarlamadan test e-posta onayı ve SMS izin hata ayıklama Yardımcıları içerir.
> 
> Bu öğretici tarafından yazıldı [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Bir ASP.NET MVC uygulaması oluşturma

Başlangıç yüklenmesi ve çalıştırılması [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Yükleme [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da daha yüksek.

> [!NOTE]
> Uyarı: Yüklemelisiniz [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da bu öğreticiyi tamamlamak için daha yüksek.


1. Yeni bir ASP.NET Web projesi oluşturun ve MVC şablonunu seçin. Web Forms da destekler ASP.NET kimliği için bir web forms uygulamasında benzer adımları izleyebilirsiniz.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Varsayılan kimlik doğrulaması olarak bırakın **tek tek kullanıcı hesaplarını**. Uygulamayı azure'da barındırmak istiyorsanız, onay kutusunun işaretli bırakın. Daha sonra öğreticide biz Azure'a dağıtacaksınız. Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Ayarlama [SSL kullanmak üzere proje](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Uygulamayı çalıştırın, tıklatın **kaydetmek** bağlantı ve bir kullanıcı kaydı. Bu noktada, yalnızca doğrulama e-posta ile olan [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) özniteliği.
5. Server Explorer'da gidin **veri Connections\DefaultConnection\Tables\AspNetUsers**sağ tıklayın ve seçin **açmak tablo tanımı**.

    Aşağıdaki resimde gösterildiği `AspNetUsers` şeması:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Sağ tıklayın **AspNetUsers** tablo ve seçin **Show Table Data**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 Bu noktada e-posta onaylanmamıştır.
7. Satırındaki'ı tıklatın ve Sil'i seçin. Bu e-posta sonraki adımda yeniden ekleyin ve bir onay e-posta gönder.

## <a name="email-confirmation"></a>E-posta onayı

Bunlar değil kimliğine bürünmek başkası doğrulamak için yeni bir kullanıcı kaydı e-posta onaylamak için en iyi bir uygulamadır (diğer bir deyişle, başka birinin e-posta ile kayıtlı olmayan). Bir tartışma Forumu vardı varsayalım önlemek isteyebilirsiniz `"bob@example.com"` olarak kaydetme gelen `"joe@contoso.com"`. E-posta onaysız `"joe@contoso.com"` istenmeyen e-posta uygulamanızdan alabilir. Bob yanlışlıkla kayıtlı varsayalım `"bib@example.com"` ve bunu fark çalıştırmadıysanız kendisine uygulama doğru e-postasını sahip olmadığından parola kurtarma kullanmak bağlanamayacak. E-posta onayı aracılarını, yalnızca sınırlı koruma sağlar ve koruma belirlendiği istenmeyen posta gönderenlerin sağlamaz, sahip oldukları çok sayıda çalışan e-posta diğer adlar kaydetmek için kullanabilirsiniz.

Genellikle, yeni kullanıcılar e-posta ile bir SMS metin iletisi ya da başka bir mekanizma onaylanmıştır önce web siteniz için herhangi bir veri gönderme önlemek isteyebilirsiniz. <a id="build"></a>Aşağıdaki bölümler size e-posta onayı etkinleştirin ve yeni kayıtlı kullanıcılar kendi e-postasının onaylanıp kadar oturum açmayı önlemek için kodu değiştirin.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>SendGrid bağlayın

Bu öğretici yalnızca e-posta bildirimi aracılığıyla ekleme gösterir, ancak [SendGrid](http://sendgrid.com/), SMTP ve diğer mekanizmalarını kullanarak e-posta gönderebilirsiniz (bkz [ek kaynaklar](#addRes)).

1. Paket Yöneticisi konsolunda aşağıdaki komutu girin: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Git [Azure SendGrid kayıt sayfasını](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) ve boş bir SendGrid hesabına kaydolun. SendGrid aşağıdakine benzer bir kod ekleyerek yapılandırma *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Aşağıdakileri içeren eklemeniz gerekir:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Bu örneği basit tutmak için uygulama ayarlarında depolarız *web.config* dosyası:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Güvenlik - hiçbir zaman deposu gizli verileri kaynak kodu. Hesabı ve kimlik bilgileri appSetting depolanır. Azure üzerinde güvenli bir şekilde bu değerleri üzerinde depolayabileceğiniz **[yapılandırma](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portalında sekmesi. Bkz: [en iyi uygulamalar parolalar ve diğer hassas verileri ASP.NET ve Azure dağıtmak için](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>E-posta onayı hesap denetleyicideki etkinleştir

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Doğrulama *Views\Account\ConfirmEmail.cshtml* dosyasının doğru razor sözdizimi vardır. (@ Karakteri ilk satır eksik olabilir. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Uygulamayı çalıştırın ve kayıt bağlantısına tıklayın. Kayıt formunu gönderme sonra oturum açtınız.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

E-posta hesabınızı denetleyin ve e-postanızı onaylamak için bağlantıyı tıklatın.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Oturum açma önce e-posta onayı iste

Şu anda bir kullanıcı kayıt formunu tamamladıktan sonra bunların oturum açtınız. Genellikle, oturum açmayı önce e-postalarına doğrulamak istersiniz. Aşağıdaki bölümde, biz yeni kullanıcılar oturum önce onaylanan bir e-posta olmasını (kimliği doğrulanmış) gerektirecek şekilde kodu değiştirin. Güncelleştirme `HttpPost Register` aşağıdaki vurgulanan değişikliklerle yöntemi:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Out yorum tarafından `SignInAsync` yöntemi, kullanıcı tarafından kayıt oturum açacaksınız değil. `TempData["ViewBagLink"] = callbackUrl;` Satır için kullanılan olabilir [uygulama hata ayıklama](#dbg) ve test kayıt e-posta göndermeden. `ViewBag.Message` Onayla yönergeleri görüntülemek için kullanılır. [Örnek indirme](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) e-posta onayı e-posta ayarlamadan test etmek için kodunu içerir ve uygulamanın hata ayıklamak için de kullanılabilir.

Oluşturma bir `Views\Shared\Info.cshtml` dosya ve aşağıdaki razor biçimlendirmeyi ekleyin:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Ekleme [Authorize özniteliği](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) için `Contact` giriş denetleyici eylem yöntemi. Tıklatabilirsiniz **kişi** anonim kullanıcıların erişim sahip değilseniz ve kimliği doğrulanmış kullanıcıların erişimi doğrulamak için bağlantı.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Aynı zamanda güncelleştirmelisiniz `HttpPost Login` eylem yöntemi:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Güncelleştirme *Views\Shared\Error.cshtml* hata iletisini görüntülemek için görünümü:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Herhangi bir hesabı silmek **AspNetUsers** test etmek istediğiniz e-posta diğer adını içeren tablo. Uygulamayı çalıştırın ve e-posta adresinizi onayladıktan kadar oturum açamazsınız doğrulayın. E-posta adresinizi doğrulayın sonra tıklayın **kişi** bağlantı.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Parola kurtarma/sıfırlama

Açıklama karakterlerinden kaldırmak `HttpPost ForgotPassword` hesabı denetleyicideki eylem yöntemi:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Açıklama karakterlerinden kaldırmak `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) içinde *Views\Account\Login.cshtml* razor görünüm dosyası:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Oturum açma sayfasında artık parola sıfırlama bağlantısı gösterilir.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>E-posta onayı bağlantısını yeniden gönder

Kullanıcı yeni bir yerel hesabı oluşturduktan sonra bunların kullanıcılar oturum açmadan önce kullanmak için gerekli onay bağlantısı e-posta gönderilir. Onay e-posta yanlışlıkla kullanıcıyı siler veya e-posta hiçbir zaman ulaştığında, bunlar yeniden gönderilen onay bağlantısı gerekir. Aşağıdaki kod değişikliklerini bunu etkinleştirmek nasıl gösterir.

' In altına aşağıdaki yardımcı yöntemi ekleyin *Controllers\AccountController.cs* dosyası:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Yeni Yardımcısı kullanmak için kayıt yöntemi güncelleştirin:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Kullanıcı hesabı doğrulanmamış, parolayı yeniden göndermek için oturum açma yöntemi güncelleştirin:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Sosyal ve yerel oturum açma hesaplarını birleştirmek

Yerel ve sosyal hesapları, e-posta bağlantısını tıklatarak birleştirebilirsiniz. Aşağıdaki sırayla **RickAndMSFT@gmail.com** yerel bir oturum olarak oluşturulur, ancak ilk sosyal günlüğüne olarak hesabı oluşturun, sonra yerel oturum açma ekleyin.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Tıklayın **Yönet** bağlantı. Not **dış oturum açma: 0** bu hesapla ilişkili.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Hizmet başka bir oturum açmak için bağlantıya tıklayın ve uygulama isteklerini kabul edin. İki hesap birlikte, her iki hesabı ile oturum açabilecek olacaktır. Kullanıcının sosyal oturum açtığında kimlik doğrulama hizmeti çalışmıyor ya da daha büyük bir olasılıkla bunlar erişim sosyal hesaplarında kesilmiş durumda yerel hesapları eklemek için kullanıcılarınızın isteyebilirsiniz.

Aşağıdaki görüntüde, sosyal günlüğüne zel olduğu (gelen görebileceğiniz **dış oturum açma: 1** sayfasında gösterilen).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Tıklayarak **parola çekme** yerel günlüğe eklemek aynı hesabı ile ilişkili sağlar.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Daha fazla derinlemesine e-posta onayı

My öğretici [hesap ve parola kurtarma ile ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) bu konuda daha fazla ayrıntı içeren girmeyeceğini.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Uygulama hata ayıklama

Bağlantısı içeren bir e-posta alamazsanız:

- Önemsiz ya da istenmeyen posta klasörünüzü kontrol edin.
- SendGrid hesabınızda oturum açın ve tıklayın [e-posta etkinliğinin bağlantı](https://sendgrid.com/logs/index).

E-posta olmadan doğrulama bağlantısını test etmek için Yükle [tamamlanan örnek](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Onay bağlantısı ve onay kodları sayfasında görüntülenir.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [Önerilen bağlantılar ASP.NET Identity için kaynaklar](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Hesap onaylama ve parola kurtarma ile ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) parola kurtarma ve hesap onayı hakkında daha fazla ayrıntı girmeyeceğini.
- [MVC 5 uygulamayla Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Bu öğreticide ASP.NET MVC 5 uygulamayı yetkilendirme Facebook ve Google OAuth 2 ile yazmak gösterilmektedir. Ayrıca, ek veri kimlik veritabanına eklemek nasıl gösterir.
- [Üyelik, OAuth ve SQL veritabanı ile Güvenli ASP.NET MVC uygulaması Azure'a dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Bu öğretici Azure dağıtım ekler uygulamanızı rolleri ile güvenli hale getirmek nasıl kullanıcılar, roller ve ek güvenlik özellikleri eklemek için üyelik API'si kullanma.
- [OAuth 2 için bir Google uygulaması oluşturma ve uygulama projesine bağlanma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [İçinde Facebook uygulaması oluşturma ve uygulama projesine bağlanma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Projedeki SSL ayarlama](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
