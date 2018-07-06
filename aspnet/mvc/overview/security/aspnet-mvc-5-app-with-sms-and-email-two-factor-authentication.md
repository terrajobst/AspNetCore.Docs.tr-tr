---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: SMS ve e-posta iki öğeli kimlik doğrulaması ile ASP.NET MVC 5 uygulaması | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, iki öğeli kimlik doğrulaması ile bir ASP.NET MVC 5 web uygulaması oluşturma işlemini göstermektedir. Oluşturma güvenli bir ASP.NET MVC 5 web uygulaması ile tamamlamanız gerekir...
ms.author: aspnetcontent
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4fd0091effcf2cc0517da91922981e49ef0eef5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803213"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>SMS ve e-posta iki öğeli kimlik doğrulaması ile ASP.NET MVC 5 uygulaması
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide, iki öğeli kimlik doğrulaması ile bir ASP.NET MVC 5 web uygulaması oluşturma işlemini göstermektedir. Tamamlamanız gereken [oturum açma, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) devam etmeden önce. Tamamlanmış uygulamayı karşıdan yükleyebileceğiniz [burada](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). İndirilen bir e-posta veya SMS Sağlayıcısı ayarlamadan test e-posta onayı ve SMS olanak tanıyan hata ayıklama Yardımcıları bulunur.
> 
> Bu öğretici tarafından yazılmıştır [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Bir ASP.NET MVC uygulaması oluşturma](#createMvc)
- [SMS ' için iki öğeli kimlik doğrulama ayarlayın](#SMS)
- [İki öğeli kimlik doğrulamayı etkinleştirme](#enable2)
- [Ek kaynaklar](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Bir ASP.NET MVC uygulaması oluşturma

Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Yükleme [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) veya üzeri.

> [!NOTE]
> Uyarı: Tamamlamanız gereken [oturum açma, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) devam etmeden önce. Yüklemelisiniz [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da bu öğreticiyi tamamlamak için daha yüksek.


1. Yeni ASP.NET Web projesi oluşturun ve MVC şablonu seçin. Web Forms da destekler ASP.NET Identity şekilde bir web forms uygulaması benzer adımları izleyebilirsiniz.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Varsayılan kimlik doğrulaması olarak bırakın **bireysel kullanıcı hesapları**. Uygulamayı azure'da barındırmak istiyorsanız, onay kutusunu işaretli bırakın. Öğreticide daha sonra Azure'da dağıtacağız. Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Ayarlama [SSL kullanmak üzere proje](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>SMS ' için iki öğeli kimlik doğrulama ayarlayın

Bu öğretici, Twilio ya da ASPSMS kullanımıyla ilgili yönergeleri sağlar, ancak herhangi bir SMS Sağlayıcısı kullanabilirsiniz.

1. **Bir SMS Sağlayıcısı ile bir kullanıcı hesabı oluşturma**  
  
   Oluşturma bir [Twilio](https://www.twilio.com/try-twilio) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) hesabı.
2. **Ek paketler yüklenirken veya hizmet başvuruları ekleme**  
  
   Twilio:  
   Paket Yöneticisi konsolunda aşağıdaki komutu girin:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Aşağıdaki servis başvurusu eklenmesi gerekir:  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Adresi:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Ad alanı:  
    `ASPSMSX2`
3. **SMS Sağlayıcısı kullanıcı kimlik bilgilerini başarınızda**  
  
   Twilio:  
   Gelen **Pano** sekmesini Twilio hesabınızın kopyalama **hesap SID'si** ve **kimlik doğrulama belirteci**.  
  
   ASPSMS:  
   Hesap ayarlarınıza gidin **Userkey** kopyalayıp, şirket içinde tanımlanan birlikte **parola**.  
  
   Daha sonra bu değerleri depolarız *web.config* anahtarları dosyasında `"SMSAccountIdentification"` ve `"SMSAccountPassword"` .
4. **Senderıd belirtme / Düzenleyicisi**  
  
   Twilio:  
   Gelen **numaraları** sekmesinde, Twilio telefon numaranızı kopyalayın.  
  
   ASPSMS:  
   İçinde **kilidini düzenleyicileri** menüsünde, bir veya daha fazla düzenleyicileri kilidini veya bir alfasayısal gönderen (tüm ağlar tarafından desteklenmez) seçin.  
  
   Bu değer daha sonra depolarız *web.config* anahtar dosyasında `"SMSAccountFrom"` .
5. **SMS Sağlayıcısı kimlik bilgilerinin uygulamaya aktarma**  
  
   Gönderen telefon numarası ve kimlik bilgileri uygulama için kullanılabilir hale. Basit bir anlatım gözetildiği için bu değerleri depolarız *web.config* dosya. Biz Azure'a dağıtırken, güvenli bir şekilde giriş değerleri depolayabilir miyim **uygulama ayarları** bölümü web sitesinde yapılandırma sekmesi. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki. Örneği basit tutmak için yukarıdaki kod, kimlik bilgilerini ve hesabı eklenir. Bkz: [parolalar ve diğer hassas verileri ASP.NET ve Azure'a dağıtmak için en iyi yöntemler](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Veri aktarımı için SMS Sağlayıcısı uygulama**  
  
   Yapılandırma `SmsService` sınıfını *uygulama\_Start\IdentityConfig.cs* dosya.  
  
   Ya da bağlı olarak kullanılan SMS Sağlayıcısı etkinleştirme **Twilio** veya **ASPSMS** bölümü: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Güncelleştirme *Views\Manage\Index.cshtml* Razor Görünüm: (Not: Yalnızca mevcut kod açıklamaları kaldırma yoksa, aşağıdaki kodu kullanın.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Doğrulama `EnableTwoFactorAuthentication` ve `DisableTwoFactorAuthentication` eylem yöntemlerinde `ManageController` sahip[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) özniteliği:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Uygulamayı çalıştırın ve daha önce kaydettiğiniz bir hesapla oturum açın.
10. Tıklayın etkinleştirir, kullanıcı kimliği üzerinde `Index` eylem yönteminde `Manage` denetleyicisi.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Ekle'ye tıklayın.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber` Eylem yöntemi, SMS mesajları alıp bir telefon numarası girmek için bir iletişim kutusu görüntüler.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. Birkaç saniye içinde doğrulama kodunu içeren bir kısa mesaj alırsınız. Girip tuşuna **Gönder**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. Telefon numaranız eklendi Yönet görüntüler.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>İki öğeli kimlik doğrulamayı etkinleştirme

Şablonu oluşturulan uygulamada iki öğeli kimlik doğrulamayı (2FA) etkinleştirmek için kullanıcı arabirimini kullanmanız gerekir. 2fa'yı etkinleştirmek için gezinti çubuğunda kullanıcı Kimliğinizi (e-posta diğer adı) tıklayın.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Üzerinde etkinleştirme 2fa'yı tıklayın.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Oturum kapatma, daha sonra yeniden oturum açın. E-posta etkinleştirdiyseniz (bkz my [önceki öğreticide](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), SMS veya e-posta için 2fa'yı seçin.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Kod (SMS veya e-posta) girebileceğiniz doğrulayın kod sayfası görüntülenir.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Tıklayarak **bu tarayıcı Hatırlansın** onay kutusunu muaf, cihaz ve tarayıcı onay kutusunun seçili olduğu kullanırken oturum açma 2fa'yı kullanmaya gerek. Kötü amaçlı kullanıcılar cihazınıza erişemezler sürece, 2fa'yı etkinleştirme ve tıklayarak **bu tarayıcı Hatırlansın** hala tüm erişim için güçlü 2FA koruma korurken kullanışlı bir adım parola erişimle sağlayacaktır. güvenilir olmayan cihazlardan. Düzenli olarak kullandığınız özel cihaz üzerinde bunu yapabilirsiniz.

Bu öğretici, yeni bir ASP.NET MVC uygulamasında 2fa'yı etkinleştirmek için hızlı bir giriş sağlar. My öğretici [SMS ve e-posta ile ASP.NET Identity kullanılarak iki öğeli kimlik doğrulama](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) örnek arkasındaki kodu ayrıntıya gider.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [SMS ve e-posta ile ASP.NET Identity kullanılarak iki öğeli kimlik doğrulama](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) iki öğeli kimlik doğrulaması ayrıntıya gider
- [Bağlantılar ASP.NET Identity önerilen kaynaklar](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Onaylama ve parola kurtarma ASP.NET Identity ile hesap](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) parola kurtarma ve Hesap doğrulama hakkında daha fazla ayrıntıya gider.
- [Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma ile MVC 5 uygulaması](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Bu öğreticide bir ASP.NET MVC 5 uygulaması Facebook ve Google OAuth 2 yetkilendirmesiyle yazma işlemi gösterilmektedir. Ayrıca kimlik veritabanı için ek veri ekleme işlemini de gösterir.
- [Membership, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC uygulaması için Azure Web dağıtımı](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Bu öğreticide Azure dağıtım ekler rolleri ile uygulamanızı güvenli hale getirmeyi kullanıcılar ve roller ve ek güvenlik özellikleri eklemek için üyelik API'si kullanma.
- [OAuth 2 için Google uygulaması oluşturma ve uygulama projesine bağlanma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Facebook uygulama oluşturma ve uygulama projesine bağlanma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Projedeki SSL ayarlama](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
