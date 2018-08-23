---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Web dağıtımı için yapı sunucunuzu bir TFS Yapılandırma | Microsoft Docs
author: jrjlee
description: Bu konu, Team Foundation Server (TFS) yapı sunucusu oluşturmak ve Team Build ve Internet Informat kullanarak çözümlerinizi dağıtmak için hazırlamak üzere açıklar...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 534a0edf5c26dd2daec3c44e41410f8c5de96c15
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752949"
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Bir TFS derleme sunucusunu Web dağıtımı için yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, derlemek ve Team Build ve Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanarak çözümlerinizi dağıtmak için Team Foundation Server (TFS) yapı sunucusu hazırlama açıklar.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Bir yapı sunucusu oluşturmak ve çözümlerinizi dağıtmak için hazırlamak için sizin yapmanız gerekir:

- Yükleme ve TFS derleme hizmetini yapılandırın.
- Visual Studio 2010'u yükleyin.
- Herhangi bir ürün veya ASP.NET MVC ve .NET Framework sürümleri gibi çözümünüzü oluşturmak için gerekli bileşenleri yükleyin.
- Web dağıtımı 2.0 veya sonraki bir sürümü yükleyin.

Bu konuda, bu yordamları gerçekleştirmek veya mevcut oldukları diğer kaynakları işaret edecek şekilde nasıl gösterir. Görevler ve bu konudaki yönergeler, varsayın:

- Windows Server 2008 R2 Service Pack 1 çalıştıran bir temiz sunucusu derleme ile başlatılıyor.
- Etki alanına katılmış bir statik IP adresiyle sunucusudur.
- TFS uygulama katmanı bölümünde anlatıldığı gibi ayrı bir sunucuya yüklediğiniz [Kurumsal Web Dağıtımı: senaryoya genel bakış](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Kimler bu yordamları gerçekleştirir?

Çoğu durumda, bir TFS Yöneticisi, yapı sunucularını yapılandırmak için sorumlu olacaktır. Bazı durumlarda, Geliştirici Takımı belirli yapı sunucularını sahipliğini alabilir.

## <a name="install-and-configure-the-tfs-build-service"></a>TFS Yapı hizmetini yükleme ve yapılandırma

Bir yapı sunucusunu yapılandırırken, ilk görevi TFS derleme hizmetini yükleme ve yapılandırma sağlamaktır. Bu işlemin bir parçası, şunları yapmanız gerekir:

- TFS Yapı hizmetini yükleyin ve bir hizmet hesabı yapılandırın. Dağıtım, dahil, herhangi bir derleme görevi, derleme hizmeti hesabının kimliğini kullanarak çalışır.
- Oluşturma bir *derleme denetleyicisi* ve bir veya daha fazla *yapı aracılarını*. Her yapı denetleyicisi, yapı aracıları kümesini yönetir. Bir derlemeyi sıraya aldığınızda, derleme denetleyicisi derleme görevi bir kullanılabilir yapı aracısına atar. Her TFS takım projesi koleksiyonunda tek bir yapı denetleyicisi için eşlenmiş.
- Bırakma klasörü yapı çıkışlarınızı için yapılandırın. Bir ağ paylaşımına budur. Tüm yapı çıkışları, web dağıtımı paketleri gibi bırakma klasörüne gönderilir.

[Yönetme Team Foundation derlemesi](https://msdn.microsoft.com/library/ms252495.aspx) MSDN'de bölüm bu görevleri gerçekleştirmek için ihtiyacınız olan tüm kaynakları içerir:

- Derleme hizmeti, yapı denetleyicilerini ve yapı aracıları, bkz: kavramsal genel bakış Team Foundation derlemesi için dahil olmak üzere [bir Team Foundation yapı sistemini anlama](https://msdn.microsoft.com/library/dd793166.aspx).
- Yapı hizmetini yükleme ve yapılandırma hakkında daha fazla bilgi için bkz: [bir yapı makinesini yapılandırma](https://msdn.microsoft.com/library/ms181712.aspx).
- Yapı denetleyicileri oluşturma hakkında daha fazla bilgi için bkz. [oluşturma ve bir yapı denetleyicisi ile iş](https://msdn.microsoft.com/library/ee330987.aspx).
- Derleme aracıları oluşturma hakkında daha fazla bilgi için bkz. [oluşturma ve derleme aracıları ile iş](https://msdn.microsoft.com/library/bb399135.aspx).
- Oluşturma ve bırakma klasörleri yapılandırma hakkında daha fazla bilgi için bkz: [bırakma klasörleri yedeklemek ayarlamak](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Gerekli ürünlerini ve bileşenleri yükleyin

Çözümlerinizi oluşturmak yapı sunucusunu etkinleştirmek için tüm ürünleri, bileşenleri veya çözümünüzün gerektirdiği derlemeleri yüklemeniz gerekir. Herhangi bir web platformu bileşenlerini yüklemeden önce Visual Studio 2010 (tüm sürümler) yapı sunucusuna yüklemeniz gerekir. Bu şekilde, çekirdek Microsoft Build Engine (MSBuild) hedef dosyalarını ve Web yayımlama işlem hattı (WPP) hedef dosyalarını derleme hizmeti için kullanılabilir olmasını sağlar. Web paketleri yapı işleminizin bir parçası olarak dağıtmayı planlıyorsanız, ihtiyacınız olacak Web dağıtımı, Visual Studio yükleyicisi de yüklemeniz gerekir.

Ortak web platformu bileşenlerini yüklemek için en iyi yolu kullanmaktır [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9805118). Bu, her ürünün en son sürümü yüklüyorsanız, ve ayrıca otomatik olarak algılar ve her ürün için tüm önkoşulları yükler sağlar. Durumunda, [Kişi Yöneticisi](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) çözüm bu ürünleri ve bileşenlerini yüklemek için Web Platformu yükleyicisi kullanmalıdır:

- **.NET framework 4.0**. Bu, .NET Framework'ün bu sürümünde oluşturulmuş uygulamaları çalıştırmak için gereklidir.
- **Web dağıtım aracı 2.1 veya üzeri**. Bu seçenek, Web dağıtımı (ve temel alınan çalıştırılabilir, MSDeploy.exe) sunucunuzda yükler. Bu işlemin bir parçası olarak, yükler ve Web dağıtım aracı hizmetini başlatır. Bu hizmet, web paketlerden uzak bir bilgisayara dağıtmanızı sağlar.
- **ASP.NET MVC 3**. Bu, ASP.NET MVC 3 uygulamaları çalıştırmak için gereken bütünleştirilmiş kodları yükler.

**Gerekli ürün ve bileşenlerini yüklemek için**

1. Visual Studio 2010'u yükleyin. Yüklenecek özellikleri seçin. sorulduğunda içermelidir:

    1. Derlemek için gereken herhangi bir programlama dili.
    2. Visual Web Developer. Bu yapı sunucunuzu WPP hedefleri eklendiğinden emin sağlar.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Visual Studio 2010 yüklemesi tamamlandığında, indirme ve yükleme [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (henüz yükleme ortamınızda dahil varsa).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 MSBuild yürütülebilir MSDeploy bulmasını engelleyebilecek bir hatayı çözümler.
3. İndirin ve başlatın [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9805118).
4. Üst kısmındaki **Web Platformu yükleyicisi 3.0** penceresinde tıklayın **ürünleri**.
5. Gezinti bölmesinde, pencerenin sol tarafındaki tıklayarak **çerçeveleri**.
6. İçinde **Microsoft .NET Framework 4** .NET Framework zaten yüklü değilse, satırı tıklatın **Ekle**.

    > [!NOTE]
    > Windows Update aracılığıyla .NET Framework 4.0 zaten yüklü olabilir. Bir ürün veya bileşeni zaten yüklüyse, Web Platformu yükleyicisi bu değiştirerek gösterecektir **Ekle** metnini içeren düğmeye **yüklü**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. İçinde **ASP.NET MVC 3 (Visual Studio 2010)** satır, tıklayın **Ekle**.
8. Gezinti bölmesinde **sunucu**.
9. İçinde **Web dağıtım aracı 2.1** satır, tıklayın **Ekle**.
10. **Yükle**'ye tıklatın. Web Platformu yükleyicisi ürünleri &#x2014; herhangi bir ilişkili bağımlılıkları &#x2014; yüklenecek birlikte listesini gösterir ve lisans koşullarını kabul isteyip istemediğinizi sorar.
11. Lisans koşullarını gözden geçirin ve koşulları kabul ediyorsa **kabul ediyorum**.
12. Yükleme tamamlandığında, tıklayın **son**ve ardından kapatın **Web Platformu yükleyicisi 3.0** penceresi.

> [!NOTE]
> Dağıtım işleminizi VSDBCMD.exe veya SQLCMD.exe gibi araçların kullanılması içeriyorsa, bu yapı sunucunuzda yüklü olduğundan emin olmanız gerekir. VSDBCMD.exe bir Visual Studio araç ve Team Foundation Build'ı yüklediğinizde, sunucuya genellikle eklenir. SQLCMD.exe bir SQL Server aracıdır. SQLCMD.exe bağımsız bir sürümünü indirebilirsiniz [Microsoft SQL Server 2008 R2 özellik paketi](https://go.microsoft.com/?linkid=9805134) sayfası.


## <a name="conclusion"></a>Sonuç

Bu noktada, yapı sunucunuzu oluştururken ve dağıtırken, web uygulama projeleri başlatmak hazırdır. Bir sonraki konu [bir derleme tanımı, destekleyen dağıtım oluşturma](creating-a-build-definition-that-supports-deployment.md), oluşturma ve denetlemek için bir derleme tanımı yapılandırma ve projelerinizi nasıl oluşturulan ve dağıtılan açıklar.

## <a name="further-reading"></a>Daha Fazla Bilgi

Team Build ile çalışma hakkında daha fazla genel yönergeler için bkz. [yönetme Team Foundation derlemesi](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Önceki](adding-content-to-source-control.md)
> [İleri](creating-a-build-definition-that-supports-deployment.md)
