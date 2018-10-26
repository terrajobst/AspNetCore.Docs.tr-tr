---
title: ASP.NET Core ve Azure ile DevOps | Sonraki adımlar
author: CamSoper
description: Azure'da barındırılan bir ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturmaya uçtan uca yönergeler sağlar. bir kılavuz.
ms.author: casoper
ms.custom: mvc
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: b82e7251b507f8d141930673d50722cfaa576db5
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089887"
---
# <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, örnek ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturdunuz. Tebrikler! ASP.NET Core web uygulamaları, Azure App Service'e yayımlama ve değişiklikler için sürekli tümleştirme otomatikleştirmek öğrenme keyif umuyoruz.

Web barındırma ve DevOps, Azure platformu-olarak-hizmet (PaaS) Hizmetleri ASP.NET Core geliştiricileri için kullanışlı bir çeşit vardır. Bu bölümde bazı sık kullanılan hizmetler kısa bir genel bakış sağlar.

## <a name="storage-and-databases"></a>Depolama ve veritabanları

[Redis cache](/azure/redis-cache/) yüksek performanslı, düşük gecikme süreli veri önbelleğe alma bir hizmet olarak kullanılabilir. Sayfa çıktısını önbelleğe alma, veritabanı istekleri azaltma ve bir uygulama birden çok örneği üzerinde ASP.NET Core oturum durumu sağlamak için kullanılabilir.

[Azure depolama](/azure/storage/) Azure'nın yüksek düzeyde ölçeklenebilir bulut depolamadır. Geliştiriciler yararlanabilir [kuyruk depolama](/azure/storage/queues/storage-queues-introduction) güvenilir message queuing için ve [tablo depolama](/azure/storage/tables/table-storage-overview) olan, muazzam, yarı yapılandırılmış veri kümeleri aracılığıyla hızlı geliştirmeye yönelik olarak tasarlanan NoSQL anahtar-değer deposu.

[Azure SQL veritabanı](/azure/sql-database/) Microsoft SQL Server altyapısını kullanan bir hizmet olarak bilinen ilişkisel veritabanı işlevlerini sağlar.

[Cosmos DB](/azure/cosmos-db/) Global olarak dağıtılmış, çok modelli bir NoSQL veritabanı hizmeti. Birden çok API'ları (eski adıyla DocumentDB olarak adlandırılır) SQL API'si, Cassandra ve MongoDB gibi kullanılabilir.

## <a name="identity"></a>Kimlik

[Azure Active Directory](/azure/active-directory/) ve [Azure Active Directory B2C](/azure/active-directory-b2c/) her iki kimlik hizmetleridir. Azure Active Directory kuruluş senaryoları için tasarlanmıştır ve Azure AD B2B (işletmeler arası) işbirliği, sosyal ağ oturum açma dahil olmak üzere, amaçlanan iş müşteri senaryoları olsa da Azure Active Directory B2C'yi etkinleştirir.

## <a name="mobile"></a>Mobil

[Bildirim hub'ları](/azure/notification-hubs/) hızlıca çeşitli cihaz türleri üzerinde çalışan uygulamalara milyonlarca mesaj göndermek için çok platformlu, ölçeklenebilir anında iletme bildirimi altyapısı.

## <a name="web-infrastructure"></a>Web altyapısı

[Azure Container Service](/azure/aks/) hızla ve kolayca kapsayıcı düzenleme uzmanlığı gerektirmeden kapsayıcıya alınmış uygulamaları dağıtma ve yönetme, barındırılan Kubernetes ortamınızı yönetir.

[Azure Search'ü](/azure/search/) özel ve heterojen içerikler üzerinde kurumsal arama çözümü oluşturmak için kullanılır.

[Service Fabric](/azure/service-fabric/) kolaylaştıran paketlemek, dağıtmak ve yönetmek ölçeklenebilir bir dağıtılmış sistemler platformudur ve güvenilir mikro Hizmetleri ve kapsayıcıları.
