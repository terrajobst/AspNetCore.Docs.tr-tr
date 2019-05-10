---
title: ASP.NET core'da korumalı yüklerin ömrünü sınırlama
author: rick-anderson
description: ASP.NET Core veri koruma API'lerini kullanarak korumalı bir yükü ömrünü sınırlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902912"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a><span data-ttu-id="07eec-103">ASP.NET core'da korumalı yüklerin ömrünü sınırlama</span><span class="sxs-lookup"><span data-stu-id="07eec-103">Limit the lifetime of protected payloads in ASP.NET Core</span></span>

<span data-ttu-id="07eec-104">Burada, belirlenen bir süre sonra süresi dolan bir korumalı yükü oluşturmak için uygulama geliştiricisinin istediği senaryoları vardır.</span><span class="sxs-lookup"><span data-stu-id="07eec-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="07eec-105">Örneğin, korumalı yükü yalnızca bir saat için geçerli olacak bir parola sıfırlama belirteci temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="07eec-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="07eec-106">Bir katıştırılmış sona erme tarihi içeren, kendi yük biçiminde oluşturmak geliştirici için kesinlikle mümkündür ve İleri düzey geliştiriciler bunu yapmak isteyebilirsiniz ancak bu süre sonu yönetme geliştiricilerin çoğu için tedious büyüyebilir.</span><span class="sxs-lookup"><span data-stu-id="07eec-106">It's certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="07eec-107">Geliştirici hedef için paket kolaylaştırmak için [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) otomatik olarak ayarlanmış bir süre sonunda süresi dolacak yükü oluşturmak için yardımcı programı API'lerini içerir.</span><span class="sxs-lookup"><span data-stu-id="07eec-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="07eec-108">Bu API'leri kapatıp askıda `ITimeLimitedDataProtector` türü.</span><span class="sxs-lookup"><span data-stu-id="07eec-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="07eec-109">API kullanımı</span><span class="sxs-lookup"><span data-stu-id="07eec-109">API usage</span></span>

<span data-ttu-id="07eec-110">`ITimeLimitedDataProtector` Koruması kaldırıldıktan süre sınırlı / şirket içinde süresi dolan yükler ve koruma için çekirdek arabirimi arabirimidir.</span><span class="sxs-lookup"><span data-stu-id="07eec-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="07eec-111">Bir örneğini oluşturmak için bir `ITimeLimitedDataProtector`, önce normal örneği gerekir [Idataprotector](xref:security/data-protection/consumer-apis/overview) belirli bir amaç ile oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="07eec-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](xref:security/data-protection/consumer-apis/overview) constructed with a specific purpose.</span></span> <span data-ttu-id="07eec-112">Bir kez `IDataProtector` örneği kullanılabilir, çağrı `IDataProtector.ToTimeLimitedDataProtector` bir koruyucu yerleşik bir sona erme özellikleriyle geri almak için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="07eec-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="07eec-113">`ITimeLimitedDataProtector` Aşağıdaki API yüzeyi ve uzantı yöntemleri sunar:</span><span class="sxs-lookup"><span data-stu-id="07eec-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="07eec-114">CreateProtector (dize amaçlı): ITimeLimitedDataProtector - bu API, varolan benzerdir `IDataProtectionProvider.CreateProtector` oluşturmak için kullanılabilir olduğunu [amaç zincirleri](xref:security/data-protection/consumer-apis/purpose-strings) kök süre sınırlı koruyucu öğesinden.</span><span class="sxs-lookup"><span data-stu-id="07eec-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](xref:security/data-protection/consumer-apis/purpose-strings) from a root time-limited protector.</span></span>

* <span data-ttu-id="07eec-115">(Bayt [] düz metin, DateTimeOffset sona erme) koruma: byte]</span><span class="sxs-lookup"><span data-stu-id="07eec-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="07eec-116">Koruma (bayt [] düz metin, TimeSpan yaşam süresi): byte]</span><span class="sxs-lookup"><span data-stu-id="07eec-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="07eec-117">(Bayt [] düz) korumak: byte]</span><span class="sxs-lookup"><span data-stu-id="07eec-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="07eec-118">(Düz dize, DateTimeOffset sona erme) koruma: dize</span><span class="sxs-lookup"><span data-stu-id="07eec-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="07eec-119">Koruma (dizesi düz metin, TimeSpan yaşam süresi): dize</span><span class="sxs-lookup"><span data-stu-id="07eec-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="07eec-120">(String düz) korumak: dize</span><span class="sxs-lookup"><span data-stu-id="07eec-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="07eec-121">Çekirdek yanı sıra `Protect` yalnızca düz metin, alan yöntemleri yükü'nın son kullanma tarihini belirterek izin veren yeni aşırı yüklemeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="07eec-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="07eec-122">Süre sonu mutlak bir tarih belirtilebilir (aracılığıyla bir `DateTimeOffset`) veya göreli zaman olarak (geçerli sisteminden zaman aracılığıyla bir `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="07eec-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="07eec-123">Bir süre sonu verdiği hızlı tepkilerden faydalanamamış aşırı çağrılırsa, yükü süresiz olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="07eec-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="07eec-124">(Bayt [] protectedData, DateTimeOffset sona erme kullanıma) korumasını: byte]</span><span class="sxs-lookup"><span data-stu-id="07eec-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="07eec-125">(Bayt [] protectedData) korumasını: byte]</span><span class="sxs-lookup"><span data-stu-id="07eec-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="07eec-126">(DateTimeOffset sona erme çıkış dizesi protectedData) korumasını: dize</span><span class="sxs-lookup"><span data-stu-id="07eec-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="07eec-127">(String protectedData) korumasını: dize</span><span class="sxs-lookup"><span data-stu-id="07eec-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="07eec-128">`Unprotect` Yöntemleri özgün korumasız veri döndürür.</span><span class="sxs-lookup"><span data-stu-id="07eec-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="07eec-129">Yükü henüz süresi dolmadığından, mutlak out parametresi özgün korumasız verileriyle birlikte isteğe bağlı olarak döndürülür.</span><span class="sxs-lookup"><span data-stu-id="07eec-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="07eec-130">Yük süresi dolmuşsa, Unprotect yöntemin tüm aşırı yüklemeler CryptographicException durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="07eec-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="07eec-131">Bu uzun vadeli veya belirsiz Kalıcılık gerektiren yüklerini korumak için bu API'leri kullanmak için önerilir değil.</span><span class="sxs-lookup"><span data-stu-id="07eec-131">It's not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="07eec-132">"I için bir ay sonra kalıcı olarak kurtarılamaz olarak korumalı yüklerin destekleyebilir?"</span><span class="sxs-lookup"><span data-stu-id="07eec-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="07eec-133">bir iyi kural karşısında hizmet verebilen; yanıt yok sonra geliştiriciler alternatif API'leri düşünmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="07eec-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="07eec-134">Örnek kullanımlar aşağıda [DI olmayan kod yollarını](xref:security/data-protection/configuration/non-di-scenarios) veri koruma sisteminde örnekleme için.</span><span class="sxs-lookup"><span data-stu-id="07eec-134">The sample below uses the [non-DI code paths](xref:security/data-protection/configuration/non-di-scenarios) for instantiating the data protection system.</span></span> <span data-ttu-id="07eec-135">Bu örneği çalıştırmak için önce Microsoft.AspNetCore.DataProtection.Extensions paketine bir başvuru eklediğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="07eec-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
