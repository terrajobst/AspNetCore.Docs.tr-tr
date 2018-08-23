---
title: ASP.NET Core ve Azure ile DevOps | Sonraki adımlar
author: CamSoper
description: Azure'da barındırılan bir ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturmaya uçtan uca yönergeler sağlar. bir kılavuz.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/next-steps
ms.openlocfilehash: 7a0f1b1b56a33b1870e0657d8ba465adb84f5a02
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754040"
---
# <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, örnek ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturdunuz. Tebrikler! ASP.NET Core web uygulamaları, Azure App Service'e yayımlama ve değişiklikler için sürekli tümleştirme otomatikleştirmek öğrenme keyif umuyoruz.

Web barındırma ve DevOps, Azure platformu-olarak-hizmet (PaaS) Hizmetleri ASP.NET Core geliştiricileri için kullanışlı bir çeşit vardır. Bu bölümde bazı sık kullanılan hizmetler kısa bir genel bakış sağlar.

## <a name="storage-and-databases"></a>Depolama ve veritabanları

[Redis cache](https://docs.microsoft.com/azure/redis-cache/) yüksek performanslı, düşük gecikme süreli veri önbelleğe alma bir hizmet olarak kullanılabilir. Sayfa çıktısını önbelleğe alma, veritabanı istekleri azaltma ve bir uygulama birden çok örneği üzerinde ASP.NET Core oturum durumu sağlamak için kullanılabilir.

[Azure depolama](https://docs.microsoft.com/azure/storage/) Azure'nın yüksek düzeyde ölçeklenebilir bulut depolamadır. Geliştiriciler yararlanabilir [kuyruk depolama](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) güvenilir message queuing için ve [tablo depolama](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) olan, muazzam, yarı yapılandırılmış veri kümeleri aracılığıyla hızlı geliştirmeye yönelik olarak tasarlanan NoSQL anahtar-değer deposu.

[Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/) Microsoft SQL Server altyapısını kullanan bir hizmet olarak bilinen ilişkisel veritabanı işlevlerini sağlar.

[Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) Global olarak dağıtılmış, çok modelli bir NoSQL veritabanı hizmeti. Birden çok API'ları (eski adıyla DocumentDB olarak adlandırılır) SQL API'si, Cassandra ve MongoDB gibi kullanılabilir.

## <a name="identity"></a>Kimlik

[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) ve [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) her iki kimlik hizmetleridir. Azure Active Directory kuruluş senaryoları için tasarlanmıştır ve Azure AD B2B (işletmeler arası) işbirliği, sosyal ağ oturum açma dahil olmak üzere, amaçlanan iş müşteri senaryoları olsa da Azure Active Directory B2C'yi etkinleştirir.

## <a name="mobile"></a>Mobil

[Bildirim hub'ları](https://docs.microsoft.com/azure/notification-hubs/) hızlıca çeşitli cihaz türleri üzerinde çalışan uygulamalara milyonlarca mesaj göndermek için çok platformlu, ölçeklenebilir anında iletme bildirimi altyapısı.

## <a name="web-infrastructure"></a>Web altyapısı

[Azure Container Service](https://docs.microsoft.com/azure/aks/) hızla ve kolayca kapsayıcı düzenleme uzmanlığı gerektirmeden kapsayıcıya alınmış uygulamaları dağıtma ve yönetme, barındırılan Kubernetes ortamınızı yönetir.

[Azure Search'ü](https://docs.microsoft.com/azure/search/) özel ve heterojen içerikler üzerinde kurumsal arama çözümü oluşturmak için kullanılır.

[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) kolaylaştıran paketlemek, dağıtmak ve yönetmek ölçeklenebilir bir dağıtılmış sistemler platformudur ve güvenilir mikro Hizmetleri ve kapsayıcıları.
