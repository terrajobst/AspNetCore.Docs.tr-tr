---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Web kurumsal dağıtım | Microsoft Docs
author: jrjlee
description: Bu öğretici, çok sayıda devel için Kurumsal ölçekte web uygulamalarının dağıtımını yönetirken karşılaşabileceğiniz sorunları karşılamak üzere açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: fc463cb689f4f63a12788b80958c9fc8fe20119d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890469"
---
<a name="web-deployment-in-the-enterprise"></a>Kurumsal Web dağıtımı
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu öğretici, çok sayıda geliştirme, test, hazırlama ve üretim ortamları için Kurumsal ölçekte web uygulamalarının dağıtımını yönetirken karşılaşabileceğiniz sorunları karşılamak üzere açıklar. Öğretici bir karışımını çeşitli ortak görevler ve yordamların yol göstermesi için kavramsal ve görev odaklı içeriği ile birlikte bir başvuru çözüm içerir.
> 
> Bu öğreticileri için bir İtalyanca çeviri ziyaret [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Kurumsal Dağıtım zorlukları

Karmaşık, Kurumsal ölçekte çözümleri dağıtımını yönetmek için baktığınızda kuruluşlar çoğunlukla bu sorunlarla karşılaşırsınız:

- Geliştirici gibi birden çok ortamlara projeleri dağıtmak veya sınama ortamlarında, platformlar ve üretim sunucuları hazırlama gerekir. Çözüm, her ortam için farklı yapılandırma ayarlarıyla dağıtılması gerekiyor.
- Birden fazla bağımlı proje aynı anda tek bir adımı veya otomatikleştirilmiş derleme ve dağıtım işleminin bir parçası olarak dağıtmanız gerekir.
- Bir otomatik işleminden sürücü dağıtımına olması gerekir. Örneğin, bir sürekli tümleştirme (CI) işlem yeni kod işaretlendiğinde bir test ortamı için web uygulamalarını dağıtmak için kullanmak istediğiniz.
- Geliştiricilerin doğru yapılandırma ayarları veya her hedef ortam için gerekli kimlik bilgilerine sahip tahmin edilemez şekilde dağıtım işlemini denetleyen ve dış Visual Studio'dan dağıtım değişkenleri ayarlayın mümkün olması gerekir.
- Şema tabanlı veritabanı projeleri dağıtmak ve sonraki dağıtımlarda mevcut verilerinizi korumak gerekir.
- Kullanıcı hesabı verileri dağıtmadan üyelik veritabanları geçici düzenli olarak dağıtmanız gerekir. Ayrıca var olan kullanıcı hesabı verilerini kaybetmeden dağıtılan üyelik veritabanı şeması güncelleştirmeniz gerekebilir.
- İçerik çeşitli hedef ortamlara dağıttığınızda belirli dosyaları veya klasörleri dışarıda gerekir.

## <a name="overview-of-approach"></a>Yaklaşım genel bakış

Bu serideki diğer öğreticileri birlikte Bu öğretici, yukarıda açıklanan zorlayan üst düzey bu yaklaşımı kullanır.

- **Genel derleme ve dağıtım işlemini kontrol eden özel Microsoft Build Engine (MSBuild) proje dosyalarını kullanın.**
- Bu yapı ve çözümdeki her projeye tek, kodlanabilir işleminin bir parçası dağıtmanızı sağlar.
- Ortama özgü ayarlar basit ortama özgü proje dosyaları kullanılarak yapılandırılır. Visual Studio – merkezli bir yaklaşım çözüm yapılandırmaları kullanmanın aksine ve farklı ortamlar için dağıtımları yapılandırmak için yayımlama profillerini, bu yaklaşım yapılandırmak ve dış Visual Studio'dan dağıtım işlemi yönetmenize olanak tanır. Başka bir deyişle, geliştiriciler bağlantı dizeleri, hizmet uç noktaları, sunucu kimlik bilgileri ve diğer dağıtım değişkenlerin hedef ortamlar için bilgi ilerletmek yoktur.
- Özel proje dosyalarını ekip Team Foundation Server (TFS) iş akışının parçası olarak tarafından çağrılabilir. Bu otomatik dağıtım CI senaryoları için yapılandırmanıza olanak sağlar.

**Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) paketi ve web uygulama projeleri dağıtmak için kullanın.**

- Web dağıtımı paketi ve bağımlılıkları, yapılandırma ayarlarını, güvenlik ayarlarını ve diğer gereksinimler ile birlikte hedef IIS web sunucusu için web uygulaması içeriğinizi dağıtmanıza olanak sağlayan bir çerçeve sağlar.
- İçinde özel MSBuild proje dosyaları için tüm paketleme ve dağıtım işlemini kontrol edebilirsiniz. Bağlantı dizeleri, hizmet uç noktaları ve IIS hedef ayrıntıları gibi web dağıtımı paketi eşlik yapılandırma ayarlarını da değiştirebilirsiniz.
- Web yayımlama ardışık birlikte Web dağıtımı çok sayıda dağıtımlarınızı özelleştirmenize olanak tanıyan genişletilebilirlik noktaları sağlar. Örneğin, web dağıtımı paketlerinden istenmeyen dosyaları ve klasörleri dışarıda bırak kolaydır.

**Dağıtma ve veritabanı şemalarını güncelleştirmek için VSDBCMD.exe yardımcı programını kullanın.**

- VSDBCMD, Visual Studio veritabanı projesi derlerken, oluşturulan bir veritabanı şema dosyası (.dbschema) veritabanlarından dağıtmanıza olanak tanır. Buna karşılık, Web dağıtımı veritabanı dağıtım işlevselliğin var olan veritabanlarını yerel bir SQL Server örneğinin dağıtımı için daha uygundur.
- Veritabanı projeleri dağıtmak için Visual Studio'nun işlevselliği VSDBCMD varolan bir hedef veritabanına fark güncelleştirmelerini dağıtmanıza olanak tanır. Bu, veritabanı şemasını yükseltirken mevcut verileri korumak sağlar.
- İçinde özel MSBuild proje dosyalarınıza ait VSDBCMD komutlar yürütebilir.

## <a name="content-map"></a>İçerik haritası

Bu öğretici, dört ana alanlarına kalan konuları içerir.

Bu konular başvuru çözüm tanıtmaktadır&#x2014;Contact Manager çözüm&#x2014;ve indirir ve yerel makinenizde yapılandırma anlatmaktadır:

- [Kişi Yöneticisi Çözümü](the-contact-manager-solution.md)
- [Kişi Yöneticisi Çözümünü Ayarlama](setting-up-the-contact-manager-solution.md)

Bu konularda MSBuild proje dosyalarını tanıtmak, nasıl oluşturabilir ve özel proje dosyalarını kullanmak ve Contact Manager çözüm için dağıtım işleminde size rehberlik tanımlar:

- [Proje Dosyasını Anlama](understanding-the-project-file.md)
- [Derleme İşlemini Anlama](understanding-the-build-process.md)

Web uygulama dağıtımı paketleme ve derleme işleminin nasıl çalıştığı, yapı işlemi Web yayımlama ardışık ile nasıl tümleşik çalıştığını, dağıtım parametrelerini değiştirme ve web paketleri hedefe dağıtma dahil olmak üzere, bu konularda açıklanmaktadır ortamları:

- [Web Uygulaması Projelerini Derleme ve Paketleme](building-and-packaging-web-application-projects.md)
- [Web Paketi Dağıtımı için Parametreleri Yapılandırma](configuring-parameters-for-web-package-deployment.md)
- [Web Paketleri Dağıtma](deploying-web-packages.md)

- [Veritabanı projeleri dağıtma](deploying-database-projects.md) olumlu ve olumsuz her yaklaşımın birlikte Visual Studio veritabanı projelerini dağıtmak için kullanabileceğiniz farklı teknikleri açıklar. [Oluşturma ve bir dağıtım komut dosyası çalıştırma](creating-and-running-a-deployment-command-file.md) dağıtım mantığınızı yalıtır ve karmaşık çözümleri tek adımlı işlem olarak dağıtmanıza olanak tanır basit komut dosyası oluşturmayı açıklar.
- Son olarak, [Web paketlerini el ile yükleme](manually-installing-web-packages.md) IIS'e web paketleri almanızı göstererek öğreticiyi sonlandırır.

## <a name="key-technologies"></a>Temel teknolojileri

Bu öğreticide öncelikle bu teknolojiler derleme ve dağıtım yönetmek için konulara bakın:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- VSDBCMD.exe veritabanı dağıtım yardımcı programı

## <a name="other-tutorials-in-this-series"></a>Bu serideki diğer öğreticileri

Bu beş eğitim serileri parçası Kurumsal ölçekte web dağıtımı oluşturur. Bu serideki diğer öğreticileri şunlardır:

- [Kuruluş senaryolarında Web uygulamalarını dağıtma](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Bu giriş içeriği bağlamsal arka plan öğretici seri için sağlar. Öğretici senaryo açıklar ve görevleri ve izlenecek yollar seriyi açıklanan daha geniş bir uygulama yaşam döngüsü yönetimi (ALM) işlemine nasıl uyduğunu göstermektedir.
- [Web dağıtımı için sunucu ortamları yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Bu öğreticide (Uzak Aracısı) Web Dağıtım Aracı hizmeti veya Web dağıtımı işleyicisi ve Uzak veritabanı dağıtım kullanarak uzak web paketi dağıtımı dahil olmak üzere çeşitli dağıtım senaryoları desteklemek için Windows sunucularının nasıl yapılandırılacağı açıklanmaktadır. Kendi ortamınız için uygun dağıtım yöntemi seçme hakkında yönergeler sağlar ve Web grubu çerçevesi (WFF) bir sunucu grubundaki tüm web sunucuları arasında dağıtılan web uygulamaları çoğaltmak için nasıl kullanılacağını açıklar.
- [Team Foundation Server için Web dağıtımı yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Bu öğretici nasıl el ile belirli derlemeleri dağıtımları tetiklenen ve otomatik dağıtım bir CI işleminin bir parçası olarak dahil olmak üzere çeşitli dağıtım senaryoları desteklemek için TFS'ye yapılandırılacağını açıklar.
- [Kurumsal Web dağıtımı Gelişmiş](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Bu öğretici, birden çok ortamlar için veritabanı dağıtımları özelleştirme, dağıtımdan dosya ve klasörleri dışarıda ve dağıtım işlemi sırasında çevrimdışı web uygulamaları alma gibi çeşitli daha gelişmiş dağıtım görevlerinin açıklar .

> [!div class="step-by-step"]
> [Next](the-contact-manager-solution.md)
