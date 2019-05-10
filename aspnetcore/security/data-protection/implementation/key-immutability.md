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
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>Anahtar değiştirilemezliği ve ASP.NET Core anahtar ayarları

Yedekleme deposu için bir nesneyi kalıcı sonra gösterimine her zaman sabittir. Yeni veri yedekleme deposu için eklenebilir, ancak hiçbir zaman mevcut verileri dönüşür. Bu davranış birincil amacı, veri bozulması engellemektir.

Bu davranış bir sonucu bir anahtarı yedekleme deposuna yazılır, sonra da değişmez olmasıdır. İptal ancak kullanarak oluşturma, etkinleştirme ve sona erme tarihleri hiçbir zaman, değiştirilebilir `IKeyManager`. Ayrıca, kendi temel alınan algoritmik bilgileri, ana anahtar malzemesini ve diğer özellikleri şifrelemeyi de sabittir.

Geliştirici anahtar kalıcılığı etkileyen herhangi bir ayar değişirse, bu değişiklikleri bir anahtar oluşturulur, zamana kadar açık bir çağrı yoluyla yürürlüğe olmaz `IKeyManager.CreateNewKey` veya veri koruma sisteminin kendi aracılığıyla [otomatik anahtar nesil](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) davranışı. Anahtar kalıcılığı etkileyen ayarlar aşağıdaki gibidir:

* [Varsayılan anahtar yaşam süresi](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [Rest mekanizması, anahtar şifreleme](xref:security/data-protection/implementation/key-encryption-at-rest)

* [Anahtarı içinde yer alan algoritmik bilgileri](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Zaman çalışırken sonraki otomatik anahtarı'den önceki etkisini göstermeye bu ayarlarına gereksinim duyarsanız, açık bir çağrı yapmayı göz önüne alın `IKeyManager.CreateNewKey` yeni bir anahtar oluşturmayı zorlamak için. Bir açık etkinleştirme tarihi vermeyi unutmayın ({artık + 2 gün} bir iyi değişiklik yayılması için zaman tanıyın için udur) ve çağrının sona erme tarihi.

>[!TIP]
> Depo temas tüm uygulamalar aynı ayarlarla belirtmelidir `IDataProtectionBuilder` genişletme yöntemleri. Aksi durumda, kalıcı anahtar özelliklerini anahtar oluşturma yordamlarını çağrılan belirli uygulamasına bağımlı olur.
