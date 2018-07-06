---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: MVC 5 oluşturma Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma (C#) ile uygulama | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, OAuth 2.0 kullanarak bir dış kimlik Doğr kimlik bilgileriyle oturum açmasına olanak tanıyan bir ASP.NET MVC 5 web uygulaması oluşturma işlemini göstermektedir...
ms.author: aspnetcontent
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 6af4990f726bfcd0c45eb6991418661f9b8ccbf6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824712"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma (C#) ile bir ASP.NET MVC 5 uygulaması oluşturma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide, kullanarak kullanıcıların oturum açma olanak sağlayan bir ASP.NET MVC 5 web uygulaması oluşturma işlemini göstermektedir [OAuth 2.0](http://oauth.net/2/) Facebook, Twitter, LinkedIn, Microsoft veya Google gibi bir dış kimlik doğrulama sağlayıcısı'ndan kimlik bilgilerine sahip. Kolaylık olması için Bu öğreticide, Facebook ve Google kimlik bilgileriyle ile çalışma hakkındaki odaklanır.
> 
> Hesapları bu dış sağlayıcıları ile milyonlarca kullanıcıya zaten olduğundan, web sitelerinizi, bu kimlik bilgileri sağlayarak bu önemli bir avantaj sunar. Bu kullanıcılar, yeni bir dizi kimlik bilgisi oluşturma ve olmadığı siteniz için kaydolmanız daha eilimli olabilir.
> 
> Ayrıca bkz: [SMS ve e-posta iki öğeli kimlik doğrulaması ile ASP.NET MVC 5 uygulaması](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Öğreticinin ayrıca kullanıcıya profil verileri ekleme ve üyelik API'si rolleri eklemek için nasıl kullanılacağını gösterir. Bu öğretici tarafından yazılmıştır [Rick Anderson](https://blogs.msdn.com/rickAndy) (Lütfen bu bana Twitter'da takip edin: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Başlarken

Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Visual Studio yükleme [2013 güncelleştirme 3](https://go.microsoft.com/fwlink/?LinkId=390521) veya üzeri. Dropbox, GitHub, LinkedIn, Instagram, arabellek, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! ve daha fazla yardım için bkz. Bu [kodunuzla](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Visual Studio yüklemelisiniz [2013 güncelleştirme 3](https://go.microsoft.com/fwlink/?LinkId=390521) veya Google OAuth 2'yi kullanın ve yerel olarak SSL uyarılar olmadan hata ayıklamak için daha yüksek.


Tıklayın **yeni proje** gelen **Başlat** sayfasında veya menüyü kullanın ve seçin **dosya**, ardından **yeni proje**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>İlk uygulamanızı oluşturma

Tıklayın **yeni proje**, ardından **Visual C#** solda, ardından **Web** seçip **ASP.NET Web uygulaması**. "MvcAuth" projenizi adlandırın ve ardından **Tamam**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda, tıklayın **MVC**. Kimlik doğrulaması değilse **bireysel kullanıcı hesapları**, tıklayın **kimlik doğrulamayı Değiştir** düğmesini tıklatın ve seçin **bireysel kullanıcı hesapları**. Denetleyerek **bulutta Barındır**, uygulamayı Azure'da barındırmak çok kolay.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Seçtiyseniz **bulutta Barındır**, yapılandırma iletişim tamamlayın.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>En son OWIN ara yazılımını güncelleştirmek için NuGet kullanma

Güncelleştirmek için NuGet paket yöneticisini kullanın [OWIN ara yazılımı](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Seçin **güncelleştirmeleri** soldaki menüde. Tıklayabilirsiniz **Tümünü Güncelleştir** düğmesini veya (sonraki görüntüde gösterilmiştir) OWIN paketleri arayabilirsiniz:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Aşağıdaki görüntüde, yalnızca OWIN paketler gösterilmektedir:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Paket Yöneticisi Konsolu (PMC'yi) öğesinden girdiğiniz `Update-Package` komutu tüm paketleri güncelleştirir.

Tuşuna **F5** veya **Ctrl + F5** uygulamayı çalıştırın. Aşağıdaki görüntüde, bağlantı noktası numarası 1234 ise. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.

Tarayıcı pencerenizin boyutuna bağlı olarak, görmek için Gezinti simgesine tıklamanız gerekebilir **giriş**, **hakkında**, **kişi**, **kaydetme**ve **oturum** bağlantıları.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Projedeki SSL ayarlama

Google ve Facebook gibi kimlik doğrulama sağlayıcıları bağlanmak için IIS Express SSL kullanacak şekilde gerekecektir. SSL oturum açtıktan sonra kullanmaya devam edebilirsiniz ve HTTP geri bırak değil önemlidir, oturum açma tanımlama bilgisinin parolası olarak ağ üzerinden düz metin olarak gönderiyorsanız SSL kullanarak olmadan ve kullanıcı adı ve parola olarak. Yanında, zaten anlaşmasını gerçekleştirmek ve güvenli (HTTPS HTTP yavaş kılan toplu olan) kanal zaman zamandaki MVC ardışık düzeni çalıştırmadan önce bu nedenle oturum açtınız sonra geri HTTP yeniden yönlendirme geçerli istek veya gelecekteki yaratmayacaktır istekler daha hızlı.

1. İçinde **Çözüm Gezgini**, tıklayın **MvcAuth** proje.
2. Proje özelliklerini göstermek için F4 tuşuna basın. Alternatif olarak, gelen **görünümü** seçebileceğiniz menü **Özellikler penceresi**.
3. Değişiklik **SSL etkin** true.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. SSL URL'sini kopyala (olacak `https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece).
5. İçinde **Çözüm Gezgini**, sağ tıklayın **MvcAuth** seçin ve proje **özellikleri**.
6. Seçin **Web** sekmesine ve ardından SSL URL'sini yapıştırın **proje URL'si** kutusu. ' % S'dosyası (Ctl + S) kaydedin. Facebook ve Google kimlik doğrulama uygulamaları yapılandırmak için bu URL gerekir.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Ekleme [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) özniteliğini `Home` denetleyicisi gerektiren tüm istekler için HTTPS kullanmalıdır. Daha güvenli bir yöntem eklemektir [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) uygulama için filtre. Bölümüne bakın &quot;SSL ve yetkilendirmek özniteliği ile uygulama koruma&quot; my tutoral içinde [kimlik denetimi ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Giriş denetleyicisine değerinin bir bölümü aşağıda gösterilmiştir.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. Geçmişte sertifika yüklü değilse, bu bölümün geri kalanında atlayın ve atlama [OAuth 2 için Google uygulaması oluşturma ve uygulama projesiyle bağlantı kurulurken](#goog), aksi takdirde otomatik olarak imzalanan güven için yönergeleri izleyin IIS Express'in oluşturduğu sertifika.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Okuma **Güvenlik Uyarısı** iletişim ve ardından **Evet** localhost temsil eden bir sertifika yüklemek istiyorsanız.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE gösterir *giriş* sayfasında ve SSL uyarı yok.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome, ayrıca sertifikayı kabul eder ve HTTPS içeriğine olmadan bir uyarı gösterilir. Firefox, kendi sertifika deposu kullanır, bu nedenle, bir uyarı görüntülenir. Uygulamamız için güvenli bir şekilde tıklayabilirsiniz **riskleri anlıyorum**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>OAuth 2 için Google uygulaması oluşturma ve uygulama projesine bağlanma

> [!WARNING]
> Geçerli Google OAuth yönergeler için bkz: [ASP.NET Core kimlik doğrulaması yapılandırma Google](/aspnet/core/security/authentication/social/google-logins).

1. Gidin [Google geliştiriciler konsol](https://console.developers.google.com/).
2. Bir proje önce oluşturmadıysanız, seçin **kimlik bilgilerini** sol sekmeye tıklayın ve ardından içinde **Oluştur**.
3. Sol sekmede tıklayın **kimlik bilgilerini**.
4. Tıklayın **kimlik bilgilerini oluştur** ardından **OAuth istemcisi kimliği**. 

    1. İçinde **istemci kodu oluşturma** iletişim kutusunda, varsayılan tutun **Web uygulaması** uygulama türü için.
    2. Ayarlama **yetkili JavaScript** kaynakları yukarıda kullanılan SSL URL'sine (`https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece)
    3. Ayarlama **yetkili yeniden yönlendirme URI'si** için:  
         `https://localhost:44300/signin-google`
5. OAuth onay ekranı menü öğesini tıklatın ve ardından e-posta adresi ve ürün adı ayarlayın. Form tıklayarak bitirdiğinizde **Kaydet**.
6. Kitaplık menü öğesini tıklayın, arama **Google + API**, buna tıklayın ardından etkinleştir tuşuna basın.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   Aşağıdaki resimde, etkin API'leri gösterir.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Google API'leri API Yöneticisi'nden ziyaret **kimlik bilgilerini** almak için sekmesinde **istemci kimliği**. Uygulama gizli dizilerini bir JSON dosyası kaydetmek için indirin. Kopyalama ve yapıştırma **ClientID** ve **ClientSecret** içine `UseGoogleAuthentication` yöntemi bulunan *Startup.Auth.cs* dosyası *App_Start* klasör. **ClientID** ve **ClientSecret** aşağıda gösterilen değerleri örnek ve çalışmıyor.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki. Örneği basit tutmak için yukarıdaki kod, kimlik bilgilerini ve hesabı eklenir. Bkz: [parolalar ve diğer hassas verileri ASP.NET ve Azure App Service'e dağıtmak için en iyi yöntemler](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Tuşuna **CTRL + F5** oluşturun ve uygulamayı çalıştırın. Tıklayın **oturum** bağlantı.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. Altında **başka bir hizmete oturum açmak için kullandığınız**, tıklayın **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Yukarıdaki adımların kaçırmanıza bir HTTP 401 hatası alırsınız. Yukarıdaki adımları uygulamanıza yeniden denetleyin. Gerekli bir ayar kaçırmanıza (örneğin **ürün adı**), eksik öğe ekleyip kaydedin; kimlik doğrulamasının çalışması için birkaç dakika sürebilir.
10. Google kimlik bilgilerinizi gireceğiniz siteye yönlendirileceksiniz.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Kimlik bilgilerinizi girdikten sonra yeni oluşturduğunuz web uygulamasına izin vermek için istenir:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Tıklayın **kabul**. Artık geri yönlendirilirsiniz **kaydetme** Burada, kaydedebilir Google hesabınızı MvcAuth uygulama sayfası. Gmail hesabınızı için kullanılan yerel e-posta kayıt adı değiştirme seçeneğiniz vardır, ancak genellikle varsayılan e-posta diğer adı (diğer bir deyişle, bir kimlik doğrulaması için kullanılan) tutmak istersiniz. Tıklayın **kaydetme**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Facebook uygulama oluşturma ve uygulama projesine bağlanma

> [!WARNING]
> Geçerli Facebook OAuth2 kimlik doğrulaması hakkında yönergeler için bkz. [yapılandırma Facebook kimlik doğrulaması](/aspnet/core/security/authentication/social/facebook-logins)

Facebook OAuth2 kimlik doğrulaması için Facebook içinde oluşturduğunuz bir uygulamadan bazı ayarları projenize kopyalamanız gerekir.

1. Tarayıcınızda gidin [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) ve oturum açma Facebook kimlik bilgilerinizi girerek.
2. Bir Facebook geliştirici olarak zaten kayıtlı değil, tıklayın **geliştiricisi olarak kaydolma** ve kaydetmek için yönergeleri izleyin.
3. Üzerinde **uygulamaları** sekmesinde **yeni uygulama oluştur**.

    ![Yeni uygulama oluştur](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. Girin bir **uygulama adı** ve **kategori**, ardından **uygulama oluşturma**.

    <strong>Uygulama Namespace</strong> uygulamanıza kimlik doğrulaması için Facebook uygulamaya erişmek için kullanacağı bir URL parçası olan (örneğin, https\://apps.facebook.com/{App Namespace}). Belirtmezseniz bir <strong>uygulama Namespace</strong>, <strong>uygulama kimliği</strong> URL için kullanılacaktır. <strong>Uygulama kimliği</strong> sonraki adımda göreceğiniz uzun sistem tarafından oluşturulan bir sayı.

    ![Yeni uygulama iletişim kutusu oluşturma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. Standart güvenlik denetimi gönderin.

    ![Güvenlik denetimi](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. Seçin **ayarları** sol menü çubuğu için![ Facebook geliştiricinin menü çubuğu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. Üzerinde **temel** sayfanın ayarları bölümü seçin **Platform Ekle** bir Web sitesi uygulama eklemekte olduğunuz belirtmek için. ![Temel ayarları](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. Seçin **Web sitesi** platformu seçeneklerden.  
  
    ![Platform seçenekleri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. Not, **uygulama kimliği** ve **uygulama gizli anahtarı** böylece daha sonra Bu öğreticide hem MVC uygulamanıza ekleyebilirsiniz. Ayrıca, sitenizin URL'sini ekleyin (`https://localhost:44300/`) MVC uygulamanızı test etmek için. Ayrıca, bir **ilgili kişi e-posta**. Ardından, **Değişiklikleri Kaydet**.   

    ![Temel Uygulama Ayrıntıları sayfası](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > Yalnızca kayıtlı e-posta diğer adı kullanarak kimlik doğrulaması için olacağını unutmayın. Diğer kullanıcılar ve test hesapları kaydetmek mümkün olmayacaktır. Facebook uygulama diğer Facebook hesapları erişimi verebilir **Geliştirici rolleri** sekmesi.
10. Visual Studio'da açın *uygulama\_Start\Startup.Auth.cs*.
11. Kopyalama ve yapıştırma **AppID** ve **uygulama gizli anahtarı** içine `UseFacebookAuthentication` yöntemi. **AppID** ve **uygulama gizli anahtarı** aşağıda gösterilen değerler örnekleri ve çalışmaz.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. Tıklayın **değişiklikleri kaydetmek**.
13. Tuşuna **CTRL + F5** uygulamayı çalıştırın.


Seçin **oturum** oturum açma sayfasını görüntüleyin. Tıklayın **Facebook** altında **oturum açmak için başka bir hizmet kullanın.**

Facebook kimlik bilgilerinizi girin.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

Genel profiliniz ve arkadaş listesi erişmek uygulamayı izni istenir.

![Facebook uygulama ayrıntıları](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

Artık oturum açtınız. Bu hesap artık uygulama ile kaydedebilirsiniz.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

Kaydolduğunuzda, bir giriş eklenen *kullanıcılar* üyelik veritabanının tablo.

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Üyelik verilerini İnceleme

İçinde **görünümü** menüsünde tıklatın **Sunucu Gezgini**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Genişletin **DefaultConnection (MvcAuth)**, genişletme **tabloları**, sağ tıklayın **AspNetUsers** tıklatıp **tablo verilerini Göster**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers tablo verileri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Kullanıcı profil verileri ekleme

Bu bölümde, doğum tarihi ve başlangıç belediye kullanıcı verilerini kayıt sırasında aşağıdaki görüntüde gösterildiği gibi ekleyeceksiniz.

![Giriş Şehir ve Bday ile kayıt defteri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Açık *Models\IdentityModels.cs* dosya ve doğum tarihi ve giriş Şehir özellikleri ekleyin:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Açık *Models\AccountViewModels.cs* dosyasının ve küme doğum tarihi ve giriş Şehir özelliklerinde `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Açık *Controllers\AccountController.cs* dosya ve doğum tarihi ve giriş şehir için kod ekleyin `ExternalLoginConfirmation` gösterildiği gibi bir eylem yöntemi:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Doğum tarihi ve giriş şehir için ekleme *Views\Account\ExternalLoginConfirmation.cshtml* dosyası:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Yeniden uygulamanızla Facebook hesabınızı kaydedin ve yeni bir doğum tarihi ve başlangıç belediye profil bilgilerini ekleyebilirsiniz doğrulayın üyelik veritabanını silin.

Gelen **Çözüm Gezgini**, tıklayın **tüm dosyaları göster** simgesine ve ardından sağ tıklama *Ekle\_Data\aspnet-MvcAuth -&lt;tarih damgası&gt;.mdf* tıklatıp **Sil**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Gelen **Araçları** menüsünde tıklayın **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu** (PMC). PMC'de aşağıdaki komutları girin.

1. Geçişleri etkinleştir
2. Geçiş başlatma
3. Veritabanını Güncelleştir

Uygulamayı çalıştırmak ve FaceBook ve Google oturum açmak ve bazı kullanıcılar kaydetmek için kullanın.

## <a name="examine-the-membership-data"></a>Üyelik verilerini İnceleme

İçinde **görünümü** menüsünde tıklatın **Sunucu Gezgini**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Sağ tıklayın **AspNetUsers** tıklatıp **tablo verilerini Göster**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown` Ve `BirthDate` alanları aşağıda gösterilmektedir.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Uygulamanızı günlüğe kaydetme ve başka bir hesap ile günlüğe kaydetme

Uygulamanızı, Facebook ile oturum açın ve ardından Oturumu Kapat ve oturum açmayı deneyin yeniden (aynı tarayıcıyı kullanarak), farklı bir Facebook hesabıyla, hemen kullandığınız önceki Facebook hesabıyla oturum açmanız. Başka bir hesap kullanmak için Facebook'a gidin ve Facebook oturumunuzu gerekir. Aynı kural, tüm 3 taraf kimlik sağlayıcıları için geçerlidir. Alternatif olarak, farklı bir tarayıcı kullanarak başka bir hesapla oturum oturum açabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Bkz: [Yahoo ve LinkedIn OAuth güvenlik sağlayıcıları için OWIN giriş](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) Jerrie Pelser Yahoo ve LinkedIn yönergeleri tarafından. Jerrie'nın bkz Yapıyorsak etkinleştir sosyal oturum açma düğmeleri almak ASP.NET MVC 5 sosyal oturum açma düğmeleri.

My öğreticiyi izleyin [kimlik denetimi ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), bu öğreticiye devam eder ve şunları gösterir:

1. Uygulamanızı Azure'a dağıtmak nasıl.
2. Uygulama rolleri ile güvenliğini sağlama.
3. Uygulamanız ile güvenli hale getirmeyi [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) ve [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtreler.
4. Kullanıcıları ve rolleri eklemek için üyelik API'si kullanma

Lütfen bu öğreticide sevmediğinizi nasıl ve ne geliştirebileceğimiz hakkında geri bildirim bırakın. Yeni konuları da isteyebilirsiniz [Show Me nasıl ile kod](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Hatta isteyin ve ASP.NET için eklenecek yeni özellikleri oy verin. Örneğin, bir aracı için oy verebilirsiniz [kullanıcıları ve rolleri oluşturun ve yönetin.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

ASP.NET Dış kimlik doğrulama hizmetleri nasıl çalışır bir iyi açıklama için bkz: Robert McMurray'nın [Dış kimlik doğrulama hizmetleri](https://asp.net/web-api/overview/security/external-authentication-services). Robert'ın makale aynı zamanda Microsoft ve Twitter kimlik doğrulamasının etkinleştirilmesinde ayrıntıya gider. Tom Dykstra mükemmel [EF/MVC Öğreticisi](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Entity Framework ile çalışma hakkında bilgi verilmektedir.
