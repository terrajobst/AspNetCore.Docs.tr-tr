---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Web dağıtımı için derleme sunucu bir TFS Yapılandırma | Microsoft Docs
author: jrjlee
description: Bu konu, yapı ve çözümlerinizi ekip ve Internet Informat kullanarak dağıtmak için bir Team Foundation Server (TFS) yapı sunucuyu hazırlamak üzere açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7b3130ca7d36ffec457e1871fa62c1077b5e3174
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
ms.locfileid: "30892523"
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>TFS yapı sunucu için Web dağıtımı yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, yapı ve çözümlerinizi ekip ve Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanarak dağıtmak için bir Team Foundation Server (TFS) yapı sunucuyu hazırlamak üzere açıklar.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları proje&#x2014;bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Derleme ve çözümlerinizi dağıtmak için bir yapı sunucuyu hazırlamak üzere için gerekir:

- Yükleyin ve TFS yapı hizmeti yapılandırın.
- Visual Studio 2010'u yükleyin.
- Herhangi bir ürün veya ASP.NET MVC ve .NET Framework sürümleri gibi çözümünüzü derlemek için gerekli bileşenlerini yükleyin.
- Web dağıtımı 2.0 veya sonraki sürümünü yükleyin.

Bu konuda bu yordamları gerçekleştirmek ya da bunlar var olduğu diğer kaynakları noktası nasıl yapacağınızı gösterir. Görevleri ve bu konudaki yönergeler, varsayın:

- Windows Server 2008 R2 Service Pack 1 çalıştıran bir temiz server yapı ile başlanır.
- Sunucu etki alanına katılmış statik bir IP adresine sahip değil.
- TFS uygulama katmanında açıklandığı gibi ayrı bir sunucuya yüklediniz [Kurumsal Web Dağıtımı: senaryoya genel bakış](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Kim, bu yordamları gerçekleştirir mi?

Çoğu durumda, bir TFS yönetici yapı sunucularını yapılandırmak için sorumlu olacaktır. Bazı durumlarda, geliştirici takım belirli yapı sunucuları sahipliğini alabilir.

## <a name="install-and-configure-the-tfs-build-service"></a>TFS Yapı hizmetini yükleme ve yapılandırma

Yapı sunucusunu yapılandırdığınızda, ilk TFS Yapı hizmetini yüklemek ve yapılandırmak için bir görevdir. Bu işlemin bir parçası olarak, ihtiyacınız vardır:

- TFS Yapı hizmetini yükleyin ve bir hizmet hesabı yapılandırın. Dağıtım dahil olmak üzere, herhangi bir derleme görevi yapı hizmeti hesabının kimliğini kullanarak çalışacaktır.
- Oluşturma bir *yapı denetleyicisi* ve bir veya daha fazla *yapı aracıları*. Her bir yapı denetleyicisi yapı aracıları kümesini yönetir. Bir yapıyı sıraya al, yapı denetleyicisi derleme görevi kullanılabilir yapı aracıyı atar. Her TFS takım projesi koleksiyonunda tek bir yapı denetleyicisi için eşlenir.
- Bırakma klasörü yapı çıktıları için yapılandırın. Bir ağ paylaşımına budur. Herhangi bir web dağıtımı paketleri gibi çıkışları derleme, bırakma klasörüne gönderilir.

[Yönetme Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) MSDN'de bölüm bu görevleri gerçekleştirmek için ihtiyacınız olan tüm kaynakları içerir:

- Team Foundation derlemesi kavramsal bir genel yapı hizmeti, yapı denetleyicileri ve yapı aracıları, bkz: de dahil olmak üzere [bir Team Foundation Build System anlama](https://msdn.microsoft.com/library/dd793166.aspx).
- Yükleme ve Yapılandırma hizmeti yapılandırma hakkında daha fazla bilgi için bkz: [bir yapı makinesini yapılandırma](https://msdn.microsoft.com/library/ms181712.aspx).
- Yapı denetleyicisi oluşturma hakkında daha fazla bilgi için bkz: [oluşturma ve bir yapı denetleyicisi çalışmak](https://msdn.microsoft.com/library/ee330987.aspx).
- Derleme aracıları oluşturma hakkında daha fazla bilgi için bkz: [oluşturma ve derleme aracıları ile çalışma](https://msdn.microsoft.com/library/bb399135.aspx).
- Oluşturma ve açılan klasörleri yapılandırma hakkında daha fazla bilgi için bkz: [bırakma klasörleri yedeklemek ayarlamak](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Gerekli ürün ve bileşenlerini yükler

Çözümlerinizi oluşturmaya yapı sunucusunu etkinleştirmek için tüm ürünleri, bileşenleri veya çözümünüzü gerektiren derlemeler yüklemeniz gerekir. Tüm web platformu bileşenlerini yüklemeden önce Visual Studio 2010 (tüm sürümler) yapı sunucusuna yüklemeniz gerekir. Bu çekirdek Microsoft Build Engine (MSBuild) hedef dosyalarını ve Web yayımlama ardışık düzen (WPP) hedef dosyaları yapı hizmeti için kullanılabilir olmasını sağlar. Visual Studio yükleyicisi, ayrıca Web web paketleri yapı işleminizin bir parçası olarak dağıtmayı planlıyorsanız, gerekir dağıttığınız, yüklemeniz gerekir.

Ortak web platformu bileşenlerini yüklemek için en iyi yolu kullanmaktır [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9805118). Bu, her ürünün en son sürümü yüklüyorsanız, ve ayrıca otomatik olarak algılar ve her ürün için herhangi bir önkoşulu yükler sağlar. Durumunda [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) çözüm bu ürünleri ve bileşenlerini yüklemek için Web Platformu yükleyicisi kullanmalıdır:

- **.NET Framework 4.0**. Bu, .NET Framework'ün bu sürümünde oluşturulan uygulamaları çalıştırmak için gereklidir.
- **Web dağıtım aracı 2.1 veya üzeri**. Bu, sunucunuzda Web dağıtımı (ve onun temel yürütülebilir MSDeploy.exe) yükler. Bu işlemin bir parçası olarak bu yükler ve Web dağıtım aracı hizmetini başlatır. Bu hizmetin web paketleri uzak bir bilgisayardan dağıtmanıza olanak tanır.
- **ASP.NET MVC 3**. Bu, ASP.NET MVC 3 uygulamaları çalıştırmak için gereken derlemeler yükler.

**Gerekli ürün ve bileşenlerini yüklemek için**

1. Visual Studio 2010'u yükleyin. Yüklenecek özellikleri seçin isteyip istemediğiniz sorulduğunda içermelidir:

    1. Derlemek için gereken herhangi bir programlama diline.
    2. Visual Web Developer. Bu WPP hedefleri derleme sunucunuza eklenen sağlar.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Visual Studio 2010 yüklemesi tamamlandığında, indirin ve yükleyin [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (Bunu önceden yükleme medyasında bulunan dahil edilmiş olup olmadığını).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 MSBuild yürütülebilir MSDeploy bulma engelleyen bir hata çözümler.
3. Karşıdan yükle ve Başlat [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9805118).
4. Üstündeki **Web Platformu yükleyicisi 3.0** penceresinde tıklatın **ürünleri**.
5. Gezinti bölmesinde, pencerenin sol tarafındaki tıklatın **çerçeveler**.
6. İçinde **Microsoft .NET Framework 4** .NET Framework zaten yüklü değilse, satır tıklatın **Ekle**.

    > [!NOTE]
    > Windows Update aracılığıyla .NET Framework 4.0 zaten yüklenmiş. Bir ürün veya bileşenin zaten yüklüyse, Web Platformu yükleyicisi bu değiştirerek gösterecektir **Ekle** düğmesi metni ile **yüklü**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. İçinde **ASP.NET MVC 3 (Visual Studio 2010)** satır, tıklatın **Ekle**.
8. Gezinti bölmesinde **Server**.
9. İçinde **Web dağıtım aracı 2.1** satır, tıklatın **Ekle**.
10. **Yükle**'ye tıklatın. Web Platformu yükleyicisi ürünleri &#x2014; herhangi bir ilişkili bağımlılıkları &#x2014; yüklenecek birlikte listesini gösterir ve lisans koşullarını kabul isteyip istemediğinizi sorar.
11. Lisans koşullarını gözden geçirin ve koşullarını kabul ederseniz tıklayın **kabul ediyorum**.
12. Yükleme tamamlandığında, tıklatın **son**ve ardından kapatın **Web Platformu yükleyicisi 3.0** penceresi.

> [!NOTE]
> Dağıtım işlemi VSDBCMD.exe veya SQLCMD.exe gibi araçlarının kullanımı içeriyorsa, bu derleme sunucunuzda yüklendiğinden emin olmak gerekir. VSDBCMD.exe bir Visual Studio aracıdır ve Team Foundation Build yüklediğinizde sunucuya genellikle eklenir. SQLCMD.exe bir SQL Server aracıdır. SQLCMD.exe tek başına bir sürümünü yükleyebilirsiniz [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) sayfası.


## <a name="conclusion"></a>Sonuç

Bu noktada, yapı sunucunuzu oluşturma ve web uygulama projeleri dağıtma başlatmak hazırdır. Sonraki konuyu [bir yapı tanımı olduğunu destekleyen dağıtım oluşturma](creating-a-build-definition-that-supports-deployment.md), oluşturma ve denetlemek için bir derleme tanımınız yapılandırmak ve projelerinizi nasıl oluşturulan ve dağıtılan açıklar.

## <a name="further-reading"></a>Daha Fazla Bilgi

Ekip ile çalışma hakkında daha fazla genel yönergeler için bkz [yönetme Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Önceki](adding-content-to-source-control.md)
> [sonraki](creating-a-build-definition-that-supports-deployment.md)
