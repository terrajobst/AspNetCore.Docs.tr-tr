---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Kurumsal Web Dağıtımı: Senaryoya genel bakış | Microsoft Docs'
author: jrjlee
description: Bu öğreticiler kümesini ref sağlamak için gerçekçi bir kurgusal kurumsal dağıtım senaryosu ile birlikte bir karmaşıklık düzeyine sahip örnek bir çözüm kullanır...
ms.author: aspnetcontent
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: ce533b246b14c9e041c89a778c022784f09d01a0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836752"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Kurumsal Web Dağıtımı: Senaryoya genel bakış
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu kümesi öğretici örnek bir çözüm gerçekçi bir kurgusal kurumsal dağıtım senaryosu ile birlikte bir karmaşıklık düzeyine sahip bir başvuru uygulaması sağlamak ve görevleri ve izlenecek yollar bir ortak bağlam sağlamak için kullanır. Bu konu, öğretici senaryo açıklar ve örnek bir çözüm sunar.


## <a name="scenario-description"></a>Senaryo açıklaması

Fabrikam, Inc. adlı kurgusal bir şirket iletişim bilgilerini bir web arabiriminden depolanıp uzak satış ekipleri olanak sağlayan bir çözümü oluşturmaktır.

Fabrikam, Inc., uygulama yaşam döngüsü yönetimi (ALM) işlemleri için üç sunucu ortamlarında yazılım geliştirme işleminin çeşitli aşamalarda dağıtılan çözümü gerektirir:

- Bir geliştirici test veya "korumalı alan" ortam.
- İntranet tabanlı hazırlık ortamı.
- Internet'e yönelik üretim ortamı.

Bu ortamların her birinde farklı yapılandırma ve güvenlik gereksinimleri vardır ve her benzersiz dağıtım zorlukları doğurur.

### <a name="the-fabrikam-inc-server-infrastructure"></a>Fabrikam, Inc. Sunucu altyapısı

Fabrikam, Inc.'in en üst düzey geliştirme ve dağıtım altyapısı budur.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Geliştirici iş istasyonları, kaynak denetimine altyapı, geliştirici test ortamını ve tüm intranet ağ Fabrikam.net etki alanı içinde bulunan bir hazırlık ortamı. İntranet ağ üzerinden bir güvenlik duvarı tarafından ayrılmış olan bir çevre ağındaki (diğer adıyla DMZ, sivil bölge ve denetimli alt ağ), üretim ortamında bulunur. Ortak bir dağıtım senaryosu budur: İç sunucu altyapınızı kullanarak güvenlik duvarı veya ağ geçidi sunucuları'ndan Internet'e yönelik web sunucularınızın yalıtın.

Bu örnekte:

- Team Foundation Server (TFS) 2010 sunucu ayrı bir yapı sunucusu ile kaynak denetimi ve sürekli tümleştirme (CI) işlevselliği sağlar.
- Geliştirici test ortamı bir Internet Information Services (IIS) 7.5 web sunucusu ve SQL Server 2008 R2 veritabanı sunucusu içerir.
- Üretim ortamında, bir SQL Server 2008 R2 veritabanı sunucusu ile birlikte bir Web grubu çerçevesi (WFF) controller sunucusu tarafından eşitlenen birden çok IIS 7.5 web sunucuları içerir. Uygulamada, veritabanı sunucusu kümesi veya yansıtma ölçeklenebilirliği ve kullanılabilirliği geliştirmek için kullanabilir.
- Hazırlık ortamından üretim ortamının yapılandırması mümkün olduğunca yakın çoğaltmak için tasarlanmıştır.
- Güvenlik Duvarı ve ağ yalıtımı ilkelerini çevre ağına doğrudan, otomatik dağıtım intranetten izin vermez.

Bu ortamların her birinde yapılandırmasını ikinci öğreticide daha ayrıntılı olarak açıklanan [yapılandırma sunucu ortamlarını Web dağıtımı için](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>ALM için ekip rolleri

Bu kullanıcılar, oluşturma, yönetme, oluşturma ve yayımlama Kişi Yöneticisi çözümü ilgilidir:

- Matt Hink Fabrikam, Inc. bir web uygulama geliştiricisi olan He Kişi Yöneticisi çözümü Visual Studio 2010 kullanılarak geliştirilmiş takımın bir parçasıdır. Matt, ona kendi gereksinimlerini karşılamak için ortamı yapılandırmak sağlayan Geliştirici test ortamlarındaki sunucularda tam yönetici haklarına sahip. Kendisi Ayrıca kullanıcı he Kişi Yöneticisi çözümü için kaynak kodu depoladığı Visual Studio 2010 TFS örneği erişebilir.
- Rob Aksoy Fabrikam, Inc. geliştirme ekibi için Sunucu Yöneticisi ' dir. Böylece yaptığı tüm yönlerini TFS ve Team Build yapılandırabilirsiniz Rob TFS sunucusunda yönetici erişimi olur. Rob ayrıca test ve hazırlama web sunucuları yönetimsel erişim sahibi ve test ve hazırlık ortamları veritabanı sunucuları için veritabanı yöneticisi (DBA) görev yapar. Bu görevleri gerçekleştirmek için TFS sunucusu üzerinde Ekip Rob yapılandırdı:

    - Oluşturun ve TFS için bir dosya içinde bir kullanıcı denetimi gerçekleştirildiğinde uygulama üzerinde birim testleri çalıştırın. Bu, CI çağrılır.
    - Kişi Yöneticisi uygulaması test ortamınıza, uygulama birim testleri başarılı olduktan sonra otomatik olarak dağıtın. Bu, ilk dağıtımdan sonra test sunucularına ilk dağıtımı ile veritabanına herhangi bir güncelleştirme veritabanı yayımlama içerir.
    - Kişi Yöneticisi uygulamayı tek adımlı işlemde hazırlık ortamına dağıtın.
    - Web Sunucu Yöneticisi ve DBA üretim ortamına uygulamayı yayımlamak için kullanabileceğiniz bir Web paketi oluşturun.
- Lisa Andrews sorumlu Fabrikam, Inc.'in üretim sunucularına uygulamaları dağıtmak için Sunucu Yöneticisi ' dir. Kişi Yöneticisi uygulama oluşturur sonra TFS Team Build web dağıtım paketi depoladığı paylaşımına okuma erişimi olan. Kendisi ayrıca o uygulamayı üretim ortamına dağıtabilirsiniz böylece üretim web sunucularına yönetimsel erişim sahibi. Ayrıca, o veritabanı sunucusuna üretim ortamında veritabanları ve veritabanı güncelleştirmeleri dağıtan DBA olarak görev yapar.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Kişi Yöneticisi çözümü

Kişi Yöneticisi çözümünü ekleyin ve bir web arabirimi üzerinden iletişim bilgilerini düzenle kayıtlı oturum açmış kullanıcıların, izin vermek için tasarlanmıştır. Kişi Yöneticisi çözümünü dört ayrı ayrı projelerin oluşur:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Çözüm için giriş noktasını temsil eden bir ASP.NET MVC3 web uygulaması projesi budur. Kullanıcıları oluşturma ve kişi ayrıntılarını görüntüleme olanağı sağlama gibi bazı temel web uygulaması işlevselliği sunar. Uygulamanın, kişiler ve kimlik doğrulama ve yetkilendirme yönetmek için bir ASP.NET uygulama hizmetleri veritabanına yönetmek için bir Windows Communication Foundation (WCF) hizmeti kullanır.
- **ContactManager.Database**. Visual Studio 2010 veritabanı projesi budur. Proje depoları ayrıntıları başvurun bir veritabanı için şema tanımlar.
- **ContactManager.Service**. Bir WCF web hizmeti projesi budur. WCF kullanıma sunan gerçekleştirin izin veren bir uç nokta oluşturma, alma, güncelleştirme ve silme (CRUD) işlemleri Kişi Yöneticisi veritabanında. Kişi Yöneticisi veritabanı ve ContactManager.Common.dll derleme hizmeti kullanır.
- **ContactManager.Common**. Bir sınıf kitaplığı projesi budur. Bu derlemede tanımlanan türlerin WCF Hizmeti kullanır.

Bu serideki ilk öğreticide sağlanan çözüm ve dağıtım gereklilikleri tam bir gözden geçirme [Kurumsal Web dağıtımı](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Dağıtım görevleri

Büyük bir kuruluş farklı ortamlarda uygulamaların dağıtılmasında kullanılan birkaç farklı görevleri vardır. Öğreticiler kapsayan en önemli görevler şunlardır:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Bu belgede daha önce açıklanan kullanıcı perspektifinden dağıtım işlemindeki her adımın bir listesi aşağıda verilmiştir:

1. Tüm takım üyeleri anahtar dağıtım gereksinimleri ve sorunları belirlemek için Visual Studio 2010 Kişi Yöneticisi çözümü gözden geçirin.
2. Matt Hink, geliştirici test ortamına dağıtım mantığının bir ilk testi yürütmek için geliştirici iş istasyonu doğrudan Kişi Yöneticisi çözümünü dağıtabilirsiniz.
3. Matt Hink TFS içinde uygulama kaynak denetimine ekler.
4. Rob Aksoy takım yapısı'nda çeşitli yapı tanımlarını Kişi Yöneticisi çözümü oluşturur. Bir derleme tanımı, yeni kodu bir kullanıcı denetimi gerçekleştirildiğinde çözümü Geliştirici test ortamına dağıtmak için CI kullanır. Başka bir derleme tanımı hazırlık ortamına gerektiği gibi kullanıcıların tetikleyici dağıtımlar sağlar.
5. Her zaman yeni kod, bir kullanıcı denetler, ekip otomatik olarak çözüm bileşenlerini derlemeler, birim testlerini çalıştırır ve derleme başarılı olduysa Geliştirici test ortamı ve birim testleri geçildi çözümü dağıtır.
6. Bir kullanıcı bir hazırlık ortamına dağıtım tetiklendiğinde, çözüm paketlenir ve tek adımlı işlemde dağıtılır. Bu işlem ayrıca el ile üretim ortamına dağıtım için bir paket oluşturur.
7. Lisa Andrews, 6. adımda oluşturduğunuz web paketini el ile içeri aktararak, üretim ortamına uygulamayı dağıtır.

### <a name="key-deployment-issues"></a>Anahtar Dağıtım sorunları

Kişi Yöneticisi çözümünü ve Fabrikam, Inc. senaryosu, çeşitli genel sorunlar ve karmaşık, Kurumsal ölçekte çözümleri dağıtırken karşılaşabileceğiniz bazı zorluklar vurgulayın. Örneğin:

- Geliştirici gibi birden çok ortama projeleri dağıtmak veya test ortamları, platformları ve üretim sunucuları hazırlama gerekir. Çözüm, her ortam için farklı yapılandırma ayarları ile dağıtılması gerekiyor.
- Tek adımlı ya da otomatikleştirilmiş bir derleme ve dağıtım sürecinin bir parçası olarak aynı anda birden çok bağımlı proje dağıtmanız gerekebilir.
- Otomatik bir işlemden sürücü dağıtımına olması gerekir. Örneğin, yeni kodu iade edildiğinde bir hazırlık ortamı için web uygulamaları dağıtmak için bir CI işlem kullanmak istiyorsunuz.
- Geliştiricilerin doğru yapılandırma ayarları veya her bir hedef ortam için gerekli kimlik bilgilerine sahip olasılığının düşük olduğu gibi dağıtım işlemini denetleyen ve dış Visual Studio'dan dağıtım değişkenlerini ayarlamak mümkün olması gerekir.
- Şema tabanlı veritabanı projeleri dağıtma ve sonraki dağıtımlarda mevcut verilerinizi korumak gerekir.
- Kullanıcı hesabı verileri dağıtıma gerek kalmadan geçici olarak üyelik veritabanları dağıtma gerekir. Ayrıca, varolan kullanıcı verilerini kaybetmeden dağıtılan üyelik veritabanı şeması güncelleştirmeniz gerekebilir.
- Çeşitli hedef ortamlara içerik dağıttığınızda, belirli dosyaları veya klasörleri dışlama gerekir.

Ayrıca, sık ve artımlı güncelleştirmeleri dağıtım yönetirken ek bazı zorluklar oluşturur. Örneğin:

- Yeni kod, bir geliştirici denetler her zaman birim testlerini çalıştırın. Yalnızca kod birim testlerini geçerse çözümü dağıtmak istersiniz.
- Bir web uygulaması için bir hazırlık veya üretim ortamı dağıttığınızda, kullanıcılara yönlendirmek, istediğiniz bir *uygulama\_offline.htm* dağıtım işlemi süresince dosya.
- Dağıtım etkinliklerini günlüğe kaydetmek istediğiniz. Dağıtım işlemi, belirtilen alıcılara e-posta bildirimleri başarılı veya başarısız dağıtımlarının göndermesi gerekir.
- Otomatik bir dağıtım başarısız olursa, dağıtım işlemi geçerli dağıtımı yeniden deneyin veya bunun yerine önceki web paketi dağıtma.

> [!div class="step-by-step"]
> [Önceki](deploying-web-applications-in-enterprise-scenarios.md)
> [İleri](application-lifecycle-management-from-development-to-production.md)
