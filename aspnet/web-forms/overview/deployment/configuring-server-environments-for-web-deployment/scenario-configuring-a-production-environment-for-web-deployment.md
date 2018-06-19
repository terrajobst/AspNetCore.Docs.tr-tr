---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Senaryo: bir üretim ortamında Web dağıtımı için yapılandırma | Microsoft Docs'
author: jrjlee
description: Bu konu, bir üretim ortamı için tipik web dağıtım senaryosunu açıklar ve benzer bir ayarlamak için tamamlanması gereken görevler açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4de5b1f20f3adcb53765c7cb9765c0d90a80e677
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882952"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Senaryo: bir üretim ortamı için Web dağıtımı yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, bir üretim ortamı için tipik web dağıtım senaryosunu açıklar ve benzer bir ortamı kurmanız için tamamlanması gereken görevler açıklanmaktadır.


Üretim ortamında bir web uygulaması veya bir Web sitesi son hedefi değil. Bu nokta uygulamanızı test üzerinden yapıldı, hazırlama ortamına dağıtılan ve "Canlı gidin." hazır Bir üretim ortamında özelliklerini yaygın yapısı ve web içeriğinize amacı, kuruluşunuz, hedef kitle ve diğer etkenlere bağlı çok sayıda boyutunu göre değişebilir. Bir kurumsal ölçekte senaryosunda, üretim ortamında bu özelliklere sahip:

- Birden çok yük dengeli web sunucuları ve bir veya daha fazla veritabanı sunucuları, genellikle Yük Devretme Kümelemesi ve veritabanı yansıtma ile ortam oluşur.
- Ortam Internet'e ise, iç ağınızdan yinelenmeli olasıdır. Bir çevre ağında farklı bir alt ağ üzerinde olabilir, farklı bir etki alanı üzerinde olabilir ve tamamen farklı ağ altyapısı üzerinde olabilir.
- Geliştiriciler ve yapı sunucu işlemi hesapları üretim sunucularında yönetici ayrıcalıklarına sahip düşüktür.
- Uygulamaları yapılan değişiklikler, test veya hazırlama dağıtımları daha az düzenli aralıklarla dağıtılır.

> [!NOTE]
> Birden çok sunucu arasında bir veritabanı dağıtım ölçeklendirme Bu öğretici kapsamında değildir. Bu alan hakkında daha fazla bilgi için lütfen bakın [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).


Örneğin, bizim [öğretici senaryo](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), ekip sunucu Contact Manager çözümü oluşturmak ve tek bir adım hazırlama ortamında dağıtmak kullanıcıların izin derleme tanımları içerir. Uygulama güvenlik gereksinimleri ve ağ altyapısı tarafından uygulanan kısıtlamaları nedeniyle üretime hazır olduğunda üretim ortamı yönetici el ile web paketini bir üretim web sunucusuna kopyalayın ve alma Internet Information Services (IIS) Yöneticisi üzerinden.

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Çözüme genel bakış

Bu senaryoda, bu bilgiler dağıtım gereksinimleri analizini öğesinden türetme:

- Güvenlik kısıtlamaları ve ağ yapılandırması nedeniyle, tek tıklamayla veya otomatik dağıtım desteklemek üzere üretim ortamına yapılandıramazsınız. Bu senaryoda yalnızca uygun yaklaşımı çevrimdışı dağıtımıdır.
- Web grubu çerçevesi (WFF) bir sunucu grubu oluşturmak için kullanabileceğiniz şekilde üretim ortamında, birden çok web sunucusu içerir. Bu yaklaşımı kullanarak, yönetici, uygulama bir web sunucusu (birincil) üzerine aktarmak yalnızca gerekir ve WFF diğer tüm web sunucularındaki üretim ortamında dağıtım çoğaltır.

Bu konular bu görevleri tamamlamak için gereken tüm bilgileri sağlar:

- [Web Farm Framework ile bir sunucu grubu oluşturma](configuring-a-database-server-for-web-deploy-publishing.md). Bu konu, oluşturma ve böylece web platformu ürünlerini ve bileşenleri, yapılandırma ayarlarını ve Web siteleri ve uygulamaları birden çok yük dengeli web sunucusu arasında çoğaltılır WFF, kullanan bir sunucu grubu yapılandırma açıklar.
- [Web dağıtımı için yayımlama (çevrimdışı dağıtımı) bir Web sunucusu yapılandırma](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Bu konu, yöneticilerin aktarabilir ve web paketleri el ile temiz bir Windows Server 2008 R2 derleme başlatma dağıtabilirsiniz olanak sağlayan bir web sunucusu oluşturmak açıklar.
- [Web dağıtımı yayımlama için veritabanı sunucusunu yapılandırın](configuring-a-database-server-for-web-deploy-publishing.md). Bu konuda, uzaktan erişim ve dağıtım, bir SQL Server 2008 R2'in varsayılan yüklemesinden başlatma desteklemek için veritabanı sunucusunun nasıl yapılandırılacağı açıklanmaktadır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Tipik Geliştirici test ortamını yapılandırma ile ilgili yönergeler için bkz: [senaryo: bir Test ortamı için Web dağıtımı yapılandırma](scenario-configuring-a-test-environment-for-web-deployment.md). Tipik bir hazırlama ortamını yapılandırma ile ilgili yönergeler için bkz: [senaryo: Web dağıtımı için bir hazırlama ortamını yapılandırma](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Önceki](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [sonraki](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
