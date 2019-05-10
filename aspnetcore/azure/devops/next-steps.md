---
title: Sonraki adımlar - ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: Ek ASP.NET Core ve Azure ile DevOps için öğrenme yolları.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: a775dc42551a43bcce72b5f9ca364ed00b1dc4e6
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898622"
---
# <a name="next-steps"></a><span data-ttu-id="086f8-103">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="086f8-103">Next steps</span></span>

<span data-ttu-id="086f8-104">Bu kılavuzda, örnek ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="086f8-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="086f8-105">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="086f8-105">Congratulations!</span></span> <span data-ttu-id="086f8-106">ASP.NET Core web uygulamaları, Azure App Service'e yayımlama ve değişiklikler için sürekli tümleştirme otomatikleştirmek öğrenme keyif umuyoruz.</span><span class="sxs-lookup"><span data-stu-id="086f8-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="086f8-107">Web barındırma ve DevOps, Azure platformu-olarak-hizmet (PaaS) Hizmetleri ASP.NET Core geliştiricileri için kullanışlı bir çeşit vardır.</span><span class="sxs-lookup"><span data-stu-id="086f8-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="086f8-108">Bu bölümde bazı sık kullanılan hizmetler kısa bir genel bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="086f8-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="086f8-109">Depolama ve veritabanları</span><span class="sxs-lookup"><span data-stu-id="086f8-109">Storage and databases</span></span>

<span data-ttu-id="086f8-110">[Redis cache](/azure/redis-cache/) yüksek performanslı, düşük gecikme süreli veri önbelleğe alma bir hizmet olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="086f8-110">[Redis Cache](/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="086f8-111">Sayfa çıktısını önbelleğe alma, veritabanı istekleri azaltma ve bir uygulama birden çok örneği üzerinde ASP.NET Core oturum durumu sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="086f8-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="086f8-112">[Azure depolama](/azure/storage/) Azure'nın yüksek düzeyde ölçeklenebilir bulut depolamadır.</span><span class="sxs-lookup"><span data-stu-id="086f8-112">[Azure Storage](/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="086f8-113">Geliştiriciler yararlanabilir [kuyruk depolama](/azure/storage/queues/storage-queues-introduction) güvenilir message queuing için ve [tablo depolama](/azure/storage/tables/table-storage-overview) olan, muazzam, yarı yapılandırılmış veri kümeleri aracılığıyla hızlı geliştirmeye yönelik olarak tasarlanan NoSQL anahtar-değer deposu.</span><span class="sxs-lookup"><span data-stu-id="086f8-113">Developers can take advantage of [Queue Storage](/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="086f8-114">[Azure SQL veritabanı](/azure/sql-database/) Microsoft SQL Server altyapısını kullanan bir hizmet olarak bilinen ilişkisel veritabanı işlevlerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="086f8-114">[Azure SQL Database](/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="086f8-115">[Cosmos DB](/azure/cosmos-db/) Global olarak dağıtılmış, çok modelli bir NoSQL veritabanı hizmeti.</span><span class="sxs-lookup"><span data-stu-id="086f8-115">[Cosmos DB](/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="086f8-116">Birden çok API'ları (eski adıyla DocumentDB olarak adlandırılır) SQL API'si, Cassandra ve MongoDB gibi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="086f8-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="086f8-117">Kimlik</span><span class="sxs-lookup"><span data-stu-id="086f8-117">Identity</span></span>

<span data-ttu-id="086f8-118">[Azure Active Directory](/azure/active-directory/) ve [Azure Active Directory B2C](/azure/active-directory-b2c/) her iki kimlik hizmetleridir.</span><span class="sxs-lookup"><span data-stu-id="086f8-118">[Azure Active Directory](/azure/active-directory/) and [Azure Active Directory B2C](/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="086f8-119">Azure Active Directory kuruluş senaryoları için tasarlanmıştır ve Azure AD B2B (işletmeler arası) işbirliği, sosyal ağ oturum açma dahil olmak üzere, amaçlanan iş müşteri senaryoları olsa da Azure Active Directory B2C'yi etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="086f8-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="086f8-120">Mobil</span><span class="sxs-lookup"><span data-stu-id="086f8-120">Mobile</span></span>

<span data-ttu-id="086f8-121">[Bildirim hub'ları](/azure/notification-hubs/) hızlıca çeşitli cihaz türleri üzerinde çalışan uygulamalara milyonlarca mesaj göndermek için çok platformlu, ölçeklenebilir anında iletme bildirimi altyapısı.</span><span class="sxs-lookup"><span data-stu-id="086f8-121">[Notification Hubs](/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="086f8-122">Web altyapısı</span><span class="sxs-lookup"><span data-stu-id="086f8-122">Web infrastructure</span></span>

<span data-ttu-id="086f8-123">[Azure Container Service](/azure/aks/) hızla ve kolayca kapsayıcı düzenleme uzmanlığı gerektirmeden kapsayıcıya alınmış uygulamaları dağıtma ve yönetme, barındırılan Kubernetes ortamınızı yönetir.</span><span class="sxs-lookup"><span data-stu-id="086f8-123">[Azure Container Service](/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="086f8-124">[Azure Search'ü](/azure/search/) özel ve heterojen içerikler üzerinde kurumsal arama çözümü oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="086f8-124">[Azure Search](/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="086f8-125">[Service Fabric](/azure/service-fabric/) kolaylaştıran paketlemek, dağıtmak ve yönetmek ölçeklenebilir bir dağıtılmış sistemler platformudur ve güvenilir mikro Hizmetleri ve kapsayıcıları.</span><span class="sxs-lookup"><span data-stu-id="086f8-125">[Service Fabric](/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
