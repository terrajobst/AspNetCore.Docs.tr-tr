---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Senaryo: Web dağıtımı için bir Test Ortamı yapılandırma | Microsoft Docs'
author: jrjlee
description: Bu konu, bir geliştiricinin normal web dağıtım senaryosu açıklanmaktadır veya test ortamları ve bir sı ayarlamak için tamamlamanız gereken görevleri açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f8636fab82b63edab50fc13ae32f4dd536f133ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382500"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Senaryo: Web dağıtımı için bir Test Ortamı yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda tipik web dağıtım senaryosu için geliştirici veya test ortamları ve benzer bir ortamı ayarlamak için tamamlanması gereken görevleri açıklar.


Geliştiricilere web uygulamaları üzerinde çalışırken, bunlar genellikle erişim gerçekçi bir ayarda uygulamalarına değişiklikleri test etmek için kullanabileceğiniz bir sunucu ortamınıza verilir. Bu tür bir geliştirme veya test ortamı genellikle şu özelliklere sahiptir:

- Ortamı, tek bir web sunucusu ve tek veritabanı sunucusu oluşur.
- Geliştiriciler genellikle uygulamalarını gereksinimler için ortamı yapılandırmak bildirmek için bu sunucularda yönetici ayrıcalıklarına sahip.
- Değişiklikleri uygulamalara ortamı tek adımlı desteklemek gereken şekilde sık olarak dağıtılan veya otomatik dağıtım.

Örneğin, bizim [öğretici senaryo](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink Fabrikam, Inc. şirketinde bir geliştirici olan Matt Kişi Yöneticisi çözümü çalıştığından ve değişiklikleri bir test ortamına dağıtmak düzenli olarak gerekiyor. Matt, test web sunucusu ve test veritabanı sunucusunda bir yöneticidir. Başlangıçta, çözüm doğrudan test ortamına dağıtın edebilmek Matt gerekir.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

İş ilerler ve geliştiricilerin takım, kişi yöneticisi çözümü sürekli tümleştirme (CI) Team Foundation Server (TFS) için yapılandırılmış katılın. Bir geliştirici içeriği denetimi gerçekleştirildiğinde, ekip Çözümü derleyin, herhangi bir birim testlerini çalıştırmak ve otomatik olarak bir çözümü test ortamına dağıtın.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Çözüme genel bakış

Test ortamı tek adımlı desteklemesi gerekir veya uzak bir bilgisayardan dağıtım için iki ana yaklaşım seçeneğiniz otomatik. Şunları yapabilirsiniz:

- Test web sunucusunu Web dağıtımı Aracı hizmeti ("Uzak Aracı") dağıtımı desteklemek üzere yapılandırın.
- Test web sunucusunu Web dağıtımı işleyicisi kullanarak dağıtımı desteklemek üzere yapılandırın.

> [!NOTE]
> Ayrıca kullanabileceğinizi [Web dağıtımı isteğe bağlı](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("geçici agent"). Bu gereksinimleri ve kısıtlamaları açısından uzak aracı yaklaşımı benzer.


Bu durumda, geliştiriciler hedef sunuculara yönetici ayrıcalıklarına sahip ve mantıksal seçimi, uzak aracı kullanarak dağıtımı desteklemek üzere test web sunucusunu yapılandırmak için bu nedenle test ortamı katı güvenlik kısıtlamalarına tabi değildir. Bu daha az karmaşık olan ve Web dağıtımı işleyicisi yaklaşım daha az başlangıç yapılandırmasını gerektirir. Uzaktan erişim ve dağıtımını desteklemek için veritabanı sunucunuzu yapılandırmak gerekir.

Bu konular bu görevleri tamamlamak için gereken tüm bilgileri sağlar:

- [Bir Web sunucusunu Web dağıtımı yayımlama (Uzak Aracı) için yapılandırma](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Bu konuda, temiz bir Windows Server 2008 R2 derleme başlatma yayımlama, uzak aracı yaklaşımı kullanarak Web dağıtımını destekleyen bir web sunucusu nasıl oluşturulduğu açıklanır.
- [Bir veritabanı sunucusunu Web dağıtımı yayımlama için yapılandırma](configuring-a-database-server-for-web-deploy-publishing.md). Bu konuda, uzaktan erişim ve dağıtımını, bir SQL Server 2008 R2'in varsayılan yüklemesinden başlatma desteklemek için bir veritabanı sunucusunun nasıl yapılandırılacağı açıklanmaktadır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Tipik bir hazırlık ortamı yapılandırma hakkında yönergeler için bkz. [senaryo: Web dağıtımı için bir hazırlık ortamı yapılandırma](scenario-configuring-a-staging-environment-for-web-deployment.md). Tipik bir üretim ortamı yapılandırma hakkında yönergeler için bkz. [senaryo: Web dağıtımı için bir üretim ortamı yapılandırma](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Önceki](choosing-the-right-approach-to-web-deployment.md)
> [İleri](scenario-configuring-a-staging-environment-for-web-deployment.md)
