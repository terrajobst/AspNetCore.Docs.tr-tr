---
title: Anahtar değiştirilemezliği ve ASP.NET Core anahtar ayarları
author: rick-anderson
description: ASP.NET Core veri koruma anahtar değiştirilemezliği API uygulama ayrıntıları öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898766"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="69974-103">Anahtar değiştirilemezliği ve ASP.NET Core anahtar ayarları</span><span class="sxs-lookup"><span data-stu-id="69974-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="69974-104">Yedekleme deposu için bir nesneyi kalıcı sonra gösterimine her zaman sabittir.</span><span class="sxs-lookup"><span data-stu-id="69974-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="69974-105">Yeni veri yedekleme deposu için eklenebilir, ancak hiçbir zaman mevcut verileri dönüşür.</span><span class="sxs-lookup"><span data-stu-id="69974-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="69974-106">Bu davranış birincil amacı, veri bozulması engellemektir.</span><span class="sxs-lookup"><span data-stu-id="69974-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="69974-107">Bu davranış bir sonucu bir anahtarı yedekleme deposuna yazılır, sonra da değişmez olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="69974-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="69974-108">İptal ancak kullanarak oluşturma, etkinleştirme ve sona erme tarihleri hiçbir zaman, değiştirilebilir `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="69974-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="69974-109">Ayrıca, kendi temel alınan algoritmik bilgileri, ana anahtar malzemesini ve diğer özellikleri şifrelemeyi de sabittir.</span><span class="sxs-lookup"><span data-stu-id="69974-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="69974-110">Geliştirici anahtar kalıcılığı etkileyen herhangi bir ayar değişirse, bu değişiklikleri bir anahtar oluşturulur, zamana kadar açık bir çağrı yoluyla yürürlüğe olmaz `IKeyManager.CreateNewKey` veya veri koruma sisteminin kendi aracılığıyla [otomatik anahtar nesil](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) davranışı.</span><span class="sxs-lookup"><span data-stu-id="69974-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="69974-111">Anahtar kalıcılığı etkileyen ayarlar aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="69974-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="69974-112">Varsayılan anahtar yaşam süresi</span><span class="sxs-lookup"><span data-stu-id="69974-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="69974-113">Rest mekanizması, anahtar şifreleme</span><span class="sxs-lookup"><span data-stu-id="69974-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

* [<span data-ttu-id="69974-114">Anahtarı içinde yer alan algoritmik bilgileri</span><span class="sxs-lookup"><span data-stu-id="69974-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="69974-115">Zaman çalışırken sonraki otomatik anahtarı'den önceki etkisini göstermeye bu ayarlarına gereksinim duyarsanız, açık bir çağrı yapmayı göz önüne alın `IKeyManager.CreateNewKey` yeni bir anahtar oluşturmayı zorlamak için.</span><span class="sxs-lookup"><span data-stu-id="69974-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="69974-116">Bir açık etkinleştirme tarihi vermeyi unutmayın ({artık + 2 gün} bir iyi değişiklik yayılması için zaman tanıyın için udur) ve çağrının sona erme tarihi.</span><span class="sxs-lookup"><span data-stu-id="69974-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="69974-117">Depo temas tüm uygulamalar aynı ayarlarla belirtmelidir `IDataProtectionBuilder` genişletme yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="69974-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="69974-118">Aksi durumda, kalıcı anahtar özelliklerini anahtar oluşturma yordamlarını çağrılan belirli uygulamasına bağımlı olur.</span><span class="sxs-lookup"><span data-stu-id="69974-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
