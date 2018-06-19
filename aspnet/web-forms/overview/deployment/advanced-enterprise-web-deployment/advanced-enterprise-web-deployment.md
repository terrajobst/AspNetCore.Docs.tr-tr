---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Kurumsal Web dağıtımı Gelişmiş | Microsoft Docs
author: jrjlee
description: Bu öğreticide gerekli veya çok sayıda kurumsal dağıtım senaryoları arzu çeşitli görevleri gerçekleştirmek nasıl yapacağınızı gösterir. İtalyanca translati için...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 892e494b6fde994c4d04952382e4d618d73cad5c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879939"
---
<a name="advanced-enterprise-web-deployment"></a>Gelişmiş Kurumsal Web dağıtımı
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu öğreticide gerekli veya çok sayıda kurumsal dağıtım senaryoları arzu çeşitli görevleri gerçekleştirmek nasıl yapacağınızı gösterir.
> 
> Bu öğreticileri için bir İtalyanca çeviri ziyaret [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Bu eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası oluşturur Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) çözüm&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları proje&#x2014;bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="scenario-overview"></a>Senaryoya genel bakış

Üst düzey senaryo bu öğreticileri için açıklanan [Kurumsal Web Dağıtımı: senaryoya genel bakış](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Bu öğretici başlamadan önce bu konuyu gözden geçirmenizi öneririz.

## <a name="how-to-use-this-tutorial"></a>Bu öğretici nasıl kullanılır?

- Her konular Bu öğretici, kendi içinde yer alan ve belirli sınama ya da kurumsal dağıtım senaryolarında ortaya çıkan sorunu giderir. Bu konular, belirli herhangi bir sırada aracılığıyla iş gerekmez. Ancak, bu öğreticide, bazı gelişmiş görevler ele alınmaktadır. Bu nedenle, kavramlar ve teknikler tanımak, [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) Öğreticisi şunlara değinir en avantajı bu içerikten sağlamak için.
- Bu öğretici, bu konuları içerir:
- ["," Dağıtımı gerçekleştiren](performing-a-what-if-deployment.md). İçinde çok sayıda senaryoları, aslında herhangi bir değişiklik yapmadan önce bir hedef ortamı veya herhangi bir varolan içerik önerilen dağıtım etkisini belirlemek istersiniz. Bu konu, içerik hedef ortam için hiçbir değişiklik yapmadan olarak dağıttıysanız günlük dosyaları ve veritabanı güncelleştirme komut dosyaları oluşturmak için "," dağıtımı nasıl çalıştırabileceğiniz açıklanır. Bu kaynaklar çözümleme olası sorunları Canlı dağıtım çalıştırılmasının nokta yardımcı olabilir.
- [Birden çok ortamlar için veritabanı dağıtımları özelleştirme](customizing-database-deployments-for-multiple-environments.md). Birden çok varış yeri için bir veritabanı projesi dağıttığınızda, genellikle her hedef ortam için dağıtım özellikleri özelleştirmek istersiniz. Hazırlık veya üretim ortamlarında, verilerinizi korumak için artımlı güncelleştirmeler yapmak çok büyük olasılıkla olur ancak örneğin, test ortamlarında, genellikle her dağıtım veritabanını yeniden oluşturmanız. Bu konu, dağıtım mantığınızı her hedef ortam için bir ortama özgü dağıtım yapılandırma (.sqldeployment) dosyası oluşturarak bu özellik değişikliklerini nasıl ekleyebilirsiniz açıklar.
- Veritabanı rol üyeliklerini Test ortamları için dağıtma. Her dağıtımda bir veritabanını yeniden ne zaman&#x2014;Örneğin, sürekli tümleştirme (CI) bir parçası olarak derleme ve test ortamı dağıtma&#x2014;genellikle her veritabanı rol üyeliklerini yapılandırmanız gerekir. Örneğin, genellikle web uygulamasıyla ilişkilendirilmiş uygulama havuzu kimliği izinleri gerekecektir. Bu konuda, bir dağıtım sonrası SQL betiği dağıtım mantığınızı ekleyerek bu işlemi nasıl otomatikleştirebilirsiniz açıklanmaktadır.
- [Üyelik veritabanları kurumsal ortamlarda dağıtmaya](deploying-membership-databases-to-enterprise-environments.md). ASP.NET üyelik veritabanları dağıtım işlemi karmaşık hale getirebilir çeşitli özelliklere sahiptir. Örneğin, yalnızca şema dağıtım veritabanını çalışmaz durumda bırakır. Çoğu senaryoda, her hedef ortamda doğrudan bir üyelik veritabanı oluşturmak için tercih edilir. Ancak, üyelik veritabanının dağıtmak varsa, bu konuda devralınmış çekişmesini karşılamak üzere kullanabileceğiniz yaklaşım bazıları açıklanmaktadır.
- [Dosya ve klasörleri dışarıda dağıtımından](excluding-files-and-folders-from-deployment.md). Bazı senaryolarda, belirli bir hedef ortamlarda, web paketinin içeriğini uyarlamak istersiniz. Örneğin, istemci tarafı hata ayıklamayı destekler, ancak bir hazırlık veya üretim ortamına dağıttığınızda kitaplıklarının küçültülmüş sürümlerini kullanmak için bir test ortamı dağıttığınızda JavaScript kitaplıklarını tam sürümleri eklemek isteyebilirsiniz. Bu konu için paket oluşturma işlemini nasıl belirli dosyaları ve klasörleri dışlayabilirsiniz açıklar.
- [Alma Web çevrimdışı ile Web uygulamalarını dağıtma](taking-web-applications-offline-with-web-deploy.md). Bir hazırlık veya üretim ortamına çözümleri dağıttığınızda, dağıtım süreci boyunca, web uygulamalarınızı çevrimdışı olması genellikle istersiniz. Bu konuda, nasıl ekleyebileceğiniz açıklanmaktadır bir *uygulama\_offline.htm* dosya web uygulamanıza dağıtım işleminin başında ve sonunda kaldırın. Sırada *uygulama\_offline.htm* dosyası yerinde, web uygulaması'na göz tüm kullanıcılar için otomatik olarak yönlendirilir *uygulama\_offline.htm* dosya.
- [MSBuild Windows PowerShell komut dosyaları çalıştırmak](running-windows-powershell-scripts-from-msbuild-project-files.md). Birçok dağıtım senaryosu kayıt defterine özel olay kaynakları ekleme veya SQL Server örnekleri arasında çoğaltmayı yapılandırma gibi daha karmaşık dağıtım sonrası eylemleri gerektirir. Bu Eylemler, genellikle Windows PowerShell komut dosyalarıyla yapılır. Bu konu, Windows PowerShell komut dosyalarını bir Microsoft Build Engine (MSBuild) proje dosyasından derleme ve dağıtım işleminin bir parçası çalıştırmak açıklar.
- [Sorun giderme paketleme işlemi](troubleshooting-the-packaging-process.md). Web yayımlama ardışık düzen (WPP) adlı bir MSBuild özelliği tanımlar **EnablePackageProcessLoggingAndAssert** web uygulama projeleri paketleme işlemiyle ilgili ayrıntılı bilgileri oluşturmak için kullanabilirsiniz. Bu konu özelliği ne yaptığını ve nasıl kullanılacağını açıklar.

## <a name="key-technologies"></a>Temel teknolojileri

Bu öğretici, bu ürün ve teknolojileri otomatik derleme ve web dağıtımını desteklemek için nasıl kullanılacağını üzerinde odaklanır:

- Visual Studio 2010 ve Team Foundation Server (TFS) 2010
- MSBuild ve TFS takım oluşturma
- Internet Information Services (IIS) 7.5
- IIS Web Dağıtım Aracı (Web dağıtımı) 2.1
- VSDBCMD.exe veritabanı dağıtım yardımcı programı

## <a name="other-tutorials-in-this-series"></a>Bu serideki diğer öğreticileri

Bu beş eğitim serileri parçası Kurumsal ölçekte web dağıtımı oluşturur. Bu serideki diğer öğreticileri şunlardır:

- [Kuruluş senaryolarında Web uygulamalarını dağıtma](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Bu giriş içeriği bağlamsal arka plan öğretici seri için sağlar. Öğretici senaryo açıklar ve görevleri ve izlenecek yollar seriyi açıklanan daha geniş bir uygulama yaşam döngüsü yönetimi (ALM) işlemine nasıl uyduğunu göstermektedir.
- [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Bu öğretici MSBuild proje dosyalarına kavramsal bir giriş, WPP, Web dağıtımı ve diğer ilgili teknolojiler sağlar. Bunu nasıl bu araçları birlikte karmaşık dağıtım işlemlerini yönetmek için kullanabileceğiniz açıklanmaktadır.
- [Web dağıtımı için sunucu ortamları yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Bu öğreticide (Uzak Aracısı) Web Dağıtım Aracı hizmeti veya Web dağıtımı işleyicisi ve Uzak veritabanı dağıtım kullanarak uzak web paketi dağıtımı dahil olmak üzere çeşitli dağıtım senaryoları desteklemek için Windows sunucularının nasıl yapılandırılacağı açıklanmaktadır. Kendi ortamınız için uygun dağıtım yöntemi seçme hakkında yönergeler sağlar ve Web grubu çerçevesi (WFF) bir sunucu grubundaki tüm web sunucuları arasında dağıtılan web uygulamaları çoğaltmak için nasıl kullanılacağını açıklar.
- [Team Foundation Server için Web dağıtımı yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Bu öğretici nasıl el ile belirli derlemeleri dağıtımları tetiklenen ve otomatik dağıtım bir CI işleminin bir parçası olarak dahil olmak üzere çeşitli dağıtım senaryoları desteklemek için TFS'ye yapılandırılacağını açıklar.

> [!div class="step-by-step"]
> [Next](performing-a-what-if-deployment.md)
