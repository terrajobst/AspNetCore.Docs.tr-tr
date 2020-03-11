---
title: Sonraki adımlar - ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: Ek ASP.NET Core ve Azure ile DevOps için öğrenme yolları.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: a775dc42551a43bcce72b5f9ca364ed00b1dc4e6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659476"
---
# <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, örnek ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturdunuz. Tebrikler! ASP.NET Core web uygulamaları, Azure App Service'e yayımlama ve değişiklikler için sürekli tümleştirme otomatikleştirmek öğrenme keyif umuyoruz.

Web barındırma ve DevOps, Azure platformu-olarak-hizmet (PaaS) Hizmetleri ASP.NET Core geliştiricileri için kullanışlı bir çeşit vardır. Bu bölümde bazı sık kullanılan hizmetler kısa bir genel bakış sağlar.

## <a name="storage-and-databases"></a>Depolama ve veritabanları

[Redis Cache](/azure/redis-cache/) , yüksek aktarım hızı, düşük gecikmeli veri önbelleğe alma hizmeti olarak kullanılabilir. Sayfa çıktısını önbelleğe alma, veritabanı istekleri azaltma ve bir uygulama birden çok örneği üzerinde ASP.NET Core oturum durumu sağlamak için kullanılabilir.

Azure [depolama](/azure/storage/) , Azure 'un büyük oranda ölçeklenebilir bulut depolama alanı. Geliştiriciler, güvenilir Message Queuing için [sıra depolamadan](/azure/storage/queues/storage-queues-introduction) yararlanabilir ve [Tablo Depolaması](/azure/storage/tables/table-storage-overview) , çok büyük ve yarı yapılandırılmış veri kümeleri kullanılarak hızlı geliştirme Için tasarlanan bir NoSQL anahtar-değer deposudur.

[Azure SQL veritabanı](/azure/sql-database/) , Microsoft SQL Server altyapısını kullanarak bir hizmet olarak tanıdık ilişkisel veritabanı işlevselliği sağlar.

[Cosmos DB](/azure/cosmos-db/) genel olarak dağıtılmış, çok modelli NoSQL veritabanı hizmeti. Birden çok API'ları (eski adıyla DocumentDB olarak adlandırılır) SQL API'si, Cassandra ve MongoDB gibi kullanılabilir.

## <a name="identity"></a>Kimlik

[Azure Active Directory](/azure/active-directory/) ve [Azure Active Directory B2C](/azure/active-directory-b2c/) her ikisi de kimlik hizmetlerdir. Azure Active Directory kuruluş senaryoları için tasarlanmıştır ve Azure AD B2B (işletmeler arası) işbirliği, sosyal ağ oturum açma dahil olmak üzere, amaçlanan iş müşteri senaryoları olsa da Azure Active Directory B2C'yi etkinleştirir.

## <a name="mobile"></a>Cep telefonu

[Notification Hubs](/azure/notification-hubs/) , çeşitli türlerde cihazlarda çalışan uygulamalara Milyonlarca iletiyi hızlı bir şekilde göndermek için çok platformlu, ölçeklenebilir bir anında iletme bildirimi altyapısıdır.

## <a name="web-infrastructure"></a>Web altyapısı

[Azure Container Service](/azure/aks/) barındırılan Kubernetes ortamınızı yöneterek kapsayıcı düzenleme uzmanlığına gerek kalmadan Kapsayıcılı uygulamaları dağıtmayı ve yönetmeyi hızlı ve kolay hale getirir.

[Azure Search](/azure/search/) , özel, heterojen içeriği üzerinden kurumsal arama çözümü oluşturmak için kullanılır.

[Service Fabric](/azure/service-fabric/) , ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştıran bir dağıtılmış sistemler platformudur.
