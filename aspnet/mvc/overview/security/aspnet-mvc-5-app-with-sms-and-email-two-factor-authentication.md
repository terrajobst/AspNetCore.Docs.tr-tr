---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: SMS ve e-posta iki öğeli kimlik doğrulama ile ASP.NET MVC 5 uygulaması | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici bir ASP.NET MVC 5 web uygulaması iki faktörlü kimlik doğrulamasıyla nasıl oluşturulacağını gösterir. Create güvenli bir ASP.NET MVC 5 web uygulaması ile tamamlanacaksa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 5e1c54b3901f2c8c85134445c1fa91ee9f2e0d59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873618"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>SMS ve e-posta iki öğeli kimlik doğrulama ile ASP.NET MVC 5 uygulaması
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici bir ASP.NET MVC 5 web uygulaması iki faktörlü kimlik doğrulamasıyla nasıl oluşturulacağını gösterir. Tamamlanmış olmalıdır [günlüğünde, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) devam etmeden önce. Tamamlanan uygulama indirebilirsiniz [burada](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Yükleme, bir e-posta veya SMS Sağlayıcısı ayarlamadan test e-posta onayı ve SMS izin hata ayıklama Yardımcıları içerir.
> 
> Bu öğretici tarafından yazıldı [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Bir ASP.NET MVC uygulaması oluşturma](#createMvc)
- [İki faktörlü kimlik doğrulaması için SMS ayarlayın](#SMS)
- [İki faktörlü kimlik doğrulamasını etkinleştir](#enable2)
- [Ek kaynaklar](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Bir ASP.NET MVC uygulaması oluşturma

Başlangıç yüklenmesi ve çalıştırılması [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Yükleme [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da daha yüksek.

> [!NOTE]
> Uyarı: Tamamlamanız [günlüğünde, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) devam etmeden önce. Yüklemelisiniz [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da bu öğreticiyi tamamlamak için daha yüksek.


1. Yeni bir ASP.NET Web projesi oluşturun ve MVC şablonunu seçin. Web Forms da destekler ASP.NET kimliği için bir web forms uygulamasında benzer adımları izleyebilirsiniz.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Varsayılan kimlik doğrulaması olarak bırakın **tek tek kullanıcı hesaplarını**. Uygulamayı azure'da barındırmak istiyorsanız, onay kutusunun işaretli bırakın. Daha sonra öğreticide biz Azure'a dağıtacaksınız. Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Ayarlama [SSL kullanmak üzere proje](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>İki faktörlü kimlik doğrulaması için SMS ayarlayın

Bu öğretici Twilio veya ASPSMS kullanma yönergeleri sağlar ancak herhangi bir SMS sağlayıcıyı kullanabilirsiniz.

1. **Bir SMS Sağlayıcısı ile bir kullanıcı hesabı oluşturma**  
  
   Oluşturma bir [Twilio](https://www.twilio.com/try-twilio) veya bir [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) hesabı.
2. **Ek paketler yükleme veya hizmet başvuruları ekleme**  
  
   Twilio:  
   Paket Yöneticisi konsolunda aşağıdaki komutu girin:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Aşağıdaki hizmet başvurusu eklenmesi gerekir:  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Adresi:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Ad alanı:  
    `ASPSMSX2`
3. **SMS Sağlayıcısı kullanıcı kimlik bilgilerini açık**  
  
   Twilio:  
   Gelen **Pano** Twilio hesabınızı kopyalama sekmesinde **hesabının SID** ve **kimlik doğrulama belirteci**.  
  
   ASPSMS:  
   Hesap ayarlarınızı, gitmek **Userkey** ve, kendi kendine tanımlı birlikte kopyalayın **parola**.  
  
   Biz daha sonra bu değerleri depolar *web.config* anahtarları dosyasında `"SMSAccountIdentification"` ve `"SMSAccountPassword"` .
4. **Senderıd belirtme / gönderen**  
  
   Twilio:  
   Gelen **numaraları** sekmesinde, Twilio telefon numaranızı kopyalayın.  
  
   ASPSMS:  
   İçinde **kilidini başlatıcılarını** menüsünde, bir veya daha fazla başlatıcılarını kilidini veya bir alfasayısal başlatanın (tüm ağları tarafından desteklenmez) seçin.  
  
   Biz bu değeri daha sonra depolayacaktır *web.config* anahtar içindeki dosya `"SMSAccountFrom"` .
5. **SMS Sağlayıcısı kimlik bilgileri bir uygulamaya aktarma**  
  
   Kimlik bilgileri ve gönderen telefon numarasını uygulama için kullanılabilir hale getirir. Örneği basit tutmak için bu değerleri depolarız *web.config* dosya. Biz Azure'a dağıtırken, güvenli bir şekilde giriş değerleri depolayabilir miyim **uygulama ayarları** bölüm web sitesinde yapılandırma sekmesi. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Güvenlik - hiçbir zaman deposu gizli verileri kaynak kodu. Hesabı ve kimlik bilgileri örneği basit tutmak için yukarıdaki kod eklenir. Bkz: [en iyi uygulamalar parolalar ve diğer hassas verileri ASP.NET ve Azure dağıtmak için](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Veri aktarımı için SMS sağlayıcı uygulaması**  
  
   Yapılandırma `SmsService` sınıfını *uygulama\_Start\IdentityConfig.cs* dosya.  
  
   Ya da bağlı olarak kullanılan SMS sağlayıcı etkinleştirme **Twilio** veya **ASPSMS** bölümü: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Güncelleştirme *Views\Manage\Index.cshtml* Razor Görünüm: (Not: değil yalnızca yorumları mevcut kodda kaldırmak için aşağıdaki kodu kullanın.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Doğrulama `EnableTwoFactorAuthentication` ve `DisableTwoFactorAuthentication` eylem yöntemlerinde `ManageController` sahip[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) özniteliği:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Uygulamayı çalıştırın ve daha önce kaydettiğiniz hesabıyla oturum açın.
10. Tıklatın etkinleştirir, kullanıcı kimliği üzerinde `Index` eylem yönteminde `Manage` denetleyicisi.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Ekle'yi tıklatın.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber` Eylem yöntemi SMS iletileri alabilen bir telefon numarası girmek için bir iletişim kutusu görüntüler.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. Birkaç saniye içinde doğrulama kodunu içeren bir kısa mesaj alırsınız. Girip tuşuna **gönderme**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. Telefon numaranız eklendi yönet görünümü gösterir.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>İki faktörlü kimlik doğrulamasını etkinleştir

Oluşturulan şablon uygulamada, iki faktörlü kimlik doğrulamasını (2FA) etkinleştirmek için kullanıcı arabirimini kullanmanız gerekir. 2FA etkinleştirmek için gezinti çubuğunda kullanıcı kimliği (e-posta diğer ad) tıklayın.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Üzerinde 2FA Etkinleştir'i tıklatın.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Oturum kapatma, ardından günlük geri. E-posta etkinleştirdiyseniz (bkz my [önceki öğretici](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), SMS veya e-posta 2FA için seçebilirsiniz.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Kod (SMS veya e-posta) girebileceğiniz doğrulayın kod sayfası görüntülenir.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Tıklayarak **bu tarayıcı unutmayın** onay kutusunu muaf, tarayıcı ve cihaz Burada onay kutusunun seçili kullanırken oturum açmak için 2FA kullanmanıza gerek. Kötü niyetli kullanıcılar Cihazınızı erişim sağlayamadığı sürece, etkinleştirme 2FA ve tıklayarak **bu tarayıcı unutmayın** hala tüm erişim için güçlü 2FA koruma korurken kullanışlı bir adım parola erişimle sağlayacaktır güvenilir olmayan cihazlardan. Düzenli olarak kullanmak istediğiniz özel cihazda bunu yapabilirsiniz.

Bu öğretici, yeni bir ASP.NET MVC uygulaması üzerinde 2FA etkinleştirmek için bir giriş sağlar. My öğretici [SMS ve e-posta ile ASP.NET Identity kullanan iki öğeli kimlik doğrulaması](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) örnek arka plan kod üzerinde ayrıntıya gider.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [SMS ve e-posta ile ASP.NET Identity kullanan iki öğeli kimlik doğrulaması](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) iki öğeli kimlik doğrulama ayrıntılı girmeyeceğini
- [Önerilen bağlantılar ASP.NET Identity için kaynaklar](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Hesap onaylama ve parola kurtarma ile ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) parola kurtarma ve hesap onayı hakkında daha fazla ayrıntı girmeyeceğini.
- [MVC 5 uygulamayla Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Bu öğreticide ASP.NET MVC 5 uygulamayı yetkilendirme Facebook ve Google OAuth 2 ile yazmak gösterilmektedir. Ayrıca, ek veri kimlik veritabanına eklemek nasıl gösterir.
- [Üyelik, OAuth ve SQL veritabanı ile Güvenli ASP.NET MVC uygulaması için Azure Web dağıtımı](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Bu öğretici Azure dağıtım ekler uygulamanızı rolleri ile güvenli hale getirmek nasıl kullanıcılar, roller ve ek güvenlik özellikleri eklemek için üyelik API'si kullanma.
- [OAuth 2 için bir Google uygulaması oluşturma ve uygulama projesine bağlanma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [İçinde Facebook uygulaması oluşturma ve uygulama projesine bağlanma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Projedeki SSL ayarlama](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
