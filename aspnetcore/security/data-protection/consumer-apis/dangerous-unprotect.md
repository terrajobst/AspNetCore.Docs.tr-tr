---
title: ASP.NET Core, anahtarları geçersiz kılmış yükü korumasını
author: rick-anderson
description: Sonra bir ASP.NET Core uygulamada iptal edilmiş anahtarlarla korunan verilerin korumasını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: ed0c2a309e899f018b09b3edc86b65b299647914
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272392"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>ASP.NET Core, anahtarları geçersiz kılmış yükü korumasını


<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

ASP.NET Core veri koruma API değil öncelikle yöneliktir gizli yüklerini belirsiz kalıcılığını. Diğer teknolojiler ister [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) ve [Azure Rights Management](https://docs.microsoft.com/rights-management/) belirsiz depolama senaryosu için daha uygundur ve buna bağlı olarak güçlü anahtar yönetim olanaklarına sahip olursunuz. Bu, gizli verilerin uzun dönem koruma için ASP.NET Core veri koruma API'ları kullanarak bir geliştirici yasaklanması hiçbir şey belirtti. Anahtarları hiçbir zaman kaldırılır anahtar halka dışında bu nedenle `IDataProtector.Unprotect` anahtarları kullanılabilir ve geçerli olduğu sürece her zaman varolan yüklerini kurtarabilirsiniz.

Ancak, geliştirici olarak iptal edilen bir anahtarla korunan verileri korumanın çalıştığında bir sorun ortaya çıkar `IDataProtector.Unprotect` bu durumda bir özel durum oluşturur. Bu tür yüklerini sistem tarafından kolaylıkla yeniden oluşturulabilir ve en kötü olasılıkla ziyaretçi yeniden oturum açmak için gerekli olabilir, bu bağlantı (gibi kimlik doğrulama belirteçleri), kısa süreli veya geçici yükü için daha iyi olabilir. Ancak sahip kalıcı yüklerini `Unprotect` throw kabul edilebilir veri kaybına neden olabilir.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Bile iptal edilen anahtarları karşısında korumasız olacak şekilde yüklerini vermeyle senaryoyu desteklemek için veri koruma sistemini içeren bir `IPersistedDataProtector` türü. Bir örneğini almak `IPersistedDataProtector`, yalnızca bir örneğini almak `IDataProtector` deneyin atama ve normal şekilde `IDataProtector` için `IPersistedDataProtector`.

> [!NOTE]
> Tüm `IDataProtector` örnekleri dönüştürülür için `IPersistedDataProtector`. Geliştiriciler C# operatör olarak kullanması gereken veya hata durumunu uygun şekilde işlemek üzere kullanılabilir olmalı ve çalışma zamanı özel durumları önlemek için tarafından geçersiz atamaları neden benzer hazır.

`IPersistedDataProtector` Aşağıdaki API yüzeyi sunar:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Bu API korumalı Yükü (olarak bir bayt dizisi) alır ve korumasız yükü döndürür. Dize tabanlı hiçbir aşırı yüklemesi yoktur. Out Parametreleri iki aşağıdaki gibidir.

* `requiresMigration`: Bu yükü korumak için kullanılan anahtar artık etkin olan varsayılan anahtardır, örn., bu yük korumak için kullanılan anahtar eski ise ve bu yana işlemi çalışırken bir anahtara sahip true olarak ayarlanırsa, gerçekleştirilen. Çağıran iş gereksinimlerine bağlı olarak yük bunun dikkate alınması gereken isteyebilir.

* `wasRevoked`: Bu yükü korumak için kullanılan anahtarı iptal edildi sahipse true olarak ayarlanmalıdır.

>[!WARNING]
> Geçirilirken son derece dikkatli olun çalışma `ignoreRevocationErrors: true` için `DangerousUnprotect` yöntemi. Bu yöntemi çağrıldıktan sonra IF `wasRevoked` değer true, sonra bu yükü korumak için kullanılan anahtarı iptal edildi ve yükü 's Orijinallik şüpheli olarak değerlendirilmelidir. Bu durumda, yalnızca, örneğin gerçek, olduğunu bazı ayrı güvence varsa korumasız yük temelinde işletim güvenilmeyen web istemcisi tarafından gönderilen yerine güvenli bir veritabanında geldiğinden emin devam edin.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
