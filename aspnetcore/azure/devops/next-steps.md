---
title: ASP.NET Core ve Azure ile DevOps | Sonraki adımlar
author: CamSoper
description: Azure'da barındırılan bir ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturmaya uçtan uca yönergeler sağlar. bir kılavuz.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/next-steps
ms.openlocfilehash: c1b0b52295e2795e608399637dc9d864e2f572de
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722734"
---
# <a name="next-steps"></a><span data-ttu-id="6d2e6-103">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="6d2e6-103">Next steps</span></span>

<span data-ttu-id="6d2e6-104">Bu kılavuzda, örnek ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="6d2e6-105">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="6d2e6-105">Congratulations!</span></span> <span data-ttu-id="6d2e6-106">ASP.NET Core web uygulamaları, Azure App Service'e yayımlama ve değişiklikler için sürekli tümleştirme otomatikleştirmek öğrenme keyif umuyoruz.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="6d2e6-107">Web barındırma ve DevOps, Azure platformu-olarak-hizmet (PaaS) Hizmetleri ASP.NET Core geliştiricileri için kullanışlı bir çeşit vardır.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="6d2e6-108">Bu bölümde bazı sık kullanılan hizmetler kısa bir genel bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="6d2e6-109">Depolama ve veritabanları</span><span class="sxs-lookup"><span data-stu-id="6d2e6-109">Storage and databases</span></span>

<span data-ttu-id="6d2e6-110">[Redis cache](https://docs.microsoft.com/azure/redis-cache/) yüksek performanslı, düşük gecikme süreli veri önbelleğe alma bir hizmet olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-110">[Redis Cache](https://docs.microsoft.com/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="6d2e6-111">Sayfa çıktısını önbelleğe alma, veritabanı istekleri azaltma ve birden çok uygulama örneği arasında ASP.NET oturum durumu sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-111">It can be used for caching page output, reducing database requests, and providing ASP.NET session state across multiple instances of an app.</span></span>

<span data-ttu-id="6d2e6-112">[Azure depolama](https://docs.microsoft.com/azure/storage/) Azure'nın yüksek düzeyde ölçeklenebilir bulut depolamadır.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-112">[Azure Storage](https://docs.microsoft.com/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="6d2e6-113">Geliştiriciler yararlanabilir [kuyruk depolama](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) güvenilir message queuing için ve [tablo depolama](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) olan, muazzam, yarı yapılandırılmış veri kümeleri aracılığıyla hızlı geliştirmeye yönelik olarak tasarlanan NoSQL anahtar-değer deposu.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-113">Developers can take advantage of [Queue Storage](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="6d2e6-114">[Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/) Microsoft SQL Server altyapısını kullanan bir hizmet olarak bilinen ilişkisel veritabanı işlevlerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-114">[Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="6d2e6-115">[Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) Global olarak dağıtılmış, çok modelli bir NoSQL veritabanı hizmeti.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-115">[Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="6d2e6-116">Birden çok API'ları (eski adıyla DocumentDB olarak adlandırılır) SQL API'si, Cassandra ve MongoDB gibi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="6d2e6-117">Kimlik</span><span class="sxs-lookup"><span data-stu-id="6d2e6-117">Identity</span></span>

<span data-ttu-id="6d2e6-118">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) ve [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) her iki kimlik hizmetleridir.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-118">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) and [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="6d2e6-119">Azure Active Directory kuruluş senaryoları için tasarlanmıştır ve Azure AD B2B (işletmeler arası) işbirliği, sosyal ağ oturum açma dahil olmak üzere, amaçlanan iş müşteri senaryoları olsa da Azure Active Directory B2C'yi etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="6d2e6-120">Mobil</span><span class="sxs-lookup"><span data-stu-id="6d2e6-120">Mobile</span></span>

<span data-ttu-id="6d2e6-121">[Bildirim hub'ları](https://docs.microsoft.com/azure/notification-hubs/) hızlıca çeşitli cihaz türleri üzerinde çalışan uygulamalara milyonlarca mesaj göndermek için çok platformlu, ölçeklenebilir anında iletme bildirimi altyapısı.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-121">[Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="6d2e6-122">Web altyapısı</span><span class="sxs-lookup"><span data-stu-id="6d2e6-122">Web infrastructure</span></span>

<span data-ttu-id="6d2e6-123">[Azure Container Service](https://docs.microsoft.com/azure/aks/) hızla ve kolayca kapsayıcı düzenleme uzmanlığı gerektirmeden kapsayıcıya alınmış uygulamaları dağıtma ve yönetme, barındırılan Kubernetes ortamınızı yönetir.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-123">[Azure Container Service](https://docs.microsoft.com/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="6d2e6-124">[Azure Search'ü](https://docs.microsoft.com/azure/search/) özel ve heterojen içerikler üzerinde kurumsal arama çözümü oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-124">[Azure Search](https://docs.microsoft.com/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="6d2e6-125">[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) kolaylaştıran paketlemek, dağıtmak ve yönetmek ölçeklenebilir bir dağıtılmış sistemler platformudur ve güvenilir mikro Hizmetleri ve kapsayıcıları.</span><span class="sxs-lookup"><span data-stu-id="6d2e6-125">[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
