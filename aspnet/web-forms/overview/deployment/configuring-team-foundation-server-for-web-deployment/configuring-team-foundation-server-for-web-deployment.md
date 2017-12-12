---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: "Team Foundation Server için Web dağıtımı yapılandırma | Microsoft Docs"
author: jrjlee
description: "Bu öğretici Team Foundation Server (çözümleri oluşturmak ve web içeriği ortamlarla hedef dağıtmak için TFS) 2010 yapılandırmak nasıl yapacağınızı gösterir. Bu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 72f60841a1381380c0ea6167077420f960180dc7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Team Foundation Server için Web dağıtımı yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu öğretici Team Foundation Server (çözümleri oluşturmak ve web içeriği ortamlarla hedef dağıtmak için TFS) 2010 yapılandırmak nasıl yapacağınızı gösterir. Bu, sürekli tümleştirme (CI) senaryolarında, geliştiricinin bir değişiklik yaparsa her zaman Burada, içerik otomatik olarak dağıtmak içerir. El ile tetikleyici senaryoları, yönetici yapı doğrulandı ve test ortamında doğrulanmış sonra belirli bir yapı hazırlama ortamına dağıtımı tetiklemek için burada isteyebilirsiniz de içerir. Bu öğreticide konuları tüm yapılandırma işleminde size kılavuzluk yapar dahil olmak üzere:
> 
> - TFS'de yeni bir takım projesi oluşturma
> - İçerik kaynak denetimine ekleme.
> - CI ve dağıtımını desteklemek için bir yapı sunucusu nasıl yapılandırılır.
> - Dağıtım mantığı içeren bir derleme tanımı oluşturmak nasıl.
> - Dağıtım için izinleri nasıl yapılandıracağınızı otomatik.
> 
> Bu öğreticileri için bir İtalyanca çeviri ziyaret [http://www.lucamorelli.it](http://www.lucamorelli.it).


Bu öğretici, TFS 2010 yüklü ve bir takım projesi koleksiyonu ilk yapılandırma işleminin bir parçası olarak oluşturulan varsayar. [İçin Visual Studio 2010 Team Foundation Yükleme Kılavuzu](https://go.microsoft.com/?linkid=9805132) bu görevlerde kapsamlı yönergeler sağlar.

## <a name="context"></a>Bağlam

Bu öğreticiler Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimlerine bağlı olarak bir dizi parçası oluşturur Bu öğretici serisi örnek çözümü & #x 2014; kullanan [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) çözüm & #x 2014; Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Communication Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları & #x 2014; proje bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="scenario-overview"></a>Senaryoya genel bakış

Üst düzey senaryo bu öğreticileri için açıklanan [Kurumsal Web Dağıtımı: senaryoya genel bakış](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Bu öğretici başlamadan önce bu konuyu gözden geçirmenizi öneririz.

## <a name="how-to-use-this-tutorial"></a>Bu öğretici nasıl kullanılır?

Bu ilk kez kullanıyorsanız bu öğreticide açıklanan görevleri gerçekleştirdiğiniz veya örnek çözümü kullanan örnekler izleyin istiyorsanız, öğretici konuları sırayla aracılığıyla çalışması gerekir. Alternatif olarak, belirli görevler için ayrı ayrı konularını kılavuz olarak kullanabilirsiniz. Bu öğretici, bu konuları içerir:

- [' De TFS takım projesi oluşturma](creating-a-team-project-in-tfs.md). Bir takım projesi kaynak denetimi, işlem yönetimi ve TFS derlemede çekirdek birimdir. Kaynak Denetim veya derleme tanımı oluşturmak için içerik ekleyebilmeniz için önce bir takım projesi oluşturmanız gerekir.
- [İçerik kaynak denetimine ekleme](adding-content-to-source-control.md). Bir takım projesi oluşturduktan sonra içerik kaynak denetimine eklemeye başlayabilirsiniz. Derlemeleri yapılandırmaya başlamadan önce projeler ve çözümler, tüm dış bağımlılıkları ile birlikte eklemeniz gerekir.
- [Web dağıtımı için derleme sunucu bir TFS Yapılandırma](configuring-a-tfs-build-server-for-web-deployment.md). Takım projesi içeriğinizi oluşturmak istiyorsanız, bir yapı sunucusu yapılandırmanız gerekir. Çoğu durumda bu, TFS yüklemesinden ayrı bir makinede olmalıdır. Yapı sunucusunu yapılandırmak için ihtiyacınız yükleyin ve TFS Yapı hizmetini yapılandırmak, Visual Studio 2010'u yüklemek, yapı denetleyicisi oluşturmak ve yapı aracıları, herhangi bir ürün veya kodunuzu başarılı bir şekilde oluşturmak ve yüklemek için gerekli bileşenleri yüklemek Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı).
- [Dağıtım destekleyen bir yapı tanımı oluşturma](creating-a-build-definition-that-supports-deployment.md). Queuing veya TFS derlemelerde tetikleme başlamadan önce en az bir derleme tanımı için takım projenizin oluşturmanız gerekir. Yapı tanımı yapının hangi şeyler yapı dahil edilmesi gereken, yapı tetiklemesi gereken ve burada, ekip oluşturma çıkışları göndermesi gerektiğini dahil olmak üzere her açıdan tanımlar. Dağıtım mantığı otomatik derlemelerde dahil etmenizi sağlayan özel Microsoft Build Engine (MSBuild) proje dosyalarını çalıştırmak için bir derleme tanımınız yapılandırabilirsiniz.
- [Belirli bir yapı dağıtma](deploying-a-specific-build.md). İçinde çok sayıda senaryoları, hedef ortam için en son sürüme yerine belirli bir yapı dağıtmak istersiniz. Bu durumda, belirli bırakma klasöründen içerik dağıtan bir derleme tanımınız yapılandırabilirsiniz.
- [Takım yapılandırma izinlerini yapı dağıtım](configuring-permissions-for-team-build-deployment.md). Otomatikleştirilmiş derleme işleminin bir parçası içerik dağıtmak üzere derleme service ise, herhangi bir hedef web sunucuları ve veritabanı sunucuları yapı hizmeti hesabına çeşitli izinleri vermeniz gerekir.

## <a name="key-technologies"></a>Temel teknolojileri

Bu öğretici, bu ürün ve teknolojileri otomatik derleme ve web dağıtımını desteklemek için nasıl kullanılacağını üzerinde odaklanır:

- Visual Studio Team Foundation Server 2010
- Ekip Oluşturma ve MSBuild
- Web Dağıtımı

Öğreticinin ayrıca Windows Server 2008 R2, IIS 7.5, SQL Server 2008 R2, ASP.NET 4.0 ve ASP.NET MVC 3 kullanımını dokunur.

## <a name="other-tutorials-in-this-series"></a>Bu serideki diğer öğreticileri

Bu beş eğitim serileri parçası Kurumsal ölçekte web dağıtımı oluşturur. Bu serideki diğer öğreticileri şunlardır:

- [Kuruluş senaryolarında Web uygulamalarını dağıtma](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Bu giriş içeriği bağlamsal arka plan öğretici seri için sağlar. Öğretici senaryo açıklar ve görevleri ve izlenecek yollar seriyi açıklanan daha geniş bir uygulama yaşam döngüsü yönetimi (ALM) işlemine nasıl uyduğunu göstermektedir.
- [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Bu öğretici MSBuild proje dosyalarına kavramsal bir giriş, Web yayımlama ardışık düzen (WPP), Web dağıtımı ve diğer ilgili teknolojiler sağlar. Bunu nasıl bu araçları birlikte karmaşık dağıtım işlemlerini yönetmek için kullanabileceğiniz açıklanmaktadır.
- [Web dağıtımı için sunucu ortamları yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Bu öğreticide (Uzak Aracısı) Web Dağıtım Aracı hizmeti veya Web dağıtımı işleyicisi ve Uzak veritabanı dağıtım kullanarak uzak web paketi dağıtımı dahil olmak üzere çeşitli dağıtım senaryoları desteklemek için Windows sunucularının nasıl yapılandırılacağı açıklanmaktadır. Kendi ortamınız için uygun dağıtım yöntemi seçme hakkında yönergeler sağlar ve Web grubu çerçevesi (WFF) bir sunucu grubundaki tüm web sunucuları arasında dağıtılan web uygulamaları çoğaltmak için nasıl kullanılacağını açıklar.
- [Kurumsal Web dağıtımı Gelişmiş](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Bu öğretici, birden çok ortamlar için veritabanı dağıtımları özelleştirme, dağıtımdan dosya ve klasörleri dışarıda ve dağıtım işlemi sırasında çevrimdışı web uygulamaları alma gibi çeşitli daha gelişmiş dağıtım görevlerinin açıklar .

>[!div class="step-by-step"]
[Sonraki](creating-a-team-project-in-tfs.md)
