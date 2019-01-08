---
title: ASP.NET Core güvenliğine genel bakış
author: tdykstra
description: ASP.NET Core kimlik doğrulaması, yetkilendirme ve güvenlik temel bilgileri öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: security/index
ms.openlocfilehash: 933501411169d89c4b24edda743c47591aa7a87a
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098967"
---
# <a name="overview-of-aspnet-core-security"></a><span data-ttu-id="c0b42-103">ASP.NET Core güvenliğine genel bakış</span><span class="sxs-lookup"><span data-stu-id="c0b42-103">Overview of ASP.NET Core Security</span></span>

<span data-ttu-id="c0b42-104">ASP.NET Core geliştiricilerinin kolayca yapılandırmak ve uygulamalarını için güvenliği yönetmek etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="c0b42-104">ASP.NET Core enables developers to easily configure and manage security for their apps.</span></span> <span data-ttu-id="c0b42-105">ASP.NET Core kimlik doğrulaması, yetkilendirme, veri koruma, HTTPS zorlama, uygulama gizli anahtarlarının, istek sahteciliğinden koruma koruması ve CORS yönetim yönetmeye yönelik özellikler içerir.</span><span class="sxs-lookup"><span data-stu-id="c0b42-105">ASP.NET Core contains features for managing authentication, authorization, data protection, HTTPS enforcement, app secrets, anti-request forgery protection, and CORS management.</span></span> <span data-ttu-id="c0b42-106">Bu güvenlik özellikleri, güçlü derleme henüz ASP.NET Core uygulamalarının güvenliğini sağlama imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="c0b42-106">These security features allow you to build robust yet secure ASP.NET Core apps.</span></span>

## <a name="aspnet-core-security-features"></a><span data-ttu-id="c0b42-107">ASP.NET Core güvenlik özellikleri</span><span class="sxs-lookup"><span data-stu-id="c0b42-107">ASP.NET Core security features</span></span>

<span data-ttu-id="c0b42-108">Birçok araca ve kitaplığa yerleşik kimlik sağlayıcıları dahil olmak üzere uygulamalarınızın güvenliğini sağlamak için ASP.NET Core sağlar ancak Facebook, Twitter ve LinkedIn gibi 3. taraf kimlik Hizmetleri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0b42-108">ASP.NET Core provides many tools and libraries to secure your apps including built-in Identity providers but you can use 3rd party identity services such as Facebook, Twitter, or LinkedIn.</span></span> <span data-ttu-id="c0b42-109">ASP.NET Core ile bir şekilde depolayın ve kod göstermek zorunda kalmadan gizli bilgiler kullanın olan, uygulama gizli anahtarlarının kolayca yönetebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0b42-109">With ASP.NET Core, you can easily manage app secrets, which are a way to store and use confidential information without having to expose it in the code.</span></span>

## <a name="authentication-vs-authorization"></a><span data-ttu-id="c0b42-110">Kimlik doğrulaması vs. Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="c0b42-110">Authentication vs. Authorization</span></span>

<span data-ttu-id="c0b42-111">Kimlik doğrulaması, bir kullanıcı bu bir işletim sistemi, veritabanı, uygulama veya kaynak depolanan karşılaştırılır kimlik bilgilerini sağlayan bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="c0b42-111">Authentication is a process in which a user provides credentials that are then compared to those stored in an operating system, database, app or resource.</span></span> <span data-ttu-id="c0b42-112">Eşleşirlerse, kullanıcıların kimliğini başarıyla doğrulamak ve, sahip olduğunuz eylemler gerçekleştirebilir bir yetkilendirme sürecinde.</span><span class="sxs-lookup"><span data-stu-id="c0b42-112">If they match, users authenticate successfully, and can then perform actions that they're authorized for, during an authorization process.</span></span> <span data-ttu-id="c0b42-113">Yetkilendirme, bir kullanıcı yapmak için izin verilenden belirleyen işlemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="c0b42-113">The authorization refers to the process that determines what a user is allowed to do.</span></span>

<span data-ttu-id="c0b42-114">Kimlik doğrulaması düşünmek için başka bir yetkilendirme (sunucu, veritabanı veya uygulama) bu alanı içinde hangi nesneleri için kullanıcının gerçekleştirebileceği eylemleri olsa da sunucu, veritabanı, uygulama veya kaynağa gibi bir alan girin. bir yolu olarak dikkate alınması gereken yoludur.</span><span class="sxs-lookup"><span data-stu-id="c0b42-114">Another way to think of authentication is to consider it as a way to enter a space, such as a server, database, app or resource, while authorization is which actions the user can perform to which objects inside that space (server, database, or app).</span></span>

## <a name="common-vulnerabilities-in-software"></a><span data-ttu-id="c0b42-115">Ortak yazılımdaki</span><span class="sxs-lookup"><span data-stu-id="c0b42-115">Common Vulnerabilities in software</span></span>

<span data-ttu-id="c0b42-116">ASP.NET Core ve EF yardımcı olan özellikler içeriyor uygulamanızı güvenli hale getirme ve güvenlik ihlallerini önleyin.</span><span class="sxs-lookup"><span data-stu-id="c0b42-116">ASP.NET Core and EF contain features that help you secure your apps and prevent security breaches.</span></span> <span data-ttu-id="c0b42-117">Aşağıdaki listede yer alan bağlantıları, web uygulamalarında en yaygın güvenlik açıklarından önlemek için yapılan teknikleri belgeleri alır:</span><span class="sxs-lookup"><span data-stu-id="c0b42-117">The following list of links takes you to documentation detailing techniques to avoid the most common security vulnerabilities in web apps:</span></span>

* [<span data-ttu-id="c0b42-118">Siteler arası betik saldırıları</span><span class="sxs-lookup"><span data-stu-id="c0b42-118">Cross-site scripting attacks</span></span>](xref:security/cross-site-scripting)
* [<span data-ttu-id="c0b42-119">SQL ekleme saldırıları</span><span class="sxs-lookup"><span data-stu-id="c0b42-119">SQL injection attacks</span></span>](/ef/core/querying/raw-sql)
* [<span data-ttu-id="c0b42-120">Siteler arası istek sahteciliği (CSRF)</span><span class="sxs-lookup"><span data-stu-id="c0b42-120">Cross-Site Request Forgery (CSRF)</span></span>](xref:security/anti-request-forgery)
* [<span data-ttu-id="c0b42-121">Açık yeniden yönlendirme saldırılarını</span><span class="sxs-lookup"><span data-stu-id="c0b42-121">Open redirect attacks</span></span>](xref:security/preventing-open-redirects)

<span data-ttu-id="c0b42-122">Bilmeniz gereken daha fazla güvenlik açıkları vardır.</span><span class="sxs-lookup"><span data-stu-id="c0b42-122">There are more vulnerabilities that you should be aware of.</span></span> <span data-ttu-id="c0b42-123">Daha fazla bilgi için bkz: diğer makaleleri **güvenlik ve kimlik** İçindekiler bölümünde.</span><span class="sxs-lookup"><span data-stu-id="c0b42-123">For more information, see the other articles in the **Security and Identity** section of the table of contents.</span></span>
