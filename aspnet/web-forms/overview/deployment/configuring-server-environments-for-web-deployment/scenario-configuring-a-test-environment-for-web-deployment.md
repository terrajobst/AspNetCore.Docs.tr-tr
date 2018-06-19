---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Senaryo: bir Test ortamı için Web dağıtımı yapılandırma | Microsoft Docs'
author: jrjlee
description: Bu konu, geliştirici için tipik web dağıtım senaryosu açıklanmaktadır veya sınama ortamlarında ve si ayarlamak için tamamlanması gereken görevler açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2976be642815e715ac19bd9db34485cf5474cb32
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879861"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Senaryo: bir Test ortamı için Web dağıtımı yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, geliştirici için tipik web dağıtım senaryosu açıklanmaktadır veya sınama ortamlarında ve benzer bir ortamı kurmanız için tamamlanması gereken görevler açıklanmaktadır.


Geliştiricilerin web uygulamaları üzerinde çalışırken, bunlar genellikle erişim uygulamalarını değişiklikler gerçekçi ayarında test etmek için kullanabileceğiniz bir sunucu ortamı için verilir. Bu tür bir geliştirme veya test ortamına genellikle şu özelliklere sahiptir:

- Ortamında bir tek bir web sunucusu ve tek veritabanı sunucusu oluşur.
- Geliştiriciler genellikle uygulamalarını gereksinimleri için ortamı yapılandırmak bildirmek için sunucularında yönetici ayrıcalıklarına sahip.
- Uygulamaları değişiklikleri düzenli aralıklarla üzerinde dağıtılır, bu nedenle ortamı tek adımlı desteklemesi gerekir veya otomatik dağıtım.

Örneğin, bizim [öğretici senaryo](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink olan bir geliştirici, Fabrikam, Inc. Matt Contact Manager çözüm üzerinde çalışmaktadır ve değişiklikleri bir test ortamını dağıtmak düzenli olarak gerekiyor. Matt test web sunucusunda ve test veritabanı sunucusunda bir yöneticidir. Başlangıçta, Matt çözümü doğrudan sınama ortamında dağıtmak gerekir.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

İş ilerler ve geliştiricilerin kişi çözüm sürekli tümleştirme (CI) Team Foundation Server (TFS) için yapılandırılmış Yöneticisi'ni takım katılın. İçeriğinde bir geliştirici denetler her ekip çözümü oluşturun, tüm birim testleri çalıştırma ve otomatik olarak çözümü sınama ortamında dağıtmak.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Çözüme genel bakış

Sınama ortamı tek adımlı desteklemesi gerekir veya uzak bir bilgisayardan dağıtım iki ana yaklaşım seçmesini şekilde otomatik. Şunları yapabilirsiniz:

- Web dağıtım aracı hizmetini ("Uzak Aracı") kullanarak Dağıtımınızı desteklemek için test web sunucusunu yapılandırın.
- Web dağıtımı işleyici kullanarak dağıtımı desteklemek üzere test web sunucusunu yapılandırın.

> [!NOTE]
> De kullanabilirsiniz [Web dağıtımı isteğe bağlı](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("geçici Aracısı"). Bu gereksinimleri ve kısıtlamaları bakımından uzak aracı yaklaşım benzer.


Bu durumda, geliştiriciler, hedef sunucular üzerinde yönetici ayrıcalıklarına sahip ve mantıksal seçimi uzak aracı kullanarak dağıtımı desteklemek üzere test web sunucusunu yapılandırma olacak şekilde test ortamı sıkı güvenlik kısıtlamaları tabi değil. Bu daha az karmaşıktır ve Web dağıtımı işleyicisi yaklaşımı daha az başlangıç yapılandırmasını gerektirir. Uzaktan erişim ve dağıtımını desteklemek için veritabanı sunucunuzu yapılandırmak gerekir.

Bu konular bu görevleri tamamlamak için gereken tüm bilgileri sağlar:

- [Web dağıtımı için yayımlama (Uzak Aracı) bir Web sunucusu yapılandırma](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Bu konu, temiz bir Windows Server 2008 R2 derleme başlatma yayımlama, uzak aracı yaklaşımı kullanarak Web dağıtımını destekleyen bir web sunucusu oluşturmak açıklar.
- [Web dağıtımı yayımlama için veritabanı sunucusunu yapılandırın](configuring-a-database-server-for-web-deploy-publishing.md). Bu konuda, uzaktan erişim ve dağıtım, bir SQL Server 2008 R2'in varsayılan yüklemesinden başlatma desteklemek için veritabanı sunucusunun nasıl yapılandırılacağı açıklanmaktadır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Tipik bir hazırlama ortamını yapılandırma ile ilgili yönergeler için bkz: [senaryo: Web dağıtımı için bir hazırlama ortamını yapılandırma](scenario-configuring-a-staging-environment-for-web-deployment.md). Tipik bir üretim ortamını yapılandırma ile ilgili yönergeler için bkz: [senaryo: bir üretim ortamında Web dağıtım için yapılandırma](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Önceki](choosing-the-right-approach-to-web-deployment.md)
> [sonraki](scenario-configuring-a-staging-environment-for-web-deployment.md)
