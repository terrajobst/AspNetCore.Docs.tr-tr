---
title: "Amaç dizeleri"
author: rick-anderson
description: "Bu belgenin amacı dizeleri ASP.NET Core veri koruma API'ları nasıl kullanıldığını ayrıntıları verilmektedir."
keywords: "ASP.NET Core, veri koruma, amacı dizeleri"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c96ed361-c382-4980-8933-800e740cfc38
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 0d759937703d2a25604042b5e74e71155d635c1b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="purpose-strings"></a><span data-ttu-id="96dee-104">Amaç dizeleri</span><span class="sxs-lookup"><span data-stu-id="96dee-104">Purpose Strings</span></span>

<a name="data-protection-consumer-apis-purposes"></a>

<span data-ttu-id="96dee-105">Tüketen bileşenleri `IDataProtectionProvider` benzersiz bir geçmelidir *amacıyla* parametresi `CreateProtector` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="96dee-105">Components which consume `IDataProtectionProvider` must pass a unique *purposes* parameter to the `CreateProtector` method.</span></span> <span data-ttu-id="96dee-106">Amacıyla *parametresi* kök şifreleme anahtarları aynı olsa bile, şifreleme tüketicileri arasında yalıtım sağlar gibi veri koruma sisteminde güvenlik devralınır.</span><span class="sxs-lookup"><span data-stu-id="96dee-106">The purposes *parameter* is inherent to the security of the data protection system, as it provides isolation between cryptographic consumers, even if the root cryptographic keys are the same.</span></span>

<span data-ttu-id="96dee-107">Bir tüketici bir amaç belirttiğinde amacı dize bu tüketiciye şifreleme alt anahtarlarını benzersiz çıkarmaya kök şifreleme anahtarları ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="96dee-107">When a consumer specifies a purpose, the purpose string is used along with the root cryptographic keys to derive cryptographic subkeys unique to that consumer.</span></span> <span data-ttu-id="96dee-108">Bu diğer şifreleme tüketicileri uygulamadaki tüketiciden yalıtır: başka bir bileşeni, yükü okuyabilir ve herhangi diğer bileşenin yüklerini okunamıyor.</span><span class="sxs-lookup"><span data-stu-id="96dee-108">This isolates the consumer from all other cryptographic consumers in the application: no other component can read its payloads, and it cannot read any other component's payloads.</span></span> <span data-ttu-id="96dee-109">Bu yalıtım da bileşen saldırısı uyuşmazlığa tüm kategorileri işler.</span><span class="sxs-lookup"><span data-stu-id="96dee-109">This isolation also renders infeasible entire categories of attack against the component.</span></span>

![Amaç diyagramı örneği](purpose-strings/_static/purposes.png)

<span data-ttu-id="96dee-111">Yukarıdaki diyagramda `IDataProtector` örnekleri A ve B **olamaz** birbirlerinin yükü, yalnızca okuma kendi.</span><span class="sxs-lookup"><span data-stu-id="96dee-111">In the diagram above, `IDataProtector` instances A and B **cannot** read each other's payloads, only their own.</span></span>

<span data-ttu-id="96dee-112">Amaç dize gizli olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="96dee-112">The purpose string doesn't have to be secret.</span></span> <span data-ttu-id="96dee-113">Yalnızca başka bir iyi çalışan bileşen aynı amacı dize erişiminizi sağlayacaktır anlamda benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="96dee-113">It should simply be unique in the sense that no other well-behaved component will ever provide the same purpose string.</span></span>

>[!TIP]
> <span data-ttu-id="96dee-114">Veri Koruma API tüketen bileşen ad alanı ve tür adını kullanarak bir iyi, bu bilgileri hiçbir zaman çakışacak yöntem olduğu gibi udur.</span><span class="sxs-lookup"><span data-stu-id="96dee-114">Using the namespace and type name of the component consuming the data protection APIs is a good rule of thumb, as in practice this information will never conflict.</span></span>
>
><span data-ttu-id="96dee-115">Taşıyıcı belirteçlerini minting için sorumlu olan Contoso yazılan bir bileşen Contoso.Security.BearerToken kendi amacı dize olarak kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="96dee-115">A Contoso-authored component which is responsible for minting bearer tokens might use Contoso.Security.BearerToken as its purpose string.</span></span> <span data-ttu-id="96dee-116">Veya, kendi amacı dize olarak Contoso.Security.BearerToken.v1 - bile daha iyi - kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96dee-116">Or - even better - it might use Contoso.Security.BearerToken.v1 as its purpose string.</span></span> <span data-ttu-id="96dee-117">Sürüm numarası ekleme Contoso.Security.BearerToken.v2 amacı kullanmak gelecekteki bir sürümüne sağlar ve yüklerini Git oldukça farklı sürümlerini birbirinden tamamen yalıtılmış olacaktır.</span><span class="sxs-lookup"><span data-stu-id="96dee-117">Appending the version number allows a future version to use Contoso.Security.BearerToken.v2 as its purpose, and the different versions would be completely isolated from one another as far as payloads go.</span></span>

<span data-ttu-id="96dee-118">Amacıyla parametresi itibaren `CreateProtector` bir dize dizisi yukarıdaki yerine olarak belirtilmiş `[ "Contoso.Security.BearerToken", "v1" ]`.</span><span class="sxs-lookup"><span data-stu-id="96dee-118">Since the purposes parameter to `CreateProtector` is a string array, the above could have been instead specified as `[ "Contoso.Security.BearerToken", "v1" ]`.</span></span> <span data-ttu-id="96dee-119">Bu amaçlar hiyerarşisini kurma verir ve veri koruma sisteminde çoklu kiracı senaryolarıyla olasılığını açar.</span><span class="sxs-lookup"><span data-stu-id="96dee-119">This allows establishing a hierarchy of purposes and opens up the possibility of multi-tenancy scenarios with the data protection system.</span></span>

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> <span data-ttu-id="96dee-120">Bileşenleri tek bir kaynak amacıyla zinciri için girdi olarak güvenilmeyen kullanıcı girişi izin vermemelidir.</span><span class="sxs-lookup"><span data-stu-id="96dee-120">Components should not allow untrusted user input to be the sole source of input for the purposes chain.</span></span>
>
><span data-ttu-id="96dee-121">Örneğin, bir bileşenin güvenli iletiler depolamak için sorumlu Contoso.Messaging.SecureMessage göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="96dee-121">For example, consider a component Contoso.Messaging.SecureMessage which is responsible for storing secure messages.</span></span> <span data-ttu-id="96dee-122">Güvenli ileti sistemi bileşeni çağırmak için olsaydı `CreateProtector([ username ])`, kötü niyetli bir kullanıcının bir hesap kullanıcı adı "Contoso.Security.BearerToken" ile çağırmak için bileşen alma girişimi oluşturup `CreateProtector([ "Contoso.Security.BearerToken" ])`, böylece yanlışlıkla güvenli Mesajlaşma neden kimlik doğrulama belirteçleri algılanan Naneli yüklerini sisteme.</span><span class="sxs-lookup"><span data-stu-id="96dee-122">If the secure messaging component were to call `CreateProtector([ username ])`, then a malicious user might create an account with username "Contoso.Security.BearerToken" in an attempt to get the component to call `CreateProtector([ "Contoso.Security.BearerToken" ])`, thus inadvertently causing the secure messaging system to mint payloads that could be perceived as authentication tokens.</span></span>
>
><span data-ttu-id="96dee-123">İleti sistemi bileşeni için daha iyi bir amacıyla zinciri olacaktır `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, uygun yalıtım sağlar.</span><span class="sxs-lookup"><span data-stu-id="96dee-123">A better purposes chain for the messaging component would be `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, which provides proper isolation.</span></span>

<span data-ttu-id="96dee-124">Tarafından sağlanan yalıtım ve davranışlarını `IDataProtectionProvider`, `IDataProtector`, ve amacıyla aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="96dee-124">The isolation provided by and behaviors of `IDataProtectionProvider`, `IDataProtector`, and purposes are as follows:</span></span>

* <span data-ttu-id="96dee-125">İçin bir verilen `IDataProtectionProvider` nesnesi `CreateProtector` yöntemi oluşturacak bir `IDataProtector` nesneyi benzersiz olarak bağlı hem de `IDataProtectionProvider` ve yöntemine geçildi amacıyla parametresini oluşturulan bir nesne.</span><span class="sxs-lookup"><span data-stu-id="96dee-125">For a given `IDataProtectionProvider` object, the `CreateProtector` method will create an `IDataProtector` object uniquely tied to both the `IDataProtectionProvider` object which created it and the purposes parameter which was passed into the method.</span></span>

* <span data-ttu-id="96dee-126">Amaç parametresi null olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="96dee-126">The purpose parameter must not be null.</span></span> <span data-ttu-id="96dee-127">(Amacıyla bir dizi olarak belirtilirse, bu dizinin sıfır uzunluğunda olmalıdır ve dizinin tüm öğeleri null olmayan olmalıdır anlamına gelir.) Boş dize amacı teknik olarak izin verilir, ancak önerilmez.</span><span class="sxs-lookup"><span data-stu-id="96dee-127">(If purposes is specified as an array, this means that the array must not be of zero length and all elements of the array must be non-null.) An empty string purpose is technically allowed but is discouraged.</span></span>

* <span data-ttu-id="96dee-128">(Sıralı bir karşılaştırıcı kullanarak) aynı dizeleri aynı sırada içerir ve yalnızca, iki amaca bağımsız değişkenleri eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="96dee-128">Two purposes arguments are equivalent if and only if they contain the same strings (using an ordinal comparer) in the same order.</span></span> <span data-ttu-id="96dee-129">Tek amaçlı bağımsız değişkeni, karşılık gelen tek öğe amacıyla diziye eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="96dee-129">A single purpose argument is equivalent to the corresponding single-element purposes array.</span></span>

* <span data-ttu-id="96dee-130">İki `IDataProtector` eşdeğerini oluşturulur ve yalnızca, nesneleri eşdeğer `IDataProtectionProvider` nesneleri eşdeğer amacıyla parametrelere sahip.</span><span class="sxs-lookup"><span data-stu-id="96dee-130">Two `IDataProtector` objects are equivalent if and only if they are created from equivalent `IDataProtectionProvider` objects with equivalent purposes parameters.</span></span>

* <span data-ttu-id="96dee-131">İçin bir verilen `IDataProtector` nesnesi, bir çağrı `Unprotect(protectedData)` özgün döndürülecek `unprotectedData` ve yalnızca, `protectedData := Protect(unprotectedData)` için eşdeğer bir `IDataProtector` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="96dee-131">For a given `IDataProtector` object, a call to `Unprotect(protectedData)` will return the original `unprotectedData` if and only if `protectedData := Protect(unprotectedData)` for an equivalent `IDataProtector` object.</span></span>

> [!NOTE]
> <span data-ttu-id="96dee-132">Biz burada bazı bileşeni ile başka bir bileşen çakışma bilinen bir amaç dize bilerek seçer çalışması düşünüyorsunuz değil.</span><span class="sxs-lookup"><span data-stu-id="96dee-132">We're not considering the case where some component intentionally chooses a purpose string which is known to conflict with another component.</span></span> <span data-ttu-id="96dee-133">Bu tür bir bileşen temelde kötü amaçlı olarak kabul edilir ve bu sistem kötü amaçlı kod içinde çalışan işlemi zaten çalışıyor gerektiğinde, güvenlik garantileri sağlamak üzere tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="96dee-133">Such a component would essentially be considered malicious, and this system is not intended to provide security guarantees in the event that malicious code is already running inside of the worker process.</span></span>
