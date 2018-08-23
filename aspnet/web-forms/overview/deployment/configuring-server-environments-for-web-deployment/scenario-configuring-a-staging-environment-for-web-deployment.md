---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Senaryo: Web dağıtımı için hazırlama ortamı yapılandırma | Microsoft Docs'
author: jrjlee
description: Bu konu, tipik web dağıtım senaryosu için bir hazırlık ortamı açıklar ve benzer bir env ayarlamak için tamamlamanız gereken görevleri açıklar...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: bee856c2ece6fda90ec35a87d111715def990603
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757327"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Senaryo: Web dağıtımı için hazırlama ortamı yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, tipik web dağıtım senaryosu için bir hazırlık ortamı açıklar ve benzer bir ortamı ayarlamak için tamamlanması gereken görevler açıklanmaktadır.


Kuruluşlar çok sayıda web uygulamaları veya Web siteleri için güncelleştirmeleri önizlemek için hazırlama ortamlarını kullanın. Bu kuruluş içindeki kişilerin keşfedin ve site "Canlı gider" veya diğer bir deyişle bir üretim ortamına dağıtıldıktan önce yeni işlevsellik ve içeriği gözden geçirmek için bir fırsat sağlar. Hazırlık ortamından üretim ortamına gerçekçi bir önizleme sağlamak için mümkün olduğunca çoğaltmak için tasarlanmıştır. Bu tür bir hazırlık ortamı genellikle şu özelliklere sahiptir:

- Ortamın birden çok yük dengeli web sunucusu ve bir veya daha fazla veritabanı sunucuları, genellikle Yük Devretme Kümelemesi ve veritabanı yansıtma oluşur.
- Uygulamaları el ile bir geliştirme ekibi tarafından veya bir takım derleme sunucusu tarafından otomatik olarak dağıtılabilir.
- Kullanıcı veya uygulama dağıtma işlemi hesapları hazırlama sunucularında yönetici ayrıcalıklarına sahip düşüktür.
- Değişiklikleri uygulamalara ortamı tek adımlı desteklemek gereken şekilde sık olarak dağıtılan veya otomatik dağıtım.

> [!NOTE]
> Birden çok sunucu arasında bir veritabanı dağıtımını ölçekleme, bu öğreticinin kapsamı dışındadır olduğu. Bu alan hakkında daha fazla bilgi için lütfen başvurun [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).


Örneğin, bizim [öğretici senaryo](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) Kişi Yöneticisi çözümü yönetir. TFS Yöneticisi, Rob Aksoy geliştiricilerinin hazırlık ortamına gerektiği gibi bir dağıtımı tetikleyecek bir yapı tanımını oluşturmuştur.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Çoğu durumda, mutlaka en son sürüme hazırlama ortamına dağıtmak istediğiniz olmaz olduğunu unutmayın. Bunun yerine, doğrulama ve doğrulama test ortamında zaten gerçekleştirdi, belirli bir yapıyı dağıtmak istediğiniz çok daha büyük olasılıkla.

## <a name="solution-overview"></a>Çözüme genel bakış

Bu senaryoda, bu bilgileri dağıtım gereksinimleri analizini gelen çıkarabilir:

- Hazırlama web sunucularını yönetici olmayan dağıtım desteklemesi için dağıtımı gerçekleştiren kullanıcı veya işlem hesabı hazırlama sunucularında yönetici ayrıcalıklarına sahip olmaz. Bu nedenle, uzak aracı yerine Web dağıtımı işleyicisi kullanmak üzere hazırlama web sunucularını yapılandırma gerekir.
- Birden çok web sunucusu hazırlık ortamı içerir ancak bu Web grubu çerçevesi (WFF) bir sunucu grubu oluşturmak için kullanılacak ihtiyacınız olacak şekilde tek tıklamayla veya otomatik dağıtım desteklemesi gerekir. Bu yaklaşımı kullanarak bir web sunucusu (birincil) bir uygulamayı dağıtabilirsiniz ve diğer tüm web sunucuları hazırlama ortamında dağıtımı WFF çoğaltır.
- Dağıtımı gerçekleştiren işlem hesabı ve kullanıcı veritabanları oluşturma izni olması gerekir. Bu nedenle, eklemeniz gerekecektir **dbcreator** uzaktan erişim ve dağıtımını desteklemek için veritabanı sunucusunu yapılandırmaya ek olarak veritabanı sunucusunda, sunucu rolü.

Bu konular bu görevleri tamamlamak için gereken tüm bilgileri sağlar:

- [Web Farm Framework ile bir sunucu grubu oluştur](creating-a-server-farm-with-the-web-farm-framework.md). Bu konu, oluşturmak ve web platformu ürünlerini ve bileşenleri, yapılandırma ayarlarını ve Web siteleri ve uygulamaları birden çok yük dengeli web sunucusu arasında çoğaltılmasını WFF, kullanan bir sunucu grubu yapılandırmak açıklar.
- [Bir Web sunucusunu Web dağıtımı yayımlama için yapılandırma (Web dağıtımı işleyicisi)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Bu konuda, temiz bir Windows Server 2008 R2 derleme başlatma yayımlama, uzak aracı yaklaşımı kullanarak Web dağıtımını destekleyen bir web sunucusu nasıl oluşturulduğu açıklanır.
- [Bir veritabanı sunucusunu Web dağıtımı yayımlama için yapılandırma](configuring-a-database-server-for-web-deploy-publishing.md). Bu konuda, uzaktan erişim ve dağıtımını, bir SQL Server 2008 R2'in varsayılan yüklemesinden başlatma desteklemek için bir veritabanı sunucusunun nasıl yapılandırılacağı açıklanmaktadır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Tipik Geliştirici test ortamı yapılandırma ile ilgili yönergeler için bkz. [senaryo: Web dağıtımı için bir Test Ortamı yapılandırma](scenario-configuring-a-test-environment-for-web-deployment.md). Tipik bir üretim ortamı yapılandırma hakkında yönergeler için bkz. [senaryo: Web dağıtımı için bir üretim ortamı yapılandırma](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Önceki](scenario-configuring-a-test-environment-for-web-deployment.md)
> [İleri](scenario-configuring-a-production-environment-for-web-deployment.md)
