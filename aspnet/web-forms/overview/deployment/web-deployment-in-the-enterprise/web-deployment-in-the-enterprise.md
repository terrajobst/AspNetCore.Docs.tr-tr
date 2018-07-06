---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Web dağıtımı kuruluştaki | Microsoft Docs
author: jrjlee
description: Bu öğreticide devel Kurumsal ölçekte web uygulamaları dağıtımını yönetirken karşılaşabileceğiniz sorunları çok sayıda karşılamak açıklar...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 2cb8cb12ec5af376b58b672813af8626cb0f95d8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806564"
---
<a name="web-deployment-in-the-enterprise"></a>Kurumsal Web dağıtımı
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu öğreticide, Kurumsal ölçekte web uygulamaları geliştirme, test, hazırlama ve üretim ortamlarında dağıtımını yönetirken karşılaşabileceğiniz sorunları çok sayıda karşılayacak şekilde açıklar. Öğretici, bir başvuru çözüm çeşitli ortak görevleri ve yordamları size kılavuzluk kavramsal ve görev odaklı içeriğinin bir karışımını birlikte içerir.
> 
> Bu öğreticiler için bir İtalyan çevirisi, ziyaret [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Kurumsal Dağıtım zorlukları

Karmaşık kurumsal ölçekli çözüm dağıtımını yönetmek için baktığınızda kuruluşlar genellikle bu zorluklar karşılaşırsınız:

- Geliştirici gibi birden çok ortama projeleri dağıtmak veya test ortamları, platformları ve üretim sunucuları hazırlama gerekir. Çözüm, her ortam için farklı yapılandırma ayarları ile dağıtılması gerekiyor.
- Tek adımlı ya da otomatikleştirilmiş bir derleme ve dağıtım sürecinin bir parçası olarak aynı anda birden çok bağımlı proje dağıtmanız gerekebilir.
- Otomatik bir işlemden sürücü dağıtımına olması gerekir. Örneğin, yeni kodu iade edildiğinde bir test ortamı için web uygulamaları dağıtmak için bir sürekli tümleştirme (CI) işlem kullanmak istiyorsunuz.
- Geliştiricilerin doğru yapılandırma ayarları veya her bir hedef ortam için gerekli kimlik bilgilerine sahip olasılığının düşük olduğu gibi dağıtım işlemini denetleyen ve dış Visual Studio'dan dağıtım değişkenlerini ayarlamak mümkün olması gerekir.
- Şema tabanlı veritabanı projeleri dağıtma ve sonraki dağıtımlarda mevcut verilerinizi korumak gerekir.
- Kullanıcı hesabı verileri dağıtıma gerek kalmadan geçici olarak üyelik veritabanları dağıtma gerekir. Ayrıca, varolan kullanıcı verilerini kaybetmeden dağıtılan üyelik veritabanı şeması güncelleştirmeniz gerekebilir.
- Çeşitli hedef ortamlara içerik dağıttığınızda, belirli dosyaları veya klasörleri dışlama gerekir.

## <a name="overview-of-approach"></a>Yaklaşım genel bakış

Bu serideki diğer öğreticileri ile birlikte Bu öğreticide, yukarıda açıklanan zorluklarını karşılayacak şekilde bu üst düzey bir yaklaşım kullanılır.

- **Genel derleme ve dağıtım işlemini denetlemek için özel Microsoft Build Engine (MSBuild) proje dosyalarını kullanın.**
- Bu, derleme ve çözümde her proje tek ve kodlanabilir bir işlemle bir parçası olarak dağıtmanıza olanak sağlar.
- Basit bir ortama özgü proje dosyalarını kullanarak ortama özgü ayarları yapılandırılır. Çözüm yapılandırmaları kullanarak Visual Studio – merkezli bir yaklaşım aksine ve farklı ortamlar için dağıtımları yapılandırmak için yayımlama profillerini, bu yaklaşım, yapılandırma ve dağıtım sürecinin dışında Visual Studio'dan yönetmeyi olanak sağlar. Başka bir deyişle, geliştiriciler bağlantı dizeleri, hizmet uç noktaları, sunucu kimlik bilgileri ve diğer dağıtım değişkenlerini hedef ortamlar için bilginizi geliştirin yoktur.
- Özel proje dosyaları, Team Foundation Server (TFS) iş akışının parçası olarak ekip tarafından çağrılabilir. Bu, CI senaryoları için otomatik dağıtım yapılandırmanıza olanak sağlar.

**Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) paketi ve web uygulama projeleri dağıtmak için kullanın.**

- Web dağıtımı paketi ve bağımlılıkları, yapılandırma ayarlarını, güvenlik ayarlarını ve diğer gereksinimler ile birlikte bir hedef IIS web sunucusu için web uygulaması içeriğinizi dağıtmanıza olanak sağlayan bir çerçeve sunar.
- Özel MSBuild proje dosyaları içinde tüm paketleme ve dağıtım işlem kontrol edebilirsiniz. Bağlantı dizeleri, hizmet uç noktaları ve IIS hedef ayrıntıları gibi web dağıtım paketi ile birlikte gelen yapılandırma ayarlarını da değiştirebilirsiniz.
- Web yayımlama ardışık birlikte Web dağıtımı, dağıtımlarınızı özelleştirmenize olanak tanıyan genişletilebilirlik noktaları çok sayıda sunar. Örneğin, web dağıtımı paketleri istenmeyen dosyaları ve klasörleri dışlamak kolaydır.

**Dağıtma ve veritabanı şemalarını güncelleştirmek için VSDBCMD.exe yardımcı programını kullanın.**

- VSDBCMD Visual Studio veritabanı projesi oluşturduğunuzda, oluşturulan bir veritabanı şema dosyası (.dbschema), veritabanlarını dağıtmanıza olanak tanır. Buna karşılık, Web dağıtımı içinde bulunan veritabanı dağıtım işlevleri var olan veritabanlarını bir yerel SQL Server örneğinin dağıtımı için daha uygundur.
- Veritabanı projeleri dağıtma için Visual Studio'nun işlevselliğini VSDBCMD var olan bir hedef veritabanı için değişiklik güncelleştirmelerini dağıtmanıza olanak tanır. Bu, veritabanı şemasını yükseltirken mevcut verileri korumak sağlar.
- Özel MSBuild proje dosyaları içinde VSDBCMD komutları yürütebilir.

## <a name="content-map"></a>İçerik haritası

Bu öğreticide, dört ana bölüme ayrılır konularını içerir.

Bu konular, başvuru çözüm tanıtmaktadır&#x2014;Kişi Yöneticisi çözümünü&#x2014;ve indirmek ve yerel makinenizde yapılandırma açıklanmaktadır:

- [Kişi Yöneticisi Çözümü](the-contact-manager-solution.md)
- [Kişi Yöneticisi Çözümünü Ayarlama](setting-up-the-contact-manager-solution.md)

Bu konular, MSBuild proje dosyalarındaki tanıtmak, nasıl oluşturabilir ve özel proje dosyalarını kullanın ve Kişi Yöneticisi çözümü dağıtım sürecinde size yol açıklar:

- [Proje Dosyasını Anlama](understanding-the-project-file.md)
- [Derleme İşlemini Anlama](understanding-the-build-process.md)

Bu konularda, derleme ve paketleme işleminin nasıl çalıştığı, yapı işlemi Web yayımlama işlem hattı ile nasıl tümleştirildiğini, dağıtım parametrelerini değiştirme ve hedefe web paketleri dağıtma dahil olmak üzere web uygulaması dağıtımı açıklanmaktadır. ortamlar:

- [Web Uygulaması Projelerini Derleme ve Paketleme](building-and-packaging-web-application-projects.md)
- [Web Paketi Dağıtımı için Parametreleri Yapılandırma](configuring-parameters-for-web-package-deployment.md)
- [Web Paketleri Dağıtma](deploying-web-packages.md)

- [Veritabanı projeleri dağıtma](deploying-database-projects.md) olumlu ve olumsuz her yaklaşımın ile birlikte Visual Studio veritabanı projelerini dağıtmak için kullanabileceğiniz farklı teknikleri açıklar. [Oluşturma ve dağıtım komut dosyası çalıştırma](creating-and-running-a-deployment-command-file.md) dağıtım mantığınızı kapsüller ve karmaşık çözümleri tek adımlı işlem olarak dağıtmanıza imkan tanır bir basit bir komut dosyası oluşturmayı açıklar.
- Son olarak, [Web paketlerini el ile yükleme](manually-installing-web-packages.md) IIS içinde web paketleri içeri aktarmak için göstererek öğreticiyi sonlandırır.

## <a name="key-technologies"></a>Anahtar teknolojileri

Bu öğreticide öncelikle bu teknolojiler derleme ve dağıtım yönetmek için konulara bakın:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- VSDBCMD.exe veritabanı dağıtım yardımcı programı

## <a name="other-tutorials-in-this-series"></a>Bu serideki diğer öğreticileri

Bu bir öğretici serisinde, beş parçası üzerinde Kurumsal ölçekte web dağıtımı oluşturur. Diğer öğreticiler serisinde şunlardır:

- [Kurumsal senaryolarda Web uygulamaları dağıtma](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Bu Tanıtım içerik bağlamsal arka plan için öğretici serisinin sağlar. Öğretici senaryo açıklar ve daha geniş bir uygulama yaşam döngüsü yönetimi (ALM) işleme görevleri ve izlenecek yollar seri boyunca açıklanan nasıl getireceğinizi göstermektedir.
- [Sunucu ortamlarını Web dağıtımı için yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Bu öğreticide, Web dağıtım aracı hizmetini (Uzak Aracı) veya Web dağıtımı işleyicisi ve Uzak veritabanı dağıtımı kullanarak uzak web paketi dağıtımı dahil olmak üzere çeşitli dağıtım senaryoları desteklemek için Windows sunucularının nasıl yapılandırılacağı açıklanır. Kendi ortamınız için uygun dağıtım yöntemi seçme hakkında rehberlik sağlar ve Web Farm Framework (WFF) bir sunucu grubundaki tüm web sunucuları arasında dağıtılmış web uygulamalarında çoğaltmak için nasıl kullanılacağını açıklar.
- [Web dağıtımı için Team Foundation Server yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Bu öğreticide derlemelerini dağıtımları el ile tetiklenen ve otomatik dağıtım bir CI işlemine bir parçası olarak dahil olmak üzere çeşitli dağıtım senaryoları desteklemek üzere TFS'ye yapılandırma açıklanır.
- [Gelişmiş Kurumsal Web dağıtımı](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Bu öğreticide dağıtım işlemi sırasında web uygulamalarını çevrimdışı alma birden çok ortam için veritabanı dağıtımlarını özelleştirme ve dosya ve klasörleri dağıtımdan dışlama gibi çeşitli daha gelişmiş dağıtım görevlerini, nasıl yapılacağını anlatmaktadır. .

> [!div class="step-by-step"]
> [Next](the-contact-manager-solution.md)
