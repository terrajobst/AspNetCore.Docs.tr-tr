---
title: ASP.NET Core anahtarları iptal edilen yüklerin korumasını kaldır
author: rick-anderson
description: ASP.NET Core bir uygulamada, bu yana gelen anahtarlarla korunan verilerin korumasını kaldırma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 26061d048dcd9c1e3d8909e9388d8b565376fa2f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666602"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>ASP.NET Core anahtarları iptal edilen yüklerin korumasını kaldır

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

ASP.NET Core veri koruma API 'Leri öncelikle gizli yüklerin sınırsız kalıcılığı için tasarlanmamıştır. [WINDOWS CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) ve [Azure Rights Management](/rights-management/) gibi diğer teknolojiler, sınırsız depolama senaryosuna daha uygundur ve bunlara karşılık olarak güçlü anahtar yönetim özelliklerine sahiptir. Yani, bir geliştiricinin gizli verilerin uzun süreli korunması için ASP.NET Core veri koruma API 'Lerini kullanmasını engelleyen bir şey yoktur. Anahtarlar hiçbir zaman anahtar halkadan kaldırılmaz, bu nedenle `IDataProtector.Unprotect` anahtarlar kullanılabilir ve geçerli olduğu sürece mevcut yükleri her zaman kurtarabilirler.

Ancak geliştirici, iptal edilmiş bir anahtarla korunan verilerin korumasını denediğinde, `IDataProtector.Unprotect` bu durumda bir özel durum oluşturdukça bir sorun ortaya çıkar. Bu yük türleri sistem tarafından kolayca yeniden oluşturulabilir ve en kötü site ziyaretçisinin tekrar oturum açması gerekebilecek için, kısa süreli veya geçici yüklere (kimlik doğrulama belirteçleri gibi) uygun olabilir. Ancak kalıcı yükleri için `Unprotect` throw olması, kabul edilemez veri kaybına neden olabilir.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

İptal edilen anahtarların yanında bile yüklerin korumasız hale geçmesine izin verme senaryosunu desteklemek için, veri koruma sistemi bir `IPersistedDataProtector` türü içerir. Bir `IPersistedDataProtector`örneğini almak için, normal biçimde `IDataProtector` bir örneğini alın ve `IDataProtector` `IPersistedDataProtector`için dönüştürmeyi deneyin.

> [!NOTE]
> `IDataProtector` örneklerinin tümü `IPersistedDataProtector`atanamaz. Geliştiriciler, geçersiz yayınları C# neden olan çalışma zamanı özel durumlarını önlemek için as işlecini veya benzer şekilde kullanmalıdır ve hata durumunu uygun şekilde işleyecek şekilde hazırlanmalıdır.

`IPersistedDataProtector` aşağıdaki API yüzeyini kullanıma sunar:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Bu API, korumalı yükü (bir bayt dizisi olarak) alır ve korumasız yükü döndürür. Dize tabanlı aşırı yükleme yoktur. İki out parametresi aşağıdaki gibidir.

* `requiresMigration`: Bu yükü korumak için kullanılan anahtar artık etkin varsayılan anahtar değilse true olarak ayarlanacak. Örneğin, bu yükü korumak için kullanılan anahtar eski ve bir anahtar toplama işlemi gerçekleştiğinden daha sonra çalışır. Çağıran, iş ihtiyaçlarına bağlı olarak yükü yeniden korumayı düşünmek isteyebilir.

* `wasRevoked`: Bu yükü korumak için kullanılan anahtar iptal edildiyse, true olarak ayarlanır.

>[!WARNING]
> `DangerousUnprotect` yöntemine `ignoreRevocationErrors: true` geçirirken çok dikkatli olun. Bu yöntemi çağırdıktan sonra `wasRevoked` değeri true olduğunda, bu yükü korumak için kullanılan anahtar iptal edilir ve yükün kimlik doğrulaması şüpheli olarak değerlendirilmelidir. Bu durumda, gerçek bir güveninin, örneğin güvenilmeyen bir Web istemcisi tarafından gönderilmesi yerine güvenli bir veritabanından geldiği durumlarda, yalnızca korumasız yük üzerinde çalışmaya devam edin.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
