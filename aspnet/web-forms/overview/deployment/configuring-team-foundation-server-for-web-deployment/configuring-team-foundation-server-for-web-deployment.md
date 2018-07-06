---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Team Foundation Server Web dağıtımı için yapılandırma | Microsoft Docs
author: jrjlee
description: Bu öğreticide, Team Foundation Server (çözümleri oluşturmanıza ve web içeriği çeşitli hedef ortamlara dağıtmak için TFS) 2010 yapılandırma gösterilmektedir. Bu...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 0155f8dc4ca05a91ed8921f83aa6fa1b0b59c1a5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829693"
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Team Foundation Server Web dağıtımı için yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu öğreticide, Team Foundation Server (çözümleri oluşturmanıza ve web içeriği çeşitli hedef ortamlara dağıtmak için TFS) 2010 yapılandırma gösterilmektedir. Bu, bir geliştirici bir değişiklik yaparsa her zaman burada içerik otomatik olarak dağıtabilmeniz, sürekli tümleştirme (CI) senaryolar içerir. Derleme doğrulandı ve test ortamında doğrulanması sonra belirli bir yapının bir hazırlık ortamına dağıtım tetiklemek için yöneticinin burada isteyebilirsiniz, el ile tetikleyici senaryoları da dahil edebilirsiniz. Konular bu öğreticideki tüm yapılandırma işlemi boyunca size rehberlik dahil olmak üzere:
> 
> - TFS'de bir takım projesi oluşturma
> - Kaynak denetimine içerik ekleme yapma.
> - CI ve dağıtımını desteklemek için bir yapı sunucusu nasıl yapılandırılır.
> - Nasıl dağıtım mantığı içeren yapı tanımı oluşturun.
> - Otomatik dağıtım için izinleri yapılandırma.
> 
> Bu öğreticiler için bir İtalyan çevirisi, ziyaret [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Bu öğreticide, TFS 2010 yüklü ve bir takım projesi koleksiyonunu ilk yapılandırma işleminin bir parçası olarak oluşturduğunuz varsayılır. [Visual Studio 2010 için Team Foundation yükleme kılavuzuna](https://go.microsoft.com/?linkid=9805132) bu görevler hakkında kapsamlı bir kılavuz sağlar.

## <a name="context"></a>Bağlam

Bu öğreticiler Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimlerine göre bir dizi parçası oluşturur Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) çözüm&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="scenario-overview"></a>Senaryoya genel bakış

Bu öğreticiler için üst düzey senaryo açıklanan [Kurumsal Web Dağıtımı: senaryoya genel bakış](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Bu öğretici başlamadan önce bu konuyu gözden geçirmenizi öneririz.

## <a name="how-to-use-this-tutorial"></a>Bu öğreticide kullanma

Bu ilk kez kullanıyorsanız bu öğreticide açıklanan görevlerin gerçekleştirdiğiniz veya örnek çözümü kullanarak örnekleri izlemek istiyorsanız, sırayla öğretici konuları ile çalışması gerekir. Alternatif olarak, tek tek ilgili konulara belirli görevler için kılavuz olarak kullanabilirsiniz. Bu öğreticide, aşağıdaki konuları içerir:

- [TFS'de bir takım projesi oluşturma](creating-a-team-project-in-tfs.md). Bir takım projesi kaynak denetimi, işlem yönetimi ve TFS yapı için temel birimdir. Bir takım projesi kaynak denetimi veya yapı tanımları oluşturmak için içerik eklemeden önce oluşturmanız gerekir.
- [Kaynak denetimine içerik ekleme](adding-content-to-source-control.md). Bir ekip projesi oluşturduktan sonra içerik kaynak denetimine eklemeye başlayabilirsiniz. Yapılar yapılandırmaya başlamadan önce projeler ve çözümler, herhangi bir dış bağımlılığın birlikte eklemeniz gerekir.
- [Web dağıtımı için yapı sunucunuzu bir TFS Yapılandırma](configuring-a-tfs-build-server-for-web-deployment.md). Takım projesi içeriğinizi oluşturmak istiyorsanız, bir yapı sunucusunu yapılandırmak gerekir. Çoğu durumda, ayrı bir makinede TFS yüklemenizin bu olmalıdır. Bir yapı sunucusunu yapılandırmak için gereken yükleyin ve TFS derleme hizmetini yapılandırın, Visual Studio 2010'u yükleyin, yapı denetleyicileri oluşturun ve yapı aracıları, herhangi bir ürün veya kodunuzu başarıyla derlenmesi ve yüklemek için gerekli bileşenleri yükleme Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı).
- [Dağıtımı destekleyen bir derleme tanımı oluşturma](creating-a-build-definition-that-supports-deployment.md). Sırada beklerken veya TFS yapılarında tetikleme başlamadan önce takım projeniz için en az bir yapı tanımı oluşturmak gerekir. Yapı tanımı hangi şey yapıya dahil edilmesi gereken, yapı tetiklemesi gereken ve Team Build yapı çıkışlarını nereye göndermemelidir dahil olmak üzere, yapı her yönüyle tanımlar. Otomatik yapılarınızı dağıtım mantığı eklemenize olanak sağlayan özel Microsoft Build Engine (MSBuild) proje dosyalarını çalıştırmak için derleme tanımı yapılandırabilirsiniz.
- [Belirli bir derlemeyi dağıtma](deploying-a-specific-build.md). İçinde çok sayıda senaryoları, en son sürüme yerine belirli bir yapıyı bir hedef ortama dağıtmak isteyebilirsiniz. Bu durumda, belirli bir bırakma klasöründen içerik dağıtan bir yapı tanımı yapılandırabilirsiniz.
- [Takım izinlerini yapılandırma derleme dağıtım](configuring-permissions-for-team-build-deployment.md). Derleme hizmeti bir otomatik yapı işleminin bir parçası içerik dağıtmak için ise, yapı hizmeti hesabı herhangi bir hedef web sunucuları ve veritabanı sunucuları üzerinde çeşitli izinler gerekir.

## <a name="key-technologies"></a>Anahtar teknolojileri

Bu öğretici, bu ürün ve teknolojileri otomatik derleme ve web dağıtımını desteklemek için nasıl kullanılacağını üzerinde odaklanır:

- Visual Studio Team Foundation Server 2010
- Takım derlemesi ve MSBuild
- Web Dağıtımı

Öğretici, Windows Server 2008 R2, IIS 7.5, SQL Server 2008 R2, ASP.NET 4.0 ve ASP.NET MVC 3 kullanımını da ele alınır.

## <a name="other-tutorials-in-this-series"></a>Bu serideki diğer öğreticileri

Bu bir öğretici serisinde, beş parçası üzerinde Kurumsal ölçekte web dağıtımı oluşturur. Diğer öğreticiler serisinde şunlardır:

- [Kurumsal senaryolarda Web uygulamaları dağıtma](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Bu Tanıtım içerik bağlamsal arka plan için öğretici serisinin sağlar. Öğretici senaryo açıklar ve daha geniş bir uygulama yaşam döngüsü yönetimi (ALM) işleme görevleri ve izlenecek yollar seri boyunca açıklanan nasıl getireceğinizi göstermektedir.
- [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Bu öğretici, MSBuild proje dosyalarındaki kavramsal bir giriş, Web yayımlama işlem hattı (Usewpp_copywebapplication), Web dağıtımı ve diğer ilgili teknolojileri sağlar. Bunu nasıl bu araçları birlikte karmaşık dağıtım işlemlerini yönetmek için kullanabileceğiniz açıklanmaktadır.
- [Sunucu ortamlarını Web dağıtımı için yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Bu öğreticide, Web dağıtım aracı hizmetini (Uzak Aracı) veya Web dağıtımı işleyicisi ve Uzak veritabanı dağıtımı kullanarak uzak web paketi dağıtımı dahil olmak üzere çeşitli dağıtım senaryoları desteklemek için Windows sunucularının nasıl yapılandırılacağı açıklanır. Kendi ortamınız için uygun dağıtım yöntemi seçme hakkında rehberlik sağlar ve Web Farm Framework (WFF) bir sunucu grubundaki tüm web sunucuları arasında dağıtılmış web uygulamalarında çoğaltmak için nasıl kullanılacağını açıklar.
- [Gelişmiş Kurumsal Web dağıtımı](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Bu öğreticide dağıtım işlemi sırasında web uygulamalarını çevrimdışı alma birden çok ortam için veritabanı dağıtımlarını özelleştirme ve dosya ve klasörleri dağıtımdan dışlama gibi çeşitli daha gelişmiş dağıtım görevlerini, nasıl yapılacağını anlatmaktadır. .

> [!div class="step-by-step"]
> [Next](creating-a-team-project-in-tfs.md)
