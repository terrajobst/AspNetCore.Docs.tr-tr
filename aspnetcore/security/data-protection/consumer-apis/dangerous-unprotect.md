---
title: ASP.NET Core anahtarları iptal edilen yüklerin korumasını kaldırma
author: rick-anderson
description: Sonra ASP.NET Core uygulamanızı iptal edilmiş anahtarlara sahip korumalı veri korumasını öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 26061d048dcd9c1e3d8909e9388d8b565376fa2f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902909"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>ASP.NET Core anahtarları iptal edilen yüklerin korumasını kaldırma

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

ASP.NET Core veri koruma API'lerini öncelikle amaçlanmayan gizli yükü belirsiz kalıcılığını. Diğer teknolojiler ister [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) ve [Azure Rights Management](/rights-management/) belirsiz depolama senaryosu için daha uygundur ve bunlar gelenlere güçlü anahtar yönetim olanaklarına sahip olursunuz. Bu, gizli verilerin uzun dönem koruma için ASP.NET Core veri koruma API'lerini kullanarak bir geliştirici yasaklanması bir şey yoktur söylenir. Anahtarları asla kaldırılır anahtar halka dışında bu nedenle `IDataProtector.Unprotect` anahtarlar kullanılabilir ve geçerli olduğu sürece her zaman var olan yükü kurtarabilirsiniz.

Ancak, geliştirici olarak iptal edilen bir anahtar ile korunan verilerin korumasını çalıştığında bir sorun ortaya çıkar `IDataProtector.Unprotect` bu durumda bir özel durum oluşturur. Sistem tarafından bu tür yüklerini kolayca yeniden oluşturulabilir ve en kötü olasılıkla ziyaretçi yeniden oturum açmak için gerekli olabilir, bu bağlantı için (örneğin, kimlik doğrulama belirteçlerinizi), kısa süreli veya geçici yükü olabilir. Ancak sahip kalıcı yüklerini `Unprotect` throw kabul edilebilir veri kaybına neden olabilir.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

İptal edilen anahtarları karşılaşıldığında bile korumasız olacak şekilde yüklerini zorlu senaryoyu desteklemek için veri koruma sisteminde içeren bir `IPersistedDataProtector` türü. Bir kopyasını almak için `IPersistedDataProtector`, yalnızca bir örneğini almak `IDataProtector` normal bir biçimde ve deneyin atama `IDataProtector` için `IPersistedDataProtector`.

> [!NOTE]
> Tüm `IDataProtector` örnekleri yayımlanabilir `IPersistedDataProtector`. Geliştiricilerin kullanması gereken C# işleci olarak ya da benzeri çalışma zamanı özel durumları engellemek için neden tarafından geçersiz yayınları ve hata durumunda uygun şekilde işlemeye hazırlıklı olmalıdır.

`IPersistedDataProtector` Aşağıdaki API yüzeyi kullanıma sunar:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Bu API, korumalı Yükü (olarak, bir bayt dizisi) alır ve korumasız yükü döndürür. Hiçbir dize tabanlı aşırı yüklemesi vardır. İki out parametreleri aşağıdaki gibidir.

* `requiresMigration`: Bu yükü korumak için kullanılan anahtarı artık etkin varsayılan anahtardır, örneğin, bu yükü korumak için kullanılan anahtarı eski ve işlem çalışırken bir anahtar beri varsa true olarak ayarlanırsa, gerçekleştirilen. Çağıran, yükü işletme gereksinimlerine bağlı olarak bulunmayı dikkate alınması gereken isteyebilir.

* `wasRevoked`: Bu yükü korumak için kullanılan anahtarı iptal edildi durumunda true olarak ayarlayın.

>[!WARNING]
> Dikkatli aşırı geçerken `ignoreRevocationErrors: true` için `DangerousUnprotect` yöntemi. Bu yöntemi çağrıldıktan sonra eğer `wasRevoked` değeri true ise daha sonra bu yükü korumak için kullanılan anahtarı iptal edildi ve yükü'nın kimlik doğrulaması şüpheli olarak değerlendirilmelidir. Bu durumda, yalnızca korumasız yükünü, örneğin gerçek, olduğundan bazı ayrı güvencesi varsa çalışan bir güvenilmeyen web istemcisi tarafından gönderilen yerine güvenli bir veritabanında geldiğinden emin devam edin.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
