---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Gelişmiş Kurumsal Web dağıtımı | Microsoft Docs
author: jrjlee
description: Bu öğreticide gerekli veya istenmediğinde kurumsal dağıtım senaryoları, birçok çeşitli görevlerin nasıl gerçekleştirileceği gösterilir. Bir İtalyan translati için...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: adadd921a96b5643eb54cd6f6384ef881ca6ba50
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835089"
---
<a name="advanced-enterprise-web-deployment"></a>Gelişmiş Kurumsal Web dağıtımı
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu öğreticide gerekli veya istenmediğinde kurumsal dağıtım senaryoları, birçok çeşitli görevlerin nasıl gerçekleştirileceği gösterilir.
> 
> Bu öğreticiler için bir İtalyan çevirisi, ziyaret [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Bu öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) çözüm&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="scenario-overview"></a>Senaryoya genel bakış

Bu öğreticiler için üst düzey senaryo açıklanan [Kurumsal Web Dağıtımı: senaryoya genel bakış](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Bu öğretici başlamadan önce bu konuyu gözden geçirmenizi öneririz.

## <a name="how-to-use-this-tutorial"></a>Bu öğreticide kullanma

- Bu öğreticideki konu her bağımsızdır ve belirli bir sınama veya kurumsal dağıtım senaryolarında ortaya çıkan sorunu giderir. Bu konular herhangi bir sırada çalışmak gerek yoktur. Ancak, Bu öğretici, bazı gelişmiş görevler ele almaktadır. Bu nedenle, kavram ve teknikleri ile planladığınızdan, [Kurumsal Web dağıtımı](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) en bu içerikten avantajı kazanmak için öğretici kapsar.
- Bu öğreticide, aşağıdaki konuları içerir:
- [Bir "What If" dağıtımı gerçekleştiren](performing-a-what-if-deployment.md). İçinde çok sayıda senaryo, gerçekte herhangi bir değişiklik yapmadan önce bir hedef ortam veya varolan içeriğin önerilen bir dağıtım etkisini belirlemek isteyebilirsiniz. Bu konuda, içeriği bir hedef ortam için hiçbir değişiklik yapmadan olarak dağıttıysanız, günlük dosyaları ve veritabanı güncelleştirme betikleri oluşturmak için "what IF" dağıtımı nasıl çalıştırabileceğiniz açıklanır. Bu kaynakları çözümleme, canlı bir dağıtım öncesinde olası sorunları bulmaya yardımcı olabilir.
- [Birden çok ortam için veritabanı dağıtımlarını özelleştirme](customizing-database-deployments-for-multiple-environments.md). Birden çok hedefe bir veritabanı projesi dağıttığınızda, genellikle her hedef ortam için dağıtım özellikleri özelleştirmek isteyebilirsiniz. Hazırlık veya üretim ortamlarında, verilerinizi korumak için artımlı güncelleştirmeler yapmak çok daha büyük olasılıkla olacaktır ancak örneğin, test ortamlarında, genellikle her dağıtım veritabanında yeniden. Bu konu, dağıtım mantığınızı her hedef ortam için bir ortama özgü dağıtım (.sqldeployment) yapılandırma dosyası oluşturarak bu özellik değişiklikleri nasıl birleştirebilirsiniz açıklar.
- Test ortamlarına veritabanı rol üyelikleri dağıtma. Her dağıtım veritabanında yeniden zaman&#x2014;gibi bir sürekli tümleştirme (CI) bir parçası olarak oluşturun ve bir test ortamına dağıtın&#x2014;genellikle her veritabanı rolü üyelikleri yapılandırmanız gerekir. Örneğin, web uygulamanızla ilişkili uygulama havuzu kimliği için izinleri vermek genellikle gerekir. Bu konuda, bir dağıtım sonrası SQL komut dosyası dağıtım mantığınızı ekleyerek bu işlemi nasıl otomatikleştirebileceğinizi açıklanmaktadır.
- [Kurumsal ortamlara üyelik veritabanları dağıtma](deploying-membership-databases-to-enterprise-environments.md). ASP.NET üyelik veritabanları, dağıtım işlemi karmaşıklaştırabilir çeşitli özelliklere sahiptir. Örneğin, yalnızca şema dağıtım veritabanını çalışmaz durumda bırakır. Çoğu senaryoda, her hedef ortamda bir doğrudan üyelik veritabanı oluşturmak için tercih edilir. Ancak, bir üyelik veritabanı dağıtmak varsa, bu konuda devralınan zorlukları karşılamak için kullanabileceğiniz yaklaşımları bazılarını açıklar.
- [Dosya ve klasörleri dağıtımdan dışlama](excluding-files-and-folders-from-deployment.md). Bazı senaryolarda, web paketinizi belirli hedef ortamlara içeriğini uyarlamak isteyeceksiniz. Örneğin, istemci tarafı hata ayıklamayı destekler, ancak bir hazırlık veya üretim ortamına dağıtma yaparken küçültülmüş kitaplığı sürümünü kullanmak için bir test ortamı dağıttığınızda tam sürümlerini JavaScript kitaplıklarını eklemek isteyebilirsiniz. Bu konu nasıl, belirli dosyaları ve klasörleri paket oluşturma işleminden hariç tutabilirsiniz açıklar.
- [Web uygulamalarını çevrimdışı yapma Web dağıtma](taking-web-applications-offline-with-web-deploy.md). Bir hazırlık veya üretim ortamına çözümleri dağıttığınızda, genellikle web uygulamalarınızı çevrimdışı dağıtım işlemi süresince almak istersiniz. Bu konu nasıl ekleyebileceğinizi açıklar bir *uygulama\_offline.htm* dosyasını web uygulamanıza dağıtım işleminin başlangıcında ve sonunda kaldırın. Sırada *uygulama\_offline.htm* dosyası yerinde, web uygulaması'na göz tüm kullanıcılar için otomatik olarak yönlendirilir *uygulama\_offline.htm* dosya.
- [Windows PowerShell betikleri çalıştırma MSBuild'den](running-windows-powershell-scripts-from-msbuild-project-files.md). Birçok dağıtım senaryosu, kayıt defterine özel olay kaynakları ekleme veya çoğaltma SQL Server örnekleri arasında yapılandırma gibi daha karmaşık dağıtım sonrası eylemleri gerektirir. Bu Eylemler, genellikle Windows PowerShell komut dosyaları aracılığıyla gerçekleştirilir. Bu konu, Windows PowerShell betikleri ile derleme ve dağıtım sürecinin bir parçası olarak bir Microsoft Build Engine (MSBuild) proje dosyasından çalıştırın açıklar.
- [Paketleme işleminin sorunlarını giderme](troubleshooting-the-packaging-process.md). Web yayımlama işlem hattı (WPP) adlı bir MSBuild özellik tanımlar **EnablePackageProcessLoggingAndAssert** web uygulama projeleri için paketleme işlemi hakkında ayrıntılı bilgi oluşturmak için kullanabilirsiniz. Bu konu, bu özellik şunu yapar ve nasıl kullanılacağını açıklar.

## <a name="key-technologies"></a>Anahtar teknolojileri

Bu öğretici, bu ürün ve teknolojileri otomatik derleme ve web dağıtımını desteklemek için nasıl kullanılacağını üzerinde odaklanır:

- Visual Studio 2010 ve Team Foundation Server (TFS) 2010
- MSBuild ve TFS Team Build
- Internet Information Services (IIS) 7.5
- IIS Web Dağıtım Aracı (Web dağıtımı) 2.1
- VSDBCMD.exe veritabanı dağıtım yardımcı programı

## <a name="other-tutorials-in-this-series"></a>Bu serideki diğer öğreticileri

Bu bir öğretici serisinde, beş parçası üzerinde Kurumsal ölçekte web dağıtımı oluşturur. Diğer öğreticiler serisinde şunlardır:

- [Kurumsal senaryolarda Web uygulamaları dağıtma](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Bu Tanıtım içerik bağlamsal arka plan için öğretici serisinin sağlar. Öğretici senaryo açıklar ve daha geniş bir uygulama yaşam döngüsü yönetimi (ALM) işleme görevleri ve izlenecek yollar seri boyunca açıklanan nasıl getireceğinizi göstermektedir.
- [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Bu öğretici, MSBuild proje dosyalarındaki kavramsal bir giriş, WPP, Web dağıtımı ve diğer ilgili teknolojileri sağlar. Bunu nasıl bu araçları birlikte karmaşık dağıtım işlemlerini yönetmek için kullanabileceğiniz açıklanmaktadır.
- [Sunucu ortamlarını Web dağıtımı için yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Bu öğreticide, Web dağıtım aracı hizmetini (Uzak Aracı) veya Web dağıtımı işleyicisi ve Uzak veritabanı dağıtımı kullanarak uzak web paketi dağıtımı dahil olmak üzere çeşitli dağıtım senaryoları desteklemek için Windows sunucularının nasıl yapılandırılacağı açıklanır. Kendi ortamınız için uygun dağıtım yöntemi seçme hakkında rehberlik sağlar ve Web Farm Framework (WFF) bir sunucu grubundaki tüm web sunucuları arasında dağıtılmış web uygulamalarında çoğaltmak için nasıl kullanılacağını açıklar.
- [Web dağıtımı için Team Foundation Server yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Bu öğreticide derlemelerini dağıtımları el ile tetiklenen ve otomatik dağıtım bir CI işlemine bir parçası olarak dahil olmak üzere çeşitli dağıtım senaryoları desteklemek üzere TFS'ye yapılandırma açıklanır.

> [!div class="step-by-step"]
> [Next](performing-a-what-if-deployment.md)
