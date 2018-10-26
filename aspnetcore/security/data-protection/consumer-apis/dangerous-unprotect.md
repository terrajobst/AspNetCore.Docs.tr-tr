---
title: ASP.NET Core anahtarları iptal edilen yüklerin korumasını kaldırma
author: rick-anderson
description: Sonra ASP.NET Core uygulamanızı iptal edilmiş anahtarlara sahip korumalı veri korumasını öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: b93ab0fa650041afdfaf1ed5572cc7e081bba244
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089356"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a><span data-ttu-id="1916c-103">ASP.NET Core anahtarları iptal edilen yüklerin korumasını kaldırma</span><span class="sxs-lookup"><span data-stu-id="1916c-103">Unprotect payloads whose keys have been revoked in ASP.NET Core</span></span>


<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

<span data-ttu-id="1916c-104">ASP.NET Core veri koruma API'lerini öncelikle amaçlanmayan gizli yükü belirsiz kalıcılığını.</span><span class="sxs-lookup"><span data-stu-id="1916c-104">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="1916c-105">Diğer teknolojiler ister [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) ve [Azure Rights Management](/rights-management/) belirsiz depolama senaryosu için daha uygundur ve bunlar gelenlere güçlü anahtar yönetim olanaklarına sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="1916c-105">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](/rights-management/) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="1916c-106">Bu, gizli verilerin uzun dönem koruma için ASP.NET Core veri koruma API'lerini kullanarak bir geliştirici yasaklanması bir şey yoktur söylenir.</span><span class="sxs-lookup"><span data-stu-id="1916c-106">That said, there's nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="1916c-107">Anahtarları asla kaldırılır anahtar halka dışında bu nedenle `IDataProtector.Unprotect` anahtarlar kullanılabilir ve geçerli olduğu sürece her zaman var olan yükü kurtarabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1916c-107">Keys are never removed from the key ring, so `IDataProtector.Unprotect` can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="1916c-108">Ancak, geliştirici olarak iptal edilen bir anahtar ile korunan verilerin korumasını çalıştığında bir sorun ortaya çıkar `IDataProtector.Unprotect` bu durumda bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1916c-108">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as `IDataProtector.Unprotect` will throw an exception in this case.</span></span> <span data-ttu-id="1916c-109">Sistem tarafından bu tür yüklerini kolayca yeniden oluşturulabilir ve en kötü olasılıkla ziyaretçi yeniden oturum açmak için gerekli olabilir, bu bağlantı için (örneğin, kimlik doğrulama belirteçlerinizi), kısa süreli veya geçici yükü olabilir.</span><span class="sxs-lookup"><span data-stu-id="1916c-109">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="1916c-110">Ancak sahip kalıcı yüklerini `Unprotect` throw kabul edilebilir veri kaybına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1916c-110">But for persisted payloads, having `Unprotect` throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="1916c-111">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="1916c-111">IPersistedDataProtector</span></span>

<span data-ttu-id="1916c-112">İptal edilen anahtarları karşılaşıldığında bile korumasız olacak şekilde yüklerini zorlu senaryoyu desteklemek için veri koruma sisteminde içeren bir `IPersistedDataProtector` türü.</span><span class="sxs-lookup"><span data-stu-id="1916c-112">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an `IPersistedDataProtector` type.</span></span> <span data-ttu-id="1916c-113">Bir kopyasını almak için `IPersistedDataProtector`, yalnızca bir örneğini almak `IDataProtector` normal bir biçimde ve deneyin atama `IDataProtector` için `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="1916c-113">To get an instance of `IPersistedDataProtector`, simply get an instance of `IDataProtector` in the normal fashion and try casting the `IDataProtector` to `IPersistedDataProtector`.</span></span>

> [!NOTE]
> <span data-ttu-id="1916c-114">Tüm `IDataProtector` örnekleri yayımlanabilir `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="1916c-114">Not all `IDataProtector` instances can be cast to `IPersistedDataProtector`.</span></span> <span data-ttu-id="1916c-115">Geliştiricilerin kullanması gereken C# işleci olarak ya da benzeri çalışma zamanı özel durumları engellemek için neden tarafından geçersiz yayınları ve hata durumunda uygun şekilde işlemeye hazırlıklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1916c-115">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="1916c-116">`IPersistedDataProtector` Aşağıdaki API yüzeyi kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="1916c-116">`IPersistedDataProtector` exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

<span data-ttu-id="1916c-117">Bu API, korumalı Yükü (olarak, bir bayt dizisi) alır ve korumasız yükü döndürür.</span><span class="sxs-lookup"><span data-stu-id="1916c-117">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="1916c-118">Hiçbir dize tabanlı aşırı yüklemesi vardır.</span><span class="sxs-lookup"><span data-stu-id="1916c-118">There's no string-based overload.</span></span> <span data-ttu-id="1916c-119">İki out parametreleri aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="1916c-119">The two out parameters are as follows.</span></span>

* <span data-ttu-id="1916c-120">`requiresMigration`: Bu yükü korumak için kullanılan anahtarı artık etkin varsayılan anahtardır, örneğin, bu yükü korumak için kullanılan anahtarı eski ve işlem çalışırken bir anahtar beri varsa true olarak ayarlanırsa, gerçekleştirilen.</span><span class="sxs-lookup"><span data-stu-id="1916c-120">`requiresMigration`: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="1916c-121">Çağıran, yükü işletme gereksinimlerine bağlı olarak bulunmayı dikkate alınması gereken isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="1916c-121">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="1916c-122">`wasRevoked`: Bu yükü korumak için kullanılan anahtarı iptal edildi durumunda true olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1916c-122">`wasRevoked`: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="1916c-123">Dikkatli aşırı geçerken `ignoreRevocationErrors: true` için `DangerousUnprotect` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1916c-123">Exercise extreme caution when passing `ignoreRevocationErrors: true` to the `DangerousUnprotect` method.</span></span> <span data-ttu-id="1916c-124">Bu yöntemi çağrıldıktan sonra eğer `wasRevoked` değeri true ise daha sonra bu yükü korumak için kullanılan anahtarı iptal edildi ve yükü'nın kimlik doğrulaması şüpheli olarak değerlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="1916c-124">If after calling this method the `wasRevoked` value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="1916c-125">Bu durumda, yalnızca korumasız yükünü, örneğin gerçek, olduğundan bazı ayrı güvencesi varsa çalışan bir güvenilmeyen web istemcisi tarafından gönderilen yerine güvenli bir veritabanında geldiğinden emin devam edin.</span><span class="sxs-lookup"><span data-stu-id="1916c-125">In this case, only continue operating on the unprotected payload if you have some separate assurance that it's authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
