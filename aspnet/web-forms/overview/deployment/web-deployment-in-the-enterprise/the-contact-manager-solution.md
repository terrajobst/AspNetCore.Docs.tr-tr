---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Kişi Yöneticisi çözümü | Microsoft Docs
author: jrjlee
description: Bu öğretici serisinde, kullanan örnek bir çözüm&#x2014;Kişi Yöneticisi çözümünü&#x2014;kurumsal ölçekli uygulamayla gerçekçi leve temsil etmek için...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 8187766190da43ded52359892601f8129b9940ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388114"
---
<a name="the-contact-manager-solution"></a>Kişi Yöneticisi çözümü
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu [öğretici serisinde,](web-deployment-in-the-enterprise.md) kullanan örnek bir çözüm&#x2014;Kişi Yöneticisi çözümünü&#x2014;kurumsal ölçekli uygulamayla gerçekçi bir karmaşıklık düzeyi temsil etmek için. Bu konu, kişi yöneticisi çözümünü sunar, çözüm'in temel bileşenlerinden açıklar ve bu tür bir kuruluş ortamında çeşitli Hedef platformlar için uygulama dağıtma karşılaşılan tanımlar.
> 
> Aşağıdaki öğreticilerde konu başlıklarını çalışırken, kurumsal dağıtım senaryolarında özgü zorlukları nasıl karşıladığını gösteren bir başvuru uygulaması olarak kişi yöneticisi çözümünü kullanabilirsiniz. Bir sonraki konu [ayarı oluşturan kişi yöneticisi çözümü](setting-up-the-contact-manager-solution.md), indirmek ve çözümü Geliştirici iş istasyonunuzda çalıştırmak açıklar.


## <a name="solution-overview"></a>Çözüme genel bakış

Kişi Yöneticisi çözümünü dört ayrı ayrı projelerin oluşur:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Çözüm için giriş noktasını temsil eden bir ASP.NET MVC 3 web uygulaması projesi budur. Kullanıcıları oluşturma ve kişi ayrıntılarını görüntüleme olanağı sağlama gibi bazı temel web uygulaması işlevselliği sunar. Uygulamanın, kişiler ve kimlik doğrulama ve yetkilendirme yönetmek için bir ASP.NET uygulama hizmetleri veritabanına yönetmek için bir Windows Communication Foundation (WCF) hizmeti kullanır.
- **ContactManager.Database**. Visual Studio veritabanı projesi budur. Proje depoları ayrıntıları başvurun bir veritabanı için şema tanımlar.
- **ContactManager.Service**. Bir WCF web hizmeti projesi budur. Alma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştirin izin veren bir uç nokta oluşturma, WCF Hizmeti kullanıma sunan **ContactManager** veritabanı. Hizmet **ContactManager** veritabanı ve **ContactManager.Common.dll** derleme.
- **ContactManager.Common**. Bir sınıf kitaplığı projesi budur. Bu derlemede tanımlanan türlerin WCF Hizmeti kullanır.

Çözüm ayrıca Yayımla adlı bir çözüm klasörü içerir. Bu, çeşitli özel proje dosyalarını ve nasıl kontrol edebilir ve düzenleme derleme ve dağıtım sürecini gösteren komut dosyalarını içerir. Bunlar, bu öğreticinin ilerleyen bölümlerinde daha ayrıntılı olarak ele alınmaktadır.

Çözüm bileşenleri, kavramsal bir düzeyde şöyle araya getireceğinizi:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Web uygulaması kapsamındaki tüm sayfaları, ASP.NET MVC 3 web uygulamasına ASP.NET üyelik sağlayıcısını kullanırken, anonim erişime izin. Bu açıkça gerçekçi bir yapılandırma değildir. Ancak, çözümü dağıtın ve kullanıcı hesapları ve rollerini yapılandırmadan çözümü test kolaylaştırmak için bu şekilde ayarlanır.


## <a name="deployment-challenges"></a>Dağıtım zorlukları

Kişi Yöneticisi çözümü, kurumsal dağıtım senaryolarında çok sayıda için ortak olan birkaç dağıtım zorlukları gösterilmektedir:

- Çözüm, birden çok bağımlı projeler oluşur. Bu projeler eş zamanlı olarak dağıtılması gerekir.
- Bağlantı dizelerini ve hizmet uç noktaları her ortam için güncelleştirilmesi gereken ve çok durumlarda bu bilgiler geliştiriciler için kullanılabilir olmayacak.
- Dağıtırken **ContactManager** veritabanı sonraki dağıtımlarda mevcut verilerinizi korumak gereken hazırlık ve üretim ortamları için.
- ASP.NET uygulama hizmetleri veritabanına dağıttığınızda, bazı yapılandırma verilerini dağıtmak, ancak herhangi bir kullanıcı hesabı veri atlarsanız gerekir.
- Bazı dosya ve klasörlerin değil dağıtılmalıdır projeleri içerir. Bu dosya ve klasörler, dağıtım işleminin dışında tutmanız gerekir.
- Team Foundation Server (TFS) yapı sunucusu otomatik dağıtımı desteklemek çözüm gerekir.

## <a name="conclusion"></a>Sonuç

Bu konuda sağlanan Kişi Yöneticisi çözümünü üst düzey bir genel bakış ve birçok kurumsal dağıtım senaryoları için ortak olan doğal dağıtım zorlukları bazılarını belirledik. Bu öğreticide kalan konularda bu zorluklar karşılamak için kullanabileceğiniz yöntemlerden bazıları açıklanmaktadır.

Bir sonraki konu [ayarı oluşturan kişi yöneticisi çözümü](setting-up-the-contact-manager-solution.md), indirmek ve çözümü Geliştirici iş istasyonunuzda çalıştırmak açıklar.

> [!div class="step-by-step"]
> [Önceki](web-deployment-in-the-enterprise.md)
> [İleri](setting-up-the-contact-manager-solution.md)
