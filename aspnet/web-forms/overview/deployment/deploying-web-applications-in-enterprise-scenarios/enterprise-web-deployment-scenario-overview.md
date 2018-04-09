---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Kurumsal Web Dağıtımı: Senaryoya genel bakış | Microsoft Docs'
author: jrjlee
description: Bu öğreticiler örnek bir çözüm kurgusal kuruluş dağıtım senaryosu birlikte karmaşıklık gerçekçi düzeyine sahip bir ref sağlamak üzere kullanıyor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 20f6e206d6aa4bebb4936246468f5ada0e213236
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="enterprise-web-deployment-scenario-overview"></a>Kurumsal Web Dağıtımı: Senaryoya genel bakış
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu öğreticiler kümesini örnek bir çözüm kurgusal kuruluş dağıtım senaryosu birlikte karmaşıklık gerçekçi düzeyine sahip bir başvuru uygulama sunmak amacıyla ve görevleri ve izlenecek yollar ortak bir bağlam vermek için kullanır. Bu konu, öğretici senaryo açıklar ve örnek çözümü sunar.


## <a name="scenario-description"></a>Senaryo açıklaması

Fabrikam, Inc., kurgusal bir şirket iletişim bilgileri bir web arabiriminden depolanıp uzak satış ekipleri olanak sağlayan bir çözümü oluşturuyor.

Fabrikam, Inc., uygulama yaşam döngüsü yönetimi (ALM) işlemleri üç sunucu ortamları için yazılım geliştirme sürecinin çeşitli aşamalarında dağıtılması için çözüm gerektirir:

- Bir geliştirici test veya "korumalı alan" ortamı.
- İntranet tabanlı hazırlama ortamı.
- Internet'e yönelik üretim ortamı.

Bu ortamların her birinde farklı yapılandırma ve güvenlik gereksinimleri vardır ve her benzersiz bir dağıtım zorluklar oluşturur.

### <a name="the-fabrikam-inc-server-infrastructure"></a>Fabrikam, Inc. Sunucu altyapısı

Fabrikam, Inc. en üst düzey geliştirme ve dağıtım altyapısı budur

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Geliştirici iş istasyonları, kaynak denetimi altyapısı, geliştirici test ortamı ve tüm Fabrikam.net etki alanı içinde intranet ağ üzerinde bulunan hazırlık ortamı. İntranet ağ üzerinden bir güvenlik duvarı tarafından ayrılmış olan bir çevre ağında (olarak da bilinen DMZ, sivil bölge ve denetimli alt ağ), üretim ortamında bulunur. Bu ortak bir dağıtım senaryodur: güvenlik duvarları veya ağ geçidi sunucularını kullanarak iç sunucu altyapınızdan, İnternete dönük web sunucuları yalıtmak.

Bu örnekte:

- Team Foundation Server (TFS) 2010 sunucusu ayrı yapı sunucuyla kaynak denetimi ve sürekli tümleştirme (CI) işlevselliği sağlar.
- Geliştirici test ortamı bir Internet Information Services (IIS) 7.5 web sunucusu ve SQL Server 2008 R2 veritabanı sunucusu içerir.
- Üretim ortamında bir SQL Server 2008 R2 veritabanı sunucusu ile birlikte bir Web grubu çerçevesi (WFF) denetleyici sunucusu tarafından eşitlenen birden çok IIS 7.5 web sunucuları içerir. Uygulamada, veritabanı sunucu kümesi veya yansıtma ölçeklenebilirlik ve kullanılabilirlik geliştirmek için kullanabilir.
- Hazırlama ortamında üretim ortamının yapılandırması mümkün olduğunca yakın çoğaltmak için tasarlanmıştır.
- Güvenlik Duvarı ve ağ yalıtımı ilkelerini intranet doğrudan, otomatik dağıtımdan çevre ağına izin vermez.

Bu ortamlar her birinin yapılandırmasını ikinci öğreticide daha ayrıntılı olarak açıklanmıştır [Web dağıtımı için sunucu ortamları yapılandırma](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>ALM için takım rolleri

Bu kullanıcılar oluşturma, yönetme, oluşturma ve yayımlama Contact Manager çözümü oynayan:

- Fabrikam, Inc., web Uygulama geliştirici Matt Hink olduğu Müşterinizle Contact Manager çözümü Visual Studio 2010 kullanılarak geliştirilmiş takımın bir parçasıdır. Matt ona kendi gereksinimlerini karşılamak için ortamı yapılandırmak sağlar Geliştirici test ortamında sunucularda tam yönetici haklarına sahip. Kendisi Ayrıca kullanıcı kendisinin Contact Manager çözüm için kaynak kodunu depoladığı Visual Studio 2010 TFS örneği erişebilir.
- Ramiz Aksoy, Fabrikam, Inc. geliştirme ekibi için bir sunucu yöneticisidir. Böylece kendisinin TFS ve ekip tüm yönlerini yapılandırabilirsiniz Ramiz TFS sunucusunda yönetici erişimi olur. Ramiz ayrıca test ve hazırlama web sunucuları için yönetim erişimine sahip ve test ve hazırlık ortamları veritabanı sunucuları için veritabanı yöneticisi (DBA) olarak davranır. Ramiz ekip bu görevleri gerçekleştirmek için TFS sunucusunda yapılandırılan:

    - Oluşturun ve her bir kullanıcı bir dosyada TFS denetim gerçekleştirildiğinde birim testleri uygulamayı çalıştırın. Bu, CI adı verilir.
    - Birim testleri uygulamayı geçirmeden sonra test ortamı Contact Manager uygulamaya otomatik olarak dağıtın. Bu, ilk dağıtım ve veritabanı için herhangi bir güncelleştirme test sunucularına ilk dağıtımdan sonra veritabanı yayımlama içerir.
    - Tek adımlık bir işlemdir ve hazırlık ortamında Contact Manager uygulamayı dağıtın.
    - Web Sunucu Yöneticisi ve DBA üretim ortamına uygulamayı yayımlamak için kullanabileceğiniz bir Web paketi oluşturun.
- Lisa Andrews Fabrikam, Inc. üretim sunucularına uygulamalarını dağıtmak için sorumlu bir sunucu yöneticisidir. İlgili Kişi Yöneticisi uygulaması oluşturur sonra TFS ekip web dağıtım paketi depoladığı paylaşımına okuma erişimi olan. Aynen uygulama üretime dağıtabileceğiniz kendisi de yönetim üretim web sunucularına erişebilir. Ayrıca, aynen üretim ortamında veritabanı sunucusuna veritabanları ve veritabanı güncelleştirmeleri dağıtan DBA olarak görev yapar.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Contact Manager çözümü

Kişi Yöneticisi çözüm ekleyin ve bir web arabirimi aracılığıyla iletişim bilgilerini düzenleyin kayıtlı, oturum açan kullanıcıların izin verecek şekilde tasarlanmıştır. Contact Manager çözüm dört ayrı projelerin oluşur:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Çözüm için giriş noktası temsil eden bir ASP.NET MVC3 web uygulaması projesini budur. Kullanıcılar oluşturma ve kişi ayrıntılarını görüntüleme olanağı sağlamak gibi bazı temel web uygulaması işlevselliği sunar. Uygulamanın, kişiler ve kimlik doğrulama ve yetkilendirme yönetmek için bir ASP.NET uygulama hizmetleri veritabanına yönetmek için bir Windows Communication Foundation (WCF) Hizmeti'ne bağlıdır.
- **ContactManager.Database**. Visual Studio 2010 veritabanı projesi budur. Proje depoları başvurun ayrıntıları bir veritabanı için şema tanımlar.
- **ContactManager.Service**. Bir WCF web hizmeti projesi budur. Arayanlar gerçekleştirmek imkan tanıyan bir uç noktası oluşturma, WCF açığa çıkarır almak, Güncelleştir ve Sil (CRUD) işlemleridir Contact Manager veritabanında. Hizmet Contact Manager veritabanı ve ContactManager.Common.dll derleme dayanır.
- **ContactManager.Common**. Bir sınıf kitaplığı proje budur. Bu derlemede tanımlanan türü WCF Hizmeti kullanır.

Bu serideki ilk öğreticide çözümü ve dağıtım gereklilikleri tam gözden sağlanan [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Dağıtım görevleri

Büyük bir kuruluşun farklı ortamlarda uygulamaları dağıtırken ilgili birkaç farklı görevleri vardır. Öğreticiler kapak en önemli görevler şunlardır:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Dağıtım işlemi her adımda bu belgenin önceki bölümlerinde açıklanan kullanıcılar perspektifinden listesi aşağıdadır:

1. Ekibinin tüm üyeleri anahtar dağıtım gereksinimleri ve sorunları belirlemek için Visual Studio 2010 Contact Manager çözüm gözden geçirin.
2. Matt Hink ilk bir sınama dağıtımı mantığı için geliştirici test ortamını, doğrudan Geliştirici istasyonundan Contact Manager çözüme dağıtabilirsiniz.
3. Matt Hink kaynak denetimi uygulamaya TFS'de ekler.
4. Ramiz Aksoy Contact Manager çözüm için çeşitli derleme tanımları ekip içinde oluşturur. Bir derleme tanımınız CI her bir kullanıcı yeni kodda denetim gerçekleştirildiğinde çözümü Geliştirici sınama ortamında dağıtmak için kullanır. Başka bir yapı tanımı kullanıcılar tetikleyici dağıtımları için hazırlama ortamı gerekli olarak sağlar.
5. Bir kullanıcı yeni kodda denetler her zaman, ekip otomatik olarak çözüm bileşenlerini oluşturur, birim testleri çalıştırır ve yapı başarılı olduysa Geliştirici test ortamı ve birim testleri geçişi çözümü dağıtır.
6. Bir kullanıcı hazırlama ortamında dağıtımına harekete geçirdiğinde çözüm paketlenmiş ve tek bir adımda dağıtılabilir. Bu işlem ayrıca üretim ortamına el ile dağıtımı için bir paket oluşturur.
7. 6. adımda oluşturduğunuz web paketini el ile içeri aktararak uygulamanızı üretim ortamına Lisa Andrews dağıtır.

### <a name="key-deployment-issues"></a>Anahtar Dağıtım sorunları

Contact Manager çözümü ve Fabrikam, Inc. senaryosu çeşitli ortak sorunlar ve karmaşık, Kurumsal ölçekte çözümleri dağıttığınızda karşılaşabileceğiniz sorunları vurgulayın. Örneğin:

- Geliştirici gibi birden çok ortamlara projeleri dağıtmak veya sınama ortamlarında, platformlar ve üretim sunucuları hazırlama gerekir. Çözüm, her ortam için farklı yapılandırma ayarlarıyla dağıtılması gerekiyor.
- Birden fazla bağımlı proje aynı anda tek bir adımı veya otomatikleştirilmiş derleme ve dağıtım işleminin bir parçası olarak dağıtmanız gerekir.
- Bir otomatik işleminden sürücü dağıtımına olması gerekir. Örneğin, bir CI işlemi yeni kod işaretlendiğinde hazırlama ortamına web uygulamalarını dağıtmak için kullanmak istediğiniz.
- Geliştiricilerin doğru yapılandırma ayarları veya her hedef ortam için gerekli kimlik bilgilerine sahip tahmin edilemez şekilde dağıtım işlemini denetleyen ve dış Visual Studio'dan dağıtım değişkenleri ayarlayın mümkün olması gerekir.
- Şema tabanlı veritabanı projeleri dağıtmak ve sonraki dağıtımlarda mevcut verilerinizi korumak gerekir.
- Kullanıcı hesabı verileri dağıtmadan üyelik veritabanları geçici düzenli olarak dağıtmanız gerekir. Ayrıca var olan kullanıcı hesabı verilerini kaybetmeden dağıtılan üyelik veritabanı şeması güncelleştirmeniz gerekebilir.
- İçerik çeşitli hedef ortamlara dağıttığınızda belirli dosyaları veya klasörleri dışarıda gerekir.

Ayrıca, sık sık ve artımlı güncelleştirmeler olduğunda dağıtımını yönetme ek zorluklara oluşturur. Örneğin:

- Yeni kodda bir geliştirici denetler her zaman birim testleri çalıştırın. Yalnızca kodu birim testleri geçerse çözümü dağıtmak istiyor.
- Bir web uygulaması bir hazırlık veya üretim ortamına dağıttığınızda, kullanıcıların yeniden yönlendirmek istediğiniz bir *uygulama\_offline.htm* dosyası için dağıtım işleminin süresi.
- Dağıtım etkinliklerini günlüğe kaydetmek istediğiniz. Dağıtım işlemi, belirtilen alıcılara e-posta bildirimleri başarılı veya başarısız dağıtımlarını göndermesi gerekir.
- Bir otomatik dağıtım başarısız olursa, dağıtım işlemi geçerli dağıtımı yeniden deneyin veya bunun yerine önceki web paketini dağıtmak gerekir.

> [!div class="step-by-step"]
> [Önceki](deploying-web-applications-in-enterprise-scenarios.md)
> [sonraki](application-lifecycle-management-from-development-to-production.md)
