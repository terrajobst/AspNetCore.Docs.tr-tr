---
title: "Kaldırmayı yükü, anahtarları iptal edildi"
author: rick-anderson
description: "Bu belgede beri bir ASP.NET Core uygulamada iptal edilmiş anahtarlarla korunan verilerin korumasını açıklanmaktadır."
keywords: "ASP.NET Core, veri koruma, anahtarları, IPersistedDataProtector iptal"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 5d431f0bbe7152525c9a360a6e90bccbd26be93d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a><span data-ttu-id="2beec-104">Kaldırmayı yükü, anahtarları iptal edildi</span><span class="sxs-lookup"><span data-stu-id="2beec-104">Unprotecting payloads whose keys have been revoked</span></span>

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

<span data-ttu-id="2beec-105">ASP.NET Core veri koruma API değil öncelikle yöneliktir gizli yüklerini belirsiz kalıcılığını.</span><span class="sxs-lookup"><span data-stu-id="2beec-105">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="2beec-106">Diğer teknolojiler ister [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) ve [Azure Rights Management](https://docs.microsoft.com/rights-management/) belirsiz depolama senaryosu için daha uygundur ve buna bağlı olarak güçlü anahtar yönetim olanaklarına sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="2beec-106">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](https://docs.microsoft.com/rights-management/) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="2beec-107">Bu, gizli verilerin uzun dönem koruma için ASP.NET Core veri koruma API'ları kullanarak bir geliştirici yasaklanması hiçbir şey belirtti.</span><span class="sxs-lookup"><span data-stu-id="2beec-107">That said, there is nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="2beec-108">Anahtarları hiçbir zaman kaldırılır anahtar halka dışında bu nedenle `IDataProtector.Unprotect` anahtarları kullanılabilir ve geçerli olduğu sürece her zaman varolan yüklerini kurtarabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2beec-108">Keys are never removed from the key ring, so `IDataProtector.Unprotect` can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="2beec-109">Ancak, geliştirici olarak iptal edilen bir anahtarla korunan verileri korumanın çalıştığında bir sorun ortaya çıkar `IDataProtector.Unprotect` bu durumda bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2beec-109">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as `IDataProtector.Unprotect` will throw an exception in this case.</span></span> <span data-ttu-id="2beec-110">Bu tür yüklerini sistem tarafından kolaylıkla yeniden oluşturulabilir ve en kötü olasılıkla ziyaretçi yeniden oturum açmak için gerekli olabilir, bu bağlantı (gibi kimlik doğrulama belirteçleri), kısa süreli veya geçici yükü için daha iyi olabilir.</span><span class="sxs-lookup"><span data-stu-id="2beec-110">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="2beec-111">Ancak sahip kalıcı yüklerini `Unprotect` throw kabul edilebilir veri kaybına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2beec-111">But for persisted payloads, having `Unprotect` throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="2beec-112">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="2beec-112">IPersistedDataProtector</span></span>

<span data-ttu-id="2beec-113">Bile iptal edilen anahtarları karşısında korumasız olacak şekilde yüklerini vermeyle senaryoyu desteklemek için veri koruma sistemini içeren bir `IPersistedDataProtector` türü.</span><span class="sxs-lookup"><span data-stu-id="2beec-113">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an `IPersistedDataProtector` type.</span></span> <span data-ttu-id="2beec-114">Bir örneğini almak `IPersistedDataProtector`, yalnızca bir örneğini almak `IDataProtector` deneyin atama ve normal şekilde `IDataProtector` için `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="2beec-114">To get an instance of `IPersistedDataProtector`, simply get an instance of `IDataProtector` in the normal fashion and try casting the `IDataProtector` to `IPersistedDataProtector`.</span></span>

> [!NOTE]
> <span data-ttu-id="2beec-115">Tüm `IDataProtector` örnekleri dönüştürülür için `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="2beec-115">Not all `IDataProtector` instances can be cast to `IPersistedDataProtector`.</span></span> <span data-ttu-id="2beec-116">Geliştiriciler C# operatör olarak kullanması gereken veya hata durumunu uygun şekilde işlemek üzere kullanılabilir olmalı ve çalışma zamanı özel durumları önlemek için tarafından geçersiz atamaları neden benzer hazır.</span><span class="sxs-lookup"><span data-stu-id="2beec-116">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="2beec-117">`IPersistedDataProtector`Aşağıdaki API yüzeyi sunar:</span><span class="sxs-lookup"><span data-stu-id="2beec-117">`IPersistedDataProtector` exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

<span data-ttu-id="2beec-118">Bu API korumalı Yükü (olarak bir bayt dizisi) alır ve korumasız yükü döndürür.</span><span class="sxs-lookup"><span data-stu-id="2beec-118">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="2beec-119">Dize tabanlı hiçbir aşırı yüklemesi yoktur.</span><span class="sxs-lookup"><span data-stu-id="2beec-119">There is no string-based overload.</span></span> <span data-ttu-id="2beec-120">Out Parametreleri iki aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="2beec-120">The two out parameters are as follows.</span></span>

* <span data-ttu-id="2beec-121">`requiresMigration`: Bu yükü korumak için kullanılan anahtar artık etkin olan varsayılan anahtardır, örn., bu yük korumak için kullanılan anahtar eski ise ve bu yana işlemi çalışırken bir anahtara sahip true olarak ayarlanırsa, gerçekleştirilen.</span><span class="sxs-lookup"><span data-stu-id="2beec-121">`requiresMigration`: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="2beec-122">Çağıran iş gereksinimlerine bağlı olarak yük bunun dikkate alınması gereken isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="2beec-122">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="2beec-123">`wasRevoked`: Bu yükü korumak için kullanılan anahtarı iptal edildi sahipse true olarak ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2beec-123">`wasRevoked`: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="2beec-124">Geçirilirken son derece dikkatli olun çalışma `ignoreRevocationErrors: true` için `DangerousUnprotect` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2beec-124">Exercise extreme caution when passing `ignoreRevocationErrors: true` to the `DangerousUnprotect` method.</span></span> <span data-ttu-id="2beec-125">Bu yöntemi çağrıldıktan sonra IF `wasRevoked` değer true, sonra bu yükü korumak için kullanılan anahtarı iptal edildi ve yükü 's Orijinallik şüpheli olarak değerlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="2beec-125">If after calling this method the `wasRevoked` value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="2beec-126">Bu durumda, yalnızca bazı ayrı güvence varsa korumasız yükü üzerinde çalışmaya devam özgün, örn., BT güvenilmeyen web istemcisi tarafından gönderilen yerine güvenli bir veritabanında'ten gelen.</span><span class="sxs-lookup"><span data-stu-id="2beec-126">In this case, only continue operating on the unprotected payload if you have some separate assurance that it is authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

[!code-csharp[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
