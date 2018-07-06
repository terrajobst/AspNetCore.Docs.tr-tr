---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure kimlik doğrulaması | Microsoft Docs
author: Rick-Anderson
description: Windows Azure Active Directory için Microsoft ASP.NET araçları kolaylaştırır Windows Azure Web sitelerinde barındırılan web uygulamaları için kimlik doğrulamasını etkinleştir...
ms.author: aspnetcontent
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: e3443fb627dc8d7d5011341828556b4c13836170
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812821"
---
<a name="windows-azure-authentication"></a>Windows Azure kimlik doğrulaması
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET araçları için Windows Azure Active Directory üzerinde barındırılan web uygulamaları için kimlik doğrulamasını etkinleştirme sürecini kolaylaştırır [Windows Azure Web siteleri](https://www.windowsazure.com/home/features/web-sites/). Kuruluşunuz, şirket içi Active Directory'nizden eşitlenen Kurumsal hesaplara veya kendi özel Windows Azure Active Directory etki alanında oluşturulan kullanıcılar Office 365 kullanıcıların kimliğini doğrulamak için Windows Azure kimlik doğrulaması'nı kullanabilirsiniz. Windows Azure kimlik doğrulamasını etkinleştirme, tek bir kullanan kullanıcıların kimliğini doğrulamak için uygulamanıza yapılandırır [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) Kiracı.
> 
> Bir bulut hizmeti web rolü için ASP.NET Windows Azure kimlik doğrulaması aracı desteklenmemektedir ancak gelecek bir sürümde bunun planlıyoruz. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF), Windows Azure web rolleri desteklenir.
> 
> Windows Azure Active Directory kiracınız ile şirket içi Active Directory'nizde arasındaki eşitleme kurulumu hakkında ayrıntılar için lütfen bkz [uygulamak ve yönetmek için kullanım AD FS 2.0 çoklu oturum açma](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory şu anda olarak bir [ücretsiz önizleme hizmeti](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Gereksinimler:

- Visual Studio 2012 veya [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Web Araçları uzantıları Visual Studio 2012 için](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) veya [Web Araçları uzantıları Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET araçları için Windows Azure Active Directory ile Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) veya [Microsoft ASP.NET araçları için Windows Azure Active Directory ile Web için Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Visual Studio 2012 ile bir ASP.NET Web uygulaması oluşturma

Visual Studio 2012 ile herhangi bir Web uygulaması oluşturabilir, bu öğreticide ASP.NET MVC Intranet şablonunu kullanır.

1. Yeni bir ASP.NET MVC 4 Intranet uygulaması oluşturun ve tüm Varsayılanları kabul edin. (In olmalıdır **tra** net değildir **ter** net Proje).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>(Genel Yöneticisi de aynen geçerli olduğunda) penceresi Azure kimlik doğrulamasını etkinleştir

Mevcut bir Windows Azure Active Directory kiracısı (örneğin, üzerinden mevcut bir Office 365 hesabı) yoksa yeni bir kiracı için kaydolarak oluşturabileceğiniz bir [yeni Windows Azure Active Directory hesabı](http://g.microsoftonline.com/0AX00en/5).

1. Proje menüsünden seçin **Windows Azure kimlik doğrulamasını etkinleştir**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Windows Azure Active Directory kiracınız için (örneğin, contoso.onmicrosoft.com) etki alanı girin ve tıklayın **etkinleştirme**:

![](windows-azure-authentication/_static/image3.png)

3. Web kimlik doğrulaması iletişim oturum açma Windows Azure Active Directory kiracınız için yönetici olarak:  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Yönetici olmayan göre uyarlamanız, Windows Azure'ı etkinleştir

Windows Azure Active Directory kiracınız için genel yönetici ayrıcalığına sahip değil, uygulama sağlama için onay kutusunun işaretini kaldırın.

![](windows-azure-authentication/_static/image6.png)

İletişim kutusu görüntüler **etki alanı**, **uygulama birincil kimliği** ve **yanıt URL'si** olan uygulama Azure Active Directory ile sağlamak için gerekli de aynen geçerli. Uygulama sağlamak için yeterli ayrıcalığı olan birisi bu bilgiler vermemiz gerekir. Bkz:[nasıl Windows Azure Active Directory - ASP.NET uygulaması ile çoklu oturum açma uygulanacağı](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) cmdlet'in el ile hizmet sorumlusu oluşturmak için nasıl kullanılacağı hakkında ayrıntılı bilgi için.  
Uygulama başarıyla sağlandıktan sonra tıklayabilirsiniz **seçilen ayarlarla web.config güncelleştirmeye devam**. Beklerken meydana gelmesine tıklayabilirsiniz sağlamak için uygulama geliştirmeye devam etmek istiyorsanız **kapatmak için Proje dosyasındaki ayarları unutmayın**. Sonraki Windows Azure kimlik doğrulamasını etkinleştir'i çağırmak ve sağlama onay kutusunun işaretini kaldırın, aynı ayarları görürsünüz ve tıklayabilirsiniz **devam**,'a tıklayın, **web.configBuayarlarıuygulamak**.

1. Uygulamanızı Windows Azure kimlik doğrulaması için yapılandırılmış ve Windows Azure Active Directory ile sağlanan bekleyin.
2. Windows Azure kimlik doğrulaması için uygulamanızı etkinleştirildikten sonra tıklayın **Kapat:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Uygulamanızı çalıştırmak için F5'e basın. Otomatik olarak oturum açma sayfasına yönlendirilirsiniz. Uygulamada oturum açmak için dizin de aynen geçerli kullanıcı kimlik bilgilerini kullan...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Uygulamanızı otomatik olarak imzalanan bir sertifika şu anda kullandığından, sertifika bir güvenilen sertifika yetkilisi tarafından verilmemiş tarayıcısından bir uyarı alırsınız.

    Bu uyarıyı güvenle yerel geliştirme sırasında tıklayarak yoksayılabilir **bu Web sitesine devam et:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Şimdi başarıyla uygulamanıza Windows Azure kimlik doğrulaması kullanarak oturum açtıktan!

    ![](windows-azure-authentication/_static/image2.jpg)

Windows Azure etkinleştirme kimlik doğrulaması, uygulamanız için aşağıdaki değişiklikleri yapar:

- Bir Anti Site siteler arası istek sahteciliğini ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) sınıfı ( *uygulama\_Start\AntiXsrfConfig.cs* ) projenize eklenir.
- NuGet paketlerini `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` projenize eklenir.
- Windows Identity Foundation, uygulama ayarlarında, Windows Azure Active Directory kiracınızın güvenlik belirteçleri kabul edecek şekilde yapılandırılır. Yapılan değişiklikleri Genişletilmiş görünümde görmek için aşağıdaki görüntüye tıklayın *Web.config* dosya.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Hizmet sorumlusu uygulamanıza Windows Azure Active Directory kiracınız için sağlanır.
- HTTPS etkin.

## <a name="deploy-the-application-to-windows-azure"></a>Uygulamayı Windows azure'a dağıtma

Eksiksiz yönergeler için bkz: [bir ASP.NET Web uygulaması için bir Windows Azure Web sitesi dağıtma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Windows Azure kimlik doğrulaması için bir Azure Web sitesini kullanarak bir uygulama yayımlamak için:

1. Uygulamanız üzerinde sağ tıklatın ve seçin **Yayımla:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. Web'i Yayımla iletişim kutusundan indirin ve Azure Web siteniz için bir yayımlama profili içeri aktarın.

    ![](windows-azure-authentication/_static/image4.jpg)
3. **Bağlantı** sekme gösterir **hedef URL** (genel kullanıma yönelik uygulamanızın URL'si). Tıklayın **bağlantıyı doğrula** bağlantınızı test etmek için:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Bu Azure Web sitesine yayımladıysanız daha önce denetlemeyi düşünün **hedefteki ek dosyaları Kaldır** düzgün bir şekilde uygulamanızı emin olmak için ayar yayımlar. Bildirim **Windows Azure kimlik doğrulamasını etkinleştir** slected onay kutusu görünür.  

    ![](windows-azure-authentication/_static/image10.png)
5. : İsteğe bağlı **Önizleme** sekmesini tıklatın **önizlemeyi Başlat** dağıtılan dosyaları görmek için.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Tıklayın **yayımlayın.**

    Hedef ana bilgisayar için Windows Azure kimlik doğrulamasını etkinleştirmek için istenir. Tıklayın **etkinleştirme** devam etmek için:

    ![](windows-azure-authentication/_static/image11.png)
7. Windows Azure Active Directory kiracınız için yönetici kimlik bilgilerinizi girin:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Uygulamanız başarıyla yayımlandıktan sonra yayımlanmış bir web sitesine bir tarayıcıda açılır.

    > [!NOTE]
    > Bu, en fazla beş dakika (genellikle gereken daha az) uygulamanızın hedef ana bilgisayar için Windows Azure kimlik doğrulaması etkinleştirdikten sonra Windows Azure Active Directory ile tam olarak sağlanması sürebilir. İlk kez çalıştırdığınızda, uygulamanızın ACS50001 hatası alırsanız: '[Bölge]' adlı bağlı olan taraf bulunamadı, sonra birkaç dakika bekleyin ve uygulamayı yeniden çalıştırmayı deneyin.
9. İstendiğinde, dizininizdeki bir kullanıcı olarak oturum açın:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Artık Azure başarıyla kapattınız barındırılan Windows Azure kimlik doğrulaması kullanarak uygulama.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Bilinen Sorunlar

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Rol tabanlı yetkilendirme başarısız Windows Azure kimlik doğrulaması < o:p >< kullanırken / o:p >

Böylece rol tabanlı yetkilendirme gerçekleştirilebilir Windows Azure kimlik doğrulaması gerekli rol talep şu anda sağlamaz. Kimliği doğrulanmış kullanıcı rolüne el ile Windows Azure Active Directory. < o:p >< alınması / o:p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Hata sonuçlarında Windows Azure kimlik doğrulaması ile bir uygulama için gözatma "(live.com) oturum açmış olan kullanıcının etki alanını herhangi eşleşmiyor ACS20016 bu etki alanı STS izin verilen" < o:p >< / o:p >

Zaten bir Microsoft Account (örneğin, hotmail.com, live.com, outlook.com) kaydedilir ve Windows Azure kimlik doğrulaması etkinleştirilmiş bir uygulamaya erişmeye çalışırsanız çünkü 400 hata yanıtı alabilirsiniz Microsoft Account etki alanı Windows Azure Active Directory tarafından tanınmıyor. Uygulamasına oturum açmak için Microsoft Account önce oturumunuzu. < o:p >< / o:p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Windows Azure etkin kimlik doğrulaması ile uygulama ve bir X509CertificateValidationMode dışındaki oturum yok < o:p >< accounts.accesscontrol.windows.net sertifikası için sertifika doğrulama hatalarını sonuçlanır / o:p >

Sertifika doğrulama gerekli değildir ve devre dışı bırakılmalıdır. Verenin sertifika parmak izi doğrulanır WSFederationAuthenticationModule. < o:p >< / o:p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Windows Azure kimlik doğrulamasını etkinleştirmek çalışırken Web kimlik doğrulaması iletişim kutusunun hata gösterir. "ACS20016: (contoso.onmicrosoft.com) oturum açmış olan kullanıcının etki alanını bu STS, izin verilen olan herhangi bir etki alanı eşleşmiyor." < o:p >< / o:p >

Daha önce başarılı bir farklı Windows Azure Active Directory hesabı aynı Visual Studio işlemini kullanarak oturum olduğunda bu hatayı görebilirsiniz. Belirtilen hesaptan oturumu kapatmayın veya Visual Studio'yu yeniden başlatın. Daha önce oturum açıp "Oturumumu açık bırak" seçeneği seçili durumunda, tarayıcı tanımlama bilgileri. < o:p >< temizlemeniz gerekebilir / o:p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: Geçerli bir WS-Federation protokolü ileti < o:p >< istek değil / o:p >

Diğer bazı Microsoft ID Azure hizmetlerinin bir oturum zaten oturum açtıysanız bu durum oluşabilir. Kullanım özel tarayıcı penceresinde IE'de InPrivate veya Incognito Chrome gibi veya tüm tanımlama bilgilerini temizleyin. < o:p >< / o:p >

## <a name="additional-resources"></a>Ek Kaynaklar

- [Microsoft ASP.NET araçları için Windows Azure Active Directory ile Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci'nin
- [Windows Azure özellikleri: kimlik](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: kuruluşunuz için uygulamalar geliştirin](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: birden çok kuruluşlar için uygulamalar geliştirin](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Windows Azure Active Directory ile çoklu oturum açma ekleme](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Çoklu oturum açma ile Windows Azure Active Directory: derinlemesine bakış](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci'nin
- [Çoklu oturum açmayı uygulamak ve yönetmek için AD FS'yi kullanma 2.0](https://technet.microsoft.com/library/jj205462.aspx)
