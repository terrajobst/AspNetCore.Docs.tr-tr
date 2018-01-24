---
title: "ASP.NET Core amacı dizeleri"
author: rick-anderson
description: "ASP.NET Core veri koruma API'ları ilgili olarak bu belgenin amacı dize hiyerarşi ve çoklu kiracı özetlenmektedir."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: e720c3e245f4a20ffeb728035ee3ad3c1320ab92
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a><span data-ttu-id="1a151-103">Amaç hiyerarşi ve ASP.NET Core, çok kiracılı</span><span class="sxs-lookup"><span data-stu-id="1a151-103">Purpose hierarchy and multi-tenancy in ASP.NET Core</span></span>

<span data-ttu-id="1a151-104">Bu yana bir `IDataProtector` de örtülü olarak başvuruluyor bir `IDataProtectionProvider`, amacıyla zincirleme yapılabilir birlikte.</span><span class="sxs-lookup"><span data-stu-id="1a151-104">Since an `IDataProtector` is also implicitly an `IDataProtectionProvider`, purposes can be chained together.</span></span> <span data-ttu-id="1a151-105">Bu bağlamdaki `provider.CreateProtector([ "purpose1", "purpose2" ])` eşdeğerdir `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.</span><span class="sxs-lookup"><span data-stu-id="1a151-105">In this sense, `provider.CreateProtector([ "purpose1", "purpose2" ])` is equivalent to `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.</span></span>

<span data-ttu-id="1a151-106">Bu veri koruma sistemi aracılığıyla ilginç bazı hiyerarşik ilişkiler sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a151-106">This allows for some interesting hierarchical relationships through the data protection system.</span></span> <span data-ttu-id="1a151-107">Önceki örnekte [Contoso.Messaging.SecureMessage](purpose-strings.md#data-protection-contoso-purpose), SecureMessage bileşeni çağırabilirsiniz `provider.CreateProtector("Contoso.Messaging.SecureMessage")` kez eylemli ve sonucu bir özel önbelleğe `_myProvide` alan.</span><span class="sxs-lookup"><span data-stu-id="1a151-107">In the earlier example of [Contoso.Messaging.SecureMessage](purpose-strings.md#data-protection-contoso-purpose), the SecureMessage component can call `provider.CreateProtector("Contoso.Messaging.SecureMessage")` once up-front and cache the result into a private `_myProvide` field.</span></span> <span data-ttu-id="1a151-108">Gelecekteki koruyucuları sonra oluşturulabilir çağrıları aracılığıyla `_myProvider.CreateProtector("User: username")`, ve bu koruyucuları tek bir ileti güvenliğini sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1a151-108">Future protectors can then be created via calls to `_myProvider.CreateProtector("User: username")`, and these protectors would be used for securing the individual messages.</span></span>

<span data-ttu-id="1a151-109">Bu ayrıca çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="1a151-109">This can also be flipped.</span></span> <span data-ttu-id="1a151-110">Tek bir mantıksal uygulama (bir CMS makul görünüyor) birden çok kiracıya ve her bir kiracı kendi kimlik doğrulama ve durum yönetimi sistemi ile yapılandırılabilir hangi ana bilgisayarların göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="1a151-110">Consider a single logical application which hosts multiple tenants (a CMS seems reasonable), and each tenant can be configured with its own authentication and state management system.</span></span> <span data-ttu-id="1a151-111">Tek bir ana sağlayıcısı şemsiyesi uygulaması yüklüyse ve çağırır `provider.CreateProtector("Tenant 1")` ve `provider.CreateProtector("Tenant 2")` her bir kiracı kendi yalıtılmış dilimin veri koruma sisteminde vermek için.</span><span class="sxs-lookup"><span data-stu-id="1a151-111">The umbrella application has a single master provider, and it calls `provider.CreateProtector("Tenant 1")` and `provider.CreateProtector("Tenant 2")` to give each tenant its own isolated slice of the data protection system.</span></span> <span data-ttu-id="1a151-112">Kiracılar kendi gereksinimlerine göre kendi bireysel koruyucuları sonra türetilen, ancak nasıl sabit bunlar deneyin olsun bunlar birbiriyle çakışır koruyucuları başka hiçbir kiracıyla sistemde oluşturamazsınız.</span><span class="sxs-lookup"><span data-stu-id="1a151-112">The tenants could then derive their own individual protectors based on their own needs, but no matter how hard they try they cannot create protectors which collide with any other tenant in the system.</span></span> <span data-ttu-id="1a151-113">Grafik, bu olarak aşağıda gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1a151-113">Graphically, this is represented as below.</span></span>

![Çoklu kiracı amaçları](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> <span data-ttu-id="1a151-115">Bu uygulama denetimleri hangi API'leri tek tek kiracıların kullanımına sunulur ve kiracılar sunucuda rastgele kod yürütülemiyor şemsiyesi varsayar.</span><span class="sxs-lookup"><span data-stu-id="1a151-115">This assumes the umbrella application controls which APIs are available to individual tenants and that tenants cannot execute arbitrary code on the server.</span></span> <span data-ttu-id="1a151-116">Bir kiracı rastgele kod yürütebilir, yalıtım garanti ayırmak için özel yansıma gerçekleştirebilir veya bunlar yalnızca ana anahtar malzemesini doğrudan okunabilir ve her alt anahtarlar türetilen, istenen.</span><span class="sxs-lookup"><span data-stu-id="1a151-116">If a tenant can execute arbitrary code, they could perform private reflection to break the isolation guarantees, or they could just read the master keying material directly and derive whatever subkeys they desire.</span></span>

<span data-ttu-id="1a151-117">Veri koruma sisteminde gerçekte varsayılan Giden kutusu yapılandırmasıyla çok kiracılı bir çeşit kullanır.</span><span class="sxs-lookup"><span data-stu-id="1a151-117">The data protection system actually uses a sort of multi-tenancy in its default out-of-the-box configuration.</span></span> <span data-ttu-id="1a151-118">Varsayılan olarak ana anahtar malzemesini çalışan işlem hesabının kullanıcı profili klasörü (veya IIS uygulama havuzu kimlikleri için kayıt defteri) depolanır.</span><span class="sxs-lookup"><span data-stu-id="1a151-118">By default master keying material is stored in the worker process account's user profile folder (or the registry, for IIS application pool identities).</span></span> <span data-ttu-id="1a151-119">Ancak birden çok uygulamaları çalıştırmak için tek bir hesap kullanmak için gerçekten oldukça yaygındır ve böylece bu uygulamaları malzemesinin asıl paylaşımı sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="1a151-119">But it is actually fairly common to use a single account to run multiple applications, and thus all these applications would end up sharing the master keying material.</span></span> <span data-ttu-id="1a151-120">Bunu çözmek için veri koruma sistemi otomatik olarak başına uygulama benzersiz tanımlayıcısı olarak genel amaçlı zinciri ilk öğe ekler.</span><span class="sxs-lookup"><span data-stu-id="1a151-120">To solve this, the data protection system automatically inserts a unique-per-application identifier as the first element in the overall purpose chain.</span></span> <span data-ttu-id="1a151-121">Örtük bu amaca hizmet eder [tek tek uygulamalarını yalıtmak](xref:security/data-protection/configuration/overview#per-application-isolation) birbirinden sistem ve koruyucu oluşturma işlemi içinde benzersiz bir kiracı yukarıdaki resimde aynı göründüğü gibi her bir uygulama etkili bir şekilde davranma tarafından.</span><span class="sxs-lookup"><span data-stu-id="1a151-121">This implicit purpose serves to [isolate individual applications](xref:security/data-protection/configuration/overview#per-application-isolation) from one another by effectively treating each application as a unique tenant within the system, and the protector creation process looks identical to the image above.</span></span>