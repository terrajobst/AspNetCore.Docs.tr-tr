---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: "MVC 5 oluşturma Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma (C#) ile uygulama | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici, OAuth 2.0 dış authenti kimlik bilgilerini kullanarak oturum açmalarını sağlar bir ASP.NET MVC 5 web uygulamalarının nasıl oluşturulacağını gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: ccf4329e6684d07570bfaabfaa1a570664fb2ca3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma (C#) ile bir ASP.NET MVC 5 uygulaması oluşturma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici kullanıcıların oturum açmasına imkan sağlayan bir ASP.NET MVC 5 web uygulamasını kullanarak nasıl oluşturulacağını gösterir [OAuth 2.0](http://oauth.net/2/) Facebook, Twitter, LinkedIn, Microsoft veya Google gibi bir dış kimlik doğrulama sağlayıcısından kimlik bilgilerine sahip. Kolaylık olması için Bu öğretici, Facebook ve Google kimlik bilgilerini ile çalışma hakkında odaklanır.
> 
> Milyonlarca kullanıcıya bu dış sağlayıcıları hesaplarıyla olduğundan bu kimlik bilgileri, web sitelerindeki etkinleştirilmesi önemli bir avantajı sağlar. Bu kullanıcılar oluşturun ve yeni bir kimlik bilgileri kümesi unutmayın gerek yoktur, siteniz için kaydolmanız daha eilimli olabilir.
> 
> Ayrıca bkz. [SMS ve e-posta iki öğeli kimlik doğrulama ile ASP.NET MVC 5 uygulaması](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Öğreticinin Ayrıca kullanıcı için profil verileri ekleme ve üyelik API'si rolleri eklemek için nasıl kullanılacağını gösterir. Bu öğretici tarafından yazıldı [Rick Anderson](https://blogs.msdn.com/rickAndy) (Lütfen bu bana Twitter'da takip edin: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Başlarken

Başlangıç yüklenmesi ve çalıştırılması [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Visual Studio yükleme [2013 güncelleştirme 3](https://go.microsoft.com/fwlink/?LinkId=390521) ya da daha yüksek. Dropbox, GitHub, LinkedIn, Instagram, arabellek, salesforce, akış, yığın Exchange, Tripit, twitch, Twitter, Yahoo ve daha fazla yardım için bkz [bir Dur Kılavuzu](http://www.oauthforaspnet.com/).

> [!NOTE]
> Visual Studio yüklemelisiniz [2013 güncelleştirme 3](https://go.microsoft.com/fwlink/?LinkId=390521) veya Google OAuth 2'yi kullanın ve yerel olarak SSL uyarılar olmadan hata ayıklamak için daha yüksek.


Tıklatın **yeni proje** gelen **Başlat** sayfa veya menüsünü kullanın ve seçin **dosya**ve ardından **yeni proje**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Tıklatın **yeni proje**seçeneğini belirleyip **Visual C#** solda, ardından **Web** ve ardından **ASP.NET Web uygulaması**. Projeniz "MvcAuth" olarak adlandırın ve ardından **Tamam**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda, tıklatın **MVC**. Kimlik doğrulaması değilse **tek tek kullanıcı hesaplarını**, tıklatın **kimlik doğrulamayı Değiştir** düğmesine tıklayın ve ardından **tek tek kullanıcı hesaplarını**. Denetleyerek **bulutta Barındır**, uygulama Azure'da barındırmak çok kolay olacaktır.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Seçtiyseniz **bulutta Barındır**, Yapılandır iletişim tamamlayın.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>En son OWIN ara yazılımı için güncelleştirmek için NuGet kullanma

Güncelleştirmek için NuGet paket yöneticisini kullanın [OWIN ara yazılımı](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Seçin **güncelleştirmeleri** soldaki menüde. Tıklatabilirsiniz **Tümünü Güncelleştir** düğmesini veya yalnızca (sonraki görüntüde gösterilen) OWIN paketler arayabilirsiniz:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Aşağıdaki resimde, yalnızca OWIN paketler gösterilir:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Paket Yöneticisi Konsolu (PMC) gelen girdiğiniz `Update-Package` tüm paketleri güncelleştirir komutu.

Tuşuna **F5** veya **Ctrl + F5** uygulamayı çalıştırın. Aşağıdaki resimde 1234 bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.

Tarayıcı pencerenizin boyutuna bağlı olarak görmek için Gezinti simgesini gerekebilir **giriş**, **hakkında**, **kişi**, **kaydetmek**ve **oturum** bağlantılar.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Projedeki SSL ayarlama

Google ve Facebook gibi kimlik doğrulama sağlayıcıları bağlanmak için SSL kullanmak üzere IIS Express'i ayarlamanız gerekir. Oturum açtıktan sonra SSL kullanmaya devam et ve HTTP geri bırakma değil de önemlidir, oturum açma tanımlama bilgisinin yalnızca gizlilik olarak kullanıcı adı ve parola olarak ve ağ üzerinden düz metin olarak gönderiyoruz SSL kullanmadan. Anlaşma gerçekleştirmek için güvenli (HTTPS HTTP yavaş kılan toplu olan) kanal zaman yanında, zaten ayırdıktan MVC ardışık düzeni çalıştırmadan önce böylece oturum açtınız sonra geri HTTP yeniden yönlendirme geçerli istek veya gelecekteki yapmaz çok daha hızlı istek sayısı.

1. İçinde **Çözüm Gezgini**, tıklatın **MvcAuth** projesi.
2. Proje özellikleri göstermek için F4 tuşuna basın. Alternatif olarak, gelen **Görünüm** seçebileceğiniz menü **Özellikler penceresini**.
3. Değişiklik **SSL özellikli** true.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. SSL URL'sini kopyala (olacak `https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece).
5. İçinde **Çözüm Gezgini**, sağ tıklatın **MvcAuth** proje ve seçin **özellikleri**.
6. Seçin **Web** sekmesini ve ardından içine SSL URL'sini yapıştırın **proje URL'sini** kutusu. (Ctl + S) dosyasını kaydedin. Facebook ve Google kimlik doğrulama uygulamaları yapılandırmak için bu URL gerekir.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Ekleme [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) özniteliğini `Home` tüm istekleri gerektirecek şekilde denetleyicisi HTTPS kullanmalıdır. Daha güvenli bir yöntem eklemektir [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtre uygulama. Bölümüne bakın &quot;SSL ve yetkilendirmek özniteliği uygulamasıyla koruma&quot; my tutoral içinde [auth ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Giriş denetleyicisi bir bölümü aşağıda verilmiştir.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın. Geçmişte sertifika yüklediyseniz, bu bölümde geri kalanını atlayın ve atlamak [için OAuth 2 bir Google uygulaması oluşturma ve uygulama projesine bağlanma](#goog), aksi takdirde otomatik olarak imzalanan güven için yönergeleri izleyin IIS Express üretti sertifikası.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Okuma **Güvenlik Uyarısı** iletişim ve ardından **Evet** localhost temsil eden sertifika yüklemek istiyorsanız.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE gösterir *giriş* sayfasında ve SSL uyarı yok.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome Ayrıca sertifikayı kabul eder ve HTTPS içerik olmadan bir uyarı gösterilir. Firefox, kendi sertifika deposuna kullanır, bu nedenle bir uyarı görüntülenir. Uygulamamız için güvenli bir şekilde tıklayabilirsiniz **riskleri anlamak**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>OAuth 2 için bir Google uygulaması oluşturma ve uygulama projesine bağlanma

1. Gidin [Google geliştiriciler konsol](https://console.developers.google.com/).
1. Önce bir projeyi oluşturmadıysanız, seçin **kimlik bilgileri** sol sekmesini ve ardından **oluşturma**.
1. Sol sekmede tıklatın **kimlik bilgileri**.
1. Tıklatın **kimlik bilgileri oluşturma** sonra **OAuth istemci kimliği**. 

    1. İçinde **istemci kimliği oluşturma** iletişim kutusunda, varsayılan tutmak **Web uygulaması** uygulama türü için.
    2. Ayarlama **yetkili JavaScript** yukarıda kullanılan SSL URL'ye kaynakları (`https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece)
    3. Ayarlama **yetkili yeniden yönlendirme URI'si** için:  
         `https://localhost:44300/signin-google`
5. OAuth izni ekran menü öğesini tıklatın, ardından e-posta adresi ve ürün adı ayarlayın. Ne zaman tamamladığınızdan form tıklatın **kaydetmek**.
6. Kitaplık menü öğesini tıklatın, arama **Google + API**, üzerinde tıklatın ardından Etkinleştir'e basın.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
 Aşağıdaki görüntü etkin API'leri gösterir.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Google API'leri API Yöneticisi'nden ziyaret **kimlik bilgileri** elde etmek için sekme **istemci kimliği**. Uygulama parolaları ile JSON dosyasının kaydedileceği indirin. Kopyalama ve yapıştırma **ClientID** ve **ClientSecret** içine `UseGoogleAuthentication` yöntemi bulunan *Startup.Auth.cs* dosyasını *App_Start* klasör. **ClientID** ve **ClientSecret** aşağıda gösterilen değerleri örnekleri ve çalışmıyor.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Güvenlik - hiçbir zaman deposu gizli verileri kaynak kodu. Hesabı ve kimlik bilgileri örneği basit tutmak için yukarıdaki kod eklenir. Bkz: [en iyi uygulamalar parolalar ve diğer hassas verileri ASP.NET ve Azure uygulama hizmeti dağıtmak için](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Tuşuna **CTRL + F5** oluşturun ve uygulamayı çalıştırın. Tıklatın **oturum** bağlantı.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. Altında **oturum açmak için başka bir hizmet kullanın**, tıklatın **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Yukarıdaki adımların kaçırmanıza bir HTTP 401 hatası alırsınız. Yukarıdaki adımları uygulamanıza yeniden denetleyin. Gerekli bir ayar kaçırılması durumunda (örneğin **ürün adı**), eksik öğesi ve kaydetme ekleme, kimlik doğrulamasının çalışması için birkaç dakika sürebilir.
10. Kimlik bilgilerinizi gireceğiniz google siteye yönlendirilir.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Kimlik bilgilerinizi girdikten sonra yeni oluşturduğunuz web uygulaması izinleri vermeniz istenir:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Tıklatın **kabul**. Artık yeniden yönlendirileceği konum **kaydetmek** Burada, kaydolabilir Google hesabınız MvcAuth uygulama sayfası. Gmail hesabınız için kullanılan yerel e-posta kayıt adını değiştirme seçeneğiniz vardır, ancak genellikle varsayılan e-posta diğer adı (diğer bir deyişle, kimlik doğrulaması için kullanılan bir) tutmak istiyor. Tıklatın **kaydetmek**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>İçinde Facebook uygulaması oluşturma ve uygulama projesine bağlanma

Facebook OAuth2 kimlik doğrulaması için Facebook içinde oluşturduğunuz bir uygulamadan bazı ayarları projenize kopyalamanız gerekir.

1. Tarayıcınızda gidin [https://developers.facebook.com/apps](https://developers.facebook.com/apps) ve oturum açma Facebook kimlik bilgilerinizi girin.
2. Facebook geliştirici olarak zaten kayıtlı değil, tıklatın **geliştiricisi olarak kaydolma** ve kaydetmek için yönergeleri izleyin.
3. Üzerinde **uygulamaları** sekmesini tıklatın, **yeni uygulama oluşturma**.

    ![Yeni uygulama oluşturma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. Girin bir **App Name** ve **kategori**, ardından **oluşturma uygulama**.

    Bu Facebook arasında benzersiz olması gerekir. **Uygulama Namespace** uygulamanız (örneğin, {uygulama Namespace} https://apps.facebook.com/) kimlik doğrulaması için Facebook uygulama erişmek için kullanacağınız URL parçasıdır. Belirtmediyseniz bir **uygulama Namespace**, **uygulama kimliği** URL için kullanılacaktır. **Uygulama kimliği** sonraki adımda görürsünüz uzun sistem tarafından oluşturulan bir sayı.

    ![Yeni uygulama iletişim oluşturma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. Standart güvenlik denetimi gönderin.

    ![Güvenlik denetimi](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. Seçin **ayarları** sol menü çubuğu'nu için![ Facebook geliştiricinin menü çubuğu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. Üzerinde **temel** ayarları bölümünde seçin **eklemek Platform** bir Web uygulaması ekleme belirtmek için. ![Temel ayarlar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. Seçin **Web sitesi** platformu seçeneklerden.  
  
    ![Platform seçenekleri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. Not, **uygulama kimliği** ve **uygulama gizli anahtarı** böylece daha sonra Bu öğreticide MVC uygulamanıza her ikisi de ekleyebilirsiniz. Ayrıca, Site URL'si ekleyin (`https://localhost:44300/`) MVC uygulamanızı test etmek için. Ayrıca, bir **ilgili kişi e-posta**. Ardından, seçin **Değişiklikleri Kaydet**.   

    ![Temel Uygulama Ayrıntıları sayfası](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > Yalnızca kaydettiğiniz e-posta diğer adını kullanarak kimlik doğrulaması için olacağını unutmayın. Diğer kullanıcılar ve test hesapları kaydetmek mümkün olmaz. Uygulama FaceBook'ta diğer Facebook hesaplarına erişim izni verebilir **Geliştirici rolleri** sekmesi.
10. Visual Studio'da açın *uygulama\_Start\Startup.Auth.cs*.
11. Kopyalama ve yapıştırma **AppID** ve **uygulama gizli anahtarı** içine `UseFacebookAuthentication` yöntemi. **AppID** ve **uygulama gizli anahtarı** aşağıda gösterilen değerleri örnekleri ve çalışmaz.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. Tıklatın **değişiklikleri kaydetmek**.
13. Tuşuna **CTRL + F5** uygulamayı çalıştırın.


Seçin **oturum** oturum açma sayfasını görüntüleyin. Tıklatın **Facebook** altında **oturum açmak için başka bir hizmet kullanın.**

Facebook kimlik bilgilerinizi girin.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

Genel profil ve arkadaş listesi erişmek uygulama izni istenir.

![Facebook uygulama ayrıntıları](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

Artık oturum açtınız. Bu gibi durumlarda, bu hesap artık uygulama ile kaydedebilirsiniz.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

Kaydolduğunuzda, bir giriş eklenen *kullanıcılar* üyelik veritabanının tablo.

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Üyelik verilerinin inceleyin

İçinde **Görünüm** menüsünde tıklatın **Sunucu Gezgini**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Genişletme **DefaultConnection (MvcAuth)**, genişletin **tabloları**, sağ tıklatın **AspNetUsers** tıklatıp **tablo verileri Göster**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers tablo verileri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Profil verileri kullanıcı sınıfına ekleme

Bu bölümde, doğum tarihi ve ev Şehir kullanıcı verilerini kayıt sırasında aşağıdaki görüntüde gösterildiği gibi ekleyeceksiniz.

![reg ev Şehir ve Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Açık *Models\IdentityModels.cs* dosya ve doğum tarihi ve ev Şehir özellikleri ekleyin:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Açık *Models\AccountViewModels.cs* dosya ve kümesi doğum tarihi ve ev Şehir özelliklerinde `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Açık *Controllers\AccountController.cs* dosya ve doğum tarihi ve giriş piyasada için kodu ekleyin `ExternalLoginConfirmation` gösterildiği gibi eylem yöntemi:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Doğum tarihi ve ev Şehir eklemek *Views\Account\ExternalLoginConfirmation.cshtml* dosyası:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Yeniden Facebook hesabınız ile uygulamanızı kaydetme ve ev Şehir profil bilgileri ve yeni doğum tarihi ekleyebilirsiniz doğrulamak için üyelik veritabanını silin.

Gelen **Çözüm Gezgini**, tıklatın **tüm dosyaları göster** simgesine ve ardından sağ tıklatma *Ekle\_Data\aspnet-MvcAuth -&lt;tarih damgası&gt;.mdf* tıklatıp **silmek**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Gelen **Araçları** menüsünde tıklatın **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu** (PMC). Aşağıdaki komutları PMC girin.

1. Enable-Migrations
2. Add-Migration başlatma
3. Update-Database

Uygulamayı çalıştırın ve FaceBook ve Google oturum açın ve bazı kullanıcılar kaydetmek için kullanabilirsiniz.

## <a name="examine-the-membership-data"></a>Üyelik verilerinin inceleyin

İçinde **Görünüm** menüsünde tıklatın **Sunucu Gezgini**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Sağ tıklayın **AspNetUsers** tıklatıp **Show Table Data**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown` Ve `BirthDate` alanları aşağıda gösterilmektedir.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Uygulamanızı günlüğe kaydetme ve içinde başka bir hesap ile oturum

Uygulamanızı,, Facebook ile oturum açın ve sonra oturumu kapatın ve oturum açmayı deneyin yeniden (aynı tarayıcıyı kullanarak), farklı bir Facebook hesabıyla, hemen kullandığınız önceki Facebook hesabıyla oturum açmanız. Başka bir hesap kullanmak için Facebook hesabına gidin ve Facebook sırasında oturumunuzu gerekir. Aynı kural herhangi diğer 3 taraf kimlik doğrulama sağlayıcısı için geçerlidir. Alternatif olarak, farklı bir tarayıcı kullanarak başka bir hesapla oturum oturum açabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Bkz: [OWIN için Yahoo ve LinkedIn OAuth güvenlik sağlayıcıları Tanıtımı](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) Jerrie Pelser Yahoo ve LinkedIn yönergeler tarafından. Jerrie'nın bkz sosyal oturum açmayı düğmeleri etkinleştir sosyal oturum açmayı düğmeleri almak ASP.NET MVC 5 için asıl.

My öğreticisini izleyin [auth ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), Bu öğretici devam eder ve aşağıdakileri gösterir:

1. Uygulamanızı Azure'a dağıtmak nasıl.
2. Uygulama rolleri ile güvenli hale getirmek nasıl.
3. Uygulamanız ile güvenli hale getirmek nasıl [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) ve [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtreler.
4. Kullanıcıları ve rolleri eklemek için üyelik API'si kullanma

Lütfen geri bildirim, Bu öğretici beğendiğinizi nasıl ve ne biz artabileceğini bırakın. En yeni konular da isteğinde bulunabilirsiniz [Göster bana nasıl kodu ile](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Hatta, sorun ve ASP.NET ile eklenecek yeni özellikler oy verin. Örneğin, bir aracı için oy kullanabilir [kullanıcıları ve rolleri oluşturun ve yönetin.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Robert McMurray'nın nasıl ASP.NET Dış kimlik doğrulama hizmetleri işe iyi açıklama için bkz: [Dış kimlik doğrulama hizmetleri](https://asp.net/web-api/overview/security/external-authentication-services). Robert'ın makale aynı zamanda Microsoft ve Twitter kimlik doğrulamasını etkinleştirme içindeki ayrıntısı girmeyeceğini. Zel Dykstra mükemmel [EF/MVC Öğreticisi](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Entity Framework ile çalışmaya nasıl gösterir.
