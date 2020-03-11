---
title: ASP.NET Core anahtar imlebilirlik ve anahtar ayarları
author: rick-anderson
description: ASP.NET Core veri koruma anahtarı imlebilirlik API 'Lerinin uygulama ayrıntılarını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664719"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>ASP.NET Core anahtar imlebilirlik ve anahtar ayarları

Bir nesne, yedekleme deposuna kalıcı olduktan sonra, temsili süresiz olarak düzeltilir. Yeni veriler, yedekleme deposuna eklenebilir, ancak mevcut veriler hiçbir şekilde değiştirilemez. Bu davranışın birincil amacı, verilerin bozulmasını önlemektir.

Bu davranışın bir sonucu, yedekleme deposuna bir anahtar yazıldıktan sonra sabittir. Oluşturma, etkinleştirme ve sona erme tarihleri hiçbir zaman değiştirilemez, ancak `IKeyManager`kullanılarak iptal edilebilir. Ayrıca, temel alınan algoritmik bilgileri, ana anahtar malzemeleri ve REST özelliklerindeki şifreleme de sabittir.

Geliştirici, anahtar kalıcılığını etkileyen herhangi bir ayarı değiştirirse, bu değişiklikler, bir sonraki `IKeyManager.CreateNewKey` ya da veri koruma sisteminin kendi [otomatik anahtar oluşturma](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) davranışı aracılığıyla bir sonraki anahtar üretilene kadar etkin olmaz. Anahtar kalıcılığını etkileyen ayarlar şunlardır:

* [Varsayılan anahtar yaşam süresi](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [REST mekanizmasında anahtar şifreleme](xref:security/data-protection/implementation/key-encryption-at-rest)

* [Algoritmik bilgileri, anahtar içinde yer alır](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Bu ayarların sonraki otomatik anahtar alma zamanından daha önce kullanıma sunulmasını istiyorsanız, yeni bir anahtarın oluşturulmasını zorlamak için `IKeyManager.CreateNewKey` açık çağrısı yapmayı düşünün. Açık bir etkinleştirme tarihi ({Now + 2 gün}), çağrının, değişikliğin yayması için zaman kullanılmasına izin veren iyi bir kural ve çağrının sona erme tarihi olduğunu unutmayın.

>[!TIP]
> Depoya dokunmadan tüm uygulamalar `IDataProtectionBuilder` uzantısı yöntemleriyle aynı ayarları belirtmelidir. Aksi takdirde, kalıcı anahtarın özellikleri, anahtar oluşturma yordamlarını çağıran belirli bir uygulamaya bağımlıdır.
