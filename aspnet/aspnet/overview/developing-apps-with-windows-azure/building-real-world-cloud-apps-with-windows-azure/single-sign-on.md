---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Çoklu oturum açma (Azure ile gerçek bulut uygulamaları derleme) | Microsoft Docs
author: MikeWasson
description: Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 82f2f99154d94074b03d580a0f491053d6f53bde
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873891"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Çoklu oturum açma (Azure ile gerçek bulut uygulamaları derleme)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Bir bulut uygulaması zaman geliştirirken hakkında düşünmek için çok sayıda güvenlik sorunları vardır, ancak bu seri için biz yalnızca birinde odaklanmak: çoklu oturum açma. Bir soru kişiler genellikle sorun şudur: "ı öncelikle uygulamaları Şirketim; çalışanlar için derleme nasıl bulutta bu uygulamaları barındırmak ve bunları my çalışanlar bilmeniz ve şirket içi ortamda bunlar uygulamalar, çalıştırırken kullanan aynı güvenlik modelini kullanmak hala etkinleştirmek barındırıldığı güvenlik duvarı içindeki?" Bu senaryo etkinleştirme yollarından biri, Azure Active Directory (Azure AD) adı verilir. Azure AD kuruluş satır iş kolu (LOB) uygulamaları Internet üzerinden kullanılabilmesini sağlar ve bu uygulamaları da iş ortakları için kullanılabilir olmasını sağlar.

## <a name="introduction-to-azure-ad"></a>Azure AD giriş

[Azure AD](https://docs.microsoft.com/azure/active-directory/) sağlar [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) bulutta. Temel özellikleri aşağıda verilmiştir:

- Şirket içi Active Directory ile tümleştirir.
- Çoklu oturum açma uygulamalarınızda sağlar.
- Gibi açık standartlar destekleyen [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), ve [OAuth 2.0](http://oauth.net/2/).
- Kurumsal destekleyen [grafik REST API'si](https://msdn.microsoft.com/library/hh974476.aspx).

Intranet uygulamalarını imzalamak çalışanlar için kullandığınız bir şirket içi Windows Server Active Directory ortamına olduğunu varsayalım:

![](single-sign-on/_static/image1.png)

Hangi Azure AD yapmanıza olanak sağlar, bulutta bir dizin oluşturun olur. Boş bir özelliktir ve ayarlamak kolay.

Şirket içi Active Directory'nizden tamamen bağımsız olabilir; Herkes içinde istediğiniz ve bunları Internet uygulamalarında kimlik doğrulaması koyabilirsiniz.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Veya şirket içi ile tümleştirmek AD.

![AD ve WAAD tümleştirme](single-sign-on/_static/image3.png)

Şimdi şirket içi kimlik doğrulaması tüm çalışanlar aynı zamanda Internet üzerinden – bir Güvenlik Duvarı'nı açın veya veri merkezinizdeki herhangi bir yeni sunucu dağıtmak zorunda kalmadan kimlik doğrulaması yapabilir. Bilmeniz ve dahili uygulamalara çoklu oturum yeteneğini vermek için bugün kullanan tüm mevcut Active Directory ortamında yüklemenizden yararlanmaya devam edebilirsiniz.

AD ve Azure AD arasındaki bu bağlantı yapmış olduğunuz sonra web uygulamaları ve mobil cihazlarınızı çalışanlarınızın bulut kimlik doğrulaması için de etkinleştirebilirsiniz ve kabul etmek için Office 365, SalesForce.com veya Google apps gibi üçüncü taraf uygulamaları etkinleştirebilirsiniz, çalışanların kimlik bilgileri. Office 365 kullanıyorsanız, Office 365 Azure AD kimlik doğrulama ve yetkilendirme için kullandığı için Azure AD ile ayarlamış.

![3 taraf uygulamalar](single-sign-on/_static/image4.png)

Güzelliği bu yaklaşım, kuruluşunuzun ekler veya bir kullanıcıyı siler dilediğiniz zaman olan veya bir kullanıcı bir parola değişikliklerini, şirket içi ortamınızda günümüzde kullandığınız aynı işlemi kullanın. Tüm şirket içi AD değişiklikleri otomatik olarak bulut ortamı yayılır.

Office 365 Azure AD kimlik doğrulaması için kullandığı için otomatik olarak ayarlanan Azure AD, şirketinizin kullanarak veya iyi haber olan Office 365'e taşıma gerekir. Bu nedenle, kendi uygulamalarında kolayca Office 365 kullanan kimlik doğrulaması kullanabilirsiniz.

## <a name="set-up-an-azure-ad-tenant"></a>Azure AD kiracısı ayarlayın

Azure AD dizini Azure AD adlı [Kiracı](https://technet.microsoft.com/library/jj573650.aspx), ve bir kiracı ayardır oldukça kolaydır. Kavramları göstermek için Azure Yönetim Portalı'nda nasıl yapıldığını göstereceğiz, ancak Elbette gibi diğer portal işlevler ayrıca bir komut dosyası veya yönetim API'si kullanarak bunu yapabilirsiniz.

Yönetim Portalı'nda Active Directory sekmesini tıklatın.

![Portalı'nda WAAD](single-sign-on/_static/image5.png)

Azure hesabınız için otomatik olarak bir Azure AD kiracısı varsa ve tıklayabilirsiniz **Ekle** ek dizinler oluşturmak için sayfanın altındaki düğmesini. Sınama ortamı için diğeri için üretim, örneğin isteyebilirsiniz. Yeni bir dizin adı hakkında dikkatlice düşünün. Dizin için adınızı kullanın ve ardından adınızı yeniden kullanıcıların biri için kullanıyorsanız, kafa karıştırıcı olabilir.

![Dizin Ekle](single-sign-on/_static/image6.png)

Portal oluşturma, silme ve bu ortam içinde kullanıcıları yönetmek için tam destek sahiptir. Örneğin, eklemek için bir kullanıcı Git **kullanıcılar** sekmesinde **Kullanıcı Ekle** düğmesi.

![Kullanıcı Ekle düğmesi](single-sign-on/_static/image7.png)

![Kullanıcı iletişim ekleyin](single-sign-on/_static/image8.png)

Yalnızca bu dizinde var. yeni bir kullanıcı oluşturabilir veya, bir Microsoft Account bu dizini ya da kaydı bir kullanıcı veya başka bir Azure AD dizininden bir kullanıcı olarak bu dizindeki bir kullanıcı olarak kaydedebilirsiniz. (Gerçek bir dizinde ContosoTest.onmicrosoft.com varsayılan etki alanı olacaktır. Ayrıca kendi, contoso.com gibi seçme bir etki alanı kullanabilirsiniz.)

![Kullanıcı türleri](single-sign-on/_static/image9.png)

![Kullanıcı iletişim ekleyin](single-sign-on/_static/image10.png)

Kullanıcı rol atayabilirsiniz.

![Kullanıcı profili](single-sign-on/_static/image11.png)

Ve hesabın bir geçici parola oluşturulur.

![Geçici parola](single-sign-on/_static/image12.png)

Bu şekilde oluşturmalarını hemen bu bulut dizini kullanarak, web uygulamalarınızı oturum açabilir.

Ancak, Kurumsal çoklu oturum açma için harika nedir olan **dizin tümleştirme** sekmesi:

![Dizin tümleştirme sekmesi](single-sign-on/_static/image13.png)

Dizin tümleştirmesi etkinleştirirseniz ve [bir aracı yükleyin](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), bu bulut dizini mevcut şirket içi Active directory'nizle kuruluşunuz içinde zaten kullanmakta olduğunuz eşitleyebilirsiniz. Ardından, dizinde saklanan tüm kullanıcıları bu bulut dizinde görünecektir. Bulut uygulamalarınızı şimdi tüm çalışanlarınız, mevcut Active Directory kimlik bilgilerini kullanarak kimlik doğrulaması yapabilir. Ve tüm bu boş – eşitleme aracı ve Azure AD kendisi.

Bu ekran görüntüleri görebileceğiniz gibi kullanımı kolay, bir sihirbaz aracıdır. Bu yönergelerin, yalnızca temel işlem gösteren bir örnek değildir. Bağlantıları ayrıntılı how-to--BT bilgi için bkz: [kaynakları](#resources) bölümün sonunda bölüm.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image14.png)

Tıklatın **sonraki**ve ardından Azure Active Directory kimlik bilgilerinizi girin.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image15.png)

Tıklatın **sonraki**ve şirket içi enter AD kimlik.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image16.png)

Tıklatın **sonraki**ve ardından AD parolalarınızı karmasını bulutta depolamak istiyorsanız, belirtin.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image17.png)

Bulutta depolayabilir parola karması tek yönlü karma olur; Gerçek parola hiçbir zaman Azure AD içinde depolanır. Karma buluta depolamaya karşı karar verirseniz, kullanmanız gerekir [Active Directory Federasyon Hizmetleri](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Ayrıca [dikkate almanız gereken diğer faktörleri ADFS kullanılıp kullanılmayacağını seçme](https://technet.microsoft.com/library/jj573653.aspx). ADFS seçeneği birkaç ek yapılandırma adımları gerektirir.

Bulutta karma değerlerini depolamak tercih ederseniz, işiniz bittiğinde ve tıkladığınızda dizin eşitleme Aracı'nı başlatır **sonraki**.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image18.png)

Ve birkaç dakika içinde bitirdiniz.

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image19.png)

Yalnızca bu kuruluşunuzdaki Windows 2003 veya daha yüksek bir etki alanı denetleyicisinde çalıştırmanız gerekir. Ve yeniden başlatma gerekmez. İşiniz bittiğinde, kullanıcılarınızın bulutta tümü ve yapabileceğiniz herhangi bir web veya mobil uygulama, SAML, OAuth veya WS-Fed kullanarak çoklu oturum açma.

Bazen biz budur ne kadar güvenli hakkında sorular – Microsoft için kendi duyarlı iş verilerinin kullanıyor mu? Ve yanıt Evet biz yapın. Örneğin iç Microsoft SharePoint sitesinde gidin, [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), oturum açmak için girmem isteniyor.

![Office 365 oturum açma](single-sign-on/_static/image20.png)

Microsoft, ADFS, etkinleştirilmiş şekilde a Microsoft ID girdiğinizde, ADFS oturum açma sayfasına yönlendirilirsiniz.

![ADFS oturum açma](single-sign-on/_static/image21.png)

Ve bir iç Microsoft AD hesapta depolanan kimlik bilgileri girdikten sonra bu dahili uygulama erişimi.

![MS SharePoint sitesi](single-sign-on/_static/image22.png)

Çoğunlukla biz Azure AD kullanılabilir hale geldi, ancak oturum açma işleminiz bulutta Azure AD dizini üzerinden geçmeden önce ayarlamanız ADFS zaten biz oturum açma bir AD sunucusu kullanıyorsunuz. Biz bizim önemli belgeleri, kaynak denetimi, performans Yönetim dosyaları, satış raporları ve daha fazla bulutta koyun ve bunları güvenli hale getirmek için bu tam aynı çözümü kullanıyorsanız.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Azure AD çoklu oturum açma için kullandığı bir ASP.NET uygulaması oluşturma

Visual Studio birkaç ekran görüntüleri gördüğünüz Azure AD çoklu oturum açma için kullanan bir uygulama oluşturmak gerçekten kolay hale getirir.

Yeni bir ASP.NET uygulaması, MVC veya Web Forms oluşturduğunuzda, varsayılan kimlik doğrulama yöntemi ASP.NET kimliğidir. Azure AD ile değiştirmek için tıklattığınız bir **değiştirmek kimlik doğrulama** düğmesi.

![Kimlik doğrulamayı Değiştir](single-sign-on/_static/image23.png)

Kuruluş hesabı seçin, etki alanı adınızı girin ve ardından çoklu oturum açma seçin.

![Kimlik doğrulama iletişim yapılandırın](single-sign-on/_static/image24.png)

Ayrıca uygulama okuma veya okuma/yazma izni dizin veriler için. Bunu yaparsanız, kullanabilirsiniz [Azure grafik REST API'sini](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) kullanıcıların telefon numarasını aramak için son oturum zaman üzerinde vb. ofiste oldukları varsa öğrenin.

Tüm yapmanız gereken - olan Visual Studio için kimlik bilgileri Azure AD kiracınız için yönetici ister ve ardından projenizi ve Azure AD kiracınıza yeni uygulama için yapılandırır.

Projeyi çalıştırdığınızda, bir oturum açma sayfasını görürsünüz ve Azure AD dizininde bir kullanıcı kimlik bilgileriyle oturum açabilirler.

![Kurumsal hesap oturum açma](single-sign-on/_static/image25.png)

![Oturum açanlar](single-sign-on/_static/image26.png)

Azure için uygulama dağıttığınızda, yapmanız gereken tek şey seçin bir **kuruluş kimlik doğrulamasını etkinleştir** onay kutusunu işaretleyin ve bir kez daha Visual Studio sizin için tüm yapılandırma ilgilenir.

![Web yayımlama](single-sign-on/_static/image27.png)

Bu ekran görüntüleri Azure AD kimlik doğrulaması kullanan bir uygulama oluşturmak nasıl oluşturulduğunu gösteren tam bir adım adım öğretici gelir: [Azure Active Directory ile ASP.NET uygulamaları geliştirmek](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Özet

Bu bölümde Azure Active Directory, Visual Studio ve ASP.NET, kuruluşunuzdaki kullanıcılar için Internet uygulamalarında Çoklu oturum açma ayarlamak kolaylaştırır, gördünüz. Kullanıcılarınızın Internet uygulamalarda iç ağınızda Active Directory kullanarak oturum için kullandıkları aynı kimlik bilgilerini kullanarak oturum açabilir.

[Sonraki bölümde](data-storage-options.md) bulut uygulaması için kullanılabilir veri depolama seçenekleri bakar.

<a id="resources"></a>
## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/). Azure AD belgelerinde windowsazure.com site için portal sayfası. Adım adım öğreticiler için bkz: **geliştirme** bölümü.
- [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Azure çok faktörlü kimlik doğrulaması ile ilgili belgeler için portal sayfası.
- [Kurumsal hesap kimlik doğrulama seçeneklerini](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Visual Studio 2013 yeni proje iletişim kutusunda Azure AD kimlik doğrulama seçenekleri açıklaması.
- [Microsoft Patterns and Practices - Federal Kimlik düzeni](https://msdn.microsoft.com/library/dn589790.aspx).
- [Nasıl yapılır: Azure Active Directory eşitleme aracını yüklemek](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory Federasyon Hizmetleri 2.0 içerik haritası](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). ADFS 2.0 hakkındaki belgelere bağlantılar.
- [Windows Azure AD uygulama rolü ve ACL tabanlı yetkilendirme](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Örnek uygulama.
- [Azure Active Directory grafik API'si blogu](https://blogs.msdn.com/b/aadgraphteam/).
- [Erişim denetimi KCG ve karma kimlik altyapınızı dizin tümleştirme](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Teknik Ed 2014 oturumunda video Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Önceki](web-development-best-practices.md)
> [sonraki](data-storage-options.md)
