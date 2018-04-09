---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Senaryo: Web dağıtımı için hazırlama ortamını yapılandırma | Microsoft Docs'
author: jrjlee
description: Bu konu, hazırlama ortamına yönelik normal web dağıtım senaryosunu açıklar ve benzer env ayarlamak için tamamlanması gereken görevler açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 3864559b0599091beeacb87e90e80a51285039df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Senaryo: Web dağıtımı için hazırlama ortamını yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, hazırlama ortamına yönelik normal web dağıtım senaryosunu açıklar ve benzer bir ortamı kurmanız için tamamlanması gereken görevler açıklanmaktadır.


Kuruluşlar çok sayıda hazırlama ortamlarını web uygulamaları veya Web siteleri için güncelleştirmeleri önizlemek için kullanın. Bu kişiler kuruluş içindeki keşfedin ve site "Canlı geçip geçmeyeceğini" veya diğer bir deyişle bir üretim ortamına dağıtılıyor önce yeni işlevsellik veya içeriği gözden geçirmek için bir fırsat sağlar. Hazırlama ortamında üretim ortamı gerçekçi Önizleme sağlamak için mümkün olduğunca yakın çoğaltmak için tasarlanmıştır. Bu tür bir hazırlama ortamında genellikle şu özelliklere sahiptir:

- Birden çok yük dengeli web sunucuları ve bir veya daha fazla veritabanı sunucuları, genellikle Yük Devretme Kümelemesi ve veritabanı yansıtma ile ortam oluşur.
- Uygulamaları el ile geliştirme ekibi tarafından veya bir ekip sunucusu tarafından otomatik olarak dağıtılabilir.
- Kullanıcı veya uygulamaları dağıtma işlemi hesapları hazırlama sunucularında yönetici ayrıcalıklarına sahip düşüktür.
- Uygulamaları değişiklikleri düzenli aralıklarla üzerinde dağıtılır, bu nedenle ortamı tek adımlı desteklemesi gerekir veya otomatik dağıtım.

> [!NOTE]
> Birden çok sunucu arasında bir veritabanı dağıtım ölçeklendirme Bu öğretici kapsamında değildir. Bu alan hakkında daha fazla bilgi için lütfen bakın [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).


Örneğin, bizim [öğretici senaryo](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) Contact Manager çözüm yönetir. TFS yönetici, Ramiz Aksoy hazırlama ortamına gerektiği gibi bir dağıtımı tetiklemek geliştiricilerinin bir derleme tanımınız oluşturdu.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Çoğu durumda, mutlaka en son sürüme hazırlama ortamına dağıtmak istediğiniz olmaz olduğunu unutmayın. Bunun yerine, doğrulama ve test ortamında doğrulama zaten gerçekleştirdi belirli bir yapı dağıtmak istediğiniz çok büyük olasılıkla.

## <a name="solution-overview"></a>Çözüme genel bakış

Bu senaryoda, bu bilgiler dağıtım gereksinimleri analizini öğesinden türetme:

- Hazırlama web sunucularını yönetici olmayan dağıtım desteklemesi için dağıtımı gerçekleştiren kullanıcı veya işlem hesap hazırlama sunucularında yönetici ayrıcalıklarına sahip olmaz. Bu nedenle, uzak aracı yerine Web dağıtımı işleyicisi kullanmak üzere hazırlama web sunucuları yapılandırmanız gerekir.
- Hazırlama ortamında birden çok web sunucusu içerir ancak, bir sunucu grubu oluşturmak için Web grubu çerçevesi (WFF) kullanmanız gerekir böylece tek tıklamayla veya otomatik dağıtım desteklemesi gerekir. Bu yaklaşımı kullanarak, bir web sunucusu (birincil) için bir uygulama dağıtabilirsiniz ve diğer tüm web sunucuları hazırlama ortamında dağıtımı WFF çoğaltır.
- Kullanıcı veya dağıtım gerçekleştirir işlem hesabı veritabanı oluşturma izinleri olmalıdır. Bu nedenle, hesaba eklemeniz gerekir **dbcreator** uzaktan erişim ve dağıtımını desteklemek için veritabanı sunucusunu yapılandırma ek olarak veritabanı sunucusundaki sunucu rolü.

Bu konular bu görevleri tamamlamak için gereken tüm bilgileri sağlar:

- [Web Farm Framework ile bir sunucu grubu oluşturma](creating-a-server-farm-with-the-web-farm-framework.md). Bu konu, oluşturma ve böylece web platformu ürünlerini ve bileşenleri, yapılandırma ayarlarını ve Web siteleri ve uygulamaları birden çok yük dengeli web sunucusu arasında çoğaltılır WFF, kullanan bir sunucu grubu yapılandırma açıklar.
- [Web dağıtımı için yayımlama bir Web sunucusu yapılandırma (Web dağıtımı işleyici)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Bu konu, temiz bir Windows Server 2008 R2 derleme başlatma yayımlama, uzak aracı yaklaşımı kullanarak Web dağıtımını destekleyen bir web sunucusu oluşturmak açıklar.
- [Web dağıtımı yayımlama için veritabanı sunucusunu yapılandırın](configuring-a-database-server-for-web-deploy-publishing.md). Bu konuda, uzaktan erişim ve dağıtım, bir SQL Server 2008 R2'in varsayılan yüklemesinden başlatma desteklemek için veritabanı sunucusunun nasıl yapılandırılacağı açıklanmaktadır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Tipik Geliştirici test ortamını yapılandırma ile ilgili yönergeler için bkz: [senaryo: bir Test ortamı için Web dağıtımı yapılandırma](scenario-configuring-a-test-environment-for-web-deployment.md). Tipik bir üretim ortamını yapılandırma ile ilgili yönergeler için bkz: [senaryo: bir üretim ortamında Web dağıtım için yapılandırma](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Önceki](scenario-configuring-a-test-environment-for-web-deployment.md)
> [sonraki](scenario-configuring-a-production-environment-for-web-deployment.md)
