---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Senaryo: bir üretim ortamında Web dağıtımı için yapılandırma | Microsoft Docs'
author: jrjlee
description: Bu konu, bir üretim ortamına yönelik normal web dağıtım senaryosu açıklar ve benzer bir ayarlamak için tamamlamanız gereken görevleri açıklar...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 3627d18e1b102c574726282780239464be04c04b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755324"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Senaryo: Web dağıtımı için bir üretim ortamı yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda bir üretim ortamına yönelik normal web dağıtım senaryosu ve benzer bir ortamı ayarlamak için tamamlanması gereken görevleri açıklar.


Bir web uygulaması veya bir Web sitesi için son hedefi üretim ortamıdır. Bu nokta uygulamanızı test sürecinde olan, bir hazırlama ortamına dağıtıldıktan ve "Canlı." kullanıma hazır Bir üretim ortamının özelliklerini yaygın doğası ve web içeriğinize amacı, kuruluşunuz, hedef kitle ve diğer faktörlere çok sayıda boyutuna göre farklılık gösterebilir. Kurumsal ölçekte bir senaryoda, üretim ortamında, bu özelliklere sahip:

- Ortamın birden çok yük dengeli web sunucusu ve bir veya daha fazla veritabanı sunucuları, genellikle Yük Devretme Kümelemesi ve veritabanı yansıtma oluşur.
- Internet'e yönelik ortam ise iç ağınıza ayrılmış olasıdır. Bir çevre ağında farklı bir alt ağdaki olabilir, farklı bir etki alanında olabilir ve bir tamamen farklı ağ altyapısında olabilir.
- Geliştiriciler ve yapı sunucu işlemi hesapları üretim sunucularında yönetici ayrıcalıklarına sahip yüksek oranda düşüktür.
- Uygulamalar için değişiklikler, test veya hazırlama dağıtımları daha az sık olarak dağıtılır.

> [!NOTE]
> Birden çok sunucu arasında bir veritabanı dağıtımını ölçekleme, bu öğreticinin kapsamı dışındadır olduğu. Bu alan hakkında daha fazla bilgi için lütfen başvurun [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).


Örneğin, bizim [öğretici senaryo](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), bir takım derleme sunucusu Kişi Yöneticisi Çözümü derleyin ve tek bir adımda bir hazırlık ortamına dağıtma izin derleme tanımlarını içerir. Uygulama güvenlik gereksinimleri ve ağ altyapısı tarafından uygulanan kısıtlamaları nedeniyle bir üretim ortamında dağıtılmaya hazır olduğunda üretim ortamı yönetici el ile web paketini bir üretim web sunucusuna kopyalayın ve içeri aktarma Bu Internet Information Services (IIS) Yöneticisi aracılığıyla.

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Çözüme genel bakış

Bu senaryoda, bu bilgileri dağıtım gereksinimleri analizini gelen çıkarabilir:

- Güvenlik kısıtlamaları ve ağ yapılandırması nedeniyle, tek tıklamayla veya otomatik dağıtımı desteklemek üzere üretim ortamına yapılandıramazsınız. Bu senaryoda yalnızca uygun yaklaşımı çevrimdışı dağıtımıdır.
- Web Farm Framework (WFF) bir sunucu grubu oluşturmak için kullanabileceğiniz şekilde üretim ortamına birden çok web sunucusu içerir. Bu yaklaşımı kullanarak yöneticisinin yalnızca bir web sunucusu (birincil) uygulamasını içeri aktarmak gereken ve diğer tüm web sunucularına üretim ortamında dağıtım WFF çoğaltır.

Bu konular bu görevleri tamamlamak için gereken tüm bilgileri sağlar:

- [Web Farm Framework ile bir sunucu grubu oluştur](configuring-a-database-server-for-web-deploy-publishing.md). Bu konu, oluşturmak ve web platformu ürünlerini ve bileşenleri, yapılandırma ayarlarını ve Web siteleri ve uygulamaları birden çok yük dengeli web sunucusu arasında çoğaltılmasını WFF, kullanan bir sunucu grubu yapılandırmak açıklar.
- [Bir Web sunucusunu Web dağıtımı yayımlama (çevrimdışı dağıtım) için yapılandırma](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Bu konuda Yöneticiler içeri aktarma ve temiz bir Windows Server 2008 R2 derleme başlatma web paketlerini el ile dağıtma olanak tanıyan bir web sunucusu nasıl oluşturulduğu açıklanır.
- [Bir veritabanı sunucusunu Web dağıtımı yayımlama için yapılandırma](configuring-a-database-server-for-web-deploy-publishing.md). Bu konuda, uzaktan erişim ve dağıtımını, bir SQL Server 2008 R2'in varsayılan yüklemesinden başlatma desteklemek için bir veritabanı sunucusunun nasıl yapılandırılacağı açıklanmaktadır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Tipik Geliştirici test ortamı yapılandırma ile ilgili yönergeler için bkz. [senaryo: Web dağıtımı için bir Test Ortamı yapılandırma](scenario-configuring-a-test-environment-for-web-deployment.md). Tipik bir hazırlık ortamı yapılandırma hakkında yönergeler için bkz. [senaryo: Web dağıtımı için bir hazırlık ortamı yapılandırma](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Önceki](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [İleri](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
