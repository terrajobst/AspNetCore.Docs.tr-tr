---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: İlgili Kişi Yöneticisi çözüm | Microsoft Docs
author: jrjlee
description: Bu öğreticiler dizi örnek bir çözüm kullanır&#x2014;Contact Manager çözüm&#x2014;Kurumsal ölçekte uygulamayla gerçekçi leve temsil etmek için...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d7034f800df98747d10401d7e2c7297fea0e46d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30883706"
---
<a name="the-contact-manager-solution"></a>Contact Manager çözümü
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu [eğitim serileri](web-deployment-in-the-enterprise.md) kullanan örnek bir çözüm&#x2014;Contact Manager çözüm&#x2014;Kurumsal ölçekte uygulamayla gerçekçi bir karmaşıklık düzeyi temsil edecek. Bu konu, kişinin Yöneticisi çözüm sunar, çözümün anahtar bileşenlerinin açıklar ve bu tür bir uygulama bir kuruluş ortamında çeşitli hedef platformlara dağıtmanın zorluklar tanımlar.
> 
> Bu öğreticileri konulara çalışırken, kurumsal dağıtım senaryolarında belirli zorluklar nasıl karşılayabileceğini gösteren bir başvuru uygulaması olarak Contact Manager çözümü kullanabilirsiniz. Sonraki konuyu [ayarı yukarı Contact Manager çözüm](setting-up-the-contact-manager-solution.md), indirin ve geliştirici iş istasyonunuza çözümü çalıştırın açıklar.


## <a name="solution-overview"></a>Çözüme genel bakış

Contact Manager çözüm dört ayrı projelerin oluşur:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Çözüm için giriş noktası temsil eden bir ASP.NET MVC 3 web uygulaması projesini budur. Kullanıcılar oluşturma ve kişi ayrıntılarını görüntüleme olanağı sağlamak gibi bazı temel web uygulaması işlevselliği sunar. Uygulamanın, kişiler ve kimlik doğrulama ve yetkilendirme yönetmek için bir ASP.NET uygulama hizmetleri veritabanına yönetmek için bir Windows Communication Foundation (WCF) Hizmeti'ne bağlıdır.
- **ContactManager.Database**. Visual Studio veritabanı projesi budur. Proje depoları başvurun ayrıntıları bir veritabanı için şema tanımlar.
- **ContactManager.Service**. Bir WCF web hizmeti projesi budur. Arayanlar gerçekleştirmek imkan tanıyan bir uç noktası oluşturma, WCF Hizmeti açığa çıkarır alma, güncelleştirme ve üzerinde Sil (CRUD) işlemleridir **ContactManager** veritabanı. Hizmetin kullandığı **ContactManager** veritabanı ve **ContactManager.Common.dll** derleme.
- **ContactManager.Common**. Bir sınıf kitaplığı proje budur. Bu derlemede tanımlanan türü WCF Hizmeti kullanır.

Çözüm ayrıca Yayımla adlı bir çözüm klasör içerir. Bu, çeşitli özel proje dosyalarını ve nasıl kontrol edebilir ve derleme ve dağıtım işlemini işlemek göstermek komut dosyaları içerir. Bu, bu öğreticinin ilerleyen bölümlerinde daha ayrıntılı olarak ele alınmıştır.

Çözüm bileşenlerden, kavramsal bir düzeyde, bu gibi araya getireceğinizi:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> ASP.NET MVC 3 web uygulamasını ASP.NET üyelik sağlayıcısı kullanıyor, ancak web uygulaması kapsamındaki tüm sayfaları anonim erişime izin verin. Bu açıkça gerçekçi bir yapılandırma değildir. Bununla birlikte, çözümü dağıtma ve kullanıcı hesapları ve rolleri yapılandırmadan çözümü test kolaylaştırmak için bu şekilde ayarlanır.


## <a name="deployment-challenges"></a>Dağıtım sorunları

Contact Manager çözüm kurumsal dağıtım senaryoları çok sayıda için ortak olan birkaç dağıtım zorluklar gösterilmektedir:

- Çözüm birden fazla bağımlı projelerin oluşur. Bu projeleri eşzamanlı olarak dağıtmanız gerekir.
- Bağlantı dizeleri ve hizmet uç noktaları her ortam için güncelleştirilmesi gereken ve çok durumlarda bu bilgileri geliştiriciler için kullanılabilir olmayacak.
- Dağıtırken **ContactManager** veritabanı hazırlama ve üretim ortamları için sonraki dağıtımlarda mevcut verilerinizi korumak gerekir.
- ASP.NET uygulama hizmetleri veritabanına dağıttığınızda, bazı yapılandırma verilerini dağıtmak ancak herhangi bir kullanıcı hesabı veri atlayın gerekir.
- Bazı dosya ve klasörleri değil dağıtılmalıdır projeleri içerir. Bu dosyaları ve klasörleri dağıtım işleminin dışında tutmanız gerekir.
- Çözüm, Team Foundation Server (TFS) Yapı sunucusundan otomatik dağıtım desteklemesi gerekir.

## <a name="conclusion"></a>Sonuç

Bu konuda Contact Manager çözüm üst düzey bir genel bakış sağlanmış ve bazı kurumsal dağıtım senaryoları çok sayıda için ortak olan devralınmış dağıtım zorlukların tanımlanır. Bu öğreticide diğer konular bu zorluklar karşılamak için kullanabileceğiniz yöntemlerden bazıları açıklanmaktadır.

Sonraki konuyu [ayarı yukarı Contact Manager çözüm](setting-up-the-contact-manager-solution.md), indirin ve geliştirici iş istasyonunuza çözümü çalıştırın açıklar.

> [!div class="step-by-step"]
> [Önceki](web-deployment-in-the-enterprise.md)
> [sonraki](setting-up-the-contact-manager-solution.md)
