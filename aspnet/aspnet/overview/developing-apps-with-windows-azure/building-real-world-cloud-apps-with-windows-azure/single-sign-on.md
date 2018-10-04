---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Çoklu oturum açma (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 22ef4c2908783e513bfb6fb63364e71378cb8719
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578438"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Çoklu oturum açma (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


Bulut uygulaması, geliştirmekte olduğunuz hakkında düşünmek için birçok güvenlik sorunları vardır, ancak bu dizi için yalnızca birinde odaklanacağız: çoklu oturum açma. Bir soru genellikle istemem şudur: "ı öncelikle uygulama Şirketim; çalışanlar için oluşturmaya nasıl bu uygulamaları bulutta barındırmak ve bunları my çalışanlar bilmeniz ve bunlar uygulamaları çalıştırıyorsanız şirket içi ortamında kullanın aynı güvenlik modelini kullanmak yine de etkinleştirmek barındırılır güvenlik duvarı içindeki?" Biz bu senaryoyu yollarından biri, Azure Active Directory (Azure AD) olarak adlandırılır. Azure AD, Kurumsal satır iş kolu (LOB) uygulamaları Internet üzerinden kullanımına olanak sağlar ve bu uygulamaları da iş ortakları için kullanılabilir olmasını sağlar.

## <a name="introduction-to-azure-ad"></a>Azure AD'ye giriş

[Azure AD](https://docs.microsoft.com/azure/active-directory/) sağlar [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) bulutta. Temel özellikleri aşağıda verilmiştir:

- Bu, şirket içi Active Directory ile tümleşir.
- Ancak, uygulamalarınızda çoklu oturum açmayı etkinleştirir.
- Gibi açık standartları destekler [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Federasyon](http://en.wikipedia.org/wiki/WS-Federation), ve [OAuth 2.0](http://oauth.net/2/).
- Kurumsal destekler [Graph REST API'si](https://msdn.microsoft.com/library/hh974476.aspx).

Çalışanlarınızın Intranet uygulamalarında oturum sağlamak için kullandığınız bir şirket içi Windows Server Active Directory ortamında olduğunu varsayalım:

![](single-sign-on/_static/image1.png)

Hangi Azure AD yapmanızı sağlayan, bulutta bir dizin oluşturun. Ücretsiz bir özelliktir ve kolay ayarlama.

Şirket içi Active Directory'nizden tamamen bağımsız olabilir. Herkes Internet uygulamalarda kimlik doğrulaması yapmak ve bunu istediğiniz koyabilirsiniz.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Ya da şirket içi ile tümleştirebilirsiniz AD.

![AD ve WAAD tümleştirme](single-sign-on/_static/image3.png)

Artık şirket içi kimlik doğrulaması tüm çalışanları ayrıca Internet üzerinden – bir Güvenlik Duvarı'nı açın veya veri merkezinizdeki herhangi bir yeni sunucu dağıtma gerek kalmadan kimlik doğrulaması yapabilir. Bildiğiniz ve bugün iç uygulamalar çoklu oturum yeteneğini vermek için kullanabileceğiniz tüm mevcut Active Directory ortamında yararlanmaya devam edebilirsiniz.

AD ve Azure AD arasındaki bu bağlantı yaptığınız, web uygulamalarınız ve mobil cihazlarınızı çalışanlarınız bulutta kimlik doğrulaması için de etkinleştirebilirsiniz ve kabul etmek için Office 365, SalesForce.com ve Google apps gibi üçüncü taraf uygulamaları etkinleştirebilirsiniz sonra çalışanların kimlik bilgileri. Office 365 kullanıyorsanız, Office 365 Azure AD kimlik doğrulama ve yetkilendirme için kullandığı için Azure AD ile ayarlamış.

![3. taraf uygulamalar](single-sign-on/_static/image4.png)

Bu yaklaşım güzelliği, kuruluşunuzun ekler ya da bir kullanıcıyı siler veya bir kullanıcının parola değişiklikleri, şirket içi ortamınızda günümüzde kullandığınız aynı işlemi kullanın. Tüm şirket içi bulut ortamında otomatik olarak AD değişiklikleri yayılır.

Şirketiniz kullanarak veya güzel bir haberimiz var olan Office 365'e taşıma yapıyorsanız Azure AD'ye otomatik olarak Office 365, Azure AD kimlik doğrulaması için kullandığı için ayarlanmış sahip olursunuz. Bu nedenle kendi uygulamalarında kolayca Office 365 kullandığı aynı kimlik doğrulamasını kullanabilirsiniz.

## <a name="set-up-an-azure-ad-tenant"></a>Bir Azure AD Kiracı ayarlamak

Azure AD dizini Azure AD çağrılır [Kiracı](https://technet.microsoft.com/library/jj573650.aspx), ve bir kiracı ayarı oldukça kolay. Kavramları göstermek için Azure Yönetim Portalı'nda nasıl yapıldığını göstereceğiz, ancak Elbette gibi diğer portal işlevler ayrıca bir betik veya yönetim API'si kullanarak bunu yapabilirsiniz.

Yönetim Portalı'nda Active Directory sekmesini tıklatın.

![Portalında WAAD](single-sign-on/_static/image5.png)

Azure hesabınız için otomatik olarak bir Azure AD kiracısına sahip ve tıklayabilirsiniz **Ekle** ek dizinler oluşturmak için sayfanın alt kısmındaki düğmesi. Örneğin bir test ortamı için bir tane ve bir üretim isteyebilirsiniz. Yeni bir dizin adı hakkında dikkatlice düşünün. Dizini için kendi adınızı kullanın ve ardından tekrar kullanıcılardan birinin için adınızı kullanırsanız, kafa karıştırıcı olabilir.

![Dizin Ekle](single-sign-on/_static/image6.png)

Portalda oluşturma, silme ve bu ortamda kullanıcıları yönetmek için tam destek bulunur. Örneğin, eklemek için bir kullanıcı Git **kullanıcılar** sekmesine **Kullanıcı Ekle** düğmesi.

![Kullanıcı Ekle düğmesi](single-sign-on/_static/image7.png)

![Kullanıcı iletişim kutusu Ekle](single-sign-on/_static/image8.png)

Yalnızca bu dizinde var. yeni bir kullanıcı oluşturabilir veya bir Microsoft Account bu dizini veya kaydı bir kullanıcı veya başka bir Azure AD dizininden bir kullanıcı olarak bu dizindeki bir kullanıcı olarak kaydedebilirsiniz. (Gerçek bir dizinde ContosoTest.onmicrosoft.com varsayılan etki alanı olacaktır. Ayrıca kendi, contoso.com gibi seçtiğiniz bir etki alanı kullanabilirsiniz.)

![Kullanıcı türleri](single-sign-on/_static/image9.png)

![Kullanıcı iletişim kutusu Ekle](single-sign-on/_static/image10.png)

Kullanıcı rol atayabilirsiniz.

![Kullanıcı profili](single-sign-on/_static/image11.png)

Ve hesabı geçici bir parola ile oluşturulur.

![Geçici parola](single-sign-on/_static/image12.png)

Bu şekilde oluşturduğunuz kullanıcılar hemen web uygulamalarınızı bu bulut dizinini kullanarak oturum açabilir.

Yine de Kurumsal çoklu oturum açma için harika nedir olan **dizin tümleştirme** sekmesinde:

![Dizin tümleştirme sekmesi](single-sign-on/_static/image13.png)

Dizin tümleştirme etkinleştirirseniz ve [bir Aracı'nı indirme](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), mevcut şirket içi Active directory'nizle kuruluşunuz içinde zaten kullanmakta olduğunuz bu bulut dizinini eşitleyebilirsiniz. Daha sonra dizininizde saklanan tüm kullanıcılar bu bulut dizini ile gösterilir. Bulut uygulamalarınızdaki tüm çalışanlarınız, mevcut Active Directory kimlik bilgilerini kullanarak doğrulayabilir. Ve tüm bu ücretsiz – eşitleme aracı ve Azure AD oturumunu.

Bu ekran görüntüleri gördüğünüz gibi kullanımı kolay, bir sihirbaz aracıdır. Bu yönergelerin yalnızca temel işlem gösteren örnek değildir. Bağlantıları, daha ayrıntılı yardım-How-to--BT bilgi için bkz. [kaynakları](#resources) bölümün sonuna bölümü.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image14.png)

Tıklayın **sonraki**ve ardından Azure Active Directory kimlik bilgilerinizi girin.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image15.png)

Tıklayın **sonraki**ve şirket içi enter AD kimlik bilgileri.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image16.png)

Tıklayın **sonraki**ve ardından AD parolalarınızı karma bulutta depolamak istiyorsanız gösterir.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image17.png)

Tek yönlü karma bulutta depolayabilirsiniz. parola karması olur; Gerçek parolaları hiçbir zaman Azure AD içinde depolanır. Karma bulutta depolama karşı karar verirseniz, kullanmanız gerekir [Active Directory Federasyon Hizmetleri](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Ayrıca [göz önüne alınması gereken diğer faktörleri ADFS kullanılıp kullanılmayacağı seçme](https://technet.microsoft.com/library/jj573653.aspx). ADFS seçeneği birkaç ek yapılandırma adımları gerektirir.

Karma bulutta depolamak seçtiğiniz, işiniz ve tıkladığınızda dizin eşitleme Aracı'nı başlatır **sonraki**.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image18.png)

Ve birkaç dakika içinde hazırsınız.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image19.png)

Bu kuruluşta Windows 2003'te veya daha yüksek bir etki alanı denetleyicisinde çalıştırmak yeterlidir. Ve yeniden başlatma gerekmez. İşiniz bittiğinde, bulutta olan tüm kullanıcılarınız ve yapabileceğiniz herhangi bir web veya mobil uygulama, SAML, OAuth ve WS-Federasyon kullanan çoklu oturum açma.

Bazen biz budur hakkında ne kadar güvenli sorulan – Microsoft kendi hassas iş verileri için kullanıyor mu? Ve yanıt evet'tir desteklemiyoruz. Örneğin iç Microsoft SharePoint sitesinde gidin, [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), oturum açmak için girmem isteniyor.

![Office 365 oturum açma](single-sign-on/_static/image20.png)

Microsoft, ADFS, etkinleştirilmiş şekilde a Microsoft ID girdiğinizde, ADFS oturum açma sayfasına yeniden yönlendirilen.

![ADFS oturum açma](single-sign-on/_static/image21.png)

Ve bir iç Microsoft AD hesapta depolanan kimlik bilgileri girdikten sonra bu iç uygulama erişebilirsiniz.

![MS SharePoint sitesi](single-sign-on/_static/image22.png)

Azure AD kullanılabilir hale geldi, ancak oturum açma işleminiz, buluttaki bir Azure AD dizini ile gittiği önce ayarlamanız ADFS zaten çoğunlukla vardı için bir AD sunucusu oturum açma kullanıyoruz. Biz, müşterilerimize önemli belgeleri, kaynak denetimi, performans yönetimi dosyalarını, satış raporları ve bulutta daha koyun ve bunların güvenliğini sağlamak için bu tam aynı çözümü kullanıyorsanız.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Azure AD için çoklu oturum açmayı kullanan ASP.NET uygulaması oluşturma

Visual Studio birkaç ekran görüntüleri gördüğünüz gibi çoklu oturum açma için Azure AD kullanan bir uygulama oluşturmak çok kolay hale getirir.

Yeni bir ASP.NET uygulaması, MVC veya Web Forms oluşturduğunuzda varsayılan kimlik doğrulama yöntemini ASP.NET kimliğidir. Azure AD'ye değiştirmek için tıklayın bir **kimlik doğrulamasını Değiştir** düğmesi.

![Kimlik doğrulamayı Değiştir](single-sign-on/_static/image23.png)

Kurumsal hesaplar'ı seçin, etki alanı adınızı girin ve ardından, çoklu oturum açma seçin.

![Kimlik doğrulaması iletişim kutusunu yapılandırma](single-sign-on/_static/image24.png)

Ayrıca uygulama okuma verin veya izin dizin verilerini okuma/yazma. Bunu yaparsanız, kullanabileceğiniz [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) kullanıcıların telefon numarası aramak için son oturum olduğunda açık, vb. Office'te olup olmadıklarını öğrenmek.

Tüm yapmanız gereken - Visual Studio için kimlik bilgileri Azure AD kiracınız için yönetici ister ve ardından hem proje hem de Azure AD kiracınıza yeni bir uygulama için yapılandırır.

Projeyi çalıştırdığınızda, bir oturum açma sayfası görürsünüz ve bir kullanıcının kimlik bilgileriyle Azure AD dizininizde oturum açabilirsiniz.

![Kuruluş hesabı oturum açma](single-sign-on/_static/image25.png)

![Oturum açanlar](single-sign-on/_static/image26.png)

Uygulamayı Azure'a dağıtırken, yapmanız gereken tek şey seçin bir **kuruluş kimlik doğrulamasını etkinleştir** onay kutusunu işaretleyin ve yine Visual Studio sizin için tüm yapılandırma üstlenir.

![Web'de Yayımla](single-sign-on/_static/image27.png)

Bu ekran görüntüleri, Azure AD kimlik doğrulamasını kullanan bir uygulamayı nasıl oluşturacağınız gösteren tam bir adım adım öğretici gelen: [Azure Active Directory ile ASP.NET uygulamaları geliştirme](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Özet

Bu bölümde, Azure Active Directory, Visual Studio ve ASP.NET, kuruluşunuzdaki kullanıcılar için Internet uygulamalarda çoklu oturum açmayı ayarlama kolaylaştırır olduğunu gördünüz. Kullanıcıların Internet uygulamalarında iç ağınızda Active Directory kullanarak oturum açmak için kullandıkları aynı kimlik bilgilerini kullanarak oturum açabilir.

[Sonraki bölümde](data-storage-options.md) kullanılabilir bir bulut uygulaması için veri depolama seçenekleri bakar.

<a id="resources"></a>
## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/). Azure AD belgelerinde windowsazure.com sitesinde için portal sayfası. Adım adım öğreticiler için bkz. **geliştirme** bölümü.
- [Azure çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/). Azure multi-Factor authentication hakkında belgeler için portal sayfası.
- [Kurumsal hesap kimlik doğrulama seçenekleri](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Visual Studio 2013 yeni proje iletişim kutusunda Azure AD kimlik doğrulama seçenekleri açıklaması.
- [Microsoft desenler ve uygulamalar - federe kimlik düzeni](https://msdn.microsoft.com/library/dn589790.aspx).
- [Nasıl yapılır: Azure Active Directory Sync aracının yükleme](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory Federasyon Hizmetleri 2.0 içerik haritası](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). AD FS 2.0 belgelerine bağlantılar.
- [Bir Windows Azure AD uygulaması ACL tabanlı ve rol tabanlı yetkilendirme](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Örnek uygulama.
- [Azure Active Directory Graph API blogu](https://blogs.msdn.com/b/aadgraphteam/).
- [Erişim denetimi KCG ve karma kimlik altyapısını dizin tümleştirme](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Teknik Ed 2014 oturumunda video Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Önceki](web-development-best-practices.md)
> [İleri](data-storage-options.md)
