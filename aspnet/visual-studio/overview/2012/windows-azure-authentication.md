---
uid: visual-studio/overview/2012/windows-azure-authentication
title: "Windows Azure kimlik doğrulaması | Microsoft Docs"
author: Rick-Anderson
description: "Windows Azure Active Directory için Microsoft ASP.NET araçları, kolaylaştırır Windows Azure Web sitelerinde barındırılan web uygulamaları kimlik doğrulamasını etkinleştirmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4deb3536699f1ef3025f8858ee71a76a1c2def18
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="windows-azure-authentication"></a>Windows Azure kimlik doğrulaması
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Windows Azure Active Directory üzerinde barındırılan web uygulamaları için kimlik doğrulamasını etkinleştirmek basit yapar için Microsoft ASP.NET Araçları [Windows Azure Web siteleri](https://www.windowsazure.com/home/features/web-sites/). Kuruluşunuz, şirket içi Active Directory'nizden eşitlenen Kurumsal hesaplara veya kendi özel Windows Azure Active Directory etki alanında oluşturulan kullanıcıların Office 365 kullanıcıların kimliğini doğrulamak için Windows Azure kimlik doğrulaması kullanabilirsiniz. Windows Azure kimlik doğrulamasını etkinleştirme yapılandırır tek bir kullanarak kullanıcıların kimliklerini doğrulamak için uygulamanızın [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) Kiracı.
> 
> ASP.NET Windows Azure kimlik doğrulaması aracı bir bulut hizmeti web rollerinde desteklenmez, ancak gelecekteki bir sürümde bunu planlıyoruz. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF), Windows Azure web rolleri desteklenir.
> 
> Şirket içi Active Directory ve Windows Azure Active Directory kiracınızın arasındaki eşitleme kurulumu hakkında ayrıntılar için lütfen bkz [uygulamak ve yönetmek için kullanım AD FS 2.0 çoklu oturum açma](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory şu anda kullanılabilir olarak bir [serbest Önizleme hizmet](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Gereksinimleri:

- Visual Studio 2012 veya [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Web için Visual Studio 2012 uzantıları Araçları](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) veya [Web araç uzantıları için Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET araçları için Windows Azure Active Directory ile Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) veya [Microsoft ASP.NET araçları için Windows Azure Active Directory ile Web için Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Visual Studio 2012 ile ASP.NET Web uygulaması oluşturma

Bu öğreticide ASP.NET MVC Intranet şablonunu kullanır, Visual Studio 2012 ile herhangi bir Web uygulaması oluşturabilirsiniz.

1. Yeni bir ASP.NET MVC 4 Intranet uygulaması oluşturun ve tüm Varsayılanları kabul edin. (Bir In olmalıdır **bağlantı** net ve de **ter** net Proje).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>(Tenet genel Yöneticisi olduğunda) penceresi Azure kimlik doğrulamasını etkinleştir

Mevcut bir Windows Azure Active Directory Kiracı (örneğin, mevcut Office 365 hesap üzerinden) yoksa yeni bir kiracı için imzalayarak oluşturabileceğiniz bir [yeni Windows Azure Active Directory hesabı](http://g.microsoftonline.com/0AX00en/5).

1. Proje menüsünden seçin **Windows Azure kimlik doğrulamasını etkinleştir**:  
  
 ![](windows-azure-authentication/_static/image2.png)

2. Windows Azure Active Directory kiracınıza ait (örneğin, contoso.onmicrosoft.com) etki alanını girin ve tıklayın **etkinleştirme**:

![](windows-azure-authentication/_static/image3.png)

3. Web kimlik doğrulaması iletişim oturum açma Windows Azure Active Directory kiracınızın için yönetici olarak:  
  
 ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Yönetici olmayan tarafından Tenet, Windows Azure etkinleştir

Windows Azure Active Directory kiracınız için genel yönetici ayrıcalıklarına sahip değil, uygulama sağlama için onay kutusunun işaretini kaldırın.

![](windows-azure-authentication/_static/image6.png)

İletişim kutusu görüntüler **etki alanı**, **uygulama sorumlu kimliği** ve **yanıt URL'si** uygulama Azure Active Directory ile sağlamak için gerekli olan tenet. Uygulama sağlamak için yeterli ayrıcalığı olan birisi bu bilgileri vermeniz gerekir. Bkz:[çoklu oturum açma Windows Azure Active Directory - ASP.NET uygulaması ile uygulama](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) cmdlet'inin hizmet sorumlusu el ile oluşturmak için nasıl kullanılacağı hakkında ayrıntılı bilgi için.  
Uygulama başarıyla kaynak sağlandı sonra tıklatabilirsiniz **seçilen ayarlarla web.config güncelleştirmeye devam**. Beklerken gerçekleşecek şekilde tıklayabilirsiniz sağlamak için uygulama geliştirmeye devam etmek istiyorsanız **kapatmak için proje dosyasında ayarlarını anımsa**. Sonraki Windows Azure kimlik doğrulamasını etkinleştir çağırma ve sağlama onay kutusunun işaretini kaldırın, aynı ayarları görürsünüz ve tıklayabilirsiniz **devam**, ardından, **web.configdosyasındabuayarlarıuygulamak**.

1. Uygulamanızı Windows Azure kimlik doğrulaması için yapılandırılmış ve Windows Azure Active Directory ile sağlanan bekleyin.
2. Windows Azure kimlik doğrulaması, uygulamanız için etkinleştirildikten sonra tıklatın **Kapat:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Uygulamanızı çalıştırmak için F5'e basın. Otomatik olarak oturum açma sayfasına yeniden yönlendirilen. Uygulamaya oturum açmak için dizin tenet kullanıcı kimlik bilgilerini kullanın...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Uygulamanız şu anda bir otomatik olarak imzalanan bir test sertifikası kullandığından sertifikası güvenilen bir sertifika yetkilisi tarafından verilmemiş tarayıcısından bir uyarı alırsınız.

    Bu uyarı güvenli bir şekilde yerel geliştirme sırasında tıklayarak göz ardı edilebilir **bu Web sitesine devam et:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Artık başarılı Windows Azure kimlik doğrulaması kullanarak uygulamanıza oturum açma!

    ![](windows-azure-authentication/_static/image2.jpg)

Windows Azure etkinleştirme kimlik doğrulaması, uygulamanıza aşağıdaki değişiklikleri yapar:

- Anti Site siteler arası istek Sahteciliğine ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) sınıfı ( *uygulama\_Start\AntiXsrfConfig.cs* ) projenize eklenir.
- NuGet paketlerini `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` projenize eklenir.
- Windows Azure Active Directory kiracınızın gelen güvenlik belirteçleri kabul etmek için uygulamanızı Windows Identity Foundation ayarlarında yapılandırılır. Yapılan değişiklikleri genişletilmiş görünümünü görmek için aşağıdaki görüntüye tıklayın *Web.config* dosya.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Hizmet sorumlusu Windows Azure Active Directory kiracınızda, uygulamanız için sağlanacak.
- HTTPS etkinleştirilir.

## <a name="deploy-the-application-to-windows-azure"></a>Windows Azure için uygulama dağıtma

Tam yönergeler için bkz: [ASP.NET Web uygulaması için bir Windows Azure Web sitesi dağıtma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Windows Azure kimlik doğrulaması için bir Azure Web sitesini kullanarak bir uygulamayı yayımlamak için:

1. Uygulamanızın üzerinde sağ tıklatın ve seçin **Yayımla:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. Web'i Yayımla iletişim kutusundan indirin ve Azure Web siteniz için bir yayımlama profili alın.

    ![](windows-azure-authentication/_static/image4.jpg)
3. **Bağlantı** sekmesinde gösterir **hedef URL** (genel kullanıma yönelik URL uygulamanız için). Tıklatın **bağlantıyı doğrula** bağlantınızı sınamak için:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Bu Azure Web sitesine yayımladıysanız önceden denetimi düşünün **hedefteki ek dosyaları Kaldır** uygulamanızı emin olmak için ayarı düzgün bir şekilde yayımlar. Bildirim **Windows Azure kimlik doğrulamasını etkinleştir** onay kutusudur slected.  

    ![](windows-azure-authentication/_static/image10.png)
5. : İsteğe bağlı **Önizleme** sekmesini tıklatın **önizlemeyi Başlat** dağıtılan dosyaları görmek için.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Tıklatın **yayımlayın.**

    Hedef ana bilgisayar için Windows Azure kimlik doğrulamasını etkinleştirmek için istenir. Tıklatın **etkinleştirmek** devam etmek için:

    ![](windows-azure-authentication/_static/image11.png)
7. İçin Windows Azure Active Directory Kiracı Yöneticisi kimlik bilgilerinizi girin:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Uygulamanız başarıyla yayımlandıktan sonra bir tarayıcı yayımlanmış bir web sitesine açın.

    > [!NOTE]
    > Beş dakika (genellikle gereken daha az) hedef ana bilgisayar için Windows Azure kimlik doğrulaması etkinleştirdikten sonra Windows Azure Active Directory ile tamamen sağlanması, uygulamanız için sürebilir. İlk kez çalıştırdığınızda, uygulamanızın ACS50001 hata alırsanız: '[Bölge]' adıyla bağlı olan taraf bulunamadı, sonra birkaç dakika bekleyin ve uygulamayı yeniden çalıştırmayı deneyin.
9. İstendiğinde, dizininizdeki bir kullanıcı olarak oturum açın:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Şimdi, Azure'da oturumunuz başarıyla Windows Azure kimlik doğrulaması kullanarak uygulama barındırılan.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Bilinen Sorunlar

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Rol tabanlı yetkilendirme başarısız Windows Azure kimlik doğrulaması < o: >< kullanılırken / o: p >

Böylece rol tabanlı yetkilendirme gerçekleştirilebilir Windows Azure kimlik doğrulaması gerekli rol talep şu anda sağlamaz. Kimliği doğrulanmış kullanıcının rolünü el ile Windows Azure Active Directory. < o: >< alınması gereken / o: p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Windows Azure kimlik doğrulaması hata sonuçlarında sahip bir uygulama için gözatma "(live.com) oturum açmış olan kullanıcının etki alanını herhangi eşleşmiyor ACS20016 bu etki alanı STS izin verilen" < o: >< / o: p >

Zaten bir Microsoft Account (örneğin hotmail.com, live.com, outlook.com) kaydedilir ve Windows Azure kimlik doğrulaması etkin bir uygulamaya erişmeyi denemek için 400 hata yanıtı alabilirsiniz Microsoft Account etki alanı Windows Azure Active Directory tarafından tanınmıyor. Uygulamasına günlüğe kaydetmek için Microsoft Account önce oturumu kapatın. < o: >< / o: p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Windows Azure etkin kimlik doğrulaması ile uygulama ve bir X509CertificateValidationMode dışındaki oturum None < o: >< accounts.accesscontrol.windows.net sertifika için sertifika doğrulama hatalarını sonuçlanır / o: p >

Sertifika doğrulama gerekli değildir ve devre dışı bırakılmalıdır. Sertifikanın parmak izi veren WSFederationAuthenticationModule. < o: >< doğrulanır / o: p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Windows Azure kimlik doğrulamasını etkinleştirmek çalışırken Web kimlik doğrulama iletişim hatayı gösteren "ACS20016: (contoso.onmicrosoft.com) oturum açmış olan kullanıcının etki alanını bu STS, izin verilen olan herhangi bir etki alanı eşleşmiyor." < o: >< / o: p >

Daha önce farklı bir Windows Azure Active Directory hesabını aynı Visual Studio işlemi içinde kullanarak oturumunuz başarıyla olduğunda bu hatayı görebilirsiniz. Belirtilen hesabından oturumunuzu veya Visual Studio'yu yeniden başlatın. Daha önce oturum açmış ve "beni imzalı tut" seçeneğini seçili varsa, tarayıcı tanımlama bilgilerini. < o: >< temizlemeniz gerekebilir / o: p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: İstek geçerli bir WS-Federasyon protokolü ileti < o: >< değilse / o: p >

Zaten diğer bazı Microsoft ID Azure hizmetlerini birine oturum açtıysanız bu durum oluşabilir. Kullanım özel bir tarayıcı penceresi IE'de InPrivate veya Incognito chrome'da gibi veya tüm tanımlama bilgilerini temizleyin. <o:p></o:p>

## <a name="additional-resources"></a>Ek Kaynaklar

- [Microsoft ASP.NET araçları için Windows Azure Active Directory ile Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows Azure özellikleri: kimlik](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: kuruluşunuz için uygulamalar geliştirme](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: birden çok kuruluşlar için uygulamalar geliştirme](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Çoklu oturum açma Windows Azure Active Directory ile uygulama](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Çoklu oturum açma Windows Azure Active Directory: derinlemesine](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [Çoklu oturum açmayı uygulamak ve yönetmek için AD FS kullanma 2.0](https://technet.microsoft.com/library/jj205462.aspx)
